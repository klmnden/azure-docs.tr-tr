---
title: Azure CDN kuralları altyapısı koşullu ifadeler | Microsoft Docs
description: Azure CDN başvuru belgelerine altyapısı eşleşme koşulları ve özellikleri kuralları.
services: cdn
documentationcenter: ''
author: Lichard
manager: akucer
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 57e56c38e003cb83dcf44f455c4451d159db8a59
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23842933"
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Azure CDN kuralları koşullu ifadeler altyapısı
Bu konu ayrıntılı açıklamaları koşullu ifadeler Azure içerik teslim ağı (CDN) için listeler [kurallar altyapısı](cdn-rules-engine.md).

İlk kural koşullu ifade parçasıdır.

Koşullu ifade | Açıklama
-----------------------|-------------
EĞER | Bir IF ifadesi her zaman bir kural ilk deyiminde parçasıdır. Tüm diğer koşullu ifadeler gibi bu IF deyimi bir eşleşme ile ilişkilendirilmiş olması gerekir. Hiçbir ek koşullu ifadeler tanımlanmışsa, bu eşleşen bir özellik kümesi için bir istek uygulanabilir önce karşılanması gereken ölçütü belirler.
VE | VE IF ifadesi yalnızca koşullu ifadeler: IF, ve eğer aşağıdaki türden sonra eklenebilir. Bu, ilk IF deyimi için karşılanması gereken başka bir koşul olduğunu belirtir.
ELSE IF| ELSE IF ifadesi bu ELSE IF deyimi belirli özellikler kümesi gerçekleşmeden önce karşılanması gereken alternatif bir koşulu belirtir. ELSE IF deyimi varlığını önceki deyimin sonuna gösterir. ELSE IF deyimi başka bir ELSE IF deyimi sonra yerleştirilebilir yalnızca koşullu ifade. Başka bir deyişle, bir ELSE IF deyimi yalnızca karşılanması gereken tek bir ek koşulu belirtmek için kullanılan.

**Örnek**: ![CDN eşleşen koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Bir sonraki kural önceki bir kural tarafından belirtilen eylemleri geçersiz kılabilir. Örnek: Bir catch tüm kural belirteç tabanlı kimlik doğrulaması aracılığıyla tüm istekleri güvenliğini sağlar. Başka bir kural doğrudan, bir özel durum istek türlerini belirli yapma oluşturulabilir.

### <a name="next-steps"></a>Sonraki adımlar
* [Azure CDN'ye genel bakış](cdn-overview.md)
* [Kuralları altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kurallar altyapısı eşleşme koşulları](cdn-rules-engine-reference-match-conditions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
