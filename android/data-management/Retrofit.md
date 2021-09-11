# [Retrofit overview](https://square.github.io/retrofit/)

turns your HTTP API into a java interface

see the actual retrofit documentation [here](https://square.github.io/retrofit/)

# basic implementation

first you need the gradle dependency:
```
  //retrofit
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.okhttp3:okhttp:3.14.9'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'

```
Notice the okhttp dependency. One of the methods uses okhttp but i don't remember which one.

After you sync the dependencies there are 3 steps to implement 
1. the interface
2. the object
3. the implementation

here is part of the json array that the tutorial is trying to receive

https://jsonplaceholder.typicode.com/posts

```
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
  },
  {
    "userId": 1,
    "id": 3,
    "title": "ea molestias quasi exercitationem repellat qui ipsa sit aut",
    "body": "et iusto sed quo iure\nvoluptatem occaecati omnis eligendi aut ad\nvoluptatem doloribus vel accusantium quis pariatur\nmolestiae porro eius odio et labore et velit aut"
  },
```




## the interface

```
public interface Retrofit_Interface {

    @GET("posts")
    Call<List<Posts>> getPosts(
            @Query("userId") Integer[] userId,
            @Query("_sort") String sort,
            @Query("_order") String order
    );
    @GET("posts")
    Call<List<Posts>> getPosts(@QueryMap Map<String, String> parameters);
    @GET("posts/{id}/comments")
    Call<List<Comment>> getComments(@Path("id") int postId);
    @GET
    Call<List<Comment>> getComments(@Url String url);
}
```
Some of these methods aren't used in this tutorial.


use the ***@GET("theJSON objects")*** anotation to specify the array to retrieve. The full url will be put together throught the retrofit background methods. The base url is specified in the actual implementation.

There are other annotations described in the retrofit documentation notibly ***@Post*** and ,in this example, the @QueryMap

The @Path annotation can be used to specify a different path by using the parameters. it's for url's shown like so... https://baseurl.com/trucks/ford

If you want it to go from trucks to bikes you can set the url to
baseurl = "https://baseurl.com/{vehicle}/ford"
you can then specify getTrucks(@Path("vehicle") String vehicle)

## the object

This is the object that retrofit turns the json turns each of the json objects into.

here is the tutorial example
```
public class Posts {

    private int userId;
    private int id;
    private String title;
    @SerializedName("body")
    private String text;
    public int getUserId() {
        return userId;
    }
    public int getId() {
        return id;
    }
    public String getTitle() {
        return title;
    }
    public String getText() {
        return text;
    }
}
```
Each object value is an object value of the json objects. The names must be the same unless you use the @SerializedName annotation.


Notice the ***@SerializedName("body")***. This annotation is used any time you want to use a different name for an object value than what the json object specifies.


## the implementation

tutorial example
```
textViewResult = findViewById(R.id.text_view_result);
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://jsonplaceholder.typicode.com/")
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        jsonPlaceHolderApi = retrofit.create(Retrofit_Interface.class);
```
and now to break it down
>Retrofit retrofit = new Retrofit.Builder()
>               .baseUrl("https://jsonplaceholder.typicode.com/")
>               .addConverterFactory(GsonConverterFactory.create())
>               .build();

The methods are most important here. The .baseurl method specifies the base url make sure to include the "/" at the end.

The addConvertedFactory is also important if you don't include a factory it will automatically use an object specified from the okHTTP library by default. 

>jsonPlaceHolderApi = retrofit.create(Retrofit_Interface.class);
this is where the magic happens. The .create method implements all of the interface for you.

# HashMap it!






# part 3

## objective
here's what what we're trying to do and all the things you need to know about it.

## implementation 
here's what you've got to do to retrieve these things

# part 4

## objective
here's what what we're trying to do and all the things you need to know about it.

## implementation 
here's what you've got to do to retrieve these things

# part 5

## objective
here's what what we're trying to do and all the things you need to know about it.

## implementation 
here's what you've got to do to retrieve these things
