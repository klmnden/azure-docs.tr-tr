---
title: Android şekillere eşler Azure haritalar içinde ekleyin | Microsoft Docs
description: Azure haritalar Android SDK'sını kullanarak bir harita için şekilleri ekleme
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: c53a3e01d471f2ca9b0878c374b00ce83848ca28
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64871010"
---
# <a name="add-a-shape-to-a-map-using-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını kullanarak bir harita için Şekil ekleme

Bu makalede Azure haritalar Android SDK'sını kullanarak bir haritadaki şekillere nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede işlemi tamamlamak için yüklemeniz gerekir [Azure haritalar Android SDK'sı](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) bir haritayı yükleyin.


## <a name="add-a-line-to-the-map"></a>Haritayı bir satır ekleyin

Harita kullanarak bir satır ekleyebilirsiniz bir **çizgi katmanı**, harita üzerinde bir satır eklemek için aşağıdaki adımları izleyin.

1. Düzen **res > Düzen > activity_main.xml** bir aşağıdaki gibi görünür:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >
    
        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_centerLat="40.743270"
            app:mapcontrol_centerLng="-74.004420"
            app:mapcontrol_zoom="12"
            />
    
    </FrameLayout>
    ```

2. İçine aşağıdaki kod parçacığı kopyalama **onCreate()** yöntemi, `MainActivity.java` sınıfı.

    ```Java
    mapControl.onReady(map -> {

        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a list of points.
        List<Point> points = Arrays.asList(
            Point.fromLngLat(-73.972340, 40.743270),
            Point.fromLngLat(-74.004420, 40.756800));
    
        //Create a LineString feature and add it to the data source.
        dataSource.add(LineString.fromLngLats(points));
    
        //Create a line layer and add it to the map.
        map.layers.add(new LineLayer(dataSource,
            strokeColor("blue"),
            strokeWidth(5f)));
    });

    ```
    
    Yukarıdaki kod parçacığında, ilk Azure haritalar eşleme denetimi kullanarak bir örneği alır **onReady()** geri çağırma yöntemi. Ardından bir veri kaynağı nesnesi kullanılarak oluşturur **DataSource** sınıfı ve haritayı ekler. Bir listesini oluşturur ardından **noktası** nesneleri. A **LineString** noktaları listesinden oluşturulur ve veri kaynağına eklendi. A **çizgi katmanı** satır harita üzerinde bir veri kaynağındaki sarmalanmış nesneleri oluşturur. Çizgi katmanı sonra oluşturulur ve veri kaynağı eklendi.

    Yukarıdaki kod parçacığı eklendikten sonra `MainActivity.java` bir aşağıdaki gibi görünmelidir:
    
    ```Java
    package com.example.myapplication;
    import android.app.Activity;
    import android.os.Bundle;
    import com.mapbox.geojson.LineString;
    import com.mapbox.geojson.Point;
    import android.support.v7.app.AppCompatActivity;
    import com.microsoft.azure.maps.mapcontrol.layer.LineLayer;
    import com.microsoft.azure.maps.mapcontrol.options.LineLayerOptions;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import java.util.Arrays;
    import java.util.List;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import static com.microsoft.azure.maps.mapcontrol.options.LineLayerOptions.strokeColor;
    import static com.microsoft.azure.maps.mapcontrol.options.LineLayerOptions.strokeWidth;
    
    
    public class MainActivity extends AppCompatActivity {
    
        static{
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
    
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {

                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a list of points.
                List<Point> points = Arrays.asList(
                    Point.fromLngLat(-73.972340, 40.743270),
                    Point.fromLngLat(-74.004420, 40.756800));
            
                //Create a LineString feature and add it to the data source.
                dataSource.add(LineString.fromLngLats(points));
            
                //Create a line layer and add it to the map.
                map.layers.add(new LineLayer(dataSource,
                    strokeColor("blue"),
                    strokeWidth(5f)));
            });
    
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    
    }
    ```

Uygulamayı şimdi çalıştırırsanız, aşağıda görüldüğü gibi bir satır haritada görmelisiniz:

<center>

![Android eşleştirme satırı](./media/how-to-add-shapes-to-android-map/android-map-line.png)</center>


## <a name="add-a-polygon-to-the-map"></a>Bir çokgenin haritaya eklemek

**Çokgen katmanı** çokgene harita alanının işlemek sağlar. Bir çokgenin haritaya eklemek için aşağıdaki adımları izleyin.

1. Düzen **res > Düzen > activity_main.xml** bir aşağıdaki gibi görünür:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >
    
        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_centerLat="40.78"
            app:mapcontrol_centerLng="-73.97"
            app:mapcontrol_zoom="12"
            />
    
    </FrameLayout>
    ```

