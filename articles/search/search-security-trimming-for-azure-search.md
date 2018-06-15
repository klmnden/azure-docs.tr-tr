---
title: Azure arama sonuçlarında kırpma için güvenlik filtreleri | Microsoft Docs
description: Güvenlik filtreleri ve kullanıcı kimliklerini kullanarak Azure Search içeriği üzerinde erişim denetimi.
ms.service: search
ms.topic: conceptual
services: search
ms.date: 08/07/2017
author: revitalbarletz
ms.author: revitalb
manager: jlembicz
ms.openlocfilehash: dd26676b74431566b3631b8a79cd06bcf3022518
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792809"
---
# <a name="security-filters-for-trimming-results-in-azure-search"></a>Azure arama sonuçlarında kırpma için güvenlik filtreleri

Azure Search kullanıcı kimliğine göre arama sonuçlarında kırpma için güvenlik filtre uygulayabilirsiniz. Bu arama deneyimi genellikle aktaranın belge izinlerine sahip ilkeleri içeren bir alana göre arama istekleri kimliği karşılaştırma gerektirir. Bir eşleşme bulunduğunda, kullanıcı veya asıl (örneğin, grubu veya rolüne) bu belgeye erişimi vardır.

Güvenlik elde etmek için tek yönlü filtreleme karmaşık bir eşitlik ifadeleri ayrım olduğu: Örneğin, `Id eq 'id1' or Id eq 'id2'`, vb. Bu yaklaşım hataya, sağlamak zor ve sorgu yanıt süresi çok sayıda saniye olarak, listenin içerdiği yüzlerce veya binlerce değerlerin durumlarda yavaşlatır. 

Daha kolay ve hızlı bir yaklaşım aracılığıyladır `search.in` işlevi. Kullanırsanız `search.in(Id, 'id1, id2, ...')` bir eşitlik ifade yerine saniyeden yanıt bekleyebilirsiniz kez.

Bu makalede, aşağıdaki adımları kullanarak güvenlik filtresi yüklemenin nasıl yapılacağını gösterir:
> [!div class="checklist"]
> * Asıl tanımlayıcıları içeren bir alanı oluşturma 
> * Anında iletme veya varolan belgeleri ile ilgili asıl tanımlayıcıları güncelleştir
> * Bir arama isteği ile sorun `search.in` `filter`

>[!NOTE]
> Asıl tanımlayıcıları alma işlemi bu belgede ele alınmamıştır. Bunu, kimlik hizmetini sağlayıcınızdan almanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sahip olduğunuz varsayılmaktadır bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), [Azure Search Hizmeti](https://docs.microsoft.com/azure/search/search-create-service-portal), ve [Azure Search dizini](https://docs.microsoft.com/azure/search/search-create-index-portal).  

## <a name="create-security-field"></a>Güvenlik alanı oluşturma

Belgelerinizi hangi grupların erişimine sahip belirten bir alan içermelidir. Bu bilgiler göre belgelerin seçili veya vereni döndürülen sonuç kümesinde reddedilen filtre ölçütünü olur.
Güvenli dosyaların bir dizinin sahibiz ve her dosyayı farklı bir kullanıcı kümesini tarafından erişilebilir olduğunu varsayalım.
1. Alan ekleme `group_ids` (burada herhangi bir ad seçebilirsiniz) olarak bir `Collection(Edm.String)`. Alan olduğundan emin olun bir `filterable` özniteliğini `true` arama sonuçları filtrelenmez böylece kullanıcının sahip olduğu erişim tabanlı. Örneğin, ayarlarsanız `group_ids` alanı `["group_id1, group_id2"]` belgeyle için `file_name` "secured_file_b", yalnızca grup kimlikleri "group_id1" veya "group_id2" dosyaya erişim okuma ait kullanıcılar.
   Alanın emin olun `retrievable` özniteliği `false` böylece arama isteğinin bir parçası olarak döndürülmez.
2. Ayrıca ekleyin `file_id` ve `file_name` Bu örnek amacıyla alanlar.  

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

## <a name="pushing-data-into-your-index-using-the-rest-api"></a>REST API kullanarak dizininize veri iletme
  
Dizininizin URL uç noktasına bir HTTP POST isteği gönderin. HTTP isteğinin gövdesi eklenecek belgeleri içeren bir JSON nesnesidir:

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

Var olan bir belgeyi grupları listesini güncelleştirmek gerekiyorsa, kullanabileceğiniz `merge` veya `mergeOrUpload` eylem:

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

Belgeler eklemek veya ilgili tam Ayrıntılar için okuduğunuz [Düzenle belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).
   
## <a name="apply-the-security-filter"></a>Güvenlik filtresini uygulayın

Dayanan belgeler kırpma için `group_ids` erişim, bir arama sorgusuyla vermek bir `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` filtre, burada 'group_id1, group_id2,...' arama isteği veren ait olduğu gruplarıdır.
Bu filtre tüm belgeler için eşleşen `group_ids` alan verilen tanımlayıcıları birini içerir.
Azure Search kullanarak belgeleri arama ile ilgili tam Ayrıntılar için okuduğunuz [Search belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents).
Bu örnek bir POST isteği kullanarak belgeleri aramak nasıl gösterdiğine dikkat edin.

HTTP POST isteği gönderin:

```
POST https://[service name].search.windows.net/indexes/securedfiles/docs/search?api-version=[api-version]  
Content-Type: application/json  
api-key: [admin or query key]
```

Filtre istek gövdesinde belirtin:

```JSON
{
   "filter":"group_ids/any(g:search.in(g, 'group_id1, group_id2'))"  
}
```

Belgeleri alması gereken durumlarda, geri `group_ids` "group_id1" veya "group_id2" içerir. Diğer bir deyişle, istek veren okuma erişimi olan belgeleri alın.

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

Kullanıcı kimliği ve Azure Search bağlı olarak sonuç nasıl filtreleyebilirsiniz budur `search.in()` işlevi. Her hedef belgeyle ilişkili asıl tanımlayıcıları eşleştirilecek isteyen kullanıcı için asıl tanımlayıcıları geçirmek için bu işlevi kullanabilirsiniz. Bir arama isteğine işlendiğinde `search.in` işlevi filtreler arama sonuçları kullanıcının sorumluları hiçbiri olan okuma erişimi. Asıl tanımlayıcıları güvenlik grupları, roller ya da hatta kullanıcının kendi kimliğini gibi şeyleri temsil edebilir.
 
## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search filtreleri kullanarak active Directory kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search'te filtreleri](search-filters.md)
+ [Azure Search işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)