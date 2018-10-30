---
title: 'Hızlı Başlangıç: Java’da (Windows veya Linux) konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Java’da (Windows veya Linux) konuşma tanıma işlemini öğrenin
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: fmegen
ms.openlocfilehash: 80ddef79392acb677555ed795bf429f5ec0266a0
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467268"
---
# <a name="quickstart-recognize-speech-in-java-on-windows-or-linux-by-using-the-speech-service-sdk"></a>Hızlı Başlangıç: Konuşma Tanıma Hizmeti SDK’sını kullanarak Windows veya Linux üzerinde Java’da konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, [Konuşma Tanıma Hizmeti SDK'sı](speech-sdk.md) kullanarak bir Java konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanlı olarak konuşmayı metne dönüştüreceksiniz. Uygulama; 64 bit Windows veya Ubuntu Linux 16.04’te Eclipse Java IDE (v4.8) ve Konuşma SDK’sı Maven paketi ile derlenmiştir. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Ön koşullar

Bu Hızlı Başlangıcı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [Konuşma Tanıma Hizmeti’ni ücretsiz olarak deneme](get-started.md).


## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

Ubuntu 16.04 kullanıyorsanız, Eclipse’i başlatmadan önce gerekli paketlerin yüklendiğinden emin olmak için aşağıdaki komutları çalıştırın.

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

1. Eclipse’i başlatın.

1. Eclipse Başlatıcısı’nda **Çalışma Alanı** alanına yeni bir çalışma alanı dizininin adını girin. Ardından **Başlat**’ı seçin.

   ![Eclipse Başlatıcısı ekran görüntüsü](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Çok geçmeden Eclipse IDE ana penceresi görüntülenir. Varsa, Hoş Geldiniz ekranını kapatın.

1. Eclipse menü çubuğundan, **Dosya** > **Yeni** > **Proje** seçeneklerini belirleyerek yeni bir proje oluşturun.

1. **Yeni Proje** iletişim kutusu görünür. **Java Projesi**’ni ve sonra **İleri**’yi seçin.

   ![Java Projesi vurgulanmış şekilde, Yeni Proje iletişim kutusunun ekran görüntüsü](media/sdk/qs-java-jre-02-select-wizard.png)

1. Yeni Java Projesi sihirbazı başlatılır. **Proje adı** alanına **hızlı başlangıç** yazın ve yürütme ortamı olarak **JavaSE-1.8** seçeneğini belirleyin. **Son**’u seçin.

   ![Yeni Java Projesi sihirbazının ekran görüntüsü](media/sdk/qs-java-jre-03-create-java-project.png)

1. **İlişkili Perspektif Açılsın mı?** penceresi görüntülenirse **Perspektifi Aç**’ı seçin.

1. **Paket gezgini**’nde **hızlı başlangıç** projesine sağ tıklayın. Bağlam menüsünden **Yapılandır** > **Maven Projesine Dönüştür**’ü seçin.

   ![Paket gezgininin ekran görüntüsü](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. **Yeni POM Oluştur** penceresi görüntülenir. **Grup Kimliği** alanına **com.microsoft.cognitiveservices.speech.samples** girin ve **Yapıt Kimliği** alanına **hızlı başlangıç** yazın. Ardından **Son**’u seçin.

   ![Yeni POM Oluştur penceresinin ekran görüntüsü](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. **pom.xml** dosyasını açıp düzenleyin.

   * Dosyanın sonunda, `</project>` kapanış etiketinden önce burada gösterildiği gibi Konuşma SDK’sı için Maven deposuna başvuru içeren bir `repositories` öğesi oluşturun:

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Ayrıca Konuşma SDK'sı sürüm 1.0.1 bağımlılığına sahip bir `dependencies` öğesi de oluşturun:

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

   * Değişiklikleri kaydedin.

## <a name="add-sample-code"></a>Örnek kod ekleme

1. Java projenize yeni bir boş sınıf eklemek için **Dosya** > **Yeni** > **Sınıf** seçeneklerini belirleyin.

1. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. `Main.java` içindeki tüm kodları şu kod parçacığıyla değiştirin:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.
Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

![Başarılı tanıma sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-java-jre-07-console-output.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/java-jre` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Java için Konuşma SDK’sını kullanarak konuşmadaki amacı tanıma](how-to-recognize-intents-from-speech-java.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
