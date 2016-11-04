---
title: İlk API’nizi Azure API Management’te Yönetme | Microsoft Docs
description: API oluşturmayı ve işlemler eklemeyi öğrenin, API Management’i kullanmaya başlayın.
services: api-management
documentationcenter: ''
author: steved0x
manager: erikre
editor: ''

ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/24/2016
ms.author: sdanie

---
# İlk API’nizi Azure API Management’te Yönetme
## <a name="overview"> </a>Genel Bakış
Bu kılavuz size Azure API Management’i hızlı bir şekilde nasıl kullanmaya başlayacağınızı ve ilk API çağrınızı yapmayı gösterir.

## <a name="concepts"> </a>Azure API Management nedir?
Azure API Management’i bir arka uç almak ve bunu temel alan tam özellikli bir API programını başlatmak için kullanabilirsiniz.

Yaygın senaryolar şunlardır:

* **Mobil altyapıyı koruma**: API anahtarlarına erişim geçişi sağlayarak, azaltma ile DOS saldırılarını önleyerek ya da JWT belirtecini doğrulama gibi gelişmiş güvenlik ilkelerini kullanarak mobil altyapıyı koruyun.
* **ISV iş ortağı ekosistemlerini etkinleştirme**: Geliştirici Portalı üzerinden hızlı iş ortağı ekleyerek ve iş ortağı kullanımı için hazır olmayan dahili uygulamalardan bir API cephesi oluşturarak ISV iş ortağı eko sistemlerini etkinleştirin.
* **Dahili API programı çalıştırma** API’lerin kullanılabilirliği ve son değişikliklerine ilişkin iletişim için kuruluşa merkezi bir konum sağlayarak ve kurumsal hesaplar temelinde erişim geçişi sağlayarak, tümü API ağ geçidi ve arka uç arasında güvenli bir kanalı temel alan dahili API programı çalıştırın.

Sistem aşağıdaki bileşenlerden oluşur:

* **API ağ geçidi** şunları yapan uç noktadır:
  
  * API çağrılarını kabul eder ve bunları arka uçlarınıza yönlendirir.
  * API anahtarları, JWT belirteçleri, sertifikaları ve diğer kimlik bilgilerini doğrular.
  * Kullanım kotalarını ve oran limitlerini uygular.
  * Kod değişiklikleri olmadan API'nizi anında dönüştürür.
  * Ayarlandığında arka uç yanıtlarını önbelleğe kaydeder.
  * Analiz amaçlı çağrı meta verilerini günlüğe kaydeder.
* **Yayımcı portalı** API programınızı ayarladığınız yönetim arabirimidir. Bunu şunlar için kullanın:
  
  * API şeması tanımlama ya da içeri aktarma.
  * API'leri ürünler halinde paketleme.
  * API’lerde kota veya dönüşüm gibi ilkeler ayarlama.
  * Analizlerden öngörüler edinme
  * Kullanıcıları yönetme.
* **Geliştirici portalı**, geliştiricilerin şunları yapabileceği ana web varlığı görevi görür:
  
  * API belgelerini okuma.
  * Etkileşimli konsol üzerinden bir API’yi deneme.
  * Bir hesap oluşturma ve API anahtarlarını almak için abone olma.
  * Kendi kullanımlarına ilişkin analize erişme.

## <a name="create-service-instance"> </a>API Management örneği oluşturma
> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme][Azure Ücretsiz Deneme].
> 
> 

API Management ile çalışmanın ilk adımı bir hizmet örneği oluşturmaktır. [Klasik Azure Portalı][Klasik Azure Portalı]’nda oturum açın ve **Yeni**, **Uygulama Hizmetleri**, **API Management**, **Oluştur**’a tıklayın.

![Yeni API Management örneği][api-management-create-instance-menu]

**URL** için, hizmet URL'sini kullanmak için bir benzersiz bir alt etki alanı adı belirtin.

Hizmet örneğiniz için istediğiniz **Abonelik** ve **Bölge**’yi seçin. Seçimlerinizi yaptıktan sonra **Sonraki** düğmesine tıklayın.

![Yeni API Management hizmeti][api-management-create-instance-step1]

**Contoso Ltd.**’yi **Kuruluş Adı** olarak girin ve e-posta adresinizi **Yönetici E-postası** alanına girin.

> [!NOTE]
> Bu e-posta adresi API Management sisteminden gelen bildirimler için kullanılır. Daha fazla bilgi için bkz. [Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma][Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma].
> 
> 

![Yeni API Management hizmeti][api-management-create-instance-step2]

API Management hizmeti örnekleri üç katmanda kullanılabilir: Geliştirici, Standart ve Premium. Varsayılan olarak, yeni API Management hizmeti örnekleri Geliştirici katmanında oluşturulur. Standart veya Premium katmanı seçmek için **Gelişmiş Ayarlar** onay kutusunu işaretleyin ve çıkan ekranda istediğiniz katmanı seçin.

