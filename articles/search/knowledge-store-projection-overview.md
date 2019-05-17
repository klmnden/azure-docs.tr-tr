---
title: Projeksiyonlar deposundaki bir Bilgi Bankası (Önizleme) - Azure Search ile çalışma
description: Kaydet ve zenginleştirilmiş arama dışında senaryolarında kullanım için yapay ZEKA dizinleme işlem hattına verilerinizden Şekil
manager: eladz
author: vkurpad
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: vikurpad
ms.custom: seomay2019
ms.openlocfilehash: f1c7278909557dc92f86c5dfc1f190fddf33f607
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540819"
---
# <a name="working-with-projections-in-a-knowledge-store-in-azure-search"></a>Azure Search'te bir Bilgi Bankası deposunda projeksiyonlar ile çalışma

> [!Note]
> Bilgi Bankası preview ve üretim kullanımı için değil amaçlayan deposudur. [2019-05-06-Önizleme REST API sürümü](search-api-preview.md) bu özelliği sağlar. .NET SDK'sı desteği şu anda yoktur.
>

Azure arama, yapay ZEKA bilişsel beceriler ve dizin oluşturma işleminin parçası olarak özel beceriler aracılığıyla içerik iyileştirmesini sağlar. Zenginleştirmelerinin yapısı belgelerinize eklemek ve daha etkili hale aranıyor. Çoğu durumda, zenginleştirilmiş belgeleri arama, örneğin bilgi araştırma dışındaki senaryolar için kullanışlıdır.

Tahminleri, bir bileşeninin [bilgi deposu](knowledge-store-concept-intro.md), araştırma amacıyla bilgi fiziksel depolama alanına kaydedildi zenginleştirilmiş belgeleri görünümleridir. Bir projeksiyon "ile Power BI gibi araçlarla veri ek çaba ile okuyabilmeniz için ilişkileri koruma gereksinimlerine uygun bir şeklin içinde verilerinizi proje" sağlar. 

Projeksiyonlar satırları ve sütunları Azure tablo depolamada depolanan veriler ya da JSON nesneleri Azure Blob depolamada depolanan tablo, olabilir. Bunu zenginleştirilmiş verilerinizin birden çok projeksiyonlar tanımlayabilirsiniz. Bireysel kullanım durumları için farklı şeklinde aynı verileri istediğinizde bu kullanışlıdır. 

Bilgi deposunu iki tür projeksiyonları destekler:

+ **Tabloları**: En iyi satırları ve sütunları temsil edilen veri tablo projeksiyonlar tablo depolamada bir şema şekil ya da projeksiyon tanımlamanızı sağlar. 

+ **Nesneleri**: Veri ve zenginleştirmelerinin JSON gösterimi, ihtiyacınız olduğunda, nesne projeksiyonlar blobları olarak kaydedilir.

Projeksiyonlar bağlamında tanımlanan görmek için adım adım [nasıl bilgi store ile çalışmaya başlama](knowledge-store-howto.md)

## <a name="projection-groups"></a>Projeksiyon grupları

Bazı durumlarda, farklı Hedeflerinizin karşılanması için farklı şekillerde zenginleştirilmiş verilerinizi proje gerekecektir. Bilgi deposunu yansıtılamadı birden fazla grubu tanımlamanıza olanak sağlar. Projeksiyon grupları karşılıklı olmama ve bağlılık aşağıdaki önemli özelliklere sahip.

### <a name="mutually-exclusivity"></a>Birbirini kurumlarına özel

Tüm içeriği tek bir grup olarak öngörülen diğer projeksiyon gruplar halinde öngörülen veri bağımsızdır. Bu, farklı şeklinde, ancak her projeksiyon grubunda yinelenen veri olduğunu gösterir. 

Projeksiyon gruplarında zorunlu bir projeksiyon türü bir projeksiyon grubuyla karşılıklı özel kullanım olanağını sınırlamadır. Yalnızca tablo projeksiyonlar ya da nesne projeksiyonlar tek bir grup içinde tanımlayabilirsiniz. Tablolar ve nesneler hem istiyorsanız, tablolar için bir yansıtma grubu ve nesneler için ikinci bir projeksiyon Grup tanımlayın.

### <a name="relatedness"></a>Bağlılık

Tüm içeriği tek bir projeksiyon gruptaki öngörülen verilerdeki ilişkileri korur. İlişkiler üzerinde oluşturulmuş bir anahtar temel alır ve her alt düğümü üst düğümün başvuru korur. İlişkiler projeksiyon grupları yayılmaz ve tablo ya da bir projeksiyon grubunda oluşturulan nesneleri hiçbir diğer projeksiyon gruplarında oluşturulan verileri ilişkisi.

## <a name="input-shaping"></a>Şekillendirme giriş
Anahtar için etkili kullanımını doğru şekle veya yapıda verileriniz alınıyor, tablo veya nesne olabilir. Şekil veya verilerinizi nasıl, erişim ve kullanmak planınıza göre yapı özelliği olarak sunulan temel özelliktir **Shaper** beceri becerilerine içinde.  

Projeksiyonlar izdüşümü şemasını eşleşen zenginleştirme ağacında bir nesne varsa tanımlamak kolaydır. Güncelleştirilmiş [Shaper beceri](cognitive-search-skill-shaper.md) zenginleştirme ağacı farklı düğümünden bir nesne oluşturun ve bunları yeni bir düğüm üst olanak tanır. **Shaper** beceri karmaşık türler ile iç içe geçmiş nesnelerde tanımlamanıza izin verir.

Bu şeklin artık kullanıma proje için ihtiyacınız olan tüm öğeleri içeren tanımlanan yeni bir şekil varsa, projeksiyonlar kaynağı olarak veya başka bir beceri girdi olarak kullanabilirsiniz.

