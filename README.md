# xamarin-sdk

## Table of Contents

* [Quick Integration Guide](#qs-basic-integration)
    * [Retrieve your app token](#qs-retrieve-app-token)
    * [Initialize the SDK](#qs-initialize-sdk)
    * [Events Tracking](#qs-track-events)
    * [Revenue Events Tracking](#qs-track-event-with-currencey)
    * [Pass the custom params in events](#qs-add-custom-parms-event)
    * [SDK Signing](#qs-sdk-signing)
* [Proguard Settings](#qs-progaurd-settings)


## <a id="qs-add-trackier-sdk"></a>Quick Integration Guide

We have created a example app for the Xamarin SDK integration. 

Please check the [Example](https://github.com/trackier/react-native-sdk/tree/main/example) directory for know to how the `Trackier SDK` can be integrated.


## <a id="qs-basic-integration"></a>Integrate Xamarin SDK to your app

For integration, you need to add the trackier plugin in your project. 

To Embed Tarckier SDK manually to the project

* Copy trackier_xamarin_sdk.dll into your project.
* On Xamarin Studio go to References and click on Edit References.
* Go to .Net Assembly tab and click on Browse… button
* Locate trackier_xamarin_sdk.dll and chose it.

Please check the below screenshot 

Screenshot[1]

<img width="1000" alt="Screenshot 2022-08-22 at 4 33 19 PM" src="https://user-images.githubusercontent.com/16884982/185906307-7c65a830-fb10-44ac-bbbc-75444c1a4702.png">

After the above process, you need to add the below `packages` in your project.

* Square.Moshi.Adapters
* Square.Moshi.Kotlin 
* Square.Retrofit2.ConverterMoshi
* Xamarin.AndroidX.Work.Work.Runtime.Ktx
* Xamarin.GooglePlayServices.Ads.ldentifier
* Xamarin.Android.Binding.InstallReferrer

For adding the above `packages` to your project follow the below steps

1. Right click on the `packages` and select `Manage NuGet Packages`
2. Search the above packages on the search option.
3. After finding the package, click on the Add Package

Please check the below screenshot 

Screenshot[2]

<img width="1000" alt="Screenshot 2022-08-22 at 4 27 17 PM" src="https://user-images.githubusercontent.com/16884982/185905789-40c27644-567b-4a0f-af37-867b7b27cda2.png">


### <a id="qs-retrieve-app-token"></a>Retrieve your app token

1. Login to your Trackier MMP account.
2. Select the application from dashboard which you want to get the app token for.
3. Go to SDK Integration via the left side navigation menu.
4. Copy the SDK Key there to be used as the `"app_token"`.


### <a id="qs-initialize-sdk"></a>SDK integration in app

You should use the following import statement on top of your `c#` file:
```c#
using Com.Trackier.Sdk;

```

In your `MainApplication.cs` file, add the following code to initialize the Trackier SDK:

```c#

using System;
using Android.App;
using Android.Content;
using Android.OS;
using Android.Runtime;
using Com.Trackier.Sdk;
using Android.Util;

namespace SampleApp.Resources
{
    [Application]
    public partial class MainApplication : Application
    {
        internal static Context ActivityContext { get; private set; }
        public Context Application { get; private set; }

        public MainApplication(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

        public override void OnCreate()
        {
            base.OnCreate();
          
            try
            {
                /* While Initializing the SDK, You need to pass the three parameter in the TrackierSDKConfig.
                 * In First argument, you need to pass the Trackier SDK api key
                 * In third argument, you need to pass the environment which can be either "EnvironmentDevelopment", "EnvironmentProduction". */
                TrackierSDKConfig trackierSDK = new TrackierSDKConfig(ApplicationContext, "0455721b-XXXX-XXXX-805e-5XXXXXd910a", "development");
                TrackierSDK.Initialize(trackierSDK);
                TrackierSDK.TrackAsOrganic(true);

            }
            catch (NullReferenceException e)
            {
                String s = e.StackTrace;
                Log.Debug("logTrackier", "Debug" + s);

            }
        }
    }
}

```

Check below the screenshot of above code 

Screenshot[1]

<img width="1000" alt="Screenshot 2022-08-22 at 4 51 37 PM" src="https://user-images.githubusercontent.com/16884982/185909581-74560171-32ee-4e7a-83f2-ee27bfeb0227.png">


### <a id="qs-track-events"></a>Events Tracking

<a id="qs-retrieve-event-id"></a>Trackier events trackings enable to provides the insights into how to user interacts with your app. 
Trackier SDK easily get that insights data from the app. Just follow with the simple events integration process

Trackier provides the `Built-in events` and `Customs events` on the Trackier panel.

#### **Built-in Events** - 

Predefined events are the list of constants events which already been created on the dashboard. 

You can use directly to track those events. Just need to implements events in the app projects.

Screenshot[2]

<img width="1000" alt="Screenshot 2022-06-10 at 1 23 01 PM" src="https://user-images.githubusercontent.com/16884982/173018185-3356c117-8b9f-46d1-bfd5-c41653f9ac9e.png">

### Example code for calling Built-in events

```c#

using Android.App;
using Android.OS;
using AndroidX.AppCompat.Widget;
using AndroidX.AppCompat.App;
using Com.Trackier.Sdk;

namespace SampleApp
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme.NoActionBar", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);
            Xamarin.Essentials.Platform.Init(this, savedInstanceState);
            SetContentView(Resource.Layout.activity_main);
            eventsTracking();
            Toolbar toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
            SetSupportActionBar(toolbar);
         
        }

        public void eventsTracking()
        {
            //Below are the example of built-in events function calling
            //The arguments - "TTrackierEvent.PURCHASE" passed in the Trackier event class is Events id.

            TrackierEvent trackierevents = new TrackierEvent(TrackierEvent.Purchase);
            trackierevents.Param1 = "XamarinTest1";
            trackierevents.Param2 = "XamarinTest2";
            TrackierSDK.SetUserEmail("abc@gmail.com");
            TrackierSDK.TrackEvent(trackierevents);
        }
    }
}

```

Also check the example app screenshot of above example

Screenshot[3]

<img width="1000" alt="Screenshot 2022-08-22 at 5 10 27 PM" src="https://user-images.githubusercontent.com/16884982/185912933-86e62af9-c0cb-4453-baa8-af04e57a9440.png">

#### **Customs Events** - 

Customs events are created by user as per their required business logic. 

You can create the events in the Trackier dashboard and integrate those events in the app project.

Screenshot[4]

<img width="1000" alt="Screenshot 2022-06-29 at 4 09 37 PM" src="https://user-images.githubusercontent.com/16884982/176417552-a8c80137-aa1d-480a-81a3-ea1e03172868.png">


### Example code for calling Customs Events.

```c#

using Android.App;
using Android.OS;
using AndroidX.AppCompat.Widget;
using AndroidX.AppCompat.App;
using Com.Trackier.Sdk;

namespace SampleApp
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme.NoActionBar", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);
            Xamarin.Essentials.Platform.Init(this, savedInstanceState);
            SetContentView(Resource.Layout.activity_main);
            eventsTracking();
            Toolbar toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
            SetSupportActionBar(toolbar);
         
        }

        public void eventsTracking()
        {
            //Below are the example of built-in events function calling
            //The arguments - "sEMWSCTXeu" passed in the Trackier event class is Events id for AppOpen

            TrackierEvent trackierevents = new TrackierEvent("sEMWSCTXeu");
            trackierevents.Param1 = "XamarinTest1";
            trackierevents.Param2 = "XamarinTest2";
            TrackierSDK.SetUserEmail("abc@gmail.com");
            TrackierSDK.TrackEvent(trackierevents);
        }
    }
}

```

Check below the example screenshot of customs events:-

Screenshots[5]

<img width="1000" alt="Screenshot 2022-08-22 at 5 12 50 PM" src="https://user-images.githubusercontent.com/16884982/185913308-5be46f3e-3e7b-4369-9818-b996b6de1abf.png">


### <a id="qs-track-event-with-currencey"></a>Revenue Event Tracking

Trackier allow user to pass the revenue data which is generated from the app through Revenue events. It is mainly used to keeping record of generating revenue from the app and also you can pass currency as well.

```c#
    
using Android.App;
using Android.OS;
using AndroidX.AppCompat.Widget;
using AndroidX.AppCompat.App;
using Com.Trackier.Sdk;

namespace SampleApp
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme.NoActionBar", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);
            Xamarin.Essentials.Platform.Init(this, savedInstanceState);
            SetContentView(Resource.Layout.activity_main);
            revenueTracking();
            Toolbar toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
            SetSupportActionBar(toolbar);
         
        }

        public void revenueTracking()
        {
            TrackierEvent trackierevents = new TrackierEvent("jYHcuyxWUW");
            trackierevents.Param1 = "XamarinTest1";
            trackierevents.Param2 = "XamarinTest2";
            trackierevents.OrderId = "TestID";
            trackierevents.Revenue = (Java.Lang.Double)2.0;
            trackierevents.CouponCode = "TestCoupons";
            trackierevents.Currency = "INR";
            TrackierSDK.SetUserEmail("abc@gmail.com");
            TrackierSDK.TrackEvent(trackierevents);
        }
    }
}

```

Check below the revenue events calling screenshots.

Screenshot[6]

<img width="1000" alt="Screenshot 2022-08-22 at 5 14 47 PM" src="https://user-images.githubusercontent.com/16884982/185913666-d02c9413-d68d-4c3d-95af-10dff5b67417.png">


### <a id="qs-add-custom-parms-event"></a>Pass the custom params in events

```c#

using Android.App;
using Android.OS;
using AndroidX.AppCompat.Widget;
using AndroidX.AppCompat.App;
using Com.Trackier.Sdk;
using System.Collections.Generic;

namespace SampleApp
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme.NoActionBar", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);
            Xamarin.Essentials.Platform.Init(this, savedInstanceState);
            SetContentView(Resource.Layout.activity_main);
            customsDataPassing();
            Toolbar toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
            SetSupportActionBar(toolbar);
         
        }

        public void customsDataPassing()
        {
            //Below are the example of revenue events function calling
            //The arguments - "sEMWSCTXeu" passed in the event class is Events id for AppOpen.
            TrackierEvent trackierevents = new TrackierEvent("sEMWSCTXeu");
            trackierevents.Param1 = "XamarinTest1";
            trackierevents.Param2 = "XamarinTest2";
            TrackierSDK.SetUserEmail("abc@gmail.com");

            //Passing the custom params in events be like below example
            var map = new Dictionary<string, string>();
            map.Add("name", "abc");
            map.Add("phone", "81xxxxx84");
            trackierevents.Ev = map;
            TrackierSDK.TrackEvent(trackierevents);
        }
    }
}


```

- First create a map.
- Pass its reference to trackierEvent.ev param of event.
- Pass event reference to trackEvent method of TrackierSDK.


### <a id="qs-passing-user-data"></a>Passing User Data to SDK

Trackier allows to pass additional data like Userid, Email, UserName, and UserPhone to SDK so that same can be correlated to the Trackier Data and logs.

Just need to pass the data of User Id, Email Id , UserName, and UserPhone and other additional data to Trackier SDK function which is mentioned below:-


```c#

using Android.App;
using Android.OS;
using AndroidX.AppCompat.Widget;
using AndroidX.AppCompat.App;
using Com.Trackier.Sdk;
using System.Collections.Generic;

namespace SampleApp
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme.NoActionBar", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);
            Xamarin.Essentials.Platform.Init(this, savedInstanceState);
            SetContentView(Resource.Layout.activity_main);
            userData();
            Toolbar toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
            SetSupportActionBar(toolbar);
         
        }

        public void userData()
        {
            //Below are the example of revenue events function calling
            //The arguments - "sEMWSCTXeu" passed in the event class is Events id for AppOpen.
            TrackierEvent trackierevents = new TrackierEvent(TrackierEvent.Login);
            trackierevents.Param1 = "XamarinTest1";
            trackierevents.Param2 = "XamarinTest2";

            //Passing the userData
            TrackierSDK.SetUserEmail("abc@gmail.com");
            TrackierSDK.SetUserId("testID");
            TrackierSDK.SetUserName("testName");
            TrackierSDK.SetUserPhone("8878XX9877");
            TrackierSDK.TrackEvent(trackierevents);
        }
    }
}

```


## <a id="qs-sdk-signing"></a>SDK Signing 

Following below are the steps to retrieve the secretId and secretKey :-

- Login your Trackier Panel and select your application.
- In the Dashboard, click on the `SDK Integration` option on the left side of panel. 
- Under on the SDK Integration, click on the Advanced tab. 
- Under the Advanced tab, you will get the secretId and secretKey.

Please check on the below screenshot

Screenshot[11]

<img width="1000" alt="Screenshot 11" src="https://user-images.githubusercontent.com/16884982/185338826-bcf802d0-c493-4a67-adb3-a9b52bae289e.png">


Check below the example code for passing the secretId and secretKey to the SDK

```c#

using System;
using Android.App;
using Android.Content;
using Android.OS;
using Android.Runtime;
using Com.Trackier.Sdk;
using Android.Util;

namespace SampleApp.Resources
{
    [Application]
    public partial class MainApplication : Application
    {
        internal static Context ActivityContext { get; private set; }
        public Context Application { get; private set; }

        public MainApplication(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

        public override void OnCreate()
        {
            base.OnCreate();
          
            try
            {
                /* While Initializing the SDK, You need to pass the three parameter in the TrackierSDKConfig.
                 * In First argument, you need to pass the Trackier SDK api key
                 * In third argument, you need to pass the environment which can be either "EnvironmentDevelopment", "EnvironmentProduction". */
                TrackierSDKConfig trackierSDK = new TrackierSDKConfig(ApplicationContext, "0455721b-33c5-4c9f-805e-596d818d910a", "development");
                trackierSDK.SetAppSecret("xxxx", "xxxxx"); // Pass the secretId and secretKey
                TrackierSDK.Initialize(trackierSDK);
            }
            catch (NullReferenceException e)
            {
                String s = e.StackTrace;
                Log.Debug("logTrackier", "Debug" + s);

            }
        }
    }
}


```


## <a id="qs-progaurd-settings"></a>Proguard Settings 

If your app is using proguard then add these lines to the proguard config file 

``` 
  -keep class com.trackier.sdk.** { *; }
  -keep class com.google.android.gms.common.ConnectionResult {
      int SUCCESS;
  }
  -keep class com.google.android.gms.ads.identifier.AdvertisingIdClient {
      com.google.android.gms.ads.identifier.AdvertisingIdClient$Info getAdvertisingIdInfo(android.content.Context);
  }
  -keep class com.google.android.gms.ads.identifier.AdvertisingIdClient$Info {
      java.lang.String getId();
      boolean isLimitAdTrackingEnabled();
  }
  -keep public class com.android.installreferrer.** { *; }
  
```
