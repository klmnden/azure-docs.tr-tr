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
ms.date: 09/04/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 724a91b6ba0be030a2281bce366e4378892df59b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835811"
---
# <a name="sap-hana-large-instances-network-architecture"></a>SAP HANA (büyük örnekler) ağ mimarisi

Azure Ağ Hizmetleri mimarisi, SAP HANA büyük örnek uygulamaları başarılı dağıtımı önemli bir bileşenidir. Genellikle, SAP HANA (büyük örnekler) Azure dağıtımlarda, farklı boyutlarda veritabanları, kaynak tüketimine CPU ve bellek kullanımı ile birçok farklı SAP çözümleri ile daha geniş bir SAP ortamının sahiptir. Bu tüm SAP sistemlerini üzerinde SAP HANA dayalı olasıdır. SAP altyapınızı kullanan karma olabilir:

- SAP sistemlerini şirket içinde dağıtılabilir. Kendi boyutları nedeniyle bu sistemleri şu an Azure'da barındırılamaz. SQL Server (veritabanı) olarak çalışır ve Vm'leri sunabileceğinden daha fazla CPU veya bellek kaynağı gerektiren SAP ERP sistemi bir ürün örneğidir.
- SAP HANA tabanlı SAP sistemlerini şirket içinde dağıtılabilir.
- Vm'lerde SAP sistemlerini dağıtılmış. Geliştirme, test, korumalı alan, bu sistemler olabilir veya üretim kaynak tüketimi ve bellek talebe göre Azure'da (VM'ler) içinde başarıyla dağıtabilirsiniz SAP NetWeaver tabanlı uygulamalardan herhangi biri için örnekler. Bu sistemler de veritabanlarını SQL Server gibi temel alabilir. Daha fazla bilgi için [SAP destek Not #1928533-azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E). Ve bu sistemler gibi SAP HANA veritabanları temel alabilir. Daha fazla bilgi için [SAP HANA sertifikalı ve Iaas platformları](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- SAP HANA (büyük örnekler) azure'da Azure büyük örnek damga yararlanan SAP uygulama sunucuları (VM üzerinde) azure'da dağıtılabilir.

Karma SAP ortamı ile dört veya daha fazla farklı dağıtım senaryolarının normaldir. Ayrıca birçok müşteri durumlar vardır, Azure'da çalışan SAP ortamlarını tamamlandı. VM'ler daha güçlü hale gibi tüm SAP çözümlerini Azure'da taşıma müşteri sayısını artırır.

Azure, SAP sistemlerini Azure'a dağıtılmış bağlamında ağ karmaşık değil. Bunu, aşağıdaki kurallara göre temel alır:

- Azure sanal ağları ExpressRoute bağlantı hattına bir şirket içi ağa bağlanan bağlanması gerekir.
- Şirket içi genellikle Azure'a bağlanan bir ExpressRoute bağlantı hattı bant genişliği 1 GB/sn veya daha yüksek olmalıdır. Bu en düşük bant genişliği, şirket içi sistemler ve VM'ler üzerinde çalışan sistemler arasında veri aktarımı için yeterli bant genişliği sağlar. Ayrıca yeterli bant genişliği bağlantısı için şirket içi kullanıcıları Azure sistemlerine sağlar.
- Azure'da tüm SAP sistemlerini birbirleri ile iletişim kurmak için sanal ağları ayarlanması gerekir.
- Active Directory ve DNS şirket içinde barındırılan şirket içinden ExpressRoute aracılığıyla azure'a genişletilir.


> [!NOTE] 
> Bir fatura açısından bakıldığında, yalnızca bir Azure aboneliği tek bir kiracıda belirli bir Azure bölgesinde bir büyük örneği damgasında bağlanabilir. Buna karşılık, tek bir büyük örnek damgası kiracının yalnızca bir Azure aboneliğine bağlanabilir. Bu gereksinim, diğer Azure Faturalanabilir nesneleri ile tutarlıdır.

SAP HANA (büyük örnekler) azure'da birden çok farklı Azure bölgelerinde dağıtılırsa, ayrı bir kiracı büyük örneği damgasında dağıtılır. Bu örnekler aynı SAP ortamının bir parçası olduğu sürece, aynı Azure aboneliği altında hem de çalıştırabilirsiniz. 

> [!IMPORTANT] 
> Yalnızca Azure Resource Manager dağıtımı (büyük örnekler) Azure üzerinde SAP HANA ile desteklenir.

 

## <a name="additional-virtual-network-information"></a>Ek sanal ağ bilgileri

ExpressRoute için sanal bir ağa bağlanmak için bir Azure ağ geçidi oluşturulmalıdır. Daha fazla bilgi için [ExpressRoute için sanal ağ geçitleri hakkında](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Azure dışındaki bir altyapı ya da bir Azure büyük örneği damgasında ExpressRoute ile bir Azure ağ geçidi kullanılabilir. Bir Azure ağ geçidi arasında sanal ağları bağlamak için de kullanılabilir. Daha fazla bilgi için [ağ ağ bağlantısı için Resource Manager PowerShell kullanarak yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu bağlantılar farklı Microsoft Kurumsal kenar yönlendiricilerine gelen sürece en fazla dört farklı ExpressRoute bağlantıları Azure ağ geçidine bağlanabilir. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Her ikisi de kullanım örnekleri için bir Azure ağ geçidi sağlar aktarım hızı farklılık gösterir. Daha fazla bilgi için [VPN Gateway hakkında](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bir ExpressRoute bağlantısı kullanarak 10 GB/sn ile bir sanal ağ geçidi ulaşabileceği en yüksek aktarım var. Bir sanal ağda bulunan bir VM ile bir sistem arasında dosya kopyalama şirket içi (bir tek kopya akış) olarak farklı ağ geçidi SKU'ları tam verimini elde etmez. Sanal ağ geçidi tam bant yararlanmak için birden çok akışı kullanın. Veya tek bir dosyayı paralel akışlarındaki farklı dosya kopyalamanız gerekir.


## <a name="networking-architecture-for-hana-large-instance"></a>HANA büyük örneği için ağ mimarisi
HANA büyük örneği için ağ mimarisinin dört farklı parçalara ayrılabilir:

- Şirket içi ve Azure ExpressRoute bağlantısı. Bu bölümü, Müşteri'nin etki alanı ve ExpressRoute aracılığıyla azure'a bağlanır. Aşağıdaki şekilde sağ alt köşedeki bakın.
- Daha önce açıklanan, ağ geçitlerini yeniden olan sanal ağlar ile azure ağ hizmetleri. Uygulama gereksinimleri, güvenlik ve uyumluluk gereksinimlerini uygun tasarımları bulmak için gerek duyduğunuz bir alanı parçasıdır. HANA büyük örneği mi kullanacağını seçmek için sanal ağlar ve Azure ağ geçidi SKU'ları sayısı bakımından dikkate alınması gereken başka bir noktadır. Şekil sağ üst köşede bakın.
- Azure ExpressRoute teknolojisi aracılığıyla bağlantı HANA büyük örneği. Bu bölümü dağıtılan ve Microsoft tarafından işlenir. Tek yapmak için ihtiyacınız olan bazı IP adresi aralıklarını varlıklarınızın HANA büyük örneği dağıtımdan sonra ExpressRoute bağlantı hattına sanal ağlara bağlanma sağlayın. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- HANA büyük örneği ağ, hangi çoğunlukla sizin için saydamdır.

![SAP HANA (büyük örnekler) Azure ve şirket içi bağlı olan sanal ağı](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Azure ExpressRoute aracılığıyla şirket içi varlıklarınızı bağlanmalısınız gereksinimi, HANA büyük örneği kullandığından değiştirmez. HANA büyük örneği birimlerinde barındırılan HANA örneklerine bağlar uygulama katmanı ana bilgisayar, sanal makineleri çalıştıran bir veya birden çok sanal ağların gereksinimini de değişmez. 

Azure'da SAP dağıtımları için fark vardır:

- Müşteri kiracınızın HANA büyük örneği birim başka bir ExpressRoute bağlantı hattı sanal ağlarınıza bağlı. Yük koşullar ayırmak için şirket içi sanal ağları ExpressRoute bağlantılarının ve sanal ağlar ve HANA büyük örneği arasındaki bağlantıları aynı yönlendiriciler paylaşmayın.
- İş yükü profili SAP uygulama katmanı ve HANA büyük örneği arasında çok sayıda küçük isteği ile farklı bir yapısı olduğundan ve verileri (sonuç kümeleri) SAP HANA'dan uygulama katmanı aktarır gibi artışlarını.
- SAP uygulama mimarisi ağ gecikmesine daha tipik senaryolar şirket içi ile Azure arasında veri alışverişi burada daha hassastır.
- Sanal ağ geçidi en az iki ExpressRoute bağlantılarına sahiptir. Her iki bağlantı için sanal ağ geçidinin gelen verileri maksimum bant genişliğini paylaşır.

Karşılaşılan ağ gecikme sürelerini VM'ler ve HANA büyük örneği arasında birimleri bir tipik VM VM ağ gidiş dönüş gecikmesi yüksek olabilir. Bağımlı Azure bölgesi, ölçülen değerleri aşağıda ortalama olarak sınıflandırılan 0,7 ms gidiş dönüş gecikmesi da aşabilir [SAP notu #1100926 - SSS: Ağ performansı](https://launchpad.support.sap.com/#/notes/1100926/E). Azure bölgesi ve aracı bir Azure VM ve HANA büyük örneği birim arasındaki gidiş dönüş ağ gecikmesini ölçmek için bağımlı, ölçülen gecikme süresi en fazla ve 2 milisaniye geçici olabilir. Bununla birlikte, müşteriler SAP HANA büyük örneği başarıyla üzerinde SAP HANA tabanlı üretim SAP uygulamaları dağıtın. Azure HANA büyük örneği İş süreçlerinizi kapsamlı olarak test emin olun.
 
VM'ler ve HANA büyük örneği arasındaki belirleyici ağ gecikme süresi sağlamak için seçtiğiniz sanal ağ geçidi SKU'su gereklidir. Şirket içi VM'ler arasındaki trafiği desenlerinden farklı olarak, HANA büyük örneği ile VM'ler arasındaki trafik desenini aktarılacak istekleri ve veri birimlerini küçük ancak yüksek ani artışlara geliştirebilirsiniz. İyi tür artışlarını işlemek için UltraPerformance ağ geçidi SKU'SUNUN kullanılmasını öneririz. HANA büyük örneği SKU'ları Type II sınıfı için bir sanal ağ geçidi olarak UltraPerformance ağ geçidi SKU'sunu kullanımını zorunludur.

> [!IMPORTANT] 
> SAP uygulama ve veritabanı katmanları arasındaki genel ağ trafiğini göz önünde bulundurulduğunda, yalnızca HighPerformance veya UltraPerformance ağ geçidi SKU'ları sanal ağlar için SAP HANA (büyük örnekler) azure'da bağlamak için desteklenir. HANA büyük örneği Type II SKU'lara için bir sanal ağ geçidi yalnızca UltraPerformance ağ geçidi SKU'sunu desteklenir.



## <a name="single-sap-system"></a>Tek SAP sistemine

Daha önce gösterilen şirket içi altyapınızı Azure'a ExpressRoute aracılığıyla bağlanır. ExpressRoute bağlantı hattı, bir enterprise sınır yönlendiricisi bağlar. Daha fazla bilgi için [Expressroute'a teknik genel bakış](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Rota gerçekleştikten sonra Azure omurgası bağlanır ve tüm Azure bölgelerinde erişilebilir.

> [!NOTE] 
> Azure'da SAP ortamlarını çalıştırmak için SAP ortamı bir Azure bölgesinde en yakın enterprise sınır yönlendiricisi bağlayın. Azure büyük örnek Damgalar Azure Iaas ve büyük örnek Damgalar VM'ler arasındaki ağ gecikmesini en aza indirmek için adanmış Kurumsal edge yönlendirici cihazları aracılığıyla bağlanır.

SAP uygulama örneklerini barındıran sanal makineler için sanal ağ geçidi ExpressRoute işlem hattına bağlı. Büyük örnek damga bağlanmaya yönelik adanmış bir ayrı enterprise sınır yönlendiricisi aynı sanal ağa bağlanır.

Bu sistem, tek bir SAP sistemiyle basit bir örnektir. SAP uygulama katmanı, Azure'da barındırılır. SAP HANA (büyük örnekler) Azure üzerinde SAP HANA veritabanı çalışır. Sanal ağ ağ geçidi bant genişliğini 2 GB/sn veya 10 GB/sn aktarım hızının bir performans sorunu temsil etmez varsayılır.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Birden çok SAP sistemlerini veya büyük SAP sistemlerini

SAP HANA (büyük örnekler) azure'da bağlanmak için birden çok SAP sistemlerini veya büyük SAP sistemlerini dağıtılırsa, aktarım hızını sanal ağ geçidi bir performans sorunu olabilir. Böyle bir durumda, uygulama katmanları birden çok sanal ağlara bölün. Ayrıca, HANA büyük örneği gibi durumlarda bağlanan özel bir sanal ağ oluşturabilirsiniz:

- NFS paylaşımlarını barındıran Azure VM HANA büyük örneği HANA örneklerde doğrudan yedeklemeleri gerçekleştirmek.
- Büyük bir yedekleme veya diğer dosyalar Azure'da yönetilen disk alanı HANA büyük örneği birimlerinden kopyalama.

Konak depolama alanını yönet VMs ayrı bir sanal ağa kullanın. Bu düzenleme büyük dosya veya veri aktarımı HANA büyük örneği Azure üzerinde SAP uygulama katmanı çalıştıran sanal makineler hizmet sanal ağ geçidi etkilerini ortadan kaldırır. 

Daha fazla ölçeklenebilir olmasına ağ mimarisi için:

- Tek ve büyük bir SAP uygulama katmanı için birden fazla sanal ağ yararlanın.
- Dağıtılan her SAP sistemi için ayrı bir sanal ağ, aynı sanal ağda altında ayrı alt ağlara bu SAP sistemlerini birkaç rolde birleştirmeye kıyasla dağıtın.

  SAP hana (büyük örnekler) azure'da daha ölçeklenebilir bir ağ mimarisi:

![SAP uygulama katmanında birden çok sanal ağ üzerinden dağıtma](./media/hana-overview-architecture/image4-networking-architecture.png)

Şekilde, SAP uygulama katmanında veya birden çok sanal ağ üzerinden dağıtılan bileşenleri gösterilmektedir. Bu yapılandırma, bu sanal ağlarında barındırılan uygulamalar arasında iletişim sırasında oluşan kaçınılmaz gecikme yükü kullanıma sunuldu. Varsayılan olarak, bu yapılandırmada kurumsal kenar yönlendiricilerine rotayı farklı sanal ağlarda bulunan sanal makineler arasında ağ trafiği. En iyi duruma getirmek ve iki sanal ağ arasındaki iletişim gecikmesi aşağı Kes aynı bölgedeki sanal ağları eşleme tarafından yoludur. Farklı Aboneliklerde bulunan sanal ağlar bu olsa bile, bu yöntem çalışır. Sanal Ağ eşlemesi ile iki farklı sanal ağlardaki sanal makineler arasındaki iletişim, doğrudan birbirleri ile iletişim kurmak için Azure omurga kullanabilirsiniz. Vm'leri aynı sanal ağda varsa gibi gecikme süresi göstermektedir. Azure sanal ağ geçidinden bağlı IP adresi aralıklarını adresleri trafik, sanal ağın tek tek sanal ağ geçidinden yönlendirilir. 

Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz. [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Azure'da yollar

Üç ağ yönlendirme konuları (büyük örnekler) Azure üzerinde SAP HANA için önemlidir:

* SAP HANA (büyük örnekler) Azure üzerinde Vm'leri ve şirket içi doğrudan özel ExpressRoute bağlantısı üzerinden erişilebilir. Şirket içi HANA büyük örneği birimleri, doğrudan erişim için Microsoft tarafından sunulan hemen mümkün değildir. SAP HANA büyük örneği için kullanılan geçerli Azure ağ mimarisi geçişli yönlendirme kısıtlamaları kaynaklanır. SAP HANA veritabanına bağlanılamıyor, bazı yönetim istemcileri ve doğrudan erişim, SAP çözümü şirket içinde çalışan Yöneticisi gibi tüm uygulamalar.

* Olağanüstü durum kurtarma için iki farklı Azure bölgelerindeki dağıtılan HANA büyük örneği birimi varsa aynı geçici yönlendirme kısıtlamaları geçerli olur. Diğer bir deyişle, IP adresleri (örneğin, ABD Batı) bir bölgedeki bir HANA büyük örneği birimin başka bir bölgede (örneğin, ABD Doğu) dağıtılan bir HANA büyük örneği birim yönlendirilmeyen. Bu kısıtlama, bölgeler arasında eşleme veya çapraz bağlanma HANA büyük örneği birimleri için sanal ağları birbirine bağlama ExpressRoute bağlantı hatları Azure ağ kullanımını bağımsızdır. Şekil "Kullanımı HANA büyük örneği birimleri birden çok bölgede." bölümünde bir grafik gösterimi için bkz. Dağıtılmış mimari ile birlikte sunulur, bu kısıtlama, HANA sistem çoğaltması olağanüstü durum kurtarma işlevi olarak anında kullanımını engeller.

* SAP HANA (büyük örnekler) Azure birimlerdeki gönderdiğiniz sunucu IP havuzu adres aralığı atanan bir IP adresi vardır. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu IP adresi, Azure aboneliği ve azure'da HANA (büyük örnekler) için sanal ağları birbirine bağlayan ExpressRoute aracılığıyla erişilebilir. Sunucu IP havuzu adres aralığı donanım birimi doğrudan atanır / IP adresi atanır. Sahip *değil* Bu çözüm ilk dağıtımları durumda olduğu gibi NAT artık atanmış. 

> [!NOTE]
> İlk iki liste öğelerini açıklandığı gibi geçici akışındaki kısıtlamayı çözmek için ek bileşenler yönlendirme için kullanın. Kısıtlamayı çözmek için kullanılan bileşenleri olabilir:
> 
> * Bir ters ve ondan veri yönlendirmek için proxy. Örneğin, F5 BIG-IP, NGINX ile trafiği bir sanal güvenlik duvarı/trafik yönlendirme çözümü olarak azure'da dağıtılan Yöneticisi.
> * Kullanarak [IPTables kurallarını](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) HANA büyük örneği birimleri farklı bölgelerde veya şirket içi konumlar ve HANA büyük örneği birimleri arasında yönlendirme etkinleştirmek için bir Linux VM'de.
> 
> Uygulama ve üçüncü taraf içeren özel çözümler için destek ağ Gereçleri veya IPTables Microsoft tarafından sağlanmayan unutmayın. Destek, kullanılan bileşenin bir satıcı veya entegratörü tarafından sağlanmalıdır. 

## <a name="internet-connectivity-of-hana-large-instance"></a>Internet bağlantısı HANA büyük örneği
HANA büyük örneği mu *değil* doğrudan internet bağlantısına sahip. Örnek olarak, bu sınırlama, işletim sistemi görüntüsü işletim sistemi satıcıyla birlikte doğrudan kaydetme olanağı kısıtlayabilir. SUSE Linux Enterprise Server abonelik yönetimi aracı yerel sunucu ya da Red Hat Enterprise Linux Abonelik Yöneticisi ile çalışmak gerekebilir.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>VM'ler ve HANA büyük örnek arasındaki veri şifrelemesi
VM'ler ve HANA büyük örneği arasında aktarılan veriler şifrelenmez. Ancak, yalnızca exchange için JDBC/ODBC tabanlı uygulamalar ve HANA DBMS yan arasında trafiği şifreleme etkinleştirebilirsiniz. Daha fazla bilgi için [SAP tarafından bu belgeleri](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>Birden çok bölgede HANA büyük örneği birimleri kullanın

SAP HANA (büyük örnekler) dışında birden çok Azure bölgesine olağanüstü durum kurtarma için azure'a dağıtmak için neden olabilir. Belki de her bölgelerdeki farklı sanal ağlarda bulunan dağıtılan VM'lerin HANA büyük örneği erişmek istediğiniz. Farklı bir HANA büyük örneği birimleri için atanan IP adreslerini, kendi ağ geçidi örneklerine üzerinden doğrudan bağlı olan sanal ağların ötesinde yayılmaz. Sonuç olarak, küçük bir değişiklik için sanal ağ tasarımı sunulmuştur. Bir sanal ağ geçidi farklı bir kurumsal kenar yönlendiricilerine dışında dört farklı ExpressRoute devreleri başa çıkabilir. Büyük örnek Damgalar birine bağlı her bir sanal ağ başka bir Azure bölgesinde büyük örneği damgasında bağlanabilir.

![Farklı Azure bölgelerindeki Azure büyük örnek Damgalar bağlı olan sanal ağı](./media/hana-overview-architecture/image8-multiple-regions.png)

Şekil nasıl iki bölgeleri farklı sanal ağlarda (büyük örnekler) Azure üzerinde SAP hana'ya bağlanmak için kullanılan iki farklı ExpressRoute devreleri için Azure her iki bölgede de bağlandığını gösterir. Yeni çıkan dikdörtgen kırmızı çizgiler bağlantılardır. Bu sanal ağlardan birine çalıştırılan sanal makinelerin sanal ağların dışında bu bağlantıları ile her iki bölgede dağıtılan farklı HANA büyük örneği birimlerinin erişebilirsiniz. Şekilde gösterildiği gibi iki Azure bölgesi için iki ExpressRoute bağlantıları şirket içinde sahip olduğunuz varsayılır. Bu düzenleme, olağanüstü durum kurtarma nedenlerle önerilir.

> [!IMPORTANT] 
> Birden çok ExpressRoute bağlantı hattına kullandıysanız, doğru yönlendirmeyi, trafiği emin olmak için AS yolu eklenmesini ve yerel tercih BGP ayarları kullanılmalıdır.

**Sonraki adımlar**
- Başvuru [SAP HANA (büyük örnekler) depolama mimarisi](hana-storage-architecture.md)