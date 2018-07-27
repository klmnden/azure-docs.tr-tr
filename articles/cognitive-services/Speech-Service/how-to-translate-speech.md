---
title: Konuşma Hizmetleri kullanarak konuşma Çevir
description: Konuşma çevirisi, konuşma tanıma hizmeti kullanmayı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: d539fe5a1a031c196c0d40e989d83575715b278a
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285270"
---
# <a name="translate-speech-using-speech-service"></a>Konuşma hizmeti kullanarak konuşma Çevir

[Speech SDK'sı](speech-sdk.md) konuşma çevirisi, uygulamanızda kullanmak için en basit yoludur. SDK hizmetinin tam işlevsellik sağlar. Konuşma çevirisi gerçekleştirmek için temel işlemi aşağıdaki adımları içerir:

1. Bir konuşma fabrikası oluşturun ve bir konuşma hizmeti abonelik anahtarı sağlayın ve [bölge](regions.md) veya bir yetkilendirme belirteci. Kaynak ve hedef çeviri dilleri bu noktada, metin ve konuşma çıkış isteyip istemediğinizi belirten yanı sıra yapılandırmanız da.

2. Bir tanıyıcı fabrikasından alın. Çeviri, çeviri tanıyıcı seçin. (Diğer tanıyıcıları içindir *Konuşmayı metne dönüştürme*.) Çeşitli kaynak ses kullandığınız temel çeviri tanıyıcı türü vardır.

4. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Geçici ve Nihai sonuç olduğunda tanıyıcı, olay işleyicilerini çağırır. Aksi takdirde, uygulamanızın bir çeviri Nihai sonuç alır.

5. Tanıma ve çeviri başlatın.

# <a name="sdk-samples"></a>SDK örnekleri

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunda](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
