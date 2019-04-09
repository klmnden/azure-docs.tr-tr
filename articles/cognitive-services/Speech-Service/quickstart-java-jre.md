---
title: 'Hızlı Başlangıç: Java (Windows, Linux) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, yakalar ve bilgisayarınızın mikrofondan gelen kullanıcı konuşma dönüştürür basit bir Java uygulaması oluşturulacağını öğreneceksiniz.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: fmegen
ms.openlocfilehash: fe565d63e72b5ec2798dde03ba4f4bd9ff4f48a7
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59009408"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-java"></a>Hızlı Başlangıç: Java Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, [Konuşma SDK'sı](speech-sdk.md) kullanarak bir Java konsol uygulaması oluşturacaksınız. Bilgisayarınızın mikrofonundan gerçek zamanlı olarak konuşmayı metne dönüştüreceksiniz. Konuşma SDK Maven paketini ve 64-bit Windows üzerinde Eclipse Java IDE (v4.8) ile oluşturulan uygulama 64-bit Ubuntu Linux 16.04 / 18.04 veya macOS 10.13 veya üzeri. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: (64-bit) Windows, Ubuntu Linux 16.04/18.04 (64-bit) ve macOS 10.13 veya üzeri
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).

Ubuntu 16.04/18.04 çalıştırıyorsanız, Eclipse başlatmadan önce bu bağımlılıkların yüklü olduğundan emin olun.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Windows (64-bit) çalıştırıyorsanız, Microsoft Visual C++ yeniden dağıtılabilir platformunuz için yüklediğinizden emin olun.
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
