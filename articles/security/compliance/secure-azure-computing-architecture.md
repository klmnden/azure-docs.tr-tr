---
title: Azure bilgi işlem mimarisi güvenli
description: Başvuru mimarisi için ağ sanal Gereçleri ve diğer araçları kullanarak bir kurumsal düzeyde bir DMZ mimari budur. Bu mimari Savunma Bakanlığı 's güvenli bulut bilgi işlem mimarisi işlevsel gereksinimlerini karşılamak üzere tasarlanmıştır. Ancak, tüm kuruluş için yararlanılabilir. Bu başvuru, Citrix veya F5 Gereçleri kullanarak iki otomatik seçenekleri içerir.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: f2e3d72db3f29dbc6d03b3259acb18daf684fb12
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917599"
---
# <a name="secure-azure-computing-architecture"></a>Azure bilgi işlem mimarisi güvenli

DoD müşteriler iş yüklerini Azure'a dağıtma hızla artan bir dizi güvenli Sanal ağları ayarlama ve güvenlik araçları ve Hizmetleri DoD standartlar ve uygulama tarafından belirlenen Yapılandırma Kılavuzu istediği. Yayımlanan DISA [güvenli bulut bilgi işlem mimarisi (SCCA) işlevsel gereksinimlerini belge](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) 2017'de. Derinlemesine bilgi sistemi ağın (DISN) güvenliğini sağlamak için işlevsel hedeflerini SCCA açıklar ve ticari bulut sağlayıcısı bağlantı noktaları ve nasıl bağlantı sınırında sahipleri güvenli bulut uygulamalarını görev. Ticari buluta bağlanan her DoD varlık içinde SCCA FRD ortaya konan yönergelerine uyduğundan emin uygulanan.
 
Dört SCCA bileşenleri vardır. Sınır bulut erişim noktası (BCAP), sanal veri merkezi güvenlik yığını (VDSS), sanal veri merkezi Hizmetleri (VDMS) yönetilen ve güvenilir bulut kimlik bilgileri Yöneticisi (TCCM). Microsoft Azure'da çalışan IL4 hem IL5 iş yükleri için SCCA gereksinimlerini karşılayacak bir çözüm geliştirdi. Bu Azure belirli çözüm güvenli Azure bilgi işlem mimarisi (SACA) olarak adlandırılır. SACA dağıtan müşteriler, SCCA FRD uyumlu olacak ve DoD müşteriler iş yüklerini bağlandığında Azure'a taşımak etkinleştirir. 

SCCA rehberlik ve mimariler DoD müşterilerine özgü olsa da, en son düzeltmeleri SACA de güvenilir Internet bağlantısı (TIC) yönergeleriyle sivil davranmalarına yanı sıra, güvenli bir DMZ'ye HTTPS'ye uygulamak istediğiniz ticari müşterilere yardımcı olur azure ortamlarını koruyun.


## <a name="secure-cloud-computing-architecture-components"></a>Güvenli bulut bilgi işlem mimarisi bileşenleri

**BCAP**

DISN bulut ortamında saldırılardan korumak için BCAP amacı olan. BCAP izinsiz giriş algılama ve önleme gerçekleştirmek yanı sıra yetkisiz trafik çıkış filtreleyin. Bu bileşen SCCA diğer bileşenlerle birlikte bulunabilir. Fiziksel donanım kullanarak, bu bileşeni dağıttığınız önemle tavsiye edilir. BCAP güvenlik gereksinimlerinin listesi aşağıda bulabilirsiniz.

***BCAP güvenlik gereksinimleri***

![BCAP gereksinimleri Matrisi](media/bcapreqs.png)


**VDSS**

VDSS amacı, Azure'da barındırılan DoD görev sahibi uygulamaları korunmasını sağlamaktır. VDSS güvenlik işlemleri, toplu SCCA gerçekleştirir. Azure'da çalışan uygulamaların güvenliğini sağlamak için trafik denetimi gerçekleştir. Bu bileşen, Azure ortamınızda sağlanabilir.

***VDSS güvenlik gereksinimleri***

![VDSS gereksinimleri Matrisi](media/vdssreqs.png)

**VDMS**

