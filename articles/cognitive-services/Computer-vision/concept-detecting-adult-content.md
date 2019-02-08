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
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 64db05e5e40b76d219ea0e3214c20297f32da4b5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861281"
---
# <a name="detecting-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içerikleri algılama

Çeşitli görsel kategoriler arasında yetişkinlere yönelik ve müstehcen içerik grubu, yetişkinlere yönelik malzemeleri algılamaya olanak tanır ve cinsel içerikli görüntülerin gösterilmesini kısıtlar. Yetişkinlere yönelik veya müstehcen içeriklerin algılanmasına yönelik filtre, kullanıcının tercihleri doğrultusunda bir kaydırıcı ölçeği üzerinde ayarlanabilir.

## <a name="defining-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içeriğin tanımlama

Tarafından kapsanan çeşitli görsel özellikleri arasında [analiz görüntü yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), yetişkinlere yönelik görsel özellik yetişkinlere yönelik ve müstehcen resim olanak tanır. "Yetişkinlere yönelik" görüntüleri doğası gereği pornografik olan ve çıplaklık ve cinsel eylem genellikle tarif bu tanımlanır. "Müstehcen" görüntülerini cinsel doğası gereği anlatan ve genellikle daha az "Yetişkin" etiketlenmiş görüntülerini cinsel içerikli içerik içeren bir görüntü olarak tanımlanır Yetişkinlere yönelik görsel özellik türü, ekran görüntülerini cinsel müstehcen içeren ve açıkça cinsel içerik kısıtlamak için yaygın olarak kullanılır.

## <a name="identifying-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içerikleri belirleme

İki özellik, analiz görüntüsü yöntemini döndürür `isAdultContent` ve `isRacyContent`, JSON yanıtındaki yöntemin sırasıyla yetişkinlere yönelik ve müstehcen içeriğin denetimi gösterir. Her iki özellik, bir Boole değeri true veya false döndürür. Yöntemi iki özellik de döndürür `adultScore` ve `racyScore`, temsil, sırasıyla, yetişkinlere yönelik ve müstehcen içeriğin denetimi tanımlamak için güven puanları. Yetişkinlere yönelik ve müstehcen içerik algılaması için bir güven filtre, belirli bir senaryoya göre tercihinizi uyum sağlamak için bir kaydırıcı ölçeğini üzerinde ayarlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [etki alanına özgü içerik algılama](concept-detecting-domain-content.md) ve [yüzleri algılama](concept-detecting-faces.md).
