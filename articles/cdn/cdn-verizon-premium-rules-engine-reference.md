---
title: Azure CDN kural altyapısı başvurusu | Microsoft Docs
description: Azure CDN için başvuru belgeleri altyapısı eşleştirme koşulları ve özellikleri kuralları.
services: cdn
author: mdgattuso
ms.service: cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: a04fcd3eaaed5c3e43f631ad1fbb6fed93ea29fb
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481692"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Azure CDN Verizon Premium kural altyapısı başvurusu

Bu makalede ayrıntılı açıklamaları için Azure Content Delivery Network (CDN) kullanılabilir eşleştirme koşulları ve özellikleri listelenmektedir [kurallar altyapısı](cdn-verizon-premium-rules-engine.md).

Kural altyapısı, belirli türde istekler son yetkilisinde CDN tarafından işlenen olacak şekilde tasarlanmıştır.

**Yaygın kullanımları**:

- Geçersiz kılabilir veya özel önbellek ilkesini tanımlayın.
- Güvenli veya hassas içerik isteklerini reddedin.
- Yeniden yönlendirme istekleri.
- Özel günlük verilerini Store.

## <a name="terminology"></a>Terminoloji

Bir kural tarafından tanımlanan [ **koşullu ifadeler**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [ **koşullara uyan**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), ve [ **özellikleri**](cdn-verizon-premium-rules-engine-reference-features.md). Bu öğeleri aşağıdaki şekilde vurgulanmıştır:

 ![CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sözdizimi

İçinde özel karakterler kabul edilir bir şekilde nasıl bir eşleşme koşulu veya özellik metin değerlerini işler göre değişir. Bir eşleşme koşulu veya özellik metin aşağıdaki yollardan biriyle yorumlayabilir:

1. [**Değişmez değerler**](#literal-values)
2. [**Değerleri joker karakter**](#wildcard-values)
3. [**Normal ifadeler**](#regular-expressions)

### <a name="literal-values"></a>Değişmez değerler

Değişmez değer yorumlanır metin % sembol hariç tüm özel karakterler eşlenmesi gereken değer bir parçası olarak değerlendirir. Diğer bir deyişle, koşul kümesi eşleşen bir sabit değer `\'*'\` değer, tam zaman yalnızca uyulmuş olur (diğer bir deyişle, `\'*'\`) bulunur.

URL kodlaması belirtmek için kullanılan bir yüzde sembolü (örneğin, `%20`).

### <a name="wildcard-values"></a>Değerleri joker karakter

Joker karakter değer olarak yorumlanır metin için özel karakterler ek anlamlar atar. Aşağıdaki tabloda, şu karakter kümesini nasıl yorumlanacağını açıklanmaktadır:

Karakter | Açıklama
----------|------------
\ | Bu tabloda belirtilen karakterlerden herhangi birini kaçış ters eğik çizgi kullanılır. Ters eğik çizgi kaçış doğrudan özel karakter önce belirtilmelidir.<br/>Örneğin, aşağıdaki söz dizimini yıldız çıkışları: `\*`
% | URL kodlaması belirtmek için kullanılan bir yüzde sembolü (örneğin, `%20`).
\* | Bir yıldız işareti, bir veya daha fazla karakterleri temsil eden bir joker karakterdir.
Uzay | Bir boşluk karakteri bir eşleşme koşulu ya da belirtilen değerleri veya desenleri tarafından karşılanması olduğunu gösterir.
'value' | Tek tırnak, özel bir anlamı yok. Ancak, tek tırnak bir dizi, değer değişmez değer değerlendirilmesi gerektiğini belirtmek için kullanılır. Aşağıdaki şekillerde kullanılabilir:<br><br/>-Bir eşleşme koşulu herhangi bir bölümünü karşılaştırma değeri belirtilen değerle eşleşen her memnun olmasını sağlar.  Örneğin, `'ma'` aşağıdaki dizelerden birini eşleşir: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/ İş/şablonu. **ma**p<br /><br />-Bu, bir değişmez değer olarak belirtilmesi için özel bir karakter sağlar. Örneğin, bir boşluk karakteri tek tırnak bir dizi kapsayan tarafından bir hazır bilgi boşluk karakteri belirtmek (diğer bir deyişle, `' '` veya `'sample value'`).<br/>-Boş bir değer belirtilmesini sağlar. Tek tırnak bir dizi belirterek boş bir değer belirtin (diğer bir deyişle, '').<br /><br/>**Önemli:**<br/>-Ardından belirtilen değer bir joker karakter içermiyorsa, tek tırnak kümesi belirtmek gerekli değildir, yani bir değişmez değer otomatik olarak değerlendirilir.<br/>-Bir ters eğik çizgi, bu tablodaki başka bir karakter kaçırmaz ise tek tırnak bir dizi içinde belirtildiğinde yoksayılır.<br/>-Değişmez bir karakter kaçış ters eğik çizgi kullanarak olarak özel bir karakter belirtmek için başka bir yol (diğer bir deyişle, `\`).

### <a name="regular-expressions"></a>Normal ifadeler

Normal ifadeler için bir metin değeri içinde aranır bir desen tanımlayın. Normal ifade gösterimi belirli anlamlara çeşitli sembolleri tanımlar. Aşağıdaki tabloda, nasıl özel karakterler gösteren eşleştirme koşulları ve normal ifadeler destekleyen özellikler tarafından kabul edilir.

Özel karakter | Açıklama
------------------|------------
\ | Ters eğik çizgi karakteri aşağıdaki çıkışları BT, normal ifade anlamını almak yerine değişmez değer olarak değerlendirilmesi bu karakteri neden olur. Örneğin, aşağıdaki söz dizimini yıldız çıkışları: `\*`
% | Bir yüzde sembolü anlamını kendi kullanıma bağlıdır.<br/><br/> `%{HTTPVariable}`: Bu sözdizimi, bir HTTP değişkenini tanımlar.<br/>`%{HTTPVariable%Pattern}`: Bu söz dizimi bir yüzde sembolü HTTP değişken ve ayırıcı olarak tanımlamak için kullanır.<br />`\%`: Bir yüzde sembolü kaçış, sağlayan bir değişmez değer kullanılacak ya da URL kodlaması belirtmek için (örneğin, `\%20`).
\* | Sıfır veya daha fazla kez eşleştirilecek önceki karakteri bir yıldız işareti sağlar.
Uzay | Bir boşluk karakteri, genellikle bir değişmez değer olarak kabul edilir.
'value' | Tek tırnak işaretleri, sabit karakter olarak kabul edilir. Tek tırnak birtakım özel bir anlamı yok.

## <a name="next-steps"></a>Sonraki adımlar

- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kural altyapısı kullanarak HTTP davranışı geçersiz kılma](cdn-verizon-premium-rules-engine.md)
- [Azure CDN'ye genel bakış](cdn-overview.md)