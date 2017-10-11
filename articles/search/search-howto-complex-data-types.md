---
title: "Azure Search'te karmaşık veri türlerini modellemek nasıl | Microsoft Docs"
description: "İç içe geçmiş veya hiyerarşik veri yapılarını düzleştirilmiş satır kümesi ve koleksiyonları veri türünü kullanarak Azure Search dizini içinde modellenebilir."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: d576fd7bb267ae7a100589413185b595e3b2be42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Azure Search'te model karmaşık veri türleri hakkında
Azure Search dizini bazen doldurmak için kullanılan dış veri kümeleri düzgünce tablo satır kümesine bozmadığını hiyerarşik veya iç içe substructures içerir. Bu tür yapıları örnekleri birden çok konumda ve telefon numaraları, tek bir rehberi birden çok yazar gibi tek bir SKU için tek bir müşteri, birden çok renkleri ve boyutları içerir ve benzeri. Koşulları modelleme içinde olarak adlandırılan bu yapıları görebilirsiniz *karmaşık veri türlerini*, *bileşik veri türleri*, *bileşik veri türleri*, veya *veri türleri bir araya*, birkaçıdır.

Karmaşık veri türleri yerel olarak Azure arama desteklenmez, ancak kanıtlanmış bir geçici çözüm yapısı düzleştirme ve ardından kullanarak iki adımlı bir işlem içerir bir **koleksiyonu** veri türü iç yapısı yeniden oluşturma. Bu makalede açıklanan teknikleri aşağıdaki aranması içerik filtre ve sıralanmış modellenmiş, sağlar.

## <a name="example-of-a-complex-data-structure"></a>Karmaşık veri yapısı örneği
Genellikle, söz konusu veri JSON veya XML belgeleri kümesi olarak ya da Azure Cosmos DB gibi bir NoSQL deposundaki öğeleri olarak bulunur. Yapısal olarak, arama ve filtre gereken birden çok alt öğe olmaktan challenge kaynaklandığını.  Geçici çözüm gösteren için başlangıç noktası olarak kişiler bir dizi örnek olarak listeler aşağıdaki JSON belgesini alın:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

'ID' alanları adlı olsa da, 'name' ve 'şirket' kolayca bire bir Azure Search dizini içinde alanlar olarak eşlenebilir, hem bir dizi konumu açıklamaları yanı sıra konumu kimlikleri sahip konumları, bir dizi 'konumları' alan içerir. O Azure Search bu destekleyen bir veri türüne sahip değil, Azure Search'te Bu model için farklı bir şekilde ihtiyacımız var. 

> [!NOTE]
> Bu tekniği de blog postasına Kirk Evans tarafından açıklanan [Azure Search dizini oluşturma DocumentDB](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), "veri düzleştirme" adında bir teknik gösterir adlı bir alan olurdu aslına `locationsID` ve `locationsDescription` her ikisi de olan [koleksiyonları](https://msdn.microsoft.com/library/azure/dn798938.aspx) (veya bir dizeler dizisi).   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a>1. Kısım: dizi tek tek alanlarına düzleştirmek
Bu veri kümesi düzenler bir Azure Search dizini oluşturmak için iç içe geçmiş düzeltilebilmenize alanları tek tek oluşturun: `locationsID` ve `locationsDescription` veri türüne sahip [koleksiyonları](https://msdn.microsoft.com/library/azure/dn798938.aspx) (veya bir dizeler dizisi). Bu alanları içine değerleri '1' ve '2' dizini `locationsID` alanında Hasan Aydın ve değerleri '3' & '4' için içine `locationsID` Jen Campbell için alan.  

Verilerinizi Azure Search içinde şöyle olabilir: 

![Örnek veriler, 2 satır](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>2. Kısım: Dizin tanımı'nda koleksiyonu alan ekleme
Dizin şemasında alan tanımları bu örneğe benzer görünür.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Arama davranışlarını doğrulamak ve isteğe bağlı olarak dizin genişletme
Dizin oluşturulur ve veriler yüklü olduğu varsayılarak, arama sorgu yürütme dataset karşı doğrulamak için çözüm artık test edebilirsiniz. Her **koleksiyonu** alan olmalıdır **aranabilir**, **filtrelenebilir** ve **modellenebilir**. Sorgular gibi çalışır olması gerekir:

* 'Adventureworks merkezde' çalışan tüm kişilerin bulun.
* 'Giriş Office' çalışan kişilerin sayısını alır.  
* Bir 'giriş ofiste' çalışan kişilerin her yerde kişiler sayısını birlikte çalıştıkları diğer ofislerdeki gösterir.  

Hem konum kimliği, hem de konum açıklaması birleştiren bir arama yapmak gerektiğinde burada bu yöntem parçalayın döner olur. Örneğin:

* Bir giriş Office sahip oldukları tüm kişileri Bul ve 4'ün bir konum kimliği vardır.  

Özgün içerik arama şu şekilde geri çağırma varsa:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Biz verilerin farklı alanlara ayrılmış, Jen Campbell teklifiyle için ancak biz olduğunu bilerek if giriş Office zorlaması `locationsID 3` veya `locationsID 4`.  

Bu durumu işlemek için tüm verileri tek bir koleksiyona birleştirir dizindeki başka bir alan tanımlayın.  Bizim örneğimizde, biz Bu alan çağıracak `locationsCombined` ve biz içerikle ayıracak bir `||` düşündüğünüz ayırıcı karakter içeriğiniz için benzersiz bir dizi olacaktır seçebilmenize rağmen. Örneğin: 

![Örnek veriler, 2 satır ayırıcı ile](./media/search-howto-complex-data-types/sample-data-2.png)

Bu kullanarak `locationsCombined` alan, biz şimdi uyum sağlayacak daha da fazla sorguları gibi:

* Bir 'giriş ofiste' konum kimliği ile '4' ın çalışan kişilerin sayısını gösterir.  
* Kimliği '4' konumu ile bir 'giriş ofiste' çalışan kişilerin arayın. 

## <a name="limitations"></a>Sınırlamalar
Bu teknik senaryolar sayısı için yararlıdır, ancak her durumda geçerli değil.  Örneğin:

1. Varsa, karmaşık veri türü statik alanları kümesine sahip değildir ve olası tüm türler için tek bir alanı eşlemek için hiçbir yolu yoktu. 
2. İç içe geçmiş nesnelerde Güncelleştirme ne Azure Search dizini güncelleştirilmesi gerektiğinde tam olarak belirlemek için bazı ek çalışmalar gerektirir

## <a name="sample-code"></a>Örnek kod
Karmaşık bir JSON veri kümesi Azure Search dizini oluşturmak ve bu veri kümesi bu üzerinden sorguları bir dizi yerine konusunda bir örnek görebilirsiniz [GitHub deposuna](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Sonraki adım
[Karmaşık veri türleri için yerel destek için oy](https://feedback.azure.com/forums/263029-azure-search) üzerinde Azure arama UserVoice sayfasında ve bize özellik uygulamasıyla ilgili dikkate alınması gereken istediğiniz herhangi bir ek giriş sağlar. Da bana doğrudan Twitter'da ulaşabilirsiniz @liamca.

