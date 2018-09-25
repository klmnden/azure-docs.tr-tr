---
title: Azure Bilişsel hizmetler, Bilişsel hizmetler konuşma SDK API'si belgeleri - öğreticiler, API Başvurusu
description: Oluşturma ve Bilişsel hizmetler konuşma SDK'sı ile uygulama geliştirme hakkında bilgi edinin
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: b920b7bcb86d94b2948ed0a9bf783cee1282b33f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041084"
---
# <a name="ship-an-application"></a>Bir uygulama teslim edin

Gözlemleyin [Speech SDK'sı lisans](license.md), hem de [üçüncü taraf yazılım bildirimleri](third-party-notices.md) Azure Bilişsel hizmetler konuşma SDK dağıttığınızda. Ayrıca, gözden [Microsoft gizlilik bildirimi](https://aka.ms/csspeech/privacy).

Platforma bağlı olarak, uygulamanızı çalıştırmak için farklı bağımlılıklara mevcut.

## <a name="windows"></a>Windows

Bilişsel hizmetler konuşma SDK'sı, Windows 10 ve Windows Server 2016 üzerinde test edilir.

Bilişsel hizmetler konuşma SDK'sı gerektirir [Microsoft Visual C++ yeniden dağıtılabilir için Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) sistem üzerinde. En son sürümü için yükleyicileri indirebileceğiniz `Microsoft Visual C++ Redistributable for Visual Studio 2017` burada:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Uygulamanız, yönetilen kod kullanıyorsa `.NET Framework 4.6.1` veya daha sonra hedef makinede gereklidir.

Mikrofon girişi için Media Foundation kitaplıkları yüklü olması gerekir. Bu kitaplıklar, Windows 10 ve Windows Server 2016'ya bir parçasıdır. Mikrofon ses giriş cihazını kullanılmayan sürece bu kitaplıklar Speech SDK'sı kullanmak mümkündür.

Gerekli dosyaları Speech SDK'sı, uygulamanızın aynı dizinde dağıtılabilir. Bu şekilde uygulamanızı kitaplıkları doğrudan erişebilirsiniz. Uygulamanızı eşleşen doğru sürümünü (Win32/x64) seçtiğinizden emin olun.

| Ad | İşlev
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Yerel ve yönetilen dağıtım için gerekli core SDK'sı
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | Yönetilen dağıtım için gerekli
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli

## <a name="linux"></a>Linux

Yerel bir uygulama için Speech SDK'sı kitaplığı göndermeye gerek `libMicrosoft.CognitiveServices.Speech.core.so`.
Uygulamanızı eşleşen sürümünü (x86, x64) seçtiğinizden emin olun. Linux sürümüne göre aşağıdaki bağımlılıkları içerecek şekilde gerekebilir:

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
