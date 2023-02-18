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

<resources>
    <string name="app_name">Am I Rich?</string>
    <string name="label_default_text">304.74</string>
    <string name="label_error_text">Error</string>
    <string name="base">Base Currency</string>
    <string name="imageview_desc">Bitcoin Logo</string>
  
    <string-array name="currency_array">
        <item>AUD</item>
        <item>BRL</item>
        <item>CAD</item>
        <item>CNY</item>
        <item>EUR</item>
        <item>GBP</item>
        <item>HKD</item>
        <item>JPY</item>
        <item>PLN</item>
        <item>RUB</item>
        <item>SEK</item>
        <item>USD</item>
        <item>ZAR</item>
    </string-array>
      
</resources>

## Step 3: Working with the activity_main.xml file

The XML codes are used to build the structure of the activity as well as its styling part. It contains an ImageView at the very top of the activity to display the logo of the app. Then it contains a TextView to display the bitcoin rate in the center of the activity. At last, we have a Spinner at the bottom of the activity to display the list of currencies from which the user can choose. This is a single activity application. Below is the code for the activity_main.xml file.

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/bkgndColour"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    tools:context="com.example.bitcointracker.MainActivity">
  
    <TextView
        android:id="@+id/priceLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
        android:text="@string/label_default_text"
        android:textColor="@color/fontColour"
        android:textSize="45sp"
        android:textStyle="bold" />
  
    <ImageView
        android:id="@+id/logoImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true"
        android:contentDescription="@string/imageview_desc"
        android:src="@drawable/bitcoin_image" />
  
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/priceLabel"
        android:gravity="center_vertical|center_horizontal"
        android:orientation="horizontal">
  
        <TextView
            android:id="@+id/textView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:layout_marginLeft="10dp"
            android:layout_marginRight="10dp"
            android:text="@string/base"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:textSize="30sp"
            android:textStyle="bold" />
  
        <Spinner
            android:id="@+id/currency_spinner"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:dropDownSelector="@color/fontColour"
            android:gravity="center_horizontal"
            android:spinnerMode="dropdown" />
          
    </LinearLayout>
  
</RelativeLayout>

## Step 4: Create new layout resource files

For the spinner to display the list we also need to create a spinner item’s XML layout as well as its item’s layout for the adapter. Add the below codes in app> res > layout > spinner_dropdown_item.xml.

<?xml version="1.0" encoding="utf-8"?>
<CheckedTextView 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@android:id/text1"
    style="?android:attr/spinnerDropDownItemStyle"
    android:layout_width="match_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:background="@drawable/color_selector"
    android:ellipsize="marquee"
    android:paddingLeft="10dp"
    android:paddingRight="10dp"
    android:singleLine="true"
    android:text="@string/label_error_text"
    android:textColor="@color/black"
    android:textSize="30sp" />
spinner_item.xml file:

<?xml version="1.0" encoding="utf-8"?>
<TextView 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="start"
    android:padding="10dip"
    android:text="@string/label_error_text"
    android:textColor="@color/black"
    android:textSize="30sp"
    android:textStyle="bold" />

## Step 5: Working with the MainActivity.java file

In the java file, we will create a function that will make HTTP requests from the URL. The URL will be consisting of the API key the base URL and the target currency code. First, we will create an adapter for the list of all the currencies and set it to the Spinner view in the main activity. Then we will call the function onItemSelectedListener and get the selected currency code. We will add this code to the URL along with its other parts. Then we will call the function that makes HTTP requests to get a JSON. We will parse the JSON object to get the required rate of the bitcoin of the selected currency. Below is the code for the MainActivity.java file. Comments are added inside the code to understand the code in more detail.

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.AdapterView;
import android.widget.AdapterView.OnItemSelectedListener;
import android.widget.ArrayAdapter;
import android.widget.Spinner;
import android.widget.TextView;
  
import androidx.appcompat.app.AppCompatActivity;
  
import com.loopj.android.http.AsyncHttpClient;
import com.loopj.android.http.JsonHttpResponseHandler;
  
import org.json.JSONException;
import org.json.JSONObject;
  
import java.io.IOException;
  
import cz.msebera.android.httpclient.Header;
  
public class MainActivity extends AppCompatActivity {
  
    // Constants:
    // TODO: Create the base URL
    private final String BASE_URL = "http://api.coinlayer.com/live?access_key=";
  
    // Member Variables:
    TextView mPriceTextView;
  
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
  
        mPriceTextView = (TextView) findViewById(R.id.priceLabel);
        Spinner spinner = (Spinner) findViewById(R.id.currency_spinner);
  
        // Create an ArrayAdapter using the String array and a spinner layout
        final ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,
                R.array.currency_array, R.layout.spinner_item);
  
        // Specify the layout to use when the list of choices appears
        adapter.setDropDownViewResource(R.layout.spinner_dropdown_item);
  
        // Apply the adapter to the spinner
        spinner.setAdapter(adapter);
  
        // TODO: Set an OnItemSelected listener on the spinner
        spinner.setOnItemSelectedListener(new OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
                String publicKey = "cd9ebbd0c5c20340b9d638e409f41fb1";
                String finalUrl = BASE_URL + publicKey + "&TARGET=" + adapterView.getItemAtPosition(i) + "&symbols=BTC";
                Log.d("Clima", "Request fail! Status code: " + finalUrl);
                try {
                    letsDoSomeNetworking(finalUrl);
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }
  
            @Override
            public void onNothingSelected(AdapterView<?> adapterView) {
            }
        });
    }
  
    // TODO: complete the letsDoSomeNetworking() method
    private void letsDoSomeNetworking(String url) throws IOException, JSONException {
        AsyncHttpClient client = new AsyncHttpClient();
        client.get(url, new JsonHttpResponseHandler() {
  
            @Override
            public void onSuccess(int statusCode, Header[] headers, JSONObject response) {
                // called when response HTTP status is "200 OK"
                Log.d("Clima", "JSON: " + response.toString());
                try {
                    JSONObject price = response.getJSONObject("rates");
                    String object = price.getString("BTC");
                    mPriceTextView.setText(object);
                } catch (JSONException E) {
                    E.printStackTrace();
                }
            }
  
            @Override
            public void onFailure(int statusCode, Header[] headers, Throwable e, JSONObject response) {
                // called when response HTTP status is "4XX" (eg. 401, 403, 404)
                Log.d("Clima", "Request fail! Status code: " + statusCode);
                Log.d("Clima", "Fail response: " + response);
                Log.e("ERROR", e.toString());
            }
        });
    }
}
Came across this Article on Geeks for geeks and  I really enjoyed implementing it !!
