#AndroidPermissions

A small android application to explain run time permissions.

  ```java
 	
public class MainActivity extends AppCompatActivity {

    GPSTracker gps;
    TextView latlang;
    int PERMISSION_REQUEST = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button get = (Button) findViewById(R.id.get);
        latlang = (TextView) findViewById(R.id.latlang);

        get.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                //request for location data
                GetLocation();
            }
        });
    }

    public void GetLocation() {

        //check for android version
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            //check for permission granted or not
            if (ActivityCompat.checkSelfPermission(MainActivity.this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
                //request permission
                RequestPermission();
            } else {
                //get location data
                Setlatlang();
            }

        } else {
            //get location data
            Setlatlang();
        }
    }

    //request permission
    private void RequestPermission() {
        final String[] permissions = {Manifest.permission.ACCESS_FINE_LOCATION};
        ActivityCompat.requestPermissions(MainActivity.this, permissions,
                PERMISSION_REQUEST);
    }

    //get location data
    public void Setlatlang() {
        gps = new GPSTracker(MainActivity.this);
        if (gps.canGetLocation() && gps.canGetNetwork()) {

            String latlang_data = "Lat:" + String.valueOf(gps.getLatitude()) + "\n" + "Lang:" + String.valueOf(gps.getLongitude());
            latlang.setText(latlang_data);
            latlang.setVisibility(View.VISIBLE);

        } else {
            //show message if gps or internet not enabled
            gps.showNetworkSettingsAlert();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode,
                                           String permissions[], int[] grantResults) {

        if (requestCode == 0) {
            // If request is cancelled, the result arrays are empty.
            if (grantResults.length > 0
                    && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                // permission was granted,
                // you can get location data
                GetLocation();
                Toast.makeText(this, "Permission granted", Toast.LENGTH_LONG).show();
            } else {
                // permission denied.
                //show message
                Toast.makeText(this, "you can't get location data", Toast.LENGTH_LONG).show();
            }
        }

    }
```
## License

  [MIT](LICENSE)
