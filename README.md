# android-navigation-simple

Start a new Android Studio project (Empty Activity)

Add resource file navigation_graph.xml

Android Studio will prompt you to add the following dependencies:
- androidx.navigation:navigation-fragment-ktx:2.3.0
- androidx.navigation:navigation-ui-ktx:2.3.0

Add HomeFragment

Add new destination HomeFragment to navigation_graph

Add NavHostFragment to activity_main.xml

```xml
<fragment
    android:id="@+id/navHostFragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
          ...
    app:defaultNavHost="true"
          ...
    app:navGraph="@navigation/navigation_graph" />
```

* id is mandatory
* defaultNavHost to handle backstack

Add MoreFragment

Add new destination MoreFragment to navigation_graph with <code>android:id="@+id/homeFragment"</code>

Add Button to HomeFragment

Trigger navigation to the MoreFragment from code:

```java
nextButton.setOnClickListener {
    findNavController().navigate(R.id.moreFragment)
}
```

Configure SafeArgs

In the top-level build.gradle:

    classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.3.0"

In the app-level build.gradle:

    apply plugin: 'androidx.navigation.safeargs.kotlin'

    android {
        ...
        kotlinOptions {
            jvmTarget = JavaVersion.VERSION_1_8
        }
    }
    
Add argument to destination in navigation_graph.xml

```xml
<fragment
        android:id="@+id/moreFragment"
        android:name="com.example.androidnavigationsimple.MoreFragment"
        android:label="fragment_more"
        tools:layout="@layout/fragment_more">

    <argument
        android:name="someNumber"
        app:argType="integer" />

</fragment>
```
     
When you build the project the plugin generates MoreFragmentArgs class.

Now from MoreFragment you can retrieve the arguments as follows:

    val args: MoreFragmentArgs by navArgs()
    val number = args.someNumber
    
Create action from HomeFragment to MoreFragment

Action is in the fragment you navigate *from*, whereas arguments are in the fragment you navigate *to*.

```xml
<fragment
    android:id="@+id/homeFragment"
    android:name="com.example.androidnavigationsimple.HomeFragment"
    android:label="fragment_home"
    tools:layout="@layout/fragment_home" >

    <action
        android:id="@+id/action_homeFragment_to_moreFragment"
        app:destination="@id/moreFragment" />

</fragment>
```

When you build the project, the navigation-safe-args-gradle-plugin generates the HomeFragmentDirections class.

Trigger the navigation from code as follows:

```java
val action = HomeFragmentDirections.actionHomeFragmentToMoreFragment(10)
findNavController().navigate(action)