## <a name="table-projections"></a>Tablo projeksiyonlar

Alma daha kolay hale getirdiği için Power BI ile veri keşfi için tablo projeksiyonlar öneririz. Ayrıca, değiştirmek için tablo ilişkisini arasındaki kardinalite tablo tahminleri sağlar. 

İlişkileri koruma dizininizdeki tek bir belgenin birden çok tabloya yansıtabilirsiniz. Bir alt düğüm aynı gruptaki başka bir tablonun kaynağı olmadığı sürece tüm şekil için birden çok tablo yansıtılırken her tabloya yansıtılacak.

### <a name="defining-a-table-projection"></a>Bir tablo projeksiyon tanımlama

İçinde bir tablo projeksiyon tanımlarken `knowledgeStore` öğesi becerilerinizi tablo kaynak iyileştirmesini ağacında bir düğümü eşleyerek başlatın. Genellikle bu düğümün çıktısı olup bir **Shaper** tablolarına proje için gereken belirli bir şekilde oluşturmak için becerileri listesine eklenen beceri. Seçtiğiniz proje düğümü dilimlenebilir birden çok tablo projeye için. Proje istediğiniz tablo listesini tablo tanımıdır. 

Her tablo üç özellik gerektirir:

+ TableName: Azure depolama tablo adı.

+ generatedKeyName: Bu satırı benzersiz olarak tanımlayan anahtar sütun adı.

+ Kaynak: Zenginleştirme ağaç düğümü, zenginleştirmelerinin gelen ayarlanabileceğine. Bu genellikle bir shaper çıktısı olan, ancak çıkış herhangi birinin becerileri olabilir.

İşte bir örnek tablo yansıtılamadı.

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [
            { "tableName": "MainTable", "generatedKeyName": "SomeId", "source": "/document/EnrichedShape" },
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/EnrichedShape/*/KeyPhrases/*" },
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/EnrichedShape/*/Entities/*" }
          ]
        },
        {
          "objects": [
            
          ]
        }
      ]
    }
}
```
Bu örnekte gösterildiği gibi anahtar ifadeleri ve varlıkları farklı tablolara modellenir ve her satır için üst (MainTable) geri başvuru içerir. 

Aşağıdaki çizim Caselaw alıştırmada bir başvurudur [bilgi store ile çalışmaya başlama konusunda](knowledge-store-howto.md). Burada birden çok fikirlerini bir durumda olması ve içerdiği varlıklar tanımlayan tarafından her fikrim zenginleştirilmiş bir senaryoda, burada gösterildiği gibi projeksiyonlar model.

![Varlıklar ve ilişkiler tablolardaki](media/knowledge-store-projection-overview/TableRelationships.png "tablo projeksiyonlar ilişkileri modelleme")

## <a name="object-projections"></a>Nesne projeksiyonlar

Nesnesi, JSON temsilleri herhangi bir düğümden kaynaklanan zenginleştirme ağacının projeksiyonlardır. Birçok durumda, aynı **Shaper** tablo projeksiyon oluşturan beceri, bir nesne yansıtma oluşturmak için kullanılabilir. 

```json
{
 
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
          "objects": [
            {
              "storageContainer": "Reviews", 
              "format": "json", 
              "source": "/document/Review", 
              "key": "/document/Review/Id" 
            }
          ]
        }
      ]
    }
}
```

Bir nesne yansıtma oluşturuluyor, birkaç nesneye özgü öznitelik gerektirir:

+ storageContainer: Nesneleri kaydedileceği kapsayıcı
+ Kaynak: İzdüşüm kökündeki olan zenginleştirme ağaç düğümünü yolu
+ Anahtar: Depolanacak nesne için benzersiz bir anahtar temsil eden bir yolu. Kapsayıcıda blob adı oluşturmak için kullanılır.

## <a name="projection-lifecycle"></a>Projeksiyon yaşam döngüsü

Projeksiyonlar, kaynak verilere veri kaynağınıza bağlı yaşam döngüleri vardır. Verilerinizi güncelleştirildi ve yeniden dizine, projeksiyonlar sonuçları, sonunda tutarlı bir veri kaynağındaki verileri projeksiyonlardır sağlama zenginleştirmelerinin ile güncelleştirilir. Projeksiyonlar dizininiz için yapılandırdığınız silme ilkesi devralır. 

## <a name="using-projections"></a>Projeksiyonlar kullanma

Dizin Oluşturucu çalıştırdıktan sonra kapsayıcıları veya projeksiyonlar belirtilen tablolarda öngörülen verileri okuyabilir. 

Analiz için Power bı'da araştırma Azure tablo depolama veri kaynağı olarak ayarlamak gibi basit bir işlemdir. Görsel öğeleri kümesini, ilişkileri içinde yararlanarak verileriniz üzerinde çok bir kolayca oluşturabilirsiniz.

Alternatif olarak, bir veri bilimi işlem hattında zenginleştirilmiş verileri kullanmanız gerekiyorsa, yapabilirsiniz [verileri bloblarından Pandas Dataframe'e yüklemek](../machine-learning/team-data-science-process/explore-data-blob.md).

Son olarak, Bilgi Bankası Mağazası'ndan verilerinizi dışarı aktarmak gerekiyorsa, Azure Data Factory, verileri dışarı aktarma ve tercih ettiğiniz veritabanına kavuşmak için bağlayıcı yok. 

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım olarak, ilk bilgi deponuza örnek veriler ve yönergeleri kullanarak oluşturun.

> [!div class="nextstepaction"]
> [Bir Bilgi Bankası deposu oluşturmak nasıl](knowledge-store-howto.md).