> [!NOTE]
> Geliştirici Katmanı; geliştirme, test ve yüksek kullanılabilirliğin gerekli görülmediği pilot API programları içindir. Standart ve Premium katmanlarda, daha fazla trafik işlemek için ayrılmış birim sayınızı ölçeklendirebilirsiniz. Standart ve Premium katmanlar, API Management hizmetinize en fazla işlem gücü ve performans sağlar. Bu öğreticiyi herhangi bir katmanı kullanarak tamamlayabilirsiniz. API Management katmanları hakkında daha fazla bilgi için bkz. [API Management fiyatlandırması][API Management fiyatlandırması].
> 
> 

Hizmet örneğinizi oluşturmak için bu onay kutusuna tıklayın.

![Yeni API Management hizmeti][api-management-instance-created]

Hizmet örneği oluşturulduktan sonra bir API oluşturmak ya da içeri aktarmak sonraki adımdır.

## <a name="create-api"> </a>Bir API’yi içeri aktarma
Bir API, istemci uygulamasından çağrılabilen işlemler grubundan oluşur. API işlemleri mevcut web hizmetlerine taşınır.

API'ler el ile oluşturulabilir (ve API’lere işlem eklenebilir) veya içeri aktarılabilir. Bu öğreticide, Microsoft tarafından sağlanan ve Azure üzerinde barındırılan bir örnek hesaplayıcı web hizmeti için API’yi içeri aktaracağız.

> [!NOTE]
> API oluşturma ve işlemleri el ile ekleme yönergeleri için bkz. [API oluşturma](api-management-howto-create-apis.md) ve [API’ye işlem ekleme](api-management-howto-add-operations.md).
> 
> 

API'ler Klasik Azure Portalı aracılığıyla erişilen yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için API Management hizmetiniz için Klasik Azure Portalı'nda **Yönet**’e tıklayın.

![Yayımcı portalı][api-management-management-console]

Hesaplayıcı API’sini içeri aktarmak için, soldaki **API Management** menüsünde **API'ler**’e tıklayın ve ardından **API’yi İçeri Aktar**’a tıklayın.

![API’yi İçeri Aktar düğmesi][api-management-import-api]

Hesaplayıcı API’sini yapılandırmak için aşağıdaki adımları uygulayın:

1. **Kaynak URL**’ye tıklayın, **Belirtim belgesi URL’si** metin kutusuna **http://calcapi.cloudapp.net/calcapi.json** girin ve **Swagger** radyo düğmesine tıklayın.
2. **Web API’si URL soneki** metin kutusuna **calc** yazın. 
3. **Ürünler (isteğe bağlı)** öğesine tıklayın ve **Starter**’ı seçin.
4. API’yi içeri aktarmak için **Kaydet**’e tıklayın.

![Yeni API ekle][api-management-import-new-api]

