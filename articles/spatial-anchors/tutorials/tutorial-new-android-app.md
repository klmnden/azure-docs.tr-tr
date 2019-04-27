---
title: Öğretici - Azure uzamsal bağlayıcılarını kullanarak yeni bir Android uygulaması oluşturmak için adım adım yönergeler | Microsoft Docs
description: Bu öğreticide, Azure uzamsal bağlayıcılarını kullanarak yeni bir Android uygulamasının nasıl oluşturulacağını öğrenin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 04/03/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 9838add4f83434848d61f3ae86db71765efdc59a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60786409"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-android-app-using-azure-spatial-anchors"></a>Öğretici: Azure uzamsal bağlayıcılarını kullanarak yeni bir Android uygulaması oluşturmak için adım adım yönergeler

Bu öğreticide Azure uzamsal Çıpasıyla ARCore işlevini birleştirir yeni bir Android uygulamasının nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

- Bir Windows veya Mac OS x ile makine <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a>.
- A <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">etkin Geliştirici</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore özellikli</a> Android cihaz.

## <a name="getting-started"></a>Başlarken

Android Studio'yu başlatın. İçinde **Android Studio'ya Hoş Geldiniz** penceresinde tıklayın **yeni bir Android Studio projesi Başlat**. Veya zaten açık bir proje varsa, seçin **dosya**->**yeni proje**.

İçinde **yeni proje oluştur** penceresinin altında **telefon ve Tablet** bölümünde, seçin **boş etkinlik**, tıklatıp **sonraki**. Ardından, altında **en düşük API düzeyi**, seçin `API 26: Android 8.0 (Oreo)`olun **dil** ayarlanır `Java`. Proje adı, konumu ve paket adı değiştirmek isteyebilirsiniz. Diğer seçenekleri olduğu gibi bırakın. **Son**'a tıklayın. **Bileşen yükleyici** çalışır. Bunu yaptıktan sonra tıklayın **son**. Bazı işlendikten sonra IDE Android Studio açılır.

## <a name="trying-it-out"></a>Denediğiniz

Yeni uygulama ölçeğinizi test etmek için geliştirici özellikli Cihazınızı geliştirme makinenize bir USB kablosu ile bağlayın. Tıklayın **çalıştırma**->**çalışma 'app'**. İçinde **dağıtım hedefini seçin** penceresinde, Cihazınızı seçin ve tıklayın **Tamam**. Android Studio, bağlı Cihazınızda uygulamayı yükler ve başlar. "Hello World!" şimdi görmeniz gerekir Cihazınızda çalıştırılan uygulamada görüntülenir. Tıklayın **çalıştırma**->**durdurma 'app'**.

## <a name="integrating-arcore"></a>Tümleştirme _ARCore_

<a href="https://developers.google.com/ar/discover/" target="_blank">_ARCore_ </a> konumunu taşır ve gerçek dünyanın kendi anlama derlemeleri izlemek için cihaz etkinleştirme, genişletilmiş gerçeklik deneyimleri oluşturmak için Google'nın platformudur.

Değiştirme `app\manifests\AndroidManifest.xml` kök içine aşağıdaki girdiler içerecek şekilde `<manifest>` düğümü. Bu kod parçacığı birkaç şey yapar:

- Bu, uygulamanızın cihaz kameranıza erişmesine izin verir.
- Ayrıca, uygulamanızı yalnızca Google Play Store içinde ARCore destekleyen cihazlar için görünür durumdadır sağlayacaktır.
- Bu, indirmek ve uygulamanızın yüklü olduğunda, zaten yüklü değilse, ARCore yüklemek için Google Play Store yapılandıracaksınız.

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera.ar" />

<application>
    ...
    <meta-data android:name="com.google.ar.core" android:value="required" />
    ...
