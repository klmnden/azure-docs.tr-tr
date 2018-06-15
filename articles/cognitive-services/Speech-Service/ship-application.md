---
title: Azure Bilişsel, Bilişsel Hizmetleri konuşma SDK API belgeleri - öğreticileri, API Başvurusu | Microsoft Docs
description: Oluşturma ve Bilişsel Services konuşma SDK'sı ile uygulamaları geliştirme hakkında bilgi edinin
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: d410dda09fdd30181b633c454b1d44610b10c472
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356237"
---
# <a name="shipping-an-application"></a>Bir uygulama aktarma

Gözlemlemek [konuşma SDK lisans](license.md), yanı sıra [üçüncü taraf yazılım bildirimleri](third-party-notices.md) Bilişsel Services konuşma SDK'sı dağıtırken. Ayrıca, gözden [Microsoft gizlilik bildirimi](https://aka.ms/csspeech/privacy).

Platforma bağlı olarak farklı bağımlılıkları uygulamanızı yürütmek için mevcut.

## <a name="windows"></a>Windows

Bilişsel hizmetler konuşma SDK'sı, Windows 10 ve Windows Server 2016 test edilir.

Bilişsel hizmetler konuşma SDK'sı gerektirir [Microsoft Visual C++ yeniden dağıtılabilir için Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) sistemdeki. En son sürümünü yükleyicilerini indirebilirsiniz `Microsoft Visual C++ Redistributable for Visual Studio 2017` burada:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Yönetilen kod, uygulamanızın kullanıyorsanız `.Net Framework 4.6.1` veya daha sonra hedef makinede gerekli.

Mikrofon girişi için Media Foundation kitaplıkları yüklü olması gerekir. Bu kitaplıklar, Windows 10 ve Windows Server 2016 bir parçasıdır. Mikrofonun ses giriş aygıtı olarak kullanılmaz sürece bu kitaplıklar konuşma SDK'yı kullanmak da mümkündür.

Gerekli konuşma SDK dosyaları, uygulamanız ile aynı dizinde dağıtılabilir. Bu şekilde, uygulamanızın kitaplıkları doğrudan erişebilirsiniz. Uygulamanızı eşleşen doğru sürümünü (Win32/x64) seçtiğinizden emin olun.

| Ad | İşlev
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Çekirdek SDK, yerel ve yönetilen dağıtım için gerekli
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | Yönetilen dağıtım için gerekli
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli

## <a name="linux"></a>Linux

Konuşma SDK'sı kitaplığı sevk gereken yerel bir uygulama için `libMicrosoft.CognitiveServices.Speech.core.so`.
Sürüm (x86, x64) seçtiğinizden emin olun, uygulamanızın eşleşen. Linux sürümüne bağlı olarak, aşağıdaki bağımlılıkları içerecek şekilde gerekebilir:

* Paylaşılan kitaplıklar GNU C Kitaplığı (iş parçacıkları POSIX programlama kitaplığı ile birlikte `libpthreads`)
* OpenSSL kitaplığı (`libssl.so.1.0.0`)
* CURL kitaplığı (`libcurl.so.4`)
* ALSA uygulamalar için paylaşılan kitaplığı (`libasound.so.2`)

Ubuntu 16.04 üzerinde örneğin, GNU C kitaplıkları zaten varsayılan olarak yüklü olmalıdır. Bu komutları kullanarak son üç yüklenebilir:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
