---
title: Konuşma cihaz SDK'sı ile - Konuşma hizmetleri kullanmaya başlayın
titleSuffix: Azure Cognitive Services
description: Önkoşullar ve konuşma cihaz SDK'sını kullanmaya başlamak için yönergeler.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 0d520a9dc8d60a386d25694d441931fbc3ed38e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457609"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Konuşma cihaz SDK'sı ile çalışmaya başlama

Bu makalede, geliştirme PC ve konuşma cihaz geliştirme seti konuşma cihaz SDK'sını kullanarak konuşma özellikli cihazlar geliştirmek için nasıl yapılandırılacağı açıklanır. Ardından, oluşturun ve bir örnek uygulamanın cihaza dağıtın.

Örnek uygulama için kaynak kodu konuşma cihaz SDK'sı ile dahil edilir. Ayrıca [github'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Konuşma cihaz SDK'sı ile geliştirmeye başlamadan önce ihtiyacınız olan yazılım ve bilgi toplayın:

* Alma bir [ROOBO geliştirme Seti'nden](http://ddk.roobo.com/). Setleri, doğrusal veya döngüsel mikrofon dizi yapılandırmalarla kullanılabilir. Gereksinimleriniz için doğru yapılandırmayı seçin.

    |Geliştirme Seti yapılandırma|Konuşmacı konumu|
    |-----------------------------|------------|
    |Döngüsel|Herhangi bir yönde bir CİHAZDAN|
    |Doğrusal|Önünde cihaz|

* Gelen bir Android örnek uygulamasını içerir konuşma cihaz SDK'sının en son sürümü Al [konuşma cihazları SDK indirme sitesi](https://shares.datatransfer.microsoft.com/). .Zip dosyasını C:\SDSDK gibi yerel bir klasöre ayıklayın.

* Yükleme [Android Studio](https://developer.android.com/studio/) ve [Vysor](https://vysor.io/download/) PC'nizde.

* Alma bir [konuşma Hizmetleri abonelik anahtarı](get-started.md). Bir anahtarı, Azure panosundan alma ya da 30 günlük ücretsiz deneme sürümü edinin.

* Konuşma Hizmetleri niyeti tanıma kullanmak istiyorsanız, abone [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) ve [bir abonelik anahtarı edinirler](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription).

    Yapabilecekleriniz [basit LUIS modeline oluşturma](https://docs.microsoft.com/azure/cognitive-services/luis/) veya LUIS modeline LUIS example.json örneği kullanın. LUIS modeline kullanılabilir örnek [konuşma cihazları SDK indirme sitesi](https://shares.datatransfer.microsoft.com/). Modelinizin JSON dosyasını karşıya yüklemek için [LUIS portalı](https://www.luis.ai/home)seçin **alma yeni uygulama**ve ardından JSON dosyasını seçin.

## <a name="set-up-the-development-kit"></a>Geliştirme Seti ' ayarlayın

1. Geliştirme Seti iki mikro USB bağlayıcı vardır. Sol bağlayıcının Geliştirme Seti desteklemek için ve güç aşağıdaki resimde vurgulanmıştır. Bunu kontrol edecek ve işaretlenmiş sağ görüntüde hata ayıklayın.

    ![dev Seti bağlanma](media/speech-devices-sdk/qsg-1.png)

1. Bir bilgisayar için güç bağlantı bağlanın veya bağdaştırıcısı güç mikro USB kablosu kullanarak Geliştirme Seti güçlendirin. Yeşil power göstergesi üst panonun altında görüntülenir.

1. Denetim Geliştirme Seti bağlanın hata ayıklama bağlantı bir bilgisayara ikinci bir mikro USB kablosu kullanarak. Güvenli iletişimler sağlamak için yüksek kaliteli kablo kullanmak için gereklidir.

1. Uygulamanızı Geliştirme Seti için döngüsel ya da doğrusal yapılandırma yönlendirmek.

    |Geliştirme Seti yapılandırma|Hizalama|
    |-----------------------------|------------|
    |Döngüsel|Şekilde, tavan mikrofonlar ile yan yana|
    |Doğrusal|(Aşağıdaki görüntüde gösterilmiştir) alt tarafında, mikrofon ile karşılaşmış|

    ![Doğrusal dev Seti yönü](media/speech-devices-sdk/qsg-2.png)

1. Sertifikaları yükleme ve ses cihazı izinlerini ayarlayın. Bir komut istemi penceresinde aşağıdaki komutları yazın:

   ```
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Android hata ayıklama köprüsü bu komutları kullanmak `adb.exe`, Android Studio yüklemesinin bir parçası olduğu. Bu araç C:\Users bulunur\[kullanıcı adı] \AppData\Local\Android\Sdk\platform araçları. Bu dizin çağırmak daha kullanışlı hale getirmek için yola ekleyebilirsiniz `adb`. Aksi takdirde, yüklemenizin adb.exe çağıran her komut için tam yolunu belirtmeniz gerekir `adb`.
    >
    > Bir hata görürseniz `no devices/emulators found` sonra USB kablosu bağlı ve yüksek kaliteli kablo kontrol edin. Kullanabileceğiniz `adb devices` cihazların bir listesini döndürür gibi geliştirme setine bilgisayarınızı geçebildiğinden emin denetlemek için.

    > [!TIP]
    > Bilgisayarınızın mikrofon ve Geliştirme Seti'nın mikrofonlar ile çalıştığından emin olmak Konuşmacı sessiz. Böylece, yanlışlıkla ses bilgisayardan cihazla tetiklemez.

1.  Bilgisayarınızda Vysor başlatın.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  Cihazınızı altında listelenmelidir **bir cihaz seçin**. Seçin **görünümü** cihazın yanındaki düğmesi.

1.  Klasör simgesini seçerek kablosuz ağınıza bağlayın ve ardından **ayarları** > **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)

    > [!NOTE]
    > Şirketiniz kendi Wi-Fi sisteme cihazlar bağlama hakkında ilkeler varsa, MAC adresini alın ve şirketinizin Wi-Fi'a bağlayın konusunda BT departmanınıza başvurun gerekir.
    >
    > Dev Seti MAC adresini bulmak için dev Seti masaüstünde dosya klasör simgesini seçin.
    >
    >  ![Vysor dosya klasörü](media/speech-devices-sdk/qsg-10.png)
    >
    > Seçin **ayarları**. "İçin mac adresi" için arama yapın ve ardından **Mac adresi** > **Gelişmiş WLAN**. İletişim kutusunun altına görünen MAC adresi yazın.
    >
    > ![Vysor MAC adresi](media/speech-devices-sdk/qsg-11.png)
    >
    > Bazı şirketler, cihazın ne kadar kalabileceği üzerinde bir zaman sınırı olabilir, Wi-Fi sisteme bağlı. Wi-Fi sisteminizle dev Seti'nın kayıt belirli bir gün sayısından sonra genişletmek gerekebilir.
    >
    > Konuşmacı geliştirme setine eklemek istiyorsanız, ses satırına bağlanabilirsiniz. Kaliteli, 3.5 mm Konuşmacı seçmeniz gerekir.
    >
    > ![Vysor ses](media/speech-devices-sdk/qsg-14.png)

## <a name="run-a-sample-application"></a>Örnek uygulamayı çalıştırma

ROOBO testleri çalıştırmak ve Geliştirme Seti kurulumunuzu doğrulama, derleme ve örnek uygulamayı yüklemek için:

1. Android Studio'yu başlatın.

1. **Var olan Android Studio projesini aç**'ı seçin.

   ![Android Studio - mevcut bir projeyi Aç](media/speech-devices-sdk/qsg-5.png)

1. İçin C:\SDSDK\Android-Sample-Release\example gidin. Seçin **Tamam** örnek projesini açın.

1. Konuşma abonelik anahtarınız için kaynak kodu ekleyin. Amaç tanıma denemek istiyorsanız, ayrıca ekleyin, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarı ve uygulama kimliği

   Uygulama bilgilerini ve anahtarları MainActivity.java kaynak dosyası aşağıdaki satırları gidin:

   ```java
   // Subscription
   private static final String SpeechSubscriptionKey = "[your speech key]";
   private static final String SpeechRegion = "westus";
   private static final String LuisSubscriptionKey = "[your LUIS key]";
   private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
   private static final String LuisAppId = "[your LUIS app ID]"
   ```

1. Varsayılan Uyandırma sözcüğünü (anahtar) "Bilgisayar" dir. Sağlanan diğer birini de deneyebilirsiniz "Machine" veya "Yardımcısı" gibi sözcükleri Uyandırma. Bu alternatif Uyandırma sözcükler için kaynak dosyaları konuşma cihazları SDK'da anahtar sözcüğü klasörü arasındadır. Örneğin, C:\SDSDK\Android-Sample-Release\keyword\Computer Uyandırma için "Bilgisayar" word kullanılan dosyaları içerir.

    Ayrıca [özel Uyandırma word oluşturmak](speech-devices-sdk-create-kws.md).

    Yeni bir Uyanma sözcük kullanmak için MainActivity.java aşağıdaki iki satırlarını güncelleştirin ve uygulamanıza Uyandırma word paketi kopyalayın. Örneğin Uyandırma kullanmak için 'Machine' Uyandırma word paket kws word-machine.zip:

   * Uyandırma word paketi "C:\SDSDK\Android-Sample-Release\example\app\src\main\assets\" klasörüne kopyalayın.
   * MainActivity.java paket adı ve anahtar sözcüğü ile güncelleştirin: 
    
     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. Mikrofon dizi geometri ayarları içeren aşağıdaki satırları güncelleştirin:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```
   Aşağıdaki tabloda kullanılabilir değerleri açıklanmaktadır:

   |Değişken|Anlamı|Kullanılabilir değerler|
   |--------|-------|----------------|
   |`DeviceGeometry`|Fiziksel MIC yapılandırma|Döngüsel Geliştirme Seti için: `Circular6+1` |
   |||Doğrusal Geliştirme Seti için: `Linear4`|
   |`SelectedGeometry`|Yazılım MIC yapılandırması|Tüm mikrofonlardan kullanan bir döngüsel Geliştirme Seti için: `Circular6+1`|
   |||Dört mikrofonlardan kullanan bir döngüsel Geliştirme Seti için: `Circular3+1`|
   |||Tüm mikrofonlardan kullanan bir doğrusal Geliştirme Seti için: `Linear4`|
   |||İki mikrofonlardan kullanan bir doğrusal Geliştirme Seti için: `Linear2`|


1. Uygulamayı derlemek için **çalıştırma** menüsünde **'uygulamayı' Çalıştır**. **Dağıtım hedefini seçin** iletişim kutusu görüntülenir.

1. Cihazınızı seçin ve ardından **Tamam** cihaza uygulamayı dağıtmak için.

    ![Dağıtım hedefi iletişim kutusunu seçin](media/speech-devices-sdk/qsg-7.png)

1. Konuşma cihaz SDK'sı örnek bir uygulama başlar ve şu seçeneklerini gösterir:

   ![Örnek konuşma cihaz SDK'sı örnek uygulama ve seçenekleri](media/speech-devices-sdk/qsg-8.png)

1. Deneyin!

## <a name="troubleshooting"></a>Sorun giderme

### <a name="certificate-failures"></a>Sertifika hataları

Konuşma hizmetlerini kullanırken sertifika hataları alırsanız, cihazınız doğru tarih ve saat olduğundan emin olun:

1. Git **ayarları**. Altında **sistem**seçin **tarih ve saat**.

    ![Tarih ve saat ayarları altında seçin](media/speech-devices-sdk/qsg-12.png)

1. Tutun **otomatik tarih ve saat** seçeneği belirlenmiş. Altında **Select saat dilimi**, geçerli saat diliminizde seçin.

    ![Tarih ve saat dilimi seçenekleri belirleyin](media/speech-devices-sdk/qsg-13.png)

    Dev Seti'nın zaman zaman bilgisayarınızda eşleştiğini gördüğünüzde, dev Seti internet'e bağlı.

    Daha fazla geliştirme için bilgi [ROOBO geliştirme Kılavuzu](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

### <a name="audio"></a>Ses

ROOBO bellek Flash tüm ses yakalayan bir araç sağlar. Ses sorunları gidermenize yardımcı olabilir. Aracı sürümü her Geliştirme Seti yapılandırması için sağlanır. Üzerinde [ROOBO site](http://ddk.roobo.com/)Cihazınızı seçin ve ardından **ROOBO Araçları** sayfanın alt kısmındaki bağlantı.
