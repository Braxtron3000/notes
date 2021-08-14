# [Room Overview](https://developer.android.com/training/data-storage/room)

The Room library helps with simplifying SQLite queries and setting up a database.

It is recommended to use the singleton design pattern when instantiating an appdatabase object

You cannot make database actions in the main thread of the application because it will greatly slow down the UI.
It is highly reccomended to use [RXJava for android](https://developer.android.com/training/data-storage/room/async-queries#one-shot). It is incredibly simple.

incorporating RXJava into room will be discussed further in the *Enacting it* section of this doc.

there are three components in Room.
1. [database class](https://developer.android.com/reference/kotlin/androidx/room/Database) 
   * holds the database and is main accesss point for connecting to the persisted data.
2. Data entries
   * represent the database tables
3. Data access objects (DAOs)
   * provide methods that your app can use to query, update, insert, etc. into the database.

in other words 
* database classes creates the database
* entries handle the propertis of each row 
* data access objects help modify the info in the table

# Setting it up

## dependencies

```
dependencies {
    def room_version = "2.3.0"

    implementation "androidx.room:room-runtime:$room_version"
    annotationProcessor "androidx.room:room-compiler:$room_version"

    // optional - RxJava2 support for Room
    implementation "androidx.room:room-rxjava2:$room_version"

    // optional - RxJava3 support for Room
    implementation "androidx.room:room-rxjava3:$room_version"

    // optional - Guava support for Room, including Optional and ListenableFuture
    implementation "androidx.room:room-guava:$room_version"

    // optional - Test helpers
    testImplementation "androidx.room:room-testing:$room_version"

    // optional - Paging 3 Integration
    implementation "androidx.room:room-paging:2.4.0-alpha04"
}
```

## Data Entity

each instance of User represents a row in a user table in the app's database

```
@Entity
public class User {
    @PrimaryKey
    public int uid;

    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    public String lastName;
}
```

## Data access object (DAO)

UserDao provides the methods that the rest of the app uses to interact with data in the user table.

I'm going to assume the headers with @ signs specify to delete something from the database. 

Notice that its an interface which means the methods are meant to be defined elsewhere

```
@Dao
public interface UserDao {
    @Query("SELECT * FROM user")
    List<User> getAll();

    @Query("SELECT * FROM user WHERE uid IN (:userIds)")
    List<User> loadAllByIds(int[] userIds);

    @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
           "last_name LIKE :last LIMIT 1")
    User findByName(String first, String last);

    @Insert
    void insertAll(User... users);

    @Delete
    void delete(User user);
}
```

## Database

lastly you need to define a class to hold the database

basically, the class will extend the ***RoomDatabase*** class and has an ***@Database*** notation. The class also needs to be declared abstract.

For each DAO class for the database, the class must define an abstract method that has zero arguments and returns and instance of the DAO class

```

@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```

again, if the app runs in a single process you should follow the singleton design pattern because RoomDatabase instance is pretty expensive. Otherwise if the app runs multiple processes include ***enableMultiInstanceInvalidation()*** in your database builder invocation.

# Enacting it

create an instance of the database object
```
AppDatabase db = Room.databaseBuilder(getApplicationContext(),
        AppDatabase.class, "database-name").build();
```

Then you can use the abstract methods from the AppDatabase to get an instance of the DAO.

```
UserDao userDao = db.userDao();
List<User> users = userDao.getAll();
```

## RXJava threading
As mentioned at the beginning of this document, Room prevents calling database actions on the main thread. An easy solution to this is to use RXJava, a java library for threading. Here's how you can do this.

1. add ***completable*** & ***Single*** to methods. 
    ```
    @Dao
    public interface UserDao {
        @Query("SELECT * FROM user")
        Single<User> getAll();

        @Query("SELECT * FROM user WHERE uid IN (:userIds)")
        Single<User> loadAllByIds(int[] userIds);


        @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
                "last_name LIKE :last LIMIT 1")
        Single<User> findByName(String first, String last);

        @Insert(onConflict = OnConflictStrategy.REPLACE)
        void insertAll(User... users);

        @Delete
        void delete(User user);
    }
    ```

    DON'T USE THESE for insert & delete methods because RX does not support for those methods

    There have been several issues made about this and many developers are suggesting the RX developers work on supporting insert & delete methods so this issue may change in the future but until then you must call the insert/delete in the activity by using a Completable object and Runnable lambda like so.

    ```
    Completable.fromRunnable(new Runnable() {
        @Override
        public void run() {
            userDao.delete(user);
            userDao.insertAll(user);
        }
    })
            .subscribeOn(Schedulers.io())
            .subscribe();
    ```

    if the return type for the INSERT or DELETE methods isn't void do this instead.

    ```
    Observable.fromCallable(() -> db.activitiesDao().insertStep(step))
        .subscribeOn(Schedulers.io())
        .subscribe(...);
    ```