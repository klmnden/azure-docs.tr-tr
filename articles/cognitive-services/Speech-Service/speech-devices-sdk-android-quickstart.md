---
title: "Hızlı Başlangıç: Konuşma cihaz SDK'sı Android'de - konuşma hizmetleri çalıştırma"
titleSuffix: Azure Cognitive Services
description: Önkoşullar ve yönergeler için bir Android konuşma cihaz SDK'sı ile çalışmaya başlama.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 7eea978456ed565f8fc58647dc548d1a7bc76b27
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606372"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-android"></a>Hızlı Başlangıç: Android'de konuşma cihaz SDK'sı örnek uygulamayı çalıştırma

Bu hızlı başlangıçta, konuşma tanıma özellikli bir ürün oluşturun veya olarak kullanmak için Android için konuşma cihaz SDK'yı kullanmayı öğreneceksiniz. bir [konuşma Transkripsiyonu](conversation-transcription-service.md) cihaz.

Bu kılavuzda gerektiren bir [Azure Bilişsel Hizmetler](get-started.md) konuşma Hizmetleri kaynak hesabı. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

Örnek uygulama için kaynak kodu konuşma cihaz SDK'sı ile dahil edilir. Ayrıca [github'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Konuşma cihaz SDK'sını kullanmaya başlamadan önce yapmanız gerekir:

* Birlikte verilen yönergeleri izleyin, [Geliştirme Seti](get-speech-devices-sdk.md) için cihazdaki güç.

