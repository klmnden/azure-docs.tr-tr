---
title: Şablonları kullanarak API Management Geliştirici portalını özelleştirme-Azure | Microsoft Docs
description: Şablonları kullanarak Azure API Management Geliştirici portalını özelleştirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 00d5e3df78e85d19a519786dad1a1b176ad7fa08
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837258"
---
# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Şablonları kullanarak Azure API Management Geliştirici portalını özelleştirme

Azure API Management'ta geliştirici portalını özelleştirmek için kullanılabilecek üç temel yöntem vardır:

* [Statik sayfaların ve sayfa düzeni öğelerinin içeriğini düzenleme][modify-content-layout]
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme] [ portal-templates] (Bu kılavuzda açıklanmıştır)

Şablonları, sistem tarafından oluşturulan Geliştirici Portalı sayfalarının (örneğin, API belgeleri, ürünler, kullanıcı kimlik doğrulaması, vb.) içeriğini özelleştirmek için kullanılır. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) söz dizimi ve yerelleştirilmiş bir dize kaynakları, simgeler ve sayfa denetimleri, sağlanan bir dizi sayfaların içeriğini istediğiniz şekilde yapılandırmak için harika esnekliğine sahip olursunuz.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="developer-portal-templates-overview"></a>Geliştirici portal şablonları genel bakış

Şablonları düzenlemek **Geliştirici Portalı** yönetici olarak oturum açmış oluştu. Var. İlk Azure portalı açık alın ve **Geliştirici Portalı** API Management örneğinizin hizmet araç çubuğundan.

Geliştirici portal şablonları erişmek için soldaki özelleştirme menüsünü görüntüleme ve özelleştirme simgesine tıklayın **şablonları**.

![Geliştirici portal şablonları][api-management-customize-menu]

Şablonları farklı Geliştirici Portalı sayfalarında kapsayan çeşitli kategorileri şablonları listesini görüntüler. Her şablon farklıdır, ancak bunları düzenlemek ve değişiklikleri yayımlamak için adımları aynıdır. Bir şablonu düzenlemek için şablonun adını tıklayın.

![Geliştirici portal şablonları][api-management-templates-menu]

Bir şablon tıkladığınızda, şablon tarafından özelleştirilebilir Geliştirici portal sayfasına yönlendirilirsiniz. Bu örnekte, **ürün listesi** şablonu görüntülenir. **Ürün listesi** şablon kırmızı dikdörtgenin tarafından gösterilen ekran alanı denetler.

![Ürünleri liste şablonu][api-management-developer-portal-templates-overview]

Bazı şablonlar ister **kullanıcı profili** şablonları, farklı bölümleri aynı sayfayı özelleştirme.

![Kullanıcı profili şablonları][api-management-user-profile-templates]

Her geliştirici portal şablonu için düzenleyici sayfasının en altında görüntülenen iki bölümden oluşur. Sol taraftaki şablon düzenleme bölmesi ve şablon için veri modeli sağ tarafı görüntüler.

