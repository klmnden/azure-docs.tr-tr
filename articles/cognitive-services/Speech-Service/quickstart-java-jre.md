---
title: 'Hızlı Başlangıç: Java (Windows veya Linux), konuşma tanıma'
titleSuffix: Microsoft Cognitive Services
description: Java (Windows veya Linux) konuşma tanımayı öğrenmesine
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 08/16/2018
ms.author: fmegen
ms.openlocfilehash: 923ab3378d5e2d833e11c5111d4dd9964fea6dc4
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126622"
---
# <a name="quickstart-recognize-speech-in-java-windows-or-linux"></a>Hızlı Başlangıç: Java (Windows veya Linux), konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu belge, bir Java tabanlı bir konsol uygulaması Java Çalışma zamanı ortamı (yaptığı konuşma SDK'yı JRE için) oluşturmayı açıklar.
Microsoft Bilişsel hizmetler SDK Maven paketi tabanlı bir uygulama.
Eclipse bir tümleşik geliştirme ortamı (IDE) olarak kullanırız.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir bilgisayar (x64 Windows, Ubuntu 16.04 x64) ile çalışma mikrofon Eclipse çalıştırma yeteneği.
* 64 bit JRE/JDK 8 Java için.
* 4.8 sürümünü [Eclipse](https://www.eclipse.org), 64 bit sürümü.
* Ubuntu 16.04 üzerinde gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="create-and-configure-your-project"></a>Oluşturun ve projenizi yapılandırın

1. Eclipse'i başlatın.

1. Eclipse başlatıcısında yeni bir dizin adını **çalışma** alan.
   Ardından **başlatma**.

   ![Eclipse çalışma alanı oluşturma](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Bir süre sonra Eclipse IDE'yi ana penceresi gösterilir.
   Varsa bir Hoş Geldiniz ekranı içinde kapatın.

1. Seçin **dosya** \> **yeni** \> **proje**.

1. İçinde **yeni proje** select görüntülenen Sihirbazı **Java projesi**, tıklatıp **sonraki**.

   ![Sihirbaz seçin](media/sdk/qs-java-jre-02-select-wizard.png)

1. Sonraki penceresinde girin **hızlı** bir proje olarak adlandırın ve seçin **JavaSE 1.8** (veya üzeri) yürütme ortamı olarak.
   **Son**'a tıklayın.

   ![Sihirbaz seçin](media/sdk/qs-java-jre-03-create-java-project.png)

1. Başlıklı bir pencere durumunda **ilişkili perspektifi Aç?** açılarak, select **perspektifi Aç**.

1. İçinde **paket Gezgini**, sağ **hızlı** proje ve seçin **yapılandırma** \> **MavenprojesineDönüştür**.

   ![Maven projesine Dönüştür](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Açılan pencerede girin **com.microsoft.cognitiveservices.speech.samples** olarak **Grup Kimliği** ve **hızlı** olarak **Yapıt kimliği**. **Son**’u seçin.

   ![Maven POM yapılandırın](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Düzen **pom.xml** dosya.

  * Kapanış etiketi önce dosyanın sonunda `</project>`, Maven deposu başvurusuyla Speech SDK'sı için depoları bölümünde oluşturun:

    [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Ayrıca, daha sonra bir bağımlılıklar bölümüne Speech SDK'sı 0.6.0 sürümü ile bir bağımlılık olarak ekleyin:

    [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

  * Değişiklikleri kaydedin.

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. Seçin **dosya** \> **yeni** \> **sınıfı** Java projenize yeni bir boş sınıf eklemek için.

1. Penceresinde **yeni Java sınıfı** girin **speechsdk.quickstart** içine **paket** alan ve **ana** içine **adı**  alan.

   ![Main sınıfı oluşturma](media/sdk/qs-java-jre-06-create-main-java.png)

1. Tüm değiştirin `Main.java` aşağıdaki kod parçacığıyla:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

F11 tuşuna basın veya **çalıştırma** \> **hata ayıklama**.
Sonraki 15 saniye Konuşma giriş mikrofonu den tanınan ve konsol penceresinde günlüğe kaydedilir.

![Başarılı tanıma sonra konsol çıktısı](media/sdk/qs-java-jre-07-console-output.png)

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/java-jre` klasör.

## <a name="next-steps"></a>Sonraki adımlar

* [Örneklerimizi Al](speech-sdk.md#get-the-samples)