VDMS amacı ana bilgisayar güvenliğini sağlamaktır yanı sıra paylaşılan veri merkezi Hizmetleri. VDMS işlevlerini, SCCA hub'ında çalıştırabilirsiniz veya iş açısından sahibi kendi belirli bir Azure aboneliğinde bu parçaları dağıtabilirsiniz. Bu bileşen, Azure ortamınızda sağlanabilir.

***VDMS güvenlik gereksinimleri***

![VDMS gereksinimleri Matrisi](media/vdmsreqs.png)


**TCCM**

TCCM iş rolüdür. Bu kişi SCCA yönetmekten sorumlu olacaktır. Planları ve kimlik sağlayarak bulut ortamında, hesap erişim ilkeleri oluşturma, görevlerini içerir ve erişim yönetimi düzgün bir şekilde, çalıştırma ve bulut kimlik yönetimi planlama korumak için. Bu kişi yetkilendirme resmi tayin. BCAP VDSS ve VDMS TCCM iş işlevleri gerçekleştirmek gereken yetenekleri sağlar.

***TCCM güvenlik gereksinimleri***

![TCCM gereksinimleri Matrisi](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>SACA bileşenleri ve planlama konuları 

SACA başvuru mimarisi, yanı sıra azure VDSS ve VDMS bileşenlerini dağıtın TCCM etkinleştirmek için tasarlanmıştır. Bu mimari modüler, tüm parçaları VDSS VDMS ve merkezi bir hub'ında live veya bazı denetimleri görev sahibi alanı veya hatta şirket içi karşılanacağı anlamına gelir. Microsoft ekibimiz, VDSS ve VDMS bileşenleri bir merkez sanal aracılığıyla tüm görev açısından sahipleri bağlanabilen Net içine birlikte bulundur önerilir. Aşağıdaki diyagramda, önerilen mimarimiz gösterilmektedir. 


![SACA başvuru mimarisi diyagramı](media/sacav2generic.png)

SCCA uymalarını zorunlu tutar stratejisi ve teknik mimari planlarken dikkate alınması gereken birçok şey vardır. Her müşteri bu kapsayacak şekilde gerekeceğinden aşağıdaki konulara, baştan dikkate alınır önemlidir. Aşağıdaki konular, gerçek DoD müşterilerle ulaştık ve planlama ve yürütme yavaşlamasına eğilimli sorunları olmuştur. 

- Kuruluşunuz hangi BCAP kullanacak mısınız?
    - DISA BCAP
        - DISA üçüncü çevrimiçi yakında ile Pentagon ve kampı yakın bir CA, iki işletimsel BCAPs sahiptir. 
        - DISA BCAPs tüm bağlantı DoD müşteriler tarafından kullanılan Azure ExpressRoute devresine sahip. 
        - DISA, bir kurumsal düzeyde Microsoft Peering oturumu zaten Office 365 gibi Microsoft SaaS araçları abone olmak istediğiniz DoD müşteriler için vardır. DISA BCAP kullanarak, bağlantı ve SACA Örneğinize eşleme etkinleştirebilirsiniz. 
    - Kendi BCAP oluşturun
        - Bu alanı bir birlikte bulunan veri merkezindeki kiralama ve Azure ExpressRoute devresine Kurulum gerektirir. 
        - Bu seçeneğin ek onay gerektirir 
        - Ek onay ve fiziksel yapımı nedeniyle, bu seçenek en çok zaman alır. 
    - Microsoft'un DISA BCAP kullanmak için önerilir. Bu seçenek, kullanıma hazır, yedeklilik içinde yerleşik olan ve üzerinde üretimde çalışan bugün müşteriler zaten sahip.
- DoD yönlendirilebilir IP alanı
    - DoD yönlendirilebilir IP alanı, ucuna kullanmak için gerekli olacaktır. NAT seçeneği bu azure'da özel IP alanı için kullanılabilir.  
    - DoD IP alanı almak için NIC başvurun ek gönderiminiz DISA ile bir parçası olarak gerekli olacaktır. 
    - Özel adres alanınızı azure'da NAT ile planlıyorsanız, en az bir/24 gerekir alt ağ adres alanının nıc'den SACA dağıtmayı planladığınız her bir bölgesi için atanmış. 
- Yedeklilik 
    - Microsoft, en az iki bölgeleri için SACA örneği dağıtma önerir. DoD bulutta bu iki kullanılabilir DoD bölgeler için dağıtmadan anlamına gelir. 
    - Ayrıca, en az iki BCAPs ayrı ExpressRoute bağlantı hatları aracılığıyla bağlanmak önerilir. Her iki Hızlı yol, ardından her bölgenin SACA örneğine bağlanabilir. 
- DoD bileşen özgü gereksinimler
    - Kuruluşunuz dışında SCCA gereksinimleri özel gereksinimleri var mı? (Bazı kuruluşların belirli IP'ler gereksinimleri vardır)
- SACA modüler bir mimaridir  
    - Ortamınız için hangi bileşenlerin ihtiyacınız kullanın. 
        - Bir tek katmanlı veya çok katmanlı nva'ları dağıtma
        - IP'ler tümleşik veya kendi IP'LERİNİZİ Getir
- Uygulamalarınız ve verileriniz, Savunma Bakanlığı etki düzeyi
    - Bizim IL5 bölgelerde çalışan uygulamaların olasılığı varsa SACA Örneğinizde IL5 yapı önerilir. Örnek IL5 yanı sıra IL4 uygulamalar önünde kullanılabilir. Ancak IL4 SACA örneğini önündeki IL5 uygulamanın büyük olasılıkla akreditasyonu almaz. 
- Hangi ağ sanal Gereci satıcı VDSS için kullanacak mısınız?
    - Daha önce bahsedildiği gibi çeşitli cihazları ve Azure hizmetlerini kullanarak bu SACA başvuru oluşturulabilir. Ancak, F5 hem Citrix SACA mimariyi dağıtmak için otomatik çözüm şablonları sahibiz. Bu çözümler aşağıda daha ayrıntılı olarak ele alınacaktır. 
- Hangi Azure hizmetlerinin kullanacaksınız?
    - Log analytics, ana bilgisayar tabanlı koruma ve Kimliklerini işlevi gereksinimlerini karşılayan Azure hizmetleri de vardır. Ancak, bazı hizmetleri bizim IL5 bölgelerde genel kullanıma açık olmayan mümkündür. Bu, bu Azure hizmetlerini, gereksinimi karşılamıyorsa, bazı 3. taraf araçları kullanmak için gereken neden olabilir. Hangi alışık olduğunuz ve Azure yerel araçları kullanmanın Uygulanabilirlik araçları konumunda aramak gerekir. 
    - Bu kadar Azure yerel Araçlar tüm bulut güvenliği göz önünde bulundurularak oluşturulur ve Azure platformunun geri kalanıyla sorunsuz şekilde tümleştirin olabildiğince kullandığınız Microsoft'un önerilir. Çeşitli SCCA gereksinimlerini karşılamak için yararlanılabilir Azure yerel Araçlar listesi aşağıdadır. 
        - [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview )
        - [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) 
        - [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
        - [Azure Anahtar Kasası.](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
        - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
        - [Application Gateway](https://docs.microsoft.com/azure/application-gateway/overview)
        - [Azure güvenlik duvarı](https://docs.microsoft.com/azure/firewall/overview) 
        - [Azure ön kapısı](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
        - [Güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview)
        - [Azure DDoS koruması](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
        - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Boyutlandırma
    - Boyutlandırma alıştırma tamamlanması gerekir. Ağ aktarım hızı gereksinimleri yanı sıra SACA örneği olabilir eş zamanlı bağlantı sayısını bakmanız gerekir. 
    - Bunu sanal makinelerin boyutu için Yardım yanı sıra gibi SACA Örneğinizde kullanarak farklı satıcıların sunduğu gerekli lisanslar belirlemek için bu önemli bir adımdır. 
    - İyi maliyet analizi yapılamaz boyutlandırma alıştırmanın, ayrıca her şeyi boyutta doğru şekilde en iyi performans için izin verecek şekilde sağlamak önemlidir. 


## <a name="most-common-deployment-scenario"></a>En yaygın dağıtım senaryosu

Microsoft tam zaten ilerlemiş çeşitli müşteriler sahip dağıtım veya en az SACA ortamlarını aşamalarını planlama. Bu en yaygın dağıtım senaryosu öngörü almak etmemizi de sağladı. Aşağıdaki diyagramda, en yaygın mimari gösterilmektedir. 


![SACA başvuru mimarisi diyagramı](media/sacav2commonscenario.png) 


Diyagramdan görebileceğiniz gibi DoD müşteriler bu ömrü sona erecek şirket ve diğer olduğu Doğu Yakası üzerinde DISA BCAPs ikilisi için genellikle abone olun. Bir ExpressRoute özel eşdüzey hizmet sağlayıcı her DISA BCAP konumundan Azure etkinleştirilir. Ardından bu ExpressRoute eş sanal ağ geçidi DoD Doğu ve DoD Orta Azure bölgeleri içinde bağlanır. DoD Doğu ve DoD Orta Azure bölgesi ve tüm giriş ve çıkış trafiği akışı onun üzerinden Express Route bağlantı için DISA BCAP SACA örneği dağıtılır. 

Görev sahibi uygulamaların hangi Azure alımımın uygulamalarında dağıtımını yapacaksınız seçin ve kullanım sanal ağ eşlemesi uygulamasına bağlanmak için kullanıcının SACA sanal ağa sanal ağ. Tüm bunların trafiğini VDSS örneği üzerinden zorlamalı tünel uygulama. 

Bu mimari, Microsoft tarafından SCCA gereksinimlerini karşılayacak, yüksek oranda kullanılabilir, kolayca ölçeklenebilir ve dağıtımını ve yönetimini basitleştirir önerilir.

## <a name="automated-saca-deployment-options"></a>Otomatik SACA dağıtım seçenekleri

 Daha önce Microsoft otomatik bir SACA altyapı şablonu oluşturmak için iki satıcılarla ortak çalışma yürütmektedir bahsedilen. İki şablon, aşağıdaki Azure bileşenlerini dağıtır. 

- SACA sanal ağ
    - Yönetim alt ağı
        - Yönetim Vm'leri ve Hizmetleri dağıtıldığı (atlama kutularının)
        - VDMS alt ağ
            - VM'ler ve hizmetler için VDMS kullanılan dağıtıldığı
        - Güvenilmeyen ve güvenilir bir alt ağlar 
            - Sanal gereçler dağıtıldığı
        - Ağ Geçidi Alt Ağı
            - ExpressRoute ağ geçidi dağıtılacağı
- Yönetim atlama kutusunda sanal makineler
    - Bant dışı yönetim için ortamın kullanılır.
- Ağ sanal Gereçleri
    - Citrix veya F5 tuşuna dağıttığınız şablona bağlı.
- Ortak IP'ler
    - ExpressRoute çevrimiçi duruma getirilene kadar ön uç için kullanılır. Bu IP'ler arka uç Azure özel adres alanınızı çevirir
- Yönlendirme Tabloları 
    - Bu rota tabloları zorlamalı tünel Otomasyon sırasında tüm trafiği sanal gereç aracılığıyla uygulanır.
- Azure yük dengeleyicileri - standart SKU
    - Bu cihazları arasında trafiğin yükünü dengelemek için kullanılır
- Ağ Güvenlik Grupları
    - Hangi trafik türlerini belirli uç noktalarına erişebilen denetlemek için kullanılan


**Citrix SACA dağıtım**

Citrix yüksek oranda kullanılabilir Citrix ADC gereçlerinde iki katman dağıtan bir dağıtım şablonu oluşturdu. Bu mimari VDSS gereksinimlerini karşılar. 

![Citrix SACA diyagramı](media/citrixsaca.png)


Citrix belgelerine ve dağıtım betiğini bulunabilir [burada.](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca)


 **F5 SACA dağıtım**

F5, iki farklı mimari kapsayan iki ayrı dağıtım şablonları oluşturdu. İlki, yüksek oranda kullanılabilir etkin-etkin yapılandırmada F5 gereçlerinde yalnızca bir katman vardır. Bu mimari VDSS gereksinimlerini karşılar. İkincisi, yüksek oranda kullanılabilir etkin-etkin F5s ikinci bir katmanı ekler. Bu ikinci katman amacı, müşterilerin kendi IP'ler F5 katmanlar arasında F5 ayrı eklemek izin vermektir. Belirli IP'ler kullanım için önceden tanımlanmış tüm DoD bileşenleri vardır. Bu durumda, F5 gereçlerinde tek katman için en beri mimarisi F5 cihazlarda IP'ler içerdiğini çalışır.  

![Citrix SACA diyagramı](media/f5saca.png)

F5 belgeleri ve dağıtım betiğini bulunabilir [burada.](https://github.com/f5devcentral/f5-azure-saca) 












