# tagipedia-android

## Installation

### To integrate Tagipedia into your Android project, add it to your gradle:

```gradle
repositories {
    maven { url "https://github.com/tagipedia/tagipedia/raw/master/" }
}

dependencies {
    compile ('com.tagipedia:tagipedia:1.0.0@aar'){
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
        // you can use topic getters to display it in view
    }

}
```

### Hint: to ask user to enable bluetooth we have method for it.
```java
TUtils.showBluetoothDialog(this, "Open bluetooth" , "we use bluetooth for .... please open it");
```