> [!NOTE]
> **API Management** şu anda içeri aktarma için Swagger belgesinin 1.2 ve 2.0 sürümünü destekler. [Swagger 2.0 belirtimi](http://swagger.io/specification) `host`, `basePath` ve `schemes` özelliklerinin isteğe bağlı olduğunu belirtse dahi, Swagger 2.0 belgeniz bu özellikleri **İÇERMELİDİR**; aksi takdirde içeri aktarılmaz. 
> 
> 

API içeri aktarıldığında, yayımcı portalında API’nin özet sayfası gösterilir.

![API özeti][api-management-imported-api-summary]

API bölümünde birden çok sekme bulunur. **Özet** sekmesi, API hakkında temel ölçümleri ve bilgileri gösterir. [Ayarlar](api-management-howto-create-apis.md#configure-api-settings) sekmesi, API yapılandırmasını görüntülemek ve düzenlemek için kullanılır. [İşlemler](api-management-howto-add-operations.md) sekmesi, API işlemlerini yönetmek için kullanılır. **Güvenlik** sekmesi, Temel kimlik doğrulaması ya da [karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md) kullanarak arka uç sunucusu için ağ geçidi kimlik doğrulamasını yapılandırmak ve [OAuth 2.0 kullanarak kullanıcı kimlik doğrulamasını](api-management-howto-oauth2.md) yapılandırmak üzere kullanılabilir.  **Sorunlar** sekmesi, API'lerinizi kullanan geliştiriciler tarafından bildirilen sorunları görüntülemek için kullanılır. **Ürünler** sekmesi bu API’yi içeren ürünleri yapılandırmak için kullanılır.

Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

* **Başlangıç**
* **Sınırsız**

Bu öğreticide, API içeri aktarılırken Başlangıç ürününe Temel Hesaplayıcı API’si eklenmiştir.

Bir API’ye çağrı yapmak için, geliştiricilerin önce buna erişim imkanı sağlayan bir ürüne abone olması gerekir. Geliştiriciler geliştirici portalında ürünlere abone olabilir ya da yöneticiler geliştiricileri yayım portalında ürünlere abone yapabilir. Öğreticinin önceki adımlarında API Management örneği oluşturduğunuz için siz bir yöneticisiniz, bu nedenle varsayılan olarak her ürüne zaten abone oldunuz.

## <a name="call-operation"> </a>Geliştirici portalından bir işlem çağırma
İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan geliştirici portalından çağrılabilir. Bu öğretici adımında, Temel Hesaplayıcı API’sinin **İki tamsayı ekle** işlemini çağıracaksınız. Yayımcı portalının sağ üst kısmında **Geliştirici Portalı**’na tıklayın.

![Geliştirici portalı][api-management-developer-portal-menu]

Üstteki menüde **API'ler**’e tıklayın ve ardından kullanılabilir tüm işlemleri görmek için **Temel Hesaplayıcı**’ya tıklayın.

![Geliştirici portalı][api-management-developer-portal-calc-api]

Bu işlemi kullanacak geliştiriciler için belgeleri sağlayarak, API ve işlemlerle birlikte, içeri aktarılan örnek açıklamalarını ve parametrelerini not edin. İşlemler el ile eklendiğinde de bu açıklamalar eklenebilir.

**İki tamsayı ekle** işlemini çağırmak için **Deneyin**’e tıklayın.

![Deneyin][api-management-developer-portal-calc-api-console]

Parametreler için bazı değerler girebilir veya varsayılanları tutabilirsiniz, sonra **Gönder**’e tıklayın.

![HTTP Al][api-management-invoke-get]

Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

![Yanıt][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Görünüm analizi
Temel Hesaplayıcı için analizleri görüntülemek üzere, geliştirici portalının sağ üst kısmındaki menüde **Yönet**’i seçerek yayımcı portalına geri dönün.

![Yönet][api-management-manage-menu]

Yayımcı portalı için varsayılan görünüm, API Management örneğinize genel bakış sağlayan **Pano**’dur.

![Pano][api-management-dashboard]

Belirli bir süre için API’nin kullanımına ilişkin belirli ölçümleri görüntülemek üzere fareyi **Temel Hesaplayıcı** grafiğinin üzerine getirin.

> [!NOTE]
> Grafiğinizde satır görmüyorsanız, geliştirici portalına dönün ve API’ye bazı çağrılar yapın, birkaç dakika bekleyip panoya geri dönün.
> 
> 

Görüntülenen ölçümlerin daha büyük bir sürümü dahil olmak üzere API için özet sayfasını görüntülemek isterseniz **Ayrıntıları Görüntüle**’ye tıklayın.

![Analiz][api-management-mouse-over]

![Özet][api-management-api-summary-metrics]

Ayrıntılı ölçümler ve raporlar için soldaki **API Management** menüsünde **Analiz**’i seçin.

![Genel Bakış][api-management-analytics-overview]

**Analiz** bölümünde aşağıdaki dört sekme yer alır:

* **Bir bakışta** sekmesi genel kullanım ve durum ölçümlerinin yanı sıra en iyi geliştiriciler, en iyi ürünler, sık kullanılan API'ler ve sık kullanılan işlemleri gösterir.
* **Kullanım** sekmesi coğrafi bir temsil dahil olmak üzere API çağrıları ve bant genişliğine ilişkin ayrıntılı bir bakış sunar.
* **Durum**sekmesi durum kodları, önbellek başarı oranları, yanıt zamanları ve API ve hizmet yanıt zamanlarına odaklanır.
* **Etkinlik**sekmesi geliştirici, ürün, API ve işleme göre belirli bir etkinliğe ilişkin ayrıntılı raporlar sunar.

## <a name="next-steps"> </a>Sonraki adımlar
* [Oran limitleri ile API’nizi koruma](api-management-howto-product-with-rules.md) hakkında bilgi edinin.

[Azure Ücretsiz Deneme]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[API Management örneği oluşturma]: #create-service-instance
[Bir API oluşturma]: #create-api
[Bir işlem ekleme]: #add-operation
[Ürüne yeni API ekleme]: #add-api-to-product
[API’yi içeren ürüne abone olma]: #subscribe
[Geliştirici Portalı'ndan bir işlem çağırma]: #call-operation
[Görünüm analizi]: #view-analytics
[Sonraki adımlar]: #next-steps


[Azure API Management'ta geliştirici hesaplarını yönetme]: api-management-howto-create-or-invite-developers.md
[API ayarlarını yapılandırma]: api-management-howto-create-apis.md#configure-api-settings
[Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma]: api-management-howto-configure-notifications.md
[Yanıtlar]: api-management-howto-add-operations.md#responses
[Ürün oluşturma ve yayımlama]: api-management-howto-add-products.md
[API Management fiyatlandırması]: http://azure.microsoft.com/pricing/details/api-management/

[Klasik Azure Portalı]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png



<!--HONumber=ago16_HO5-->


