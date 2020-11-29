# xExplorer
browse files inside SD Card and Storage

## Requirements

1 - Add permission to Manifest

```
 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
 <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

2 .If your android version is grather than 8 add application to Manifest

```
android:requestLegacyExternalStorage="true"
```

3. Request permission for read and write in your activity

```
String[] per2 = {Manifest.permission.WRITE_EXTERNAL_STORAGE};

 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


int camper2 = ActivityCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE);

if (camper2 != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, per2, 100);
        }else {
        
        }
        
      
      
 @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if(requestCode==100&& grantResults[0]==PackageManager.PERMISSION_GRANTED) {
            
        }
    }
        
```

## Usage

###### For Activity

1. Add layout

```
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SDCardFragment">

    <!-- TODO: Update blank fragment layout -->
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">



        <ProgressBar
            android:id="@+id/progressBar"
            style="?android:attr/progressBarStyle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"/>


        <ListView
            android:layout_above="@id/rl_adView"
            android:id="@+id/listView"
            android:background="#ee918A8A"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <RelativeLayout
            android:layout_gravity="center"
            android:gravity="center"
            android:background="#ee918A8A"
            android:id="@+id/rl_adView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_alignParentBottom="true"/>
    </RelativeLayout>

</FrameLayout>
```
2. Add Activity

```
ArrayList<String> pathList;
ArrayList<String> pathList_name;
ListView listView;
ProgressBar progressBar;

File file=new File("/storage");


 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_select);

        listView = view.findViewById(R.id.listView);
        pathList = new ArrayList<>();
        pathList_name = new ArrayList<>();
        progressBar=view.findViewById(R.id.progressBar);
        new Loading().execute();
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
                    @Override
                    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                                  
                        Intent i = new Intent(MainActivity.this, yourActivity.class);
                        i.putExtras("path",pathList.get(position));          
                        startActivity(i);
                    }
                });
}

 public void SearchFile(File dir) {

        File[] files = dir.listFiles();
        //Collections.reverse(Arrays.asList(files));

        if (files != null) {
            //for each

            for (File file : files) {
                if (file.isDirectory()) {
                    SearchFile(file);

                } 
                // do you want extension wrirte 
                else if (file.getAbsolutePath().endsWith(".mp4")) {
                    pathList.add(file.getAbsolutePath());
                    String path=file.getAbsolutePath();
                    String filename=path.substring(path.lastIndexOf("/")+1);

                    pathList_name.add(filename);
                    // Log.d("pathList_name",pathList_name.toString());

                }
            }
        }

    }


 public class Loading extends AsyncTask<Void, Void, Void> {

        @Override
        protected Void doInBackground(Void... voids) {
            
            // This search from phone storage
            ///SearchFile(Environment.getExternalStorageDirectory());

            //This search from sd card
            SearchFile(file);
          

            return null;
        }

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            

        }

        @Override
        protected void onPostExecute(Void aVoid) {
            super.onPostExecute(aVoid);      
            progressBar.setVisibility(View.INVISIBLE);
            ArrayAdapter arrayAdapter=new ArrayAdapter(getContext(),android.R.layout.simple_list_item_1,pathList_name);
            listView.setAdapter(arrayAdapter);

        }
    }

```

###### For Fragment

1. Add anim

slide_from_left
```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" android:duration="250" android:interpolator="@android:anim/accelerate_decelerate_interpolator">
    <translate
        android:fromXDelta="-100%"
        android:toXDelta="0%"
        android:fromYDelta="0%"
        android:toYDelta="0%"/>

</set>
```

slide_from_right
```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" android:duration="250" android:interpolator="@android:anim/accelerate_decelerate_interpolator">
    <translate
        android:fromXDelta="100%"
        android:toXDelta="0%"
        android:fromYDelta="0%"
        android:toYDelta="0%"/>

</set>
```

slideout_from_right
```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" android:duration="250" android:interpolator="@android:anim/accelerate_decelerate_interpolator">
    <translate
        android:fromXDelta="0%"
        android:toXDelta="100%"
        android:fromYDelta="0%"
        android:toYDelta="0%"/>

</set>
```

slideout_from_left
```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" android:duration="250" android:interpolator="@android:anim/accelerate_decelerate_interpolator">
    <translate
        android:fromXDelta="0%"
        android:toXDelta="-100%"
        android:fromYDelta="0%"
        android:toYDelta="0%"/>

</set>
```

2. Add layout

activity_my_video

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MyVideoActivity">

    <FrameLayout
        android:id="@+id/myvideo_framelayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

    </FrameLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

fragment_sd_card
```
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SDCardFragment">

    <!-- TODO: Update blank fragment layout -->
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MyVideoActivity">



        <ProgressBar
            android:id="@+id/progressBar"
            style="?android:attr/progressBarStyle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"/>


        <ListView
            android:layout_above="@id/rl_adView"
            android:id="@+id/listView"
            android:background="#EEE8E8E8"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <RelativeLayout
            android:layout_gravity="center"
            android:gravity="center"
            android:background="#EEE8E8E8"
            android:id="@+id/rl_adView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_alignParentBottom="true"/>
    </RelativeLayout>

</FrameLayout>
```
fragment_phone

```
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".PhoneFragment">

    <!-- TODO: Update blank fragment layout -->
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MyVideoActivity">



        <ProgressBar
            android:id="@+id/progressBar"
            style="?android:attr/progressBarStyle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"/>


        <ListView
            android:layout_above="@id/rl_adView"
            android:id="@+id/listView"
            android:background="#EEE8E8E8"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <RelativeLayout
            android:layout_gravity="center"
            android:gravity="center"
            android:background="#EEE8E8E8"
            android:id="@+id/rl_adView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_alignParentBottom="true"/>
    </RelativeLayout>

</FrameLayout>
```

3.Add Activity

MyVideoActivity
```
public class MyVideoActivity extends AppCompatActivity {

    boolean isStoragePH;
    private FrameLayout frameLayout;
    public static boolean isLoading=false;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_my_video);
        
        frameLayout=findViewById(R.id.myvideo_framelayout);
        setDefaultFragment(new PhoneFragment());
    }
    
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main, menu);
        return super.onCreateOptionsMenu(menu);
    }
    
    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
        switch (item.getItemId()) {
            case R.id.play:

                if(isStoragePH){
                    if(isLoading==false) {
                        isStoragePH = false;
                        item.setIcon(R.drawable.ic_sd_card);
                        setFragmentLeft(new PhoneFragment());
                    }
                }else {
                    if(isLoading==false) {
                        isStoragePH = true;
                        item.setIcon(R.drawable.ic_smartphone);
                        setFragmentRight(new SDCardFragment());
                    }
                }

                break;

        }
        return super.onOptionsItemSelected(item);
    }
    
    private void setDefaultFragment(Fragment fragment) {
        FragmentTransaction fragmentTransaction=getSupportFragmentManager().beginTransaction();
        fragmentTransaction.replace(frameLayout.getId(),fragment);
        fragmentTransaction.commit();
    }
    private void setFragmentLeft(Fragment fragment) {
        FragmentTransaction fragmentTransaction=getSupportFragmentManager().beginTransaction();
        fragmentTransaction.setCustomAnimations(R.anim.slide_from_left,R.anim.slideout_from_right);
        fragmentTransaction.replace(frameLayout.getId(),fragment);
        fragmentTransaction.commit();
    }


    private void setFragmentRight(Fragment fragment) {
        FragmentTransaction fragmentTransaction=getSupportFragmentManager().beginTransaction();
        fragmentTransaction.setCustomAnimations(R.anim.slide_from_right,R.anim.slideout_from_left);
        fragmentTransaction.replace(frameLayout.getId(),fragment);
        fragmentTransaction.commit();
    }

}


```

4.Add Fragment

PhoneFragment
```

public class PhoneFragment extends Fragment {

    
    private static final String ARG_PARAM1 = "param1";
    private static final String ARG_PARAM2 = "param2";

   
    private String mParam1;
    private String mParam2;
    ArrayList<String> pathList;
    ArrayList<String> pathList_name;
    ListView listView;
    ProgressBar progressBar;

    public PhoneFragment() {
        // Required empty public constructor
    }

    public static PhoneFragment newInstance(String param1, String param2) {
        PhoneFragment fragment = new PhoneFragment();
        Bundle args = new Bundle();
        args.putString(ARG_PARAM1, param1);
        args.putString(ARG_PARAM2, param2);
        fragment.setArguments(args);
        return fragment;
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (getArguments() != null) {
            mParam1 = getArguments().getString(ARG_PARAM1);
            mParam2 = getArguments().getString(ARG_PARAM2);
        }
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        View view=inflater.inflate(R.layout.fragment_phone, container, false);

        listView = view.findViewById(R.id.listView);
        pathList = new ArrayList<>();
        pathList_name = new ArrayList<>();
        progressBar=view.findViewById(R.id.progressBar);
     
        isLoading=true;
        new Loading().execute();
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            
            
              Intent i = new Intent(getActivity(), yourActivity.class);
              i.putExtras("path",pathList.get(position));          
              startActivity(i);
              
            }
        });
        return view;
    }

    public void SearchFile(File dir) {

        File[] files = dir.listFiles();
        //Collections.reverse(Arrays.asList(files));

        if (files != null) {
            //for each

            for (File file : files) {
                if (file.isDirectory()) {
                    SearchFile(file);

                } 
                // do you want extension wrirte 
                else if (file.getAbsolutePath().endsWith(".mp4")) {
                    pathList.add(file.getAbsolutePath());
                    String path=file.getAbsolutePath();
                    String filename=path.substring(path.lastIndexOf("/")+1);

                    pathList_name.add(filename);
                    // Log.d("pathList_name",pathList_name.toString());

                }
            }
        }

    }
    public class Loading extends AsyncTask<Void, Void, Void> {

        @Override
        protected Void doInBackground(Void... voids) {
            
            SearchFile(Environment.getExternalStorageDirectory());
           

            return null;
        }

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
           

        }

        @Override
        protected void onPostExecute(Void aVoid) {
            super.onPostExecute(aVoid);
          
            progressBar.setVisibility(View.INVISIBLE);
            isLoading=false;
         
            ArrayAdapter arrayAdapter=new ArrayAdapter(getContext(),android.R.layout.simple_list_item_1,pathList_name);
            listView.setAdapter(arrayAdapter);

        }
    }
}
```

SDCardFragment

```

public class SDCardFragment extends Fragment {

   
    private static final String ARG_PARAM1 = "param1";
    private static final String ARG_PARAM2 = "param2";

    
    private String mParam1;
    private String mParam2;
    ArrayList<String> pathList;
    ArrayList<String> pathList_name;
    ListView listView;
    ProgressBar progressBar;

    File file=new File("/storage");

    public SDCardFragment() {
        // Required empty public constructor
    }

   
    public static SDCardFragment newInstance(String param1, String param2) {
        SDCardFragment fragment = new SDCardFragment();
        Bundle args = new Bundle();
        args.putString(ARG_PARAM1, param1);
        args.putString(ARG_PARAM2, param2);
        fragment.setArguments(args);
        return fragment;
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (getArguments() != null) {
            mParam1 = getArguments().getString(ARG_PARAM1);
            mParam2 = getArguments().getString(ARG_PARAM2);
        }
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        View view=inflater.inflate(R.layout.fragment_sd_casrd, container, false);

        listView = view.findViewById(R.id.listView);
        pathList = new ArrayList<>();
        pathList_name = new ArrayList<>();
        progressBar=view.findViewById(R.id.progressBar);
        isLoading=true;
        new Loading().execute();
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
               
              Intent i = new Intent(getActivity(), yourActivity.class);
              i.putExtras("path",pathList.get(position));          
              startActivity(i);
            }
        });

        return view;
    }

    public void SearchFile(File dir) {

        File[] files = dir.listFiles();
        //Collections.reverse(Arrays.asList(files));

        if (files != null) {
            //for each

            for (File file : files) {
                if (file.isDirectory()) {
                    SearchFile(file);

                }
                // do you want extension wrirte 
                else if (file.getAbsolutePath().endsWith(".mp4")) {
                    pathList.add(file.getAbsolutePath());
                    String path=file.getAbsolutePath();
                    String filename=path.substring(path.lastIndexOf("/")+1);

                    pathList_name.add(filename);
                    // Log.d("pathList_name",pathList_name.toString());

                }
            }
        }

    }
    public class Loading extends AsyncTask<Void, Void, Void> {

        @Override
        protected Void doInBackground(Void... voids) {
           
            SearchFile(file);
            

            return null;
        }

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            

        }

        @Override
        protected void onPostExecute(Void aVoid) {
            super.onPostExecute(aVoid);
          
            progressBar.setVisibility(View.INVISIBLE);
            isLoading=false;
            ArrayAdapter arrayAdapter=new ArrayAdapter(getContext(),android.R.layout.simple_list_item_1,pathList_name);
            listView.setAdapter(arrayAdapter);

        }
    }
}
```


