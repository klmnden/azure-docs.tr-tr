---
title: Yetişkinlere yönelik ve müstehcen içeriğin - görüntü işleme açıklayan
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak resimlerdeki yetişkinlere yönelik ve müstehcen içerikleri algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 71866149e3d2dca4b39585ce8da73aae658a4d59
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344940"
---
# <a name="detecting-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içerikleri algılama

Çeşitli visual kategorileri yetişkinlere yönelik malzemeleri olanak tanır ve görüntülerini cinsel içerik içeren görüntülenmesini sınırlar yetişkinlere yönelik ve müstehcen, grubudur. Yetişkinlere yönelik ve müstehcen içerik algılama için filtre kullanıcı tercihi uyum sağlamak için bir kaydırıcı ölçeğini üzerinde ayarlanabilir.

## <a name="defining-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içeriğin tanımlama

Tarafından kapsanan çeşitli görsel özellikleri arasında [analiz görüntü yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), yetişkinlere yönelik görsel özellik yetişkinlere yönelik ve müstehcen resim olanak tanır. "Yetişkinlere yönelik" görüntüleri doğası gereği pornografik olan ve çıplaklık ve cinsel eylem genellikle tarif bu tanımlanır. "Müstehcen" görüntülerini cinsel doğası gereği anlatan ve genellikle daha az "Yetişkin" etiketlenmiş görüntülerini cinsel içerikli içerik içeren bir görüntü olarak tanımlanır Yetişkinlere yönelik görsel özellik türü, ekran görüntülerini cinsel müstehcen içeren ve açıkça cinsel içerik kısıtlamak için yaygın olarak kullanılır.

## <a name="identifying-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içerikleri belirleme

İki özellik, analiz görüntüsü yöntemini döndürür `isAdultContent` ve `isRacyContent`, JSON yanıtındaki yöntemin sırasıyla yetişkinlere yönelik ve müstehcen içeriğin denetimi gösterir. Her iki özellik, bir Boole değeri true veya false döndürür. Yöntemi iki özellik de döndürür `adultScore` ve `racyScore`, temsil, sırasıyla, yetişkinlere yönelik ve müstehcen içeriğin denetimi tanımlamak için güven puanları. Yetişkinlere yönelik ve müstehcen içerik algılaması için bir güven filtre, belirli bir senaryoya göre tercihinizi uyum sağlamak için bir kaydırıcı ölçeğini üzerinde ayarlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [etki alanına özgü içerik algılama](concept-detecting-domain-content.md) ve [yüzleri algılama](concept-detecting-faces.md).