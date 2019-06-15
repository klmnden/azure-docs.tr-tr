---
title: Ağ (büyük örnekler) Azure üzerinde SAP HANA mimarisi | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da dağıtma ağ mimarisi.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8794a93cecb50774f30746c22931db31a9fa9194
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66239608"
---
# <a name="sap-hana-large-instances-network-architecture"></a>SAP HANA (büyük örnekler) ağ mimarisi

Azure Ağ Hizmetleri mimarisi, SAP HANA büyük örnek uygulamaları başarılı dağıtımı önemli bir bileşenidir. Genellikle, SAP HANA (büyük örnekler) Azure dağıtımlarda, farklı boyutlarda veritabanları, kaynak tüketimine CPU ve bellek kullanımı ile birçok farklı SAP çözümleri ile daha geniş bir SAP ortamının sahiptir. Tüm BT sistemlerinin Azure'da zaten bulunan olasıdır. SAP altyapınızı karma de DBMS noktası ve SAP uygulama, NetWeaver ve S/4HANA, SAP HANA ve diğer DBMS bir karışımını kullanarak açısından genellikle olur. Azure, Azure üzerinde farklı DBMS, NetWeaver ve S/4HANA sistemlerini çalışmasına olanak tanıyan farklı hizmetleri sunar. Azure ayrıca Ara Azure yapmak için teknoloji ağ teklifler bir sanal veri merkezi için şirket içi yazılımları dağıtımlarınızla ister

Azure'da barındırılan tam BT sistemlerinizi sürece. Azure ağ işlevleri Azure varlıklarınızı bir sanal veri merkezi sizinki gibi ara Azure'u şirket içi dünyanın bağlanmak için kullanılır. Kullanılan Azure ağ işlevselliğini aşağıdaki gibidir: 

