---
title: Konuşma hizmetlerini kullanarak konuşma Çevir | Microsoft Docs
description: Konuşma çeviri konuşma hizmetinde kullanmayı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 7f39f284998489574049d82c44b3d3a0a3797adb
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356285"
---
# <a name="translate-speech-using-speech-service"></a>Konuşma hizmetini kullanarak konuşma Çevir

[Konuşma SDK](speech-sdk.md) konuşma çevirisi, uygulamanızda kullanmak için en basit yolu. SDK hizmetinin tam işlevsellik sağlar. Konuşma çeviri gerçekleştirmek için temel işlem aşağıdaki adımları içerir:

1. Bir konuşma fabrikası oluşturun ve bir konuşma hizmeti abonelik anahtar veya bir yetki belirteci sağlayın. Kaynak ve hedef çeviri dillerini bu noktada, metin veya konuşma çıktı isteyip istemediğinizi belirtme yanı sıra yapılandırmanız da.

2. Bir tanıyıcı fabrikası'ndan alın. Çeviri için çeviri tanıyıcı seçin. (Diğer tanıyıcıları içindir *metin konuşma*.) Kullanmakta olduğunuz ses kaynağına göre çeviri tanıyıcı çeşitli özellikleri vardır.

4. Zaman uyumsuz işlemi için olayları isterseniz bağlayın. Ara ve nihai sonuçları sahip olduğunda, olay işleyicileri tanıyıcı çağırır. Aksi durumda, uygulamanızın son çeviri sonucu alır.

5. Tanıma ve çeviri başlatın.

# <a name="sdk-samples"></a>SDK örnekleri

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# Konuşma tanıması](quickstart-csharp-windows.md)
