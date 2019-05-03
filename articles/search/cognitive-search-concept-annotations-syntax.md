---
title: Ardışık Düzen - Azure Search başvuru girişleri ve çıkışları bilişsel arama
description: Ek açıklama söz dizimi ve açıklamanın girişleri ve çıkışları Azure Search'te bilişsel arama ardışık düzeninde bir beceri kümesi başvurmak nasıl açıklar.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 637edc0e45daa37a753fbaa15313b076e8af4d7c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023870"
---
# <a name="how-to-reference-annotations-in-a-cognitive-search-skillset"></a>Bilişsel arama standartlarındaki şu ek açıklamalarda başvuru yapma

Bu makalede, çeşitli senaryoları göstermek için örnekleri kullanarak yeteneği tanımlarıyla, ek açıklamalarda yapmayı öğrenin. Bir belge içeriğini bir dizi akan açıklamalarla zenginleştirilmiş. Ek açıklamalar için daha da aşağı akış zenginleştirme girdi olarak kullanılan veya bir dizin çıkış alanına eşlenmiş. 
 
Bu makaledeki örneklerde temel *içeriği* tarafından otomatik olarak oluşturulan alan [Azure Blob dizin oluşturucuları](search-howto-indexing-azure-blob-storage.md) belge kırma aşamasının bir parçası olarak. Bir Blob kapsayıcısından belgelere söz konusu olduğunda gibi bir biçim kullanmak `"/document/content"`burada *içeriği* alandır parçası *belge*. 

## <a name="background-concepts"></a>Arka plan kavramları

Söz dizimi gözden geçirilmeden önce bu makalenin sonraki bölümlerinde sağlanan örnekleri daha iyi anlamak için birkaç önemli kavram şimdi yeniden ziyaret edin.

| Sözleşme Dönemi | Açıklama |
|------|-------------|
| Zenginleştirilmiş belge | Zenginleştirilmiş bir belge oluşturulur ve bir belgeyle ilişkili tüm ek açıklamaları tutmak için işlem hattı tarafından kullanılan iç bir yapıdır. Zenginleştirilmiş bir belgenin ek açıklamaları ağaç olarak düşünün. Genel olarak, önceki bir ek açıklamanın oluşturulan bir ek açıklama alt haline gelir.<p/>Zenginleştirilmiş belgeleri beceri yürütmesi süresi boyunca yalnızca mevcut. İçerik arama dizinine eşleştirildikten sonra zenginleştirilmiş belge artık gerekli değildir. Doğrudan zenginleştirilmiş belgelerle etkileşimde bulunmaz olsa da, bir beceri kümesi oluştururken belgeleri zihinsel bir modelini sağlamak kullanışlıdır. |
| Zenginleştirme bağlamı | Zenginleştirme açısından öğesi zenginleştirilmiş bir yerde aldığı bağlamı. Varsayılan olarak zenginleştirme, bağlamıdır `"/document"` tek tek belgeler için kapsamlı düzeyi. Bir beceri çalıştığında, duruma yetenek çıkışlarına [tanımlı bağlamının özellikleri](#example-2).|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Örnek 1: Basit bir ek açıklama başvurusu

Azure Blob Depolama alanında, varlık tanıma ile ayıklamak istediğiniz kişilerin adları başvuruları içeren dosyaları çeşitli olduğunu varsayalım. Aşağıdaki beceri tanımındaki `"/document/content"` tüm belgeyi değerinin metinsel gösterimini ve "Kişiler" ise bir ayıklama kişileri olarak tanımlanan varlıklar için tam ad.

Varsayılan bağlamı olduğundan `"/document"`, kişilerin listesini artık olarak başvurulabilir `"/document/people"`. Bu belirli durumda `"/document/people"` artık bir alanda dizin eşlendi veya başka bir beceri aynı beceri kümesi içinde kullanılan bir ek açıklaması.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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

## <a name="example-2-reference-an-array-within-a-document"></a>Örnek 2: Bir belge içindeki bir dizi başvurusu

Bu örnekte, zenginleştirme adım birden çok kez aynı belge üzerinde çağırmak nasıl gösteren Öncekine üzerine inşa edilmiştir. Önceki örnekte, tek bir belgeden 10 kişi adlarını içeren bir dize dizisi oluşturulan varsayılır. Makul bir sonraki adım, bir tam adı soyadı ayıklar ikinci bir zenginleştirme olabilir. 10 adları olduğundan, bu adım 10 kez bu belgede, her kişi için volat pouze istersiniz. 

Doğru yineleme sayısını çağrılacak bağlamı olarak ayarlanmış `"/document/people/*"`, yıldız işareti (`"*"`) alt öğeleri zenginleştirilmiş belgedeki tüm düğümleri temsil `"/document/people"`. Bu yetenek yalnızca bir kez becerileri dizide tanımlanır olsa da, tüm üyeleri işlenene kadar bu belgedeki her üye için çağrılır.

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

Ek açıklamalar, dizi veya dize koleksiyonları olduğunda, bir bütün olarak dizi yerine belirli üyeleri hedeflemek isteyebilirsiniz. Yukarıdaki örnek adlı bir ek açıklama oluşturur `"last"` bağlam tarafından temsil edilen her düğümü altında. Ek açıklamaları bu aile için başvurmak istiyorsanız, söz dizimi kullanabileceğinizi `"/document/people/*/last"`. Belirli bir ek açıklamaya başvurmak istiyorsanız, bir açık dizin kullanabilirsiniz: `"/document/people/1/last`"belgede tanımlanan ilk kişinin soyadı başvuracak şekilde değiştirin. Bu söz diziminde "dizin 0" dizilerdir dikkat edin.

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Örnek 3: Başvuru üyeleri bir dizi

Bazen belirli bir nitelik geçirilecek belli bir türdeki tüm ek açıklamaları grubu gerekir. Örnek 2'de ayıklanan son adlarından en yaygın Soyadı tanımlayan kuramsal özel bir yetenek göz önünde bulundurun. Özel nitelik yalnızca son adlar sağlamak için bağlamı olarak belirtin. `"/document"` ve giriş olarak `"/document/people/*/lastname"`.

Dikkat edin önem düzeyini `"/document/people/*/lastname"` belge büyük. Bu belge için yalnızca bir belge düğümü varken 10 lastname düğümü olabilir. Bu durumda, sistem bir dizi otomatik olarak oluşturur `"/document/people/*/lastname"` belgedeki tüm öğeleri içeren.

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
+ [Özel bir yetenek bir zenginleştirme komut zinciriyle tümleştirmeyi öğreneceksiniz](cognitive-search-custom-skill-interface.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Nasıl bir dizin zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
