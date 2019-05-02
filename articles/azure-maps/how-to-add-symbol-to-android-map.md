---
title: Android sembol katmana eşler Azure haritalar içinde ekleyin | Microsoft Docs
description: Azure haritalar Android SDK'sını kullanarak bir harita için simgeler ekleme
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: add6e23d023753e217c102dc946837a71a64c781
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64871085"
---
# <a name="add-a-symbol-layer-to-a-map-using-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını kullanarak bir harita için bir simge katmanı Ekle

Bu makalede Azure haritalar Android SDK'sını kullanarak bir harita üzerinde bir sembol katmanı olarak bir veri kaynağından veri noktası nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamen uygulamaya yüklemeniz gerekir [Azure haritalar Android SDK'sı](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) bir haritayı yükleyin.

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

Bir işaretçi simge katmanını kullanarak haritaya eklemek için aşağıdaki adımları izleyin:

1. Düzen **res** > **Düzen** > **activity_main.xml** aşağıdaki XML gibi görünür:
    
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
            app:mapcontrol_centerLat="47.64"
            app:mapcontrol_centerLng="-122.33"
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
    
        //Create a point feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
    
        //Add a custom image icon to the map resources.
        map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
    
        //Create a symbol layer and add it to the map.
        map.layers.add(new SymbolLayer(dataSource,
            iconImage("my-icon")));
        });
    
    ```
    
    Yukarıdaki kod parçacığında, ilk Azure haritalar eşleme denetimi kullanarak bir örneği alır **onReady()** geri çağırma yöntemi. Ardından bir veri kaynağı nesnesi kullanılarak oluşturur **DataSource** sınıfı ve haritayı ekler. Ardından ekler bir **özellik** bir noktası geometriye içeren. Kırmızı işaret resmi ardından simge için simge olarak ayarlanır. A **sembol katman** metin veya simge harita üzerinde bir sembol olarak veri kaynağında sarmalanmış noktası tabanlı veri işleme için kullanır. Sembol katman sonra oluşturulur ve veri kaynağını oluşturmak için geçirilen ve sonra haritanın katmanlarını eklenir.
    
    Yukarıdaki kod parçacığı eklendikten sonra `MainActivity.java` bir aşağıdaki gibi görünmelidir:
    
    ```Java
    package com.example.myapplication;
    
    import android.app.Activity;
    import android.os.Bundle;
    import com.mapbox.geojson.Feature;
    import com.mapbox.geojson.Point;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
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
            
                //Create a point feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
            
                //Add a custom image icon to the map resources.
                map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            
                //Create a symbol layer and add it to the map.
                map.layers.add(new SymbolLayer(dataSource,
                    iconImage("my-icon")));
            });
        }
    
        @Override
        public void onStart() {
            super.onStart();
            mapControl.onStart();
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
    
Uygulamanızı çalıştırırsanız bu noktada, bir işaretçi harita üzerinde burada gösterildiği gibi görmeniz gerekir:

<center>

![Android harita raptiyesini](./media/how-to-add-symbol-to-android-map/android-map-pin.png)</center>


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla şey haritanıza eklemek için bkz:

> [!div class="nextstepaction"]
> [Bir Android eşlemesine şekiller ekleme](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)