- Azure sanal ağlarına bağlı [ExpressRoute](https://azure.microsoft.com/services/expressroute/) şirket içi ağ varlıklarınızı bağlanan bağlantı hattı.
- Şirket içi Azure'a bağlanan bir ExpressRoute bağlantı hattı en düşük bant genişliği olmalıdır [1 GB/sn veya daha yüksek](https://azure.microsoft.com/pricing/details/expressroute/). Bu en düşük bant genişliği, şirket içi sistemler ve VM'ler üzerinde çalışan sistemler arasında veri aktarımı için yeterli bant genişliği sağlar. Ayrıca yeterli bant genişliği bağlantısı için şirket içi kullanıcıları Azure sistemlerine sağlar.
- Azure'da tüm SAP sistemlerini birbirleri ile iletişim kurmak için sanal ağları ayarlanır.
- Active Directory ve DNS şirket içinde barındırılan şirket içinden azure'a ExpressRoute aracılığıyla genişletilmiş veya Azure'da tam olarak çalışıyor.

Özel durum HANA büyük örnekleri, Azure veri merkezi ağ fabric'te tümleştirme Azure ExpressRoute teknoloji de kullanılır


> [!NOTE] 
> Yalnızca bir Azure aboneliği, belirli bir Azure bölgesinde bir HANA büyük örneği damgasında yalnızca tek bir kiracı için bağlanabilir. Buna karşılık, tek bir HANA büyük örnek damgası kiracının yalnızca bir Azure aboneliğine bağlanabilir. Bu gereksinim, diğer Azure Faturalanabilir nesneleri ile tutarlıdır.

SAP HANA (büyük örnekler) azure'da birden çok farklı Azure bölgelerinde dağıtılırsa, ayrı bir kiracı HANA büyük örneği damgasında dağıtılır. Bu örnekler aynı SAP ortamının bir parçası olduğu sürece, aynı Azure aboneliği altında hem de çalıştırabilirsiniz. 

> [!IMPORTANT] 
> Yalnızca Azure Resource Manager dağıtım yöntemi, SAP HANA (büyük örnekler) azure'da ile desteklenir.

 

## <a name="additional-virtual-network-information"></a>Ek sanal ağ bilgileri

ExpressRoute için sanal bir ağa bağlanmak için bir Azure ExpressRoute ağ geçidi oluşturulmalıdır. Daha fazla bilgi için [hakkında Expressroute ağ geçitleri için ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Bir Azure ExpressRoute ağ geçidi ExpressRoute ile Azure dışındaki bir altyapı ya da bir Azure büyük örneği damgasında kullanılır. Bu bağlantılar farklı Microsoft Kurumsal kenar yönlendiricilerine gelen sürece en fazla dört farklı ExpressRoute bağlantı hatları Azure ExpressRoute ağ geçidine bağlanabilir. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Bir ExpressRoute ağ geçidiyle birlikte ulaşabileceği en yüksek aktarım 10 Gbps ExpressRoute bağlantısı kullanılmasıdır. Bir sanal ağda bulunan bir VM ile bir sistem arasında dosya kopyalama şirket içi (bir tek kopya akış) olarak farklı ağ geçidi SKU'ları tam verimini elde etmez. ExpressRoute ağ geçidini tam bant yararlanmak için birden çok akışı kullanın. Veya tek bir dosyayı paralel akışlarındaki farklı dosya kopyalamanız gerekir.


## <a name="networking-architecture-for-hana-large-instance"></a>HANA büyük örneği için ağ mimarisi
HANA büyük örneği için ağ mimarisinin dört farklı parçalara ayrılabilir:

- Şirket içi ve Azure ExpressRoute bağlantısı. Bu bölümü, Müşteri'nin etki alanı ve ExpressRoute aracılığıyla azure'a bağlanır. Bu Expressroute bağlantı hattı, bir müşteri olarak tam olarak sizin tarafınızdan ödenir. Bant genişliği, şirket içi varlıklarınızı karşı bağlanan bir Azure bölgesi arasındaki ağ trafiğini işlemek için büyük olmalıdır. Aşağıdaki şekilde sağ alt köşedeki bakın.
- Daha önce yeniden eklenen ExpressRoute ağ geçitleri gerekir, sanal ağlarla açıklandığı gibi azure ağ hizmetleri. Uygulama gereksinimleri, güvenlik ve uyumluluk gereksinimlerini uygun tasarımları bulmak için gerek duyduğunuz bir alanı parçasıdır. HANA büyük örneği mi kullanacağını seçmek için sanal ağlar ve Azure ağ geçidi SKU'ları sayısı bakımından dikkate alınması gereken başka bir noktadır. Şekil sağ üst köşede bakın.
- Azure ExpressRoute teknolojisi aracılığıyla bağlantı HANA büyük örneği. Bu bölümü dağıtılan ve Microsoft tarafından işlenir. Tek yapmak için ihtiyacınız olan bazı IP adresi aralıklarını varlıklarınızın HANA büyük örneği dağıtımdan sonra ExpressRoute bağlantı hattına sanal ağlara bağlanma sağlayın. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Azure veri merkezi ağ yapısı ve HANA büyük örneği birimler arasında bağlantı kurmak için bir müşteri olarak, hiçbir ek ücret yoktur.
- HANA büyük örneği damga ağ, hangi çoğunlukla sizin için saydamdır.

![SAP HANA (büyük örnekler) Azure ve şirket içi bağlı olan sanal ağı](./media/hana-overview-architecture/image1-architecture.png)

Azure ExpressRoute aracılığıyla şirket içi varlıklarınızı bağlanmalısınız gereksinimi, HANA büyük örneği kullandığından değiştirmez. HANA büyük örneği birimlerinde barındırılan HANA örneklerine bağlar uygulama katmanı ana bilgisayar, sanal makineleri çalıştıran bir veya birden çok sanal ağların gereksinimini de değişmez. 

Azure'da SAP dağıtımları için fark vardır:

- Müşteri kiracınızın HANA büyük örneği birim başka bir ExpressRoute bağlantı hattı sanal ağlarınıza bağlı. Yük koşullar ayırmak için şirket içi-Azure sanal ağı ExpressRoute bağlantı hatları ve HANA büyük örnekleri ile Azure sanal ağları arasında bağlantı hatlarının aynı yönlendiriciler paylaşmayın.
- İş yükü profili SAP uygulama katmanı ve HANA büyük örneği arasında çok sayıda küçük isteği ile farklı bir yapısı olduğundan ve verileri (sonuç kümeleri) SAP HANA'dan uygulama katmanı aktarır gibi artışlarını.
- SAP uygulama mimarisi ağ gecikmesine daha tipik senaryolar şirket içi ile Azure arasında veri alışverişi burada daha hassastır.
- Azure ExpressRoute ağ geçidini en az iki ExpressRoute bağlantılarına sahiptir. Şirket içi ve HANA büyük örnekleri bağlı bir bağlı bir bağlantı hattı. ExpressRoute ağ geçidi üzerinde bağlanmak için farklı Msee bu başka bir iki ek devreler için yalnızca belirli bir oda bırakır. Bu kısıtlama, ExpressRoute hızlı yolu kullanımını bağımsızdır. Bağlı tüm devreler ExpressRoute ağ geçidinin gelen veriler için maksimum bant genişliğini paylaşır.

Karşılaşılan ağ gecikme sürelerini VM'ler ve HANA büyük örneği arasında birimleri bir tipik VM VM ağ gidiş dönüş gecikmesi yüksek olabilir. Bağımlı Azure bölgesi, ölçülen değerleri aşağıda ortalama olarak sınıflandırılan 0,7 ms gidiş dönüş gecikmesi da aşabilir [SAP notu #1100926 - SSS: Ağ performansı](https://launchpad.support.sap.com/#/notes/1100926/E). Azure bölgesi ve aracı bir Azure VM ve HANA büyük örneği birim arasındaki gidiş dönüş ağ gecikmesini ölçmek için bağımlı, ölçülen gecikme süresi en fazla ve 2 milisaniye geçici olabilir. Bununla birlikte, müşteriler SAP HANA büyük örneği başarıyla üzerinde SAP HANA tabanlı üretim SAP uygulamaları dağıtın. Azure HANA büyük örneği İş süreçlerinizi kapsamlı olarak test emin olun. ExpressRoute hızlı yol adı verilen yeni bir işlevsellik HANA büyük örnekleri ile uygulama katmanı azure'da sanal makineler arasındaki ağ gecikme süresi önemli ölçüde azaltabilirsiniz (aşağıya bakın). 

VM'ler ve HANA büyük örneği, ExpressRoute ağ geçidini seçimi arasındaki belirleyici ağ gecikme süresi sağlamak için SKU gereklidir. Şirket içi VM'ler arasındaki trafiği desenlerinden farklı olarak, HANA büyük örneği ile VM'ler arasındaki trafik desenini aktarılacak istekleri ve veri birimlerini küçük ancak yüksek ani artışlara geliştirebilirsiniz. İyi tür artışlarını işlemek için UltraPerformance ağ geçidi SKU'SUNUN kullanılmasını öneririz. UltraPerformance ağ geçidi SKU'sunu kullanımını ExpressRotue ağ geçidi olarak HANA büyük örneği SKU'ları Type II sınıfı için zorunludur.

> [!IMPORTANT] 
> SAP uygulama ve veritabanı katmanları arasındaki genel ağ trafiğini göz önünde bulundurulduğunda, yalnızca HighPerformance veya UltraPerformance ağ geçidi SKU'ları sanal ağlar için SAP HANA (büyük örnekler) azure'da bağlamak için desteklenir. HANA büyük örneği Type II SKU'lara için bir ExpressRoute ağ geçidi olarak yalnızca UltraPerformance ağ geçidi SKU'sunu desteklenir. ExpressRoute hızlı yolu (aşağıya bakın) kullanılırken özel durumlarını uygulayın

### <a name="expressroute-fast-path"></a>ExpressRoute Fast Path
Daha düşük gecikme süresi için ExpressRoute hızlı yolu sunulan ve Mayıs 2019 ', SAP uygulama Vm'lerine konak HANA büyük örnekleri, Azure sanal ağları belirli bağlantısını için yayımlanan. Şu ana kadar piyasaya sunuluyor çözüm için en önemli fark olduğu VM'ler ve HANA büyük örnekleri arasında bir veri akışı ExpressRoute ağ geçidi üzerinden artık yönlendirildiğinden değil. Bunun yerine alt ağı, başarılı bir Azure sanal ağının atanmış Vm'leri adanmış enterprise sınır yönlendiricisi ile doğrudan iletişim. 

> [!IMPORTANT] 
> ExpressRoute hızlı yolu işlevselliğini HANA büyük örneklerine bağlı aynı Azure sanal ağında çalışan SAP uygulama VM'lerin alt ağları olmasını gerektirir. ExpressRoute hızlı yoldan HANA büyük örneği birimlerine doğrudan bağlı olan Azure sanal ağı ile eşlenmiş Azure sanal ağlarda bulunan sanal makineleri yararlanmaya değil. Burada bir hub sanal ağı ExpressRoute bağlantı hatları bağlanıyorsanız ve SAP uygulama katmanına (bağlı bileşen) içeren sanal ağlar eşlenmiş, sonuç olarak normal merkez ve uç sanal ağ tasarımları ExpressRoute hızlı tarafından en iyi duruma getirme Yol çalışmaz. Gateway'e ek olarak, ExpressRoute hızlı yol (UDR) kullanıcı tanımlı yönlendirme kuralları bugün desteklemez. Daha fazla bilgi için [ExpressRoute sanal ağ geçidiniz ile FastPath](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways). 


ExpressRoute hızlı yolu yapılandırma hakkında daha fazla bilgi okumak için belgeyi [HANA büyük örnekleri için bir sanal ağı bağlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).    

> [!NOTE]
> UltraPerformance ExpressRoute ağ geçidi ExpressRoute hızlı yolu çalışması için gereklidir


## <a name="single-sap-system"></a>Tek SAP sistemine

Daha önce gösterilen şirket içi altyapınızı Azure'a ExpressRoute aracılığıyla bağlanır. ExpressRoute bağlantı hattı, Microsoft enterprise sınır yönlendiricisi (MSEE) bağlar. Daha fazla bilgi için [Expressroute'a teknik genel bakış](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Rota gerçekleştikten sonra Azure omurgası bağlanır.

> [!NOTE] 
> Azure'da SAP ortamlarını çalıştırmak için SAP ortamı bir Azure bölgesinde en yakın enterprise sınır yönlendiricisi bağlayın. HANA büyük örneği Damgalar Azure Iaas Vm'leri ve HANA büyük örneği Damgalar arasındaki ağ gecikmesini en aza indirmek için adanmış Kurumsal edge yönlendirici cihazları aracılığıyla bağlanır.

SAP uygulama örneklerini barındıran sanal makineler için ExpressRoute ağ geçidini şirket içine bağlanan bir ExpressRoute bağlantı hattına bağlı. Büyük örnek damga bağlanmaya yönelik adanmış bir ayrı enterprise sınır yönlendiricisi aynı sanal ağa bağlanır. ExpressRoute hızlı yolu, HANA büyük örnekleri veri akışından SAP uygulama katmanına kullanarak sanal makineler ExpressRoute ağ geçidi üzerinden artık yönlendirilmeyen ve ağ gidiş dönüş gecikmesine azaltmak.

Bu sistem, tek bir SAP sistemiyle basit bir örnektir. SAP uygulama katmanı, Azure'da barındırılır. SAP HANA (büyük örnekler) Azure üzerinde SAP HANA veritabanı çalışır. ExpressRoute ağ geçidi bant genişliğini 2 GB/sn veya 10 GB/sn aktarım hızının bir performans sorunu temsil etmez varsayılır.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Birden çok SAP sistemlerini veya büyük SAP sistemlerini

SAP HANA (büyük örnekler) azure'da bağlanmak için birden çok SAP sistemlerini veya büyük SAP sistemlerini dağıtılırsa, aktarım hızı ExpressRoute ağ geçidi bir performans sorunu olabilir. Veya, üretim ve üretim dışı sistemlere farklı Azure sanal ağlarında yalıtmak istiyorsanız. Böyle bir durumda, uygulama katmanları birden çok sanal ağlara bölün. Ayrıca, HANA büyük örneği gibi durumlarda bağlanan özel bir sanal ağ oluşturabilirsiniz:

- NFS paylaşımlarını barındıran Azure VM HANA büyük örneği HANA örneklerde doğrudan yedeklemeleri gerçekleştirmek.
- Büyük bir yedekleme veya diğer dosyalar Azure'da yönetilen disk alanı HANA büyük örneği birimlerinden kopyalama.

Konağı HANA büyük örnekleri ve Azure arasında toplu veri aktarımı için depolama alanını yönet VM'ler için ayrı bir sanal ağ'ı kullanın. Bu düzenleme büyük dosya veya veri aktarımı HANA büyük örneği Azure üzerinde SAP uygulama katmanı çalıştıran sanal makineler hizmet ExpressRoute ağ geçidi etkilerini ortadan kaldırır. 

Daha fazla ölçeklenebilir olmasına ağ mimarisi için:

- Tek ve büyük bir SAP uygulama katmanı için birden fazla sanal ağ yararlanın.
- Dağıtılan her SAP sistemi için ayrı bir sanal ağ, aynı sanal ağda altında ayrı alt ağlara bu SAP sistemlerini birkaç rolde birleştirmeye kıyasla dağıtın.

  SAP hana (büyük örnekler) azure'da daha ölçeklenebilir bir ağ mimarisi:

![SAP uygulama katmanında birden çok sanal ağ üzerinden dağıtma](./media/hana-overview-architecture/image4-networking-architecture.png)

Kuralları ve kısıtlamaları bağımlı, sanal makinelerin farklı SAP sistemlerini barındırma farklı sanal ağlar arasında uygulamak istediğiniz, bu sanal ağları eşleyebilme. Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz. [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Azure'da yollar

Varsayılan dağıtım tarafından üç ağ yönlendirme konuları (büyük örnekler) Azure üzerinde SAP HANA için önemlidir:

* SAP HANA (büyük örnekler) azure'da yalnızca Azure Vm'leri ve şirket içi doğrudan özel ExpressRoute bağlantısı üzerinden erişilebilir. Şirket içi HANA büyük örneği birimleri, doğrudan erişim için Microsoft tarafından sunulan hemen mümkün değildir. SAP HANA büyük örneği için kullanılan geçerli Azure ağ mimarisi geçişli yönlendirme kısıtlamaları kaynaklanır. SAP HANA veritabanına bağlanılamıyor, bazı yönetim istemcileri ve doğrudan erişim, SAP çözümü şirket içinde çalışan Yöneticisi gibi tüm uygulamalar. Özel durumlar için 'Doğrudan yönlendirme HANA büyük örnekler için' bölümüne bakın.

* İki farklı Azure bölgelerindeki olağanüstü durum kurtarma için geçmişte uygulanan aynı geçici yönlendirme kısıtlamaları dağıtılan HANA büyük örneği birim varsa. Diğer bir deyişle, IP adresleri (örneğin, ABD Batı) bir bölgedeki bir HANA büyük örneği birimin başka bir bölgede (örneğin, ABD Doğu) dağıtılan bir HANA büyük örneği birimine yönlendirilmiş değil. Bu kısıtlama, bölgeler arasında eşleme veya çapraz bağlanma HANA büyük örneği birimleri için sanal ağları birbirine bağlama ExpressRoute bağlantı hatları Azure ağ kullanımını bağımsızdır. Şekil "Kullanımı HANA büyük örneği birimleri birden çok bölgede." bölümünde bir grafik gösterimi için bkz. Dağıtılmış mimari ile birlikte gelen, bu kısıtlama, HANA sistem çoğaltması olağanüstü durum kurtarma işlevi olarak anında kullanmak yasaktır. Üzerindeki son zamanlardaki değişiklikler, 'Kullanım HANA büyük örneği birimi birden fazla bölgede' bölümüne bakın. 

* SAP HANA (büyük örnekler) Azure birimlerdeki HANA büyük örneği dağıtım isteğinde bulunurken gönderilen sunucu IP havuzu adres aralığı atanan bir IP adresi vardır. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu IP adresi, Azure abonelikleri ve HANA büyük örnekler için Azure sanal ağları bağlayan bağlantı hattı üzerinden erişilebilir. Sunucu IP havuzu adres aralığı donanım birimi doğrudan atanır / IP adresi atanır. Sahip *değil* Bu çözüm ilk dağıtımları durumda olduğu gibi NAT artık atanmış. 

### <a name="direct-routing-to-hana-large-instances"></a>Doğrudan yönlendirme için HANA büyük örnekleri
Varsayılan olarak, HANA büyük örneği birimleri ve şirket içi arasında veya iki farklı bölgede dağıtılan HANA büyük örneği yönlendirme arasında geçişli yönlendirmeye çalışmaz. Böyle bir geçişli yönlendirmeye etkinleştirmek için birkaç olasılık vardır.

- Bir ters ve ondan veri yönlendirmek için proxy. Örneğin, F5 BIG-IP, NGINX ile trafik Azure sanal ağında dağıtılan Yöneticisi, HANA büyük örnekler için ve şirket içi bir sanal güvenlik duvarı/trafik yönlendirme çözümü olarak bağlar.
- Kullanarak [IPTables kurallarını](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) HANA büyük örneği birimleri farklı bölgelerde veya şirket içi konumlar ve HANA büyük örneği birimleri arasında yönlendirme etkinleştirmek için bir Linux VM'de. IPTables çalıştıran VM, HANA büyük örnekleri bağlayan Azure sanal ağında dağıtılan ve şirket içi gerekir. VM ihtiyacı olacak şekilde boyutlandırılmış buna göre bunu, VM ağ verimliliği beklenen ağ trafiği için yeterli olduğunu. Ağ bant genişliği VM hakkında ayrıntılı bilgi için makale onay [Azure sanal makineler'de Linux boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json).
- [Azure Güvenlik Duvarı](https://azure.microsoft.com/services/azure-firewall/) etkinleştirmek şirket içi ve HANA büyük örneği birimleri arasında trafiği yönlendirmek için başka bir çözüm olacaktır. 

Bu çözümlerin tüm trafik bir Azure sanal ağı yönlendirilirdi ve bu nedenle trafik kullanılan yazılım Gereçleri veya Azure ağ güvenlik gelen belirli IP adresleri veya IP adresi aralıkları grupları, bu nedenle, ayrıca kısıtlı olabilir Şirket içi engellendi veya erişim HANA büyük örnekleri açıkça izin. 

> [!NOTE]  
> Uygulama ve üçüncü taraf içeren özel çözümler için destek ağ Gereçleri veya IPTables Microsoft tarafından sağlanmayan unutmayın. Destek, kullanılan bileşenin bir satıcı veya entegratörü tarafından sağlanmalıdır. 

#### <a name="express-route-global-reach"></a>Express route küresel erişim
Microsoft gelen adlı yeni bir işlevsellik [ExpressRoute Global erişim](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach). Küresel erişim HANA büyük örnekler için iki senaryolarda kullanılabilir:

- Farklı bölgelerde dağıtılan HANA büyük örneği Birimleriniz için şirket içinden doğrudan erişimini etkinleştir
- Farklı bölgelerde dağıtılan, HANA büyük örneği birimleri arasında doğrudan iletişim etkinleştir


##### <a name="direct-access-from-on-premise"></a>Şirket içinden doğrudan erişim
Global erişim Burada sunulan Azure bölgelerinde de, HANA büyük örneği birimine bağlanan Azure sanal ağı şirket içi ağınıza bağlanır, ExpressRoute bağlantı hattı için Global erişim işlevlerini etkinleştirmek isteyebilirsiniz. ExpressRoute bağlantı hattı, şirket içi tarafı için bazı maliyet etkileri vardır. Fiyatlar için fiyatı denetleyin [genel ulaşmak eklenti](https://azure.microsoft.com/pricing/details/expressroute/). Ek maliyet olmadan, HANA büyük örneği birim Azure'a bağlanan bağlantı hattına ilgili vardır. 

> [!IMPORTANT]  
> Global erişim HANA büyük örneği birimleri ve şirket içi varlıkları arasında doğrudan erişimini etkinleştirmek için olması durumunda, ağ verisi ve denetim akışı kullanmaktır **Azure sanal ağları yönlendirilmedi**, ancak Microsoft arasında doğrudan Kurumsal exchange yönlendiriciler. Sonuç olarak herhangi bir NSG veya ASG kuralları veya güvenlik duvarı, NVA veya bir Azure sanal ağında dağıtılan proxy herhangi bir türü dokunmaz. **ExpressRoute Global erişim HANA büyük örneği birimleri kısıtlamaları şirket içinden doğrudan erişim sağlamak amacıyla kullanıyorsanız ve HANA büyük örneği birimleri erişim izni güvenlik duvarları şirket içi tarafında tanımlanması gerekiyor** 

##### <a name="connecting-hana-large-instances-in-different-azure-regions"></a>Farklı Azure bölgelerindeki HANA büyük örnekleri bağlanma
ExpressRoute genel ulaşmak için HANA büyük örneği birimi, şirket içi bağlamak için kullanılabilir olarak aynı şekilde, bunu sizin için iki farklı bölgede dağıtılan, HANA büyük örneği Kiracı satırındaki bağlanmak için kullanılabilir. Yalıtım bağlanmak için her iki bölgeleri'nde Azure HANA büyük örneği kiracılarınız kullanan ExpressRoute devreleri ' dir. İki farklı bölgede dağıtılan iki HANA büyük örneği Kiracı bağlamak için herhangi bir ek ücret yoktur. 

> [!IMPORTANT]  
> Denetim akışı farklı HANA örneği kiracılar azure ağları yönlendirilmez büyük arasındaki ağ trafiği ve veri akışı. Sonuç olarak, iki HANA büyük örnekleri kiracılar arasında iletişimi kısıtlamalarını uygulamak için Azure işlevleri veya nva'ları kullanamazsınız. 

Etkin ExpressRoute Global erişim alma hakkında daha fazla bilgi için belgeyi okumak [HANA büyük örnekleri için bir sanal ağı bağlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).


## <a name="internet-connectivity-of-hana-large-instance"></a>Internet bağlantısı HANA büyük örneği
HANA büyük örneği mu *değil* doğrudan internet bağlantısına sahip. Örnek olarak, bu sınırlama, işletim sistemi görüntüsü işletim sistemi satıcıyla birlikte doğrudan kaydetme olanağı kısıtlayabilir. SUSE Linux Enterprise Server abonelik yönetimi aracı yerel sunucu ya da Red Hat Enterprise Linux Abonelik Yöneticisi ile çalışmak gerekebilir.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>VM'ler ve HANA büyük örnek arasındaki veri şifrelemesi
VM'ler ve HANA büyük örneği arasında aktarılan veriler şifrelenmez. Ancak, yalnızca exchange için JDBC/ODBC tabanlı uygulamalar ve HANA DBMS yan arasında trafiği şifreleme etkinleştirebilirsiniz. Daha fazla bilgi için [SAP tarafından bu belgeleri](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>Birden çok bölgede HANA büyük örneği birimleri kullanın

Olağanüstü durum kurtarma ayarlama ups hayata geçirmek için birden çok Azure bölgesinde ŞANA büyük örnek birimi kullanma gerekir. Hatta kullanarak Azure [genel sanal ağ eşleme] ile geçişli yönlendirmeye varsayılan olarak iki farklı bölgede HANA büyük örneği kiracılar arasında çalışmıyor. Ancak, iki farklı bölgede sağladığınız HANA büyük örneği birimleri arasındaki iletişim yolunun'kurmak Global erişim açılır. Bu kullanım senaryosu, ExpressRoute Global erişim sağlar:

 - Herhangi bir ek proxy'leri veya güvenlik duvarlarını olmadan HANA sistem çoğaltması
 - Sistem kopyaları veya sistem yenileme gerçekleştirmek için iki farklı bölgede HANA büyük örneği birimleri arasında kopyalama yedeklemeleri


![Farklı Azure bölgelerindeki Azure büyük örnek Damgalar bağlı olan sanal ağı](./media/hana-overview-architecture/image8-multiple-regions.png)

Şekil (büyük örnekler) Azure üzerinde SAP hana'ya bağlanmak için kullanılan iki farklı ExpressRoute bağlantı hatları için her iki bölgeleri farklı sanal ağlarda nasıl bağlandığını, hem Azure bölgeleri (gri çizgi) gösterir. Her iki tarafında Msee'ler çalışmadığını korumak için bu iki çapraz bağlantıları neden olmasıdır. İki Azure bölgesindeki iki sanal ağ arasındaki iletişimi akış üzerinden ele alınması gereken [genel eşleme](https://blogs.msdn.microsoft.com/azureedu/2018/04/24/how-to-setup-global-vnet-peering-in-azure/) iki sanal ağın iki farklı bölgelerde (mavi noktalı çizgi). Kalın kırmızı çizgi kiracılarınız birbirleri ile iletişim kurmak için iki farklı bölge HANA büyük örnek sayısı izin veren ExpressRoute Global erişim bağlantı açıklar. 

> [!IMPORTANT] 
> Birden çok ExpressRoute bağlantı hattına kullandıysanız, doğru yönlendirmeyi, trafiği emin olmak için AS yolu eklenmesini ve yerel tercih BGP ayarları kullanılmalıdır.

**Sonraki adımlar**
- Başvuru [SAP HANA (büyük örnekler) depolama mimarisi](hana-storage-architecture.md)