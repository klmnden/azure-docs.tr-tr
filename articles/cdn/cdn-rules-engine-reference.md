---
title: "Azure CDN kuralları altyapısı başvurusu | Microsoft Docs"
description: "Azure CDN başvuru belgelerine altyapısı eşleşme koşulları ve özellikleri kuralları."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: c10145661a8c575381493c9aaa901c3ef92c2e81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı
Bu konuda ayrıntılı açıklamaları Azure içerik teslim ağı (CDN) için kullanılabilir eşleşme koşulları ve özellikleri listelenmektedir [kurallar altyapısı](cdn-rules-engine.md).

HTTP kurallar altyapısı nasıl belirli türde istekler son yetkilisinde CDN tarafından işlenen olacak şekilde tasarlanmıştır.

**Ortak kullanımlar**:

- Geçersiz kılma veya özel önbellek ilkesi tanımlayın.
- Güvenli veya hassas içerik isteklerini reddedin.
- Yeniden yönlendirme istekleri.
- Özel günlük verilerini depolar.

## <a name="terminology"></a>Terminoloji
Bir kural kullanılarak tanımlanan [ **koşullu ifadeler**](cdn-rules-engine-reference-conditional-expressions.md), [ **eşleşen**](cdn-rules-engine-reference-match-conditions.md), ve [ **özellikleri**](cdn-rules-engine-reference-features.md). Bu öğeler aşağıdaki çizimde vurgulanır.

 ![CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sözdizimi

Özel karakterleri kabul şekilde nasıl bir eşleşme koşul veya özellik metin değerlerini işleme göre değişir. Bir eşleşme koşul veya özellik metin aşağıdaki yollardan biriyle yorumlayabilir:

1. [**Değişmez değerler**](#literal-values) 
2. [**Joker karakter değerleri**](#wildcard-values)
3. [**Normal ifadeler**](#regular-expressions)

### <a name="literal-values"></a>Değişmez değerler
Değişmez değer yorumlanır metin % simgenin hariç olmak üzere tüm özel karakterleri eşleşmesi gereken değerinin bir parçası kabul eder. Diğer bir deyişle, bir hazır değer kümesine koşul eşleşen `\'*'\` değeri tam olduğunda yalnızca karşılanması (yani, `\'*'\`) bulunur.
 
Bir yüzde simge URL kodlaması göstermek için kullanılır (örneğin, `%20`).

### <a name="wildcard-values"></a>Joker karakter değerleri
Bir joker karakter değeri olarak yorumlanır metin ek anlam özel karakterleri atar. Aşağıdaki tabloda şu karakter kümesini nasıl yorumlanacak açıklanmaktadır.

Karakter | Açıklama
----------|------------
\ | Ters eğik çizgi bu tabloda belirtilen karakterlerden herhangi birini kaçınmak için kullanılır. Ters eğik çizgi kaçış doğrudan özel karakter önce belirtilmiş olması gerekir.<br/>Örneğin, aşağıdaki söz dizimini yıldız çıkışları:`\*`
% | Bir yüzde simge URL kodlaması göstermek için kullanılır (örneğin, `%20`).
* | Bir yıldız işareti bir veya daha fazla karakter temsil eden bir joker karakterdir.
Alanı | Bir boşluk karakteri bir eşleşme koşul ya da belirtilen değerler veya desenleri tarafından karşılanması olduğunu gösterir.
'value' | Tek bir tırnakla özel bir anlamı yoktur. Ancak, tek tırnak kümesi, bir değer değişmez değer ele alınması gerektiğini belirtmek için kullanılır. Aşağıdaki şekillerde kullanılabilir:<br><br/>-Bu bir eşleşme koşul herhangi bir kısmının karşılaştırma değeri belirtilen değerle eşleşen her memnun olmasını sağlar.  Örneğin, `'ma'` aşağıdaki dizelerden herhangi birinde eşleşir: <br/><br/>/Business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/ İş/şablonu. **ma**p<br /><br />-Bu harf karakter olarak belirtilmesi için bir özel karakter sağlar. Örneğin, bir dizi tek tırnak içinde bir boşluk karakteri kapsayan tarafından bir değişmez değer boşluk karakteri belirtmiş olabilir (yani, `' '` veya `'sample value'`).<br/>-Belirtilmesi boş bir değer verir. Tek tırnak kümesi belirterek boş bir değer belirtin (yani, '').<br /><br/>**Önemli:**<br/>-Belirtilen değer bir joker karakter içermiyorsa, ardından bunu otomatik olarak bir hazır değer kabul edilir. Bu, tek tırnak kümesi belirtmeye gerek olmadığı anlamına gelir.<br/>-Bir ters eğik çizgi bu tablodaki başka bir karakteri kaçış değil, ardından bunu bir dizi tek tırnak içinde belirtildiğinde yoksayılacak.<br/>-Bir özel karakter bir harf karakter ters eğik çizgi kullanarak çıkış olarak belirtmek için başka bir yol (yani, `\`).

### <a name="regular-expressions"></a>Normal ifadeler

Normal ifadeler bir metin değerini, içinde arama yapılacak bir desen tanımlayın. Normal ifade gösterimi belirli anlamları çeşitli simgeler tanımlar. Aşağıdaki tabloda nasıl özel karakterleri belirten eşleşme koşullar ve normal ifadeler destek özellikleri tarafından kabul edilir.

Özel karakter | Açıklama
------------------|------------
\ | Ters eğik çizgi karakteri aşağıdaki çıkışları onu. Bu, normal ifade anlamlarını almak yerine bir hazır değer olarak kabul edilmesi bu karakteri neden olur. Örneğin, aşağıdaki söz dizimini yıldız çıkışları:`\*`
% | Bir yüzde simge anlamı üzerinde kullanım bağlıdır.<br/><br/> `%{HTTPVariable}`: Bu sözdizimi bir HTTP değişkeni tanımlar.<br/>`%{HTTPVariable%Pattern}`: Bu sözdizimi bir yüzde simge bir HTTP değişken ve ayırıcı olarak tanımlamak için kullanır.<br />`\%`: Bir yüzde simge kaçış sağlar URL kodlaması belirtmek için veya bir hazır değer kullanılacak (örn., `\%20`).
* | Bir yıldız işareti sıfır veya daha fazla kez eşleştirilmesini önceki karakteri sağlar. 
Alanı | Bir boşluk karakteri genellikle harf karakter olarak kabul edilir. 
'value' | Tek tırnak değişmez değer karakter olarak kabul edilir. Tek tırnak birtakım özel bir anlamı yok.


## <a name="next-steps"></a>Sonraki adımlar
* [Kurallar altyapısı eşleşme koşulları](cdn-rules-engine-reference-match-conditions.md)
* [Kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-conditional-expressions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
* [Azure CDN'ye genel bakış](cdn-overview.md)
