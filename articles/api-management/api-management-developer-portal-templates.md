---
title: "Şablonları kullanarak API Management Geliştirici portalını özelleştirme-Azure | Microsoft Docs"
description: "Şablonları kullanarak Azure API Management Geliştirici Portalı nasıl özelleştireceğinizi öğrenin."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 40d25726d31d2018785b77d169a8811c565316bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Şablonları kullanarak Azure API Management Geliştirici Portalı nasıl özelleştireceğinizi

Azure API Management'ta geliştirici portalını özelleştirmek için kullanılabilecek üç temel yöntem vardır:

* [Statik sayfaların ve sayfa düzeni öğelerinin içeriğini düzenleme][modify-content-layout]
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirmek] [ portal-templates] (Bu kılavuzda açıklanan)

Şablonlar, sistem tarafından oluşturulan Geliştirici portal sayfalarına (örneğin API belgeleri, ürünler, kullanıcı kimlik doğrulaması, vb.) içeriğini özelleştirmek için kullanılır. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) sözdizimi ve yerelleştirilmiş dize kaynakları, simgeler ve sayfa denetimleri, sağlanan bir dizi sayfaların içeriğini uygun gördüğünüz şekilde yapılandırmak için büyük esneklik vardır.

## <a name="developer-portal-templates-overview"></a>Geliştirici Portalı şablonlarına genel bakış
Şablonları düzenleme gelen yapılır **Geliştirici Portalı** yönetici olarak oturum açmış oluştu. Var. Azure Portalı'nı Aç almak ilk ve tıklatın için **yayımcı portalına** , API Management örneğinin hizmet araç çubuğundan.

![Yayımcı portalı][api-management-management-console]

Ardından sağ üst köşedeki **Geliştirici portalı**'na tıklayın. 

![Geliştirici portal menüsü][api-management-developer-portal-menu]

Geliştirici Portalı şablonları erişmek için Özelleştir özelleştirme menüsünü görüntüleme ve'ı tıklatın soldaki simgesini **şablonları**.

![Geliştirici Portalı şablonları][api-management-customize-menu]

Geliştirici Portalı'nda farklı sayfaları kapsayan şablonları çeşitli kategorileri şablonları listesini görüntüler. Her şablon farklıdır, ancak bunları düzenleyebilir ve değişiklikleri yayımlamak için adımları aynıdır. Bir şablonu düzenlemek için şablon adına tıklayın.

![Geliştirici Portalı şablonları][api-management-templates-menu]

Bir şablon tıklamak Bu şablon tarafından özelleştirilebilir Geliştirici Portalı sayfasına götürür. Bu örnekte **ürün listesi** şablonu görüntülenir. **Ürün listesi** şablonu kırmızı dikdörtgeni tarafından belirtilen ekran alanının denetler. 

![Ürün şablonu listesi][api-management-developer-portal-templates-overview]

Bazı şablonlar ister **kullanıcı profili** şablonları, aynı sayfa farklı kısımlarını özelleştirin. 

![Kullanıcı profil şablonları][api-management-user-profile-templates]

Her Geliştirici Portalı şablonu Düzenleyicisi sayfasının en altında görüntülenen iki bölümü vardır. Sol taraftaki şablon düzenleme bölmesi ve şablon için veri modeli ve sağ taraftaki görüntüler. 

