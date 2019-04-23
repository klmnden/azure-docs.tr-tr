---
title: Azure API Management'ta ürün şablonları | Microsoft Docs
description: Azure API Management Geliştirici Portalı'nda ürün sayfaların içeriğini özelleştirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 14090e21fb7c6ca07fe63220ffd1d44d483ac869
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443636"
---
# <a name="product-templates-in-azure-api-management"></a>Azure API Management'ta ürün şablonları

Azure API Management içeriklerini yapılandıran bir dizi kullanarak Geliştirici portal sayfalarının içeriğini özelleştirme becerisi sunuyor. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) söz dizimi ve tercih ettiğiniz düzenleyiciyi gibi [tasarımcılarına yönelik DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), ve sağlanan bir dizi yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [karakter Kaynakları](api-management-template-resources.md#glyphs), ve [sayfasında denetimleri](api-management-page-controls.md), sayfaların içeriğini bu şablonları kullanarak dilediğiniz şekilde yapılandırmak için harika esnekliğine sahip olursunuz.  
  
 Bu bölümdeki şablonları, geliştirici portalında ürün sayfalarının içeriğini özelleştirmenizi sağlar.  
  
-   [Ürün Listesi](#ProductList)  
  
-   [Ürün](#Product)  
  
> [!NOTE]
>  Örnek varsayılan şablonları aşağıdaki belgelerde bulunan, ancak sürekli geliştirmeler nedeniyle değiştirilebilir. İstenen bireysel şablonlara giderek Canlı varsayılan şablonları Geliştirici Portalı'nda görüntüleyebilirsiniz. Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="ProductList"></a> Ürün Listesi  
 **Ürün listesi** şablon Geliştirici Portalı ürün listesi sayfasının gövdesi özelleştirmenize olanak sağlar.  
  
 ![Ürün listesini](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<search-control></search-control>  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if products.size > 0 %}  
    <ul class="list-unstyled">  
    {% for product in products %}  
        <li>  
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>  
            {{product.description}}  
        </li>     
    {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 `Product list` Şablon aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [sayfalama denetimi](api-management-page-controls.md#paging-control)  
  
-   [arama denetimi](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Paging|[Disk belleği](api-management-template-data-model-reference.md#Paging) varlık.|Disk belleği bilgiler ürünleri koleksiyonu.|  
|Filtering|[Filtreleme](api-management-template-data-model-reference.md#Filtering) varlık.|Ürün Listesi Sayfası için bir filtre bilgileri.|  
|Products|Koleksiyonu [ürün](api-management-template-data-model-reference.md#Product) varlıklar.|Geçerli kullanıcıya görünür olan ürünleri.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 2,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Filtering": {  
        "Pattern": null,  
        "Placeholder": "Search products"  
    },  
    "Products": [  
        {  
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <a name="Product"></a> Ürün  
 **Ürün** şablon Geliştirici Portalı ürün sayfasının gövdesi özelleştirmenize olanak sağlar.  
  
 ![Geliştirici Portalı ürün sayfası](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a>Denetimler  
 `Product list` Şablon aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [Abone düğmesi](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Product|[Ürün](api-management-template-data-model-reference.md#Product)|Belirtilen ürün.|  
|IsDeveloperSubscribed|boole|Olup geçerli kullanıcının bu ürüne abone olur.|  
|SubscriptionState|number|Abonelik durumu. Olası durumlar şunlardır:<br /><br /> -   `0 - suspended` – Abonelik engellenir ve abone ürünün herhangi bir API çağrılamaz.<br />-   `1 - active` – Aboneliğinizin etkin olduğunu.<br />-   `2 - expired` – Abonelik ulaştı, sona erme tarihine ve devre dışı bırakıldı.<br />-   `3 - submitted` – abonelik isteğini geliştirici tarafından yapılır, ancak henüz onaylanamıyor veya reddedilemiyor.<br />-   `4 - rejected` – bir yönetici tarafından abonelik isteği reddedildi.<br />-   `5 - cancelled` – Abonelik geliştirici veya yönetici tarafından iptal edildi.|  
|Limits|array|Bu özellik, kullanım dışıdır ve kullanılmamalıdır.|  
|DelegatedSubscriptionEnabled|boole|Olmadığını [temsilci](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) Bu abonelik için etkin.|  
|DelegatedSubscriptionUrl|dize|Temsilci etkinleştirilirse, temsilci abonelik URL'si.|  
|IsAgreed|boole|Ürün koşulları varsa, geçerli kullanıcının olup koşulları kabul etmiştir.|  
|Subscriptions|Koleksiyonu [abonelik Özet](api-management-template-data-model-reference.md#SubscriptionSummary) varlıklar.|Abonelikler ürüne.|  
|Apis|Koleksiyonu [API](api-management-template-data-model-reference.md#API) varlıklar.|Bu ürün API'leri.|  
|CannotAddBecauseSubscriptionNumberLimitReached|boole|Geçerli kullanıcının abonelik sınırını ile ilgili bu ürüne abone için uygun olup olmadığı.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|boole|Geçerli kullanıcının veya izin verilmesinden birden çok abonelik ile ilgili bu ürüne abone için uygun olup olmadığı.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](api-management-developer-portal-templates.md).
