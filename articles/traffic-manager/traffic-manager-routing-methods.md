---
title: Azure Traffic Manager - trafik yönlendirme yöntemleri | Microsoft Docs
description: Bu trafik Yöneticisi tarafından kullanılan farklı trafik yönlendirme yöntemleri anlamanıza yardımcı makaleler
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: timlt
editor: ''
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: kumud
ms.openlocfilehash: c9bd9b4913e38ed5c1f7f4ec8ee7e3210fa3be8f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager yönlendirme yöntemleri

Azure Traffic Manager çeşitli hizmet uç noktaları için ağ trafiğini yönlendirmek nasıl belirlemek için dört trafik yönlendirme yöntemleri destekler. Traffic Manager trafik yönlendirme yöntemini aldığı her DNS sorgusu için geçerlidir. Hangi uç noktaya DNS yanıtında trafik yönlendirme yöntemini belirler.

Trafik Yöneticisi'nde dört trafik yönlendirme yöntemleri vardır:

* **[Öncelik](#priority):** seçin **öncelik** zaman tüm trafiği için birincil hizmet uç noktası ve birincil durumda yedeklemeler sağlamak isteyen veya yedekleme uç noktaları kullanılamıyor.
* **[Weighted](#weighted):** seçin **Weighted** eşit ya da, ağırlıklara göre trafiği bir uç nokta kümesi boyunca dağıtmak istediğinizde, tanımlamanızı.
* **[Performans](#performance):** seçin **performans** ne zaman farklı coğrafi konumlarda uç noktaları varsa ve son kullanıcıların en düşük ağ gecikmesini bakımından "en yakın" uç nokta kullanmak istiyor.
* **[Coğrafi](#geographic):** seçin **coğrafi** böylece kullanıcılar hangi coğrafi konum temelinde belirli Uç noktalara (Azure, dış veya iç içe) yönelik DNS sorgularını kaynaklandığı. Bu, bir kullanıcının coğrafi bölge bilerek ve bunları oturum tabanlı yönlendirme önemli olduğu senaryoları etkinleştirmek için trafik Yöneticisi müşteriler güçlendirir. Örnek veri egemenliği gerektirir, içerik & kullanıcı deneyiminin yerelleştirme uymaktan ve farklı bölgelerdeki trafiğini ölçme verilebilir.

Tüm trafik Yöneticisi Profil uç noktası durumu ve otomatik uç nokta yük devretme izlemeyi içerir. Daha fazla bilgi için bkz: [trafik Yöneticisi uç noktası izleme](traffic-manager-monitoring.md). Tek bir Traffic Manager profilini yalnızca bir trafik yönlendirme yöntemini kullanabilirsiniz. Profiliniz için herhangi bir zamanda farklı trafik yönlendirme yöntemini seçebilirsiniz. Bir dakika içinde değişiklikler uygulanır ve kapalı kalma süresi olmadan oluşur. Trafik yönlendirme yöntemleri, iç içe trafik Yöneticisi profilleri kullanılarak birleştirilebilir. İç içe geçme daha büyük ve karmaşık uygulamaların gereksinimlerini karşılayan karmaşık ve esnek trafik yönlendirme yapılandırmaları sağlar. Daha fazla bilgi için bkz: [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Öncelik trafik yönlendirme yöntemi

Genellikle bir kuruluşun kendi birincil hizmet arıza durumunda bir veya daha fazla yedekleme hizmetleri dağıtarak güvenilirlik için hizmetlerini sağlamak istiyor. Trafik yönlendirme 'Priority' yöntemi, kolayca bu yük devretme desen uygulamak Azure müşterilerin olanak tanır.

![Azure trafik Yöneticisi 'Priority' trafik yönlendirme yöntemi][1]

Trafik Yöneticisi profili hizmet uç noktaları öncelikli listesi içerir. Varsayılan olarak, trafik Yöneticisi tüm trafiği birincil (en yüksek öncelik) uç noktasına gönderir. Birincil uç noktası kullanılabilir durumda değilse, Traffic Manager trafik ikinci uç noktasına yönlendirir. Birincil ve ikincil uç kullanılabilir değilse, üçüncü ve benzeri için trafiği gider. Uç nokta kullanılabilirliğini yapılandırılan durumu (etkin veya devre dışı) ve devam eden uç nokta izleme dayanır.

### <a name="configuring-endpoints"></a>Uç noktalarını yapılandırma

Azure Resource Manager ile açıkça her uç noktası için 'priority' özelliğini kullanarak uç nokta önceliği yapılandırın. Bu özellik, 1 ile 1000 arasında bir değerdir. Daha düşük değerler daha yüksek önceliği temsil eder. Uç noktaları öncelik değerleri paylaşamaz. Özelliği isteğe bağlıdır. Atlanmış, uç nokta sıraya göre bir varsayılan öncelik kullanılır.

##<a name = "weighted"></a>Ağırlıklı trafik yönlendirme yöntemi
'Ağırlıklı' trafik yönlendirme yöntemini trafiği eşit olarak dağıtmanızı veya önceden tanımlanmış ağırlığı kullanılacak sağlar.

![Azure trafik Yöneticisi 'Ağırlıklı' trafik yönlendirme yöntemi][2]

Ağırlıklı trafik yönlendirme yöntemini, her uç nokta Traffic Manager profili yapılandırması için bir ağırlık atayın. Ağırlık 1000 ile 1 arasında bir tamsayı değil. Bu parametre isteğe bağlıdır. Belirtilmezse, trafiği yöneticileri varsayılan ağırlık '1' olarak kullanır. Daha yüksek ağırlık, öncelik o kadar yüksektir.

Alınan her DNS sorgusu için trafik Yöneticisi kullanılabilir uç nokta rastgele seçer. Bir uç noktasını olasılığını tüm kullanılabilir uç noktaları için atanan ağırlığı temel alır. Hatta trafik dağılımı tüm uç noktaları sonuçlarında arasında aynı ağırlıkta kullanma. Daha yüksek veya düşük ağırlıkları üzerinde belirli uç noktalarını kullanarak DNS yanıtları fazla veya az sık döndürülecek Bu uç neden olur.

Ağırlıklı yöntemi bazı yararlı senaryolarına olanak sağlar:

* Aşamalı uygulama yükseltme: yeni bir bitiş noktasına yönlendirmek için trafiği yüzdesi ayırmak ve trafiği için % 100 zaman içinde aşamalı olarak artırmak.
* Azure uygulama geçiş: hem Azure hem de dış uç noktalar ile bir profili oluşturun. Yeni uç nokta tercih uç ağırlığını ayarlayın.
* Ek kapasite için bulut emniyeti: hızlı bir şekilde buluta bir şirket içi dağıtıma bir Traffic Manager profilini koyarak genişletin. Bulut ek kapasite gerektiğinde, ekleyin veya daha fazla uç noktaları etkinleştirin ve hangi trafik bölümünü her uç noktasına gider belirtin.

Resource Manager Azure portal ağırlıklı trafik yönlendirme yapılandırmasını destekler.  Azure PowerShell'i, CLI ve REST API'leri Resource Manager sürümleri kullanma ağırlıkları yapılandırabilirsiniz.

DNS yanıtları, DNS adlarını çözümlemek için istemcilerin kullandığı yinelemeli DNS sunucuları ve istemcileri tarafından önbelleğe alınan anlamak önemlidir. Bu önbelleğe alma ağırlıklı trafiği dağıtımları üzerinde bir etkisi olabilir. İstemcilerin ve yinelemeli DNS sunucularının sayısı büyük olduğunda trafik dağılımı beklendiği gibi çalışır. İstemcileri veya yinelemeli DNS sunucularının sayısı az olduğunda, ancak, önbelleğe alma önemli ölçüde trafik dağılımı eğme.

Ortak kullanım durumları şunlardır:

* Geliştirme ve test ortamları
* Uygulama uygulama iletişimleri
* Uygulamalar bir dar kullanıcı ortak bir yinelemeli DNS altyapısı (örneğin, bir proxy üzerinden bağlanma şirket çalışanlarının) paylaşan temel yönelik

Bu DNS önbelleğe alma etkileri tüm DNS tabanlı trafik yönlendirme sistemler için yalnızca Azure Traffic Manager yaygındır. Bazı durumlarda, açıkça DNS önbelleğini temizlemek geçici bir çözüm sağlayabilir. Diğer durumlarda, alternatif bir trafik yönlendirme yöntemi daha uygun olabilir.

## <a name = "performance"></a>Performans trafik yönlendirme yöntemi

Dünya çapında uç noktaları iki veya daha fazla konumlarda dağıtma ' size en yakın ' konumuna yönlendirme trafiğini tarafından birçok uygulamalarının yanıtlama hızını artırabilir. Trafik yönlendirme 'Performans' yöntemi, bu yeteneği sağlar.

![Azure trafik Yöneticisi 'Performans' trafik yönlendirme yöntemi][3]

'En yakın' bitiş noktası tarafından coğrafi uzaklığı ölçülen mutlaka yakın değil. Bunun yerine, ağ gecikmesi ölçerek en yakın uç noktası 'Performans' trafik yönlendirme yöntemini belirler. Trafik Yöneticisi IP adresi aralıkları ve her Azure veri merkezi gidiş dönüş süresi izlemek için bir Internet gecikme tablosu tutar.

Trafik Yöneticisi Internet gecikme tablosundaki gelen DNS isteğin kaynak IP adresi arar. Trafik Yöneticisi kullanılabilir uç nokta IP adres aralığı için en düşük gecikme süresine sahiptir ve ardından bu uç DNS yanıtında döndürür Azure veri merkezinde seçer.

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](traffic-manager-overview.md#how-traffic-manager-works), trafik Yöneticisi DNS sorgularını doğrudan istemcilerden almaz. Bunun yerine, DNS sorgularını, istemcilerin kullanmak üzere yapılandırılmış özyinelemeli DNS hizmetinden gelir. Bu nedenle, IP adresi 'en yakın' uç noktası istemcinin IP adresi değil, ancak yinelemeli DNS hizmeti IP adresidir belirlemek için kullanılır. Uygulamada, istemci için iyi bir proxy bu IP adresidir.


Trafik Yöneticisi genel Internet'e ve yeni Azure bölgeleri değişiklikleri hesaba Internet gecikme tablosu düzenli olarak güncelleştirir. Ancak, uygulama performansı göre yük gerçek zamanlı Çeşitlemeler Internet üzerinden değişir. Performans trafik yönlendirme verilen hizmet uç noktası üzerindeki yük izlemez. Bir uç nokta kullanılamaz duruma gelirse, ancak, trafik Yöneticisi, DNS sorgu yanıtlarında içermeyecek.


Dikkat edilecek noktalar:

* Profilinizi aynı Azure bölgesinde birden fazla uç noktası içeriyorsa, sonra Traffic Manager trafik eşit bu bölgede kullanılabilir uç noktaları arasında dağıtır. Bir bölge içinde farklı trafik dağılımı tercih ederseniz, kullanabileceğiniz [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md).
* En yakın Azure bölgesi etkinleştirilmiş tüm uç noktaları düşürülmüş, trafik Yöneticisi uç noktalarına sonraki en yakın Azure bölgesinde trafik taşınır. Tercih edilen yük devretme sırası tanımlamak istiyorsanız, kullanmak [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md).
* Performans trafik yönlendirme yöntemini dış uç noktalar veya iç içe geçmiş uç noktaları ile kullanırken bu uç konumunu belirtmeniz gerekir. Dağıtımınız için en yakın Azure bölgesi seçin. Konumların Internet gecikme tablo tarafından desteklenmeyen değerlerdir.
* Uç nokta seçtiği belirleyici algoritmasıdır. Yinelenen DNS sorgularını aynı istemciden aynı bitiş noktasına yönlendirilirsiniz. Genellikle, istemcilerin seyahat halindeyken farklı yinelemeli DNS sunucusu kullanın. İstemci farklı bir bitiş noktasına yönlendirilebilir. Yönlendirme ayrıca Internet gecikme tabloya yapılan güncelleştirmeler tarafından etkilenebilir. Bu nedenle, performans trafik yönlendirme yöntemini bir istemci her zaman aynı uç noktasına yönlendirilir garanti etmez.
* Internet gecikme tablosu değiştiğinde bazı istemciler farklı bir bitiş noktasına yönlendirilirsiniz fark edebilirsiniz. Bu yönlendirme değişiklik geçerli gecikme verilere göre daha doğru olur. Bu güncelleştirmeler doğruluğu performans trafik yönlendirme, Internet sürekli geliştikçe korumak için gereklidir.

## <a name = "geographic"></a>Coğrafi trafik yönlendirme yöntemi

Trafik Yöneticisi profilleri, böylece kullanıcılar kendi DNS sorgusu kaynaklandığı hangi coğrafi konumuna dayalı belirli Uç noktalara (Azure, dış veya iç içe) yönlendirilir coğrafi yönlendirme yöntemini kullanmak üzere yapılandırılabilir. Bu, bir kullanıcının coğrafi bölge bilerek ve bunları oturum tabanlı yönlendirme önemli olduğu senaryoları etkinleştirmek için trafik Yöneticisi müşteriler güçlendirir. Örnek veri egemenliği gerektirir, içerik & kullanıcı deneyiminin yerelleştirme uymaktan ve farklı bölgelerdeki trafiğini ölçme verilebilir.
Bir profili coğrafi yönlendirme için yapılandırıldığında, her bitiş profili atanmış coğrafi bölgeler kümesine sahip olması gerektiğini ilişkili. Coğrafi bir bölgenin ayrıntı düzeyi aşağıdaki düzeylerde olabilir 
- Dünya – herhangi bir bölgeyi
- Bölgesel gruplandırma – Örneğin, Afrika, Orta Doğu, Avustralya/Pasifik vs. 
- Ülke/bölge – Örneğin, İrlanda, Peru, Hong Kong ÖİB vs. 
- Bölge – Örneğin, ABD California, Avustralya-Queensland, Kanada Alberta vb. (Not: Bu ayrıntı düzeyi yalnızca durumları için desteklenen / il Avustralya, Kanada, İngiltere ve ABD).

Bir bölge veya bölge kümesi bir bitiş noktasına atandığında, bu bölgeler gelen tüm isteklerin yönlendirilmiş yalnızca bu uç noktasına. Trafik Yöneticisi DNS sorgusu kaynak IP adresi gelen bir kullanıcı sorgulama – genellikle bu kullanıcı adına sorgu yapılması yerel DNS Çözümleyicisi IP adresini adıdır bölgeyi belirlemek için kullanır.  

![Azure trafik Yöneticisi 'Coğrafi' trafik yönlendirme yöntemi](./media/traffic-manager-routing-methods/geographic.png)

Trafik Yöneticisi DNS sorgusu kaynak IP adresi bilgilerini okur ve veritabanından kaynaklanan hangi coğrafi bölge karar verir. Ardından bu coğrafi bölgede eşlenmiş sahip bir uç noktası olup olmadığını görmek için görünüyor. Bu arama en düşük ayrıntı düzeyinde (burada, desteklenir, başka ülke/bölge düzeyinde bölge) başlatır ve olan tüm en yüksek düzeye kadar girer **World**. Bu geçişi kullanarak uç noktası olarak sorgu yanıtında dönmek için atanan ilk eşleşme. Bir uç nokta, alt profilindeki bir iç içe türü uç noktasıyla eşleşen döndürüldüğünde, kendi yönlendirme yöntemini esas alarak. Aşağıdaki hususları için bu davranış geçerlidir:

- Yönlendirme türü coğrafi yönlendirme olduğunda bir coğrafi bölge yalnızca trafik Yöneticisi profili, bir uç nokta eşlenebilir. Bu kullanıcıları yönlendirme belirleyici ve müşteriler anlaşılır coğrafi sınırları gerektiren senaryolar etkinleştirebilirsiniz sağlar.
- Kullanıcının bölge altında iki farklı uç noktalar coğrafi eşleme geliyorsa, trafik Yöneticisi uç noktası en düşük ayrıntı düzeyi içeren seçer ve diğer uç nokta bu bölge yönlendirme isteklerini dikkate almaz. Örneğin, coğrafi yönlendirme türü profili iki uç noktaları - bitiş noktası 1 ve Endpoint2 göz önünde bulundurun. Bitiş noktası 1 İrlanda'dan trafiği almak için yapılandırılır ve Endpoint2 Avrupa trafiği almak için yapılandırılır. Bir istek İrlanda'dan kaynaklanıyorsa bitiş noktası 1 için her zaman yönlendirilir.
- Yalnızca bir uç nokta eşlenebilir. bir bölge olduğundan, trafik Yöneticisi, uç nokta sağlıklı olup olmamasına bakılmaksızın döndürür.

    >[!IMPORTANT]
    >Coğrafi yönlendirme yöntemini kullanan müşteriler, her içinde en az iki uç nokta içeren alt profili bulunan iç içe türü uç noktaları ile ilişkilendirme önerilir.
- Bir uç nokta eşleşme bulundu ve bu uç bulunduğu **durduruldu** durumunda, trafik Yöneticisi NODATA yanıt verir. Bu durumda, başka bir arama yapılır yukarıya coğrafi bölge hiyerarşi içinde. Alt profili olduğunda bu davranış da iç içe geçmiş uç nokta türleri için geçerli **durduruldu** veya **devre dışı** durumu.
- Bir uç nokta görüntülerse bir **devre dışı** durumu, işlem eşleştirme bölgede eklenmeyecek. Uç nokta olduğunda bu davranış da iç içe geçmiş uç nokta türleri için geçerli **devre dışı** durumu.
- Bir sorgu, bu profilde eşleme olan coğrafi bölgedeki geliyorsa, trafik Yöneticisi NODATA yanıtı döndürür. Bu nedenle, müşterilerin ideal türünde, iç içe alt profiliyle bölge içinde en az iki uç nokta bir uç nokta ile coğrafi yönlendirme kullandığı önerilir **World** atanmış. Bu, aynı zamanda bir bölgeye eşlemediğinden herhangi bir IP adresi işlenmesini sağlar.

İçinde anlatıldığı gibi [nasıl trafik Yöneticisi çalışır](traffic-manager-how-traffic-manager-works.md), trafik Yöneticisi DNS sorgularını doğrudan istemcilerden almaz. Bunun yerine, DNS sorgularını, istemcilerin kullanmak üzere yapılandırılmış özyinelemeli DNS hizmetinden gelir. Bu nedenle, IP adresi bölge istemcinin IP adresi değil, ancak yinelemeli DNS hizmeti IP adresidir belirlemek için kullanılır. Uygulamada, istemci için iyi bir proxy bu IP adresidir.


## <a name="next-steps"></a>Sonraki adımlar

Kullanarak yüksek kullanılabilirlik uygulamaları geliştirmeyi öğrenin [trafik Yöneticisi uç nokta izleme](traffic-manager-monitoring.md)

Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