Şablon bölmesinde düzenleme görünümünü ve davranışını Geliştirici portalında karşılık gelen sayfanın denetimleri biçimlendirmeyi içerir. Şablondaki biçimlendirme kullanan [DotLiquid](http://dotliquidmarkup.org/) söz dizimi. DotLiquid için popüler bir düzenleyici [tasarımcılarına yönelik DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Düzenleme sırasında şablonuna yapılan değişiklikleri gerçek zamanlı olarak görüntülenir tarayıcıda ancak dek müşterileriniz için görünür değildir [Kaydet](#to-save-a-template) ve [yayımlama](#to-publish-a-template) şablonu.

![Şablon biçimlendirme][api-management-template]

**Şablon verileri** bölmesi, belirli bir şablonda kullanılabilir varlıklar için veri modeli için bir kılavuz sağlar. Bu kılavuz, şu anda Geliştirici Portalı'nda görüntülenen canlı verileri görüntüleyerek sağlar. Sağ üst köşesindeki dikdörtgen tıklayarak şablon bölmeleri genişletebilirsiniz **şablon verileri** bölmesi.

![Şablon veri modeli][api-management-template-data]

Önceki örnekte, görüntülenen verileri alındı Geliştirici portalında gösterilen iki ürünü mevcuttur **şablon verileri** bölmesinde, aşağıdaki örnekte gösterildiği gibi:

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

İşaretlemede **ürün listesi** istenen çıkış bilgileri ve bağlantı için her bir ürün görüntülenecek ürünleri koleksiyonu ile Yinelem yaparak sağlamak için veri şablonunu işler. Not `<search-control>` ve `<page-control>` işaretleme öğeleri. Bu sayfadaki denetimleri sayfalama ve arama görünümünü denetler. `ProductsStrings|PageTitleProducts` içeren bir yerelleştirilmiş dize arar: başvuru `h2` sayfa için üstbilgi metni. Dize kaynakları, sayfa denetimleri ve geliştirici portal şablonları için kullanılabilir simgeler listesi için bkz. [API Management Geliştirici portal şablonları başvurusu](api-management-developer-portal-templates-reference.md).

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

## <a name="to-save-a-template"></a>Bir şablonu kaydetmek için
Bir şablonu kaydetmek için şablonu Düzenleyicisi'nde Kaydet'e tıklayın.

![Şablonu kaydetme][api-management-save-template]

Kaydedilen değişiklikleri yayımlanmalarından kadar Geliştirici Portalı'nda Canlı değildir.

## <a name="to-publish-a-template"></a>Şablon yayımlama
Kaydedilen şablonları, ayrı ayrı veya tümünü bir araya yayımlanabilir. Tek bir şablonu yayımlamak için tıklatın şablonu Düzenleyicisi'nde yayımlayın.

![Şablonu yayımlama][api-management-publish-template]

Tıklayın **Evet** onaylamak ve şablonun sağlamak için live Geliştirici portalında.

![Onayla yayımlama][api-management-publish-template-confirm]

Şu anda yayımlanmamış tüm sürümlerine yayımlamak için tıklatın **Yayımla** şablonları listesinde. Yayımlanmamış şablonları şablonu adından bir yıldız işaretiyle belirtilir. Bu örnekte, **ürün listesi** ve **ürün** şablonları yayımlanır.

![Şablonları Yayımlama][api-management-publish-templates]

Tıklayın **Özelleştirmeleri Yayımla** onaylamak için.

![Onayla yayımlama][api-management-publish-customizations]

Yeni yayımlanan şablonların Geliştirici Portalı'nda hemen etkili olur.

## <a name="to-revert-a-template-to-the-previous-version"></a>Bir şablonu önceki sürüme geri döndürmek için
Bir şablonu önceki yayımlanan sürüme geri döndürmek için tıklatın şablonu Düzenleyicisi'nde döndürün.

![Şablon geri döndür][api-management-revert-template]

Onaylamak için **Evet**’e tıklayın.

![Onayla][api-management-revert-template-confirm]

Geri döndürme işlemi tamamlandıktan sonra daha önce yayımlanmış bir şablonu Geliştirici Portalı'nda Canlı sürümüdür.

## <a name="to-restore-a-template-to-the-default-version"></a>Bir şablon için varsayılan sürüm geri yüklemek için
Şablonları, varsayılan sürümüne geri yüklemek iki adımlı bir işlemdir. Şablonları öncelikle geri yüklenmesi gerekir ve ardından geri yüklenen sürümler yayımlanması gerekir.

Tek bir varsayılan sürüm şablonuna geri yüklemek için şablonu Düzenleyicisi'ni geri yükleme'yi tıklatın.

![Şablon geri döndür][api-management-reset-template]

Onaylamak için **Evet**’e tıklayın.

![Onayla][api-management-reset-template-confirm]

Tüm şablonları varsayılan sürümlerine geri yüklemek için **geri varsayılan şablonları** şablonu listesinde.

![Şablonları geri yükleme][api-management-restore-templates]

Geri yüklenen şablonları sonra tek tek veya tek seferde içindeki adımları izleyerek yayımlanmalıdır [şablon yayımlama için](#to-publish-a-template).

## <a name="next-steps"></a>Sonraki adımlar
Geliştirici portal şablonları, dize kaynakları, simgeler ve sayfa denetimleri için başvuru bilgileri için bkz [API Management Geliştirici portal şablonları başvurusu](api-management-developer-portal-templates-reference.md).

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