* En son sürümünü indirin [konuşma cihaz SDK'sı](https://aka.ms/sdsdk-download)ve çalışma dizininize .zip ayıklayın.
   > [!NOTE]
   > Android örnek uygulaması Android örnek Release.zip dosyasını içerir ve bu hızlı başlangıçta uygulama için C:\SDSDK\Android-Sample-Release ayıklanır varsayar.

* Alınacak bir [konuşma Hizmetleri için Azure abonelik anahtarı](get-started.md)

* Konuşma Transkripsiyonu kullanmayı planlıyorsanız kullanmalısınız bir [döngüsel mikrofon cihaz](get-speech-devices-sdk.md) ve bu özellik şu anda yalnızca "en-US" ve "zh-CN" bölgeleri, "centralus" ve "ping'in ekran" için kullanılabilir. Bir konuşma anahtarı konuşma Transkripsiyonu kullanmak için bu bölgelerden birinde olmalıdır.

* Konuşma Hizmetleri hedefleri (veya Eylemler) kullanıcı konuşma tanımlamak için kullanmayı planlıyorsanız, ihtiyacınız olacak bir [Language Understanding hizmeti (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) abonelik. LUIS ve niyeti tanıma hakkında daha fazla bilgi için bkz: [amaçlarıyla LUIS, konuşma tanıma C# ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp).

    Yapabilecekleriniz [basit LUIS modeline oluşturma](https://docs.microsoft.com/azure/cognitive-services/luis/) veya LUIS modeline LUIS example.json örneği kullanın. LUIS modeline kullanılabilir örnek [konuşma cihazları SDK indirme sitesi](https://aka.ms/sdsdk-luis). Modelinizin JSON dosyasını karşıya yüklemek için [LUIS portalı](https://www.luis.ai/home)seçin **alma yeni uygulama**ve ardından JSON dosyasını seçin.

* Yükleme [Android Studio](https://developer.android.com/studio/) ve [Vysor](https://vysor.io/download/) PC'nizde.

## <a name="set-up-the-device"></a>Cihazı ayarlama

1. Bilgisayarınızda Vysor başlatın.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1. Cihazınızı altında listelenmelidir **bir cihaz seçin**. Seçin **görünümü** cihazın yanındaki düğmesi.

1. Klasör simgesini seçerek kablosuz ağınıza bağlayın ve ardından **ayarları** > **WLAN**.

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

## <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Geliştirme Seti kurulumunuzu doğrulamak için derleme ve örnek uygulamayı yükleyin:

1. Android Studio'yu başlatın.

1. **Var olan Android Studio projesini aç**'ı seçin.

   ![Android Studio - mevcut bir projeyi Aç](media/speech-devices-sdk/qsg-5.png)

1. İçin C:\SDSDK\Android-Sample-Release\example gidin. Seçin **Tamam** örnek projesini açın.

1. Konuşma abonelik anahtarınız için kaynak kodu ekleyin. Amaç tanıma denemek istiyorsanız, ayrıca ekleyin, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarı ve uygulama kimliği

   Konuşma ve LUIS, bilgilerinizi MainActivity.java geçer:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Konuşma transkripsiyonu kullanıyorsanız, konuşma anahtarı ve bölge bilgilerinizi de conversation.java içinde gerekli:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Varsayılan Uyandırma sözcüğünü (anahtar) "Bilgisayar" dir. Sağlanan diğer birini de deneyebilirsiniz "Machine" veya "Yardımcısı" gibi sözcükleri Uyandırma. Bu alternatif Uyandırma sözcükler için kaynak dosyaları konuşma cihazları SDK'da anahtar sözcüğü klasörü arasındadır. Örneğin, C:\SDSDK\Android-Sample-Release\keyword\Computer Uyandırma için "Bilgisayar" word kullanılan dosyaları içerir.

   > [!TIP]
   > Ayrıca [özel Uyandırma word oluşturmak](speech-devices-sdk-create-kws.md).

    Yeni bir Uyanma sözcük kullanmak için aşağıdaki iki satırları güncelleştirme `MainActivity.java`ve Uyandırma word paketini uygulamanıza kopyalayın. Örneğin, Uyandırma word paket kws Uyandırma Word'den 'Machine' kullanmak için-machine.zip:

   * Uyandırma word paketi "C:\SDSDK\Android-Sample-Release\example\app\src\main\assets\" klasörüne kopyalayın.
   * Güncelleştirme `MainActivity.java` anahtar sözcüğü ve paket adı:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. Mikrofon dizi geometri ayarları içeren aşağıdaki satırları güncelleştirin:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   Bu tabloda, desteklenen değerler listelenmiştir:

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

1. Konuşma Transkripsiyonu Tanıtımı deneyin. 'Start Session' ile çoğaltmaya başlayın. Varsayılan olarak bir konuk herkese açıktır. Katılımcının ses imzalara sahip olduğunu, ancak bunlar bir dosyaya konulabilir `/video/participants.properties` cihazda. Ses imzayı üretmek için bakmak [konuşmaları (SDK) konuşmaların](how-to-use-conversation-transcription-service.md).

   ![Tanıtım konuşma Transkripsiyonu uygulaması](media/speech-devices-sdk/qsg-15.png)

1. Deneyin!

## <a name="troubleshooting"></a>Sorun giderme

   Konuşma cihaza bağlanamıyorsanız. Bir komut istemi penceresinde aşağıdaki komutu yazın. Bu cihazların bir listesini döndürür:

   ```powershell
    adb devices
   ```

   > [!NOTE]
   > Bu komut, Android hata ayıklama köprüsü kullanır `adb.exe`, Android Studio yüklemesinin bir parçası olduğu. Bu araç C:\Users bulunur\[kullanıcı adı] \AppData\Local\Android\Sdk\platform araçları. Bu dizin çağırmak daha kullanışlı hale getirmek için yola ekleyebilirsiniz `adb`. Aksi takdirde, yüklemenizin adb.exe çağıran her komut için tam yolunu belirtmeniz gerekir `adb`.
   >
   > Bir hata görürseniz `no devices/emulators found` sonra USB kablosu bağlı denetleyin ve yüksek kaliteli kablo kullanılan emin olun.
   >

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm notlarını gözden geçirin](devices-sdk-release-notes.md)
