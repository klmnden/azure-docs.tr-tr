---
title: C++ ve Linux için konuşma SDK Quickstart | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get konuşma SDK'sı ile Linux ve Bilişsel hizmetler C++ kullanmaya başlayın.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: cee70ba585f93dda3249fc5b39f25fb613b57a45
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753680"
---
# <a name="quickstart-for-c-and-linux"></a>C++ ve Linux için hızlı başlangıç

Bilişsel hizmetler konuşma SDK'ın geçerli sürümü `0.4.0`.

Bilişsel hizmetler konuşma SDK'sı Linux için 64-bit ve 32-bit uygulamaları oluşturma için kullanılabilir. Gerekli dosyalar tar dosyasından olarak indirilebilir https://aka.ms/csspeech/linuxbinary.

> [!NOTE]
> C++ ve Windows için hızlı başlangıç arıyorsanız, Git [burada](quickstart-cpp-windows.md).
> C# ve Windows için hızlı başlangıç için arıyorsanız, Git [burada](quickstart-csharp-windows.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Bu yönergeler, Ubuntu 16.04 (x86 veya x64) PC'de üzerinde çalıştırıyorsanız varsayalım.
> Farklı bir Ubuntu sürüm veya başka bir Linux dağıtım, gerekli adımları uyarlamanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

[!include[Ubuntu Prerequisites](includes/ubuntu1604-prerequisites.md)]

## <a name="getting-the-binary-package"></a>İkili paketini alma

[!include[License Notice](includes/license-notice.md)]

1. Burada konuşma SDK ikili dosyaları ve üstbilgileri yerleştirmek istediğiniz bir dizin (mutlak yolu) seçin.
   Örneğin, yol çekme `speechsdk` giriş dizininize altında:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. İndirmeyi ve ayıklamayı `.tar.gz` konuşma SDK ikililerini arşiv:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Ayıklanan paketin en üst düzey dizinin içeriğini doğrulama:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Üçüncü taraf dikkat edin ve lisans dosyaları göstermelidir yanı sıra bir `include` üstbilgileri için dizin ve `lib` kitaplıkları için dizin.

   [!include[Linux Binary Archive Content](includes/linuxbinary-content.md)]

## <a name="sample-code"></a>Örnek kod

Aşağıdaki kod Mikrofonunuz gelen İngilizce konuşma tanır.
Adlı bir dosyaya yerleştirin `quickstart-linux.cpp`:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Linux/quickstart-linux/quickstart-linux.cpp#code)]

Abonelik anahtarı kodda aldığınız adla değiştirin.

## <a name="building"></a>Derleniyor

> [!NOTE]
> Aşağıdaki yapı komutları olarak kopyalayıp emin olun bir _tek satır_.

* X x64 uygulamayı oluşturmak için aşağıdaki komutu çalıştırın makine:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Bir x86 uygulamayı oluşturmak için aşağıdaki komutu çalıştırın makine:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="running"></a>Çalışıyor

Uygulamayı çalıştırmak için loader'ın kitaplık yolu konuşma SDK'sı Kitaplığı'na işaret edecek şekilde yapılandırmanız gerekir.

* X x64 üzerinde makine, çalıştırın:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
  ```

* Bir x86 üzerinde makine, çalıştırın:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
  ```

Uygulamayı aşağıdaki gibi çalıştırın:

```sh
./quickstart-linux
```

Tüm kalırsa iyi, aşağıdakine benzer bir çıktı görmeniz gerekir:

```text
Say something...
We recognized: What's the weather
```

## <a name="downloading-the-sample"></a>Örnek indirme

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Sonraki adımlar

* Ziyaret [örnekleri sayfa](samples.md) ek örnekler için.
