---
title: Azure haritalar içinde Android harita denetimini kullanma | Microsoft Docs
description: Azure haritalar içinde Android harita denetimi kullanın.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e3f7579324e1218cc2e2c3594889db776da6e529
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56143859"
---
# <a name="how-to-use-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını kullanma

Azure haritalar Android SDK'sı, Android için vektör haritalar kitaplığıdır. Bu makalede, Azure haritalar Android SDK'sını yükleme, bir harita yükleniyor ve üzerinde bir PIN yerleştirme işleminde size yol gösterir.

## <a name="prerequisites-to-get-started"></a>Başlamak için Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

Bu kılavuzdaki adımları takip etmek için ilk görmek ihtiyacınız [hesapları ve anahtarları yönetme](how-to-manage-account-keys.md) oluşturup fiyatlandırma katmanı, hesap aboneliğiniz S1 ile yönetebilirsiniz.

### <a name="download-android-studio"></a>Android Studio'yu indirin

İndirebileceğiniz [Android Studio](https://developer.android.com/studio/) Google ücretsiz olarak edinebilirsiniz. Azure haritalar Android SDK'sı yükleyebilmek için önce Android Studio'yu indirin ve boş bir etkinliği ile bir proje oluşturmak gerekir.

## <a name="create-a-project-in-android-studio"></a>Android Studio'da bir proje oluşturma

Boş bir etkinlik yeni bir proje oluşturmanız gerekir. Yeni bir Android Studio projesi oluşturmak için aşağıdaki adımları izleyin:

1. Altında *projenizi seçin*, uygulamanızın üzerinde çalışacağı form faktörü olarak "Telefon ve Tablet" denetleyin.
2. Tıklayın *boş etkinlik* tıklayın ve form faktörü altında **sonraki**.
3. Altında *Nakonfigurovat projekt*seçin `API 21: Android 5.0.0 (Lollipop)` en düşük SDK'sı olarak. Azure haritalar Android SDK tarafından desteklenen en düşük sürüm budur.
4. Varsayılan değerleri kabul `Activity Name` ve `Layout Name` tıklatıp **son**

Bkz: [Android Studio belgeleri](https://developer.android.com/studio/intro/) Android Studio yükleme ve yeni proje oluşturma daha fazla bilgi için Yardım.

![Yeni bir proje oluşturun](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Sanal bir cihaz ayarlama

Android Studio bilgisayarınızı sanal bir Android cihazında ayarlamanıza olanak tanır. Hangi geliştirirken, uygulamanızı test etmek için yardımcı olabilir. Sanal cihaz üst Android Virtual Device (AVD) Manager simgeye kurulum için proje ekranınızın sağ. Ardından **sanal cihaz oluşturma** düğmesi. Araçlar aracılığıyla yöneticisine alabilirsiniz > Android > AVD Yöneticisi'nde araç. Gelen **telefonlar** kategorisi, select **Nexus 5 X** tıklatıp **sonraki**.

İçinde bir AVD ayarlama hakkında daha fazla bilgi [Android Studio belgeleri](https://developer.android.com/studio/run/managing-avds).

![Android öykünücüsü](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını yükleyin

Uygulamanızı oluşturmaya yönelik ileri taşımadan önce Azure haritalar Android SDK'sını yüklemek için aşağıdaki adımları izleyin. 

1. Ekleyin **allprojects**, depoları engellenmesi, **build.gradle** dosya.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Güncelleştirme, **app/build.gradle** ve aşağıdakileri ekleyin:

    1. Aşağıdaki Android bloğuna ekleyin:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Bağımlılıkları engellemeniz güncelleştirin ve aşağıdakileri ekleyin:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. 'AndroidManifest.xml' ekleyerek izinleri ayarlama

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Düzen **res > Düzen > activity_main.xml**, onu görünmesi aşağıdaki XML ister:
    
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
            app:mapcontrol_cameraTargetLat="47.64"
            app:mapcontrol_cameraTargetLng="-122.33"
            app:mapcontrol_cameraZoom="12"
            />

    </FrameLayout>
    ```

5. Düzen **MainActivity.java** harita görünümü etkinlik sınıfı oluşturmak için. Düzenledikten sonra sınıfına aşağıdaki gibi görünmelidir:

    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static{
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);

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

## <a name="import-classes"></a>Sınıfları içeri aktarma

Yukarıdaki adımları tamamladıktan sonra büyük olasılıkla uyarıları Android Studio bazı kod metni alırsınız. Bu, başvurulan sınıfların bazıları almak için sahip olacak işlemek için `MainActivity.java`.

Bu sınıfların tuşlarına basarak otomatik olarak içeri aktarabilirsiniz `Alt` + `Enter`(`Option` + `Return` Mac üzerinde). 

Tıklayın **Çalıştır 'App'** düğme (veya `Control` + `R` Mac'te) uygulamanızı oluşturmak için.

![Çalıştır'a tıklayın](./media/how-to-use-android-map-control-library/run-app.png)

Uygulamayı oluşturmak için android studio birkaç saniye sürer. Son Derleme tamamlandıktan sonra benzetilmiş Android cihazda uygulamanızı test edebilirsiniz. Aşağıdaki gibi bir harita görürsünüz.

![Android eşleme](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Haritayı bir işaretleyici Ekle

Bir işaretçi haritanızı açın eklemek için Ekle `mapView.getMapAsync()` işlevi `MainActivity.java`. En son `MainActivity.java` aşağıdaki gibi görünmelidir:

```java
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
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

    MapControl mapControl;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mapControl = findViewById(R.id.mapcontrol);

        mapControl.onCreate(savedInstanceState);

        mapControl.getMapAsync(map -> {
            DataSource dataSource = new DataSource();
            dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

            SymbolLayer symbolLayer = new SymbolLayer(dataSource);
            symbolLayer.setOptions(iconImage("my-icon"));

            map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            map.sources.add(dataSource);
            map.layers.add(symbolLayer);
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

Uygulamanızı yeniden çalıştırın ve aşağıdaki gibi harita üzerinde işaretçiyi görmeniz gerekir.

![Android harita raptiyesini](./media/how-to-use-android-map-control-library/android-map-pin.png)