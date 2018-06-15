---
title: Azure CDN kuralları altyapısı başvurusu | Microsoft Docs
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
ms.openlocfilehash: 602b4303dd1940791c11b8b71ac6a27f0474a6d5
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29733688"
---
# <a name="azure-cdn-rules-engine-reference"></a>Azure CDN kuralları başvuru altyapısı
Bu makalede ayrıntılı açıklamaları için Azure içerik teslim ağı (CDN) kullanılabilir eşleşme koşulları ve özellikleri listelenmektedir [kurallar altyapısı](cdn-rules-engine.md).

Kurallar altyapısı nasıl belirli türde istekler son yetkilisinde CDN tarafından işlenen olacak şekilde tasarlanmıştır.

**Ortak kullanımlar**:

- Geçersiz kılma veya özel önbellek ilkesi tanımlayın.
- Güvenli veya hassas içerik isteklerini reddedin.
- Yeniden yönlendirme istekleri.
- Özel günlük verilerini depolar.

## <a name="terminology"></a>Terminoloji
Bir kural kullanılarak tanımlanan [ **koşullu ifadeler**](cdn-rules-engine-reference-conditional-expressions.md), [ **eşleşen koşullar**](cdn-rules-engine-reference-match-conditions.md), ve [ **özellikleri**](cdn-rules-engine-reference-features.md). Bu öğeleri aşağıdaki şekilde vurgulanmıştır:

 ![CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sözdizimi

Özel karakterler kabul edilir şekilde nasıl bir eşleşme koşul veya özellik metin değerlerini işleme göre değişir. Bir eşleşme koşul veya özellik metin aşağıdaki yollardan biriyle yorumlayabilir:

1. [**Değişmez değerler**](#literal-values) 
2. [**Joker karakter değerleri**](#wildcard-values)
3. [**Normal ifadeler**](#regular-expressions)

### <a name="literal-values"></a>Değişmez değerler
Değişmez değer yorumlanır metin % simgenin hariç olmak üzere tüm özel karakterleri eşleşmesi gereken değerinin bir parçası değerlendirir. Diğer bir deyişle, bir hazır değer kümesine koşul eşleşen `\'*'\` değeri tam olduğunda yalnızca sağlanıyorsa (diğer bir deyişle, `\'*'\`) bulunur.
 
Bir yüzde simge URL kodlaması göstermek için kullanılır (örneğin, `%20`).

### <a name="wildcard-values"></a>Joker karakter değerleri
Bir joker karakter değeri olarak yorumlanır metin ek anlam özel karakterleri atar. Aşağıdaki tabloda şu karakter kümesini nasıl yorumlanacağını açıklanmaktadır:

Karakter | Açıklama
----------|------------
\ | Ters eğik çizgi bu tabloda belirtilen karakterlerden herhangi birini kaçınmak için kullanılır. Ters eğik çizgi kaçış doğrudan özel karakter önce belirtilmiş olması gerekir.<br/>Örneğin, aşağıdaki söz dizimini yıldız çıkışları: `\*`
% | Bir yüzde simge URL kodlaması göstermek için kullanılır (örneğin, `%20`).
* | Bir yıldız işareti bir veya daha fazla karakter temsil eden bir joker karakterdir.
Boşluk | Bir boşluk karakteri bir eşleşme koşul ya da belirtilen değerler veya desenleri tarafından karşılanması olduğunu gösterir.
'value' | Tek bir tırnakla özel bir anlamı yoktur. Ancak, tek tırnak kümesi, bir değer değişmez değer ele alınması gerektiğini belirtmek için kullanılır. Aşağıdaki şekillerde kullanılabilir:<br><br/>-Bu bir eşleşme koşul herhangi bir kısmının karşılaştırma değeri belirtilen değerle eşleşen her memnun olmasını sağlar.  Örneğin, `'ma'` aşağıdaki dizelerden herhangi birinde eşleşir: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/ İş/şablonu. **ma**p<br /><br />-Bu harf karakter olarak belirtilmesi için bir özel karakter sağlar. Örneğin, bir dizi tek tırnak içinde bir boşluk karakteri kapsayan tarafından bir değişmez değer boşluk karakteri belirtmiş olabilir (diğer bir deyişle, `' '` veya `'sample value'`).<br/>-Belirtilmesi boş bir değer verir. Tek tırnak kümesi belirterek boş bir değer belirtin (diğer bir deyişle, '').<br /><br/>**Önemli:**<br/>-Belirtilen değer bir joker karakter içermiyorsa, sonra onu otomatik olarak bir dizi tek tırnak belirtmeye gerek olmadığı anlamına gelir bir sabit değer olarak kabul edilir.<br/>-Bir ters eğik çizgi bu tablodaki başka bir karakteri kaçış değil, tek tırnak kümesinde belirtildiğinde yoksayılır.<br/>-Bir özel karakter bir harf karakter ters eğik çizgi kullanarak çıkış olarak belirtmek için başka bir yol (diğer bir deyişle, `\`).

### <a name="regular-expressions"></a>Normal ifadeler

Normal ifadeler için bir metin değerini, içinde arama deseni tanımlayın. Normal ifade gösterimi belirli anlamları çeşitli simgeler tanımlar. Aşağıdaki tabloda nasıl özel karakterleri belirten eşleşme koşullar ve normal ifadeler destek özellikleri tarafından kabul edilir.

Özel karakter | Açıklama
------------------|------------
\ | Ters eğik çizgi karakteri aşağıdaki çıkışları normal ifade anlamlarını almak yerine bir hazır değer olarak kabul edilmesi bu karakteri neden olan BT. Örneğin, aşağıdaki söz dizimini yıldız çıkışları: `\*`
% | Bir yüzde simge anlamı üzerinde kullanım bağlıdır.<br/><br/> `%{HTTPVariable}`: Bu sözdizimi bir HTTP değişkeni tanımlar.<br/>`%{HTTPVariable%Pattern}`: Bu sözdizimi bir yüzde simge bir HTTP değişken ve ayırıcı olarak tanımlamak için kullanır.<br />`\%`: Bir yüzde simge kaçış sağlar URL kodlaması belirtmek için veya bir hazır değer kullanılmak üzere (örneğin, `\%20`).
* | Bir yıldız işareti sıfır veya daha fazla kez eşleştirilmesini önceki karakteri sağlar. 
Boşluk | Bir boşluk karakteri genellikle harf karakter olarak kabul edilir. 
'value' | Tek tırnak değişmez değer karakter olarak kabul edilir. Tek tırnak birtakım özel bir anlamı yok.


## <a name="next-steps"></a>Sonraki adımlar
* [Kurallar altyapısı eşleşme koşulları](cdn-rules-engine-reference-match-conditions.md)
* [Kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-conditional-expressions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
* [Azure CDN'ye genel bakış](cdn-overview.md)
