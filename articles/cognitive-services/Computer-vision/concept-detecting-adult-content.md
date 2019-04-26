---
title: Yetişkinlere yönelik ve müstehcen içeriğin - görüntü işleme açıklayın
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak resimlerdeki yetişkinlere yönelik ve müstehcen içerikleri algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 69a4c136e9c210dd40e004b8d5e1c1a2a8fceaa7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60368365"
---
# <a name="detect-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içerikleri algılama

Böylece geliştiriciler kendi yazılım görüntüleri görüntülenmesini kısıtlayabilirsiniz görüntü işleme görüntülerde yetişkinlere yönelik malzeme algılayabilir. Böylece geliştiriciler kendi tercihleri göre sonuçları çevirebilir içerik bayrakları arasında sıfır ve tek bir puanı ile uygulanır. 

> [!NOTE]
> Bu özellik ayrıca tarafından sunulan [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) hizmeti. Bu alternatif çözümler için metin denetimi ve insan tarafından inceleme iş akışları gibi daha ayrıntılı içerik denetleme senaryolarına bakın.

## <a name="content-flag-definitions"></a>İçeriği bayrağı tanımları

**Yetişkin** görüntüleri hangi doğası ve genellikle pornografik çıplaklık ve cinsel eylem tarif olarak tanımlanır. 

**Müstehcen** görüntüleri doğası ve genellikle cinsel müstehcen resimleri görüntüleri olarak etiketlenmiş cinsel içerikli daha az içerik olarak tanımlanmış **yetişkinlere yönelik**. 

## <a name="identify-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içeriği tanımlayın

[Analiz](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API.

İki boolean özelliği, analiz görüntüsü yöntemini döndürür `isAdultContent` ve `isRacyContent`, JSON yanıtındaki yöntemin yetişkinlere yönelik ve müstehcen içerik sırasıyla olduğunu gösterir. Yöntemi iki özellik de döndürür `adultScore` ve `racyScore`, yetişkinlere yönelik ve müstehcen içeriğin sırasıyla tanımlamak için güven puanları temsil eder.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [etki alanına özgü içerik algılama](concept-detecting-domain-content.md) ve [yüzleri algılama](concept-detecting-faces.md).
