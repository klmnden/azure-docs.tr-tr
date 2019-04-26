---
title: Azure CDN kural altyapısı koşullu ifadeleri | Microsoft Docs
description: Azure CDN için başvuru belgeleri altyapısı eşleştirme koşulları ve özellikleri kuralları.
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
ms.openlocfilehash: 73c41b754c0aca5ddb1a49fcd2794aa41b2fa705
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324201"
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Azure CDN kural altyapısı koşullu ifadeleri
Bu konu Azure Content Delivery Network (CDN) için ayrıntılı açıklamaları koşullu ifadeler listeler [kurallar altyapısı](cdn-rules-engine.md).

İlk kural koşullu ifade parçasıdır.

Koşullu ifade | Açıklama
-----------------------|-------------
EĞER | Eğer ifade her zaman bir kural içindeki ilk deyimde parçasıdır. Tüm diğer koşullu ifadeler gibi bu IF deyimi bir eşleşme ile ilişkilendirilmesi gerekir. Hiçbir ek koşullu ifadeler tanımlanmışsa, bu eşleşen bir istek için bir özellik kümesi uygulanabilir önce karşılanması gereken kriterleri belirler.
VE | Bir ve eğer ifade yalnızca koşullu ifadeler: IF, ve eğer aşağıdaki türden sonra eklenebilir. Bu, ilk IF deyimi için karşılanması gereken başka bir koşul olduğunu belirtir.
IF ELSE| IF ELSE deyim özellikleri bu ELSE IF deyimi için belirli bir dizi gerçekleşmeden önce karşılanması gereken alternatif bir koşulu belirtir. ELSE IF deyimi varlığını önceki deyimin sonunu gösterir. ELSE IF deyimi başka bir ELSE IF deyimi sonra yerleştirilmiş olabilir yalnızca koşullu ifade. Bu, başka bir IF deyimi karşılanması tek bir ek koşulu belirtmek için yalnızca kullanılabilir anlamına gelir.

**Örnek**: ![CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Bir sonraki kural, bir önceki kuralı tarafından belirtilen eylemleri geçersiz kılabilir. Örnek: Bir genel kural, tüm istekleri belirteç tabanlı kimlik doğrulaması güvenliğini sağlar. Doğrudan, bir özel durum, belirli türde istekler yapmak için başka bir kural oluşturulabilir.

### <a name="next-steps"></a>Sonraki adımlar
* [Azure CDN'ye genel bakış](cdn-overview.md)
* [Kural altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kural altyapısı eşleştirme koşulları](cdn-rules-engine-reference-match-conditions.md)
* [Kural altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kural altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
