---
title: Azure API Management'te ürün şablonları | Microsoft Docs
description: Azure API Management Geliştirici Portalı ürün sayfalarında içeriğini özelleştirmeyi öğrenin.
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
ms.openlocfilehash: dae757231d8f2ff7fcd8e032d941c0fa9f192796
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23835170"
---
# <a name="product-templates-in-azure-api-management"></a>Azure API Management'te ürün şablonları
Azure API Management Geliştirici portal sayfalarına içeriklerini yapılandırma şablonları kümesini kullanarak içeriği özelleştirme yeteneği sağlar. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) sözdizimi ve düzenleyiciyi, gibi [DotLiquid tasarımcıları için](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), ve sağlanan bir dizi yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [karakter kaynakları](api-management-template-resources.md#glyphs), ve [sayfa denetimleri](api-management-page-controls.md), bu şablonları kullanarak uygun gördüğünüz şekilde sayfaların yapılandırmak için büyük esneklik vardır.  
  
 Bu bölümdeki şablonları Geliştirici Portalı ürün sayfalarında içeriğini özelleştirmenize olanak sağlar.  
  
-   [Ürün Listesi](#ProductList)  
  
-   [Ürün](#Product)  
  
> [!NOTE]
>  Örnek varsayılan şablonları aşağıdaki belgelerde yer alır ancak değişikliği sürekli geliştirmeler nedeniyle tabidir. İstenen tek tek şablonları giderek Geliştirici Portalı'nda Canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="ProductList"></a>Ürün Listesi  
 **Ürün listesi** şablonu Geliştirici portalında ürün listesi sayfasının gövdesi özelleştirmenizi sağlar.  
  
 ![Ürün listesini](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
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
 `Product list` Şablonu, aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [disk belleği denetimi](api-management-page-controls.md#paging-control)  
  
-   [arama denetimi](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfalama|[Disk belleği](api-management-template-data-model-reference.md#Paging) varlık.|Ürünler koleksiyonu için disk belleği bilgileri.|  
|Filtreleme|[Filtreleme](api-management-template-data-model-reference.md#Filtering) varlık.|Ürün Listesi Sayfası için filtre bilgileri.|  
|Ürünler|Koleksiyonu [ürün](api-management-template-data-model-reference.md#Product) varlıklar.|Geçerli kullanıcı için görünür olan ürünleri.|  
  
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
  
##  <a name="Product"></a>Ürün  
 **Ürün** şablonu Geliştirici portalında ürün sayfasının gövdesi özelleştirmenizi sağlar.  
  
 ![Geliştirici Portalı ürün sayfası](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
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
 `Product list` Şablonu, aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [Abone düğmesi](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Ürün|[Ürün](api-management-template-data-model-reference.md#Product)|Belirtilen ürün.|  
|IsDeveloperSubscribed|Boole değeri|Olup geçerli kullanıcının bu ürüne abone olur.|  
|SubscriptionState|Sayı|Abonelik durumu. Olası durumlar şunlardır:<br /><br /> -   `0 - suspended`– Abonelik engellenir ve abone ürünün herhangi bir API çağrılamaz.<br />-   `1 - active`– Aboneliğinizin etkin olduğunu.<br />-   `2 - expired`– Abonelik sona erme tarihini ulaştı ve devre dışı bırakıldı.<br />-   `3 - submitted`– Abonelik isteğinin geliştirici tarafından yapılan ancak henüz onaylanamıyor veya reddedilemiyor.<br />-   `4 - rejected`– Abonelik isteğinin bir yönetici tarafından reddedildi.<br />-   `5 - cancelled`– Abonelik geliştirici veya yönetici tarafından iptal edildi.|  
|Sınırlar|Dizi|Bu özellik kullanım dışıdır ve kullanılmamalıdır.|  
|DelegatedSubscriptionEnabled|Boole değeri|Olup olmadığını [temsilci](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) Bu abonelik için etkin.|  
|DelegatedSubscriptionUrl|Dize|Temsilci etkinleştirilirse, yetkilendirilmiş abonelik URL.|  
|IsAgreed|Boole değeri|Olup ürün koşulları varsa, geçerli kullanıcının koşullarını kabul etmiştir.|  
|Abonelikler|Koleksiyonu [abonelik özeti](api-management-template-data-model-reference.md#SubscriptionSummary) varlıklar.|Ürün abonelikleri.|  
|API'leri|Koleksiyonu [API](api-management-template-data-model-reference.md#API) varlıklar.|Bu ürüne API.|  
|CannotAddBecauseSubscriptionNumberLimitReached|Boole değeri|Geçerli kullanıcının abonelik sınırı açısından bu ürüne abone olmak uygun olup olmadığı.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|Boole değeri|Geçerli kullanıcının bu ürünü veya izin verilmeden birden çok abonelik ile abone olmak uygun olup olmadığı.|  
  
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
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).