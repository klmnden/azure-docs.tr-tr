---
title: 'Hızlı Başlangıç: Java (Windows, Linux) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, öğreneceksiniz yakalar ve bilgisayarınızın mikrofondan gelen kullanıcı konuşma dönüştürür basit bir Java uygulaması oluşturma.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 87360d49892698458a021287d88240d98ba2ee19
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55881519"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-java"></a>Hızlı Başlangıç: Java Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, [Konuşma Tanıma Hizmeti SDK'sı](speech-sdk.md) kullanarak bir Java konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanlı olarak konuşmayı metne dönüştüreceksiniz. Uygulama konuşma SDK Maven paketini ve Windows 64-bit veya 64-bit Ubuntu Linux 16.04 Eclipse Java IDE (v4.8) ile oluşturulmuş / 18.04. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: Windows (64-bit) veya Ubuntu Linux 16.04/18.04 (64-bit)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

Ubuntu 16.04/18.04 çalıştırıyorsanız, Eclipse başlatmadan önce bu bağımlılıkların yüklü olduğundan emin olun.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
```

Windows (64-bit) çalıştırıyorsanız, lütfen Microsoft Visual C++ yeniden dağıtılabilir platformunuz için yüklediğinizden emin olun.
* [Microsoft Visual C++ için Visual Studio 2017 yeniden dağıtılabilir'i indirin](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)


## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

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

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasından okuma gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub üzerinde Java örnekleri keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Hızlı Başlangıç: Java (Windows, Linux) konuşma Çevir](quickstart-translate-speech-java-jre.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
