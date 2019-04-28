---
title: Sonuçları - Azure Search kırpma için güvenlik filtreleri
description: Güvenlik filtreleri ve kullanıcı kimlikleri kullanarak Azure Search içerik üzerinde erişim denetimi.
ms.service: search
ms.topic: conceptual
services: search
ms.date: 08/07/2017
author: brjohnstmsft
ms.author: brjohnst
manager: jlembicz
ms.custom: seodec2018
ms.openlocfilehash: 326a449d3992d22a4be2d365061c99ef8b13aef9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282402"
---
# <a name="security-filters-for-trimming-results-in-azure-search"></a>Azure Search sonuçlarında kırpma için güvenlik filtreleri

Kullanıcı kimliğine göre Azure Search'te arama sonuçlarını kırpmak için güvenlik filtre uygulayabilirsiniz. Bu arama deneyimi, genellikle yapabilmek aramayı belge izinleri ilkeleri içeren bir alanda karşı kimlik karşılaştırma gerektirir. Bir eşleşme bulunduğunda, kullanıcı veya sorumlusu (örneğin, bir grubu veya rolü) bu belgeye erişimi vardır.

Güvenlik elde etmek için tek yönlü filtreleme karmaşık bir eşitlik ifadeleri ayrım olduğu: Örneğin, `Id eq 'id1' or Id eq 'id2'`, vb. Bu yaklaşım, hata yapmaya açık, Bakımı zordur ve burada listelenmiştir yüzlerce veya binlerce değerleri durumlarda, sorgu yanıt süresini saniye sayısını tarafından yavaşlatır. 

Daha kolay ve hızlı bir yaklaşım aracılığıyladır `search.in` işlevi. Kullanırsanız `search.in(Id, 'id1, id2, ...')` eşitlik ifade yerine, saniyenin altındaki yanıt bekleyebileceğiniz kez.

Bu makalede, aşağıdaki adımları kullanarak güvenlik filtresi nasıl yapılacağını gösterir:
> [!div class="checklist"]
> * Asıl tanımlayıcıları içeren bir alan oluşturun 
> * Anında iletme veya varolan belgeleri ile ilgili asıl tanımlayıcıları güncelleştir
> * Bir arama isteği ile `search.in` `filter`

