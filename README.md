# android-navigation-simple

Start a new Android Studio project (Empty Activity)

Add resource file navigation_graph.xml

Android will prompt you to add the following dependencies:
- androidx.navigation:navigation-fragment-ktx:2.3.0
- androidx.navigation:navigation-ui-ktx:2.3.0

Add HomeFragment

Add new destination HomeFragment to navigation_graph

Add NavHostFragment to activity_main.xml

    <fragment
      android:id="@+id/navHostFragment"
      ...
      android:name="androidx.navigation.fragment.NavHostFragment"
      ...
      app:navGraph="@navigation/navigation_graph" />
