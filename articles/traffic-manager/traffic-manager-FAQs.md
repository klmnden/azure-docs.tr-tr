---
title: Azure Traffic Manager - sık sorulan sorular
description: Bu makalede, Traffic Manager hakkında sık sorulan soruların yanıtlarını sağlar
services: traffic-manager
documentationcenter: ''
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: allensu
ms.openlocfilehash: 640d36649f59842a740b4c12b4e3ab39a6d98c13
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050955"
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager'ın sık sorulan sorular (SSS)

## <a name="traffic-manager-basics"></a>Traffic Manager temelleri

### <a name="what-ip-address-does-traffic-manager-use"></a>Traffic Manager hangi IP adresi kullanıyor mu?

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager DNS düzeyinde çalışır. Uygun hizmet uç noktası istemcilere yönlendirmek için DNS yanıtları gönderir. İstemciler ardından hizmet uç noktası için doğrudan değil Traffic Manager aracılığıyla bağlanın.

Bu nedenle, Traffic Manager, bir uç nokta veya IP adresi için istemcilerin bağlanması için sağlamaz. Hizmetiniz için statik IP adresi isterseniz, değil Traffic Manager'da hizmetine yapılandırılmalıdır.

### <a name="what-types-of-traffic-can-be-routed-using-traffic-manager"></a>Ne tür trafiği olabilir Traffic Manager'ı kullanarak yönlendirilmiş?
İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](../traffic-manager/traffic-manager-how-it-works.md), Azure içindeki veya dışındaki barındırılan hizmet Internet'e herhangi bir Traffic Manager uç noktası olabilir. Bu nedenle, Traffic Manager ayrıca internet'e yönelik olan, bir uç nokta kümesine için genel internet'ten kaynaklanan trafiği yönlendirebilirsiniz. İçinde özel bir ağda uç noktalar varsa (örneğin, bir iç sürümünü [Azure Load Balancer](../load-balancer/load-balancer-overview.md#internalloadbalancer)) veya sahip gibi iç ağlardan DNS istekleri yapabilen kullanıcıları bu trafiği yönlendirmek için Traffic Manager'ı kullanamazsınız.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Traffic Manager, 'Yapışkan' oturumları destekliyor mu?

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager DNS düzeyinde çalışır. Uygun hizmet uç noktası istemcilere yönlendirmek için DNS yanıtları kullanır. İstemciler için hizmet uç noktasını Traffic Manager aracılığıyla değil, doğrudan bağlanır. Bu nedenle, Traffic Manager, istemci ve sunucu arasında HTTP trafiğini görmez.

Ayrıca, özyinelemeli DNS hizmeti istemci tarafından Traffic Manager alınan DNS sorgusu kaynak IP adresini aittir. Bu nedenle, Traffic Manager, tek tek istemcilerin izlemek için bir yol yoktur ve 'Yapışkan' oturumları uygulayamaz. Bu sınırlama, tüm DNS tabanlı trafik yönetimi sistemlerine yaygındır ve Traffic Manager için özel değildir.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Traffic Manager kullanırken bir HTTP hatası neden görüyorum?

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager DNS düzeyinde çalışır. Uygun hizmet uç noktası istemcilere yönlendirmek için DNS yanıtları kullanır. İstemciler ardından hizmet uç noktası için doğrudan değil Traffic Manager aracılığıyla bağlanın. Trafik Yöneticisi değil, bkz: HTTP trafiği istemci ve sunucu arasında yapar. Bu nedenle, gördüğünüz herhangi bir HTTP hatası uygulamanızdan gelen gerekir. İstemci uygulamaya bağlanmak tüm DNS çözüm adımları tam olarak. Traffic Manager uygulama trafiği akış sahip herhangi bir etkileşim dahildir.

Daha fazla araştırma, bu nedenle uygulamada odaklanmanız gerekir.

İstemcinin tarayıcısından gönderilen HTTP ana bilgisayar üst bilgisini, sorunların en yaygın kaynağıdır. Uygulama kullanmakta olduğunuz etki alanı adı için doğru barındırma üst bilgisi kabul edecek şekilde yapılandırıldığından emin olun. Azure App Service kullanarak uç noktalar için bkz. [Traffic Manager'ı kullanarak Azure App Service içinde bir web uygulaması için özel etki alanı adı yapılandırma](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Traffic Manager'ı kullanarak performans etkisi nedir?

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager DNS düzeyinde çalışır. İstemcileri için hizmet uç noktalarınıza doğrudan bağlantı olduğundan, bağlantı kurulduktan sonra Traffic Manager kullanırken sonucunda herhangi bir performans etkisi yoktur.

Traffic Manager DNS düzeyinde uygulamaları ile tümleşik olduğundan, DNS çözümlemesi zincirine eklenecek ek bir DNS araması gerektirir. Traffic Manager'ın etkisi DNS çözümleme süresi kısadır. Traffic Manager ad sunucularından oluşan bir küresel ağ kullanır ve kullandığı [anycast](https://en.wikipedia.org/wiki/Anycast) DNS emin olmak için ağ sorguları her zaman en yakın kullanılabilir ad sunucusuna yönlendirilir. Ayrıca DNS yanıtları önbelleğe alma Traffic Manager kullanılarak oluşturulan ek DNS gecikmenin oturumlarının bir kesir uygulanacağı anlamına gelir.

Performans yöntemi en yakın olan kullanılabilir uç nokta trafiği yönlendirir. Bu yöntemle ilişkili genel performans etkisi çok az olması gerektiğini net sonucudur. Herhangi bir artış DNS gecikme süresi, daha düşük ağ gecikme süresi uç noktasına göre uzaklığı.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Traffic Manager ile hangi uygulama protokolleri kullanabilirim?

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](../traffic-manager/traffic-manager-how-it-works.md), Traffic Manager DNS düzeyinde çalışır. DNS araması tamamlandıktan sonra istemciler uygulama uç noktası için Traffic Manager aracılığıyla değil, doğrudan bağlanır. Bu nedenle, bağlantı herhangi bir uygulama protokolünü kullanabilirsiniz. İzleme protokolün, Traffic Manager TCP seçeneğini belirlerseniz sistem durumu uç nokta izleme yapılabilir herhangi bir uygulama protokolleri kullanmadan. Uygulama protokolü kullanılarak sistem durumu seçerseniz, uç noktanın HTTP veya HTTPS GET isteklerini yanıtlamak mümkün olması gerekir.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Traffic Manager bir 'naked' etki alanı adı ile kullanabilir miyim?

