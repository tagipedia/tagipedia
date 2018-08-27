# tagipedia-android

## Installation

### To integrate Tagipedia into your Android project, add it to your gradle:

```gradle
repositories {
    maven { url "https://github.com/tagipedia/tagipedia/raw/master/" }
}

dependencies {
    compile ('com.tagipedia:tagipedia:1.0.1@aar'){
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
        callback.onLoggedEventRecordListener loggedEventRecordListener = new callback.onLoggedEventRecordListener() {
            @Override
            public void onEventLoggedRecord(HashMap hashMap) {
                System.out.print(hashMap);
            }
        };
        tBuilder.setEventLoggerListener(loggedEventRecordListener);
        tBuilder.build();
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
        Topic topic = (Topic) bundle.getSerializable("topic");
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

## Sample code
https://github.com/tagipedia/tagipedia-android-sample
