---
title: Android harita denetiminin içinde Azure haritalar kullanma | Microsoft Docs
description: Azure haritalar Android harita denetimi.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 15706addbe6b7f6310223978130158c792a47c89
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770388"
---
# <a name="how-to-use-the-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını kullanma

Azure haritalar Android SDK'sı, Android için vektör harita kitaplığıdır. Bu makalede Azure haritalar Android SDK'sını yükleme, bir harita yükleme ve harita üzerinde bir PIN yerleştirme işlemleri size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Bu makalede yer alan yordamları tamamlamak için önce yapmanız [bir Azure haritalar hesabı oluştur](how-to-manage-account-keys.md) S1 fiyatlandırma katmanını içinde.

### <a name="download-android-studio"></a>Android Studio’yu indirin

Android Studio'yu indirin ve Azure haritalar Android SDK'yı yüklemeden önce boş bir etkinliği ile bir proje oluşturmak için ihtiyacınız. Yapabilecekleriniz [Android Studio'yu indir](https://developer.android.com/studio/) Google ücretsiz olarak edinebilirsiniz. 

## <a name="create-a-project-in-android-studio"></a>Android Studio'da bir proje oluşturma

İlk olarak boş bir etkinlik yeni bir proje oluşturmak gerekir. Bir Android Studio projesi oluşturmak için aşağıdaki adımları tamamlayın:

1. Altında **projenizi seçin**seçin **telefon ve Tablet**. Uygulama bu form faktörüne üzerinde çalışır.
2. Üzerinde **telefon ve Tablet** sekmesinde **boş etkinlik**ve ardından **sonraki**.
3. Altında **Nakonfigurovat projekt**seçin `API 21: Android 5.0.0 (Lollipop)` en düşük SDK'sı olarak. Azure haritalar Android SDK tarafından desteklenen en erken sürüm budur.
4. Varsayılan değerleri kabul `Activity Name` ve `Layout Name` seçip **son**.

Bkz: [Android Studio belgeleri](https://developer.android.com/studio/intro/) yeni proje oluşturma ve yükleme Android Studio ile daha fazla bilgi için Yardım.

![Proje oluşturma](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Sanal bir cihaz ayarlama

Android Studio bilgisayarınızı sanal bir Android cihazında ayarlamanıza olanak tanır. Bunun yapılması, uygulama geliştirme sırasında test etmenize yardımcı olabilir. Bir sanal cihazınızın kurulumunun yapılabilmesi için proje ekranınızın sağ üst köşesinde bulunan Android Virtual Device (AVD) Manager simgesini seçin ve ardından **sanal cihaz oluşturma**. İçin AVD Yöneticisi'ni seçerek de sahip olabilirsiniz **Araçları** > **Android** > **AVD Manager** araç çubuğundan. İçinde **telefonlar** kategorisi, select **Nexus 5 X**ve ardından **sonraki**.

İçinde bir AVD ayarlama hakkında daha fazla bilgi [Android Studio belgeleri](https://developer.android.com/studio/run/managing-avds).

![Android öykünücüsü](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını yükleyin

Azure haritalar Android SDK'sı uygulamanızı oluşturduktan sonraki adım yüklemektir. SDK yüklemek için aşağıdaki adımları tamamlayın:

1. Aşağıdaki kodu ekleyin **tüm projeleri**, **depoları** engellenmesi, **build.gradle** dosya.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Güncelleştirme, **app/build.gradle** ve aşağıdaki kodu ekleyin:

    1. Android bloğuna aşağıdaki kodu ekleyin:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Bağımlılıkları engellemeniz güncelleştirin ve aşağıdaki kodu ekleyin:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Aşağıdaki XML ekleyerek izinleri ayarlama, **AndroidManifest.xml** dosyası:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Düzen **res** > **Düzen** > **activity_main.xml** bu XML gibi görünür:
    
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

5. Düzen **MainActivity.java** harita görünümü etkinlik sınıfı oluşturmak için. Dosyayı düzenledikten sonra bu sınıf gibi görünmesi gerekir:

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
        
        static {
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

Yukarıdaki adımları tamamladıktan sonra büyük olasılıkla uyarıları Android Studio bazı kodları hakkında elde edersiniz. Bu uyarıları gidermek için başvurulan sınıflarını içeri aktarmak `MainActivity.java`.

Otomatik olarak Alt + Enter (seçeneği + Return Mac'te) seçerek bu sınıflarını içeri aktarabilirsiniz.

Aşağıdaki grafik (veya bir Mac üzerinde denetim + R tuşuna basın), uygulamanızı oluşturmak için gösterildiği gibi Çalıştır düğmesi seçin.

![Çalıştır'a tıklayın](./media/how-to-use-android-map-control-library/run-app.png)

Android Studio, uygulamayı oluşturmak için birkaç saniye sürer. Derleme tamamlandıktan sonra benzetilmiş Android cihazda uygulamanızı test edebilirsiniz. Bunun gibi bir eşleme görmeniz gerekir:

![Android eşleme](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Haritayı bir işaretleyici Ekle

Haritanıza bir işaret eklemek için Ekle `mapView.getMapAsync()` işlevi `MainActivity.java`. En son `MainActivity.java` kod şu şekilde görünmelidir:

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

Uygulamanızı yeniden çalıştırın. Burada gösterildiği gibi harita üzerinde işaretçiyi görmeniz gerekir:

![Android harita raptiyesini](./media/how-to-use-android-map-control-library/android-map-pin.png)