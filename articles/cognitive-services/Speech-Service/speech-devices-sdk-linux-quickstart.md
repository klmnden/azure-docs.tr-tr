---
title: "Hızlı Başlangıç: Konuşma cihaz SDK'sı Linux - konuşma Hizmetleri çalıştırın"
titleSuffix: Azure Cognitive Services
description: Önkoşullar ve yönergeler için bir Linux konuşma cihaz SDK'sı ile çalışmaya başlama.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: erhopf
ms.openlocfilehash: d755f3388466369ee1edc3d9ff1e353173babc10
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723394"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-linux"></a>Hızlı Başlangıç: Konuşma cihaz SDK'sı örnek uygulamayı Linux üzerinde çalıştırın

Bu hızlı başlangıçta, Linux için konuşma cihaz SDK'sı konuşma özellikli bir ürün oluşturun veya olarak kullanmak için nasıl kullanılacağını öğreneceksiniz. bir [konuşma Transkripsiyonu](conversation-transcription-service.md) cihaz. Şu anda yalnızca [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) desteklenir.

Uygulama, konuşma SDK paketini ve 64-bit Linux'ta (Ubuntu 16.04, 18.04 Ubuntu, Debian 9) Eclipse Java IDE (v4) ile oluşturulmuştur. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

Bu kılavuzda gerektiren bir [Azure Bilişsel Hizmetler](get-started.md) konuşma Hizmetleri kaynak hesabı. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

