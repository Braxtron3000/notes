A recycler view helps create a list of views for a large amount of data. It makes things faster by taking the view that has been scrolled off the page and substituting (recycling) the newly scrolled data into it. 


# three key parts


1. **data class**
content that goes inside each viewholder. It's an object. Example: spotify playlists, title, icon image and shortened description.
1. **adapter**
prepares the data for the recyclerview to display

2. **viewholder**
a bunch of views for the recyclerview to use and display
 


# datasource class

You need a class that takes all the data and puts them into a data collection that will work for the adapter's constructor so the input's can be changed for each one.

```
package brax.quality.android_scratch_practice;


import android.content.Context;

public class DataSource {
    private Context context;

    public DataSource(Context context) {
        this.context = context;
    }


    public String[] loadAffirmations() {
        return new String[]{
                context.getResources().getString(R.string.affirmation1),
                context.getResources().getString(R.string.affirmation2),
                context.getResources().getString(R.string.affirmation3),
                context.getResources().getString(R.string.affirmation4),
                context.getResources().getString(R.string.affirmation5),
                context.getResources().getString(R.string.affirmation6),
                context.getResources().getString(R.string.affirmation7),
                context.getResources().getString(R.string.affirmation8),
                context.getResources().getString(R.string.affirmation9),
                context.getResources().getString(R.string.affirmation10),
        };



    }
}
```
basically it returns a string array of the strings established in the strings.xml file. the Context in the constructor is there so the Context 'this' can be set and the the getResources can be used.



# adapter class
**the big one**

the adapter class extends the recyclerview adapter of type viewholder.

```
public class CustomAdapter extends RecyclerView.Adapter<CustomAdapter.ViewHolder> {
    private String[] localDataSet;

    /*reference to the type of views I'm using goes here*/

    public static class ViewHolder extends RecyclerView.ViewHolder {
        private final TextView textView;

        public ViewHolder(View view) {
            super(view);
            textView = (TextView) view.findViewById(R.id.item_title);
        }

        public TextView getTextView() {
            return textView;
        }
    }

    public CustomAdapter(String[] dataSet) {
        localDataSet = dataSet;
    }

```

## viewholder
View holder could probably set up in a file class by itself. This is where you define the attributes that will be used in each view. 

the Layout in this example is just a simple Textview. The file was called *list_item.xml* and made out of a simple file **not a layout file**.

```
<?xml version="1.0" encoding="utf-8" ?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/item_title"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />

```


There are three methods you will need to override that will help you create a custom adapter for a recyclerview.

It has three methods
1. onCreateViewHolder - viewholder creator
2. onBindViewHolder - binding data
3. getItemCount - number of items

## 1. onCreateViewHolder
for creating new views. it creates a new view holder and uses the layoutinflator to create a view

```
@Override
    public ViewHolder onCreateViewHolder(ViewGroup viewGroup, int viewType) {
        //create a new view
        View view = LayoutInflater.from(viewGroup.getContext())
                .inflate(R.layout.list_item, viewGroup, false);
        //pass the list_item view & the parent group,
        // set attachToRoot parameter to false because recyclyer view does this

        return new ViewHolder(view); //returns adapter layout
    }
```

basically what this does is creates a new list_item layout to the viewgroup and returns a Viewholder with the 

## 2. onBindViewHolder
associates the viewholder with the data

```
@Override
    public void onBindViewHolder(ViewHolder viewHolder, final int position){
        viewHolder.getTextView().setText(localDataSet[position]);
    }
```

basically this is where the parts of the view template's parts are set based upon the array string given.


## 3. getItemCount
returns the length of the items

```
@Override
    public  int getItemCount(){
        return localDataSet.length;
    }
```

# implementation


int the xml
```
<androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:scrollbars="vertical"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"/>
```


in the activity's java class
```
public void setRecyclerView() {
        RecyclerView recyclerView = findViewById(R.id.recycleView);
        DataSource dataSource = new DataSource(this);
        CustomAdapter customAdapter = new CustomAdapter(dataSource.loadAffirmations());
        recyclerView.setAdapter(customAdapter);
    }
```

# extras

## horizontal
in java add this to the layout activity
```
RecyclerView.LayoutManager linearLayoutManager =new LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL,false);
        recyclerView.setLayoutManager(linearLayoutManager);
```
## gridview