2. Aşağıdaki kod parçacığını kopyalayıp kopyalama **onCreate()** yöntemi, `MainActivity.java` sınıfı.

    ```Java
    mapControl.onReady(map -> {
        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a list of point arrays to create polygon rings.
        List<List<Point>> rings = Arrays.asList(Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.7968),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)));
    
        //Create a Polygon feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Polygon.fromLngLats(rings)));
    
        //Add a polygon layer for rendering the polygon area.
        map.layers.add(new PolygonLayer(dataSource,
            fillColor("red")));
    
        //Add a line layer for rendering the polygon outline.
        map.layers.add(new LineLayer(dataSource,
            strokeColor("blue"),
            strokeWidth(2f)));
    });
    ```

    Yukarıdaki kod parçacığında, ilk Azure haritalar eşleme denetimi kullanarak bir örneği alır **onReady()** geri çağırma yöntemi. Ardından bir veri kaynağı nesnesi kullanılarak oluşturur **DataSource** sınıfı ve haritayı ekler. A **Çokgen** nesne listesinden sonra oluşturulan **noktası** nesneleri ve veri kaynağına eklenir. A **Çokgen katmanı** harita üzerinde veri kaynağı içinde sarmalanmış verileri işler. Çokgen alan işlemek için bir Çokgen katmanı oluşturur ve veri kaynağını ekler. A **çizgi katmanı** satır sarmalanmış bir veri kaynağı nesneleri oluşturur. Kod parçacığı son kısmını çokgenin anahat işlemek için bir çizgi katmanı oluşturur ve veri kaynağını ekler.

    Yukarıdaki kod parçacığı eklendikten sonra `MainActivity.java` bir aşağıdaki gibi görünmelidir:

    ```Java
    package com.example.myapplication;
    
    import android.app.Activity;
    import android.os.Bundle;
    import java.util.Arrays;
    import android.util.Log;
    import java.util.Collections;
    import android.support.v7.app.AppCompatActivity;
    import com.mapbox.geojson.Point;
    import com.mapbox.geojson.Polygon;
    import com.mapbox.geojson.Feature;
    import com.microsoft.azure.maps.mapcontrol.layer.LineLayer;
    import com.microsoft.azure.maps.mapcontrol.options.LineLayerOptions;
    import com.microsoft.azure.maps.mapcontrol.layer.PolygonLayer;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import static com.microsoft.azure.maps.mapcontrol.options.LineLayerOptions.strokeColor;
    import static com.microsoft.azure.maps.mapcontrol.options.LineLayerOptions.strokeWidth;
    import static com.microsoft.azure.maps.mapcontrol.options.PolygonLayerOptions.fillColor;
    
    public class MainActivity extends AppCompatActivity {
    
        static{
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
    
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {
                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a list of point arrays to create polygon rings.
                List<List<Point>> rings = Arrays.asList(Arrays.asList(
                    Point.fromLngLat(-73.98235, 40.76799),
                    Point.fromLngLat(-73.95785, 40.80044),
                    Point.fromLngLat(-73.94928, 40.7968),
                    Point.fromLngLat(-73.97317, 40.76437),
                    Point.fromLngLat(-73.98235, 40.76799)));
            
                //Create a Polygon feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Polygon.fromLngLats(rings)));
            
                //Add a polygon layer for rendering the polygon area.
                map.layers.add(new PolygonLayer(dataSource,
                    fillColor("red")));
            
                //Add a line layer for rendering the polygon outline.
                map.layers.add(new LineLayer(dataSource,
                    strokeColor("blue"),
                    strokeWidth(2f)));
            });
    
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    
    }
    ```

Uygulamayı şimdi çalıştırırsanız, aşağıda görüldüğü gibi bir Çokgen harita üzerinde göreceksiniz:

<center>

![Android harita Çokgen](./media/how-to-add-shapes-to-android-map/android-map-polygon.png)</center>


## <a name="next-steps"></a>Sonraki adımlar

Eşleme stilleri ayarlama yolları hakkında daha fazla bilgi edinmek için şu makaleye bakın

> [!div class="nextstepaction"]
> [Android Maps değişiklik eşleme stilleri](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)