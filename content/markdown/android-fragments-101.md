Title: Android Fragments 101
Date: 2013-01-01 00:00:00
Modified: 2013-01-01 11:30:00+05:30
Slug: android-fragments-101
Authors: ikram
Tags: android
Summary: Ever wanted a code snippet from an Activity to be available to other activities. Perhaps a Button or a ListView, maybe a Layout or any View/ViewGroup for the matter. Fragments let us do just that.
_**Prerequisite**: You are already aware of the [basics of building a HelloWorld](http://developer.android.com/training/index.html) in Android and know [how to use the APIs provided in the support library](http://developer.android.com/training/basics/fragments/support-lib.html)._

_The code example is available on [github](http://github.com/iontech/Fragments_Example "Fragments Example")._
_____________________________________________________________
Ever wanted a code snippet from an Activity to be available to other activities? Perhaps a Button or a ListView, maybe a Layout or any View/ViewGroup for that matter? Fragments let us do just that.

Necessity is the mother of invention.  
Before understanding what Fragments are and how they work, we must first realize their existence in the first place.

The Problem
-----------
Suppose we have an Android app with two Activities- [*FirstActivity*](https://github.com/iontech/Fragments_Example/blob/master/src/main/java/com/github/iontech/fragments_example/FirstActivity.java) and [*SecondActivity*](https://github.com/iontech/Fragments_Example/blob/master/src/main/java/com/github/iontech/fragments_example/SecondActivity.java).  
*FirstActivity* contains two Views, a `TextView` (*textView*) and a `Button` (*button1*); and *button1* has an `onClick()` callback that `Toast`'s a simple message "Button pressed".  
*SecondActivity* contains both the Views present in *FirstActivity* and a `Button` (*button2*).

Now we want to utilize the two layout components(Views) of *FirstActivity* in *SecondActivity*, we can go about this with two approaches:

1. Copy and Paste the xml elements of the two Views.
2. Create a separate layout for common Views and reuse it using `<include />` layout element.  
    More about this [here](http://developer.android.com/training/improving-layouts/reusing-layouts.html).

Electing the second approach makes sense cause it enables us to make reusable layouts. Everything seems great till now. We are able to make reusable layouts and use them as many times as we want.

Now recollect that we have an `onClick()` callback assigned to *button1*. How do we reuse the same callback functionality of *button1* across multiple Activities? `<include />` lets us reuse layouts and not the Activity source.  
This is where Fragments come into play.

Fragments
---------
<center> ![image](http://iontech.files.wordpress.com/2013/01/androidfragmentation1-264x300.png) </center>
Fragments encompass both layout resource and Java source. Hence, unlike `<include />`, they allow us to reuse the View components along with their functionality, if needed.
Fragments were first introduced in Honeycomb(API 11), living under the `android.app` package.  
**Note**: API 11 implies that Fragments have no support for devices less than Honeycomb and, for the record, as of writing this post, [more than 50% of Android devices worldwide run versions of Android below Honeycomb](http://developer.android.com/about/dashboards/index.html). Developer dissapointed? You don't have to be, cause google has been cautious enough to add the Fragment APIs to the support library. Yay!

In the support library Fragment APIs sit in the `android.support.v4.app` package. This post assumes that your `minSdk` support is below API 11. Hence we concentrate on the Fragment APIs of the support library.

### Diving into code

Performing code reuse with Fragments involves three major steps:

1. Creating reusable View components - Creating a layout for the fragment.
2. Creating reusable Java source - Writing the layout's corresponding Fragment class.
3. Employing the reusable components in Activity - Making an Activity to host this Fragment.

#### 1. Creating reusable View components
##### Creating a layout for the Fragment
This is done precisely as we do it for our activity layouts. The layout contains a root element (ViewGroup) defining the layout, For instance in our example we use a LinearLayout and its child elements(the reusable Views) that we want to have in our fragment.

> [fragment_common.xml](https://github.com/iontech/Fragments_Example/blob/master/res/layout/fragment_common.xml)

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:orientation="vertical"
			  android:layout_width="fill_parent"
			  android:layout_height="fill_parent">

    	<TextView
    			android:layout_width="wrap_content"
    			android:layout_height="wrap_content"
    			android:text="@string/textView"
    			android:id="@+id/textView" android:layout_gravity="center"
    			android:layout_margin="5dp"/>
    
    	<Button
    			android:layout_width="wrap_content"
    			android:layout_height="wrap_content"
    			android:text="@string/button1"
    			android:id="@+id/button1"
    			android:layout_gravity="center"/>

    </LinearLayout>

#### 2. Creating reusable Java source
##### Writing the layout's corresponding Fragment class

> [CommonFragment.java](https://github.com/iontech/Fragments_Example/blob/master/src/main/java/com/github/iontech/fragments_example/CommonFragment.java)

This class will inherit `Fragment` class and must override the `onCreateView()` method.
In this method we inflate the fragment layout using the following line of code.

    View view = inflater.inflate(R.layout.fragment_common, container, false);

*container* is the parent ViewGroup that the fragment's UI should be attached to.
Once inflation is done, we can perform various operations on the component views of the fragment.
Accessing the view elements from the layout is done exactly as we do in an Activity (using `findViewById()`) except that we use the `View` for the fragment's UI or an instance of the host Activity.

In Activity we access Views(a Button for our example) as follows

    Button button1 = (Button) findViewById(R.id.button1);    
In Fragment, we need to use the inflated view(if in `onCreateView()`) or get the instance of the host activity and access the views through this instance(when in a [lifecycle callback](http://developer.android.com/guide/components/fragments.html#Lifecycle "Fragment lifecycle callbacks") after `onCreateView()`, I generally do it in `onActivityCreated()`).

    Button button1 = (Button) view.findViewById(R.id.button1);
or

    Button button1 = (Button) getActivity().findViewById(R.id.button1);
`getActivity()` returns the instance of the Activity that is hosting this Fragment.

Finally, in `onCreateView()` we must return the View for the fragment's UI.

    return view;
    
#### 3. Employing the reusable components in Activity
##### Making an Activity to host this Fragment
This is done in two ways, statically by adding `<fragment />` elements into the Activity layout or dynamically, at run time, by using `FragmentTransaction`s.
**Note**: First thing we need to ensure is that our Activity extends `FragmentActivity` class instead of the regular `Activity`.
If the `minSdk` is API 11 or higher, then we can leave our inheritance to `Activity` class and not bother about `FragmentActivity`.

##### a. Static approach to hosting the Fragments
###### Adding `<fragment />` element in activity layout

> [activity_static.xml](https://github.com/iontech/Fragments_Example/blob/master/res/layout/activity_static.xml)

Inorder to statically add a Fragment into your Activity, just add `<fragment />` element with the necessary layout attributes and the `android:name` attribute set to the fully qualified class name of the corresponding Fragment.

    <fragment android:layout_width="wrap_content"
    		  android:layout_height="wrap_content"
			  android:name="com.github.iontech.fragments_example.CommonFragment"
			  android:id="@+id/fragment" android:layout_gravity="center"/>
##### b. Dynamic approach
###### Using `FragmentTransaction`s

> [ADynamicFragmentActivity.java](https://github.com/iontech/Fragments_Example/blob/master/src/main/java/com/github/iontech/fragments_example/ADynamicFragmentActivity.java)

Can be done using `FragmentManager` and `FragmentTransaction` classes. We call `add()`, in our FragmentActivity implementation, on an instance of `FragmentTransaction` to add a fragment to the host Activity. But that is not enough to show the fragment on the screen, i.e. the FragmentTransaction is not complete. We must call `commit()` to finish the transaction.

    CommonFragment fragment = new CommonFragment();
    FragmentManager manager = getSupportFragmentManager();
    FragmentTransaction transaction = manager.beginTransaction();
    transaction.add(R.id.dynamicFragmentLayout, fragment);
    transaction.commit();

Similarly Fragments can be removed(`remove()`) as well as replaced(`replace()`) from the activity all at runtime.

Congratulations, now you can use Fragments to write reusable code and easily host them over multiple activities.

Till next time.

next time.