</application>
```

Değiştirme `Gradle Scripts\build.gradle (Module: app)` şu girdiyi eklenecek. Bu kod, uygulama hedefleri ARCore sürüm 1.7 sağlayacaktır. Bu değişiklikten sonra bir bildirim Gradle eşitleme isteyen alabilirsiniz: tıklayın **Şimdi Eşitle**.

```
dependencies {
    ...
    implementation 'com.google.ar:core:1.7.0'
    ...
}
```

## <a name="integrating-sceneform"></a>Tümleştirme _Sceneform_

<a href="https://developers.google.com/ar/develop/java/sceneform/" target="_blank">_Sceneform_ </a> OpenGL öğrenmek zorunda kalmadan, genişletilmiş gerçeklik uygulamalarında gerçekçi 3B Sahne işleme sürecini kolaylaştırır.

Değiştirme `Gradle Scripts\build.gradle (Module: app)` aşağıdaki girişler dahil etmek için. Bu kod Java 8, dil yapıları kullanmak için uygulamanızı sağlayacak olan `Sceneform` gerektirir. Kullanarak uygulamanızın hedeflediği sağlayacak `Sceneform` sürüm 1.7, bu yana uygulamanızı kullanarak ARCore sürümüyle eşleşmesi. Bu değişiklikten sonra bir bildirim Gradle eşitleme isteyen alabilirsiniz: tıklayın **Şimdi Eşitle**.

```
android {
    ...

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    ...
    implementation 'com.google.ar.sceneform.ux:sceneform-ux:1.7.0'
    ...
}
```

Açık, `app\res\layout\activity_main.xml`, mevcut Hello Wolrd değiştirin `<TextView>` aşağıdaki ArFragment sahip öğe. Bu kod, kamera hareket ederken, cihaz konumunuzu izlemek ARCore etkinleştirme ekranda görüntülenecek akışını neden olur.

```xml
<fragment android:name="com.google.ar.sceneform.ux.ArFragment"
    android:id="@+id/ux_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

[Yeniden](#trying-it-out) uygulamanıza bir kez daha doğrulamak için cihaz. Bu kez, kamera izinlerini sorulan. Onaylandıktan sonra kameranız işleme ekranınızda akışı görmeniz gerekir.

## <a name="place-an-object-in-the-real-world"></a>Gerçek dünyada bir nesne getirin

Şimdi oluşturma ve uygulamanızı kullanarak bir nesne getirin. İlk olarak, içine aşağıdaki içeri aktarmaları ekleyin, `app\java\<PackageName>\MainActivity`:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=23-33)]

Ardından, içine aşağıdaki üye değişkenlerini ekleyin, `MainActivity` sınıfı:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=52-57)]

Ardından, aşağıdaki kodu ekleyin, `app\java\<PackageName>\MainActivity` `onCreate()` yöntemi. Bu kodu adlı dinleyiciyi kanca `handleTap()`, kullanıcı ekranı Cihazınızda dokunduğunda algılar. Dokunun, bir gerçek dünya yüzeyinde ARCore'nın izleme tarafından tanınan olması durumda, dinleyici çalıştırılır.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=68-74,85&highlight=6-7)]

Son olarak, aşağıdaki ekleyin `handleTap()` yönteminin her şeyi birbirine. Bir küre oluştur ve dokunulan konuma yerleştirin. Küre başlangıçta bu yana siyah olur `this.recommendedSessionProgress` şu anda sıfır olarak ayarlanır. Bu değer daha sonra ayarlanır.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-171,174-182,198-199)]

