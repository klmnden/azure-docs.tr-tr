---
title: Basit Sorgu söz dizimi - Azure Search
description: Azure Search'te tam metin arama sorguları için kullanılan basit sorgu söz dizimi referansı.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 75e2d7c493b535c984b0ef61dd9a9fae53aee80a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024195"
---
# <a name="simple-query-syntax-in-azure-search"></a>Azure Search'te Basit Sorgu söz dizimi
Azure arama, iki Lucene tabanlı sorgu dili uygular: [Basit Sorgu ayrıştırıcı](https://lucene.apache.org/core/4_7_0/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) ve [Lucene sorgu ayrıştırıcı](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Azure Search'te belirsiz/slop seçenekleri basit sorgu söz dizimi dahil değildir.  

> [!NOTE]  
>  Azure arama, bir alternatif sağlar [Lucene sorgu söz dizimi](query-lucene-syntax.md) daha karmaşık sorgular için. Sorgu mimarisi ve her bir sözdizimi avantajları hakkında daha fazla bilgi için bkz: [nasıl tam metin araması Azure Search'te çalışır](search-lucene-query-architecture.md).

## <a name="how-to-invoke-simple-parsing"></a>Basit ayrıştırma çağırmak nasıl

Basit söz dizimi varsayılandır. Çağırma söz dizimi tamdan basite sıfırlayarak gereklidir. Söz dizimi açıkça ayarlamak için kullanın `queryType` arama parametresi. Geçerli değerler `simple|full`, ile `simple` varsayılan olarak ve `full` Lucene için. 

## <a name="query-behavior-anomalies"></a>Sorgu davranış anormallikleri

Bir veya daha fazla koşullarıyla herhangi bir metin, sorgu yürütme için geçerli bir başlangıç noktası olarak kabul edilir. Azure arama, tüm metin analizi sırasında bulunan tüm farklılıklar da dahil olmak üzere terimleri içeren belgeleri eşleşir. 

Bu ses gibi açık bir özelliği, Azure Search'te sorgu yürütme yoktur, *olabilir* ürün sonuçları daha fazla hüküm ve işleçler arama azalan yerine artan beklenmeyen sonuçlar girişi eklendi dize. Olup bu genişletmesinin gerçekten oluşup eklenmesi ile birleştirilmiş bir NOT işleci, bağlıdır. bir `searchMode` belirleyen nasıl değil parametre ayarı açısından yorumlanır ve veya veya davranışlar. Varsayılan olarak, verilen `searchMode=Any`, ve bir NOT işleci, işlem veya eylem olarak, hesaplanan şekilde `"New York" NOT Seattle` Seattle olmayan tüm şehirleri döndürür.  

Genellikle, büyük olasılıkla kullanıcılar büyük olasılıkla daha fazla yerleşik gezinti yapıları sahip e-ticaret sitelerini sorgu operatörün dahil olduğu bu içerikler üzerinde arama uygulamaları için kullanıcı etkileşimi desenleri davranışları görürsünüz. Daha fazla bilgi için [NOT işleci](#not-operator). 

## <a name="boolean-operators-and-or-not"></a>Boole işleçleri (AND, OR, NOT) 

Zengin karşı eşleşen belgelerin bulunduğu bir ölçüt oluşturmak için bir sorgu dizesi işleçleri ekleyebilir. 

### <a name="and-operator-"></a>AND işleci `+`

Artı işareti ve işlecidir. Örneğin, `wifi+luxury` her ikisini de içeren belgeleri arar `wifi` ve `luxury`.

### <a name="or-operator-"></a>OR işleci `|`

OR işleci, dikey çubuk veya dikey çizgi karakterinden ' dir. Örneğin, `wifi | luxury` ya da içeren belgeleri arar `wifi` veya `luxury` veya her ikisini de.

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT işleci `-`

NOT işleci bir eksi işaretidir. Örneğin, `wifi –luxury` olan belgeleri arar `wifi` terimi ve/veya olmadığı `luxury` (ve/veya denetlenen `searchMode`).

> [!NOTE]  
>  `searchMode` Bir terim işleç and işleciyle ORed olmaması sorgusunda diğer koşullarıyla olup denetimleri seçeneği bir `+` veya `|` işleci. Bu geri çağırma `searchMode` olarak ayarlanabilir `any` (varsayılan) veya `all`. Kullanırsanız `any`, daha fazla sonuç ekleyerek ve varsayılan değer sorguları geri çağırma bildirimi yayımlayabiliriz artırmak `-` "Veya NOT" yorumlanacaktır. Örneğin, `wifi -luxury` ya da terimi içeren belgeleri eşleşecektir `wifi` ya da terim içermeyen `luxury`. Kullanırsanız `all`, daha az sonuç ekleyerek sorguları duyarlığını artırır ve varsayılan olarak - "Ve NOT" yorumlanacaktır. Örneğin, `wifi -luxury` terimlerini içeren belgelerle eşleşmesini `wifi` ve "lüks" terimi içermez. Tartışmaya daha sezgisel bir davranışı budur `-` işleci. Bu nedenle, kullanmayı düşünmelisiniz `searchMode=all` yerine `searchMode=any` duyarlık geri çekme, yerine arar iyileştirmek istiyorsanız *ve* kullanıcılarınızın sık kullandığınız `-` aramalarındaki işleci.

## <a name="suffix-operator"></a>Sonek operatörü

Bir yıldız işareti soneki işlecidir `*`. Örneğin, `lux*` ile başlayan bir terim olan belgeleri arar `lux`, çalışması yok sayılıyor.  

## <a name="phrase-search-operator"></a>Tümcecik arama işleci

İfade işleci bir ifade tırnak içine alır `" "`. Örneğin, `Roach Motel` (tırnak işaretleri olmadan) içeren belgeleri arama `Roach` ve/veya `Motel` herhangi bir sırada herhangi bir yerindeki `"Roach Motel"` birlikte ve söz konusu anlatılmak isteneni içeren belgeler (tırnak işaretleri) yalnızca eşleşir Sipariş (metin analizi hala geçerlidir).

## <a name="precedence-operator"></a>İşleç önceliği

Öncelik işleci, parantez içinde dize kapsayan `( )`. Örneğin, `motel+(wifi | luxury)` motel terim ve ya da içeren belgeleri arar `wifi` veya `luxury` (veya her ikisi de).  

## <a name="escaping-search-operators"></a>Arama işleçlerini kaçış  

 Yukarıdaki sembol arama metnini gerçek bir parçası olarak kullanmak için ters eğik çizgi ile önek tarafından Atlanan. Örneğin, `luxury\+hotel` terimini sonuçlanır `luxury+hotel`. Şeyleri daha tipik durumlarda işlemlerini kolaylaştırmak için iki istisna mevcuttur bu kuralın nerede kaçış gerekli değildir:  

- NOT işleci `-` yalnızca ilk karakterin boşluk sonra ise döneminin ortasında değilse atlanması gerekiyor. Örneğin, `wi-fi` tek bir terim; takvimidir GUID'leri (gibi `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`) tek bir belirteç kabul edilir.
- Sonek operatörü `*` yalnızca boşluk önce son karakter ise döneminin ortasında değilse atlanması gerekiyor. Örneğin, `wi*fi` tek bir belirteç kabul edilir.

> [!NOTE]  
>  Tutar belirteçleri birlikte kaçış olsa da, metin analizi, analiz modunuza bağlı olarak bölüp. Bkz: [dil desteği &#40;Azure arama hizmeti REST API'si&#41; ](index-add-language-analyzers.md) Ayrıntılar için.  

## <a name="see-also"></a>Ayrıca bkz.  

+ [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [OData ifadesi söz dizimi](query-odata-filter-orderby-syntax.md) 
