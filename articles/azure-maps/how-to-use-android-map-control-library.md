---
title: Azure haritalar Android harita denetimi ile çalışmaya başlama | Microsoft Docs
description: Azure haritalar Android harita denetimi.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9df5eb9fa4493f82c6efd4a8e30eee324e4eac2a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273837"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sı ile çalışmaya başlama

Azure haritalar Android SDK'sı, Android için vektör harita kitaplığıdır. Bu makalede Azure haritalar Android SDK'sını yükleme ve bir harita yükleme işlemleri size yol gösterir.

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

1. En üst düzey açın **build.gradle** dosyasını açıp aşağıdaki kodu ekleyin **tüm projeleri**, **depoları** bölüm engelle:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Güncelleştirme, **app/build.gradle** ve aşağıdaki kodu ekleyin:
    
    1. Emin olun, projenizin **minSdkVersion** API 21 veya üzeri.

    2. Aşağıdaki kod Android bölümüne ekleyin:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Bağımlılıklar bloğuna güncelleştirmek ve en son Azure haritalar Android SDK için yeni bir uygulama bağımlılık satır ekleyin:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

    > [!Note]
    > Azure haritalar Android SDK'sı düzenli olarak yüklenmekte olan gelişmiş ve yükseltildi. Gördüğünüz [Android harita denetimi ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) belgeleri, en son Azure haritalar uygulama sürüm numarasını almak için. Ayrıca, "0 her zaman en son sürüme işaret edecek şekilde +", "0,2-" sürüm numarasını ayarlayabilirsiniz.

3. Düzen **res** > **Düzen** > **activity_main.xml** aşağıdakiyle değiştirin:
    
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
            />
    </FrameLayout>
    ```

4. İçinde **MainActivity.java** dosya gerekir:
    
    * Azure haritalar SDK'sı için içeri aktarmaları ekleyin
    * Azure haritalar kimlik bilgilerinizi ayarlayın
    * Harita denetimi örneği alma **onCreate** yöntemi

    Kimlik bilgilerinizi her görünüm eklemek zorunda kalmamanız için AzureMaps sınıfında genel setSubscriptionKey veya setAadProperties yöntemleri kullanarak kimlik doğrulama bilgilerini ayarlama, kolaylaştırır. Harita denetiminin doğrudan içeren etkinliğinden çağrılmalıdır Android OpenGL ömrü yönetmek için kendi yaşam döngüsü yöntemleri içerir. Uygulamanızın doğru bir şekilde harita denetimin yaşam döngüsü yöntemleri çağırmak sırada içeren harita denetiminin etkinliğinde aşağıdaki yaşam döngüsü yöntemleri geçersiz kılın ve ilgili Harita denetimi yöntemi çağırın. 

    Düzen **MainActivity.java** aşağıdaki gibi:
    
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
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
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

<center>

![Android eşleme](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Haritada yer paylaşımı veri eklemeyi öğrenin:

> [!div class="nextstepaction"]
> [Bir Android eşlemesi için bir simge katmanı Ekle](https://review.docs.microsoft.com/azure/azure-maps/how-to-add-symbol-to-android-map)

> [!div class="nextstepaction"]
> [Bir Android eşlemesine şekiller ekleme](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Android Maps değişiklik eşleme stilleri](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)
