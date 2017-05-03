---
title: "Azure App Service’a İçerik Teslim Ağı Ekleme | Microsoft Docs"
description: "Kenar düğümlerinden statik dosyalarınızı teslim etmek için Azure App Service’a bir İçerik Teslim Ağı ekleyin."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 7ba3737566401152a3171e8926beca188045230c
ms.lasthandoff: 04/22/2017

---
# <a name="add-a-content-deliver-network-on-an-azure-app-service"></a>Azure App Service’a İçerik Teslim Ağı Ekleme

Bu öğreticide, uç sunucusundaki statik içeriği ortaya çıkarmak için Azure App Service’ınıza bir Content Delivery Network (CDN) ekleyeceksiniz. En çok 10 CDN Uç Noktasından oluşturulmuş bir koleksiyon olan bir CDN Profili oluşturacaksınız.

Content Delivery Network, kullanıcılara içerik teslim etmek için en yüksek verimliliği sağlamak üzere stratejik olarak yerleştirilmiş konumlardaki statik web içeriğini önbelleğe alır. Web sitesi varlıklarını önbelleğe almak için CDN kullanmanın avantajları şunlardır:

* Özellikle içeriğin yüklenmesi için birden çok gidiş dönüş gerektiren uygulamaların kullanımı sırasında, son kullanıcılar için daha iyi performans ve kullanıcı deneyimi.
* Bir ürün sunumu etkinliğinin başlangıcında olduğu gibi, anlık yüksek düzeyde yükü daha iyi işleyebilmek için büyük ölçeklendirme.
* Kullanıcı isteklerinin dağıtımı ve uç sunuculardan içerik sunulması yoluyla kaynağa daha az trafik gönderilir.

> [!TIP]
> [Azure CDN pop konumlarının](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations) güncel listesini gözden geçirin.
>

## <a name="deploy-the-sample"></a>Örneği dağıtma

Bu öğreticiyi tamamlamak için Web Uygulamasında dağıtılmış bir uygulamaya ihtiyacınız olacaktır. Bu öğreticiye temel oluşturmak için [statik HTML hızlı başlangıcını](app-service-web-get-started-html.md) izleyin.

## <a name="step-1---login-to-azure-portal"></a>Adım 1 - Azure Portalında oturum açın

