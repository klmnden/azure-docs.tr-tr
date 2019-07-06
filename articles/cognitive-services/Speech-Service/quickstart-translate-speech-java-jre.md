---
title: 'Hızlı Başlangıç: Java (Windows, Linux) - konuşma Hizmetleri konuşma Çevir'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, komut satırına metin çıktısı kullanıcı konuşma yakalamak ve başka bir dile çevirmek için basit bir Java uygulaması oluşturacaksınız. Bu kılavuz, Windows ve Linux kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 68567e673cf6fb53b8d9b6f7b9ef96aee691ac3a
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604961"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-java"></a>Hızlı Başlangıç: Java Speech SDK'sı ile Konuşma Çevir

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma metin](quickstart-java-jre.md) ve [ses öncelikli sanal asistan](quickstart-virtual-assistant-java-jre.md).

Bu hızlı başlangıçta, bilgisayarınızın mikrofondan gelen kullanıcı konuşma yakalar, konuşma çevirir ve gerçek zamanlı olarak komut satırında çevrilmiş metne dönüştürür basit bir Java uygulaması oluşturacaksınız. Bu uygulama, Windows 64-bit veya 64-bit Linux'ta (Ubuntu 16.04, 18.04 Ubuntu, Debian 9) veya macOS 10.13 veya sonraki bir sürümü üzerinde çalışacak şekilde tasarlanmıştır. Konuşma SDK Maven paketini ve Eclipse Java IDE ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dilleri tam bir listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: 64 bit Windows, 64-bit Linux (Ubuntu 16.04, 18.04 Ubuntu, Debian 9) veya macOS 10.13 veya üzeri
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

Linux çalıştırıyorsanız, Eclipse başlatmadan önce bu bağımlılıkların yüklü olduğundan emin olun.

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

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Java projenize yeni bir boş sınıf eklemek için **Dosya** > **Yeni** > **Sınıf** seçeneklerini belirleyin.

1. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. `Main.java` içindeki tüm kodları şu kod parçacığıyla değiştirin:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.

Mikrofondan giriş konuşma Almanca transcribed ve konsol penceresinde günlüğe kaydedilir. Konuşma Yakalamayı durdurmak için "Enter" tuşuna basın.

![Başarılı tanıma sonrası konsol çıktısının ekran görüntüsü](media/sdk/qs-translate-java-jre-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasını okuma ve Sentezlenen konuşma olarak çevrilmiş metin çıktısı gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub üzerinde Java örnekleri keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Hızlı Başlangıç: Java (Windows, Linux), konuşma tanıma](quickstart-java-jre.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
