---
title: "Azure Traffic Manager - sık sorulan sorular | Microsoft Docs"
description: "Bu makalede trafik Yöneticisi ile ilgili sık sorulan soruların yanıtlarını sağlar"
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2017
ms.author: kumud
ms.openlocfilehash: 5b0a7d423bc0d8d9f9f7cad56838bd006e944050
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Trafik Yöneticisi sık sorulan sorular (SSS)

## <a name="traffic-manager-basics"></a>Trafik Yöneticisi temelleri

### <a name="what-ip-address-does-traffic-manager-use"></a>Hangi IP adresi trafik Yöneticisi kullanıyor mu?

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), trafik Yöneticisi DNS düzeyinde çalışır. Uygun hizmet uç noktası istemcilere yönlendirmek için DNS yanıtlarının gönderir. İstemciler daha sonra hizmet uç noktası için doğrudan, trafik Yöneticisi ile bağlanır.

Bu nedenle, trafik Yöneticisi uç noktası veya bağlanmak istemciler için IP adresi sağlamaz. Hizmetiniz için statik IP adresi istiyorsanız, konuşması değil trafik Yöneticisi'nde yapılandırılmış olması gerekir.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Trafik Yöneticisi, 'Yapışkan' oturumları destekliyor mu?

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), trafik Yöneticisi DNS düzeyinde çalışır. Uygun hizmet uç noktası istemcilere yönlendirmek için DNS yanıtları kullanır. İstemcileri için hizmet uç noktası değil trafik Yöneticisi ile doğrudan bağlanır. Bu nedenle, trafik Yöneticisi istemci ve sunucu arasındaki HTTP trafiğini görmez.

Ayrıca, özyinelemeli DNS hizmeti istemci değil de Traffic Manager tarafından alınan DNS sorgusu kaynak IP adresi aittir. Bu nedenle, trafik Yöneticisi ayrı istemcilerin izlemek için bir yolu yoktur ve 'Yapışkan' oturumları uygulayamaz. Bu sınırlama tüm trafiği DNS tabanlı yönetim sistemleri için ortak olan ve trafik Yöneticisi için özel değildir.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanırken bir HTTP hata neden görüyorum?

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), trafik Yöneticisi DNS düzeyinde çalışır. Uygun hizmet uç noktası istemcilere yönlendirmek için DNS yanıtları kullanır. İstemciler daha sonra hizmet uç noktası için doğrudan, trafik Yöneticisi ile bağlanır. Trafik Yöneticisi olmayan istemci ve sunucu arasındaki HTTP trafiğini bakın yapar. Bu nedenle, gördüğünüz herhangi bir HTTP hata uygulamanızdan gelen gerekir. İstemcinin uygulamaya bağlanmak için tüm DNS çözüm adımları tamamlandı. Trafik Yöneticisi uygulama trafik akışını sahip herhangi bir etkileşim dahildir.

Daha fazla araştırma, bu nedenle uygulamanın odaklanmanız gerekir.

İstemcinin tarayıcıdan gönderilen HTTP ana bilgisayar üstbilgisi sorunlarının en yaygın kaynağıdır. Uygulama, kullanmakta olduğunuz etki alanı adı için doğru ana bilgisayar üstbilgisi kabul edecek şekilde yapılandırıldığından emin olun. Azure uygulama hizmeti kullanarak uç noktalar için bkz: [trafik Yöneticisi'ni kullanarak Azure App Service içinde bir web uygulaması için bir özel etki alanı adı yapılandırma](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak performans etkisi nedir?

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), trafik Yöneticisi DNS düzeyinde çalışır. İstemcileri, hizmet uç noktaları için doğrudan bağlanması bağlantı kurulduktan sonra trafik Yöneticisi'ni kullanırken ücrete performans üzerinde etkisi yoktur.

Trafik Yöneticisi DNS düzeyinde uygulamalarıyla tümleşir olduğundan, DNS çözümlemesi zincirine eklenecek ek bir DNS araması gerektirir. Trafik Yöneticisi'nin DNS çözümleme süresi üzerindeki etkiyi en alt düzeydedir. Trafik Yöneticisi genel bir ağ ad sunucuları ve kullanır [her noktaya yayın](https://en.wikipedia.org/wiki/Anycast) DNS emin olmak için ağ sorguları her zaman en yakın kullanılabilir ad sunucusuna yönlendirilir. Buna ek olarak, DNS yanıtların önbelleğe alma trafik Yöneticisi'ni kullanarak oluşturulan ek DNS gecikmenin oturumlarının bir kesir uygulanacağı anlamına gelir.

Performans yöntemi en yakın kullanılabilir uç nokta trafiğini yönlendirir. Bu yöntem ile ilişkili genel performans etkisi en düşük olması gerektiğini net sonucudur. Alt ağ gecikmesi uç tarafından DNS gecikme süresi içinde herhangi bir artış uzaklık.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Hangi uygulama protokolleri Traffic Manager ile kullanabilir miyim?

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), trafik Yöneticisi DNS düzeyinde çalışır. DNS araması tamamlandıktan sonra istemciler uygulama uç noktasına değil trafik Yöneticisi ile doğrudan bağlanın. Bu nedenle, bağlantı herhangi bir uygulama protokolünü kullanabilirsiniz. İzleme protokolün, trafik Yöneticisi TCP seçerseniz uç noktası durumunu izleme yapılabilir herhangi bir uygulama protokol kullanmadan. Uygulama protokolü kullanılarak doğrulandı sistem tercih ediyorsanız, uç noktanın HTTP veya HTTPS GET isteklerine yanıt verebilmesini olması gerekir.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Trafik Yöneticisi 'naked' etki alanı adıyla kullanabilir miyim?

Hayır. DNS standartları aynı ada diğer DNS kayıtlarını bulunabilmelerini CNAME'ler izin vermez. DNS bölge tepesinde (veya kök) her zaman iki önceden var olan DNS kayıtlarını içerir; SOA ve yetkili NS kayıtlarını. Başka bir deyişle, DNS standartlarında ihlal etmeden bölgenin tepesinde CNAME kaydı oluşturulamıyor.

Trafik Yöneticisi gösterim DNS adını eşleştirmek için bir DNS CNAME kaydı gerektirir. Örneğin, www.contoso.com için trafik Yöneticisi profili DNS adı olan contoso.trafficmanager.NET'e eşleştirin. Ayrıca, trafik Yöneticisi profili istemcinin bağlanması gereken hangi uç noktaya belirtmek için ikinci bir DNS CNAME döndürür.

