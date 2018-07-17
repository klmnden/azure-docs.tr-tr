---
title: Bilişsel hizmetler konuşma SDK'sı hakkında | Microsoft Docs
description: Konuşma hizmeti için kullanılabilir SDK'lar genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: bf37ffa9b5a185845c8eeafa7e00d352486fdce2
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069473"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Bilişsel hizmetler konuşma SDK hakkında

Bilişsel hizmetler konuşma Yazılım Geliştirme Seti (SDK), uygulamalarınızın yazılım geliştirmeyi kolaylaştıran konuşma hizmeti işlevlerini yerel erişim sağlar. Şu anda Erişim SDK'sı sağlar **Konuşmayı metne dönüştürme**, **konuşma çevirisi**, ve **amaç tanıma**.

[!include[Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!include[License Notice](includes/license-notice.md)]

## <a name="get-the-windows-sdk"></a>Windows SDK'sı Al

Speech SDK'sı Windows sürümünü, 32-bit ve 64-bit C/C++ istemci kitaplıklarının yanı sıra C# ile kullanmak için yönetilen (.NET) kitaplıkları içerir. SDK'sı NuGet kullanarak Visual Studio'da yüklenebilir; Arama `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Linux SDK'yı edinme

Aşağıdaki Kabuk komutları çalıştırarak gerekli derleyici ve kitaplıkları olduğundan emin olun:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Ubuntu 16.04 (x86 veya x64) bir bilgisayar çalıştırıyorsanız bu yönergeler varsayar. Farklı bir Ubuntu sürümüne veya farklı bir Linux dağıtımı, ortamınız için bu adımları uyarlayabilirsiniz.

Ardından, [SDK'sını indirin](https://aka.ms/csspeech/linuxbinary) ve tercih ettiğiniz bir dizine dosyalarında paket. Bu tabloda SDK klasörü yapısı gösterilmektedir.

|Yol|Açıklama|
|-|-|
|`license.md`|Lisans|
|`third-party-notices.md`|Üçüncü taraf bildirimleri|
|`include`|C ve C++ üstbilgi dosyaları|
|`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
|`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

Bir uygulama oluşturmak için kopyalayın veya gerekli ikili dosyaların (ve kitaplıkları) geliştirme ortamınıza taşıyın ve bunları, derleme sürecinizle gerektiği gibi ekleyin.

## <a name="get-the-java-sdk"></a>Alma Java SDK'sı

Android için Java SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkların yanı sıra kullanım Android gerekli izinleri içerir.
Maven deponun barındırılan `https://csspeechstorage.blob.core.windows.net/maven/` paketi olarak `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`.

* Android Studio projenize ekleyin

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:0.5.0'
  ```

  içinde `dependencies` , Modül düzeyinde bölümünü `build.gradle` dosya.

Java SDK'sını da olan parçası [konuşma cihaz SDK'sı](speech-devices-sdk.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
