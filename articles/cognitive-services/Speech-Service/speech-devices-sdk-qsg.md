---
title: Konuşma aygıtları SDK'sı ile çalışmaya başlama | Microsoft Docs
description: Önkoşulları ve konuşma aygıtları SDK'sı ile çalışmaya başlama yönergeleri.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 6cc5ff1c532d67c48beac1a2a10d034f5d9d7501
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355654"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Konuşma aygıtları SDK'sı ile çalışmaya başlama

Bu makalede, geliştirme PC ve konuşma özellikli cihazlar konuşma aygıtları SDK'sını kullanarak geliştirmek için konuşma cihaz Geliştirme Seti nasıl yapılandırılacağı açıklanmaktadır. Ardından yapı ve cihaz için örnek bir uygulama dağıtabilirsiniz. 

Örnek uygulama için kaynak kodu konuşma aygıtları SDK'da bulunan ve aynı zamanda [github'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Geliştirme konuşma aygıtları SDK'sını kullanmaya başlamadan önce yazılım, gerekir ve bilgi toplayın.

* Geliştirme Seti elde [Roobo gelen](http://ddk.roobo.com/). Setleri doğrusal veya döngüsel mikrofon dizi yapılandırmalarla kullanılabilir; gereksinimleriniz için doğru olanı seçin.

    |Geliştirme Seti yapılandırma|Konuşmacı konumu|
    |-----------------------------|------------|
    |Döngüsel|Herhangi bir yönde aygıttan|
    |Doğrusal|Önündeki cihaz|

* Bir Android örnek uygulamadan konuşma aygıtları SDK'ın dahil olmak üzere konuşma aygıtları SDK'ın en son sürümü edinmek [yükleme sitesine](https://shares.datatransfer.microsoft.com/). Bir yerel klasör ZIP dosyasını ayıklayın (gibi `C:\SDSDK`).

* Yükleme [Android Studio](https://developer.android.com/studio/) ve [Vysor](http://vysor.io/download/) PC'nizde.

* Bir konuşma hizmet elde [abonelik anahtarı](get-started.md). 30 günlük ücretsiz deneme sürümü edinin veya Azure panodan bir anahtarı edinin.

* Konuşma hizmetinin hedefi tanıma kullanmak istiyorsanız, abone [dil anlama hizmet](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (HALUK) ve [bir abonelik anahtarı edinme](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    Gerekebilir [basit bir HALUK model oluşturmak](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) veya HALUK model örneği kullanmak `LUIS-example.json`, konuşma aygıtları SDK'sı bulunan [yükleme sitesine](https://shares.datatransfer.microsoft.com/). Modelinizin JSON dosyasını karşıya [HALUK portal](https://www.lui.ai/applications) tıklayarak **alma yeni uygulama** ve JSON dosyası seçme.

## <a name="set-up-the-development-kit"></a>Geliştirme Seti ayarlayın

1. Geliştirme Seti 's güç bağdaştırıcısı takın. Yeşil güç göstergesi altında üst tablosu açık.

1. Geliştirme Seti mini USB kablosu kullanarak bir bilgisayara bağlanın.

    ![Geliştirme Seti bağlanma](media/speech-devices-sdk/qsg-1.jpg)

1. Geliştirme Seti uygun şekilde hizalanması.

    |Geliştirme Seti yapılandırma|Hizalama|
    |-----------------------------|------------|
    |Döngüsel|Tavan karşılıklı mikrofonlar ile dikey|
    |Doğrusal|Alt tarafında (aşağıda gösterilen) yan yana mikrofonlar ile|

    ![Doğrusal Geliştirme Seti yönü](media/speech-devices-sdk/qsg-2.jpg)

1. Sertifikaları ve uyku modundan çıkarma word (anahtar) tablo dosyası yükleyin ve ses cihazı izinleri ayarlayın. Bir komut penceresinde aşağıdaki komutları yazın.

    > [!NOTE]
    > Bu komutlar Android hata ayıklama köprü kullanmak `adb.exe`, Android Studio yüklemesinin bir parçası olduğu. Bu araç bulunabilir `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. Bu dizin yolunu çağırma daha kullanışlı hale getirmek ekleyebilirsiniz `adb`. Aksi takdirde, yüklemenize tam yolunu belirtmelisiniz `adb.exe` çağırır her komutta `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > Bilgisayarınızın mikrofon ve Konuşmacı sessiz. Bu şekilde, Geliştirme Seti 's mikrofonlar ile çalışıyorsanız ve ses PC'nizden aygıtla yanlışlıkla tetiklemez emin olabilirsiniz.
    
1.  Vysor bilgisayarınızda başlatın.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Cihazınızı altında "bir cihazını Seç" listelenmelidir. Tıklatın **Görünüm** yanında düğmesi. 
 
1.  Tıklayarak, kablosuz ağa bağlanmak **ayarları**, ardından **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
## <a name="run-a-sample-application"></a>Örnek uygulamayı çalıştırın

Roobo testleri çalıştırın ve Geliştirme Seti kurulumunuzu doğrulamak için derleme ve örnek uygulamayı yükleyin.

1.  Android Studio'yu başlatın.

1.  Mevcut bir Android Studio projesini açmak bu seçeneği seçin.

    ![Android studio - var olan proje Aç](media/speech-devices-sdk/qsg-5.png)
 
1.  Gözat `C:\SDSDK\Android-Sample-Release\example`, ardından **Tamam** örnek proje açmak için.
 
1.  Konuşma abonelik anahtarınızı kaynak kodu ekleyin. Hedefi tanıma denemek istiyorsanız, ayrıca ekleyin, [dil anlama hizmet](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarı ve uygulama kimliği 

    Anahtarlarınızı ve uygulama bilgilerini girer kaynak dosyası aşağıdaki satırları `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. Varsayılan Uyandırma Word'ün (anahtar) "." bilgisayardır  İsterseniz, diğer sağlanan birini deneyebilirsiniz Uyandırma sözcükler, "Makinesi" ve "Yardımcısı." Kaynak dosyaları için alternatif bu sözcükleri Konuşma aygıtları SDK'da "anahtar" klasöründe bulunabilir. Örneğin, `C:\SDSDK\Android-Sample-Release\keyword\Computer` "Bilgisayar" için kullanılan dosyaları içerir

    Ayrıca olabilir [özel Uyandırma word oluşturmak](speech-devices-sdk-create-kws.md).

    İstenen Uyandırma word yüklemek için:
 
    * Oluşturma bir `keyword` klasöründe \data\ komut penceresinde aşağıdaki komutları çalıştırarak aygıtta.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Dosyaları kopyalamak `kws.table`, `kws_g.fst`, `kws_k.fst`, ve `words_kw.txt`) cihazın \data\keyword\ klasörüne. O komut penceresinde aşağıdaki komutları çalıştırın.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Bu örnek uygulama dosyalarında başvuru. Aşağıdaki satırları bulmak `MainActivity.java`. Belirtilen anahtar sözcüğü bir kullanmakta olduğunu ve yolu işaret ediyor olun `kws.table` cihaza gönderilir dosya.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > Kendi kodunuzda kullanabilirsiniz `kws.table` anahtar sözcüğü model örneği oluşturun ve tanıma şu şekilde başlatmak için dosya.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Mikrofon dizi geometri ayarlarını içeren aşağıdaki satırları güncelleştirin.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Değişken|Anlamı|Kullanılabilir değerler|
    |--------|-------|----------------|
    |`DeviceGeometry`|Fiziksel MIC yapılandırma|`Circular6+1` Döngüsel Geliştirme Seti|
    ||| `Linear4` Doğrusal Geliştirme Seti|
    |`SelectedGeometry`|Yazılım MIC yapılandırması|`Circular6+1` Tüm mikrofon döngüsel Geliştirme Seti için kullanma|
    |||`Circular3+1` Döngüsel Geliştirme Seti için dört mikrofon kullanma|
    |||`Linear4` Tüm mikrofon doğrusal Geliştirme Seti için kullanma|
    |||`Linear2` Doğrusal Geliştirme Seti için iki mikrofon kullanma|


1.  Uygulama seçerek yapı **'uygulamayı' Çalıştır** Çalıştır menüsünde. Dağıtım hedefini seçin iletişim kutusu görüntülenir. Cihazınızı seçin ve tıklatın **Tamam** aygıta uygulamayı dağıtmak için.

    ![Dağıtım hedefi seçin](media/speech-devices-sdk/qsg-7.png)
 
1.  Burada gösterilen seçenekler görüntüleme konuşma aygıtları SDK örnek uygulamayı başlatır.

    ![Örnek konuşma cihaz uygulaması](media/speech-devices-sdk/qsg-8.png)

1. Bunu yürütmesine!

## <a name="troubleshooting"></a>Sorun giderme

Konuşma hizmetini kullanırken sertifika hataları alırsanız, doğru tarih ve saat aygıtı olduğundan emin olun.

Daha fazla geliştirme için Roobo'nın bilgi [geliştirme Kılavuzu](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo Flash ses sorunları gidermenize yardımcı olabilecek belleğe tüm ses yakalayan bir araç sağlar. Aracı sürümü için her Geliştirme Seti yapılandırma sağlanır. Cihazınızı adresindeki seçin [Roobo site](http://ddk.roobo.com/), ardından **ROOBO Araçları** sayfanın sonundaki bağlantı.
