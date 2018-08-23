---
title: Bilişsel hizmetler konuşma SDK hakkında
description: Konuşma hizmeti için kullanılabilir SDK'lar genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 08/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 6b5796bf4d049579dbdede2251f2ca67cc9c4bfd
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41987475"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Bilişsel hizmetler konuşma SDK hakkında

Bilişsel hizmetler konuşma Yazılım Geliştirme Seti (SDK), uygulamalarınızın yazılım geliştirmeyi kolaylaştıran konuşma hizmeti işlevlerini yerel erişim sağlar. Şu anda Erişim SDK'sı sağlar **Konuşmayı metne dönüştürme**, **konuşma çevirisi**, ve **amaç tanıma**.

[!include[Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK'yı edinme

### <a name="windows"></a>Windows

Windows için biz aşağıdaki dilleri destekler:

* C# (UWP ve .NET), C++: başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın.
  Paket yönetilen (.NET) kitaplıklarının yanı sıra 32 bit ve 64 bit istemci kitaplıkları içerir.
  SDK'sı NuGet kullanarak Visual Studio'da yüklenebilir; Arama `Microsoft.CognitiveServices.Speech`.

* Java: Başvurabilir ve yalnızca Windows x64 destekleyen bizim konuşma SDK Maven paketini en son sürümünü kullanın.
  Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0` bağımlılık olarak. 

### <a name="linux"></a>Linux

> [!NOTE]
> Şu anda yalnızca Ubuntu 16.04 PC'de (x86 veya x64 C++ geliştirme için .NET Core ve Java için x64) destekliyoruz.

Gerekli derleyici ve kitaplıkları, aşağıdaki Kabuk komutları çalıştırarak yüklü olduğundan emin olun:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C# ' ta: Başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın.
  SDK'ya başvurmak için şu paket başvuruyu projenize ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="0.6.0" />
  ```

* Java: Başvurabilir ve bizim konuşma SDK Maven paketini en son sürümünü kullanın.
  Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0` bağımlılık olarak. 

* C++: SDK'yı indirin bir [.tar paketinin](https://aka.ms/csspeech/linuxbinary) ve tercih ettiğiniz bir dizine dosyalarında paket. Aşağıdaki tabloda, SDK klasörü yapısı gösterilmektedir.

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve C++ üstbilgi dosyaları|
  |`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
  |`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

  Bir uygulama oluşturmak için kopyalayın veya gerekli ikili dosyaların (ve kitaplıkları) geliştirme ortamınıza taşıyın ve bunları, derleme sürecinizle gerektiği gibi ekleyin.

### <a name="android"></a>Android

Android için Java SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkların yanı sıra kullanım Android gerekli izinleri içerir.
Maven deponun barındırılan `https://csspeechstorage.blob.core.windows.net/maven/` paketi olarak `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0`.
Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

* Proje düzeyi `build.gradle` dosyasında, aşağıdaki ekleyin `repository` bölümü:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyi `build.gradle` dosyasında, aşağıdaki ekleyin `dependencies` bölümü:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:0.6.0'
  ```

Java SDK'sını da olan parçası [konuşma cihaz SDK'sı](speech-devices-sdk.md).

[!include[Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