Bu sorunu çözmek için bir HTTP yeniden yönlendirme doğrudan trafiğe, trafik Yöneticisi daha sonra kullanabileceğiniz farklı bir URL naked etki alanı adından kullanmanızı öneririz. Örneğin, "contoso.com" çıplak etki alanı kullanıcıları 'trafik Yöneticisi DNS adına işaret www.contoso.com' CNAME yönlendirebilirsiniz.

Naked etki alanı trafik Yöneticisi'nde için tam destek bizim özellik kapsamı izlenir. Bu özellik istekleri için destek kaydedebilirsiniz [için topluluk geri bildirim sitemizde oylama](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Trafik Yöneticisi DNS sorguları işlerken istemci alt ağ adresi dikkate almaz? 
Evet, ek olarak, DNS sorgusu kaynak IP adresi (genellikle olan DNS Çözümleyicisi IP adresidir), aldığı etkinleştirilmişse aramaları için Geographic ve performans yönlendirme yöntemleri gerçekleştirirken, trafik Yöneticisi ayrıca istemci alt ağ adresi göz önünde bulundurur Sorgu istekte son kullanıcı adına çözümleyici tarafından dahil.  
Özellikle, [RFC 7871 – istemci alt ağdaki DNS sorgularını](https://tools.ietf.org/html/rfc7871) sağlayan bir [DNS (EDNS0) için uzantı mekanizması](https://tools.ietf.org/html/rfc2671) hangi iletebilir istemci alt ağ adresi destekleyen çözümleyiciler.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>DNS TTL nedir ve Kullanıcılarım nasıl etkiler?

Trafik Yöneticisi bir DNS sorgusu adlandırıldığını, yaşam süresi (TTL) olarak adlandırılan yanıt olarak bir değer ayarlar. Saniye cinsinden, birimidir, bu değer, DNS Çözümleyicileri aşağı üzerinde için ne kadar süreyle bu yanıt önbelleğe almak için gösterir. DNS Çözümleyicileri bu sonucu önbelleğe garanti edilmez olurken, önbelleğe alma bunları trafik Yöneticisi DNS sunucularına giden yerine önbelleği devre dışı sonraki tüm sorgularını yanıtlamak üzere sağlar. Bu yanıtları şu şekilde etkiler:
- daha yüksek bir TTL sunulan sorgu sayısı Faturalanabilir kullanım olduğundan, bir müşteri için maliyetini azaltabilir trafik Yöneticisi DNS sunucularında güden olan sorgu sayısını azaltır.
- daha yüksek bir TTL olası DNS araması yapmak için geçen süreyi azaltabilir.
- daha yüksek bir TTL verilerinizi trafik Yöneticisi yoklama aracılarına elde edilen en son sistem durumu bilgileri yansıtmaz anlamına gelir.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Trafik Yöneticisi yanıtlar için TTL değeri, nasıl yüksek veya düşük ayarlayabilir miyim?

Ayarlayabileceğiniz, konumundaki bir profil gerçekleştiriliyordu 0 saniye olarak en düşük ve yüksek 2.147.483.647 saniye olarak kullanabilmek için DNS TTL (ile uyumlu en büyük aralığı [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). TTL değeri 0 aşağı akış DNS Çözümleyicileri sorgu yanıtlarını önbelleğe alma ve trafik Yöneticisi DNS çözümlemesi için sunucuları ulaşmak için tüm sorguların beklenen anlamına gelir.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Trafik Yöneticisi Geographic trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Coğrafi yönlendirme yararlı olduğu bazı kullanım örnekleri nelerdir? 
Coğrafi yönlendirme türü burada coğrafi bölgelerine bağlı kullanıcılarının ayırt etmek bir Azure müşterinin gereken herhangi bir senaryoda kullanılabilir. Örneğin, coğrafi trafik yönlendirme yöntemini kullanarak, kullanıcıların belirli bölgelerdeki diğer bölgelerdeki olandan farklı bir kullanıcı deneyimi verebilirsiniz. Başka bir örnek, belirli bir bölgede kullanıcılardan yalnızca bu bölgede uç noktaları tarafından sunulması gerektiren yerel veri egemenliği gerektirir ile uymak.

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Trafik Yöneticisi tarafından coğrafi yönlendirme için desteklenen bölgeler nelerdir? 
Trafik Yöneticisi tarafından kullanılan ülke/bölge hiyerarşisi bulunabilir [burada](traffic-manager-geographic-regions.md). Bu sayfayı tüm değişikliklerle güncel tutulur, ancak, program aracılığıyla da aynı bilgileri kullanarak alabilirsiniz [Azure Traffic Manager REST API'si](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Trafik Yöneticisi gelen bir kullanıcı burada sorgulama nasıl belirler? 
Trafik Yöneticisi (büyük olasılıkla kullanıcı adına sorgulama yaparken yerel bir DNS Çözümleyicisi budur) sorgusunun kaynak IP bakar ve bölge harita dahili IP'ye konumunu belirlemek için kullanır. Bu haritada Internet değişiklikleri hesap için devam eden düzenli olarak güncelleştirilir. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Trafik Yöneticisi kullanıcı her durumda tam coğrafi konumu doğru belirleyebilirsiniz sağlanır?
Hayır, trafik Yöneticisi aşağıdaki nedenlerden dolayı kullanıcının konumuna biz bir DNS sorgusu kaynak IP adresinden Infer coğrafi bölge her zaman karşılık gelir garanti edemez: 

- İlk olarak, önceki SSS bölümünde açıklandığı gibi biz bkz: kaynak IP adresi, kullanıcı adına arama yaparken bir DNS Çözümleyicisi değil. DNS Çözümleyicisi coğrafi konumunu kullanıcının coğrafi konum için iyi bir proxy olsa da, farklı bir müşteri kullanmak için seçtiği belirli DNS çözümleyicisini ve DNS Çözümleyicisi hizmet ayak bağlı olarak da olabilir. Örnek olarak, Malezya bulunan bir müşteri, cihazın ayarları kullanımda Singapur, DNS sunucusu kullanıcı/cihaz için sorgu çözümleri işlemek için çekildi bir DNS çözümleyicisini belirtebilirsiniz. Bu durumda, trafik Yöneticisi yalnızca Singapur konuma karşılık gelen Çözümleyici'nın IP adresi görebilirsiniz. Ayrıca, bu sayfada istemci alt ağ adresi desteği ile ilgili önceki SSS Bölümüne bakın.

- İkinci olarak, trafik Yöneticisi coğrafi bölge çeviri IP adresine yapmak için bir iç eşlemesi kullanır. Bu haritada doğrulanır ve doğruluğunu artırmak ve internet gelişen yapısı için hesap için sürekli olarak güncelleştirilmesi sırasında da hala bizim bilgi tüm IP adreslerini coğrafi konumunu tam bir temsili olmadığından olasılığı yoktur.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Bir uç nokta fiziksel olarak coğrafi yönlendirme için yapılandırılır biri ile aynı bölgede yer alması gerekiyor mu? 
Hayır, uç nokta konumu üzerinde bölgeler için eşlenebilir herhangi bir kısıtlama uygular. Örneğin, ABD-Orta Azure bölgesindeki bir uç nokta için yönlendirilmiş Hindistan tüm kullanıcıların sahip olabilir.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Coğrafi bölgeler Uç noktalara coğrafi yönlendirme yapmak için yapılandırılmamış bir profilinde atayabilirsiniz? 

Evet, bir profil yönlendirme yöntemini coğrafi değilse, kullanabileceğiniz [Azure Traffic Manager REST API'si](https://docs.microsoft.com/rest/api/trafficmanager/) uç noktaları bu profilde coğrafi bölgeler atamak için. Coğrafi olmayan yönlendirme türü profilleri söz konusu olduğunda, bu yapılandırmayı göz ardı edilir. Bu tür bir profil için coğrafi yönlendirme türü daha sonra değiştirirseniz, trafik Yöneticisi bu eşlemeleri kullanabilirsiniz.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Neden Geographic için bir profil yönlendirme yöntemini değiştirmek çalıştığınızda bir hata alıyorum?

Coğrafi yönlendirme ile bir profil altında tüm uç noktaları eşlenmiş en az bir bölge olması gerekir. Varolan bir profili coğrafi yönlendirme türüne dönüştürmek için önce kullanarak kendi uç noktalarına coğrafi bölgeler ilişkilendirmeniz gerekir [Azure Traffic Manager REST API'si](https://docs.microsoft.com/rest/api/trafficmanager/) yönlendirme türü için coğrafi değiştirmeden önce. Portal kullanıyorsanız, ilk uç noktaları silmenizi, profilinin yönlendirme yöntemi için coğrafi değiştirin ve ardından uç noktaları kendi coğrafi bölge eşleme birlikte ekleyin. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Neden müşteriler coğrafi yönlendirme etkinleştirilmiş bir profil altında uç noktaları yerine iç içe profil oluşturmak önerilir? 

Bir bölge bir profili içinde yalnızca bir uç nokta ise atanabilir kendi coğrafi yönlendirme türü kullanıyor. Bu uç iç içe geçmiş tür ekli bir alt profiline sahip değilse bu uç sağlıksız, giden trafik Yöneticisi trafiği tüm trafik değil herhangi daha iyi gönderme alternatif itibaren göndermeye devam eder. Atanan bölge "(örneğin, bölge İspanya sahip bir uç nokta başka bir uç nokta için yük devretme bunu sağlıksız kalırsa sağlıksız oluştu uç noktasına atanmış bir üst" bölgesinin olsa bile başka bir uç noktası, yük devretme trafik Yöneticisi yapar Avrupa atanmış bölge vardır). Bu trafik Yöneticisi bir müşteri kendi profilinde Kurulum sahip coğrafi sınırlar uyar emin olmak için gerçekleştirilir. Bir uç nokta sağlıksız durumda olduğunda, başka bir uç nokta için yapabilmesini elde etmek için coğrafi bölgeler içindeki birden çok uç nokta tekil uç noktalarını yerine iç içe geçmiş profiller atanmış önerilir. Bu şekilde, bir uç nokta iç içe alt profildeki başarısız olursa, trafiği için aynı iç içe alt profili içinde başka bir uç nokta yük devretme.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Bu yönlendirme türü destekleyen API sürümü üzerinde herhangi bir kısıtlamanın var mı?

Evet, yalnızca API sürümü 2017-03-01 ve daha yeni destekler coğrafi yönlendirme yazın. Herhangi bir eski API sürümü coğrafi yönlendirme türü oluşturulan profiller için kullanılması veya coğrafi bölgeler uç noktalarına atayın. Azure aboneliğinden profillerini almak için eski bir API sürümü kullandıysanız, coğrafi yönlendirme türü herhangi bir profil döndürülmez. Üstelik, eski API sürümleri kullanırken, herhangi bir profil, uç nokta bir coğrafi bölge atama ile gösterilen coğrafi bölge atamasını yok döndürdü.

## <a name="real-user-measurements"></a>Gerçek Kullanıcı Ölçümleri

>[!NOTE]
>Trafik Yöneticisi'nde gerçek kullanıcı ölçümleri özellik genel önizlemede ve genel kullanılabilirlik özellikleri sürüm gibi aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Özellik desteklenmiyor, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. Kullanılabilirlik ve bu özellik durumunu en güncel bildirimleri için denetleme [Azure trafik Yöneticisi'ni güncelleştirir](https://azure.microsoft.com/updates/?product=traffic-manager) sayfası.

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Gerçek kullanıcı ölçümleri kullanmanın avantajları nelerdir?
Performans yönlendirme yöntemini kullandığınızda, trafik Yöneticisi (geçirilen değilse) kaynak IP ve EDNS istemci alt inceleyerek bağlanmak, son kullanıcı için en iyi Azure bölgesi seçer ve ağ gecikmesi Intelligence karşı hizmet denetimi korur. Gerçek kullanıcı ölçümler, bu tabloda, son kullanıcılarınız için Azure eriştikleri son kullanıcı ağların yeterli yayılan olmanın yanı sıra bu gecikme tablo katkıda deneyimlerini sağlayarak bu, son kullanıcı için temel geliştirir. Bu son kullanıcılarınıza akışında artan bir doğruluk neden olmaktadır.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Azure olmayan bölgeleri ile gerçek kullanıcı ölçümleri kullanabilir miyim?
Gerçek kullanıcı ölçümleri ölçer ve Azure bölgeleri ulaşması gecikmesine yalnızca raporlar. Azure olmayan bölgelerde barındırılan uç noktaları performans tabanlı yönlendirme kullanıyorsanız, gecikme bilgilerini Bu uç nokta ile ilişkilendirilmesi için seçtiğinizden temsilcisi Azure bölgesi artan tarafından bu özelliğinden yararlanabilirsiniz.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Hangi yönlendirme yöntemini gerçek kullanıcı ölçümler yarar?
Gerçek kullanıcı ölçümleri ek bilgiler performans yönlendirme yöntemini kullanan profiller için geçerlidir. Azure portalı üzerinden görüntülediğinizde gerçek kullanıcı ölçümleri bağlantı tüm profillerden kullanılabilir olduğunu unutmayın.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>Gerçek kullanıcı ölçümleri her profili ayrı olarak etkinleştirmeniz gerekiyor mu?
Hayır, yalnızca abonelik başına bir kez etkinleştirmeniz gerekiyor ve gecikme bilgileri ölçülen ve raporlanan tüm profiller için kullanılabilir.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Aboneliğimi için nasıl devre dışı gerçek kullanıcı ölçümleri dışı?
Gerçek kullanıcı ölçümleri toplamak ve geri gecikme ölçümleri, istemci uygulamasından gönderme durdurduğunuzda ilgili ücretler tahakkuk durdurabilirsiniz. Örneğin, ölçüm web sayfalarında JavaScript katıştırılan JavaScript kaldırarak veya sayfanın işlendiğinde kendi çağırma kapatarak bu özelliği kullanarak durdurabilirsiniz.
Gerçek kullanıcı ölçümleri devre dışı bırakma başka bir yolu, anahtarınızı silmektir. Bunu gerçekleştirdikten sonra bu anahtarla trafik Yöneticisi için gönderilen tüm ölçümleri atılır.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Web sayfaları dışındaki istemci uygulamaları ile gerçek kullanıcı ölçümleri kullanabilir miyim?
Evet, gerçek kullanıcı ölçümleri farklı türde bir son kullanıcı istemcileri toplanan veri alma için tasarlanmıştır. Bu SSS, yeni istemci uygulaması türünü desteklenen olarak güncelleştirilir.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Kaç tane ölçümleri my gerçek kullanıcı ölçümler web sayfasının etkin her zaman yapılan işlenir?
Gerçek kullanıcı ölçümleri sağlanan JavaScript ölçüm ile kullanıldığında, her sayfa işleme gerçekleştirilmesini altı ölçüleri sonuçlanır. Bunlar daha sonra geri trafik Yöneticisi hizmeti bildirilir. Trafik Yöneticisi hizmeti bildirdi ölçümleri sayısına dayalı bu özellik için ücretlendirilirsiniz unutmayın. Örneğin, kullanıcı ölçümleri alınır ancak bildirildi önce başka bir sayfaya, Web sayfası giderse, bu ölçümleri fatura amaçlar için değerlendirilmez.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>My Web sayfasında gerçek kullanıcı ölçümleri komut dosyasını çalıştırmadan önce bir gecikme vardır?
Hayır, komut çağrılmadan önce programlanmış gecikme yoktur.

### <a name="can-i-use-configure-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Kullanabilirim ölçmek istediğiniz yalnızca Azure bölgeler ile gerçek kullanıcı ölçümleri yapılandırma?
Hayır, her zaman çağrılır, gerçek kullanıcı ölçümleri betik altı Azure bölgeleri hizmet tarafından belirlenen bir dizi ölçer. Bu farklı çağrıları arasındaki değişiklikleri ayarlayın ve çok sayıda böyle çağrılarını yaşandığında, ölçüm kapsamı farklı Azure bölgeler arasında yayılır.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Belirli bir numara ile yapılan ölçümleri sayısını sınırlamak?
JavaScript sayfanıza ve içinde katıştırılmış ölçüm üzeresiniz tam denetiminde üzerinden, başlatma ve durdurma kullanıyor. Trafik Yöneticisi hizmeti ölçülecek Azure bölgelerin bir listesi için bir istek alırsa sürece, bir dizi bölgeler döndürülür. Ayrıca Önizleme dönemi boyunca, trafik Yöneticisi için bildirilen tüm ölçümler için Fatura edilecek değil olduğunu aklınızda bulundurun

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Gerçek kullanıcı ölçümler bir parçası olarak my istemci uygulaması tarafından alınan ölçümlerin görüyor musunuz?
Ölçüm mantığı istemci uygulamanızı çalıştırın olduğundan, tam denetimi gecikme ölçümleri görmesini dahil olanlar demektir. Trafik Yöneticisi aboneliğinize bağlı anahtarı altında alınan ölçümler birleşik bir görünümünü bildirmiyor

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Trafik Yöneticisi tarafından sağlanan ölçüm komut dosyasını değiştirebilir mi?
Web sayfanızda katıştırılmış denetim işlenirken biz kesinlikle, ölçüleri ve gecikme doğru raporları emin olmak için ölçüm betik herhangi bir değişiklik yapmadan önerilmemektedir.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Başkalarının gerçek kullanıcı ölçümlerle kullandığınız anahtarı görmek olası olacak?
Bir web sayfasına ölçüm betik katıştırma başkaları için komut dosyası ve gerçek kullanıcı ölçümleri (RUM) anahtarınızı görmek kullanılabilecektir. Ancak bu anahtar, abonelik kimliği farklıdır ve yalnızca bu amaçla kullanılmak üzere trafik Yöneticisi'tarafından üretilen bilmeniz önemlidir. RUM anahtarınızı bilerek Azure hesabı güvenliğiniz güvenliğinin aşılmasına neden değil

### <a name="can-others-abuse-my-rum-key"></a>Başkalarının RUM anahtarımı kötüye?
Başkalarının anahtarınızı Azure için yanlış bilgi göndermek için kullanılacak mümkün olmakla birlikte, aldığımız tüm ölçüleri birlikte dikkate beri yönlendirme birkaç yanlış ölçümleri değiştirmez unutmayın. Anahtarlarınızı değiştirmeniz gerekiyorsa, bu noktada eski anahtarı atılan hale anahtarı yeniden oluşturabilir.

###  <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>Tüm web sayfalarımı JavaScript ölçüm put gerekiyor mu?
Gerçek kullanıcı ölçümleri ölçümleri artış sayısı arttıkça daha fazla değer sunar. Bununla, az sayıda tüm web sayfalarınıza veya bir select koyabilir gerekip gerekmediğini konusunda karar olduğu. Bizim önerimiz, en sık ziyaret edilen sayfanızda bir kullanıcı beş saniye veya daha fazla bu sayfada kalmak için beklenirken koyarak başlatmaktır.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Gerçek kullanıcı ölçümleri kullanırsanız, son kullanıcılar hakkında bilgi trafik Yöneticisi tarafından tanımlanabilir?
Trafik Yöneticisi, sağlanan ölçüm JavaScript kullanıldığında, son kullanıcının istemci IP adresi ve kullandıkları yerel DNS Çözümleyicisi kaynak IP adresi görünürlük sahip olur. Trafik Yöneticisi, yalnızca bu belirli ölçümleri gönderen kullanıcıdan belirlemek mümkün olmadığı kesilmiş sahip sonra istemci IP adresini kullanır. 

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>Trafik Yöneticisi yönlendirme için kullanılmasını ölçme gerçek kullanıcı ölçümleri gereken Web sayfası mu?
Hayır, bu trafik Yöneticisi'ni kullanmak zorunda değildir. Trafik Yöneticisi'nin yönlendirme yan gerçek kullanıcı ölçüm bölümünden ayrı olarak çalışır ve bunu her ikisinin de aynı web özelliği için harika bir fikir olsa da, bunlar olmanız gerekmez.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>Herhangi bir gerçek kullanıcı ölçümlerle kullanmak için Azure bölgeleri hizmetinde konak gerekiyor mu?
Hayır, çalışması gerçek kullanıcı ölçümler için Azure üzerinde herhangi bir sunucu tarafı bileşeni barındıran gerek yoktur. Tek pikselli bir görüntü JavaScript ölçüm tarafından indirilir ve farklı Azure bölgelerinde çalışan hizmeti barındırılan ve Azure tarafından yönetilir. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Gerçek kullanıcı ölçümleri kullandığınızda my Azure bant genişliği kullanımını artırır?
Önceki yanıtında belirtildiği gibi gerçek kullanıcı ölçümleri sunucu tarafı bileşenlerini ait ve Azure tarafından yönetilir. Bu, gerçek kullanıcı ölçümleri kullandığından, Azure bant genişliği kullanımı artırmaz anlamına gelir. Lütfen bu hangi Azure giderleri dışında herhangi bir bant genişliği kullanımını içermediğinden, unutmayın. Ölçüm bir Azure bölgesine gecikme için yalnızca bir tek pikselli bir görüntü yükleyerek kullanılan bant genişliğini en aza indirin. 

## <a name="traffic-view"></a>Trafik Görünümü

>[!NOTE]
>Trafik Yöneticisi'nde trafiği görünümü özelliği genel önizlemede ve genel kullanılabilirlik özellikleri sürüm gibi aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Özellik desteklenmiyor, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. Kullanılabilirlik ve bu özellik durumunu en güncel bildirimleri için denetleme [Azure trafik Yöneticisi'ni güncelleştirir](https://azure.microsoft.com/updates/?product=traffic-manager) sayfası.

### <a name="what-does-traffic-view-do"></a>Trafik görünüm ne yapar?
Trafik görünüm trafiği kullanıcılarınız ve deneyimlerini nasıl olduğu hakkında daha fazla anlamanıza yardımcı olacak Yöneticisi'nin bir özelliktir. Trafik Yöneticisi ve aşağıdaki ile sağlamak için hizmeti tutar ağ gecikmesi Intelligence tabloları tarafından alınan sorguları kullanır:
- Burada, uç noktalara Azure kullanıcılarınızın bağlanmasını bölgeleri.
- Bu bölgelerden bağlanan kullanıcıları birim.
- Azure bölgeleri için bunlar yönlendirilir.
- Bu Azure bölgeleri kendi gecikmesi deneyimine.

Bu bilgiler, bir tablo görünümü karşıdan yüklenmek üzere ham veriler olarak kullanılabilir olmasını durduracak yanı sıra Portalı'nda kullanmak kullanılabilir.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Trafik görünümünü kullanarak nasıl yararlı olabilir?

Trafik görünümü trafik Yöneticisi profillerinizi alma trafiği genel görünümünü verir. Özellikle, burada gelen kullanıcı tabanınızı bağlanır ve eşit olarak önemlisi ortalama gecikme süresi deneyimlerini nedir anlamak için kullanılabilir. Bu bilgiler daha sonra kullanıcılarla daha düşük gecikme süresi ile kullanılabileceği bir bölge için Azure ayak izini genişleterek odaklanmak gereken alanlar bulmak için de kullanabilirsiniz. Başka bir Insight trafiği görünümünü kullanarak türetilemeyeceğini sırayla bu bölgelerde stok artırarak veya azaltarak ilgili kararları vermenize yardımcı olabilecek farklı bölgelere trafik düzenlerini görmek için ' dir.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Trafik görünüm Azure İzleyicisi aracılığıyla kullanılabilir trafik Yöneticisi ölçümleri farklı mı?

Azure İzleyicisi, bir toplama düzeyinde profilinizi ve kendi uç noktaları tarafından alınan trafik anlamak için kullanılabilir. Ayrıca, sistem durumu denetimi sonuçları göstererek uç noktaları sistem durumunu izlemenize olanak sağlar. Bunlar gidin ve Azure bölgesel düzeyde bağlanma son kullanıcı deneyimini anlama gerektiğinde trafiği görünümü, elde etmek için kullanılabilir.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>Trafik görünüm EDNS istemci alt ağ bilgilerini kullanıyor mu?

Trafik görünüm EDNS istemci alt bilgi çıktısını oluştururken dikkate almaz. Bunları gruplandırmak için kullanıcılarınızın yerel DNS Çözümleyicisi IP adresini kullanır.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>Kaç gün veri trafiği görünümü kullanıyor mu?

Trafik görünümü, sizin tarafınızdan görüntülendiğinde önceki gün önce yedi gün verilerden işleyerek çıktısını oluşturur. Bu taşıma bir penceredir ve en son verileri ziyaret ettiğiniz her zaman kullanılır.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Trafik görünüm dış uç noktalar nasıl işler?

Dış uç noktalar bir Traffic Manager profilini Azure bölgelerinde dışında barındırılan kullandığınızda (performans yönlendirme yöntemini kullanıyorsanız bu aslında gerekli değildir), gecikme özellikleri için bir proxy olan bir Azure bölgesine eşlenen seçebilirsiniz. Bu Azure bölgesi eşleme varsa, Azure bölgenin gecikme ölçümleri trafiği görünüm çıkış oluşturulurken kullanılır. Azure bölge belirtilirse, gecikme bilgileri bu dış uç noktalar verileri boş olur.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>Aboneliğimi her profili için trafiği görünüm etkinleştirmeniz gerekiyor mu?
Önizleme dönemi sırasında trafiği görünümü abonelik düzeyinde etkinleştirilir ve tüm trafik Yöneticisi profilleri altında bu abonelik için kullanılabilir.

### <a name="how-can-i-turn-off-traffic-view"></a>Trafik görünümünü nasıl kapatabilir miyim?
Önizleme dönemi boyunca trafiği görünümü, aboneliğiniz için devre dışı bırakmak için destek bileti oluşturma isteği.

### <a name="how-does-traffic-view-billing-work"></a>Trafik görünüm Faturalaması nasıl çalışır?

Görünüm trafiği fiyatlandırma çıktı oluşturmak için kullanılan veri noktalarının sayısını temel alır. Şu anda desteklenen tek veri profilinizi alır sorguları türüdür. Ayrıca, yalnızca trafiği etkin görünüm varsa, yapıldığı işleme için faturalandırılır. Bu, bazı ayda bir zaman diliminde için trafiği görünümü etkinleştir ve diğer saatlerde kapatmak, yalnızca özellik alırken işlenen veri noktası sayısı faturanızı doğrultusunda etkin, anlamına gelir.
Önizleme dönemi boyunca, trafiği görünümü kullanmak için sizden ücret istenmese.

## <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Trafik Yöneticisi uç noktaları birden çok abonelik kullanabilir?

Birden çok abonelik uç noktalarını kullanarak Azure Web Apps ile mümkün değildir. Azure Web uygulamaları, Web uygulamaları ile kullanılan bir özel etki alanı adı yalnızca tek bir abonelik içinde kullanılan gerektirir. Aynı etki alanı adına sahip birden çok aboneliklerden Web uygulamaları kullanmak mümkün değil.

Diğer uç nokta türleri için trafik Yöneticisi uç noktaları birden fazla aboneliğe ilişkin kullanmak da mümkündür. Kaynak Yöneticisi'nde, uç nokta için okuma erişimi Traffic Manager profilinin yapılandırma kişinin sahip olduğu sürece herhangi bir abonelik uç noktalarının trafik Yöneticisi için eklenebilir. Bu izinler kullanılarak verilebilir [Azure Resource Manager rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Trafik Yöneticisi bulut hizmeti 'Staging' yuvası ile kullanabilir miyim?

Evet. Bulut hizmeti 'yuvası hazırlama' trafik Yöneticisi'nde dış uç noktalar olarak yapılandırılabilir. Sistem durumu denetimlerinin hala olması Ücretli Azure uç noktaları oranı. Dış uç nokta türü kullanımda olduğundan, temel alınan hizmet üzerindeki değişiklikler otomatik olarak toplanmaz. Bulut hizmeti durdurulmuş veya silindiğinde dış uç noktalar ile trafik Yöneticisi algılayamıyor. Bu nedenle, trafik Yöneticisi uç noktası devre dışı veya kadar sistem durumu denetimleri için fatura devam eder.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Trafik Yöneticisi IPv6 uç noktaları destekliyor mu?

Trafik Yöneticisi IPv6 addressible ad sunucuları şu anda sağlamaz. Ancak, trafik Yöneticisi IPv6 Uç noktalara bağlanırken IPv6 istemcileri tarafından hala kullanılabilir. Bir istemci doğrudan trafik Yöneticisi DNS isteklerine yapmaz. Bunun yerine, istemci bir yinelemeli DNS hizmeti kullanır. Bir yalnızca IPv6 istemci istekleri IPv6 üzerinden yinelemeli DNS hizmetine gönderir. Ardından özyinelemeli hizmet IPv4 kullanarak trafik Yöneticisi ad sunucuları ile bağlantı kurabiliyor olması gerekir.

Trafik Yöneticisi uç noktası DNS adı ile yanıt verir. Bir IPv6 uç nokta desteklemek için uç nokta DNS adı IPv6 adresine işaret eden bir DNS AAAA kaydı bulunması gerekir. Trafik Yöneticisi sistem durumu denetimlerini yalnızca IPv4 adreslerini destekler. Aynı DNS adı bir IPv4 uç noktada kullanıma sunmak hizmet gerekiyor.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Aynı bölgede birden fazla Web uygulaması ile trafik Yöneticisi kullanabilir miyim?

Genellikle, trafik Yöneticisi, farklı bölgelerde dağıtılan uygulamalar için trafiği yönlendirmek için kullanılır. Ancak, bu da bir uygulama birden fazla dağıtım aynı bölgede sahip olduğu kullanılabilir. Trafik Yöneticisi Azure uç noktaları birden fazla Web uygulaması uç noktası için aynı trafik Yöneticisi profili eklemek için aynı Azure bölgesinden izin vermez.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group"></a>Farklı bir kaynak grubuna nasıl my trafik Yöneticisi profilinin Azure uç noktaları taşıma?

Trafik Yöneticisi profili ile ilişkili azure uç noktaları, kendi kaynak kimlikleri kullanılarak izlenir. Ne zaman bir uç nokta (örneğin, genel IP, Klasik bulut hizmeti, WebApp veya iç içe geçmiş bir şekilde kullanılan başka bir trafik Yöneticisi profilini) olarak kullanılan bir Azure kaynağı farklı bir kaynak grubuna kaynak kimliği değişikliklerin taşınır. Bu senaryoda, şu anda, ilk silme ve uç nokta profile geri ekleyerek trafik Yöneticisi profili güncelleştirmelisiniz. 

##  <a name="traffic-manager-endpoint-monitoring"></a>Trafik Yöneticisi uç nokta izleme

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Trafik Yöneticisi Azure bölgesi hatalarına dayanıklı nedir?

Trafik Yöneticisi Azure yüksek oranda kullanılabilir uygulamalarda teslimini anahtar bir bileşenidir.
Yüksek kullanılabilirlik sağlamak için trafik Yöneticisi olağanüstü yüksek düzeyde kullanılabilirlik sahip ve bölgesel hatalarına karşı dayanıklı olmasını gerekir.

Tasarım gereği, trafik Yöneticisi tam bir herhangi bir Azure bölgesine hatalarına karşı dayanıklı bileşenleridir. Bu esneklik tüm trafik Yöneticisi bileşenleri için geçerlidir: DNS sunucuları, API, depolama katmanı ve İzleme Hizmeti uç nokta adı.

Tüm Azure bölgesinin bir kesinti olasılığı olayda normal şekilde çalışmaya devam etmek için trafik Yöneticisi beklenir. Birden çok Azure bölgelerinde dağıtılan uygulamaları kendi uygulama kullanılabilir bir örneğini trafiği yönlendirmek için trafik Yöneticisi'üzerinde güvenebilirsiniz.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Kaynak grubu konumu seçiminin trafik Yöneticisi nasıl etkiler?

Trafik Yöneticisi tek, genel bir hizmettir. Bölgesel değil. Kaynak grubu konumu seçiminin bu kaynak grubunda dağıtılan trafik Yöneticisi profillerine fark etmez.

Azure Resource Manager bu kaynak grubunda dağıtılan kaynaklar için varsayılan konum belirleyen bir konum belirtmek tüm kaynak gruplarının gerektirir. Trafik Yöneticisi profili oluşturduğunuzda, bir kaynak grubu oluşturulur. Tüm trafik Yöneticisi profilleri kullanmak **genel** konumlarına kaynak grubu varsayılan geçersiz kılma.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Her uç noktasının geçerli durumu nasıl belirlerim?

Genel profil yanı sıra her bitiş geçerli izleme durumunu Azure Portalı'nda görüntülenir. Bu bilgiler ayrıca trafiği İzleyicisi kullanılabilir [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell cmdlet'leri](https://msdn.microsoft.com/library/mt125941.aspx), ve [platformlar arası Azure CLI](../cli-install-nodejs.md).

Uç noktalarınızı sağlığını izlemek ve bunları görsel gösterimi görmek için Azure İzleyicisi'ni de kullanabilirsiniz. Azure İzleyicisi'ni kullanma hakkında daha fazla bilgi için bkz: [Azure Monitoring belgelerine](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

### <a name="can-i-monitor-https-endpoints"></a>HTTPS uç noktalarının izleyebilir mi?

Evet. Trafik Yöneticisi HTTPS üzerinden yoklama destekler. Yapılandırma **HTTPS** izleme yapılandırması protokol olarak.

Trafik Yöneticisi tüm sertifika doğrulaması sağlayamazsınız dahil olmak üzere:

* Sunucu tarafı sertifikalar doğrulanmaz
* SNI sunucu tarafı sertifikalar desteklenmez
* İstemci sertifikalarını desteklenmez.

### <a name="i-stopped-an-azure-cloud-service--web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this"></a>Bir Azure bulut hizmeti durduruldu / web uygulama uç noktasını Traffic Manager Profilim ancak bile t yeniden sonra herhangi bir trafik alıyorum değil. Bu nasıl çözebilir mi?

Ne zaman bir Azure bulut hizmeti / uygulama bitiş noktası web sistem durumu denetimi durdurulmuş trafik Yöneticisi durakları olan ve yalnızca uç nokta başlatıldı algılandıktan sonra sistem durumu denetimleri yeniden başlatır. Bu gecikmeyi önlemek için devre dışı bırakın ve uç nokta yeniden başlatıldıktan sonra bu trafik Yöneticisi Profil uç noktasını yeniden etkinleştirin.   

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Uygulamam HTTP veya HTTPS desteği yoksa bile trafik Yöneticisi kullanabilir miyim?

Evet. İzleme protokol olarak TCP belirtebilirsiniz ve trafik Yöneticisi bir TCP bağlantı başlatmak ve uç noktasından bir yanıt bekleyin. Zaman aşımı süresi içinde bağlantı kurmak için bağlantı isteğine yanıt veren bir uç nokta yanıt verirse Bu uç sağlıklı olarak işaretlenir.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>TCP izleme kullanırken hangi belirli yanıtları uç noktasından gereklidir?

TCP izleme kullanıldığında, trafik Yöneticisi uç noktada belirtilen bağlantı noktası için bir Eşitlemeye isteği göndererek üç yönlü TCP el sıkışma başlatır. Bir süre (zaman aşımı ayarları'nda belirtildiği gibi) yanıt uç noktasından sonra bekler. Uç nokta Eşitlemeye Eşitlemeye ACK yanıt izleme ayarlarında belirtilen zaman aşımı süresi içinde isteğe yanıt veren, bu uç sağlıklı olarak değerlendirilir. Eşitlemeye ACK yanıt alınmazsa, trafik Yöneticisi geri RST ile yanıt vererek bağlantıyı sıfırlar.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Trafik Yöneticisi Kullanıcılarım sağlıksız bir uç nokta çıktığınızda ne kadar hızlı hareket?

Trafik Yöneticisi gibi Traffic Manager profilinizin yük devretme davranışını denetlemek için yardımcı olabilecek birden çok ayarları sağlar:
- Trafik Yöneticisi uç noktaları daha sık yoklama aralığı 10 saniyede ayarlayarak yoklamaları gerektiğini belirtebilirsiniz. Bu, sağlıksız giderek herhangi bir uç nokta mümkün olan en kısa sürede algılanabilir sağlar. 
- bir sistem durumu kullanıma önce istek zamanları beklemek için ne kadar süreyle belirtebilirsiniz (en düşük zaman aşımı değeri olan 5 saniye).
- uç nokta olarak sağlıksız olarak işaretlenmiş önce kaç tane hataları oluşabilir belirtebilirsiniz. Bu değer ilk sistem durumu denetimi başarısız hemen sonra uç nokta Durumu sağlıksız işaretlenmiş 0 düşük olabilir. Ancak, toleranslı hata sayısı için 0 en küçük değerini kullanarak döndürme algılanıyor sırasında ortaya çıkabilecek geçici sorunları nedeniyle dışında gerçekleştirilmesini uç noktaları neden olabilir.
- 0 olarak kadar düşük olması DNS yanıtı için yaşam süresi (TTL) belirtin. Trafik Yöneticisi sahip en güncel sistem durumu bilgileri içeren DNS Çözümleyicileri yanıtı önbelleğe alamıyor ve her yeni bir sorgu bir yanıt alır anlamına gelir. bunları yapmak.

Trafik Yöneticisi, bu ayarları kullanarak bir uç nokta sağlıksız gider ve bir DNS sorgusu karşılık gelen bir profili karşı yaptıktan sonra altında 10 saniye yerine sağlayabilir.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Nasıl t farklı uç noktalar için farklı izleme ayarları profilde belirtebilir miyim?

Trafik Yöneticisi izleme ayarlardır sırasında bir profil gerçekleştiriliyordu. Yalnızca bir uç noktası için farklı bir izleme ayarı kullanmanız gerekiyorsa, o uç noktası olarak sağlayarak yapılabilir bir [iç içe profil](traffic-manager-nested-profiles.md) izleme ayarları olan üst profilinden farklı.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Hangi ana bilgisayar üstbilgisi uç nokta durumu kullanım denetler?

Trafik Yöneticisi ana bilgisayar üstbilgisi HTTP ve HTTPS sistem durumu denetimlerini kullanır. Trafik Yöneticisi tarafından kullanılan ana bilgisayar üstbilgisi profilinde yapılandırılan uç nokta hedef adıdır. Ana bilgisayar üstbilgisi için kullanılan değer hedef özelliğinden ayrı olarak belirtilemez.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>İçinden kaynaklanan sistem denetler IP adresleri nelerdir?

Aşağıdaki listede kaynaklanan, Traffic Manager sistem durumu denetler IP adreslerini içerir. Sistem durumunu denetlemek için bu IP adreslerinden gelen bağlantıları uç noktada izin verilmesini sağlamak için bu listeyi kullanabilir.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203
* 52.173.90.107
* 52.173.250.232
* 104.45.149.110
* 40.114.5.197
* 52.240.151.125
* 52.240.144.45
* 13.65.95.152
* 13.65.92.252
* 40.78.67.110
* 104.42.192.195

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Kaç tane durumu denetimleri için Noktam ı trafik Yöneticisi'nden bekleyebilirsiniz?

Uç noktanız ulaşmasını aşağıdakilere bağlıdır Traffic Manager sistem durumu sayısını denetler:
- İzleme aralığı için ayarladığınız değerin (küçük aralık, uç belirli bir dönemde giriş daha fazla isteği anlamına gelir).
- Burada durumu denetimleri (burada bu denetimler önceki SSS bölümünde listelenen bekleyebilirsiniz gelen IP adresleri) kaynaklanan gelen konumları sayısı.

## <a name="traffic-manager-nested-profiles"></a>Trafik Yöneticisi profilleri iç içe geçmiş

### <a name="how-do-i-configure-nested-profiles"></a>İç içe profil nasıl yapılandırırım?

İç içe trafik Yöneticisi profilleri, Azure Resource Manager ve klasik Azure REST API'leri, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI komutları kullanarak yapılandırılabilir. Bunlar, yeni Azure portalı üzerinden de desteklenir. Klasik portalda desteklenmez.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>İç içe geçmiş mu trafik Yöneticisi kaç katmanları destekliyor?

Profilleri 10 düzey derinliğinde kadar yerleştirebilirsiniz. 'Döngü' izin verilmez.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Diğer uç nokta türleri aynı trafik Yöneticisi profili içinde iç içe alt profilleriyle karıştırabilirsiniz?

Evet. Bir profili içindeki farklı türden uç noktaları nasıl birleştirmek üzerinde bir kısıtlama yoktur.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Faturalama modeli için iç içe profil nasıl uygulansın mı?

İç içe geçmiş profilleri kullanarak etkisi fiyatlandırma negatif yoktur.

Trafik Yöneticisi faturalama iki bileşeni vardır: uç noktası sistem durumu denetimlerinin ve DNS sorgularını milyonlarca

* Uç nokta durumu denetimleri: bir üst profilinde bir uç nokta olarak yapılandırıldığında alt profili için herhangi bir ücret alınmaz. Bitiş alt profildeki izleme normal şekilde faturalandırılır.
* DNS sorguları: her sorgu yalnızca bir kez sayılır. Bir sorgu alt profilden bir uç nokta döndüren bir üst profili yalnızca üst profili karşı sayılır.

Ayrıntılar için bkz: [trafik Yöneticisi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>İç içe profil için bir performans etkisi vardır?

Hayır. İç içe profil kullanılırken ücrete performans üzerinde etkisi yoktur.

Trafik Yöneticisi ad sunucuları profili hiyerarşi dahili olarak her DNS sorgusu işlenirken çapraz geçiş. Bir DNS sorgusu üst profil için bir uç nokta ile bir DNS yanıtı alt profilinden alabilir. Tek bir CNAME kaydı, tek bir profil veya iç içe profil kullanıp kullanmadığınızı kullanılır. Hiyerarşideki her bir profil için bir CNAME kaydı oluşturmak için gerek yoktur.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Trafik Yöneticisi, bir üst profilinde iç içe geçmiş bir uç noktası durumunu nasıl işlem?

Üst profili sistem durumu denetimlerinin alt doğrudan gerçekleştirmez. Bunun yerine, alt profilinin uç noktaları durumunu alt profili genel durumunu hesaplamak için kullanılır. Bu bilgiler, iç içe geçmiş uç noktasının sistem durumunu belirlemek için bir iç içe profil hiyerarşi yayılır. Üst profili toplanmış bu sistem durumu trafiği alt yönlendirilmiş olup olmadığını belirlemek için kullanır.

Aşağıdaki tabloda, sistem durumu iç içe geçmiş bir uç nokta için denetler trafik Yöneticisi davranışını tanımlar.

| Alt profili izleme durumu | Üst uç nokta İzleyicisi durumu | Notlar |
| --- | --- | --- |
| Devre dışı. Alt profili devre dışı bırakıldı. |Durduruldu |Üst uç nokta durumu devre dışı durdurulur. Devre dışı durumunu üst profil uç devre dışı bırakmış belirten için ayrılmıştır. |
| Düşürülmüş. En az bir alt profil uç Degraded durumda değil. |Çevrimiçi: alt profildeki çevrimiçi uç noktaların sayısını en az MinChildEndpoints değeridir.<BR>CheckingEndpoint: alt profildeki çevrimiçi artı CheckingEndpoint uç noktaların sayısını en az MinChildEndpoints değeridir.<BR>Düşürülmüş: Aksi takdirde. |Trafik bir durum CheckingEndpoint uç noktasına yönlendirilir. MinChildEndpoints çok yüksek olarak ayarlanırsa, uç nokta her zaman düşer. |
| Çevrimiçi. En az bir alt profil uç noktası çevrimiçi bir durumdur. Uç nokta yok Degraded durumda değil. |Yukarıya bakın. | |
| CheckingEndpoints. En az bir alt profil uç noktası 'CheckingEndpoint' dir. Hiçbir noktalarıdır 'Çevrimiçi' veya 'Düşürülmüş' |Yukarıdaki ile aynı. | |
| Etkin değil. Tüm alt profili devre dışı veya durdurulmuş noktalarıdır veya bu profil hiçbir uç noktası vardır. |Durduruldu | |

## <a name="next-steps"></a>Sonraki adımlar:
- Trafik Yöneticisi hakkında daha fazla bilgi [uç nokta izleme ve otomatik yük devretme](../traffic-manager/traffic-manager-monitoring.md).
- Trafik Yöneticisi hakkında daha fazla bilgi [trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).
