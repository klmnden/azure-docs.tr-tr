---
title: Azure Bilişsel hizmetler, Bilişsel hizmetler konuşma SDK API'si belgeleri - öğreticiler, API Başvurusu | Microsoft Docs
description: Oluşturma ve Bilişsel hizmetler konuşma SDK'sı ile uygulama geliştirme hakkında bilgi edinin
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: fe171ba9f6f0ff36a7c23c47f145d83f7a94fb5d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069497"
---
# <a name="shipping-an-application"></a>Uygulamaya aktarma

Gözlemleyin [Speech SDK'sı lisans](license.md), hem de [üçüncü taraf yazılım bildirimleri](third-party-notices.md) Bilişsel hizmetler konuşma SDK'sı dağıtırken. Ayrıca, gözden [Microsoft gizlilik bildirimi](https://aka.ms/csspeech/privacy).

Platforma bağlı olarak, uygulamanızı çalıştırmak için farklı bağımlılıklara mevcut.

## <a name="windows"></a>Windows

Bilişsel hizmetler konuşma SDK'sı, Windows 10 ve Windows Server 2016 üzerinde test edilir.

Bilişsel hizmetler konuşma SDK'sı gerektirir [Microsoft Visual C++ yeniden dağıtılabilir için Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) sistem üzerinde. En son sürümü için yükleyicileri indirebileceğiniz `Microsoft Visual C++ Redistributable for Visual Studio 2017` burada:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Yönetilen kod, uygulamanızın kullanıyorsa `.Net Framework 4.6.1` veya daha sonra hedef makinede gereklidir.

Mikrofon girişi için Media Foundation kitaplıkları yüklü olması gerekir. Bu kitaplıklar, Windows 10 ve Windows Server 2016'ya bir parçasıdır. Mikrofon ses giriş cihazını kullanılmaz sürece bu kitaplıklar Speech SDK'sı kullanmak mümkündür.

Gerekli dosyaları Speech SDK'sı, uygulamanızın aynı dizinde dağıtılabilir. Bu şekilde uygulamanızı kitaplıkları doğrudan erişebilirsiniz. Uygulamanızı eşleşen doğru sürüm (Win32/x64) seçtiğinizden emin olun.

| Ad | İşlev
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Yerel ve yönetilen dağıtım için gerekli core SDK'sı
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | Yönetilen dağıtım için gerekli
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli

## <a name="linux"></a>Linux

İhtiyacınız olan Speech SDK'sı kitaplığı göndermeye yerel bir uygulama için `libMicrosoft.CognitiveServices.Speech.core.so`.
Sürüm (x86, x64) seçtiğinizden emin olun, uygulamanızın eşleşen. Linux sürümüne göre aşağıdaki bağımlılıkları içerecek şekilde gerekebilir:

* GNU C Kitaplığı'nın paylaşılan kitaplıklar (iş parçacıkları POSIX programlama kitaplığı dahil olmak üzere `libpthreads`)
* OpenSSL kitaplığını (`libssl.so.1.0.0`)
* CURL kitaplığı (`libcurl.so.4`)
* ALSA uygulamalar için paylaşılan kitaplığı (`libasound.so.2`)

Ubuntu 16.04 üzerinde örneğin, GNU C kitaplıklarını zaten varsayılan olarak yüklü olması gerekir. Son üç şu komutları kullanarak yüklenebilir:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
