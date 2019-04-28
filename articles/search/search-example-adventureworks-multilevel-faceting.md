---
title: 'Örnek: Çok düzeyli modeller - Azure Search'
description: Çok düzeyli Taksonomi, üzerinde uygulama sayfaları dahil edebilirsiniz bir iç içe geçmiş bir gezinti yapısı oluşturmak için model oluşturma yapıları oluşturmayı öğrenin.
author: cstone
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: chstone
ms.openlocfilehash: 7fa17528931be40109d81edac0f15a6a6822ec01
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61291780"
---
# <a name="example-multi-level-facets-in-azure-search"></a>Örnek: Azure Arama’daki çok düzeyli modeller

Azure arama şemaları çok düzeyli sınıflandırma kategorisi açıkça desteklemez, ancak bunları dizin oluşturma ve sonra sonuçları bazı özel işlem uygulayarak önce içeriği işleyerek yaklaşık. 

## <a name="start-with-the-data"></a>Verilerle başlayın

Bu makalede örnek bir önceki örnekte derlemeler [AdventureWorks stok veritabanı modeli](search-example-adventureworks-modeling.md), Azure Arama'daki çok düzeyli model oluşturma göstermek için.

AdventureWorks üst-alt ilişkisi olan basit bir iki düzeyli sınıflandırma vardır. Bu yapı sabit uzunluklu sınıflandırma derinliklerine ulaşmak için basit bir SQL birleştirme sorgusu sınıflandırma gruplandırmak için kullanılabilir:

```T-SQL
SELECT 
  parent.Name Parent, category.Name Category, category.ProductCategoryId
FROM 
  SalesLT.ProductCategory category
LEFT JOIN
  SalesLT.ProductCategory parent
  ON category.ParentProductCategoryId=parent.ProductCategoryId
```

  ![Sorgu sonuçları](./media/search-example-adventureworks/prod-query-results.png "sorgu sonuçları")

## <a name="indexing-to-a-collection-field"></a>Bir koleksiyon alan için dizin oluşturma

Bu yapıyı içeren dizin oluşturma bir **Collection(Edm.String)** alan öznitelikleri aranabilir, filtrelenebilir, modellenebilir içerdiğinden emin olarak, bu verileri depolamak için Azure Search şema alan ve alınabilir.

Artık, belirli bir sınıflandırma kategorisine başvuruyor içeriği dizin oluşturulurken sınıflandırma her sınıflandırma düzeyini metni içeren bir dizi olarak gönderin. Örneğin, bir varlık için `ProductCategoryId = 5 (Mountain Bikes)`, alan olarak gönder `[ "Bikes", "Bikes|Mountain Bikes"]`

Alt kategori değeri "Dağ bisikleti" içinde "Bisiklet" üst kategori dahil edilmesi dikkat edin. Her alt kategorisi kök öğe göreli yolun tamamını katıştırmanız. Kanal karakter ayırıcısı isteğe bağlıdır, ancak tutarlı olması ve kaynak metni görünmemelidir. Ayırıcı karakter uygulama kodunda modeli sonuçları sınıflandırma ağacından yeniden oluşturmak için kullanılır.

## <a name="construct-the-query"></a>Sorgu yapısı

Sorgu gönderirken aşağıdaki modeli belirtimi (Sınıflandırma, modellenebilir Sınıflandırma alanı olduğu) şunları içerir: `facet = taxonomy,count:50,sort:value`

Sayısı değeri tüm olası sınıflandırma değerleri döndürülecek yüksek olmalıdır. AdventureWorks veri 41 farklı bir sınıflandırma değerleri, bu nedenle içeren `count:50` yeterlidir.

  ![Çok yönlü filtre](./media/search-example-adventureworks/facet-filter.png "çok yönlü filtreleme")

## <a name="build-the-structure-in-client-code"></a>Yapı istemci kodu oluşturma

İstemci uygulama kodunuzda, dikey çizgi karakterinden her modeli değere bölerek sınıflandırma ağaç yeniden yapılandırma.

```javascript
var sum = 0
  , categories = {children:{},length:0}
  , results = getSearchResults();
separator = separator || '|';
field = field || 'taxonomy';
results['@search.facets'][field].forEach(function(d) {
  var node = categories;
  var parts = d.value.split(separator);
  sum += d.count;
  parts.forEach(function(c, i) {
    if (!_(node.children).has(c)) {
      node.children[c] = {};
      node.children[c].count = d.count;
      node.children[c].children = {};
      node.children[c].length = 0;
      node.children[c].filter = parts.slice(0,i+1).join(separator);
      node.length++;
    }
    node = node.children[c];
  });
});
categories.count = sum;
```

**Kategorileri** nesne artık doğru sayıları içeren bir daraltılabilir sınıflandırma ağacı oluşturmak için kullanılabilir:

  ![çok düzeyli çok yönlü filtre](./media/search-example-adventureworks/multi-level-facet.png "çok düzeyli çok yönlü filtreleme")

 
Her bağlantı ağacında ilgili filtre uygulamanız gerekir. Örneğin:

+ **Sınıflandırma/any** `(x:x eq 'Accessories')` Donatılar dalda tüm belgeleri döndüren
+ **Sınıflandırma/any** `(x:x eq 'Accessories|Bike Racks')` bisiklet raflar kategorisidir Donatılar dalının altındaki yalnızca belgelerle döndürür.

Bu teknik derin sınıflandırma ağaçları gibi daha karmaşık senaryoları kapsayacak şekilde ölçeklendirilir ve ortaya çıkan farklı ana kategoriler altında alt kategoriler yinelenen (örneğin, `Bike Components|Forks` ve `Camping Equipment|Forks`).

> [!TIP]
> Döndürülen modeller sayısına göre sorgu hızı etkilenir. Çok büyük bir taksonomi kümelerini destekler, bir modellenebilir eklemeyi düşünün **Edm.String** her belge için üst düzey sınıflandırma değerini tutacak bir alan. Ardından yukarıdaki aynı tekniği uygulayabilirsiniz, ancak yalnızca (kök sınıflandırma alanında filtrelenmiş) koleksiyonu modeli sorguyu gerçekleştirmek, kullanıcı genişletir üst düzey düğüm. Veya, % 100 geri çağırma gerekmiyorsa, yalnızca makul bir sayıya modeli sayısını azaltın ve sayısına göre sıralanmış modeli girdileri emin olun.

## <a name="see-also"></a>Ayrıca bkz.

[Örnek: Azure arama için stok AdventureWorks veritabanı modeli](search-example-adventureworks-modeling.md)