İlk olarak, sık kullandığınız tarayıcıyı açın ve Azure [Portal](https://portal.azure.com)’a gidin.

## <a name="step-2---create-a-cdn-profile"></a>Adım 2 - CDN Profili oluşturun

Sol gezinti bölmesindeki `+ New` düğmesine ve **Web + Mobil**’e tıklayın. Web + Mobil kategorisi altında **CDN**’yi seçin.

Aşağıdaki alanları belirtin:

| Alan | Örnek değer | Açıklama |
|---|---|---|
| Adı | myCDNProfile | CDN profilinin adı. |
| Konum | Batı Avrupa | Burası CDN profil bilgilerinizin depolanacağı Azure konumdur. CDN uç noktalarına hiç etkisi yoktur. |
| Kaynak grubu | myResourceGroup | Kaynak Grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups) |
| Fiyatlandırma katmanı | Standart Akamai | Fiyatlandırma katmanlarını karşılaştırmak için bkz. [CDN’ye Genel Bakış](../cdn/cdn-overview.md#azure-cdn-features). |

**Oluştur**’a tıklayın.

Sol gezinti bölmesinden kaynak grupları merkezini açıp, **myResourceGroup**’u seçin. Kaynak listesinden **myCDNProfile** öğesini seçin.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Adım 3 - CDN Uç Noktası oluşturun

Arama kutusunun yanındaki komutlardan **+ Uç Nokta** öğesine tıkladığınızda Uç Nokta oluşturma dikey penceresi başlatılır.

Aşağıdaki alanları belirtin:

| Alan | Örnek değer | Açıklama |
|---|---|
| Ad |  | Bu ad, `<endpointname>.azureedge.net` etki alanındaki önbelleğe alınmış kaynaklarınıza erişmek için kullanılır. |
| Çıkış noktası türü | Web Uygulaması | Çıkış noktası türünün seçilmesi, kalan alanlar için size bağlam menüleri sağlar. Özel çıkış noktasının seçilmesi, çıkış noktası konağı için size bir metin alanı sağlar. |
| Çıkış noktası konağı | |  Belirttiğiniz çıkış noktası türündeki tüm kullanılabilir kaynaklar açılır listede listelenir. Çıkış noktası türünüz olarak Özel çıkış noktasını seçtiyseniz, özel çıkış noktanızın etki alanını yazarsınız  |

**Ekle**'ye tıklayın.

Uç Nokta oluşturulur ve İçerik Teslim Ağı uç noktası oluşturulduktan sonra durum **çalışıyor** olarak güncelleştirilir.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---serve-from-azure-cdn"></a>4. Adım - Azure CDN’den hizmet verme

Artık CDN Uç Noktası **çalıştığına** göre, CDN uç noktasından içeriğe erişebiliyor olmalısınız.

Bu öğreticinin temeli olarak [statik HTML hızlı başlangıcını](app-service-web-get-started-html.md) kullandığımızı düşünürsek, CDN’mizde aşağıdaki dosyaların bulunması gerekir: `css`, `img`, `js`.

Web Uygulaması URL’si `http://<app_name>.azurewebsites.net/img/` ile CDN Uç Nokta URL’si `http://<endpointname>.azureedge.net/img/` arasında içerik yolları aynıdır; bu da CDN’den hizmet alması için herhangi bir statik içeriğin yerine CDN Uç Nokta etki alanını kullanabileceğiniz anlamına gelir.

Şimdi sık kullandığınız web tarayıcısında şu url’ye giderek CDN Uç Noktası’ndan ilk görüntümüzü alalım:

```bash
http://<endpointname>.azureedge.net/img/03-enterprise.png
```

Artık statik içerik CDN’de kullanılabilir olduğundan, içeriği son kullanıcıya teslim etmek için CDN uç noktasını kullanacak şekilde uygulamanızı güncelleştirebilirsiniz.

Sitenizin oluşturulduğu dile bağlı olarak, CDN geri dönüşünde yardımcı olacak birçok çerçeve bulunabilir. Örneğin, ASP.NET [paketleme ve küçültme](https://docs.microsoft.com/en-us/aspnet/mvc/overview/performance/bundling-and-minification#using-a-cdn) desteği sağladığı gibi, CDN geri dönüş özelliklerini de etkinleştirir.

Kullandığınız dilde yerleşik olarak veya bir kitaplıkla sağlanan CDN geri dönüş desteği yoksa, [betikleri](https://github.com/dolox/fallback/tree/master/examples/loading-scripts), [stil sayfalarını](https://github.com/dolox/fallback/tree/master/examples/loading-stylesheets) ve [görüntüleri](https://github.com/dolox/fallback/tree/master/examples/loading-images) yüklemeyi destekleyen [FallbackJS](http://fallback.io/) gibi bir javascript çerçevesi kullanabilirsiniz.

## <a name="step-5---purge-the-cdn"></a>5. Adım - CDN’yi temizleme

Bazen yaşam süresi (TTL) dolmadan önce içeriğin kullanım süresini sona erdirmek isterseniz, CDN’de bir temizleme işlemini zorlamanız gerekebilir.

Azure CDN’yi, CDN Profili dikey penceresinden veya CDN Uç Noktası dikey penceresinden el ile temizlemeniz mümkündür. Profil sayfasında temizleme seçeneğini kullanırsanız, hangi uç noktayı temizlemek istediğinizi seçmeniz gerekir.

İçeriği temizlemek için, temizlemek istediğiniz içerik yollarını yazın. Tek bir dosyayı temizlemek için tam bir dosya yolu geçirebileceğiniz gibi, belirli bir klasördeki içeriği temizlemek ve yenilemek için bir yol kesimi de geçirebilirsiniz.

Temizlemek istediğiniz tüm içerik yollarını sağladıktan sonra **Temizle**’ye tıklayın.

![app-service-web-purge-cdn](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

## <a name="step-6---map-a-custom-domain"></a>6. Adım - Özel etki alanını eşleme

CDN uç noktanıza özel bir etki alanı eşlemek, web uygulamanız için tekdüzen bir etki alanı sağlar.

Özel bir etki alanını CDN Uç Noktanıza eşlemek için, etki alanı kayıt şirketinizde bir CNAME kaydı oluşturun.

> [!NOTE]
> CNAME kaydı, `www.contosocdn.com` veya `static.contosocdn.com` gibi bir kaynak etki alanını hedef etki alanına eşleyen bir DNS özelliğidir.

Bizim durumumuzda, bir `static.contosocdn.com` kaynak etki alanı ekleyecek, CDN Uç Noktası olan hedef etki alanına işaret edeceğiz.

| kaynak etki alanı | hedef etki alanı |
|---|---|
| static.contosocdn.com | &lt;endpointname&gt;.azureedge.net |

CDN Uç Noktasına genel bakış dikey penceresinde `+ Custom domain` düğmesine tıklayın.

Özel etki alanı ekle dikey penceresinde, iletişim kutusuna özel etki alanınızı alt etki alanıyla birlikte girin. Örneğin, etki alanı adını `static.contosocdn.com` biçiminde girin.

**Ekle**'ye tıklayın.

## <a name="step-7---version-content"></a>7. Adım - Sürüm içeriği

CDN Uç Noktası sol gezintisinde, Ayarlar başlığı altından **Önbellek**’i seçin.

**Önbellek** dikey penceresi CDN’nin istekteki sorgu dizelerini nasıl işleyeceğini yapılandırmanıza olanak tanır.

> [!NOTE]
> Sorgu dizesi önbellek davranışı seçeneklerinin açıklaması için, [Azure CDN’nin sorgu dizeleriyle önbellek davranışını denetleme](../cdn/cdn-query-string.md) konusuna bakın.

Sorgu dizesi önbellek davranışı için açılan listeden **Her benzersiz URL’yi önbelleğe al**’ı seçin.

**Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure CDN nedir?](../best-practices-cdn.md?toc=%2fazure%2fcdn%2ftoc.json)
* [Azure CDN özel etki alanı üzerinde HTTPS'yi etkinleştirme](../cdn/cdn-custom-ssl.md)
* [Azure CDN’de dosyaları sıkıştırarak performansı geliştirme](../cdn/cdn-improve-performance.md)
* [Azure CDN uç noktasında varlıkları önceden yükleme](../cdn/cdn-preload-endpoint.md)

