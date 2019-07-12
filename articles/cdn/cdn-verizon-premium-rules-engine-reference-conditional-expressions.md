---
title: Premium Verizon'dan Azure CDN kural altyapısı koşullu ifadeleri | Microsoft Docs
description: Azure CDN from Verizon Premium için başvuru belgeleri altyapısı eşleştirme koşulları ve özellikleri kuralları.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: f790e37ae876c0640d55ebfb51abb43c6a705f04
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593217"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>Azure CDN Verizon Premium kural altyapısı koşullu ifadeleri

Bu makalede ayrıntılı açıklamaları Azure Content Delivery Network (CDN) için bir koşullu ifadenin listeler [kurallar altyapısı](cdn-verizon-premium-rules-engine.md).

İlk kural koşullu ifade parçasıdır.

Koşullu ifade | Açıklama
-----------------------|-------------
EĞER | Eğer ifade her zaman bir kural içindeki ilk deyimde parçasıdır. Tüm diğer koşullu ifadeler gibi bu IF deyimi bir eşleşme ile ilişkilendirilmesi gerekir. Hiçbir ek koşullu ifadeler tanımlanmışsa, bu eşleşen bir istek için bir özellik kümesi uygulanabilir önce karşılanması gereken kriterleri belirler.
VE | Bir ve eğer ifade yalnızca koşullu ifadeler: IF, ve eğer aşağıdaki türden sonra eklenebilir. Bu, ilk IF deyimi için karşılanması gereken başka bir koşul olduğunu belirtir.
IF ELSE| IF ELSE deyim özellikleri bu ELSE IF deyimi için belirli bir dizi gerçekleşmeden önce karşılanması gereken alternatif bir koşulu belirtir. ELSE IF deyimi varlığını önceki deyimin sonunu gösterir. ELSE IF deyimi başka bir ELSE IF deyimi sonra yerleştirilmiş olabilir yalnızca koşullu ifade. Bu, başka bir IF deyimi karşılanması tek bir ek koşulu belirtmek için yalnızca kullanılabilir anlamına gelir.

**Örnek**: ![CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Bir sonraki kural, bir önceki kuralı tarafından belirtilen eylemleri geçersiz kılabilir.
   > Örnek: Bir genel kural, tüm istekleri belirteç tabanlı kimlik doğrulaması güvenliğini sağlar. Doğrudan, bir özel durum, belirli türde istekler yapmak için başka bir kural oluşturulabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kural altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-verizon-premium-rules-engine.md)