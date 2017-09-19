---
title: "API’nizi Azure API Management ile koruma | Microsoft Belgeleri"
description: "API’nizi kotalar ve azaltma (hız sınırlama) ilkeleriyle korumayı öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 300b1d762a61c810dbffde5aaacd8a85f12c9fca
ms.contentlocale: tr-tr
ms.lasthandoff: 09/14/2017

---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>API’nizi, Azure API Management kullanarak hız sınırlarıyla koruma | Microsoft Azure
Bu kılavuz size Azure API Management ile hız sınırı ve kota ilkeleri yapılandırarak arka uç API’niz için koruma eklemenin ne kadar kolay olduğunu gösterir.

Bu öğreticide, geliştiricilerin [Abonelik başına çağrı hızını sınırla](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ve [Abonelik başına kullanım kotası ayarla](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) ilkelerini kullanarak API’nize dakikada en fazla 10 ve haftada maksimum 200 çağrı yapmasını sağlayan “Ücretsiz Deneme” API ürünü oluşturacaksınız. Ardından, API yayımlayacak ve hız sınırı ilkesini test edeceksiniz.

[rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ve [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) ilkeleriyle daha gelişmiş azaltma senaryoları için bkz. [Azure API Management ile gelişmiş istek azaltma](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Ürün oluşturmak için
Bu adımda, abonelik onayı gerektirmeyen bir Ücretsiz Deneme ürünü oluşturacaksınız.

> [!NOTE]
> Önceden yapılandırılmış bir ürününüz varsa ve ürünü bu öğretici için kullanmak istiyorsanız, [Çağrı hızı sınırı ve kota ilkeleri yapılandırma][Configure call rate limit and quota policies]'ya geçebilir ve Ücretsiz Deneme ürünü yerine ürününüzü kullanarak öğreticiyi izleyebilirsiniz.
> 
> 

Kullanmaya başlamak için API Management hizmetiniz için Azure Portal'da **Yayımcı portalı**’na tıklayın.

![Yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'te ilk API'nizi yönetme][Manage your first API in Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

**Ürünler** sayfasını görüntülemek için soldaki **API Management** menüsünde **Ürünler**’e tıklayın.

![Ürün ekleme][api-management-add-product]

**Yeni ürün ekle** iletişim kutusunu görüntülemek için **Ürün ekle**’ye tıklayın.

![Yeni ürün ekleme][api-management-new-product-window]

**Başlık** kutusuna **Ücretsiz Deneme** yazın.

**Açıklama** kutusuna aşağıdaki metni yazın: **Aboneler erişim reddedildikten sonra haftada en fazla 200 çağrı olmak üzere dakikada 10 çağrı yürütebilir.**

API Management ürünleri açık ya da korumalı olabilir. Korumalı ürünlerin kullanılabilmesi için bunlara abone olunmalıdır. Açık ürünler abonelik olmadan kullanılabilir. Bir abonelik gerektiren korumalı ürün oluşturmak için **Abonelik iste**’nin seçili olduğundan emin olun. Bu varsayılan ayardır.

Yöneticinin bu ürüne yapılan abonelik denemelerini gözden geçirip kabul etmesini ya da reddetmesini istiyorsanız **Abonelik onayı iste**’yi seçin. Onay kutusu seçili değilse abonelik girişimleri otomatik olarak onaylanır. Bu örnekteki abonelikler otomatik olarak onaylanmıştır, bu nedenle kutuyu işaretlemeyin.

Geliştirici hesaplarının yeni ürüne birden çok kez abone olmasına izin vermek için **Aynı anda birden çok aboneliğe izin ver** onay kutusunu seçin. Bu öğretici aynı anda birden çok abonelik kullanmaz, bu nedenle onay kutusunu işaretlenmemiş olarak bırakın.

Tüm değerleri girdikten sonra ürünü oluşturmak için **Kaydet**’e tıklayın.

![Ürün eklendi][api-management-product-added]

Varsayılan olarak, yeni ürünleri **Yöneticiler** grubundaki kullanıcılar görür. Biz **Geliştiriciler** grubunu ekleyeceğiz. **Ücretsiz Deneme**’ye ve ardından **Görünürlük** sekmesine tıklayın.

> API Management’te, ürünlerin geliştiricilere görünürlüğünü yönetmek için gruplar kullanılır. Ürünler gruplara görünürlük sağlar ve geliştiriciler ait oldukları gruplar tarafından görünür olan ürünleri görüntüleyip bunlara abone olabilir. Daha fazla bilgi için bkz. [Azure API Management'te grupları oluşturma ve kullanma][How to create and use groups in Azure API Management].
> 
> 

![Geliştiriciler grubu ekleme][api-management-add-developers-group]

**Geliştiriciler** onay kutusunu işaretleyin ve ardından **Kaydet**’e tıklayın.

## <a name="add-api"> </a>Ürüne bir API eklemek için
Öğreticinin bu adımında yeni Ücretsiz Deneme ürününe Echo API’sini ekleyeceğiz.

> Her API Management hizmeti örneği, API Management’i denemek ve hakkında bilgi almak için kullanılabilecek bir Echo API’si ile önceden yapılandırılmış olarak gelir. Daha fazla bilgi için bkz. [Azure API Management'te ilk API'nizi yönetme][Manage your first API in Azure API Management].
> 
> 

Soldaki **API Management** menüsünde **Ürünler**’e tıklayın ve ardından ürünü yapılandırmak için **Ücretsiz Deneme**’ye tıklayın.

![Ürünü yapılandırma][api-management-configure-product]

**Ürüne API ekle**’ye tıklayın.

![Ürüne API ekleme][api-management-add-api]

**Echo API’si** seçeneğini belirleyin ve ardından **Kaydet**’e tıklayın.

![Echo API’si ekleme][api-management-add-echo-api]

## <a name="policies"> </a>Çağrı hızı sınırını ve kota ilkelerini yapılandırmak için
Hız sınırları ve kotalar ilke düzenleyicisinde yapılandırılır. Bu öğreticide ekleyeceğimiz iki ilke [Abonelik başına çağrı hızını sınırlama](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ve [Abonelik başına kullanım kotası ayarlama](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)’dır. Bu ilkeler ürün kapsamında uygulanmalıdır.

Soldaki **API Management** menüsü altında **İlkeler**’e tıklayın. **Ürün** listesinde **Ücretsiz Deneme**’ye tıklayın.

![Ürün ilkesi][api-management-product-policy]

İlke şablonunu içeri aktarmak ve hız sınırı ve kota ilkeleri oluşturmaya başlamak için **İlke Ekle**’ye tıklayın.

![İlke ekleme][api-management-add-policy]

Hız sınırı ve kota ilkeleri gelen ilkeleridir, bu nedenle imleci gelen öğede konumlandırın.

![İlke düzenleyicisi][api-management-policy-editor-inbound]

İlke listesini kaydırın ve **Abonelik başına çağrı hızını sınırla** ilke girişini bulun.

![İlke deyimleri][api-management-limit-policies]

İmleç **gelen** ilke öğesinde konumlandırıldıktan sonra, kendi ilke şablonunu eklemek için **Abonelik başına çağrı hızını sınırla**’nın yanındaki oka tıklayın.

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

Kod parçacığından görebileceğiniz gibi ilke, ürünün API ve işlemleri için sınırlar belirlemeye olanak tanır. Bu öğreticide söz konusu özellik kullanılmayacaktır; bu nedenle aşağıdaki örnekte gösterildiği gibi yalnızca dış **rate-limit** öğesi kalacak şekilde **rate-limit** öğesinin **api** ve **operation** öğelerini silin.

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

Ücretsiz Deneme ürününde izin verilen maksimum çağrı hızı dakikada 10 çağrıdır, bu nedenle **calls** öznitelik değeri olarak **10**, **renewal-period** öznitelik değeri olarak **60** girin.

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

**Abonelik başına kullanım kotasını ayarla** ilkesini yapılandırmak için, imlecinizi **gelen** öğesinde yeni eklenen **rate-limit** öğesinin hemen altına getirin ve ardından **Abonelik başına kullanım kotası ayarla**’nı bulun ve solundaki oka tıklayın.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

**Abonelik başına çağrı sayısını sınırla** ilkesine benzer şekilde, **Abonelik başına kullanım kotası belirle** ilkesi ürünün API ve işlemleri için uç ayarlamaya olanak tanır. Bu öğreticide söz konusu özellik kullanılmayacaktır; bu nedenle aşağıdaki örnekte gösterildiği gibi **quota** öğesinden **api** ve **operation** öğelerini silin.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

Kotalar aralık, bant genişliği ya da hem aralık hem de bant genişliği başına çağrı sayısını temel alabilir. Bu öğreticide, bant genişliği temelinde azaltma yapmıyoruz, bu nedenle **bandwidth** özniteliğini silin.

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

Ücretsiz Deneme ürününde, kota haftada 200 çağrıdır. **calls** öznitelik değeri olarak **200**, **renewal-period** öznitelik değeri olarak **604800** girin.

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> İlke aralıkları saniye cinsinden belirtilir. Bir hafta aralığını hesaplamak üzere, gün sayısını (7) bir gündeki saat sayısıyla (24), çıkan sonucu bir saatteki dakika sayısıyla (60) ve son sonucu bir dakikadaki saniye sayısıyla (60) çarpabilirsiniz: 7 * 24 * 60 * 60 = 604800.
> 
> 

İlke yapılandırmayı tamamladığınızda ilkenin aşağıdaki örnekle eşleşmesi gerekir.

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

İstenen ilkeleri yapılandırdıktan sonra **Kaydet**’e tıklayın.

![İlkeyi kaydetme][api-management-policy-save]

## <a name="publish-product"> </a> Ürünü yayımlamak için
Artık API'ler eklendiğine ve ilkeler yapılandırıldığına göre, geliştiriciler tarafından kullanılması için ürünün yayımlanması gerekir. Soldaki **API Management** menüsünde **Ürünler**’e tıklayın ve ardından ürünü yapılandırmak için **Ücretsiz Deneme**’ye tıklayın.

![Ürünü yapılandırma][api-management-configure-product]

**Yayımla**’ya ve ardından **Evet, yayımla**’ya tıklayarak onaylayın.

![Ürünü yayımlama][api-management-publish-product]

## <a name="subscribe-account"> </a>Bir geliştirici hesabını ürüne abone yapmak için
Artık ürün yayımlandığına göre, ürüne abone olunabilir ve ürün, geliştiriciler tarafından kullanılmaya hazırdır.

> Bir API Management örneğinin yöneticileri otomatik olarak her ürüne abone olur. Bu öğretici adımında, yönetici olmayan geliştirici hesaplarından birini Ücretsiz Deneme ürününe abone yapacağız. Geliştirici hesabınız Yönetici rolünün bir parçası ise, zaten abone olmanıza rağmen bu adımın üzerinden devam edebilirsiniz.
> 
> 

Soldaki **API Management** menüsünde **Kullanıcılar**’a tıklayın ve ardından geliştirici hesabınızın adına tıklayın. Bu örnekte **Clayton Gragg** geliştirici hesabını kullanıyoruz.

![Geliştirici yapılandırma][api-management-configure-developer]

**Abonelik Ekle**’ye tıklayın.

![Abonelik ekleme][api-management-add-subscription-menu]

**Ücretsiz Deneme**’yi seçin ve ardından **Abone ol**’a tıklayın.

![Abonelik ekleme][api-management-add-subscription]

> [!NOTE]
> Bu öğreticide, Ücretsiz Deneme ürünü için aynı anda birden çok abonelik etkin değildir. Etkin olsaydı, aşağıdaki örnekte gösterildiği gibi abonelik adı girmeniz istenirdi.
> 
> 

![Abonelik ekleme][api-management-add-subscription-multiple]

**Abone ol**’a tıkladıktan sonra kullanıcılar **Abonelik** listesinde ürünü görür.

![Abonelik eklendi][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Bir işlem çağırmak ve hız sınırını test etmek için
Ücretsiz Deneme ürünü yayımlanmış ve yapılandırılmış olduğuna göre, bazı işlemler çağırabilir ve hız sınırı ilkesini test edebiliriz.
Sağ üstteki menüde **Geliştirici portalı**’na tıklayarak geliştirici portalına geçin.

![Geliştirici portalı][api-management-developer-portal-menu]

Üstteki menüde **API'ler**’e tıklayın ve **Echo API’si**’ne tıklayın.

![Geliştirici portalı][api-management-developer-portal-api-menu]

**GET Kaynağı**’na ve ardından **Deneyin**’e tıklayın.

![Konsolu açma][api-management-open-console]

Varsayılan parametre değerlerini koruyun ve Ücretsiz Deneme ürünü için abonelik anahtarınızı seçin.

![Abonelik anahtarı][api-management-select-key]

> [!NOTE]
> Birden çok aboneliğiniz varsa, **Ücretsiz Deneme** anahtarını seçtiğinizden emin olun, aksi takdirde önceki adımlarda yapılandırılmış ilkeler etkin olmayacaktır.
> 
> 

**Gönder**’e tıklayın ve ardından yanıtı görüntüleyin. **200 Tamam** **Yanıt durumunu** not edin.

![İşlem sonuçları][api-management-http-get-results]

Dakikada 10 çağrı olan hız sınır ilkesinden daha büyük bir hızda **Gönder**’e tıklayın. Hız sınırı ilkesi aşıldıktan sonra, **429 Çok Fazla İstek** yanıt durumu döndürülür.

![İşlem sonuçları][api-management-http-get-429]

**Yanıt içeriği** yeniden denemeler başarılı olmadan önce kalan aralığı gösterir.

Dakikada 10 çağrılık hız sınırı ilkesi etkinken, hız sınırı aşılmadan önce ürüne yapılan ilk 10 başarılı çağrının sona ermesinin üzerinden 60 saniye geçinceye kadar sonraki çağrılar başarısız olur. Bu örnekte, kalan aralık 54 saniyedir.

## <a name="next-steps"> </a>Sonraki adımlar
* Aşağıdaki videoda hız sınırlarını ve kotaları ayarlama gösterisini izleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How to create and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota

