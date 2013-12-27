---
layout: post
title: "在Android里面进行GPS定位"
description: ""
category: ""
tags: []
comments: true
---
###1.添加权限

```
#AndroidManifest.xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```


###2. 创建自己的LcationListener

```
//BestLocationListener.java

import android.location.Location;
import android.location.LocationListener;
import android.location.LocationManager;
import android.os.Bundle;
import java.util.Date;
import java.util.List;
import java.util.Observable;
public class BestLocationListener extends Observable implements LocationListener {
    private static final String TAG = "BestLocationListener";
    public static final long LOCATION_UPDATE_MIN_TIME = 0;
    public static final long LOCATION_UPDATE_MIN_DISTANCE = 0;
    public static final long SLOW_LOCATION_UPDATE_MIN_TIME = 1000 * 60 * 5;
    public static final long SLOW_LOCATION_UPDATE_MIN_DISTANCE = 50;
    public static final float REQUESTED_FIRST_SEARCH_ACCURACY_IN_METERS = 100.0f;
    public static final int REQUESTED_FIRST_SEARCH_MAX_DELTA_THRESHOLD = 1000 * 60 * 5;
    private static final long LOCATION_UPDATE_MAX_DELTA_THRESHOLD = 1000 * 60 * 5;
    private Location mLastLocation;
    public BestLocationListener() {
        super();
    }
    @Override
    public void onLocationChanged(Location location) {
        updateLocation(location);
    }
    @Override
    public void onProviderDisabled(String provider) {
        // do nothing.
    }
    @Override
    public void onProviderEnabled(String provider) {
        // do nothing.
    }
    @Override
    public void onStatusChanged(String provider, int status, Bundle extras) {
        // do nothing.
    }
    synchronized public void onBestLocationChanged(Location location) {
        mLastLocation = location;
        setChanged();
        notifyObservers(location);
    }
    synchronized public Location getLastKnownLocation() {
        return mLastLocation;
    }
    public void updateLocation(Location location) {
        // Cases where we only have one or the other.
        if (location != null && mLastLocation == null) {
            onBestLocationChanged(location);
            return;
        } else if (location == null) {
            return;
        }
        long now = new Date().getTime();
        long locationUpdateDelta = now - location.getTime();
        long lastLocationUpdateDelta = now - mLastLocation.getTime();
        boolean locationIsInTimeThreshold = locationUpdateDelta <= LOCATION_UPDATE_MAX_DELTA_THRESHOLD;
        boolean lastLocationIsInTimeThreshold = lastLocationUpdateDelta <= LOCATION_UPDATE_MAX_DELTA_THRESHOLD;
        boolean locationIsMostRecent = locationUpdateDelta <= lastLocationUpdateDelta;
        boolean accuracyComparable = location.hasAccuracy() || mLastLocation.hasAccuracy();
        boolean locationIsMostAccurate = false;
        if (accuracyComparable) {
            // If we have only one side of the accuracy, that one is more
            // accurate.
            if (location.hasAccuracy() && !mLastLocation.hasAccuracy()) {
                locationIsMostAccurate = true;
            } else if (!location.hasAccuracy() && mLastLocation.hasAccuracy()) {
                locationIsMostAccurate = false;
            } else {
                // If we have both accuracies, do a real comparison.
                locationIsMostAccurate = location.getAccuracy() <= mLastLocation.getAccuracy();
            }
        }

        // Update location if its more accurate and w/in time threshold or if
        // the old location is
        // too old and this update is newer.
        if (accuracyComparable && locationIsMostAccurate && locationIsInTimeThreshold) {
            onBestLocationChanged(location);
        } else if (locationIsInTimeThreshold && !lastLocationIsInTimeThreshold) {
            onBestLocationChanged(location);
        }
    }
    public boolean isAccurateEnough(Location location) {
        if (location != null && location.hasAccuracy()
                && location.getAccuracy() <= REQUESTED_FIRST_SEARCH_ACCURACY_IN_METERS) {
            long locationUpdateDelta = new Date().getTime() - location.getTime();
            if (locationUpdateDelta < REQUESTED_FIRST_SEARCH_MAX_DELTA_THRESHOLD) {
                return true;
            }
        }
        return false;
    }
    public void register(LocationManager locationManager, boolean gps) {
        long updateMinTime = SLOW_LOCATION_UPDATE_MIN_TIME;
        long updateMinDistance = SLOW_LOCATION_UPDATE_MIN_DISTANCE;
        if (gps) {
            updateMinTime = LOCATION_UPDATE_MIN_TIME;
            updateMinDistance = LOCATION_UPDATE_MIN_DISTANCE;
        }
        List<String> providers = locationManager.getProviders(true);
        int providersCount = providers.size();
        for (int i = 0; i < providersCount; i++) {
            String providerName = providers.get(i);
            if (locationManager.isProviderEnabled(providerName)) {
                updateLocation(locationManager.getLastKnownLocation(providerName));
            }
            // Only register with GPS if we've explicitly allowed it.
            if (gps || !LocationManager.GPS_PROVIDER.equals(providerName) || !LocationManager.NETWORK_PROVIDER.equals(providerName)) {
                locationManager.requestLocationUpdates(providerName, updateMinTime,
                        updateMinDistance, this);
            }
        }
    }

    public void unregister(LocationManager locationManager) {
        locationManager.removeUpdates(this);
    }
}

```

###3.在Activity中去使用

```
//MainActivity.java
public void onCreate(Bundle savedInstanceState){
//...
    locationManager = (LocationManager) getSystemService(LOCATION_SERVICE);
    mBestLocationListener = new BestLocationListener();
    mBestLocationListener.register(locationManager, true);
    //使用getLastKnownLocation() 获取经纬度
    Location location = mBestLocationListener.getLastKnownLocation();

//...
}

@Override
protected void onResume() {
//注册Listener
   mBestLocationListener.register(locationManager, true);
   super.onResume();
}

@Override
protected void onDestroy() {
//注销Listener
   mBestLocationListener.unregister(locationManager);
   super.onDestroy();
}

@Override
protected void onPause() {
//注销Listener
   mBestLocationListener.unregister(locationManager);
   super.onPause();
}
```