Şablon bölmesinde düzenleme görünümü ve davranışı Geliştirici portalında karşılık gelen sayfasının denetimleri biçimlendirme içeriyor. Şablon biçimlendirmede kullanan [DotLiquid](http://dotliquidmarkup.org/) sözdizimi. Bir popüler düzenleyicisidir DotLiquid için [DotLiquid tasarımcıları için](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Düzenleme sırasında şablona yapılan değişiklikler gerçek zamanlı olarak görüntülenen tarayıcıda, ancak dek müşterileriniz için görünür değildir [kaydetmek](#to-save-a-template) ve [yayımlama](#to-publish-a-template) şablonu.

![Şablon biçimlendirme][api-management-template]

**Şablon verileri** bölmesi, belirli bir şablon kullanmak için kullanılabilir varlıklar için veri modeli için bir kılavuz sağlar. Bu kılavuz, Geliştirici Portalı'nda görüntülenmekte olan canlı verileri görüntüleyerek sağlar. Sağ üst köşesindeki dikdörtgen tıklayarak şablon bölmeleri genişletebilirsiniz **şablon verileri** bölmesi.

![Şablon veri modeli][api-management-template-data]

Önceki örnekte içinde görüntülenen verileri alındı Geliştirici Portalı'nda görüntülenen iki ürün yok **şablon verileri** bölmesinde, aşağıdaki örnekte gösterildiği gibi.

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
            "Id": "56ec64c380ed850042060001",
            "Title": "Starter",
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
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

Biçimlendirme **ürün listesi** şablonu bilgileri ve bağlantı için ayrı ayrı her ürünün görüntülenecek ürünleri koleksiyonu üzerinden yineleme tarafından istenen çıkış sağlamak üzere verileri işler. Not `<search-control>` ve `<page-control>` biçimlendirme öğeleri. Bu, arama ve sayfadaki denetimleri disk belleği görünümünü denetler. `ProductsStrings|PageTitleProducts`içeren bir yerelleştirilmiş dize başvuru `h2` sayfa için üstbilgi metni. Dize kaynakları, sayfa denetimleri ve simgeleri Geliştirici Portalı şablonlarındaki kullanıma listesi için bkz [API Management Geliştirici Portalı şablonları başvurusu](api-management-developer-portal-templates-reference.md).

```html
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

## <a name="to-save-a-template"></a>Şablon kaydetmek için
Bir şablonu kaydetmek için şablon Düzenleyicisi'nde Kaydet'i tıklatın.

![Şablonu kaydetme][api-management-save-template]

Bunlar yayımlanan kadar değişiklikleri kaydedildi Geliştirici Portalı'nda Canlı değildir.

## <a name="to-publish-a-template"></a>Bir şablon yayımlamak için
Tek tek veya birlikte kaydedilen şablonları yayımlanabilir. Tıklatın tek tek bir şablonu yayımlamak için şablon Düzenleyicisi'nde yayımlayın.

![Yayın şablonu][api-management-publish-template]

Tıklatın **Evet** onaylamak ve şablonun sağlamak için dinamik Geliştirici Portalı.

![Onayla yayımlama][api-management-publish-template-confirm]

Tüm şu anda yayımdan şablonu sürümleri yayımlamak için tıklatın **Yayımla** şablonları listesinde. Yayımdan şablonları şablonu adından bir yıldız işaretiyle belirtilir. Bu örnekte, **ürün listesi** ve **ürün** şablonları yayımlanır.

![Şablonlarını yayınlama][api-management-publish-templates]

Tıklatın **özelleştirmeleri yayımlamak** onaylamak için.

![Onayla yayımlama][api-management-publish-customizations]

Yeni yayımlanan şablonların Geliştirici Portalı'nda hemen etkili olur.

## <a name="to-revert-a-template-to-the-previous-version"></a>Bir şablonu önceki sürüme geri döndürmek için
Tıklatın bir şablon önceki yayımlanan sürümüne geri dönmek için şablon Düzenleyicisi'nde geri dönün.

![Şablon geri][api-management-revert-template]

Onaylamak için **Evet**’e tıklayın.

![Onayla][api-management-revert-template-confirm]

Geri döndürme işlemi tamamlandıktan sonra önceden yayımlanmış bir şablon Geliştirici Portalı'nda Canlı sürümüdür.

## <a name="to-restore-a-template-to-the-default-version"></a>Bir şablonu varsayılan sürüme geri yüklemek için
Şablonlar kullanıcıların varsayılan sürümüne geri yüklemek iki adımlı bir işlemdir. Şablonları ilk geri gerekir ve ardından geri yüklenen sürümleri yayımlanması gerekir.

Varsayılan sürüm için tek bir şablon geri yüklemek için geri yükleme şablonu Düzenleyicisi'ni tıklatın.

![Şablon geri][api-management-reset-template]

Onaylamak için **Evet**’e tıklayın.

![Onayla][api-management-reset-template-confirm]

Tüm şablonları varsayılan sürümlerine geri yüklemek için **geri varsayılan şablonları** şablon listesinde.

![Şablonları geri yükleme][api-management-restore-templates]

Geri yüklenen şablonları sonra ayrı ayrı veya tümünü bir defada içindeki adımları izleyerek yayımlanmalıdır [şablon yayımlamak için](#to-publish-a-template).

## <a name="next-steps"></a>Sonraki adımlar
Geliştirici Portalı şablonları, dize kaynaklarını, simgeler ve sayfa denetimleri için başvuru bilgileri için bkz: [API Management Geliştirici Portalı şablonları başvurusu](api-management-developer-portal-templates-reference.md).

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-management-console]: ./media/api-management-developer-portal-templates/api-management-management-console.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







