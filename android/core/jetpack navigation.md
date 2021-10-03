# bottom navigation

in mainactivity.xml
```
<androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:defaultNavHost="true"
        app:navGraph="@navigation/pages_destinations" />
```


in the mainactivity class
```
 NavHostFragment navHostFragment = (NavHostFragment) getSupportFragmentManager().findFragmentById(R.id.nav_host);
        assert navHostFragment != null;
        NavController navController = navHostFragment.getNavController();
        BottomNavigationView bottomNavigationView = findViewById(R.id.bottomNavigationView);
        bottomNavigationView.setBackground(null);
        NavigationUI.setupWithNavController(bottomNavigationView, navController);
```


# drawerlayout