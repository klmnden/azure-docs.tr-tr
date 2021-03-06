---
title: 'Hızlı Başlangıç: C++ (macOS) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Speech SDK'sı kullanarak macOS üzerinde c++ Konuşma tanımayı öğrenmesine
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: de170955ced45040541565800fc9853edea322f5
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603073"
---
# <a name="quickstart-recognize-speech-in-c-on-macos-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak macOS üzerinde c++ konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, macOS 10.13 ve üzeri bir C++ konsol uygulaması oluşturun. Bilişsel Hizmetler'i kullanma [Speech SDK'sı](speech-sdk.md) , Mac mikrofondan metni gerçek zamanlı konuşma tanıma özelliği. Uygulamanın oluşturulduğu [Speech SDK'sı macOS için](https://aka.ms/csspeech/macosbinary) ve Mac'ın varsayılan C++ derleyicisi (örneğin, `g++`).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için bir konuşma Hizmetleri abonelik anahtarı ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Bkz: [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) Ayrıntılar için.

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`.

Sıkıştırılmış çerçeve paketinden olarak Speech SDK'sı macOS için indirilebilir https://aka.ms/csspeech/macosbinary.

SDK'yı aşağıda gösterildiği gibi indirin ve yükleyin:

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. İndirin ve ayıklayın `.zip` Speech SDK'sı çerçevesi içeren arşiv:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizin listeleme lisans dosyaları ve üçüncü taraf bildirimi içermelidir yanı sıra bir `MicrosoftCognitiveServicesSpeech.framework` dizin.

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. `helloworld.cpp` adlı bir C++ kaynak dosyası oluşturun ve aşağıdaki kodu dosyaya yapıştırın.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-macos/helloworld.cpp#code)]

1. Bu yeni dosya, dize değiştirin `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınız ile.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="build-the-app"></a>Uygulama oluşturma

> [!NOTE]
> Aşağıdaki komutları _tek bir komut satırı_ olarak girdiğinizden emin olun. Bunu yapmanın en kolay yolu, komutu her komutun yanındaki **Kopyala** düğmesini kullanarak kopyalayıp kabuk isteminize yapıştırmaktır.

* Uygulamayı oluşturmak için aşağıdaki komutu çalıştırın.

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Yükleyicinin kitaplık yolunu Konuşma SDK'sı kitaplığına işaret edecek şekilde yapılandırın.

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. Uygulamayı çalıştırın.

   ```sh
   ./helloworld
   ```

1. Konsol penceresinde bir istem görünerek bir şey söylemenizi ister. İngilizce bir deyim ya da cümle söyleyin. Konuşma konuşma hizmetlere iletilen ve transcribed aynı pencerede görünen metin.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde C++ örneklerini keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