[Yeniden](#trying-it-out) uygulamanıza bir kez daha doğrulamak için cihaz. Bu kez, ortamınızı algılamayı başlatmak için ARCore almak için Cihazınızı taşıyabilirsiniz. Ardından, ekranın oluşturmak ve tercih ettiğiniz yüzeyi üzerinde siyah, küre yerleştirmek için dokunun.

## <a name="attach-a-local-azure-spatial-anchor"></a>Yerel bir Azure uzamsal bağlantı ekleme

Değiştirme `Gradle Scripts\build.gradle (Module: app)` şu girdiyi eklenecek. Bu kod, uygulama hedefleri Azure uzamsal bağlayıcılarını sürümü 1.0.2 sağlayacaktır. Bu, herhangi bir son sürümü Azure uzamsal çıpalarının başvuran çalışmalıdır belirtti.

```
dependencies {
    ...
    implementation "com.microsoft.azure.spatialanchors:spatialanchors_jni:[1.0.2]"
    implementation "com.microsoft.azure.spatialanchors:spatialanchors_java:[1.0.2]"
    ...
}
```

Sağ `app\java\<PackageName>` -> **yeni**->**Java sınıfı**. Ayarlama **adı** için _MyFirstApp_, ve **sınıfın** için _android.app.Application_. Diğer seçenekleri olduğu gibi bırakın. **Tamam** düğmesine tıklayın. Dosya adında `MyFirstApp.java` oluşturulur. Aşağıdaki içeri aktarma ekleyin:

```java
import com.microsoft.CloudServices;
```

Ardından, yeni içine aşağıdaki kodu ekleyin `MyFirstApp` Azure uzamsal bağlantıları sağlar sınıfını, uygulamanızın bağlamı ile başlatılır.

```java
    @Override
    public void onCreate() {
        super.onCreate();
        CloudServices.initialize(this);
    }
```

Şimdi değiştirmek `app\manifests\AndroidManifest.xml` kök içinde aşağıdaki girdisi eklemek için `<application>` düğümü. Bu kodu uygulamanıza oluşturduğunuz uygulama sınıfı denetime.

```xml
    <application
        android:name=".MyFirstApp"
        ...
    </application>
```

Geri `app\java\<PackageName>\MainActivity`, içine aşağıdaki içeri aktarmaları ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=33-40&highlight=2-8)]

Ardından, içine aşağıdaki üye değişkenlerini ekleyin, `MainActivity` sınıfı:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=57-60&highlight=3-4)]

Ardından, aşağıdaki ekleyelim `initializeSession()` yöntemi içinde `mainActivity` sınıfı. Çağrıldığında, bir Azure uzamsal bağlayıcılarını oturumu oluşturulur ve uygulama başlatma sırasında düzgün başlatılmadı sağlayacaktır.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=89-97,146)]

Şimdi, şimdi kanca, `initializeSession()` yönteme, `onCreate()` yöntemi. Ayrıca, biz çerçeveler kameranızdan akış işleme için Azure uzamsal bağlayıcılarını SDK gönderildiğinden emin olmak.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=68-85&highlight=9-17)]

Son olarak, aşağıdaki kodu ekleyin, `handleTap()` yöntemi. Gerçek dünyada biz yerleştirme siyah Küre, yerel bir Azure uzamsal bağlantı bağlanacağı.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-182,198-199&highlight=12-13)]

[Yeniden](#trying-it-out) uygulamanızı bir kez daha. Cihazınızı taşımak, ekrana dokunmanız ve siyah bir küre yerleştirin. Bu kez, yine de kodunuzu oluşturacak ve yerel bir Azure uzamsal bağlantı, küreyle ekleniyor.

Başka bir işlem yapmadan önce bir Azure uzamsal yer işaretleri oluşturmanız gerekecektir bunları zaten yoksa, hesap tanımlayıcı ve anahtar. Bunları almak için aşağıdaki bölümü izleyin.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Yerel, yer işareti buluta yükleyin

Uzamsal bağlayıcılarını Azure hesabınızı tanımlayıcı ve anahtar oluşturduktan sonra size geri gidip gidemeyeceğini `app\java\<PackageName>\MainActivity`, içine aşağıdaki içeri aktarmaları ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=40-45&highlight=3-6)]

Ardından, içine aşağıdaki üye değişkenlerini ekleyin, `MainActivity` sınıfı:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=60-65&highlight=3-6)]

Şimdi aşağıdaki kodu ekleyin, `initializeSession()` yöntemi. Bu kod uygulamanızın ilerleme durumunu izlemek ilk olarak tanıyacak akışı makinenizden çerçeveler toplayan Azure uzamsal bağlayıcılarını SDK sağlar. Çalıştığı gibi küre rengini, özgün siyahtan gri değiştirerek başlayın. Ardından, yer işareti buluta göndermek için yeterli çerçeveler toplandığında beyaz kapatır. İkinci olarak, bu kod bulut arka ucu ile iletişim kurmak için gerekli kimlik bilgilerini sağlar. İşte burada hesap tanımlayıcı ve anahtar kullanacak şekilde yapılandıracaksınız. Bir metin düzenleyiciye kopyaladığınız zaman [uzamsal bağlayıcılarını kaynağı ayarı](#create-a-spatial-anchors-resource).

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=89-120,142-146&highlight=11-36)]

Ardından, aşağıdaki ekleyin `uploadCloudAnchorAsync()` yöntemi içinde `mainActivity` sınıfı. Cihazınızın toplanan yeterli çerçeveler kadar bu yöntem çağrıldığında, zaman uyumsuz olarak bekler. Bu durumda hemen sonra küre rengi sarı olarak geçer ve ardından bunu yerel Azure uzamsal bağlantı buluta karşıya yükleme başlar. Karşıya yükleme işlemi tamamlandıktan sonra kodu bir yer işareti tanımlayıcı döndürür.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=uploadCloudAnchorAsync)]

Son olarak, şimdi her şeyi birbirine bağlayın. İçinde `handleTap()` yöntemine aşağıdaki kodu ekleyin. Bunu çağırır, `uploadCloudAnchorAsync()` , küre oluşturulduktan hemen sonra yöntemi. Bir kez yöntemi döndüğünde, aşağıdaki kod, küre rengini Mavi olarak değiştirilmesi, son bir güncelleştirme gerçekleştirir.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-199&highlight=24-37)]

[Yeniden](#trying-it-out) uygulamanızı bir kez daha. Cihazınızı geçici olarak taşımak, ekrana dokunmanız ve, küre yerleştirin. Kamera çerçeveler toplanan şekilde bu kez, yine de, küre rengini beyaz doğru siyahtan değiştirir. Biz yeterli çerçeveler oluşturduktan sonra kürenin sarı kapatır ve bulut karşıya yükleme başlar. Karşıya yükleme tamamlandığında, küre mavi kapatır. İsteğe bağlı olarak da kullanabilirsiniz `Logcat` günlük iletilerini izlemek için Android Studio penceresine uygulamanızı gönderiyor. Örneğin, çerçeve sırasında oturum durumu yakalar ve bulut karşıya yüklemeyi bir kez döndürür bağlantı tanımlayıcı tamamlandı.

## <a name="locate-your-cloud-spatial-anchor"></a>Bulut uzamsal yer işareti bulun

Buluta yüklenen bir, bağlantı, tekrar bulma girişiminde hazırız. İlk olarak, aşağıdaki içeri aktarmaları kodunuzla ekleyelim.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=45-48&highlight=3-4)]

Ardından, aşağıdaki kodu ekleyelim, `handleTap()` yöntemi. Bu kod size:

- Varolan müşterilerimizin mavi küre ekranından kaldırın.
- Müşterilerimizin Azure uzamsal bağlayıcılarını oturumu yeniden başlatın. Bu eylem, bulunacak yapacağız bağlantı yerine yerel yer işaretini oluşturduk buluttan geldiğini sağlayacaktır.
- Buluta dosyamızı bağlantı için bir sorgu yürütün.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=handleTap&highlight=10-19)]

Şimdi, şimdi biz sorgulama için bağlantı bulunduğunda, çağrılacak ayarlarız. İçinde `initializeSession()` yöntemine aşağıdaki kodu ekleyin. Bu kod parçacığı oluşturma ve bulut uzamsal bağlantı bulunduktan sonra yeşil bir küre yerleştirin. Tüm senaryo bir kez daha yineleyin için onu da ekranı yeniden dokunarak etkinleştirir: başka bir yerel bağlantı oluşturmak, karşıya yükleyin ve tekrar bulun.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=initializeSession&highlight=34-53)]

İşte bu kadar! [Yeniden](#trying-it-out) uygulamanızı bir tam senaryoyu uçtan uca deneyin için en son zaman. Cihazınızı taşıyın ve siyah, küre yerleştirin. Ardından, kamera çerçeveleri küre sarıya kadar yakalamak için Cihazınızı taşıma tutun. Yerel, yer işareti yüklenevek ve, küre mavi kapatır. Son olarak, ekranınıza bir kez daha, yerel bağlantı kaldırılır ve ardından şu bulut çözümlemesiyle sorgulayacaksınız dokunun. Bulut uzamsal yer işareti bulunana kadar Cihazınızı taşınması devam edin. Yeşil bir küre doğru konumda görüntülenmelidir ve, parlatıcı & tam senaryoyu yeniden tekrarlayın.

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-new-android-app-finished.md)]
