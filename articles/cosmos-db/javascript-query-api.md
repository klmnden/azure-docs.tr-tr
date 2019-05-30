---
title: JavaScript dil ile tümleşik sorgu API'si, Azure Cosmos DB ile çalışma
description: Bu makalede, Azure Cosmos DB'de saklı yordamları ve Tetikleyicileri oluşturmak JavaScript dil ile tümleşik sorgu API'si için kavramlar tanıtılmaktadır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: cc1815ce4a7a9ed40848e4a67a7fd9e032c1daa1
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226205"
---
# <a name="javascript-query-api-in-azure-cosmos-db"></a>Azure Cosmos DB'de JavaScript sorgu API'si

Azure Cosmos DB'de SQL API'sini kullanarak sorgu göndermeye ek olarak [Cosmos DB sunucu tarafı SDK](https://azure.github.io/azure-cosmosdb-js-server/) bir JavaScript arabirimi kullanarak en iyi duruma getirilmiş sorguları yapmanıza olanak tanır. Bu JavaScript arabirimi kullanmak için SQL dilinin haberdar olmanız gerekmez. API işlevi dizisine koşul işlevleri geçirerek sorguları programlı olarak oluşturmanıza olanak sağlayan JavaScript sorgu ECMAScript5'ın dizi öğelerin ve yaygın olarak kullanılan JavaScript kitaplıklarını Lodash gibi tanıdık bir söz dizimi ile çağırır. Sorgular, JavaScript çalışma zamanı tarafından ayrıştırılan ve Azure Cosmos DB dizinleri kullanarak verimli bir şekilde yürütüldü.

## <a name="supported-javascript-functions"></a>Desteklenen JavaScript işlevleri

| **İşlevi** | **Açıklama** |
|---------|---------|
|`chain() ... .value([callback] [, options])`|Value()) ile bitirilmelidir zincirleme bir arama başlatır.|
|`filter(predicateFunction [, options] [, callback])`|Sonuç kümesine girdi belgelerini içeri/dışarı filtrelemek için true/false döndüren bir koşul işlevini kullanarak giriş filtreler. Bu işlev bir WHERE yan tümcesinde SQL benzer şekilde davranır.|
|`flatten([isShallow] [, options] [, callback])`|Bir araya getirir ve tek bir dizide her giriş öğesini dizilerden düzleştirir. Bu işlev SelectMany LINQ benzer şekilde davranır.|
|`map(transformationFunction [, options] [, callback])`|Her giriş öğesini bir JavaScript nesnesi veya değer eşleyen bir dönüştürme işlevi verilen bir projeksiyon geçerlidir. Bu işlev bir SELECT yan tümcesi SQL benzer şekilde davranır.|
|`pluck([propertyName] [, options] [, callback])`|Bu işlev, her giriş öğesinden tek bir özelliğinin değerini ayıklar bir eşleme için bir kısayoldur.|
|`sortBy([predicate] [, options] [, callback])`|Belgeler giriş belgesi akış, belirli bir koşul kullanarak artan düzende sıralayarak yeni bir belge kümesi üretir. Bu işlev bir ORDER BY yan tümcesi içinde SQL benzer şekilde davranır.|
|`sortByDescending([predicate] [, options] [, callback])`|Belgeler giriş belgesi akış, belirli bir koşul kullanarak azalan düzende sıralayarak yeni bir belge kümesi üretir. Bu işlev bir x DESC ORDER BY yan tümcesinde SQL benzer şekilde davranır.|
|`unwind(collectionSelector, [resultSelector], [options], [callback])`|İç diziyi ile iç birleşim gerçekleştirir ve sonuçları her iki taraftan tanımlama grupları halinde sonucu yansıtma ekler. Bir kişinin belgeyi person.pets ile birleştirme [kişi, evcil hayvan] diziler örneği için oluşturur. Bu .NET bağlantısında SelectMany benzer.|

Koşul ve/veya Seçici işlevlerinin içine dahil edilirse, aşağıdaki JavaScript yapıları otomatik olarak doğrudan Azure Cosmos DB dizinlerini çalıştırılmak üzere iyileştirilen:

