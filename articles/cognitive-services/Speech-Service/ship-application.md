---
title: SDK'sı - konuşma Hizmetleri Konuşma ile uygulama geliştirme
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'yı kullanarak uygulamaları oluşturmayı öğrenin.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: f3522f065d22ce276174fbd165c37df3914e32b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61456181"
---
# <a name="ship-an-application"></a>Bir uygulama teslim edin

Gözlemleyin [Speech SDK'sı lisans](https://aka.ms/csspeech/license201809), hem de [üçüncü taraf yazılım bildirimleri](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) Azure Bilişsel hizmetler konuşma SDK dağıttığınızda. Ayrıca, gözden [Microsoft gizlilik bildirimi](https://aka.ms/csspeech/privacy).

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
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli

>[!NOTE]
> Dosya sürümünden 1.3.0 başlayarak `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sevk) artık gerekli değildir. İşlevselliği artık çekirdek SDK'sı tümleşiktir.

## <a name="linux"></a>Linux

Speech SDK'sı şu anda Ubuntu 16.04 ve 18.04 dağıtımları destekler.
Yerel bir uygulama için Speech SDK'sı kitaplığı göndermeye gerek `libMicrosoft.CognitiveServices.Speech.core.so`.
Uygulamanızı eşleşen sürümünü (x86, x64) seçtiğinizden emin olun. Linux sürümüne göre aşağıdaki bağımlılıkları içerecek şekilde gerekebilir:

* GNU C Kitaplığı'nın paylaşılan kitaplıklar (iş parçacıkları POSIX programlama kitaplığı dahil olmak üzere `libpthreads`)
* OpenSSL kitaplığını (`libssl.so.1.0.0`)
* ALSA uygulamalar için paylaşılan kitaplığı (`libasound.so.2`)

Ubuntu'da GNU C kitaplıklarını zaten varsayılan olarak yüklü olması gerekir. Son üç şu komutları kullanarak yüklenebilir:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2 wget
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