>[!NOTE]
> Asıl tanımlayıcıları alma işlemi bu belgede ele alınmamaktadır. Bunu, kimlik hizmet sağlayıcınızdan almanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sahip olduğunuz varsayılır bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), [Azure Search Hizmeti](https://docs.microsoft.com/azure/search/search-create-service-portal), ve [Azure Search dizini](https://docs.microsoft.com/azure/search/search-create-index-portal).  

## <a name="create-security-field"></a>Güvenlik alanı oluşturma

Belgelerinizi erişim grupları belirten bir alan içermelidir. Bu bilgiler filtre ölçütlerini karşı belgeleri seçilir veya yayınlayanla döndürülen sonuç kümesinde reddetti haline gelir.
Güvenli dosyaların dizinini sunuyoruz ve her dosya farklı sayıda kullanıcı tarafından erişilebilir olduğunu varsayalım.
1. Alan ekleme `group_ids` (burada herhangi bir ad seçebileceğiniz) olarak bir `Collection(Edm.String)`. Alan olduğundan emin olun bir `filterable` özniteliğini `true` arama sonuçları filtrelenmez, kullanıcının sahip olduğu erişimi bağlı. Örneğin, ayarlarsanız `group_ids` alanı `["group_id1, group_id2"]` belgeyle için `file_name` "secured_file_b", grup kimlikleri "group_id1" veya "group_id2" dosyasına erişimi okuma ait olan kullanıcıların.
   Alanın emin `retrievable` özniteliği `false` böylece arama isteğin bir parçası döndürülmez.
2. Ayrıca `file_id` ve `file_name` açısından bu örnek alanları.  

```JSON
{
    "name": "securedfiles",  
    "fields": [
        {"name": "file_id", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "file_name", "type": "Edm.String"},
        {"name": "group_ids", "type": "Collection(Edm.String)", "filterable": true, "retrievable": false}
    ]
}
```

## <a name="pushing-data-into-your-index-using-the-rest-api"></a>REST API kullanarak dizininize veri gönderme
  
Dizininizin URL uç noktasına bir HTTP POST isteği göndermektir. HTTP isteğinin gövdesi eklenecek belgeleri içeren bir JSON nesnesidir:

```
POST https://[search service].search.windows.net/indexes/securedfiles/docs/index?api-version=[api-version]  
Content-Type: application/json
api-key: [admin key]
```

İstek gövdesinde belgelerinizi içeriğini belirtin:

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "file_id": "1",
            "file_name": "secured_file_a",
            "group_ids": ["group_id1"]
        },
        {
            "@search.action": "upload",
            "file_id": "2",
            "file_name": "secured_file_b",
            "group_ids": ["group_id1", "group_id2"]
        },
        {
            "@search.action": "upload",
            "file_id": "3",
            "file_name": "secured_file_c",
            "group_ids": ["group_id5", "group_id6"]
        }
    ]
}
```

Var olan bir belgeyi gruplarının listesini güncelleştirmeniz gerekirse, kullanabileceğiniz `merge` veya `mergeOrUpload` eylem:

```JSON
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "file_id": "3",
            "group_ids": ["group_id7", "group_id8", "group_id9"]
        }
    ]
}
```

Ekleme veya belgeleri güncelleştirme tüm ayrıntılar için edinebilirsiniz [Düzenle belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).
   
## <a name="apply-the-security-filter"></a>Güvenlik Filtresi Uygula

Trim dayanan belgeler için `group_ids` erişim, bir arama sorgusuyla sorun bir `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` filtre, burada 'group_id1 group_id2...' Arama isteği gönderene ait olduğu gruplarıdır.
Bu filtre tüm belgeler için eşleşen `group_ids` alan belirli tanımlayıcılarından birini içerir.
Kullanarak Azure Search belgeleri arama ile ilgili tam Ayrıntılar için edinebilirsiniz [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents).
Bu örnek, bir POST isteği kullanarak belgelerde arama yapmak nasıl gösterdiğine dikkat edin.

HTTP POST isteği yürütün:

```
POST https://[service name].search.windows.net/indexes/securedfiles/docs/search?api-version=[api-version]  
Content-Type: application/json  
api-key: [admin or query key]
```

Filtre, istek gövdesinde belirtin:

```JSON
{
   "filter":"group_ids/any(g:search.in(g, 'group_id1, group_id2'))"  
}
```

Belgeleri almalısınız geri burada `group_ids` "group_id1" veya "group_id2" içeriyor. Diğer bir deyişle, isteği gönderene okuma erişimi olan belgeleri alın.

```JSON
{
 [
   {
    "@search.score":1.0,
     "file_id":"1",
     "file_name":"secured_file_a",
   },
   {
     "@search.score":1.0,
     "file_id":"2",
     "file_name":"secured_file_b"
   }
 ]
}
```
## <a name="conclusion"></a>Sonuç

Bu kullanıcı kimliği ve Azure Search göre sonuçları nasıl filtre, `search.in()` işlevi. Bu işlev, her hedef belgeyle ilişkili asıl tanımlayıcıları eşleştirilecek isteyen kullanıcı için İlke tanımlayıcıları olarak geçirmek için kullanabilirsiniz. Arama isteği işlendiğinde `search.in` işlevi filtreler arama sonuçları kullanıcının sorumluları hiçbiri olan okuma erişimi. Asıl tanımlayıcıları, güvenlik grupları, roller ya da kullanıcının kendi kimlik gibi şeyleri temsil edebilir.
 
## <a name="see-also"></a>Ayrıca bkz.

+ [Azure arama filtreleri kullanarak active Directory kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search'te filtreler](search-filters.md)
+ [Azure Search işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)