- Basit işleçleri: `=` `+` `-` `*` `/` `%` `|` `^` `&` `==` `!=` `===` `!===` `<` `>` `<=` `>=` `||` `&&` `<<` `>>` `>>>!` `~`
- Nesne sabit değeri de dahil olmak üzere hazır: {}
- dönüş var

Aşağıdaki JavaScript yapıları için Azure Cosmos DB dizinleri en iyi duruma getirilmiş değil:

- Denetim akışı (örneğin, varsa, ancak)
- İşlev çağrıları

Daha fazla bilgi için [Cosmos DB sunucu tarafı JavaScript belgeleri](https://azure.github.io/azure-cosmosdb-js-server/).

## <a name="sql-to-javascript-cheat-sheet"></a>SQL için JavaScript kopya kağıdı

Aşağıdaki tabloda, çeşitli SQL sorguları ve karşılık gelen JavaScript sorguları sunulmaktadır. SQL sorguları gibi ile özellikleri (örneğin, öğesi.kimlik) büyük küçük harfe duyarlıdır.

> [!NOTE]
> `__` (çift alt çizgi) olan bir diğer `getContext().getCollection()` JavaScript sorgu API'si kullanılırken.

|**SQL**|**JavaScript sorgu API'si**|**Açıklama**|
|---|---|---|
|SEÇİN *<br>Belgelerinden| __.map(function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;Belge döndürür;<br>});|Sonuç tüm belgelerde (devamlılık belirteci ile sayfalandırılmış) olur.|
|SELECT <br>&nbsp;&nbsp;&nbsp;docs.id,<br>&nbsp;&nbsp;&nbsp;docs.Message msg, olarak<br>&nbsp;&nbsp;&nbsp;docs.Actions <br>Belgelerinden|__.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;{döndürür<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|Kimliği, ileti (diğer adlı msg için) ve tüm belgeleri eylemden projeleri.|
|SEÇİN *<br>Belgelerinden<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;doc.id dönüş === "X998_Y998";<br>});|Sorgular için koşul belgelerle: kimlik = "X998_Y998".|
|SEÇİN *<br>Belgelerinden<br>WHERE<br>&nbsp;&nbsp;&nbsp;ARRAY_CONTAINS(docs.Tags, 123)|__.filter(function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;x.Tags dönüş & & x.Tags.indexOf(123) > -1;<br>});|Etiketler özelliği ve etiketlere sahip olan belgeler için sorgulardır 123 değerini içeren bir dizi.|
|SELECT<br>&nbsp;&nbsp;&nbsp;docs.id,<br>&nbsp;&nbsp;&nbsp;docs.Message msg olarak<br>Belgelerinden<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc.id dönüş === "X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{döndürür<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|Bir koşul ile belgeler için sorgu kimliği = "X998_Y998" ve ardından kodu ve iletinin (ileti için diğer adlı) projeleri.|
|DEĞER etiketi<br>Belgelerinden<br>Etiket IN docs katılın. Etiketleri<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Belge döndürür. Etiketler & & Array.isArray (doc. Etiketler);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc._ts döndürür;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|Etiketler, bir dizi özelliği ve elde edilen belgeler _ts zaman damgası sistem özelliği sıralar ve ardından projeleri + etiketler dizisi düzleştirir belgeler için filtreler.|

## <a name="next-steps"></a>Sonraki adımlar

Kavramları ve nasıl yapılır yazma daha fazla bilgi edinin ve saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB'de kullanın:

- [Saklı yordamları ve Tetikleyicileri Javascript sorgu API'si kullanarak yazma](how-to-write-javascript-query-api.md)
- [Azure Cosmos DB ile çalışma saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB'de saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler kullanma](how-to-use-stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB JavaScript sunucu tarafı API Başvurusu](https://azure.github.io/azure-cosmosdb-js-server)
- [JavaScript ES6 (ECMA 2015)](https://www.ecma-international.org/ecma-262/6.0/)
