# Android advertising using beacons

## Installation

### To integrate Tagipedia into your Android project, add it to your gradle:

```gradle
repositories {
    maven { url "https://github.com/tagipedia/tagipedia/raw/master/" }
}

dependencies {
    compile ('com.tagipedia:tagipedia:2.2.5@aar'){
        transitive = true;
    }
}
```

## Usage

```java
public class MyAppContext extends MultiDexApplication {
    public void onCreate() {
        super.onCreate();
        TBuilder tBuilder = new TBuilder(this, "CLIENT_ID", "CLIENT_SECRET", "IDENTIFIER", "UUID");
        // intent that opened when notification pressed
        Intent intent = new Intent(getApplicationContext(), NotificationActivity.class);
        tBuilder.setIntent(intent);
        // set notification icon
        tBuilder.setSmallIcon(R.drawable.ic_launcher);
        // change notify period between different beacons notification in millisecond
        // DEFAULT: 10 * 60 * 1000 (10 minutes)
        tBuilder.setDifferentBeaconNotifyPeriod(1000);
        // change notify period between same beacons notification in millisecond
        // DEFAULT: 30 * 60 * 1000 (30 minutes)
        tBuilder.setSameBeaconNotifyPeriod(20000);
        Callback.OnLoggedEventRecordListener loggedEventRecordListener = new Callback.OnLoggedEventRecordListener() {
            @Override
            public void onEventLoggedRecord(HashMap hashMap) {
                System.out.print(hashMap);
            }
        };
        tBuilder.setEventLoggerListener(loggedEventRecordListener);
        // to receive feature_id if the action of ad is navigate to map location
        // you should open the map and navigate to the location that recived
        Callback.OnMapButtonPressedListener onMapButtonPressedListener = new Callback.OnMapButtonPressedListener() {
            @Override
            public void onMapButtonPressed(HashMap hashMap) {
                System.out.print(hashMap);
                // dispatch to tagipedia maps to navigate to location should be like this
                // LinkedHashMap<String, Object> navigationParams = new LinkedHashMap<String, Object>();
                // navigationParams.put("route_to", (String)hashMap("feature_id"));
                // new HashMap<String, Object>(){{
                // put("type", "SHOW_NAVIGATION_DIALOG");
                // put("navigation_params", navigationParams);
                // }}
            }
        };
        tBuilder.setMapButtonPressedListener(onMapButtonPressedListener);
        
        //to receive when user enter beacon region
        Callback.OnEnterBeaconRegionListener onEnterBeaconRegion = new Callback.OnEnterBeaconRegionListener(){
            @Override
            public void onEnterBeaconRegion(HashMap data) {
                System.out.print(data);
            }
        };
        tBuilder.setEnterBeaconRegionListener(onEnterBeaconRegion);
        //to receive when user exit beacon region
        Callback.OnExitBeaconRegionListener onExitBeaconRegion = new Callback.OnExitBeaconRegionListener(){
            @Override
            public void onExitBeaconRegion(HashMap data) {
                System.out.print(data);
                //time_spent in milliseconds
                //enter_date and exit_date in millisecond since 1970
            }
        };
        tBuilder.setExitBeaconRegionListener(onExitBeaconRegion);
        
        //to monitoring specific regions
        tBuilder.setRegions(new ArrayList<TRegion>(Arrays.asList(new TRegion(UUID, major, minor), ...)));
        tBuilder.build();
        //to register user with interests
        //this will show ads based on matching between ad interests and user interests otherwise it will show ads that was created without interests
        //String[] interests
        TBuilder.identifyUser("USER_NAME", interests);
    }
}
```

```java
public class NotificationActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_notification);
        Bundle bundle = getIntent().getExtras();
        HashMap topic = (HashMap) bundle.getSerializable("topic");
        System.out.println("Topic " + topic);
        // you can show ad with its assigned template
        TUtils.showAdDialog(this,topic);
        // you can use topic getters to display it in view
    }

}
```

### Hint: to ask user to enable bluetooth we have method for it.
```java
TUtils.showBluetoothDialog(this, "Open bluetooth" , "we use bluetooth for .... please open it");
```


### Hint: to ask user to enable location for android bigger than or equal 23 we have method for it.
```java
TUtils.showLocationDialog(this, "Open Location" , "we use location for .... please open it");
```

### Hint: to show ad with its assigned template.
```java
TUtils.showAdDialog(this,topic);
```

### Hint: to logout user..
```java
TBuilder.logoutUser();
```
### Hint: to receive current region user located at. you should set the regions (ArrayList of TRegion ) before build
```java
tBuilder.setRegions(new ArrayList<TRegion>(Arrays.asList(new TRegion(UUID, major, minor), ...)));
```

## Sample code
https://github.com/tagipedia/tagipedia-android-sample
