---
title: Güvenli Azure Bilgi İşlem Mimarisi
description: Bu başvuru mimarisi için kurumsal düzeyde bir çevre ağı mimarisi, ağ sanal Gereçleri ve diğer araçları kullanır. Bu mimari Savunma Bakanlığı 's güvenli bulut bilgi işlem mimarisi işlevsel gereksinimlerini karşılamak üzere tasarlanmıştır. Tüm kuruluşlar için de kullanılabilir. Bu başvuru, Citrix veya F5 gereçler kullanan iki otomatik seçenekleri içerir.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: 017a26d5672f666d4d8eaf629a0f53fe0cfe517f
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963238"
---
# <a name="secure-azure-computing-architecture"></a>Güvenli Azure Bilgi İşlem Mimarisi

ABD Savunma Bakanlığı (DoD) müşteriler iş yüklerini Azure'a dağıtmak, güvenli Sanal ağları ayarlayın ve DoD standartlar ve uygulama tarafından belirlenen hizmetler ve Güvenlik Araçları'nı yapılandırmak Kılavuzu istediniz. 

Derinlemesine bilgi sistemi Agency (DISA) yayımlanan [güvenli bulut bilgi işlem mimarisi (SCCA) işlevsel gereksinimlerini belge (FRD)](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) 2017'de. SCCA derinlemesine bilgi sistemi ağın (DISN) ve ticari bulut sağlayıcısı bağlantı noktaları güvenliğini sağlamak için işlevsel hedefler açıklanmaktadır. Nasıl iş açısından sahipleri güvenli bulut uygulamalarına bağlantı sınırında SCCA da açıklar. Ticari buluta bağlanan her DoD varlık içinde SCCA FRD ortaya konan yönergelere uyması gerekir.
 
SCCA dört bileşenden oluşur:
 
- Sınır bulut erişim noktası (BCAP)
- Sanal veri merkezi güvenlik yığını (VDSS)
- Yönetilen sanal Veri Merkezi hizmeti (VDMS)
- Güvenilir bulut kimlik bilgileri Yöneticisi (TCCM) 

Microsoft Azure'da çalıştırmak IL4 hem IL5 iş yükleri için SCCA gereksinimleri karşılayacak bir çözüm geliştirdi. Bu Azure özgü çözüm güvenli Azure bilgi işlem mimarisi (SACA) olarak adlandırılır. SACA dağıtan müşteriler, uygun SCCA FRD ' dir. Bunlar, DoD müşteriler bunların bağlandıktan sonra iş yüklerini Azure'a taşımak etkinleştirebilirsiniz.

SCCA rehberlik ve mimariler DoD müşterilerine özgü, ancak en son düzeltmeler SACA Yardım sivil müşterilere güvenilir Internet bağlantısı (TIC) yönergeleriyle uyumlu. En son düzeltmeler, ayrıca Azure ortamlarını korumak için güvenli DMZ uygulamak istediğiniz ticari müşterilere yardımcı olur.


## <a name="secure-cloud-computing-architecture-components"></a>Güvenli bulut bilgi işlem mimarisi bileşenleri

### <a name="bcap"></a>BCAP

BCAP amacı DISN bulut ortamında gerçekleştirilen saldırılardan koruma sağlamaktır. Yetkisiz giriş algılama ve önleme BCAP gerçekleştirir. Ayrıca, yetkisiz trafik çıkış filtreler. Bu bileşen SCCA diğer bileşenlerle birlikte bulunabilir. Bu bileşen, fiziksel donanım kullanarak dağıtma öneririz. BCAP güvenlik gereksinimleri aşağıdaki tabloda listelenmiştir.

#### <a name="bcap-security-requirements"></a>BCAP güvenlik gereksinimleri

![BCAP gereksinimleri Matrisi](media/bcapreqs.png)


### <a name="vdss"></a>VDSS

VDSS amacı, Azure'da barındırılan DoD görev sahibi uygulamaları korunmasını sağlamaktır. VDSS güvenlik işlemleri, toplu SCCA gerçekleştirir. Bu, Azure'da çalışan uygulamaları güvenli hale getirmek için trafik denetimi yapar. Bu bileşen, Azure ortamınızda sağlanabilir.

#### <a name="vdss-security-requirements"></a>VDSS güvenlik gereksinimleri

![VDSS gereksinimleri Matrisi](media/vdssreqs.png)

### <a name="vdms"></a>VDMS