Kaynak kodu [örnek uygulama](https://aka.ms/sdsdk-download-JRE) konuşma cihaz SDK'sı ile eklenmiştir. Ayrıca [github'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: 64-bit Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9)
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) yalnızca.
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* En son sürümünü indirin [konuşma cihaz SDK'sı](https://aka.ms/sdsdk-download-JRE) Java ve çalışma dizininizin .zip ayıklayın.
   > [!NOTE]
   > JRE örnek Release.zip dosya JRE örnek uygulamayı içerir ve bu hızlı başlangıçta uygulama için /home/wcaltest/JRE-Sample-Release ayıklanır varsayar.

Eclipse başlatmadan önce bu bağımlılıkların yüklü olduğundan emin olun.

* Ubuntu üzerinde:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.0 libasound2
  ```

* Debian 9:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.2 libasound2
  ```

Konuşma Transkripsiyonu şu anda yalnızca "en-US" ve "zh-CN", "centralus" ve "ping'in ekran" bölgelerinde kullanılabilir. Bir konuşma anahtarı konuşma Transkripsiyonu kullanmak için bu bölgelerden birinde olmalıdır.

Intents kullanmayı planlıyorsanız ihtiyacınız olacak bir [Language Understanding hizmeti (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) abonelik. LUIS ve niyeti tanıma hakkında daha fazla bilgi için bkz: [amaçlarıyla LUIS, konuşma tanıma C# ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). A [LUIS modeline örnek](https://aka.ms/sdsdk-luis) bu uygulama için kullanılabilir.

## <a name="create-and-configure-the-project"></a>Projesi oluşturun ve yapılandırın

1. Eclipse’i başlatın.

1. İçinde **Eclipse IDE Başlatıcısı**, **çalışma** yeni bir çalışma alanı dizin adını girin. Ardından **Başlat**’ı seçin.

   ![Eclipse Başlatıcısı ekran görüntüsü](media/speech-devices-sdk/eclipse-launcher-linux.png)

1. Çok geçmeden Eclipse IDE ana penceresi görüntülenir. Varsa, Hoş Geldiniz ekranını kapatın.

1. Eclipse menü çubuğundan seçerek yeni bir proje oluşturun **dosya** > **yeni** > **Java projesi**. Yoksa seçin **proje** ardından **Java projesi**.

1. **Yeni bir Java projesi** Sihirbazı'nı başlatır. **Gözat** örnek proje konumu. **Son**’u seçin.

   ![Yeni Java Projesi sihirbazının ekran görüntüsü](media/speech-devices-sdk/eclipse-new-java-project-linux.png)

1. İçinde **paket Gezgini**, projenize sağ tıklayın. Bağlam menüsünden **Yapılandır** > **Maven Projesine Dönüştür**’ü seçin. **Son**’u seçin.

   ![Paket gezgininin ekran görüntüsü](media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. İçinde **paket Gezgini**, projenize sağ tıklayın. Seçin **özellikleri**, ardından **Çalıştır/hata ayıklama ayarları** > **yeni...** > **Java uygulaması**. 

1. **Yapılandırmasını Düzenle** penceresi görüntülenir. İçinde **adı** alana **ana**ve **arama** için **ana sınıfı** bulmak ve seçmek için  **com.microsoft.cognitiveservices.speech.samples.FunctionsList**.

   ![Düzenleme başlatma yapılandırması ekran görüntüsü](media/speech-devices-sdk/eclipse-edit-launch-configuration-linux.png)

1. Yine **yapılandırmasını Düzenle** penceresi seçin **ortam** sayfası ve **yeni**. **Yeni ortam değişkeni** penceresi görüntülenir. İçinde **adı** alana **LD_LIBRARY_PATH** ve **değer** alanı girin, örneğin *.so dosyaları içeren klasör   **/home/wcaltest / JRE örnek sürüm**

1. Kopyalama `kws.table` ve `participants.properties` proje klasörüne **hedef/sınıfları**


## <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Konuşma abonelik anahtarınız için kaynak kodu ekleyin. Amaç tanıma denemek istiyorsanız, ayrıca ekleyin, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarı ve uygulama kimliği

   Konuşma ve LUIS, bilgilerinizi girmeyeceğini `FunctionsList.java`:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Konuşma transkripsiyonu kullanıyorsanız, konuşma anahtarı ve bölge bilgilerinizi de gereklidir `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Varsayılan Uyandırma sözcüğünü (anahtar) "Bilgisayar" dir. Sağlanan diğer birini de deneyebilirsiniz "Machine" veya "Yardımcısı" gibi sözcükleri Uyandırma. Bu alternatif Uyandırma sözcükler için kaynak dosyaları konuşma cihazları SDK'da anahtar sözcüğü klasörü arasındadır. Örneğin, `/home/wcaltest/JRE-Sample-Release/keyword/Computer` Uyandırma için "Bilgisayar" word kullanılan dosyaları içerir.

   > [!TIP]
   > Ayrıca [özel Uyandırma word oluşturmak](speech-devices-sdk-create-kws.md).

    Yeni bir Uyanma sözcük kullanmak için aşağıdaki iki satırları güncelleştirme `FunctionsList.java`ve Uyandırma word paketini uygulamanıza kopyalayın. Örneğin, Uyandırma word paketinden Uyandırma word 'Machine' kullanmak için `kws-machine.zip`:

   * Word Uyandırma paketi proje klasörüne kopyalamak **hedef/sınıfları**.

   * Güncelleştirme `FunctionsList.java` anahtar sözcüğü ve paket adı:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Eclipse'ten örnek uygulamayı çalıştırma

1. Eclipse menü çubuğundan **çalıştırma** > **çalıştırın** 

1. Konuşma cihaz SDK'sı örnek bir uygulama başlar ve şu seçeneklerini gösterir:

   ![Örnek konuşma cihaz SDK'sı örnek uygulama ve seçenekleri](media/speech-devices-sdk/java-sample-app-linux.png)

1. Yeni deneyin **konuşma Transkripsiyonu** Tanıtımı. İle fotoğrafını Başlat **oturumu** > **Başlat**. Varsayılan olarak bir konuk herkese açıktır. Katılımcının ses imzalara sahip olduğunu, ancak bunlar içine yerleştirilebilir `participants.properties` proje klasöründeki **hedef/sınıfları**. Ses imzayı üretmek için bakmak [konuşmaları (SDK) konuşmaların](how-to-use-conversation-transcription-service.md).

   ![Tanıtım konuşma Transkripsiyonu uygulaması](media/speech-devices-sdk/cts-sample-app-linux.png)

## <a name="create-and-run-standalone-the-application"></a>Oluşturma ve tek başına uygulamayı çalıştırma

1. İçinde **paket Gezgini**, projenize sağ tıklayın. Seçin **dışarı**. 
1. **Dışarı** penceresi görüntülenir. Genişletin **Java** seçip **çalıştırılabilir JAR dosyasını** seçip **sonraki**.

   ![Dışarı aktarma penceresinin ekran görüntüsü](media/speech-devices-sdk/eclipse-export-linux.png) 

1. **Çalıştırılabilir JAR dosyasını dışarı** penceresi görüntülenir. Seçin bir **dışa aktarma hedefi** uygulama ve ardından **son**.
 
   ![Çalıştırılabilir JAR dosyasını dışarı aktarma ekran görüntüsü](media/speech-devices-sdk/eclipse-export-jar-linux.png)

1. Lütfen yerleştirme `kws.table` ve `participants.properties` uygulama tarafından desteklenmediklerinden gerektiğinde, yukarıda seçilen hedef klasörde.

1. LD_LIBRARY_LIB *.so dosyalarını içeren klasörü ayarlayın

     ```bash
     export LD_LIBRARY_PATH=/home/wcaltest/JRE-Sample-Release
     ```

1. Tek başına uygulamayı çalıştırmak için

     ```bash
     java -jar SpeechDemo.jar
     ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm notlarını gözden geçirin](devices-sdk-release-notes.md)