Evet. Bir Azure Traffic Manager profili başvurmak, etki alanı adı apex için bir diğer ad kaydı oluşturmayı öğrenmek için bkz: [apex etki alanı adları ile Traffic Manager'ı desteklemek için bir diğer ad kaydı yapılandırmak](../dns/tutorial-alias-tm.md).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Traffic Manager istemci alt ağ adresi DNS sorgularının işlenirken göz önünde bulundurmaz? 

Evet, ek olarak, DNS sorgusu kaynak IP adresini (genellikle olan IP adresini DNS Çözümleyicisi), aldığı aramaları için coğrafi, performans ve alt ağ yönlendirme yöntemleri gerçekleştirirken, traffic manager ayrıca istemci alt ağ adresi olmadığını göz önünde bulundurur Çözümleyici istekte son kullanıcılar, sorguda dahil edilir.  
Özellikle, [RFC 7871 – DNS sorguları istemci alt](https://tools.ietf.org/html/rfc7871) sağlayan bir [DNS (EDNS0) için uzantı mekanizması](https://tools.ietf.org/html/rfc2671) hangi geçirebilirsiniz istemci alt ağ adresi üzerinde destekleyen Çözümleyicileri.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>DNS TTL'si nedir ve nasıl kullanıcılarımın etkiler mi?

Traffic Manager'ı bir DNS sorgusu gölünüzdeki, yaşam süresi (TTL) olarak adlandırılan yanıtta bir değer ayarlar. Birimi saniye içinde olup, bu değer, DNS Çözümleyicileri aşağı yönde üzerinde ne kadar süreyle bu yanıtı önbelleğe almak için gösterir. DNS Çözümleyicileri bu sonucu önbelleğe garanti edilmez, ancak bu önbelleğe alma bunları sonraki tüm Traffic Manager DNS sunucularına değil önbellek sorgularına yanıt vermesini sağlar. Bu yanıt şu şekilde etkiler:

- daha yüksek bir TTL sunulan sorgularının sayısı Faturalanabilir kullanım olduğundan, bir müşterinin maliyetini azaltabilir Traffic Manager DNS sunucularında kavuşmak sorguların sayısını azaltır.
- daha yüksek bir TTL, büyük olasılıkla bir DNS araması yapmak için gereken süreyi azaltabilir.
- daha yüksek bir TTL verilerinizi Traffic Manager araştırma aracılarına elde edilen en son sistem durumu bilgileri yansıtmaz anlamına gelir.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Traffic Manager yanıtları için TTL, nasıl yüksek veya düşük ayarlayabilir miyim?

Ayarlayabileceğiniz, konumundaki bir profili düzeyi 0 saniye kadar düşük ile 2.147.483.647 saniye olarak yüksek olması için DNS TTL'yi başına (ile uyumlu en büyük aralık [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). TTL değeri 0 aşağı akış DNS Çözümleyicileri sorgu yanıtları önbelleğe alma ve Traffic Manager DNS çözümlemesi için sunucuları ulaşmak için tüm sorguların beklenen anlamına gelir.

### <a name="how-can-i-understand-the-volume-of-queries-coming-to-my-profile"></a>Profilime yakında Sorgu hacmini nasıl anlamak? 

Ölçümler biri tarafından Traffic Manager profili tarafından cevap sorgularının sayısı verilen sağlanır. Profili düzeyi toplama sırasında bu bilgileri elde edebilirsiniz veya, bunu daha fazla sınırlandıramazsınız bakın özel uç noktalar burada döndürülmedi sorguları hacmi bölebilirsiniz. Ayrıca, sorgu yanıt birimi ayarladığınız koşullar aşması durumunda bunu size bildirecek uyarılar ayarlayabilirsiniz. Daha fazla ayrıntı için [Traffic Manager ölçümleri ve Uyarıları](traffic-manager-metrics-alerts.md).

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Traffic Manager coğrafi trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Coğrafi yönlendirme yararlı olduğu bazı kullanım örnekleri nelerdir?

Coğrafi yönlendirme türü herhangi bir senaryoda, bir Azure müşterisi coğrafi bölgelerine bağlı kullanıcıları ayırt etmek gereken yere kullanılabilir. Örneğin, coğrafi trafik yönlendirme yöntemini kullanarak, diğer bölgelerden olanlardan farklı bir kullanıcı deneyimi belirli bölgelerdeki kullanıcılara verebilirsiniz. Başka bir örnek ile gerektiren belirli bir bölgedeki kullanıcılar yalnızca bu bölgede uç noktaları tarafından sunulan yerel veri egemenliği mandates uymak.

### <a name="how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method"></a>Performans yönlendirme yöntemini veya coğrafi yönlendirme yöntemini kullanmanız gerekir nasıl karar verebilirim?

Bu iki popüler yönlendirme yöntemleri arasındaki temel fark olan coğrafi yönlendirme birincil amacı bir coğrafi kullanımını zorunlu tutmaktır gelirken, çağırana, düşük gecikme süresi sağlayan uç nokta trafiği göndermek için birincil amacınız yöntemi yönlendirme performansı böylece bu görüntüleri kasıtlı olarak belirli bir uç noktasına yönlendirebilir, arayanlar için Çit. Coğrafi eşleşme ve daha düşük gecikme süresi arasında bir bağıntı olduğundan her zaman true olmasa çakışma olur. Arayan için gecikme süresi daha iyi bir deneyim sağlayan farklı bir coğrafi uç nokta olabilir ve bu durumda performans yönlendirme kullanıcı için bu endpoint gönderir ancak coğrafi yönlendirme her zaman Gönder bunları için eşlenmiş uç noktasına, coğrafi bölge. Daha fazla işaretini kaldırın, aşağıdaki örnekte - Geographic ile göz önünde bulundurun yapmak için yönlendirmeyi sizin gibi nadir eşlemeleri uç ABD'de ve tüm ABD trafiği uç noktalarına Asya'daki Asya tüm trafiği göndermek yapabilirsiniz. Bu durumda, coğrafi yönlendirme kasıtlı olarak tam olarak ne yapması yapılandırdığınız yapar ve performans iyileştirme önemli bir unsur değildir. 
>[!NOTE]
>Burada, hem performans gerekebilir ve bu senaryoları iç içe geçmiş profiller için coğrafi yönlendirme özelliklerini harika bir seçim olabilir senaryolar olabilir. Örneğin, burada, tüm trafiği Kuzey Amerika uç ABD'de bulunan bir iç içe geçmiş profil göndermek coğrafi yönlendirme ile bir üst profili ayarlama ve performansı en iyi uç noktayı bu kümesi içinde bu trafiği göndermek için yönlendirme kullanın. 

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Coğrafi yönlendirme için Traffic Manager tarafından desteklenen bölgeleri nelerdir?

Traffic Manager tarafından kullanılan ülke/bölge hiyerarşisi bulunabilir [burada](traffic-manager-geographic-regions.md). Bu sayfada değişiklikler ile güncel tutulduğu olsa da, programlama yoluyla da aynı bilgileri kullanarak alabilirsiniz [Azure Traffic Manager REST API'si](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Traffic manager gelen bir kullanıcı burada sorgulama nasıl belirliyor?

Traffic Manager (büyük olasılıkla kullanıcı adına sorgulama yapan bir yerel DNS Çözümleyicisi budur) sorgusu kaynak IP'si bakar ve dahili IP'ye bölge Haritası konumunu belirlemek için kullanır. Bu harita internet değişiklikleri hesaba düzenli olarak güncelleştirilir. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Traffic Manager her durumda kullanıcı tam coğrafi konumu doğru belirleyebilirsiniz sağlanır?

Hayır, Traffic Manager, aşağıdaki nedenlerden dolayı kullanıcının konumuna biz bir DNS sorgusu kaynak IP adresinden Infer coğrafi bölgeyi her zaman karşılık gelir garanti edemez:

- İlk olarak önceki SSS bölümünde açıklandığı gibi görüyoruz kaynak IP adresi, kullanıcı adına arama yapan bir DNS Çözümleyicisi olmasıdır. DNS Çözümleyicisi coğrafi konumunu kullanıcının coğrafi konumu için iyi bir proxy olsa da, farklı DNS çözümleyicisini ve kullanmak için bir müşteri tarafından seçmiş belirli DNS çözümleyicisini ayak izine bağlı da olabilir. Örnek olarak, Malezya içinde bulunan bir müşteri, Singapur, DNS sunucusu bu kullanıcı/cihaz için sorgu çözümleri işlemek için teslim DNS çözümleyicisi hizmeti cihazlarının ayarlarını kullanımda belirtebilirsiniz. Bu durumda, Traffic Manager, Singapur konuma karşılık gelen Çözümleyici'nın IP adresi yalnızca görebilirsiniz. Ayrıca, bu sayfada istemci alt ağ adresi desteği ile ilgili önceki SSS Bölümüne bakın.

- İkinci olarak, Traffic Manager, IP adresi için coğrafi bölgeyi çeviri yapmak için bir iç eşlemesini kullanır. Bu harita doğrulanır ve doğruluğunu artırmak ve gelişen yapısı, hesap için düzenli olarak güncelleştirilir ancak olasılığı vardır hala bilgi tüm IP coğrafi konumunu tam bir temsilini değil adresleri.

###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Bir uç nokta aynı bölgede coğrafi yönlendirme ile yapılandırıldığı bir fiziksel olarak bulunması gerekiyor mu?

Hayır, uç nokta konumu üzerinde bölgeleri için eşlenebilir herhangi bir kısıtlama uygular. Örneğin, ABD Orta Azure bölgesindeki bir uç nokta Hindistan için yöneltilen tüm kullanıcıların sahip olabilir.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Coğrafi bölgede coğrafi yönlendirme yapmak için yapılandırılmayan bir profilde uç noktalarına atayabilirsiniz?

Evet, coğrafi yönlendirme yöntemini profili değilse, kullanabileceğiniz [Azure Traffic Manager REST API'si](https://docs.microsoft.com/rest/api/trafficmanager/) coğrafi bölgeler, bu profilde uç noktalarına atamak için. Coğrafi yönlendirme türü profillerine söz konusu olduğunda, bu yapılandırma yok sayılır. Traffic Manager, böyle bir profil için coğrafi yönlendirme türü daha sonra değiştirirseniz, bu eşlemeleri kullanabilirsiniz.

### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Mevcut bir profili yönlendirme yöntemi için coğrafi değiştirmeye çalıştığınızda bir hata neden alıyorum?

Coğrafi yönlendirme ile bir profili altındaki tüm uç noktaları en az bir bölge eşlenmiş olmalıdır. Mevcut bir profili coğrafi yönlendirme türüne dönüştürmek için önce kullanan kendi uç coğrafi bölgeye ilişkilendirilecek ihtiyacınız [Azure Traffic Manager REST API'si](https://docs.microsoft.com/rest/api/trafficmanager/) yönlendirme türü için coğrafi değiştirmeden önce. Portalı kullanırken, uç noktalarını silin, yönlendirme yöntemini profil için coğrafi olarak değiştirin ve sonra uç noktaları, coğrafi bölge eşleme birlikte ekleyin.

### <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Neden müşteriler etkin coğrafi yönlendirme ile uç noktaları bir profili altındaki yerine iç içe geçmiş profilleri oluşturmanız önemle tavsiye edilir?

Coğrafi yönlendirme yöntemini kullanıyorsanız bir profili içinde tek bir uç nokta için bir bölgeye atanabilir. Uç noktanın ekli alt profil ile iç içe türü değilse, bu uç nokta iyi durumda olmayan, giden trafik Yöneticisi için tüm trafik olmayan her daha iyi gönderme alternatif beri trafiği göndermek devam eder. Atanan bölge "(örneğin, bir uç nokta bölgesi İspanya sahip başka bir uç noktaya yük devretme bunu sağlıksız aşması durumunda kötü gittiğini uç noktası atanmış bir üst" alanının olsa bile trafik Yöneticisi değil başka bir uç noktaya yük devretme işlemi gerçekleştirir. Avrupa atanmış bölgesi vardır). Bu, Traffic Manager profilinde bir müşteri Kurulum olduğunu coğrafi sınırlar uyar emin olmak için gerçekleştirilir. Bir uç nokta sistem durumu kötü olduğunda başka bir uç noktaya yük devretme faydalanmak için coğrafi bölge içindeki birden fazla uç nokta tekil uç noktalarını yerine iç içe geçmiş profiller atanması önerilir. Bu şekilde, iç içe geçmiş alt profilinde bir uç nokta başarısız olursa, trafik için aynı iç içe geçmiş alt profili içinde başka bir uç noktaya yük devretme.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Bu yönlendirme türünü destekleyen bir API sürümü üzerinde herhangi bir kısıtlama var mıdır?

Evet, yalnızca yeni destekler ve API Sürüm 2017-03-01 coğrafi yönlendirme yazın. Herhangi bir eski API sürümü veya oluşturulan coğrafi yönlendirme türü profilleriyle kullanılabilir uç noktalar için coğrafi bölgeyi atayın. Eski bir API sürümü profillerini Azure aboneliğinden alınacak kullanılırsa, coğrafi yönlendirme türünde herhangi bir profil döndürülmez. Üstelik, önceki API sürümlerini kullanırken, herhangi bir profil, uç noktaları bir coğrafi bölgede atamasına sahip olan, coğrafi bölgede atamasını gösterilen yoktur döndürdü.

## <a name="traffic-manager-subnet-traffic-routing-method"></a>Alt traffic Manager trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-subnet-routing-is-useful"></a>Alt ağ yönlendirme yararlı olduğu bazı kullanım örnekleri nelerdir?

Yönlendirme alt ağı için DNS istekleri IP adreslerini kaynak IP tarafından tanımlanan kullanıcılardan oluşan belirli ayarlar teslim deneyimi ayırt etmenize olanak sağlar. Kullanıcılar bir Web sitesine, Kurumsal HQ bağlanıyorsanız gösteren farklı içerik örnek verilebilir. Başka bir olacaktır yalnızca IPv6 kullanıldığında bu ISS'leri alt par performans varsa, yalnızca IPv4 bağlantılarını destekleyen uç noktalarına erişmek için belirli ISS'leri kullanıcıları kısıtlayarak.
Alt ağ yönlendirme yöntemini kullanmak için başka bir nedeni, iç içe geçmiş profil diğer profillerinde birlikte ayarlanır. Örneğin, coğrafi yönlendirme yöntemini, kullanıcılarınızın şirketin coğrafı için kullanmak istediğiniz, ancak farklı yönlendirme yöntemi yapmak istediğiniz belirli bir ISP yaparsanız, profili withy alt ağ yönlendirme yöntemi üst profil olarak sahip ve pro belirli bir alt kullanılacak ISS'den geçersiz kılma Dosya ve standart coğrafi profilini herkes için vardır.

### <a name="how-does-traffic-manager-know-the-ip-address-of-the-end-user"></a>Traffic Manager, son kullanıcının IP adresi nasıl biliyor musunuz?

Son kullanıcı cihazları, bir DNS Çözümleyicisi genellikle gerçekleştirilemeyeceğine ilişkin DNS araması yapmak için kullanın. Bu tür Çözümleyicileri giden IP'si ne Traffic Manager kaynak IP olarak görür ' dir. Ayrıca, alt ağ yönlendirme yöntemini, istekle birlikte geçirildi EDNS0 genişletilmiş istemci alt ağ (ECS) bilgi olup olmadığını görmek için de arar. ECS bilgisinin mevcut olması durumunda, üretim belirlemek için kullanılan adres olmasıdır. ECS bilgi olmaması durumunda, sorgunun kaynak IP yönlendirme amacıyla kullanılır.

### <a name="how-can-i-specify-ip-addresses-when-using-subnet-routing"></a>Nasıl miyim IP adresleri alt ağ yönlendirme kullanırken belirtebilir miyim?

Bir uç nokta ile ilişkilendirmek için IP adreslerini iki yolla belirtilebilir. İlk olarak, başlangıç ve bitiş adresleriyle dört noktalı ondalık sekizlik gösterim (örneğin, 1.2.3.4-5.6.7.8 veya 3.4.5.6-3.4.5.6) aralığını belirtmek için kullanabilirsiniz. İkinci olarak, CIDR gösteriminde (örneğin, 1.2.3.0/24) aralığını belirtmek için kullanabilirsiniz. Her iki gösterimi türü de aralık kümesinde kullanabilirsiniz ve birden çok aralık belirtebilirsiniz. Bazı kısıtlamalar geçerlidir.

-   Her IP yalnızca tek bir uç nokta için eşlenmesi gereken bu yana örtüşme adres aralıklarının sahip olamaz
-   Başlangıç adresi bitiş adresinden daha fazla olamaz
-   CIDR gösteriminin söz konusu olduğunda, IP adresi önce '/', bu aralığın başlangıç adresi olmalıdır (örneğin, 1.2.3.0/24 geçerlidir ancak 1.2.3.4.4/24 değil geçerli)

### <a name="how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing"></a>Nasıl ben bir geri dönüş uç noktası alt ağ yönlendirme kullanırken belirtebilir miyim?

Yönlendirme alt ağı, profilinde, eşlenmiş hiçbir alt ağ ile bir uç nokta varsa diğer uç noktaları ile eşleşmeyen herhangi bir istek için buraya yönlendirilirsiniz. Bir istek gelirse Traffic Manager NXDOMAIN yanıtı döndürür beri geri dönüş bir uç profilinizde yoksa ve tüm uç noktalar ile eşlenmemiş veya uç nokta iyi durumda olmayan ancak bu, bir uç noktaya eşlendiği önemle tavsiye edilir.

### <a name="what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile"></a>Bir alt ağ yönlendirme türü profilinde bir uç nokta devre dışı bırakılırsa ne olur?

Yönlendirme alt ağı ile bir profili varsa, noktayla devre dışı bırakıldı, Traffic Manager uç noktanın ve bunun alt ağ eşlemeleri yok gibi davranır. Kendi IP adresi eşleme ile eşleşen bir sorgu alınır ve uç noktayı devre dışı bırakıldı, Traffic Manager bir geri dönüş uç noktası (hiçbir eşleme ile bir adet) döndürür veya böyle bir uç nokta yoksa NXDOMAIN yanıtı döndürür.

## <a name="traffic-manager-multivalue-traffic-routing-method"></a>Traffic Manager birden çok değerli trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-multivalue-routing-is-useful"></a>Çok değerli yönlendirme yararlı olduğu bazı kullanım örnekleri nelerdir?

Çok değerli yönlendirme birden fazla sağlıklı bir uç nokta tek sorgu yanıt olarak döndürür. Bunun başlıca avantajı, bir uç nokta kötü durumda, istemcinin (Bu değerin, bir Yukarı Akış önbellekten döndürebilir) başka bir DNS arama yapmadan yeniden denemek için daha fazla seçenek olduğunu ' dir. Bu, kapalı kalma süresini en aza indirmek için istediğiniz kullanılabilirlik duyarlı uygulamalar için geçerlidir.
Bir uç nokta "çift hem de IPv4 için bağlantılı" ve IPv6 adresleri ve bağlantı uç noktasına başlattığı zaman gelen seçmek için her iki seçenek arayan vermek istediğiniz çok değerli yönlendirme yöntemi için başka bir kullanılır.

### <a name="how-many-endpoints-are-returned-when-multivalue-routing-is-used"></a>Çok değerli yönlendirme kullanıldığında, kaç tane uç noktaları döndürülür?

Döndürülecek uç noktaları sayısı belirtebilirsiniz ve bir sorgu alındığında birden çok değerli birçok sağlıklı uç noktalar daha fazla döndürür. Bu yapılandırma için en olası değer 10'dur.

### <a name="will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used"></a>Çok değerli yönlendirme kullanıldığında, aynı uç nokta kümesine alırım?

Her sorgu bitiş noktaları aynı dizi döndürdü, garanti edemez. Bu da uç bazıları yanıtta eklenmeyecek hangi noktada sağlıksız geçebilir olgu tarafından etkilenir

## <a name="real-user-measurements"></a>Gerçek Kullanıcı Ölçümleri

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Gerçek kullanıcı ölçümleri kullanmanın avantajları nelerdir?

Performans yönlendirme yöntemini kullandığınızda, Traffic Manager kaynak IP ve EDNS istemci alt (iletilmezse) inceleyerek bağlanmak son kullanıcı için en iyi Azure bölgesini seçer ve ağ gecikme süresi bilgilerinin karşı hizmet denetimi tutar. Gerçek kullanıcı ölçümleri, bu tablo, son kullanıcılarınız için Azure eriştikleri son kullanıcı ağların yeterince kapsayan olmanın yanı sıra bu gecikme süresi tablo katkıda deneyimlerini sağlayarak bu son kullanıcı tabanınızı için geliştirir. Bu, son kullanıcısına akışındaki artırılmış bir doğruluk neden olur.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Gerçek kullanıcı ölçümleri Azure dışı bölge ile kullanabilir miyim?

Gerçek kullanıcı ölçümleri, ölçer ve Azure bölgeleri erişmek için yalnızca gecikme süresine raporlar. Azure olmayan bölgelerde barındırılan uç noktaları ile performans tabanlı yönlendirme kullanıyorsanız, gecikme süresi bilgilerini Bu uç nokta ile ilişkilendirilecek seçtiğinizden temsili Azure bölgesi artırılmış tarafından bu özellikten yararlanabilir.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Hangi yönlendirme yöntemini gerçek kullanıcı ölçümleri fayda sağlar?

Gerçek kullanıcı ölçümleri ek bilgiler kazanılan performans yönlendirme yöntemini kullanan profil için geçerli değildir. Azure portalı üzerinden görüntülediğinizde gerçek kullanıcı ölçümleri bağlantı profillerini tüm kullanılabilir.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>Gerçek kullanıcı ölçümleri her profili ayrı olarak etkinleştirme gerekiyor mu?

Hayır, yalnızca abonelik başına bir kez etkinleştirmeniz gerekir ve gecikme süresi bilgileriyle ölçülür ve bildirilen tüm profiller için kullanılabilir.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Gerçek kullanıcı ölçümleri için Aboneliğimi nasıl kapatırım?

İçin gerçek kullanıcı ölçümleri toplamak ve geri gecikmesi ölçümlerinin, istemci uygulamasından gönderme durdurduğunuzda ilgili ücretler tahakkuk durdurabilirsiniz. Örneğin, ölçüm JavaScript web sayfaları'nda yerleşik JavaScript kaldırarak veya sayfa işlendiğinde kendi çağrılmasına kapatarak bu özelliği kullanmayı bırakabilir.

Ayrıca, anahtarınızı silerek gerçek kullanıcı ölçümleri etkinleştirebilirsiniz. Anahtarı sildikten sonra bu anahtarla Traffic Manager'a gönderilen tüm ölçümleri atılır.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Web sayfaları dışındaki istemci uygulamaları ile gerçek kullanıcı ölçümleri kullanabilirim?

Evet, gerçek kullanıcı ölçümleri, farklı türde son kullanıcı istemcileri toplanan verilerin alımı için tasarlanmıştır. Bu SSS, desteklenen istemci uygulamalarının yeni türleri olarak güncelleştirilecektir.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Web sayfası my gerçek kullanıcı ölçümleri etkin her zaman kaç ölçümleri yapılan oluşturulur?

Gerçek kullanıcı ölçümleri sağlanan JavaScript ölçümü ile birlikte kullandığınızda, her sayfa işleme alınan altı ölçüleri sonuçlanır. Bunlar daha sonra geri Traffic Manager hizmetine bildirilir. Bu özellik, Traffic Manager hizmeti için bildirilen ölçülerin sayısına göre ücretlendirilirsiniz. Kullanıcı ölçümleri alınır ancak rapor edildi önce sayfanıza uzağa giderse, örneğin, bu ölçümleri faturalandırma için dikkate alınmaz.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>My Web sayfasında gerçek kullanıcı ölçümleri komut dosyası çalışmadan önce bir gecikme vardır?

Hayır, komut çağrılmadan önce programlanmış gecikme yoktur.

### <a name="can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Gerçek kullanıcı ölçümleri ölçmek istediğiniz yalnızca Azure bölgeler ile kullanabilir miyim?

Hayır, her zaman çağrılır, altı Azure bölgeleri hizmeti tarafından belirlenen şekilde bir dizi gerçek kullanıcı ölçümleri betik ölçer. Bu değişiklikleri farklı çağrılarını arasında ayarlayın ve çok sayıda böyle çağrılar meydana geldiğinde, farklı Azure bölgelerindeki ölçüm kapsamı yayılır.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Ben, belirli bir sayıya yapılan ölçümleri sayısını sınırlayabilir?

JavaScript sayfanıza ve içinde gömülü ölçüm üzeresiniz tam denetimi üzerinden başlatma ve durdurma kullanmadan olduğunda. Traffic Manager hizmeti ölçülecek Azure bölgelerinin listesi için bir istek alırsa sürece bölge kümesi döndürülür.

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Gerçek kullanıcı ölçümleri bir parçası olarak istemci uygulamamın tarafından alınan ölçümlerin görebilir miyim?

Ölçüm mantığı, istemci uygulamasından çalıştırılan olduğundan, hangi gecikmesi ölçümlerinin görmesini de dahil olmak üzere'olmuyor tam denetimi sizdedir. Traffic Manager, aboneliğinize bağlı anahtarı altındaki alınan ölçümleri birleşik bir görünümünü raporlamaz.

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Traffic Manager tarafından sağlanan ölçüm betiğini değiştirebiliyorum?

Web sayfasındaki katıştırılmış denetim içerikteyken biz kesinlikle, ölçer ve gecikme doğru şekilde rapor emin olmak için ölçüm betiği herhangi bir değişiklik yapmasını önleyin.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Başkalarının kullanmam ile gerçek kullanıcı ölçümleri anahtarını görmek olası olacak mı?

Bir web sayfasına ölçüm betik eklediğinizde başkaları için komut dosyası ve gerçek kullanıcı ölçümleri (RUM) anahtarınızı görmek mümkün olacaktır. Ancak, bu anahtarı abonelik kimliğinizi farklıdır ve yalnızca bu amaçla kullanılmak üzere Traffic Manager'tarafından oluşturulan bilmeniz önemlidir. ÇALIŞTIRMA anahtarınızı bilmek, bir Azure hesabı güvenliği tehlikeye değil.

### <a name="can-others-abuse-my-rum-key"></a>Başkalarının ÇALIŞTIRMA anahtarımı kötüye?

Anahtarınızı yanlış bilgi Azure'a göndermek için kullanmak üzere başkalarını için mümkün olsa da, diğer tüm ölçümler birlikte aldığımız hesabı alınmış olduğundan yönlendirme birkaç yanlış ölçümleri değiştirmez. Anahtarlarınızı değiştirmeniz gerekiyorsa, bu noktada, eski anahtarı atılan olur anahtarı yeniden oluşturabilirsiniz.

### <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>JavaScript ölçüm tüm web sayfalarımı put gerekiyor mu?

Gerçek kullanıcı ölçümleri ölçümleri artış sayısı arttıkça daha fazla değer sunar. Bununla birlikte, bu kararınız dair birkaç tüm web sayfalarını veya select koymak ihtiyacınız olur. Bizim önerimiz, en çok ziyaret edilen sayfanızda beş saniye veya daha fazla söz konusu sayfada kalmak için bir kullanıcı burada beklenen koyarak başlamaktır.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Gerçek kullanıcı ölçümleri kullanırsam son Kullanıcılarım hakkında bilgi Traffic Manager tarafından tanımlanabilir?

Traffic Manager, belirtilen ölçüm JavaScript kullanıldığında, son kullanıcının istemci IP adresi ve kullandıkları yerel DNS Çözümleyicisi kaynak IP adresini görünürlük sahip olur. Traffic Manager, yalnızca bu belirli ölçümleri gönderen kullanıcıdan tanımlamak yükleyemeyecektir kesilmiş atandıktan sonra istemci IP adresini kullanır.

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>Ölçüm gerçek kullanıcı ölçümleri, Traffic Manager yönlendirme için kullanılmasını gerek Web sayfası mu?

Hayır, bu Traffic Manager'ı kullanmanız gerekmez. Yönlendirme yan Traffic Manager'a gerçek kullanıcı ölçüm bölümünden ayrı olarak çalışır ve her ikisinin de aynı web özelliği için harika bir fikriniz olmasına rağmen bunlar olmanız gerekmez.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>Gerçek kullanıcı ölçümleri ile kullanılacak Azure bölgeleri herhangi bir hizmeti ana bilgisayar gerekiyor mu?

Hayır, herhangi bir sunucu tarafı bileşeni çalışmak gerçek kullanıcı ölçümleri için azure'da barındırmak gerekmez. Tek pikselli bir görüntü JavaScript ölçümü tarafından indirilir ve farklı Azure bölgelerinde çalıştırılan hizmet barındırılan ve Azure tarafından yönetilen. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Gerçek kullanıcı ölçümleri kullandığımda Azure bant genişliği kullanımı artırır?

Önceki yanıt belirtildiği gibi sunucu tarafı bileşenlerini gerçek kullanıcı ölçümleri sahip olduğu ve Azure tarafından yönetilen. Bu, gerçek kullanıcı ölçümleri kullandığından, Azure bant genişliği kullanımını artırmaz anlamına gelir. Bu, hangi Azure ücretleri dışında herhangi bir bant genişliği kullanımını içermez. Bir Azure bölgesine gecikme ölçüm için yalnızca bir tek pikselli bir görüntü yükleyerek kullanılan bant genişliğini en aza indirin. 

## <a name="traffic-view"></a>Trafik Görünümü

### <a name="what-does-traffic-view-do"></a>Trafik görünümü ne yapar?

Trafik görünümü trafiği kullanıcılarınız ve onların deneyimini nasıl olduğu hakkında daha fazla anlamanıza yardımcı olan Manager'ın bir özelliğidir. Traffic Manager ve şu sağlamak için hizmet tutar ağ gecikme süresi zeka tablolarını tarafından. alınan sorgular kullanır:

- Burada Azure uç noktalarınıza kullanıcılarınızın bağlanmasını bölgeleri.
- Bu bölgelerden bağlanan kullanıcıları hacmi.
- Azure bölgeleri için bunlar yönlendirilir.
- Bu Azure bölgeleri için kendi gecikme deneyimi.

Bu bilgiler, coğrafi harita katmanını ve indirmek ham verileri olarak almalarının yanı sıra portaldaki tablosal görünümleri aracılığıyla kullanmak kullanılabilir.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Trafik görünümü kullanarak nasıl yararlanabilir?

Trafik görünümü, Traffic Manager profillerini almak trafiğin genel görünümünü sunar. Özellikle, burada gelen kullanıcı tabanınızı bağlanır ve eşit olarak önemlisi ortalama gecikme süresi deneyimlerini nedir anlamak için kullanılabilir. Ardından, bu kullanıcıların daha düşük gecikme süresi ile hizmet verebilen bir bölgeye Azure kaplama alanınızı genişleterek odaklanmanıza gerek alanlar bulmak için bu bilgileri kullanabilirsiniz. Trafik görünümü kullanarak türetebilirsiniz başka bir öngörü trafiğini sırayla bu bölgelerde stok artan veya azalan kararları vermenize yardımcı olabilecek farklı bölgelere desenlerini görmektir.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Trafik görünümü Azure İzleyici kullanılabilir olan bir Traffic Manager ölçümleri farklı mı?

Azure İzleyici, bir toplama düzeyinde profilinizi ve kendi uç tarafından alınan trafik anlamak için kullanılabilir. Ayrıca, sistem durumu uç nokta sistem durumu denetimi sonuçlarını göstererek izlemenize olanak sağlar. Bunlar gidin ve bölge düzeyinde azure'a bağlanma, son kullanıcı deneyimini anlamak için ihtiyacınız olduğunda, elde etmek için trafik görünümü kullanılabilir.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>Trafik görünümü EDNS istemci alt bilgi kullanıyor mu?

DNS sorgularının Azure Traffic Manager tarafından sunulan, yönlendirme doğruluğunu artırmak için ECS bilgileri göz önünde bulundurun. Ancak burada kullanıcıları bağlanırken kullandığınız gösteren veri kümesi oluştururken, trafik görünümü yalnızca DNS Çözümleyicisi IP adresini kullanıyor.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>Trafik görünümü verileri kaç gün kullanıyor mu?

Trafik görünümü, sizin tarafınızdan görüntülendiğinde, önceki gün önceki yedi güne ait verileri işleyerek çıktısını oluşturur. Bu değişken bir ve en son verileri ziyaret ettiğiniz her saat kullanılacaktır.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Trafik görünümü, dış uç noktalar nasıl işliyor?

Dış uç noktaları Traffic Manager profili Azure bölgelerinde dışında barındırılan kullandığınızda bir ara sunucu gecikme süresi özelliklerini (performans yönlendirme yöntemini kullanıyorsanız bu aslında gereklidir) için olan bir Azure bölgesine eşlenmiş seçim yapabilirsiniz. Bu Azure bölgesi eşlemesi varsa, bu Azure bölgesinin gecikme süresi ölçülerini trafik görünümü çıkış oluşturulurken kullanılır. Azure bölge belirtilirse, gecikme süresi bilgileriyle bu dış uç noktalar verilerinde boş olacaktır.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>Aboneliğimi her profilinde için trafik görünümünü etkinleştir gerekiyor mu?

Önizleme dönemi boyunca, abonelik düzeyinde trafik görünümü etkinleştirildi. Genel kullanılabilirlik önce yaptığımız geliştirmelerin bir parçası olarak, böylece daha ayrıntılı bu özelliği etkinleştirmek trafik görünümü profili düzeyinde etkinleştirebilirsiniz. Varsayılan olarak, bir profil için trafik görünümü devre dışı bırakılacak.

>[!NOTE]
>Önizleme süresi abonelik düzeyinde trafik görünümü etkinleştirilirse, artık her profil söz konusu abonelik altında yeniden etkinleştirmeniz gerekir.
 
### <a name="how-can-i-turn-off-traffic-view"></a>Trafik görünümü devre dışı nasıl kapatabilir miyim?

Portalı veya REST API'sini kullanarak herhangi bir profil için trafik görünümünü açabilirsiniz. 

### <a name="how-does-traffic-view-billing-work"></a>Trafik görünümü faturalandırması nasıl çalışır?

Trafik görünümü fiyatlandırma, çıktı oluşturmak için kullanılan veri noktalarının sayısına bağlıdır. Şu anda desteklenen tek veri profilinizi alan sorguları türüdür. Ayrıca, yalnızca trafik görünümü etkin olduğunda, bitti dedik işleme için faturalandırılırsınız. Bu, bazı zaman dilimi ayda trafik görünümünü etkinleştir ve diğer zamanlarda kapatmak, yalnızca özellik almışken işlenen veri noktası sayısı, faturanıza doğrultusunda etkin, anlamına gelir.

## <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Traffic Manager uç noktaları birden çok abonelik ile kullanabilir?

Birden çok Abonelikteki uç noktaları kullanarak, Azure Web Apps ile mümkün değildir. Azure Web Apps, Web Apps ile kullanılan herhangi bir özel etki alanı adı yalnızca tek bir abonelik içinde kullanılmasını gerektirir. Web Apps ile aynı etki alanı adı birden çok abonelik kullanmak mümkün değildir.

Diğer uç nokta türleri için Traffic Manager uç noktalarının birden fazla abonelik ile kullanılacak mümkündür. Kaynak Yöneticisi'nde okuma erişimi uç nokta Traffic Manager profilinin yapılandırma kişinin sahip olduğu sürece herhangi bir Abonelikteki uç noktaları Traffic Manager için eklenebilir. Bu izinler kullanılarak verilebilir [Azure Resource Manager rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md).

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Traffic Manager 'Hazırlama' bulut hizmeti yuvası ile kullanabilir miyim?

Evet. 'Hazırlama yuvası' bulut hizmeti, dış uç noktalar olarak Traffic Manager'dan yapılandırılabilir. Sistem durumu denetimleri hala olması ücretlendirilir Azure uç noktaları oranı.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Traffic Manager, IPv6 uç noktaları destekliyor mu?

Traffic Manager, şu anda IPv6 adresleme ad sunucularını sağlamaz. Ancak, Traffic Manager Uç noktalara IPv6 IPv6 istemciler tarafından hala kullanılabilir. Bir istemci doğrudan Traffic Manager DNS istekleri yapmaz. Bunun yerine, istemci bir özyinelemeli DNS hizmeti kullanır. Bir yalnızca IPv6 istemci IPv6 üzerinden özyinelemeli DNS hizmeti istekleri gönderir. Ardından özyinelemeli hizmet IPv4 kullanarak Traffic Manager ad sunucularıyla ile bağlantı kurabiliyor olması.

Traffic Manager uç noktasının IP adresini ve DNS adı ile yanıt verir. Bir IPv6 uç nokta desteklemek için iki seçenek vardır. Uç nokta Traffic Manager'ın sistem durumu denetimi, uç nokta ve sorgu yanıtına bir CNAME kaydı olarak türüne olur ve ilişkili bir AAAA kaydı olan bir DNS adı olarak ekleyebilirsiniz. IPv6 adresi ve Traffic Manager kullanarak bir AAAA türü kayıt sorgu yanıtında döndürür doğrudan bu uç nokta da ekleyebilirsiniz.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Aynı bölgede birden fazla Web uygulaması ile Traffic Manager kullanabilir miyim?

Genellikle, Traffic Manager, farklı bölgelerde dağıtılan uygulamalar için trafiği yönlendirmek için kullanılır. Ancak, bu da bir uygulama da aynı bölgede birden fazla dağıtım bulunduğu kullanılabilir. Azure Traffic Manager uç noktaları birden fazla Web uygulaması uç noktası için aynı Traffic Manager profili eklenmesi için aynı Azure bölgelerine izin vermez.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group"></a>Azure uç noktaları Traffic Manager Profilimin farklı kaynak grubuna nasıl taşırım?

Kendi kaynak kimliklerini kullanarak bir Traffic Manager profili ile ilişkili azure uç noktaları izlenir. Ne zaman (örneğin, genel IP, Klasik bulut hizmeti, WebApp veya iç içe geçmiş bir şekilde kullanılan başka bir Traffic Manager profili) bir uç nokta olarak kullanılan bir Azure kaynağı, kaynak kodu değişiklikleri farklı kaynak grubuna taşınır. Bu senaryoda, şu anda, ilk silerek ve uç noktaları profile geri ekleyerek Traffic Manager profili güncelleştirmelidir.

## <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager uç nokta izleme

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Traffic Manager, Azure bölgesi hatalara karşı dayanıklı mi?

Traffic Manager, azure'da yüksek kullanılabilirliğe sahip uygulamalar teslimini anahtar bir bileşenidir.
Traffic Manager yüksek kullanılabilirlik sunmak üzere bir yayılan yüksek kullanılabilirlik düzeyine sahip ve için bölgesel hatalara dayanıklı olması gerekir.

Tasarım gereği, Traffic Manager bileşenleri için herhangi bir Azure bölgesinin bir tam hatayla esnektir. Bu esneklik tüm Traffic Manager bileşenleri için geçerlidir: DNS ad sunucuları, API, depolama katmanı ve İzleme Hizmeti uç noktası.

Azure bölgesinin tamamını çalışmadığını olası olayda Traffic Manager, normal şekilde çalışmaya devam beklenir. Birden çok Azure bölgesinde dağıtılan uygulamaları, uygulamanın kullanılabilir bir örneğe trafiği yönlendirmek için Traffic Manager'üzerindeki güvenebilirsiniz.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Kaynak grubu konumu seçiminin Traffic Manager nasıl etkiler?

Traffic Manager tek, genel bir hizmettir. Bölgesel değil. Kaynak grubu konumu seçiminin bu kaynak grubunda dağıtılan Traffic Manager profillerine nasıl yönelttiğiniz fark etmez.

Azure Resource Manager, bu kaynak grubunda dağıtılan kaynakların varsayılan konumunu belirleyen bir konum belirtmek tüm kaynak grupları gerektirir. Traffic Manager profili oluşturduğunuzda, bir kaynak grubunda oluşturulur. Tüm Traffic Manager profillerini kullanma **genel** konumlarına kaynak grubu varsayılan geçersiz kılma.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Her uç noktasının geçerli durumunu nasıl belirlerim?

Genel profil ek olarak her uç nokta izleme geçerli durumunu Azure portalında görüntülenir. Bu bilgiler ayrıca trafiği izleyici aracılığıyla kullanılabilir [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/az.trafficmanager), ve [platformlar arası Azure CLI'yı](../cli-install-nodejs.md).

Azure İzleyici, uç noktalarınızın sistem durumunu izleyebilir ve bunları görsel bir temsilini görmek için de kullanabilirsiniz. Azure İzleyicisi'ni kullanma hakkında daha fazla bilgi için bkz. [Azure izleme belgeleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

### <a name="can-i-monitor-https-endpoints"></a>HTTPS uç noktaları izleyebilirim?

Evet. Traffic Manager, HTTPS üzerinden yoklama destekliyor. Yapılandırma **HTTPS** İzleme Yapılandırması Protokolü olarak.

Traffic manager, herhangi bir sertifika doğrulama sağlayamazsınız dahil olmak üzere:

* Sunucu tarafı sertifikalar doğrulanmaz
* Sunucu tarafı sertifikaları SNI doğrulanmaz
* İstemci sertifikaları desteklenmez.

### <a name="do-i-use-an-ip-address-or-a-dns-name-when-adding-an-endpoint"></a>Bir IP adresi veya DNS adı bir uç nokta eklerken kullanabilirim?

Traffic Manager kullanarak bunları – bir DNS adı olarak, bir IPv4 adresi ve bir IPv6 adresi olarak başvurmak için üç yol ekleme uç noktaları destekliyor. Sorgu yanıtına kayıt türü A veya AAAA, uç nokta IPv4 veya IPv6 adresi eklediyseniz sırasıyla olacaktır. Uç nokta DNS adı olarak eklendiyse, sorgu yanıtına CNAME kayıt türü olabilir. Uç nokta türü ise yalnızca IPv4 veya IPv6 adresi buldukça, uç noktaları ekleyerek **dış**.
Yönlendirme yöntemleri ve izleme ayarlarını üç uç nokta adresi türleri tarafından desteklenir.

### <a name="what-types-of-ip-addresses-can-i-use-when-adding-an-endpoint"></a>Bir uç nokta eklerken hangi tür IP adresi kullanabilirim?

Traffic Manager uç noktaları belirtmek için IPv4 veya IPv6 adresleri kullanmanıza olanak sağlar. Aşağıda listelenen bazı kısıtlamalar vardır:

- Ayrılmış özel IP adresi alanları karşılık gelen adresleri izin verilmez. RFC 1918, RFC 6890, RFC 5737, RFC 3068, RFC 2544 ve RFC 5771 çekilerek bu adresleri içerir
- Adresi (Profil yapılandırma ayarlarında kullanılacak bağlantı noktası belirtebilirsiniz) herhangi bir bağlantı noktası numaralarını içermemelidir.
- İki uç nokta aynı profildeki aynı hedef IP adresine sahip olabilir

### <a name="can-i-use-different-endpoint-addressing-types-within-a-single-profile"></a>Farklı uç nokta türleri tek bir profili içinde adresleme kullanabilir miyim?

Hayır, Traffic Manager uç nokta adresi türleri burada IPv4 karıştırabilirsiniz çok değerli yönlendirme türü ve IPv6 adres türü ile bir profil durumu dışında bir profili içinde karıştırmak izin vermez

### <a name="what-happens-when-an-incoming-querys-record-type-is-different-from-the-record-type-associated-with-the-addressing-type-of-the-endpoints"></a>Gelen bir sorgunun kayıt türü, uç nokta adresi türü ile ilişkili kayıt türünün farklıdır ne olur?

Bir profili karşı bir sorgu alındığında, Traffic Manager belirtilen yönlendirme yöntemine göre döndürülmesi gereken uç nokta ve sistem durumu uç nokta ilk bulur. Ardından gelen sorguyu istenen kayıt türüne ve tabloyu temel alan bir yanıt döndürmeden önce uç noktası ile ilişkili kayıt türünün görünüyor.

Çoklu değer dışında herhangi bir yönlendirme yöntemi olan profiller için:

|Gelen sorgu isteği|    Uç noktası türü|  Sağlanan yanıt|
|--|--|--|
|TÜM |  A / AAAA / CNAME |  Hedef uç noktası| 
|A |    A / CNAME | Hedef uç noktası|
|A |    AAAA |  NODATA |
|AAAA | AAAA / CNAME |  Hedef uç noktası|
|AAAA | A | NODATA |
|CNAME |    CNAME | Hedef uç noktası|
|CNAME  |A / AAAA | NODATA |
|

Yönlendirme yöntemi olan profiller için birden çok değerli ayarlamak için:

|Gelen sorgu isteği|    Uç noktası türü | Sağlanan yanıt|
|--|--|--|
|TÜM |  Karışımını A ve AAAA | Hedef uç noktaları|
|A |    Karışımını A ve AAAA | Yalnızca hedef uç nokta türü a|
|AAAA   |Karışımını A ve AAAA|     Yalnızca hedef uç nokta türü AAAA|
|CNAME |    Karışımını A ve AAAA | NODATA |

### <a name="can-i-use-a-profile-with-ipv4--ipv6-addressed-endpoints-in-a-nested-profile"></a>IPv4 ile bir profil kullanabilirsiniz / IPv6, iç içe geçmiş bir profilde uç noktalar ele?

Evet, bir profil türü birden çok değerli bir iç içe geçmiş profil üst profilinde olamaz özel durum ile ayarlayabilirsiniz.

### <a name="i-stopped-an-web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this"></a>Ben bir web uygulaması uç nokta Traffic Manager Profilimi durduruldu ancak bile ben bunu yeniden sonra herhangi bir trafik alıyorum değil. Bunu nasıl düzeltebilirim?

Bir Azure web uygulaması uç nokta durdurulduğunda Traffic Manager sistem durumu denetimi durdurur ve yalnızca uç nokta başlatıldı algılandıktan sonra sistem durumu denetimleri yeniden başlatır. Bu gecikmeyi önlemek için devre dışı bırakın ve uç noktayı yeniden başlattıktan sonra Bu uç nokta Traffic Manager profilindeki yeniden etkinleştirin.

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Traffic Manager, Uygulamam HTTP veya HTTPS desteğini yoksa bile kullanabilir miyim?

Evet. İzleyici protokolü olarak TCP belirtebilirsiniz ve Traffic Manager TCP bağlantısı başlatır ve uç noktasından bir yanıt bekler. Zaman aşımı süresi içinde bağlantı kurmak için bağlantı isteğine yanıt veren bir uç nokta yanıt verirse Bu uç nokta sağlıklı olarak işaretlenir.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>Belirli yanıtları kullanırken TCP izleme uç noktasından gereklidir?

TCP izleme kullanıldığında, Traffic Manager uç noktası belirtilen bağlantı noktasında SYN istek göndererek üç yönlü TCP el sıkışması başlatır. Ardından süreyi (zaman aşımı ayarları'nda belirtildiği şekilde) uç noktasından bir yanıt bekler. İzleme ayarlarında belirtilen zaman aşımı süresi içinde SYN isteğine SYN ACK yanıt veren bir uç nokta yanıt verirse, bu uç nokta sağlıklı olarak değerlendirilir. Traffic Manager SYN ACK yanıt alınmazsa, bağlantı geri lk ile yanıt vererek sıfırlar.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Traffic Manager, sağlıklı bir uç nokta uzağa kullanıcılarımın ne kadar hızlı taşınsın mı?

Traffic Manager, yük devretme davranışı, Traffic Manager profilinizin gibi denetlemenize yardımcı olabilecek birden çok ayarları sunar:

- Traffic Manager uç noktaları daha sık yoklama aralığı 10 saniyeden ayarlayarak araştırmaları gerektiğini belirtebilirsiniz. Bu, iyi durumda olmayan giderek herhangi bir uç noktaya olabildiğince çabuk algılanabilir sağlar. 
- İstek sürelerinin bir sistem durumu atın önce beklenecek süreyi belirtebilirsiniz (en düşük zaman aşımı değeri olan 5 saniye).
- uç nokta olarak sağlıksız olarak işaretlenmeden önce kaç hataları oluşabilir belirtebilirsiniz. Bu değer 0, ilk sistem durumu denetimi başarısız olarak, uç nokta durum iyi durumda olmayan işaretlenmiş düşük olabilir. Tolere edilen hata sayısı için en düşük değeri 0'ı kullanarak, yoklama sırasında oluşabilecek geçici sorunları nedeniyle rotasyon dışında alınan uç noktalarına ancak açabilir.
- DNS yanıtı 0 düşük olması için yaşam süresi (TTL) belirtebilirsiniz. DNS Çözümleyicileri yanıtı önbelleğe olamaz ve bir yanıt her yeni bir sorgu alır anlamına gelir, Traffic Manager sahip en güncel durumu bilgileri içerir böylece yapılıyor.

Bu ayarları kullanarak, Traffic Manager yük devretmeleri altında 10 saniye sonra bir uç nokta iyi durumda olmayan gider ve bir DNS sorgusu karşılık gelen profili karşı yapılır sağlayabilir.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Nasıl miyim farklı uç noktalar için farklı izleme ayarlarını profilde belirtebilir miyim?

Traffic Manager izleme ayarlarını ndadır bir profili düzeyi başına. Tek bir uç nokta için farklı bir izleme ayarını kullanmanız gerekiyorsa, bunu, uç noktası olarak sağlayarak yapılabilir bir [iç içe profil](traffic-manager-nested-profiles.md) izleme ayarları olan üst profilinden farklı.

### <a name="how-can-i-assign-http-headers-to-the-traffic-manager-health-checks-to-my-endpoints"></a>Nasıl miyim HTTP üstbilgileri için benim uç noktaları için durum denetimleri olan Traffic Manager atayabilirim miyim?

Traffic Manager özel üst bilgiler, HTTP (durum için uç noktalarınız başlattığı denetimleri S) belirtmenize olanak sağlar. Özel bir başlık belirtmek istiyorsanız, profil düzeyinde (tüm uç noktalar için geçerlidir) bunun veya uç nokta düzeyinde belirtebilirsiniz. Üst bilgi, iki düzeyde tanımlanmazsa, uç nokta düzeyinde belirtilen bir profili düzeyi bir geçersiz kılar.
Traffic Manager isteklerini doğru bir çok kiracılı Ortamı'nda barındırılan bir uç noktaya yönlendirilir, böylece bu yaygın bir kullanım örneği barındırma üstbilgileri belirtilmesidir. Başka bir kullanım örneği bu Traffic Manager uç noktanın HTTP (S) istek günlükleri isteklerinden belirlemektir.

## <a name="what-host-header-do-endpoint-health-checks-use"></a>Hangi konak üst bilgisi uç nokta sistem durumu kullanım denetler?

Hiçbir özel ana bilgisayar üstbilgisi ayarı sağlanırsa, kullanılabilir durumdaysa Traffic Manager tarafından kullanılan ana bilgisayar üst bilgisini DNS profilinde yapılandırılan uç noktası hedef adıdır.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>IP adreslerini kendisinden kaynaklanan durum denetimleri nelerdir?

Tıklayın [burada](https://azuretrafficmanagerdata.blob.core.windows.net/probes/azure/probe-ip-ranges.json) sistem durumu denetimleri hangi trafik Yöneticisi'nden kaynaklanan IP adreslerini listeler JSON dosyasını görüntülemek için. Sistem durumunu denetlemek için uç noktada bu IP adreslerinden gelen bağlantılara izin verilmesini sağlamak için JSON dosyasında listelenen IP'ler gözden geçirin.

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Kaç sistem durumu denetimleri Noktam için Traffic Manager'dan beklemeliyim?

Uç noktanız ulaşmasını aşağıdakilere bağlıdır Traffic Manager durumu sayısını denetler:

- İzleme aralığı için ayarladığınız değerin (daha fazla istekleri belirli bir süre içinde uç noktanız üzerinde giriş daha küçük bir aralık anlamına gelir).
- Durum denetimleri (burada, bu denetimler önceki SSS bölümünde listelenen bekleyebileceğiniz IP adresleri) burada kaynaklanan gelen konumları sayısı.

### <a name="how-can-i-get-notified-if-one-of-my-endpoints-goes-down"></a>My uç noktalardan biri arıza yaparsa nasıl bildirim alabilirim?

Traffic Manager tarafından sağlanan ölçümler profilde uç nokta sistem durumu biridir. Bu profili içindeki tüm uç noktaların bir toplam olarak görebilirsiniz (örneğin, uç noktalarınıza %75 sağlıklı), veya en bir uç nokta düzeyine başına. Traffic Manager ölçümler, Azure İzleyici sunulur ve kullanabileceğiniz kendi [uyarı verme özellikleri](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) sistem durumu uç noktanızın bir değişiklik olduğunda bildirim almak için. Daha fazla ayrıntı için [Traffic Manager ölçümleri ve Uyarıları](traffic-manager-metrics-alerts.md).  

## <a name="traffic-manager-nested-profiles"></a>Traffic Manager iç içe profiller

### <a name="how-do-i-configure-nested-profiles"></a>İç içe geçmiş profilleri nasıl yapılandırılır?

İç içe Traffic Manager profilleri, hem Azure Resource Manager ve klasik Azure REST API'leri, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI komutlarını kullanarak yapılandırılabilir. Bunlar, yeni Azure portalı da desteklenir.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>İç içe geçme mu trafik Yöneticisi'ni kaç katmanları destekliyor?

Profilleri derin 10 düzeye kadar iç içe yerleştirebilirsiniz. Döngü' izin verilmez.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Diğer uç nokta türleri aynı Traffic Manager profili içinde iç içe geçmiş alt profilleriyle karıştırabilirsiniz?

Evet. Bir profili içinde farklı türde uç noktalar birleştirerek nasıl hiçbir kısıtlamalar vardır.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Faturalandırma modeli, iç içe geçmiş profilleri için nasıl uygulanır?

İç içe profilleri kullanarak etkisi fiyatlandırma negatif yoktur.

Traffic Manager faturalandırması, iki bileşenden oluşur: uç nokta sistem durumu denetimleri ve DNS sorgularının sayısı

* Uç nokta durum denetimlerinin: Bir üst profilinde bir uç noktası olarak yapılandırıldığında bir alt ağ profili için bir ücret yoktur. Alt profilinde bir uç nokta izleme, her zamanki gibi faturalandırılır.
* DNS sorguları: Her sorgu yalnızca bir kez sayılır. Bir uç nokta, alt profilinden döndüren bir üst profili yönelik bir sorgu yalnızca üst profili karşı sayılır.

Tüm Ayrıntılar için bkz. [Traffic Manager fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>İç içe profiller için bir performans etkisi var mı?

Hayır. İç içe geçmiş profilleri kullanırken sonucunda herhangi bir performans etkisi yoktur.

Traffic Manager ad sunucularıyla profil hiyerarşisindeki her DNS sorgusu işlenirken dahili olarak geçiş. Üst profil için bir DNS sorgusu bir DNS yanıtına bir uç nokta, alt profilinden alabilir. Tek bir CNAME kaydı, tek bir profil ya da iç içe geçmiş profilleri kullanıp kullanmadığınızı kullanılır. Hiyerarşideki her bir profil için bir CNAME kaydı oluşturmak için gerek yoktur.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Traffic Manager, üst profilinde iç içe geçmiş bir uç noktasının durumunu nasıl işlem?

Üst profili sistem durumu denetimleri alt doğrudan gerçekleştirmez. Bunun yerine, sistem durumunu alt profilin uç noktaları alt profili genel durumunu hesaplamak için kullanılır. Bu bilgiler, iç içe uç noktasının durumunu belirlemek için iç içe geçmiş profil hiyerarşisinde yukarı yayılır. Üst profili bu toplanan sistem durumu trafiği alt yönlendirilebilir olup olmadığını belirlemek için kullanır.

Aşağıdaki tabloda iç içe uç noktası için durum denetimleri Traffic Manager davranışı açıklanmaktadır.

| Alt profil İzleyicisi durumu | Üst uç noktası İzleyicisi durumu | Notlar |
| --- | --- | --- |
| Devre dışı. Alt profili devre dışı bırakıldı. |Durduruldu |Üst uç nokta durumu durduruldu, devre dışı. Devre dışı durumunu belirten üst profilde uç noktayı devre dışı bıraktığınız ayrılmıştır. |
| Düzeyi düşürülmüş. En az bir alt profili bitiş noktasına Degraded durumda. |Çevrimiçi: alt profildeki çevrimiçi uç noktaların sayısını en az MinChildEndpoints değeridir.<BR>CheckingEndpoint: alt profildeki çevrimiçi artı CheckingEndpoint uç noktaların sayısını en az MinChildEndpoints değeridir.<BR>Düzeyi düşürülmüş: Aksi takdirde. |Trafiği bir uç noktaya durumu CheckingEndpoint yönlendirilir. MinChildEndpoints çok yüksek olarak ayarlanırsa, uç nokta her zaman düşer. |
| Çevrimiçi. Çevrimiçi bir durumda en az bir alt profili uç noktadır. Uç nokta Degraded durumdadır. |Yukarıya bakın. | |
| CheckingEndpoints. 'CheckingEndpoint' en az bir alt profili uç noktadır. Uç nokta 'Çevrimiçi' veya 'Düşürülmüş' olan |Yukarıdakiyle aynı. | |
| Etkin değil. Tüm alt profil uç noktaları devre dışı veya durdurulmuş olan veya bu profile sahip uç nokta yok. |Durduruldu | |

## <a name="next-steps"></a>Sonraki adımlar:

- Traffic Manager hakkında daha fazla bilgi edinin [uç nokta izleme ve otomatik yük devretme](../traffic-manager/traffic-manager-monitoring.md).
- Traffic Manager hakkında daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](../traffic-manager/traffic-manager-routing-methods.md).
