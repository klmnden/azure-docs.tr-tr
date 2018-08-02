---
title: Azure Traffic Manager - trafik yönlendirme yöntemleri | Microsoft Docs
description: Bu Traffic Manager tarafından kullanılan farklı trafik yönlendirme yöntemleri anlamanıza yardımcı makaleler
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
ms.openlocfilehash: 03f1cc3a34fa8a472dcab9654b65cc97b8473993
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398626"
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager yönlendirme yöntemleri

Azure Traffic Manager çeşitli hizmet uç noktaları için ağ trafiğini yönlendirmek üzere nasıl dağıtılacağını belirlemenin dört trafik yönlendirme yöntemlerini destekler. Traffic Manager trafik yönlendirme yöntemini aldığı her DNS sorgusu için geçerlidir. DNS yanıtında hangi uç noktaya trafik yönlendirme yöntemini belirler.

Traffic Manager'daki dört trafik yönlendirme yöntemleri vardır:

* **[Öncelik](#priority):** seçin **öncelik** ne zaman tüm trafiği için birincil hizmet uç noktası ve birincil durumda yedekleme sağlamak istediğiniz veya yedekleme uç noktaları kullanılamıyor.
* **[Ağırlıklı](#weighted):** seçin **ağırlıklı** trafiği bir uç nokta kümesine arasında dağıtmak istediğinizde, eşit veya, ağırlıklara göre tanımlayın.
* **[Performans](#performance):** seçin **performans** zaman uç noktalar farklı coğrafi konumlarda olan ve son kullanıcıların en düşük ağ gecikme süresi açısından "en yakın" uç nokta kullanmasını istediğiniz.
* **[Coğrafi](#geographic):** seçin **coğrafi** DNS sorgularını, böylece kullanıcılar hangi coğrafi konum temelinde belirli Uç noktalara (Azure, dış veya iç içe) yönlendirilir kaynaklanan. Bu, kullanıcının coğrafi bölgede bilerek ve bunları oturum tabanlı yönlendirme önemli olduğu senaryolar etkinleştirmek için Traffic Manager müşterileri güçlendirir. Veri egemenliği mandates içeriği ve kullanıcı deneyimi yerelleştirmesi uymak ve farklı bölgelerden trafik ölçme verilebilir.

Tüm Traffic Manager profillerini uç nokta sistem durumu ve otomatik bir uç nokta yük devretme izleme içerir. Daha fazla bilgi için [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md). Tek bir Traffic Manager profili, yalnızca bir trafik yönlendirme yöntemini kullanabilirsiniz. Herhangi bir zamanda profiliniz için farklı trafik yönlendirme yöntemini seçebilirsiniz. Bir dakika içinde değişiklikler uygulanır ve kapalı kalma süresi olmadan oluşur. Trafik yönlendirme yöntemleri, iç içe Traffic Manager profilleri kullanılarak birleştirilebilir. İç içe geçme daha büyük ve karmaşık uygulamaların gereksinimlerini karşılayan karmaşık ve esnek trafik yönlendirme yapılandırmaları sağlar. Daha fazla bilgi için [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Öncelik trafik yönlendirme yöntemi

Genellikle bir kuruluş, birincil hizmet arıza durumunda bir veya daha fazla yedekleme hizmetlerini dağıtarak hizmetlerinin için güvenilirlik sağlamak istiyor. Azure müşterileri kolayca bu yük devretme deseni uygulamak 'Öncelik' trafik yönlendirme yöntemi sağlar.

![Azure Traffic Manager 'Öncelik' trafik yönlendirme yöntemi][1]

Traffic Manager profili, hizmet uç noktalarının öncelikli listesi içerir. Varsayılan olarak, Traffic Manager tüm trafiği birincil (en yüksek öncelik) uç noktasına gönderir. Birincil uç nokta kullanılabilir durumda değilse, Traffic Manager trafiği ikinci uç noktasına yönlendirir. Birincil ve ikincil uç kullanılabilir durumda değilse, trafiği üçüncü ve benzeri gider. Uç noktanın kullanılabilirliğini yapılandırılan durumu (etkin veya devre dışı) ve devam eden bir uç nokta izleme bağlıdır.

### <a name="configuring-endpoints"></a>Uç noktalarını yapılandırma

Azure Resource Manager ile 'priority' özelliği kullanılarak açıkça her uç nokta için uç nokta önceliği yapılandırın. Bu özellik, 1 ile 1000 arasında bir değerdir. Düşük değerler daha yüksek önceliği temsil eder. Uç noktaları öncelik değerleri paylaşamazsınız. Özelliği isteğe bağlıdır. Atlanırsa, varsayılan uç nokta sıraya göre bir öncelik kullanılır.

##<a name = "weighted"></a>Ağırlıklı trafik yönlendirme yöntemi
'Ağırlıklı' trafik yönlendirme yöntemini trafiği eşit olarak dağıtmak için veya önceden tanımlanmış ağırlığı kullanmayı sağlar.

![Azure Traffic Manager 'Ağırlıklı' trafik yönlendirme yöntemi][2]

Ağırlıklı trafik yönlendirme yöntemini her bir uç nokta Traffic Manager profili yapılandırması için bir ağırlık atayın. Ağırlık 1000 ile 1 arasında bir tamsayı olduğu. Bu parametre isteğe bağlıdır. Atlanırsa, trafik yöneticileri '1' varsayılan ağırlığı kullanır. Daha yüksek ağırlık, öncelik o kadar yüksektir.

Traffic Manager, alınan her DNS sorgusu için kullanılabilir uç nokta rastgele seçer. Bir uç noktasını olasılığını tüm kullanılabilir uç noktaları için atanan ağırlıklara göre faturalandırılır. Genelinde tüm uç noktalar sonuçları bir bile trafik dağılımı ağırlık kullanıyor. Belirli Uç noktalara daha yüksek veya düşük Ağırlıklar kullanılıyor, bu Uç noktalara daha fazla veya daha az sık DNS yanıtlarını döndürülecek neden olur.

Ağırlıklı yöntemin bazı yararlı senaryolara olanak tanır:

* Aşamalı uygulama yükseltme: yeni bir uç noktasına yönlendirme için trafik yüzdesi ayırın ve yavaş yavaş % 100 zaman içinde trafik artırın.
* Uygulamayı azure'a: hem Azure hem de dış uç noktaları ile bir profil oluşturun. Tercih ettiğiniz yeni uç nokta için uç nokta ağırlığı ayarlayın.
* Ek kapasite için bulut Patlaması: hızlı bir şekilde bir Traffic Manager profili yerleştirerek bir şirket içi dağıtım buluta genişletin. Bulutta ek kapasiteye ihtiyaç duyduğunuzda, ekleyin veya daha fazla uç noktalarının etkinleştirilmesi ve trafiğin hangi kısmını her uç noktaya gider belirtin.

Azure Resource Manager portalı ağırlıklı trafik yönlendirme yapılandırmasını destekler.  Resource Manager sürümleri, Azure PowerShell, CLI ve REST API'lerini kullanarak ağırlıkları yapılandırabilirsiniz.

DNS yanıtları, DNS adlarını çözümlemek için istemcilerin kullandığı yinelenen DNS sunucuları ve istemciler tarafından önbelleğe alınır anlamak önemlidir. Bu önbelleğe alma, ağırlıklı trafiği dağıtımlarında bir etkisi olabilir. İstemcileri ve yinelenen DNS sunucuları sayısı büyük olduğunda trafik dağılımı beklendiği gibi çalışır. İstemciler veya yinelenen DNS sunucuları sayısı az olduğunda, ancak önbelleğe alma önemli ölçüde trafik dağılımı eğriltilebilir.

Genel kullanım örnekleri şunlardır:

* Geliştirme ve test ortamları
* Uygulama uygulama iletişimleri
* Bir dar kullanıcı ortak bir özyinelemeli DNS altyapısı (örneğin, bir proxy üzerinden bağlanma şirket çalışanları) paylaşan tabanlı uygulamaları özeliklerin

Bu DNS önbelleğe alma etkileri tüm DNS tabanlı trafik yönlendirme sistemler için yalnızca Azure Traffic Manager yaygındır. Bazı durumlarda, açıkça DNS önbelleği temizleniyor, geçici bir çözüm sağlayabilir. Diğer durumlarda, alternatif bir trafik yönlendirme yöntemi daha uygun olabilir.

## <a name = "performance"></a>Performans trafiği yönlendirme yöntemi

İki veya daha fazla konumda uç noktaları, dünya çapında dağıtma 'size en yakın' konumu için trafiği yönlendirme tarafından birçok uygulama yanıt verme hızını artırabilir. Bu özelliği 'Performans' trafik yönlendirme yöntemini sunar.

![Azure Traffic Manager 'Performans' trafik yönlendirme yöntemi][3]

'En yakın' uç noktası tarafından coğrafi uzaklık ölçülen mutlaka en yakın değil. Bunun yerine, ağ gecikmesi ölçerek en yakın uç nokta 'Performans' trafik yönlendirme yöntemini belirler. Traffic Manager her bir Azure veri merkezi IP adres aralıkları arasında gidiş dönüş süresi izlemek için bir Internet gecikme tablosu tutar.

Traffic Manager, gelen Internet gecikme tablo DNS isteğinin kaynak IP adresi arar. Bu IP adresi aralığı için en düşük gecikme süresine sahip, sonra Bu uç nokta DNS yanıtında döndürür. Azure veri merkezinde kullanılabilir uç nokta traffic Manager'ı seçer.

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](traffic-manager-how-it-works.md), Traffic Manager DNS sorguları doğrudan istemcilerden almaz. Bunun yerine, DNS sorguları, istemcilerin kullanmak üzere yapılandırılmış özyinelemeli DNS hizmetinden gelir. Bu nedenle, 'en yakın' uç noktası, istemcinin IP adresi değil, ancak özyinelemeli DNS hizmeti IP adresi olduğu belirlemek için kullanılan IP adresi. Uygulamada, istemci için iyi bir proxy bu IP adresidir.


Traffic Manager, genel İnternet'e ve yeni Azure bölgeleri değişiklikleri hesaba Internet gecikme tablosu düzenli olarak güncelleştirir. Ancak, uygulama performansı göre gerçek zamanlı çeşitlemeleri yük Internet üzerinden değişir. Performans trafiği yönlendirme belirli hizmet uç noktası üzerindeki yük izlemez. Bir uç nokta kullanılamaz duruma gelirse, ancak, Traffic Manager, DNS sorgu yanıtları içermez.


Dikkat edilecek noktalar:

* Profilinizi aynı Azure bölgesinde birden fazla uç noktası içeriyorsa, ardından Traffic Manager trafik bu bölgede kullanılabilir uç noktalar arasında dağıtır. Bir bölge içinde farklı trafik dağıtım tercih ederseniz, kullanabileceğiniz [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md).
* Traffic Manager trafiği en yakın Azure bölgesine etkinleştirilmiş tüm uç noktalar düşürülmüş ise sonraki en yakın Azure bölgesinde uç noktalarına taşır. Tercih edilen bir yük devretme sırası tanımlamak istediğiniz kullanırsanız [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md).
* Performans trafiği yönlendirme metodunu dış uç noktalar veya iç içe uç noktaları ile kullanırken, bu Uç noktalara konumunu belirtmek gerekir. Dağıtımınız için en yakın Azure bölgesi seçin. Konumların Internet gecikme tablo tarafından desteklenen değerler.
* Uç nokta seçtiği belirleyici algoritmasıdır. Yinelenen DNS sorguları aynı istemciden aynı uç noktaya yönlendirilir. Genellikle, istemciler farklı yinelenen DNS sunucuları dolaşırken kullanır. İstemci, farklı bir uç noktasına yönlendirilebilir. Yönlendirme de Internet gecikme tabloya yapılan güncelleştirmeler etkilenebilir. Bu nedenle, performans trafiği yönlendirme yöntemini bir istemci her zaman aynı uç noktaya yönlendirilir garantilemez.
* Internet gecikme tablosu değiştiğinde, bazı istemciler farklı bir uç noktaya yönlendirilir fark edebilirsiniz. Bu yönlendirme değişiklik geçerli gecikme verilere göre daha doğru olur. Bu güncelleştirmeler, Internet sürekli geliştikçe performans trafiği yönlendirme doğruluğunu sağlamak için gereklidir.

## <a name = "geographic"></a>Coğrafi trafik yönlendirme yöntemi

Traffic Manager profilleri, böylece kullanıcılar kendi DNS sorgusu kaynaklandığı hangi coğrafi konum tabanlı belirli Uç noktalara (Azure, dış veya iç içe) yönlendirilir coğrafi yönlendirme yöntemini kullanmak için yapılandırılabilir. Bu, kullanıcının coğrafi bölgede bilerek ve bunları oturum tabanlı yönlendirme önemli olduğu senaryolar etkinleştirmek için Traffic Manager müşterileri güçlendirir. Veri egemenliği mandates içeriği ve kullanıcı deneyimi yerelleştirmesi uymak ve farklı bölgelerden trafik ölçme verilebilir.
Bir profili devre dışı coğrafi yönlendirme için yapılandırıldığında, her bir uç nokta profili coğrafi bölgesinde atanmış bir dizi sahip olması gerektiğini ilişkili. Aşağıdaki ayrıntı düzeylerinde bir coğrafi bölgede olabilir 
- Dünya – herhangi bir bölge
- Bölgesel gruplandırma – örneğin Afrika, Orta Doğu, Avustralya/Pasifik vs. 
- Örneğin, İrlanda, ülke/bölge – Peru, Hong Kong ÖİB vs. 
- Eyalet/il – örneğin California ABD, Avustralya-Queensland, Kanada Alberta vb. (Not: Bu ayrıntı düzeyi yalnızca durum için desteklenen / il Avustralya, Kanada, Birleşik Krallık ve ABD).

Bir bölge veya bölge kümesinin bir uç noktasına atandığında, tüm istekler bu bölgelerden yönlendirilmiş yalnızca bu uç noktaya. Traffic Manager, gelen bir kullanıcı sorgulama: genellikle bu kullanıcı adına sorgu yapmadan yerel DNS Çözümleyicisi IP adresi olan bölgeyi belirlemek için DNS sorgusu kaynak IP adresini kullanır.  

![Azure Traffic Manager 'Coğrafi' trafik yönlendirme yöntemi](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager DNS sorgusu kaynak IP adresini okur ve veritabanından kaynaklanan hangi coğrafi bölgeyi belirler. Ardından bu coğrafi bölgede eşlenmiş olan bir uç noktası olup olmadığını görmek için görünüyor. En düşük ayrıntı düzeyinde (burada, desteklenir, aksi takdirde ülke/bölge düzeyinde eyalet/il) bu aramayı başlatır ve olan tüm en yüksek düzeye kadar **dünya**. Bu geçişi kullanarak uç noktası olarak sorgu yanıtına döndürülecek atanır ilk eşleşme bulundu. Bu alt profilinde bir uç nokta bir iç içe türü uç noktasıyla eşleşen döndürüldüğünde, yönlendirme yöntemini temel. Aşağıdaki noktaları için bu davranış geçerlidir:

- Yönlendirme türü coğrafi yönlendirme olduğunda yalnızca bir Traffic Manager profilinde bir uç nokta için bir coğrafi bölgede eşlenebilir. Bu kullanıcıları yönlendirme kararlı ve müşterilerin benzersiz coğrafi sınırlar gerektiren senaryolar etkinleştirebilirsiniz sağlar.
- Bir kullanıcının bölge iki farklı uç noktaların coğrafi eşleme geliyorsa, Traffic Manager uç noktası ile en düşük ayrıntı düzeyi seçer ve bu bölgedeki başka bir uç noktası yönlendirme isteklerini dikkate almaz. Örneğin, bir coğrafi yönlendirme türü profili ile iki uç nokta - bitiş noktası 1 ve Endpoint2 göz önünde bulundurun. Bitiş noktası 1, İrlanda'dan trafiği almak için yapılandırılır ve Endpoint2 Avrupa'dan trafiği almak için yapılandırılır. İrlanda'dan bir isteğin kaynaklandığı, her zaman için bitiş noktası 1 yönlendirilir.
- Bir bölge yalnızca bir uç noktaya eşlenebilecek olduğundan, Traffic Manager uç nokta iyi durumda olup olmamasına bakılmaksızın döndürür.

    >[!IMPORTANT]
    >Coğrafi yönlendirme yöntemini kullanan müşteriler, her içinde en az iki uç noktaları içeren alt profil iç içe türü uç noktası ile ilişkilendirdiğinizden de önemle tavsiye edilir.
- Bir uç nokta eşleşme bulundu ve bu uç noktaya **durduruldu** durumunda, Traffic Manager NODATA yanıt verir. Bu durumda, başka hiçbir arama yapılır yukarıya coğrafi bölgede hiyerarşi içinde. Alt profili olduğunda bu davranış da iç içe uç nokta türleri için geçerli **durduruldu** veya **devre dışı bırakılmış** durumu.
- Bir uç nokta görüntülerse bir **devre dışı bırakılmış** durumu, işlem eşleştirme bölgede dahil edilmeyecektir. Uç nokta olduğunda bu davranış da iç içe uç nokta türleri için geçerli **devre dışı bırakılmış** durumu.
- Sorgu hiçbir eşleme profilde sahip bir coğrafi bölgesinden geliyorsa, Traffic Manager NODATA yanıtı döndürür. Bu nedenle, müşterilerin ideal olarak alt profiliyle bölge içinde en az iki uç noktaları ile iç içe türü, bir uç nokta ile coğrafi yönlendirme kullanmak önerilir **dünya** atanmış. Bu, ayrıca bir bölgeye eşlemeyin herhangi bir IP adresi işlenmesini sağlar.

İçinde anlatıldığı gibi [Traffic Manager nasıl çalışır](traffic-manager-how-it-works.md), Traffic Manager DNS sorguları doğrudan istemcilerden almaz. Bunun yerine, DNS sorguları, istemcilerin kullanmak üzere yapılandırılmış özyinelemeli DNS hizmetinden gelir. Bu nedenle, kullanılan IP adresini bölge istemcinin IP adresi değil, ancak özyinelemeli DNS hizmeti IP adresi olduğu belirlemek için. Uygulamada, istemci için iyi bir proxy bu IP adresidir.


## <a name="next-steps"></a>Sonraki adımlar

Kullanarak yüksek kullanılabilirlik uygulamalarını nasıl geliştirebileceğinizi öğrenin [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md)

Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



