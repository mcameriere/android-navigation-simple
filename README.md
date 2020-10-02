# android-navigation-simple

A simple Android app with a navigation graph and two destinations.

The two destinations are the fragments **HomeFragment** and **MoreFragment**.

MoreFragment will receive an **integer** argument with name **someNumber** and a default value of **10**.

## Start a new Android Studio project

* Launch **Android Studio** and click **Start a new Android Studio project**
* In **Select a Project Template** select **Empty Activity**
* Name your project
* Choose language **Kotlin**
* Click **Finish**

## Add new resource file navigation_graph.xml

In res folder add new **Android Resource File**

Android Studio will prompt you:

    Add Project Dependency

    This operation requires the libraries
    - androidx.navigation:navigation-fragment-ktx:+,
    - androidx.navigation:navigation-ui-ktx:+.
    
    Would you like to add these now?

Click **OK** to accept

## Add HomeFragment

Add a new **Fragment (Blank)** with name **HomeFragment**

## Add new destination HomeFragment to navigation_graph

* Open navigation_graph.xml, select **Design**
* Click **New Destination** and select **fragment_home**

## Add NavHostFragment to activity_main.xml

```xml
<fragment
    android:id="@+id/navHostFragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
          ...
    app:navGraph="@navigation/navigation_graph" />
```

Run the app: the HomeFragment is displayed.

## Add MoreFragment

Add a new **Fragment (Blank)** with name **MoreFragment**

## Add a second destination to navigation_graph

* Open navigation_graph.xml, select **Design**
* Click **New Destination** and select **fragment_more**

## Add Button to HomeFragment

```xml
<Button
    android:id="@+id/buttonMore"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="More" />
```

In HomeFragment, trigger navigation to MoreFragment

```java
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    buttonMore.setOnClickListener {
        findNavController().navigate(R.id.moreFragment)
    }
}
```

Run the app: click More on HomeFragment navigates to MoreFragment.

**Problem:** If you click back on MoreFragment, the app is closed.

## Configure NavHostFragment to handle backstack

To handle backstack, configure NavHostFragment in activity_main.xml.

```xml
<fragment
        android:id="@+id/navHostFragment"
        ...
        app:defaultNavHost="true"
        ...
        app:navGraph="@navigation/navigation_graph" />
```

Run app: click back on MoreFragment to navigate *back* to HomeFragment

## Add argument to destination

In navigation_graph.xml, add argument to moreFragment

```xml
<fragment
    android:id="@+id/moreFragment"
    android:name="com.example.androidnavigationsimple.MoreFragment"
    android:label="fragment_more"
    tools:layout="@layout/fragment_more">

    <argument
        android:name="someNumber"
        app:argType="integer"
        android:defaultValue="10"/>

</fragment>
```

In MoreFragment, you can now read "someNumber" argument

```java
class MoreFragment : Fragment() {

    private var someNumber: Int? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
            someNumber = it.getInt("someNumber")
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        textView.text = someNumber.toString()
    }
    
}
```

## Configure SafeArgs

In the top-level <code>build.gradle</code>

    classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.3.0"

In the app-level <code>build.gradle</code>

    apply plugin: 'androidx.navigation.safeargs.kotlin'

    android {
        ...
        kotlinOptions {
            jvmTarget = JavaVersion.VERSION_1_8
        }
    }
    
When you build the project the SafeArgs plugin generates <code>MoreFragmentArgs</code> class.

In MoreFragment you can now retrieve the **someNumber** argument in a type-safe way.

```java
class MoreFragment : Fragment() {

    private val args: MoreFragmentArgs by navArgs()
    
    ...

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        
        textView.text = args.someNumber.toString()
    }

}
```
