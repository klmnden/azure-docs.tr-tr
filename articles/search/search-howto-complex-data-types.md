---
title: Karmaşık veri türlerini modelleme - Azure Search hakkında
description: İç içe geçmiş veya hiyerarşik veri yapılarını düzleştirilmiş satır kümesi ve koleksiyon veri türü kullanarak, Azure Search dizini modellenebilir.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2017
ms.custom: seodec2018
ms.openlocfilehash: 973623d6c4cb57518af2012bccf67c969146d23c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076217"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Azure Search karmaşık veri türlerini modelleme hakkında
Bazen bir Azure Search dizinini doldurmak için kullanılan dış veri kümeleri düzgünce tablosal bir satır kümesine kesintiye uğratmadığından hiyerarşik veya iç içe substructures içerir. Bu yapıların örneklerini tek bir müşteri, birden çok renkler ve boyutlar, tek bir kitap için birden çok yazarları gibi tek bir SKU için birden çok konumda ve telefon numaralarını içerir ve benzeri. Koşulları modelleme araçlarındaki olarak adlandırılan bu yapıları görebileceğiniz *karmaşık veri türlerini*, *bileşik veri türleri*, *bileşik veri türleri*, veya *toplama veri türleri*, birkaçıdır.

Karmaşık veri türleri, Azure arama'yı yerel olarak desteklenmez, ancak kendini kanıtlamış bir geçici çözüm yapısı düzleştirme ve ardından kullanarak iki adımlı bir işlem içeren bir **koleksiyon** iç yapıyı yeniden oluşturmak için veri türü. Bu makalede açıklanan tekniği aşağıdaki içeriği aratılmak üzere filtrelenir ve sıralanmış çok yönlü, sağlar.

## <a name="example-of-a-complex-data-structure"></a>Karmaşık veri yapısı örneği
Genellikle, söz konusu verileri, JSON veya XML belgeleri bir dizi olarak veya Azure Cosmos DB gibi bir NoSQL deposu öğelerinde yer alıyor. Yapısal olarak, arama ve filtre gereken birden çok alt öğe kalmamasını sınama kaynaklanır.  Geçici çözüm gösteren bir başlangıç noktası olarak, kişiler bir dizi örnek olarak listeleyen aşağıdaki JSON belgesini alın:

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

'İd' alanlar adlandırılmış olsa da 'name' ve 'şirket' kolayca bire bir Azure Search dizini içindeki alanları olarak eşlenebilir, konumları, hem bir dizi konumu kimlikleri ve bunun yanı sıra konumu açıklamaları dizisi 'konum' alanı içerir. Azure Search, bunu destekleyen bir veri türü yok düşünüldüğünde, Azure Search'te Bu model için farklı bir şekilde ihtiyacımız var. 

> [!NOTE]
> Bu teknik, ayrıca Kirk Evans tarafından bir blog gönderisinde açıklanan [Azure Cosmos DB ile Azure Search dizini oluşturma](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), "veri düzleştirme" olarak adlandırılan tekniği gösterir adlı bir alan yoktur gerçekleştirilmesine `locationsID` ve `locationsDescription` her ikisi de olan [koleksiyonları](https://msdn.microsoft.com/library/azure/dn798938.aspx) (veya dizeler dizisi).   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a>1. Bölüm: Her bir alanı dizi düzleştirme
Bu veri kümesine gönderme bir Azure Search dizini oluşturmak için iç içe geçmiş düzeltilebilmenize alanları tek tek oluşturun: `locationsID` ve `locationsDescription` veri türüne sahip [koleksiyonları](https://msdn.microsoft.com/library/azure/dn798938.aspx) (veya dizeler dizisi). Bu alanlarda içine '1' ve '2' değerlerini dizin `locationsID` alanını John Smith ve '3' & '4' değerleri için `locationsID` Jen Campbell için alan.  

Azure Search içinde verilerinizin şöyle görünür: 

![Örnek veriler, 2 satır](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>2. Bölüm: Dizin tanımında koleksiyon alan ekleme
Dizin şemasında alan tanımları bu örnektekine benzer görünebilir.

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
Dizini oluşturulan ve verileri varsayıldığında, veri kümesini arama sorgu yürütmeyi doğrulamak için çözüm artık test edebilirsiniz. Her **koleksiyon** alana olmalıdır **aranabilir**, **filtrelenebilir** ve **modellenebilir**. Gibi sorguları çalıştırmak alabiliyor olmanız gerekir:

* 'Adventureworks merkezde' çalışan tüm kişilerin bulun.
* 'Giriş Office' çalışan kişilerin sayısını alın.  
* Bir 'ana ofiste' çalışan kişiler her yerde kişilerin sayısını birlikte çalıştıkları diğer ofislerdeki gösterir.  

Hem konum kimliği, hem de konum açıklaması birleştiren bir arama yapmanız gereken burada bu yöntem parçalayın denk olur. Örneğin:

* Giriş Office sahip olduğu tüm kişileri bulun ve 4'ün bir konum kimliği vardır.  

Aşağıdaki gibi görünüyordu özgün içerik geri çağırma varsa:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Verileri ayrı alanlara ayırdık, Jen Campbell ilişkili olduğu için ancak biz biri olduğunu bilerek giriş Office zorlaması `locationsID 3` veya `locationsID 4`.  

Bu durumu işlemek için başka bir alan tanımlayın dizindeki tüm verileri tek bir koleksiyon birleştirir.  Bizim örneğimizde, biz Bu alan çağıracak `locationsCombined` ve içerikle ayırabiliriz bir `||` düşündüğünüz herhangi bir ayırıcı karakter içeriğiniz için benzersiz bir dizi olacaktır seçebilmenize rağmen. Örneğin: 

![Örnek veriler, 2 satır ayırıcı ile](./media/search-howto-complex-data-types/sample-data-2.png)

Bunu kullanarak `locationsCombined` alan, biz artık uyum sağlayacak daha da fazla sorgular gibi:

* Bir 'ana ofiste' konum kimliği, '4' çalışan kişilerin sayısını gösterir.  
* Kimliği '4' konumu ile bir 'ana ofiste' çalışan kişiler arayın. 

## <a name="limitations"></a>Sınırlamalar
Bu teknik, çeşitli senaryolar için yararlıdır, ancak her durumda geçerli değildir.  Örneğin:

1. Varsa, karmaşık veri türü statik alanları kümesine sahip değil ve tüm olası türleri tek bir alan için eşleme yolu yoktu. 
2. İç içe geçmiş nesnelerde Güncelleştirme ne Azure Search dizini güncelleştirilmesi gerekiyor tam olarak belirlemek için bazı ek çalışmalar gerektirir

## <a name="sample-code"></a>Örnek kod
Karmaşık bir JSON veri kümesi ile Azure Search dizini ve sorgu sayısı üzerinden bu veri kümesi bu gerçekleştirme hakkında bir örnek görebilirsiniz [GitHub deposunu](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Sonraki adım
[Karmaşık veri türleri için yerel destek için oy](https://feedback.azure.com/forums/263029-azure-search) Azure arama Uservoice'ta sayfasında ve bize özellik uygulamasıyla ilgili dikkate alınması gereken istediğiniz herhangi bir ek giriş sağlar. Siz de benim için doğrudan Twitter'da ulaşabilir @liamca.

