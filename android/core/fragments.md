# fragments

fragments are used for modulization.
it is a good android design principle to use 1 activity and make the rest a bunch of fragments
fragments must be hosted by an activity. they can also have subfragments

# creating the fragment. 

1. in the *app* folder go to *java* -> package name ex: "com.quality.brax" then right click
2. select new fragment

## add to activity

add a framelayout. it's ok if some of the stuff isn't auto suggested. It didn't suggest it to me but it still compiled fine regardless

<FrameLayout
        android:id="@+id/fragment_container"
        android:name="BlankFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:layout="@layout/fragment_blank" />


# using jetpack (much easier)

## changing fragments

int mainactivity write
```
BlankFragment blankFragment = BlankFragment.newInstance("foo", "bar");
getSupportFragmentManager()
        .beginTransaction()
        .replace(R.id.fragment_container,blankFragment)
        .commit();
```

that's legit it. the blank fragment was created by right clicking the java file and clicking new blankfragment.




# guides telling me to do the long way....
just don't read this section
## how to call the fragment and transition

this is what the guides said to do....
```
BlankFragment blankFragment= BlankFragment.newInstance("cowboy","indian");
        FragmentManager fragmentManager = getSupportFragmentManager();
        FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
        fragmentTransaction
                .replace(R.id.fragment_container,fragmentClass)
                .commit();
```

here is my simplified version which should do the same thing.
```
getSupportFragmentManager()
                .beginTransaction()
                .replace(R.id.fragment_container,fragmentClass)
                .commit();
```


# additional methods.
|method|description|
|---|---|
|.add|adds it to the fragment queue. It will show on top of the other ones
|.addToBackStack(null)| you can go backwards to the one before it.