VDMS amacı ana bilgisayar güvenliğini sağlar ve paylaşılan veri merkezi Hizmetleri. VDMS işlevlerini, SCCA hub'ında çalıştırabilirsiniz veya iş açısından sahibi kendi belirli bir Azure aboneliğinde bu parçaları dağıtabilirsiniz. Bu bileşen, Azure ortamınızda sağlanabilir.

#### <a name="vdms-security-requirements"></a>VDMS güvenlik gereksinimleri

![VDMS gereksinimleri Matrisi](media/vdmsreqs.png)


### <a name="tccm"></a>TCCM

TCCM iş rolüdür. Bu kişi SCCA yönetmekten sorumludur. Görevleri vardır: 

- Plan ve hesap erişim bulut ortamı için ilkeler oluşturun. 
- Kimlik ve erişim yönetimi düzgün çalıştığından emin olun. 
- Bulut kimlik bilgileri yönetim planı korur. 

Bu kişi tarafından yetki verme resmi tayin. BCAP VDSS ve VDMS TCCM işlerini gerçekleştirmek için gereken yetenekleri sağlar.

#### <a name="tccm-security-requirements"></a>TCCM güvenlik gereksinimleri

![TCCM gereksinimleri Matrisi](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>SACA bileşenleri ve planlama konuları 

SACA başvuru mimarisinde, azure'da VDSS ve VDMS bileşenlerini dağıtmak ve TCCM etkinleştirmek için tasarlanmıştır. Bu mimari modülerdir. Tüm parçaları VDSS VDMS ve merkezi bir hub'ında Canlı çalıştırabilirsiniz. Bazı denetimler, görev sahibi alanı veya hatta şirket içi karşılanabilir. Microsoft, VDSS ve VDMS bileşenleri tüm görev açısından sahipleri üzerinden bağlanabilir merkezi bir sanal ağ içinde birlikte bulundur olmasını önerir. Aşağıdaki diyagram bu mimari gösterilir: 


![SACA başvuru mimarisi diyagramı](media/sacav2generic.png)

SCCA uymalarını zorunlu tutar stratejisi ve teknik mimari planlarken, bunlar her müşteri etkilediğinden baştan aşağıdaki konuları göz önünde bulundurun. Aşağıdaki sorunlar, DoD müşterilerle ulaştık ve planlama ve yürütme yavaş eğilimindedir. 

#### <a name="which-bcap-will-your-organization-use"></a>Kuruluşunuz hangi BCAP kullanacak mısınız?
   - DISA BCAP:
        - DISA iki işletimsel BCAPs Pentagon ve kampı yakın, CA vardır. Üçüncü bir kısa süre içinde çevrimiçi olması planlanmaktadır. 
        - Azure ExpressRoute devreleri için bağlantı DoD müşteriler tarafından kullanılabilen Azure DISA BCAPs tüm vardır. 
        - DISA, Office 365 gibi bir hizmet (SaaS) araçları olarak Microsoft yazılımları için abone olmak istediğiniz DoD müşteriler için bir kurumsal düzeyde Microsoft eşleme oturumu vardır. DISA BCAP kullanarak, bağlantı ve SACA Örneğinize eşleme etkinleştirebilirsiniz. 
    - Kendi BCAP derleme:
        - Bu seçenek, birlikte bulunan veri merkezi alanında kiralama ve Azure ExpressRoute bağlantı hattı kurmak gerektirir. 
        - Bu seçenek, ek onay gerektirir. 
        - Ek onay ve fiziksel bir yapı çıkış nedeniyle, bu seçenek en çok zaman alır. 
    - DISA BCAP kullanmanızı öneririz. Bu seçenek, kullanıma hazır, yerleşik yedeklilik var ve üzerinde bugün üretimde çalışan müşterilerin sahip.
- DoD yönlendirilebilir IP alanı:
    - Ucunuzdaki DoD yönlendirilebilir IP alanı kullanmanız gerekir. Azure'da özel IP alanı bu boşlukları bağlanmak için NAT'ı kullanma seçeneği kullanılabilir.
    - IP alanı almak için DoD Ağ Bilgi Merkezi'nı (NIC) başvurun. Sistem/ağ onay işlemi (ek) gönderiminiz DISA ile bir parçası olarak gerekir. 
    - Azure'da özel adres alanınızı bağlamak için NAT'ı kullanmayı planlıyorsanız, en az bir/24 gereken alt ağ adres alanının SACA dağıtma planladığınız her bölge için nıc'den atanmış.
- Yedeklilik:
    - SACA örneği en az iki bölgeye dağıtın. DoD bulutta, hem kullanılabilir DoD bölgeler için dağıtmadan.
    - En az iki BCAPs ayrı ExpressRoute bağlantı hatları aracılığıyla bağlanın. Her iki ExpressRoute bağlantı sonra her bölgenin SACA örneğe bağlanabilir. 
- DoD bileşen özgü gereksinimler:
    - Kuruluşunuz dışında SCCA gereksinimleri özel gereksinimleri var mı? Bazı kuruluşların belirli IP'ler gereksinimleri vardır.
- SACA modüler bir mimaridir:
    - Yalnızca ortamınız için ihtiyacınız olan bileşenleri kullanın. 
        - Tek katmanlı veya çok katmanlı ağ sanal gereçlerini dağıtma.
        - Tümleşik IP'ler veya Getir kendi IP'ler kullanın.
- Uygulamalarınız ve verileriniz, Savunma Bakanlığı etki düzeyi:
    - SACA Örneğinizde IL5 Microsoft IL5 bölgelerde çalışan uygulamaların olasılığı varsa oluşturun. Örnek IL4 uygulamalar ve IL5 önünde kullanılabilir. IL5 uygulamanın büyük olasılıkla önünde IL4 SACA örneği akreditasyonu almazsınız.

#### <a name="which-network-virtual-appliance-vendor-will-you-use-for-vdss"></a>Hangi ağ sanal Gereci satıcısı için VDSS kullanacak mısınız?
Daha önce bahsedildiği gibi çeşitli cihazları ve Azure hizmetlerini kullanarak bu SACA başvurusu oluşturabilirsiniz. Microsoft Çözüm şablonları F5 hem Citrix SACA mimariyi dağıtmak için otomatik hale getirmiştir. Bu çözümler, aşağıdaki bölümde ele alınmıştır.

#### <a name="which-azure-services-will-you-use"></a>Hangi Azure hizmetlerinin kullanacaksınız?
- Log analytics, ana bilgisayar tabanlı koruma ve Kimliklerini işlevselliği gereksinimlerini karşılayan Azure hizmetleri de vardır. Bazı hizmetler Microsoft IL5 bölgelerde genel kullanıma açık olmayan mümkündür. Bu durumda, bu Azure hizmetlerini gereksinimlerinizi karşılıyorsa, üçüncü taraf araçları kullanmanız gerekebilir. Alışık olduğunuz araçları ve Azure yerel araçları kullanmanın Uygulanabilirlik bakın.
- Mümkün olduğu kadar Azure yerel Araçlar kullanmanızı öneririz. Bunlar, güvenlikten ödün bulut tasarlanmaları ve Azure platformunun geri kalanıyla sorunsuz şekilde tümleştirin. Azure yerel Araçlar çeşitli SCCA gereksinimlerini karşılamak için aşağıdaki listede kullanın:

    - [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview )
    - [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) 
    - [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
    - [Azure Anahtar Kasası.](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
    - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
    - [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/overview)
    - [Azure güvenlik duvarı](https://docs.microsoft.com/azure/firewall/overview) 
    - [Azure ön kapısı](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
    - [Azure güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview)
    - [Azure DDoS koruması](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
    - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Boyutlandırma
    - Boyutlandırma alıştırma tamamlanması gerekir. Ağ aktarım hızı gereksinimleri SACA örneği ile olabilir eş zamanlı bağlantı sayısını bakın. 
    - Bu adım önemlidir. Sanal makinelerin boyutu ve SACA Örneğinizde kullandığınız farklı satıcıların sunduğu gerekli olan lisansları tanımlamak için yardımcı olur. 
    - İyi maliyet analizi boyutlandırma alıştırmanın gerçekleştirilemez. Doğru boyutlandırma en iyi performans için de sağlar. 


## <a name="most-common-deployment-scenario"></a>En yaygın dağıtım senaryosu

 Çeşitli Microsoft müşterilerine aracılığıyla tam dağıtım veya sahiplikten planlama SACA ortamlarını aşamalarında en az. Yaşadıkları deneyimleri en yaygın dağıtım senaryosu bir anlayış göstermiştir. Aşağıdaki diyagramda, en yaygın mimari gösterilmektedir: 


![SACA başvuru mimarisi diyagramı](media/sacav2commonscenario.png) 


Diyagramdan görebileceğiniz gibi DoD müşteriler genellikle iki DISA BCAPs abone. Bunlardan biri, Doğu Yakası üzerinde Batı Yakası ve diğer olduğu bulunur. Her DISA BCAP konumundan Azure ExpressRoute ile özel bir eş etkin. Ardından bu ExpressRoute eş sanal ağ geçidi DoD Doğu ve DoD Orta Azure bölgelerinde bağlanır. DoD Doğu ve DoD Orta Azure bölgelerinde SACA örneği dağıtılır. Tüm giriş ve çıkış trafiği, ExpressRoute bağlantısı DISA BCAP gelen ve giden üzerinden akar.

Görev sahibi uygulamaları, bunlar uygulamalarına dağıtmayı planladığınız Azure bölgeleri seçin. Sanal ağ eşleme, uygulamanın sanal ağ SACA sanal ağa bağlanmak için kullandıkları. Ardından, zorlamalı tünel VDSS örneği aracılığıyla kendi tüm trafiği.

SCCA gereksinimlerini karşılayan çünkü bu mimari öneririz. Bu, kolayca ölçeklenebilir ve yüksek oranda kullanılabilir. Ayrıca, dağıtım ve yönetimini basitleştirir.

## <a name="automated-saca-deployment-options"></a>Otomatik SACA dağıtım seçenekleri

 Daha önceden belirtildiği gibi Microsoft, bir otomatik SACA altyapı şablonu oluşturmak için iki satıcılarla kurdu. İki şablon, aşağıdaki Azure bileşenlerini dağıtın: 

- SACA sanal ağ
    - Yönetim alt ağı
        - Bu alt ağ bir Sıçrama kutusu olarak da bilinen yönetim VM'ler ve hizmetler dağıtıldığı, ' dir.
        - VDMS alt ağ
            - Bu alt ağ, VM'ler ve hizmetler için VDMS kullanılan dağıtıldığı ' dir.
        - Güvenilmeyen ve güvenilir bir alt ağlar
            - Sanal gereçler dağıtıldığı bu alt ağlardır.
        - Ağ geçidi alt ağı
            - Bu alt ağ, ExpressRoute ağ geçidini dağıtıldığı ' dir.
- Yönetim atlama kutusunda sanal makineler
    - Bunlar ortamın bant dışı yönetimi için kullanılır.
- Ağ sanal Gereçleri
    - Her iki Citrix kullanın veya şablona göre dağıttığınız F5'e bağlı.
- Ortak IP'ler
    - ExpressRoute çevrimiçi duruma getirilene kadar ön uç için kullanılır. Bu IP'ler, arka uç Azure özel adres alanınızı çevir.
- Rota tabloları 
    - Otomasyon işlemi sırasında uygulanan bu tablolar zorlamalı tünel tüm trafiği sanal gereçten yönlendirilen.
- Azure yük dengeleyicileri - standart SKU
    - Cihazları arasında trafiğin yükünü dengelemek için kullanılırlar.
- Ağ güvenlik grupları
    - Bunlar hangi trafik türlerini belirli uç noktalarına erişebilen denetlemek için kullanılır.


### <a name="citrix-saca-deployment"></a>Citrix SACA dağıtım

Citrix dağıtım şablonu, yüksek oranda kullanılabilir Citrix ADC gereçlerinde iki katman dağıtır. Bu mimari VDSS gereksinimlerini karşılar. 

![Citrix SACA diyagramı](media/citrixsaca.png)


Citrix belgelerine ve dağıtım betiği için bkz: [bu GitHub bağlantı](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca).


 ### <a name="f5-saca-deployment"></a>F5 SACA dağıtım

İki ayrı F5 dağıtım şablonları, iki farklı mimari kapsar. İlk şablon F5 gereçlerinde yalnızca bir katman, yüksek oranda kullanılabilir etkin-etkin yapılandırmada vardır. Bu mimari VDSS gereksinimlerini karşılar. İkinci şablon yüksek oranda kullanılabilir etkin-etkin F5s ikinci bir katmanı ekler. Bu ikinci katman, müşterilerin kendi IP'ler F5 katmanlar arasında F5 ayrı sağlar. Belirli IP'ler kullanım için önceden tanımlanmış tüm DoD bileşenleri vardır. Bu durumda, o mimaride F5 cihazlarda IP'ler içerdiğinden F5 gereçlerinde tek katman çoğu için çalışır.

![F5 SACA diyagramı](media/f5saca.png)

F5 belgeleri ve dağıtım betiği için bkz: [bu GitHub bağlantı](https://github.com/f5devcentral/f5-azure-saca).












