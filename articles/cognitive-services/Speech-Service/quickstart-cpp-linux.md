---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sı kullanarak Linux'ta c++ konuşma tanıma | Microsoft Docs"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sı kullanarak Linux'ta c++ Konuşma tanımayı öğrenmesine
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: b5f5cdbe202b406c724a9f4f5787e566b432a66c
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116155"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-using-the-speech-sdk"></a>Hızlı Başlangıç: c++ konuşma Speech SDK'sı kullanarak Linux'ta tanıması

Bu makalede, Linux (Ubuntu 16.04) Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sını kullanarak bir C++ konsol uygulaması oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir Ubuntu 16.04 bilgisayar bir çalışma mikrofon ile.
* Yüklenecek paketler oluşturmak ve bu örneği çalıştırmak için gereken şu komutu çalıştırın:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="get-the-speech-sdk"></a>Konuşma SDK'sı Al

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel hizmetler konuşma SDK'ın geçerli sürümü `0.5.0`.

Bilişsel hizmetler konuşma SDK'sı Linux, 64-bit ve 32-bit uygulamaları derleme için kullanılabilir.
Gerekli dosyaları tar dosyasından olarak indirilebilir https://aka.ms/csspeech/linuxbinary.
İndirin ve SDK'sını aşağıda gösterildiği gibi yükleyin:

1. Bir dizin (mutlak yol), üst bilgiler ve Speech SDK'sı ikili dosyaları yerleştirmek istediğiniz seçin.
   Örneğin, yol çekme `speechsdk` giriş dizininizin altında:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa bir dizin oluşturun:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. İndirin ve ayıklayın `.tar.gz` Speech SDK'sı ikili dosyalarla arşiv:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Ayıklanan paketin en üst düzey dizinin içeriklerini doğrulama:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Üçüncü taraf bildirimi ve lisans dosyaları göstermelidir yanı sıra bir `include` üstbilgi dizinini ve `lib` kitaplıkları için dizin.

   [!include[Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. Adlı bir dosyaya aşağıdaki kodu ekleyin `helloworld.cpp`:

  [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

## <a name="building"></a>Derleniyor

> [!NOTE]
> Aşağıdaki yapı komutları olarak kopyalayıp emin bir _tek satırlı_.

* Üzerinde bir **x64** makine, uygulamayı oluşturmak için aşağıdaki komutu çalıştırın:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Üzerinde bir **x86** makine, uygulamayı oluşturmak için aşağıdaki komutu çalıştırın:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Yapılandırma yükleyicisinin kitaplık yolu Speech SDK'sı Kitaplığı'na işaret edecek şekilde yapılandırın.

   * Üzerinde bir **x64** makine çalıştırın:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Üzerinde bir **x86** makine çalıştırın:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Uygulamayı şu şekilde çalıştırın:

   ```sh
   ./helloworld
   ```

1. Şuna benzer bir çıktı görmeniz gerekir:

   ```text
   Say something...
   We recognized: What's the weather
   ```

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/cpp-linux` klasör.

## <a name="next-steps"></a>Sonraki adımlar

* Ziyaret [örnekleri sayfası](samples.md) ek örnekler için.
