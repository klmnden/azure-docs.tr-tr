---
title: Azure App Service’e CDN Ekleme | Microsoft Docs
description: Statik dosyalarınızı, dünya çapındaki müşterilerinize yakın sunucularda önbelleğe almak ve dağıtmak için bir Azure Uygulama Hizmetine Content Delivery Network (CDN) ekleyin.
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 74344b72869ef6b27f9e7329c7a1777a40662b17
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="tutorial-add-a-content-delivery-network-cdn-to-an-azure-app-service"></a>Öğretici: Azure App Service’e Content Delivery Network (CDN) Ekleme

[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md), kullanıcılara içerik teslim etmek için en yüksek aktarım hızını sağlamak üzere stratejik olarak yerleştirilmiş konumlardaki statik web içeriğini önbelleğe alır. CDN, ayrıca web uygulamanızdaki sunucu yükünü de azaltır. Bu öğretici, [Azure Uygulama Hizmeti'ndeki bir web uygulamasına](app-service-web-overview.md) nasıl Azure CDN ekleyeceğinizi gösterir. 

Kullanacağınız örnek statik HTML sitesinin ana sayfası:

![Örnek uygulama ana sayfası](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * CDN uç noktası oluşturma.
> * Önbelleğe alınan varlıkları yenileme.
> * Önbelleğe alınan sürümleri denetlemek için sorgu dizeleri kullanma.
> * CDN uç noktası için özel bir etki alanı kullanma.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Git'i yükleyin](https://git-scm.com/)
- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-web-app"></a>Web uygulaması oluşturma

Kullanacağınız web uygulamasını oluşturmak için, [statik HTML hızlı başlangıç](app-service-web-get-started-html.md) ila **Uygulamaya göz atma** adımlarını izleyin.

### <a name="have-a-custom-domain-ready"></a>Özel bir etki alanına sahip olma

Bu öğreticinin özel etki alanı adımını tamamlamak için, bir özel etki alanına sahip olmanız ve etki alanı sağlayıcınız (GoDaddy gibi) için DNS kayıt defterinize erişiminiz olması gerekir. Örneğin, `contoso.com` ve `www.contoso.com` için DNS girdileri eklemek üzere `contoso.com` kök etki alanının DNS ayarlarını yapılandırmanız gerekir.

Henüz bir etki alanınız yoksa, Azure portalını kullanarak bir etki alanı satın almak için [App Service etki alanı öğreticisini](custom-dns-web-site-buydomains-web-app.md) izlemeniz yararlı olabilir. 

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

Bir tarayıcıyı açın ve [Azure portalına](https://portal.azure.com) gidin.

## <a name="create-a-cdn-profile-and-endpoint"></a>CDN profili ve uç noktası oluşturma

Sol gezinti bölmesinde **Uygulama Hizmetleri**’ni ve sonra [statik HTML hızlı başlangıç](app-service-web-get-started-html.md) içinde oluşturduğunuz uygulamayı seçin.

![Portalda, App Service uygulamasını seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

**App Service** sayfasındaki **Ayarlar** bölümünde, **Ağ > Uygulamanız için Azure CDN'i yapılandırın** seçeneğini belirleyin.

![Portalda CDN’yi seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

**Azure Content Delivery Network** sayfasında, **Yeni uç nokta** ayarlarını tabloda belirtildiği gibi girin.

![Portalda profil ve uç nokta oluşturma](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Ayar | Önerilen değer | Açıklama |
| ------- | --------------- | ----------- |
| **CDN profili** | myCDNProfile | CDN profili oluşturmak için **Yeni oluştur**’u seçin. CDN profili, aynı fiyatlandırma katmanına sahip bir CDN uç noktaları koleksiyonudur. |
| **Fiyatlandırma katmanı** | Standart Akamai | [Fiyatlandırma katmanı](../cdn/cdn-overview.md#azure-cdn-features), sağlayıcıyı ve kullanılabilir özellikleri belirtir. Bu öğreticide, Standart Akamai kullanıyoruz. |
| **CDN uç noktası adı** | azureedge.net etki alanında benzersiz olan tüm adlar | Önbelleğe alınmış kaynaklarınıza *\<uçnoktaadı> .azureedge.net* etki alanından erişebilirsiniz.

**Oluştur**’u seçin.

Azure, profili ve uç noktayı oluşturur. Yeni uç nokta, aynı sayfada bulunan **Uç noktalar** listesinde görünür ve sağlandığında **Çalışıyor** durumundadır.

![Listede yeni uç nokta](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a>CDN uç noktasını sınama

Verizon fiyatlandırma katmanını seçtiyseniz, uç noktayı yayma işlemi genellikle yaklaşık 90 dakika sürer. Akamai için, yayma işlemi birkaç dakika sürer

Örnek uygulama, `index.html` dosyası ile *css*, *img* ve diğer statik varlıkları içeren *js* klasörlerine sahiptir. Bu dosyaların tümünün içerik yolları, CDN uç noktasında aynıdır. Örneğin, aşağıdaki URL'ler *css* klasöründeki *bootstrap.css* dosyasına erişir:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Bir tarayıcıdan aşağıdaki URL’ye gidin:

```
http://<endpointname>.azureedge.net/index.html
```

![CDN'den sunulan örnek uygulama ana sayfası](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 Azure web uygulamasında daha önce çalıştırdığınız aynı sayfayı görürsünüz. Azure CDN, kaynak web uygulamasının varlıklarını almıştır ve CDN uç noktasından bunları sunmaktadır

Bu sayfanın CDN'de önbelleğe alınmasını sağlamak için sayfayı yenileyin. CDN'nin istenen içeriği önbelleğe alması için bazen aynı varlığa ilişkin iki istekte bulunulması gerekir.

Azure CDN profilleri ve uç noktaları oluşturma hakkında daha fazla bilgi için bkz. [Azure CDN kullanmaya başlama](../cdn/cdn-create-new-endpoint.md).

## <a name="purge-the-cdn"></a>CDN’yi temizleme

CDN, yaşam süresi (TTL) yapılandırmasına bağlı olarak kaynak web uygulamasındaki kaynaklarını düzenli aralıklarla yeniler. Varsayılan TTL yedi gündür.

TTL’nin süresi dolmadan önce CDN'yi yenilemeniz gerekebilir (örn. güncelleştirilmiş içeriği web uygulamasına dağıtırken). Yenileme tetiklemek için, CDN kaynaklarını el ile temizleyebilirsiniz. 

Öğreticinin bu bölümünde, web uygulamasına bir değişiklik dağıtabilir ve önbelleğini yenilemek üzere CDN’i tetiklemek için CDN’i temizleyebilirsiniz.

### <a name="deploy-a-change-to-the-web-app"></a>Web uygulamasına değişiklik dağıtma

Aşağıdaki örnekte gösterildiği gibi, `index.html` dosyasını açın ve H1 başlığına "- V2" ekleyin: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Değişikliğinizi uygulayın ve web uygulamasına dağıtın.

```bash
git commit -am "version 2"
git push azure master
```

Dağıtım tamamlandıktan sonra, web uygulaması URL'sine göz atarak değişikliği görebilirsiniz.

```
http://<appname>.azurewebsites.net/index.html
```

![Web uygulamasındaki başlıkta V2](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Ana sayfa için CDN uç noktası URL'sine göz attığınızda, CDN'de önbelleğe alınmış sürümün henüz süresi dolmadığından, değişikliği göremezsiniz. 

```
http://<endpointname>.azureedge.net/index.html
```

![CDN'deki başlıkta V2 yok](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-the-cdn-in-the-portal"></a>Portalda CDN'i temizleme

Önbelleğe alınmış sürümünü güncelleştirmek üzere CDN'i tetiklemek için, CDN'i temizleyin.

Portalın sol gezinti bölümünde, **Kaynak grupları**’nı ve ardından ve web uygulamanız için oluşturduğunuz kaynak grubunu (myResourceGroup) seçin.

![Kaynak grubu seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

Kaynak listesinden CDN uç noktanızı seçin.

![Uç nokta seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

**Uç nokta** sayfasının üst kısmında, **Temizle**'ye tıklayın.

![Temizleme seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Temizlemek istediğiniz içerik yollarını girin. Tek bir dosyayı temizlemek için tam bir dosya yolunu geçirebileceğiniz gibi, bir klasördeki tüm içeriği temizlemek ve yenilemek için bir yol kesimini de geçirebilirsiniz. `index.html` öğesini değiştirdiğinizden, bu öğenin yollardan biri olduğundan emin olun.

Sayfanın alt kısmındaki **Temizle**'yi seçin.

![Sayfayı temizleyin](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a>CDN'in güncelleştirildiğini doğrulayın

Temizleme isteğinin işlenmesi tamamlanana kadar bekleyin. Bu işlem bu genellikle birkaç dakika sürer. Geçerli durumu görmek için, sayfanın üst kısmındaki zil simgesini seçin. 

![Temizleme bildirimi](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

`index.html` için CDN uç noktası URL'sine göz attıktan sonra, ana sayfadaki başlığa eklediğiniz V2'yi görürsünüz. Bu, CDN önbelleğinin yenilendiğini gösterir.

```
http://<endpointname>.azureedge.net/index.html
```

![CDN'deki başlıkta V2](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Daha fazla bilgi için bkz. [Azure CDN uç noktasını temizleme](../cdn/cdn-purge-endpoint.md). 

## <a name="use-query-strings-to-version-content"></a>Sürüm içeriğini belirlemek için sorgu dizelerini kullanın

Azure CDN, aşağıdaki önbelleğe alma davranışı seçeneklerini sunar:

* Sorgu dizelerini yoksay
* Sorgu dizeleri için önbelleğe almayı atla
* Her benzersiz URL'yi önbelleğe al 

Bunlardan ilki varsayılan davranıştır ve URL’de kullanılan sorgu dizesine bakılmaksızın, bir varlığın yalnızca önbelleğe alınmış bir sürümü olduğu anlamına gelir. 

Öğreticinin bu bölümünde, önbelleğe alma davranışını, her benzersiz URL'yi önbelleğe alacak şekilde değiştirme hakkında bilgi edineceksiniz.

### <a name="change-the-cache-behavior"></a>Önbellek davranışını değiştirme

Azure portalında bulunan **CDN Uç Noktası** sayfasında **Önbellek**’i seçin.

**Sorgu dizesi önbellek davranışı** açılan listesinden, **Her benzersiz URL’yi önbelleğe al**’ı seçin.

**Kaydet**’i seçin.

![Sorgu dizesini önbelleğe alma davranışı seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Benzersiz URL'lerin ayrı olarak önbelleğe alındığını doğrulayın

Bir tarayıcıda, CDN uç noktasındaki ana sayfaya gidin, ancak bir sorgu dizesi ekleyin: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

CDN, başlıkta "V2" içeren geçerli web uygulaması içeriğini döndürür. 

Bu sayfanın CDN'de önbelleğe alınmasını sağlamak için sayfayı yenileyin. 

`index.html` öğesini açın, "V2"yi "V3" olarak değiştirin ve değişikliği dağıtın. 

```bash
git commit -am "version 3"
git push azure master
```

Bir tarayıcıda, `q=2` gibi yeni bir sorgu dizesini içeren CDN uç noktası URL'sine gidin. CDN geçerli `index.html` dosyasını alır ve "V3" öğesini görüntüler.  Ancak, `q=1` sorgu dizesini içeren CDN uç noktasına giderseniz "V2" öğesini görürsünüz.

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![CDN'deki başlıkta V3, sorgu dizesi 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![CDN'deki başlıkta V2, sorgu dizesi 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

Bu çıktı, her sorgu dizesinin farklı şekilde değerlendirildiğini gösterir:

* q = 1 daha önce kullanılmıştır, bu nedenle önbelleğe alınan içerikler döndürülür (V2).
* q = 2 yenidir, bu nedenle en son web uygulaması içerikleri alınır ve döndürülür (V3).

Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](../cdn/cdn-query-string.md).

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a>Özel bir etki alanını CDN uç noktası ile eşleme

CNAME kaydı oluşturarak, özel etki alanınızı CDN Uç Noktanız ile eşleyebilirsiniz. CNAME kaydı, bir kaynak etki alanını hedef etki alanına eşleyen bir DNS özelliğidir. Örneğin, `cdn.contoso.com` veya `static.contoso.com` öğesini `contoso.azureedge.net` ile eşleyebilirsiniz.

Özel bir etki alanınız yoksa, Azure portalını kullanarak bir etki alanı satın almak için [App Service etki alanı öğreticisini](custom-dns-web-site-buydomains-web-app.md) izlemeniz yararlı olabilir. 

### <a name="find-the-hostname-to-use-with-the-cname"></a>CNAME ile kullanılacak ana bilgisayar adını bulma

Azure portalının **Uç nokta** sayfasında, sol gezinti bölmesinde **Genel Bakış** öğesinin seçili olduğundan emin olun ve ardından sayfanın üst kısmındaki **+ Özel Etki Alanı** düğmesini seçin.

![Özel Etki Alanı Ekle'yi seçin](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

**Özel alan adı ekle** sayfasında, bir CNAME kaydı oluştururken kullanılacak uç nokta ana bilgisayar adını görürsünüz. Ana bilgisayar adı, CDN uç nokta URL'nizden türetilir: **&lt;UçnoktaAdı>.azureedge.net** . 

![Etki Alanı Ekle sayfası](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a>Etki alanı kayıt şirketinizle CNAME yapılandırma

Etki alanı kayıt şirketinizin web sitesine gidin ve DNS kayıtları oluşturma bölümünü bulun. Bunu, **Etki Alanı Adı**, **DNS** veya **Ad Sunucusu Yönetimi** gibi bir bölümde bulabilirsiniz.

CNAME'leri yönetme ile ilgili bölümü bulun. Gelişmiş ayarlar sayfasına gidip CNAME, Diğer Ad veya Alt Etki Alanları sözcüklerini aramanız gerekebilir.

Seçtiğiniz alt etki alanını (örneğin, **statik** veya **cdn**) portalın başında gösterilen **Uç nokta konak adı** ile eşleyen bir CNAME kaydı oluşturun. 

### <a name="enter-the-custom-domain-in-azure"></a>Azure'de özel etki alanını girme

**Özel etki alanı ekle** sayfasına geri dönün ve iletişim kutusuna alt etki alanıyla birlikte özel etki alanınızı girin. Örneğin, `cdn.contoso.com` girin.   
   
Azure, girdiğiniz alan adı için CNAME kaydının bulunduğunu doğrular. CNAME doğruysa, özel alan adınız doğrulanır.

CNAME kaydını İnternet üzerindeki ad sunucularına yayma işlemi zaman alabilir. Etki alanınız hemen doğrulanmazsa birkaç dakika bekleyin ve tekrar deneyin.

### <a name="test-the-custom-domain"></a>Özel etki alanını sınama

Bir tarayıcıda, sonuçların doğrudan `<endpointname>azureedge.net/index.html` öğesine gidildiğinde aynı doğruluğunu doğrulamak için, özel etki alanınızı (örneğin, `cdn.contoso.com/index.html`) kullanarak `index.html` dosyasına gidin.

![Özel etki alanı URL'si kullanan örnek uygulama ana sayfası](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

Daha fazla bilgi için bkz. [Azure CDN içeriğini özel bir etki alanıyla eşleme](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * CDN uç noktası oluşturma.
> * Önbelleğe alınan varlıkları yenileme.
> * Önbelleğe alınan sürümleri denetlemek için sorgu dizeleri kullanma.
> * CDN uç noktası için özel bir etki alanı kullanma.

CDN performansını nasıl iyileştirebileceğinizi öğrenmek için aşağıdaki makalelere göz atın:

> [!div class="nextstepaction"]
> [Azure CDN’de dosyaları sıkıştırarak performansı geliştirme](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [Azure CDN uç noktasında varlıkları önceden yükleme](../cdn/cdn-preload-endpoint.md)
