---
title: Konuşma cihaz SDK'sı ile çalışmaya başlama
description: Önkoşullar ve konuşma cihaz SDK'sını kullanmaya başlamak için yönergeler.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 463a015b7c01dafc5b30de56b95fa0510ffb98e4
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42424378"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Konuşma cihaz SDK'sı ile çalışmaya başlama

Bu makalede, geliştirme PC ve konuşma özellikli cihazlar konuşma cihaz SDK'sını kullanarak geliştirme, konuşma cihaz Geliştirme Seti nasıl yapılandırılacağı açıklanır. Ardından derleme ve cihaz için örnek uygulama dağıtırsınız. 

Örnek uygulama için kaynak kodu konuşma cihaz SDK'sı ile eklenir ve ayrıca [github'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Geliştirme konuşma cihaz SDK'sını kullanmaya başlamadan önce ihtiyacınız olan yazılım ve bilgi toplayın.

* Geliştirme setini edinmek [Roobo gelen](http://ddk.roobo.com/). Setleri, doğrusal veya döngüsel mikrofon dizi yapılandırmalarla kullanılabilir; gereksinimleriniz için doğru olanı seçin.

    |Geliştirme Seti yapılandırma|Konuşmacı konumu|
    |-----------------------------|------------|
    |Döngüsel|Herhangi bir yönde bir CİHAZDAN|
    |Doğrusal|Önünde cihaz|

* Bir Android örnek uygulamadan konuşma cihaz SDK'ları da dahil olmak üzere konuşma cihaz SDK'sının en son sürümü edinmek [yükleme sitesine](https://shares.datatransfer.microsoft.com/). Yerel bir klasöre ZIP dosyasını ayıklayın (gibi `C:\SDSDK`).

* Yükleme [Android Studio](https://developer.android.com/studio/) ve [Vysor](http://vysor.io/download/) PC'nizde.

* Konuşma hizmeti elde [abonelik anahtarı](get-started.md). 30 günlük ücretsiz deneme edinebilir veya Azure panonuzdan bir anahtar alın.

* Amaç tanıma konuşma hizmetin kullanmak istiyorsanız, abone [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) ve [bir abonelik anahtarı edinme](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    Gerekebilir [basit LUIS modeline oluşturma](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) veya örneği LUIS modeline `LUIS-example.json`konuşma cihaz SDK'sı erişilebilir [site indirin](https://shares.datatransfer.microsoft.com/). Modelinizin JSON dosyasını karşıya yükleme [LUIS portalı](https://www.luis.ai/home) tıklayarak **alma yeni uygulama** ve JSON dosyasını seçme.

## <a name="set-up-the-development-kit"></a>Geliştirme Seti ' ayarlayın

1. Güç bir mini USB kablosu kullanarak dev Seti açık bir PC veya güç adptor bağlı. Yeşil power göstergesi üst Pano vurgulamasında.

1. Geliştirme Seti, ikinci bir mini USB kablosu kullanarak bir bilgisayara bağlanın.

    ![dev Seti bağlanma](media/speech-devices-sdk/qsg-1.png)

1. Uygulamanızı Geliştirme Seti uygun şekilde yönlendirmek.

    |Geliştirme Seti yapılandırma|Yönlendirme|
    |-----------------------------|------------|
    |Döngüsel|Şekilde, tavan mikrofonlar ile yan yana|
    |Doğrusal|(Aşağıda gösterilen) alt tarafında, mikrofon ile karşılaşmış|

    ![Doğrusal dev Seti yönü](media/speech-devices-sdk/qsg-2.png)

1. Sertifikaları ve Uyandırma sözcüğünü (anahtar) tablo dosyası yükleyin ve ses cihazı izinlerini ayarlayın. Bir komut penceresinde aşağıdaki komutları yazın.

    > [!NOTE]
    > Android hata ayıklama köprüsü bu komutları kullanmak `adb.exe`, Android Studio yüklemesinin bir parçası olduğu. Bu araç bulunabilir `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. Yolunuza çağırmak daha kullanışlı hale getirmek için bu dizine ekleyebilir `adb`. Aksi takdirde, yüklemenize tam yolunu belirtmeniz gerekir `adb.exe` çağıran her komut `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > Bilgisayarınızın mikrofon ve konuşmacının sesini kapatın. Bu şekilde Geliştirme Seti'nın mikrofonlar ile çalışıyorsanız ve ses bilgisayardan cihazla yanlışlıkla tetiklemez emin olabilirsiniz.
    
1.  Bilgisayarınızda Vysor başlatın.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Cihazınızı altında "bir cihaz seçin" listelenmelidir. Tıklayın **görünümü** yanında düğmesi. 
 
1.  Ardından klasör simgesine tıklayarak, kablosuz ağa bağlamak **ayarları**, ardından **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
 > [!NOTE]
 > Şirketinizin ilkeleri varsa bağlanan cihazların ilgili wifi sisteme bağlanmak için BT Departmanınızla iletişime geçin ve Mac adresini almak için ihtiyacınız wifi sisteme. Dev Seti Mac adresini bulmak için dosya klasörü simgesine masaüstünde dev Seti, ardından **ayarları**, arama "İçin Mac adresi" a tıklayın **Mac adresi** almak istediğiniz **Gelişmiş WLAN** , doğru alt bulunan Mac adresi yazın. Ayrıca, bazı şirketler, bir cihazın ne kadar süre sınırı wifi sistemlerine bağlı olarak bir zaman olabilir. Belirli sayıda gün sonra dev Seti'nın kayıt wifi sisteminizle genişletmek gerekebilir.  
 
 
   ![Vysor dosya klasörü](media/speech-devices-sdk/qsg-10.png)
   
   ![Vysor Mac adresi](media/speech-devices-sdk/qsg-11.png)
   
   
 > Konuşmacı geliştirme setine eklemek istiyorsanız, ses satırına bağlanabilirsiniz. Ayrıca, kaliteli 3,5 mm Konuşmacı seçmeniz gerekir.
 
   ![Vysor ses](media/speech-devices-sdk/qsg-14.png)
 
## <a name="run-a-sample-application"></a>Örnek uygulamayı çalıştırma

Roobo testleri çalıştırmak ve Geliştirme Seti kurulumunuzu doğrulamak için derleme ve örnek uygulamayı yükleyin.

1.  Android Studio'yu başlatın.

1.  Var olan bir Android Studio projesini açmak seçin.

    ![Android studio - var olan proje Aç](media/speech-devices-sdk/qsg-5.png)
 
1.  Gözat `C:\SDSDK\Android-Sample-Release\example`, ardından **Tamam** örnek projesini açın.
 
1.  Konuşma abonelik anahtarınız için kaynak kodu ekleyin. Amaç tanıma denemek istiyorsanız, ayrıca ekleyin, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarı ve uygulama kimliği 

    Anahtarlarınızı ve uygulama bilgilerini gider kaynak dosyası aşağıdaki satırları `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. Varsayılan Uyandırma sözcüğünü (anahtar) "Bilgisayar" olan  İsterseniz, diğer sağlanan birini deneyebilirsiniz sözcükler, "Machine" ve "Yardımcısı" uyandır Bu alternatif sözcükler için kaynak dosyaları konuşma cihaz SDK'sı "anahtar sözcüğü" klasöründe bulunabilir. Örneğin, `C:\SDSDK\Android-Sample-Release\keyword\Computer` "Bilgisayar" için kullanılan dosyalar içerir

    Ayrıca [özel Uyandırma word oluşturmak](speech-devices-sdk-create-kws.md).

    İstenen Uyandırma word yüklemek için:
 
    * Oluşturma bir `keyword` klasöründe \data\ komut penceresinde aşağıdaki komutları çalıştırarak cihazda.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Dosyaları kopyalama `kws.table`, `kws_g.fst`, `kws_k.fst`, ve `words_kw.txt`) cihazın \data\keyword\ klasörüne. O komut penceresinde aşağıdaki komutları çalıştırın. Oluşturduysanız bir [özel Uyandırma word](speech-devices-sdk-create-kws.md), Web'den oluşturulan kws.table dosyası aynı dizinde olacağı `kws.table`, `kws_g.fst`, `kws_k.fst`, ve `words_kw.txt` dosyalarıdır. Lütfen adb anında iletme C:\SDSDK\Android-Sample-Release\keyword kullanın\[wake_word_name]\kws.table/data/anahtar sözcüğü komutu yerine geliştirme setine kws.table dosya göndermek.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Bu örnek uygulama dosyalarında başvuru. Aşağıdaki satırları bulun `MainActivity.java`. Belirtilen anahtar sözcüğü bir kullanmakta olduğunu ve yolunu işaret ettiğinden emin `kws.table` cihaza gönderilen dosya.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > Kendi kodunuzda kullanabileceğiniz `kws.table` dosya bir anahtar sözcüğü model örneği oluşturup tanıma şu şekilde başlatın.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Mikrofon dizi geometri ayarları içeren aşağıdaki satırları güncelleştirin.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Değişken|Anlamı|Kullanılabilir değerler|
    |--------|-------|----------------|
    |`DeviceGeometry`|Fiziksel MIC yapılandırma|`Circular6+1` Döngüsel Geliştirme Seti|
    ||| `Linear4` Doğrusal Geliştirme Seti|
    |`SelectedGeometry`|Yazılım MIC yapılandırması|`Circular6+1` Tüm mikrofonlardan kullanarak döngüsel Geliştirme Seti|
    |||`Circular3+1` dört mikrofonlardan kullanarak döngüsel Geliştirme Seti|
    |||`Linear4` Tüm mikrofonlardan kullanarak doğrusal Geliştirme Seti|
    |||`Linear2` iki mikrofonlardan kullanarak doğrusal Geliştirme Seti|


1.  Seçerek uygulama derleme **'uygulamayı' Çalıştır** menüsünden çalıştırın. Dağıtım hedefini seçin iletişim kutusu görüntülenir. Cihazınızı seçin ve tıklayın **Tamam** cihaza uygulamayı dağıtmak için.

    ![Dağıtım hedefi seçin](media/speech-devices-sdk/qsg-7.png)
 
1.  Burada gösterilen seçenekler görüntüleme konuşma cihaz SDK'sı örnek uygulamayı başlatır.

    ![Örnek konuşma cihaz uygulaması](media/speech-devices-sdk/qsg-8.png)

1. Atabilirsiniz!

## <a name="troubleshooting"></a>Sorun giderme

Konuşma hizmeti kullanılırken sertifika hataları alırsanız, doğru tarih ve saat cihaz sahip olduğundan emin olun. Git **ayarları**, tıklayın **tarih ve saat** sistemi altında ve **saat dilimi seçin** geçerli saat diliminizde olacak. Tutun **otomatik tarih ve saat** açık. Bilgisayarınızın saat dev Seti'nın kez eşleşir ve ardından dev Seti öğrenmiş olacaksınız gördüğünüzde, internet'e bağlı. 

 ![Vysor dosya klasörü](media/speech-devices-sdk/qsg-12.png)
 
 ![Vysor dosya klasörü](media/speech-devices-sdk/qsg-13.png)

Daha fazla geliştirme için Roobo'nın bilgi [geliştirme Kılavuzu](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo ses sorunları gidermeye yardımcı olabilecek bellek, Flash tüm ses yakalayan bir araç sağlar. Aracı sürümü her Geliştirme Seti yapılandırması için sağlanır. Cihazınıza seçin [Roobo site](http://ddk.roobo.com/), ardından **ROOBO Araçları** sayfanın alt kısmındaki bağlantı.
