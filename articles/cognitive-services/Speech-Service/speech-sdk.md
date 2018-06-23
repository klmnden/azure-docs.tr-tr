---
title: Bilişsel hizmetler konuşma SDK hakkında | Microsoft Docs
description: SDK'ları konuşma hizmeti için kullanılabilir bir genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: b9b7b8af5ce3d75788fd2c4f5e0309b5ca561a8f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355648"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Bilişsel hizmetler konuşma SDK hakkında

Bilişsel hizmetler konuşma Yazılım Geliştirme Seti (SDK) yazılım geliştirme kolaylaştırma uygulamalarınızı konuşma hizmeti işlevleri yerel erişim sağlar. Şu anda, SDK erişim sağlayan **metin konuşma**, **konuşma çeviri**, ve **hedefi tanıma**.

Tablo, işletim sistemleri ve desteklenen programlama dilleri listeler.

|Desteklenen işletim sistemi|Programlama dili|
|-|-|
|Windows|C/C++, C#|
|Linux|C/C++|
|Cihazlar|Java\*|

\* *Java SDK'sı parçası olan [konuşma aygıtları SDK](speech-devices-sdk.md).*

## <a name="get-the-windows-sdk"></a>Windows SDK'sı Al

Windows sürümü konuşma SDK'sı, C# ile kullanmak için yönetilen (.NET) kitaplıkları yanı sıra 32 bit ve 64-bit C/C++ istemci kitaplıklarını içerir. SDK'sı NuGet kullanarak Visual Studio'da yüklenebilir; Basit Arama `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Linux SDK'sı Al

Gerekli derleyici ve kitaplıkları aşağıdaki kabuk komutlarını çalıştırarak olduğundan emin olun:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Bu yönergeler, Ubuntu 16.04 (x86 veya x64) bir bilgisayar çalıştırıyorsanız varsayalım. Farklı bir Ubuntu sürüm veya başka bir Linux dağıtım, adımları ortamınıza uyarlayabilirsiniz.

Ardından, [SDK'sını indirin](https://aka.ms/csspeech/linuxbinary) ve tercih ettiğiniz bir dizine dosyalarında paketini açın. Bu tablo SDK klasör yapısını gösterir.

|Yol|Açıklama|
|-|-|
|`license.md`|Lisans|
|`third-party-notices.md`|Üçüncü taraf bildirimleri|
|`include`|C ve C++ üstbilgi dosyaları|
|`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
|`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

Bir uygulama oluşturmak için kopyalama veya gerekli ikili dosyaları (ve kitaplıklar) geliştirme ortamınıza gider ve bunları yapı işlemine gerektiği gibi ekler.

## <a name="get-the-java-sdk"></a>Java al SDK

Java SDK'sı parçası olan [konuşma aygıtları SDK](speech-devices-sdk.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
