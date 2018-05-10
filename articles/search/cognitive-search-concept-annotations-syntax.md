---
title: Bir açıklama girişleri içinde ve Azure Search'te bilişsel arama ardışık düzeninde çıkış başvurusu | Microsoft Docs
description: Ek açıklamanın sözdizimi ve açıklamanın girişleri ve çıkışları skillset Azure Search'te bilişsel arama ardışık düzeninde başvurmak nasıl açıklanmaktadır.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 2e838e9c94d5b19565bea3d02890fe6164bb37d0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-reference-annotations-in-a-cognitive-search-skillset"></a>Bilişsel arama skillset ek açıklamalar başvuru yapma

Bu makalede, çeşitli senaryolarda göstermek için örnekler kullanarak yetenek tanımları, ek açıklamalar başvurmak nasıl öğrenin. Bir belge içeriğini becerileri bir dizi ile akıp ek açıklamalar ile zenginleştirilmiş. Ek açıklama daha aşağı akış iyileştirmesini için girdi olarak kullanılır ve dizin çıktı alanına eşlenir. 
 
Bu makaledeki örneklerde temel *içerik* tarafından otomatik olarak oluşturulan alan [Azure Blob dizin oluşturucular](search-howto-indexing-azure-blob-storage.md) aşaması çözme belge bir parçası olarak. Bir Blob kapsayıcısından belgelere söz konusu olduğunda gibi bir biçim kullanmak `"/document/content"`, burada *içerik* alandır parçası *belge*. 

## <a name="background-concepts"></a>Arka plan kavramları

Sözdizimi gözden geçirme önce şimdi daha sonra bu makalede sağlanan örnekleri daha iyi anlamak için birkaç önemli kavramları yeniden ziyaret.

| Sözleşme Dönemi | Açıklama |
|------|-------------|
| Zenginleştirilmiş belge | Zenginleştirilmiş bir belge oluşturulur ve bir belgeyle ilgili tüm ek açıklamaları tutmak için ardışık düzen tarafından kullanılan iç bir yapıdır. Zenginleştirilmiş bir belge açıklamalarının ağaç olarak düşünün. Genellikle, önceki bir ek açıklamanın oluşturulan ek açıklama alt haline gelir.<p/>Zenginleştirilmiş belgeleri yalnızca skillset yürütme süresi için mevcut. İçerik arama dizinine eşleştirildikten sonra zenginleştirilmiş belgenin artık gerekli değildir. Zenginleştirilmiş belgeleri ile doğrudan etkileşim yoktur ancak belgeleri zihinsel modelinin bir skillset oluştururken sağlamak kullanışlıdır. |
| İyileştirmesini bağlamı | İyileştirmesini açısından öğesi zenginleştirilmiş yer aldığı bağlamı. Varsayılan olarak, iyileştirmesini bağlam altındadır `"/document"` tek bir belgeye kapsamlı düzeyi. Bir yetenek çalıştığında, hale yetenek çıkışları [tanımlanan bağlam özelliklerini](#example-2).|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Örnek 1: Basit ek açıklama başvurusu

Azure Blob depolama alanına adlandırılmış varlık tanıma kullanarak ayıklamak istediğiniz kişilerin adlarını başvuruları içeren dosyaları, çeşitli olduğunu varsayalım. Aşağıda, yetenek tanımında `"/document/content"` tüm belgeyi metinsel gösterimini ve "Kişiler" bir ayıklama kişileri olarak tanımlanan varlıklar için bir tam ad.

Varsayılan bağlam olduğundan `"/document"`, kişilerin listesi şimdi olarak başvurulabilir `"/document/people"`. Bu belirli durumda `"/document/people"` artık dizin alanına eşlenmesine veya başka bir yetenek aynı skillset içinde kullanılan bir ek açıklamanın olduğu.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```

<a name="example-2"></a>

## <a name="example-2-reference-an-array-within-a-document"></a>Örnek 2: bir belge içinde bir dizi başvuru

Bu örnek bir iyileştirmesini adım birden çok kez aynı belge üzerinde çağırmak nasıl gösteren öncekinin inşa edilmiştir. Önceki örnekte 10 kişiler adlarıyla bir dizeler dizisi tek bir belgeden oluşturulan varsayalım. Makul bir sonraki adım, tam adı soyadı ayıklar ikinci bir iyileştirmesini olabilir. 10 adları olduğundan, 10 kez bu belgede, bir kez her kişi için çağrılması için bu adımı istiyor. 

Yineleme doğru sayıda çağrılacak bağlamı olarak ayarlanmış `"/document/people/*"`, yıldız işareti (`"*"`) zenginleştirilmiş belgedeki tüm düğümleri alt öğeleri temsil eden `"/document/people"`. Bu yetenek yalnızca bir kez becerileri dizide tanımlı rağmen tüm üyeleri işlenene kadar belge içindeki her üye için çağrılır.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the last name from a full name",
    "uri": "http://names.azurewebsites.net/api/GetLastName",
    "context" : "/document/people/*",
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "fullname",
        "source": "/document/people/*"
      }
    ],
    "outputs": [
      {
        "name": "lastname",
        "targetName": "last"
      }
    ]
  }
```

Ek açıklamalar koleksiyonlarını dizeler veya diziler olduğunda, bir bütün olarak dizi yerine belirli üyeleri hedeflemek isteyebilirsiniz. Yukarıdaki örnekte adlı ek açıklama oluşturur `"last"` bağlamı tarafından temsil edilen her düğümü altında. Bu ek açıklamaları ailesine başvurmak istiyorsanız, söz dizimi kullanabilirsiniz `"/document/people/*/last"`. Belirli bir ek açıklama için başvurmak istiyorsanız, açık bir dizini kullanabilir: `"/document/people/1/last`"belgede tanımlanan ilk kişinin soyadını başvurmak için. Bu sözdiziminde "1 dizini" diziler olduğuna dikkat edin.

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Örnek 3: üyeleri bir dizi başvuru

Bazen belirli bir yetenek iletmek için belirli bir türdeki tüm ek açıklamaları gruplandırmanız gerekir. Örnek 2'de ayıklanan son adları en yaygın son adından tanımlayan kuramsal bir özel nitelik göz önünde bulundurun. Özel nitelik yalnızca son adlar sağlamak için bağlamı olarak belirtmek `"/document"` ve giriş olarak `"/document/people/*/lastname"`.

Unutmayın önem düzeyini `"/document/people/*/lastname"` , belgeyi büyük. Bu belge için yalnızca bir belge düğümü durumdayken 10 lastname düğümler olabilir. Bu durumda, sistem bir dizi otomatik olarak oluşturacak `"/document/people/*/lastname"` belgedeki tüm öğeleri içeren.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the most common string from an array of strings",
    "uri": "http://names.azurewebsites.net/api/MostCommonString",
    "context" : "/document",
    "inputs": [
      {
        "name": "strings",
        "source": "/document/people/*/lastname"
      }
    ],
    "outputs": [
      {
        "name": "mostcommon",
        "targetName": "common-lastname"
      }
    ]
  }
```



## <a name="see-also"></a>Ayrıca bkz.
+ [Özel bir yetenek iyileştirmesini ardışık düzenine tümleştirme](cognitive-search-custom-skill-interface.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturma](ref-create-skillset.md)
+ [Bir dizine zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
