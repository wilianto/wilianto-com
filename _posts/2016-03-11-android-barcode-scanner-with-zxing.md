---
title: Android Barcode Scanner with Zxing
date: 2016-03-11 09:52:00 Z
layout: post
meta_keywords: android barcode, barcode scanner, android zxing, android scanner
meta_description: Step by step create an android barcode scanner with zxing library
comments: true
---

Barcode and QR Code use to give an unique identity in many sectors now. People use them for ticketing system, inventory system, logictic system, etc. It uses by many people because its simplicity, you only need a scanner to read the data in barcode in just a second.

There are many types of scanners you can use. In fact you can also use your Android device as a scanner and integrate it into your other systems via web service, etc.

This is the example Android apps to read barcode and QR code with Zxing library. First you have to make a new project on Android Studio and then update the depedencies on your build.gradle file.

```
compile 'com.journeyapps:zxing-android-embedded:3.1.0@aar'
compile 'com.google.zxing:core:3.2.0'
```

Sync the project.

Second, make a new layout. There is only a button view on the layout which has an on click event. So when you click the button, the scan mode will active.

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical" android:layout_width="match_parent"
  android:layout_height="match_parent">


  <Button
    android:text="@string/label_scan"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:onClick="scan"/>
</LinearLayout>
{% endhighlight %}

Third, make an activity and show the layout when created.

{% highlight java %}
package com.wilianto.tutorial;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Toast;

import com.google.zxing.integration.android.IntentIntegrator;
import com.google.zxing.integration.android.IntentResult;

/**
 * Created by wilianto on 16-12-3.
 */
public class ScannerActivity extends AppCompatActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_scanner);
  }

  public void scan(View view){
    //show the scanner
    new IntentIntegrator(this).initiateScan();
  }

  @Override
  public void onActivityResult(int requestCode, int resultCode, Intent intent){
    //get activity result here
    IntentResult result = IntentIntegrator.parseActivityResult(requestCode, resultCode, intent);

    if(result == null){
      super.onActivityResult(requestCode, resultCode, intent);
    }else{
      //get the content from barcode
      String barcode = result.getContents();
      //show in a Toast
      Toast.makeText(this, barcode, Toast.LENGTH_LONG).show();
    }
  }
}
{% endhighlight %}

Last, don’t forget to modify your manifests file. You need to add camera permission and your main activity.

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.wilianto.tutorial" >
  <uses-permission android:name="android.permission.CAMERA"/>

  <application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme" >
    <activity
      android:name=".ScannerActivity"
      android:label="@string/app_name" >
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
  </application>
</manifest>
{% endhighlight %}

That’s all. Compile your code and run your application.

Reference:

[https://github.com/journeyapps/zxing-android-embedded](https://github.com/journeyapps/zxing-android-embedded
)
