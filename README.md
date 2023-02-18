# Bitcoin-Tracker-Android-Project-GFGArticle
we will be building a Bitcoin Tracker App Project using Java and XML in Android. The application will display the current rates of Bitcoin in different countries using Bitcoin API. 

![BuildaBitcoinTrackerAndroidApp](https://user-images.githubusercontent.com/93249038/219827844-f6ace5f3-70af-4c7e-bacd-188d74c3a020.gif)

 
Step by Step Implementation
## Step 1: Create a New Project

To create a new project in Android Studio please refer to How to Create/Start a New Project in Android Studio. Note that select Java as the programming language.


## Step 2: Before going to the coding section first you have to do some pre-task

Permission: Add internet permission in the AndroidManifest.xml file

<uses-permission android:name=”android.permission.INTERNET”/>

Change the style to NoActionBar in the themes.xml file: 

<style name=”AppTheme” parent=”Theme.AppCompat.NoActionBar”>

Get the API key: You need to create a free account on Coinlayer and get an API key for this project.

Add dependency: 

We need to add this dependency in the app gradle file to make HTTP requests

implementation ‘com.loopj.android:android-async-http:1.4.9’


Add Currency list in string.xml:

We need to add the list of all currencies in the strings.xml. From here we will display it in the activity to the users. Add the below code in the strings.xml file.




# Step 3: Working with the activity_main.xml file

The XML codes are used to build the structure of the activity as well as its styling part. It contains an ImageView at the very top of the activity to display the logo of the app. Then it contains a TextView to display the bitcoin rate in the center of the activity. At last, we have a Spinner at the bottom of the activity to display the list of currencies from which the user can choose. This is a single activity application. Below is the code for the activity_main.xml file.



# Step 4: Create new layout resource files

For the spinner to display the list we also need to create a spinner item’s XML layout as well as its item’s layout for the adapter. Add the below codes in app> res > layout > spinner_dropdown_item.xml.



# Step 5: Working with the MainActivity.java file

In the java file, we will create a function that will make HTTP requests from the URL. The URL will be consisting of the API key the base URL and the target currency code. First, we will create an adapter for the list of all the currencies and set it to the Spinner view in the main activity. Then we will call the function onItemSelectedListener and get the selected currency code. We will add this code to the URL along with its other parts. Then we will call the function that makes HTTP requests to get a JSON. We will parse the JSON object to get the required rate of the bitcoin of the selected currency. Below is the code for the MainActivity.java file. Comments are added inside the code to understand the code in more detail.


Came across this Article on Geeks for geeks and  I really enjoyed implementing it !!
