---
title: "Genel bakış ve SAP HANA azure'da (büyük örnekler) mimarisini | Microsoft Docs"
description: "SAP HANA (büyük örnekler) azure'da dağıtmak nasıl mimari genel bakış."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4246ecfa50176400c54cd80857e25675290e7170
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>SAP HANA (büyük örnekler) genel bakış ve Azure üzerinde mimarisi

## <a name="what-is-sap-hana-on-azure-large-instances"></a>SAP HANA azure'da (büyük örnekler) nedir?

SAP HANA azure'da (büyük örneği), Azure için benzersiz bir çözümdür. Azure sanal makineleri dağıtmak ve SAP HANA çalıştırmak amacıyla sağlamanın yanı sıra, Azure çalıştırmak ve size bir müşteri olarak ayrılmış tam sunuculardaki SAP HANA dağıtmak için olanağı sunar. Bir müşteri olarak atadığınız paylaşılmayan konak/sunucu tam donanım üzerinde Azure (büyük örnekler) çözüm üzerinde SAP HANA oluşturur. Sunucu donanımı işlem/sunucu, ağ ve depolama altyapısı içeren büyük damga olarak katıştırılır. Bir birleşim olarak olduğu sertifikalı HANA TDI. SAP HANA Azure (büyük örnekler) ile ilgili hizmet teklifi çeşitli farklı sunucu SKU'ları veya 72 CPU'lar ve 960 CPU'lar birimlerine 768 GB bellek ve 20 TB belleğe sahip birimleriyle başlangıç boyutları sunar.

Altyapı damga içindeki müşteri yalıtımı, kiracılar, ayrıntılı benzer gerçekleştirilir:

- Ağ: Kiracı yalıtımı müşterilerin müşteri başına sanal ağlar altyapı yığınından içinde atanır. Bir kiracı için tek bir müşteri atanır. Bir müşteri, birden çok kiracıya sahip olabilir. Ağ yalıtımı kiracıların kiracılar altyapı damga düzeyinde arasındaki ağ iletişimi engelliyor. Kiracılar aynı müşteriye ait olsa bile.
- Depolama bileşenleri: atanmış depolama birimleri depolama sanal makinelerde yalıtımı. Depolama birimleri, yalnızca bir depolama sanal makineye atanabilir. Bir depolama alanı sanal makine yalnızca tek bir kiracı SAP HANA TDI sertifikalı altyapı yığınında atanır. Sonuç olarak bir belirli ve ilişkili Kiracı içinde yalnızca bir depolama alanı sanal makineye atanan depolama birimleri erişilebilir. Ve farklı dağıtılan kiracılar arasında görünmez.
- Sunucu veya ana bilgisayar: bir sunucu veya konak birimi müşteriler veya kiracılar arasında paylaşılmaz. Bir sunucu veya bir müşteri'ye dağıtılan konak tek bir kiracı için atanan bir atomik tam işlem birimi değil. **Hayır** donanım bölümleme veya yumuşak bölümleme kullanılırsa, neden size, bir konak ya da bir sunucu paylaşımı ile başka bir müşteri, bir müşteri olarak. Belirli Kiracı depolama sanal makineye atanan depolama birimleri, bu tür bir sunucuya bağlanır. Bir kiracı farklı SKU'ları özel olarak atanan sunucu sayısı birine sahip olabilir.
- Azure (büyük örneği) altyapısı damga üzerinde bir SAP HANA içinde birçok farklı kiracıların dağıtılır ve diğer karşı ağ, depolama ve işlem düzeyi Kiracı kavramları aracılığıyla yalıtılmış. 


Bu tam sunucu birimleri, SAP HANA yalnızca çalıştırmak için desteklenir. SAP uygulama katmanı veya iş yükü donanımlar orta katman Microsoft Azure sanal makinelerde çalışan. SAP HANA Azure (büyük örneği) birimler üzerinde çalışan altyapı Damgalar SAP HANA Azure (büyük örneği) birimlerdeki ve Azure sanal makineler arasında düşük gecikme süresi bağlantısı sağlanan Azure ağ omurgalarında için bu nedenle, bağlı.

Bu belge, Azure (büyük örneği) üzerinde SAP HANA kapak birden çok belge biridir. Bu belgede, temel mimari, sorumlulukları, sağlanan hizmetlere ve çözüm özelliklerini aracılığıyla üst düzey gidin. Alanlar, ağ ve bağlantısı gibi birçok ilgili diğer dört belge ayrıntıları kapsayan ve listeleri ayrıntıya gidin. SAP HANA Azure (büyük örneği) ile ilgili belgelere SAP NetWeaver yükleme yönlerini ve Azure vm'lerinde SAP NetWeaver dağıtımları kapsamaz. SAP NetWeaver Azure üzerinde aynı Azure belgelerine kapsayıcıda bulunan ayrı belgelerinde ele alınmıştır. 


Farklı belgeler HANA büyük örneği kılavuzunun aşağıdaki alanlarda kapsar:

- [SAP HANA (büyük örneği) genel bakış ve Azure üzerinde mimarisi](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure ile ilgili](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) sorun giderme ve Azure üzerinde izleme](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Yüksek kullanılabilirlik STONITH kullanarak SUSE içinde ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [İşletim sistemi yedekleme ve geri yükleme türü II SKU'ları için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Tanımlar

Birkaç ortak tanımları mimari ve teknik dağıtım kılavuzu yaygın olarak kullanılır. Aşağıdaki terimler ve anlamlarını dikkat edin:

- **Iaas:** hizmet olarak altyapı
- **PaaS:** hizmet olarak Platform
- **SaaS:** hizmet olarak yazılım
- **SAP bileşen:** ECC, bant genişliği, çözüm Yöneticisi veya EP'deki gibi tek bir SAP uygulama SAP bileşenleri geleneksel ABAP veya Java teknolojiler ya da olmayan-tabanlı NetWeaver uygulama iş nesneleri gibi temel alabilir.
- **SAP ortamı:** SAP bileşenlerden bir veya daha fazla mantıksal olarak gruplandırılmış geliştirme, QAS, eğitim, DR veya üretim gibi bir iş işlevi gerçekleştirmek için.
- **SAP yatay:** BT yatay tüm SAP varlıkları anlamına gelmektedir. SAP yatay tüm üretim ve üretim dışı ortamlar içerir.
- **SAP sistem:** DBMS katman ve uygulama katmanı SAP ERP geliştirme sistemi, SAP BW test sistemini, SAP CRM üretim sistem, vb. birleşimi. Azure dağıtımları, bu iki şirket içi ve Azure arasında bölünen desteklemez. Bir SAP sistemidir ya da dağıtılmış şirket içi veya Azure'de dağıtılan anlamına gelir. Ancak, Azure veya şirket içi SAP yatay farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM üretim sistem şirket içi dağıtırken Azure, SAP CRM geliştirme ve test sistemlerinde dağıtabilirsiniz. Azure'da (büyük örnekler) SAP HANA için Azure VM'de SAP sistemlerinin SAP uygulama katmanı ve HANA büyük örneği damga biriminde ilgili SAP HANA örneğinde ana yöneliktir.
- **Büyük örneği damga:** SAP HANA TDI olan bir donanım altyapı yığını sertifikalı ve Azure içindeki SAP HANA örnekleri çalıştırmak için ayrılmış.
- **SAP HANA azure'da (büyük örnekler):** SAP HANA büyük örneği Damgalar farklı Azure bölgelerinde dağıtılmış donanım sertifikalı TDI üzerinde HANA çalıştırmak Azure teklifin resmi adı örnekler içinde. İlgili dönem **HANA büyük örneği** azure'da (büyük örnekler) kısaltması SAP HANA ve yaygın olarak kullanılan bu teknik dağıtım kılavuzu.
- **Şirket içi:** VM'ler dağıtıldığı siteden siteye, çok siteli veya şirket içi inizdeki ve Azure arasında ExpressRoute bağlantısı bulunan bir Azure aboneliği için bir senaryo açıklanmaktadır. Ortak Azure belgelerine dağıtımları bu tür şirket içi senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Active Directory/OpenLDAP ve şirket içi DNS Azure'da genişletmek için nedenidir. Şirket içi yatay Azure aboneliklerinden Azure varlıklar için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunucularına erişebilir ve hizmetler (ör. DBMS Hizmetleri) bu sanal makineleri üzerinde çalışabilir. Dağıtılan VM'ler şirket içi ve Azure dağıtılan VM'ler arasında iletişim ve ad çözümleme mümkündür. Örneğin, hangi çoğu SAP varlıklar dağıtılan tipik senaryodur. Kılavuzlardan bkz [planlama ve tasarım VPN ağ geçidi için](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [bir siteden siteye bağlantı Azure portalını kullanarak VNet oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) daha ayrıntılı bilgi için.
- **Kiracı:** HANA büyük örnekleri damga içinde dağıtılan bir müşteri bir "Kiracı." yalıtılmış Bir kiracı ağı, depolama ve bilgi işlem katmanını diğer kiracıdan yalıtılmış. Bu nedenle, farklı kiracıların atanan depolama ve işlem birimleri birbirine göremez veya HANA büyük örneği damga düzeyinde birbirleri ile iletişim kurmak olduğunu. Müşteri dağıtımları farklı kiracıların uygulamasına sahip olmayı seçebilirsiniz. Daha sonra HANA büyük örneği damga düzeyinde kiracılar arasında iletişim yoktur.
- **SKU Kategori:** HANA büyük örnekleri için aşağıdaki iki kategoriden SKU sunulur.
    - **Type ı sınıfı:** S72, S72m, S144, S144m, S192 ve S192m
    - **Tür II sınıfı:** S384, S384m, S384xm, S576, S768 ve S960


Çeşitli Microsoft Azure genel bulut SAP iş yüküne dağıtmayı yayımlanan ek kaynaklar vardır. Herkes planlama ve SAP HANA dağıtımını Azure'da yürütme deneyimli ve Azure Iaas ilkeleri ve Azure Iaas iş yükünü SAP dağıtımının önerilir. Aşağıdaki kaynaklar, daha fazla bilgi sağlayın ve devam etmeden önce başvurulan:


- [Microsoft Azure sanal makinelerde SAP çözümleri kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Sertifika

SAP NetWeaver sertifika yanı sıra Azure Iaas gibi belirli altyapıları kullanan, SAP HANA desteklemek SAP HANA için özel bir sertifika gerektirir.

SAP NetWeaver ve bir derece SAP HANA sertifika için Not çekirdeği [SAP Not #1928533 – Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533).

Bu [SAP Not #2316233 - Microsoft azure'da (büyük örnekler) SAP HANA](https://launchpad.support.sap.com/#/notes/2316233/E) de önemlidir. Bu kılavuzda açıklanan çözüm kapsar. Ayrıca, SAP HANA Azure GS5 VM türünü çalıştırmak için desteklenir. [Bilgi için bu durumda, SAP Web sitesinde yayımlanır](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

SAP HANA SAP Not #2316233 başvurulan Azure (büyük örnekler) çözüm üzerinde Microsoft ve SAP müşteriler büyük SAP Business Suite, SAP Business Warehouse (BW), S/4 HANA, BW/4HANA veya diğer SAP HANA iş yüklerini azure'a dağıtma yeteneği sağlar. SAP HANA sertifikalı ayrılmış donanım damgası tabanlı çözümün ([SAP HANA uyarlanmış Datacenter tümleştirmesi – TDI](https://scn.sap.com/docs/DOC-63140)). SAP HANA TDI çalışan yapılandırılmış çözümün tüm SAP HANA tabanlı uygulamalar (SAP Business Suite SAP HANA üzerinde SAP HANA, S4/HANA ve BW4/HANA SAP Business Warehouse (BW) dahil) donanımda çalışmaya kalacaklarını bilmek güvenirlik sağlar Altyapı.

SAP HANA Azure bu çözümü olan çalışan sanal makinelere, bir avantajı karşılaştırıldığında — kadar büyük bellek birimleri için sağlar. Bu çözüm etkinleştirmek istiyorsanız, anlamak için bazı unsur vardır:

- SAP uygulama katmanı ve SAP olmayan uygulamaları, Azure sanal normal Azure donanım Damgalar barındırılan sanal makineler (VM'ler) çalıştırın.
- Müşterinin şirket içi altyapı, veri merkezleri ve uygulama dağıtımları (önerilen) Azure ExpressRoute aracılığıyla Microsoft Azure bulut platformu ya da sanal özel ağ (VPN) bağlanır. Active Directory (AD) ve DNS ayrıca Azure'da genişletilmiş.
- SAP HANA veritabanı örneği HANA iş yükü için SAP HANA Azure (büyük örnekler) üzerinde çalışır. Azure Vm'lerinde çalışan yazılımı HANA büyük durumlarda HANA örneği ile etkileşim kurabilmesi büyük örneği damga Azure ağ uygulamasına bağlandı.
- SAP HANA Azure (büyük örnekler) ile ilgili donanım SUSE Linux Enterprise Server ile hizmet (Iaas) olarak bir altyapıda sağlanan ayrılmış donanım veya Red Hat Enterprise Linux, önceden yüklü değil. Azure sanal makineler ile daha fazla güncelleştirmeler ve Bakım işletim sistemi için olduğundan sizin sorumluluğunuzdadır.
- Tüm ilgili devam eden işlemler ve Azure üzerinde SAP HANA yönetimler olduğu gibidir, HANA veya SAP HANA HANA büyük örnekleri birimler üzerinde çalıştırmak için gereken diğer ek bileşenleri yüklemesi sizin sorumluluğunuzdadır kullanır.
- Burada açıklanan çözümler yanı sıra, Azure (büyük örnekler) üzerinde SAP HANA bağlandığı Azure aboneliğinizde diğer bileşenleri yükleyebilirsiniz.  Örneğin, iletişimin ile ve/veya SAP HANA veritabanına doğrudan bileşenleri (atlama sunucular, RDP sunucuları, SAP HANA Studio, SAP BI senaryoları için SAP veri hizmetleri veya izleme çözümü ağ).
- Azure olduğu gibi HANA büyük örnekleri destekleyen yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevleri sunar.

## <a name="architecture"></a>Mimari

Bir üst düzey sırasında Azure (büyük örnekler) çözüm üzerinde SAP HANA Azure Vm'lerde bulunan SAP uygulama katmanı ve bir büyük örneği damga Azure Iaas bağlı aynı Azure bölgesinde bulunan SAP yapılandırılmış TDI donanımda bulunan veritabanı katmanı vardır .

> [!NOTE]
> SAP uygulama katmanı SAP DBMS katmanı olarak aynı Azure bölgesinde dağıtmanız gerekir. Bu Azure üzerinde SAP iş yükü hakkında yayımlanan bilgi iyi belgelenmiş kuralıdır. 

SAP HANA Azure (büyük örnekler) ile ilgili genel mimarisi bir SAP TDI sertifikalı donanım yapılandırması (sanallaştırılmamış, çıplak metal, SAP HANA veritabanı için yüksek performanslı sunucusu) özelliği ve Azure esneklik için ölçek kaynakları sağlar ihtiyaçlarınızı karşılamak için SAP uygulama katmanı.

![SAP HANA Azure (büyük örnekler) ile ilgili mimari genel bakış](./media/hana-overview-architecture/image1-architecture.png)

Gösterilen mimarisi üç bölümlere ayrılmıştır:

- **Sağ:** son LOB uygulamaları (örneğin, SAP) erişen kullanıcılar ile veri merkezlerinde farklı uygulamaları çalıştıran bir şirket içi altyapı. İdeal olarak, bu altyapı sonra Azure ile Azure bağlı şirket içi [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Merkezi:** gösterir Azure Iaas ve bu durumda, SAP veya SAP HANA DBMS sistem olarak kullanan diğer uygulamalar barındırmak için Azure Vm'leri kullanın. Azure VM'ler bellek işleviyle sağlayan küçük HANA örnekleri Azure Vm'lerde kendi uygulama katmanı ile birlikte dağıtılır. Hakkında daha fazla bilgi bulmak [sanal makineleri](https://azure.microsoft.com/services/virtual-machines/).
<br />Azure ağı SAP sistemleri Azure sanal ağlar (Vnet'ler) diğer uygulamalara birlikte gruplamak için kullanılır. Şirket içi sistemlere de Azure (büyük örnekler) üzerinde SAP HANA konusunda bu Vnet'lerdeki bağlayın.
<br />SAP NetWeaver uygulamalar ve Microsoft Azure üzerinde çalıştırmak için desteklenen veritabanları için bkz: [SAP destek Not #1928533 – Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533). Azure SAP çözümlerini dağıtma belgelerini gözden geçirin:

  -  [Windows sanal makinelerde (VM'ler) SAP kullanma](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Microsoft Azure sanal makinelerde SAP çözümleri kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Sol:** gösterir SAP HANA TDI sertifikalı donanım Azure büyük örneği damgasında. Şirket içi bağlantı Azure içine olarak aynı teknolojiyi kullanarak aboneliğinizin Azure Vnet HANA büyük örneği birimleri bağlı.

Azure büyük örneği damga aşağıdaki bileşenleri birleştirir:

- **Bilgi işlem:** gerekli hesaplama yeteneği sağlamak ve SAP HANA sertifikalı olan Intel Xeon E7-8890v3 veya Intel Xeon E7-8890v4 işlemcilerini esas alan sunucuları.
- **Ağ:** A birleşik bilgi işlem, depolama ve LAN bileşenleri bağlantılar yüksek hızlı ağ yapısı.
- **Depolama:** birleşik ağ yapısı erişilen bir depolama altyapısını. Belirli bir depolama kapasitesi, Azure (büyük örnekler) yapılandırmasında dağıtılan belirli SAP HANA bağlı olarak sağlanır (daha fazla depolama kapasitesi ek bir aylık ücret kullanılabilir).

Büyük örneği damga çok kiracılı altyapısı içinde müşteriler yalıtılmış Kiracı dağıtılır. Kiracı dağıtımı sırasında bir Azure aboneliğine Azure kaydınızı içinde adı gerekir. Bu Azure aboneliği olacağını, HANA büyük örnekler karşı faturalandırılmaya geçiyor. Bu kiracılar Azure aboneliği 1:1 ilişkisi. Ağ akıllıca, farklı Azure abonelikleri ait farklı Azure Vnet gelen bir Azure bölgesindeki bir kiracı dağıtılmış HANA büyük örneği birim erişim mümkündür. Ancak bu Azure abonelikleri aynı Azure kayıt ait olmaları gerekir. 

Azure vm'lerle gibi Azure (büyük örnekler) üzerinde SAP HANA birden çok Azure bölgelerinde sunulur. Olağanüstü durum kurtarma özellikleri sunmak için kabul seçebilirsiniz. Farklı büyük örneği Damgalar bir siyasi coğrafi bölge içinde birbirlerine bağlı. Örneğin, HANA büyük örneği damgaları Batı ABD ve BİZE Doğu, DR çoğaltma amacıyla bir adanmış ağ bağlantısı üzerinden bağlanır. 

Farklı VM türler ile Azure sanal makineler arasında yalnızca seçebileceğiniz gibi farklı SKU'ları, HANA büyük SAP HANA farklı iş yükü türleri için özel olarak hazırlanmış örnekleri arasından seçim yapabilirsiniz. SAP bellek işlemci yuva oranları üzerinde Intel işlemci nesli göre değişen iş yükleri için geçerlidir. Aşağıdaki tabloda sunulan SKU türleri gösterilmektedir.

Temmuz 2017 itibariyle SAP HANA Azure (büyük örnekler) ile ilgili çeşitli yapılandırmaları Azure bölgeleri, ABD Batı ABD Doğu, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa ve Kuzey Avrupa kullanılabilir:

| SAP Çözümü | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| OLAP için en iyi duruma getirilmiş: SAP BW, BW/4HANA<br /> veya SAP HANA Genel OLAP iş yükü için | SAP HANA Azure S72 üzerinde<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU çekirdekleri ve 72 CPU iş parçacıkları |  768 GB |  3 TB | Kullanılabilir |
| --- | SAP HANA on Azure S144<br /> – 4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU çekirdekleri ve 144 CPU iş parçacıkları |  1,5 TB |  6 TB | Artık sunulmuyor |
| --- | SAP HANA Azure S192 üzerinde<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU çekirdekleri ve 192 CPU iş parçacıkları |  2.0 TB |  8 TB | Kullanılabilir |
| --- | SAP HANA Azure S384 üzerinde<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU çekirdekleri ve 384 CPU iş parçacıkları |  4.0 TB |  16 TB | Kullanılabilir |
| OLTP için en iyi duruma getirilmiş: SAP Business paketi<br /> SAP HANA veya S/4HANA (OLTP)<br /> Genel OLTP | SAP HANA Azure S72m üzerinde<br /> – 2 x Intel® Xeon® Processor E7-8890 v3<br /> 36 CPU çekirdekleri ve 72 CPU iş parçacıkları |  1,5 TB |  6 TB | Kullanılabilir |
|---| SAP HANA Azure S144m üzerinde<br /> – 4 x Intel® Xeon® Processor E7-8890 v3<br /> 72 CPU çekirdekleri ve 144 CPU iş parçacıkları |  3.0 TB |  12 TB | Artık sunulmuyor |
|---| SAP HANA Azure S192m üzerinde<br /> – 4 x Intel® Xeon® Processor E7-8890 v4<br /> 96 CPU çekirdekleri ve 192 CPU iş parçacıkları  |  4.0 TB |  16 TB | Kullanılabilir |
|---| SAP HANA Azure S384m üzerinde<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU çekirdekleri ve 384 CPU iş parçacıkları |  6.0 TB |  18 TB | Kullanılabilir |
|---| SAP HANA Azure S384xm üzerinde<br /> – 8 x Intel® Xeon® Processor E7-8890 v4<br /> 192 CPU çekirdekleri ve 384 CPU iş parçacıkları |  8.0 TB |  22 TB |  Kullanılabilir |
|---| SAP HANA Azure S576 üzerinde<br /> – 12 x Intel® Xeon® Processor E7-8890 v4<br /> 288 CPU çekirdekleri ve 576 CPU iş parçacıkları |  12.0 TB |  28 TB | Kullanılabilir |
|---| SAP HANA Azure S768 üzerinde<br /> – 16 x Intel® Xeon® Processor E7-8890 v4<br /> 384 CPU çekirdekleri ve 768 CPU iş parçacıkları |  16.0 TB |  36 TB | Kullanılabilir |
|---| SAP HANA Azure S960 üzerinde<br /> – 20 x Intel® Xeon® Processor E7-8890 v4<br /> 480 CPU çekirdekleri ve 960 CPU iş parçacıkları |  20.0 TB |  46 TB | Kullanılabilir |

- CPU çekirdekleri = olmayan-hiper iş parçacıklı CPU çekirdeği sunucusu birimi işlemcileri toplamı toplamı.
- CPU iş parçacıkları = hiper iş parçacıklı CPU çekirdeği sunucusu birimi işlemcileri toplamı tarafından sağlanan işlem iş parçacıklarının toplamı. Tüm birimlerin varsayılan olarak Hyper-Threading kullanacak şekilde yapılandırılır.


Seçilen belirli yapılandırmalar, iş yükü, CPU kaynaklarını ve istenen bellek bağımlıdır. OLAP iş yükü için en iyi duruma getirilir SKU'ları kullanmak OLTP iş yükü için mümkündür. 

SAP HANA sertifikalı TDI tüm teklifleri için donanım temel var. Ancak, biz içine SKU'ları böler donanımı, iki farklı sınıflar arasında ayrım:

- S72, S72m, S144, S144m, S192 ve adlandırdığımız 'ı sınıf türü olarak' S192m SKU.
- S384, S384m, S384xm, S576, S768 ve adlandırdığımız 'Type II sınıfı' SKU'ları S960.

Tam HANA büyük örneği damga yalnızca tek bir müşteri &#39; ayrılmamış olduğunu dikkate almak önemlidir s kullanın. Azure üzerinde de dağıtılan ağ yapısı bağlı işlem ve depolama kaynaklarını raflarının olgunun uygular. Azure gibi HANA büyük örnekleri altyapı, farklı müşteri dağıtır &quot;kiracılar&quot; birbirlerinden aşağıdaki üç düzeyin yalıtılmış olan:

- Ağ: HANA büyük örneği damga içindeki sanal ağlar yalıtımı.
- Depolama: Depolama birimleri depolama sanal makinelerde yalıtımı atanan ve depolama birimleri kiracılar arasında yalıtır.
- İşlem: sunucu birimlerini atamasının tek bir kiracı için ayrılmış. Hayır sabit veya sunucu birimleri için yumuşak bölümleme. Hiçbir tek bir sunucu veya konak birimi kiracılar arasında paylaştırma. 

Bu nedenle, farklı kiracılar arasında HANA büyük örnekleri birimleri dağıtımları diğer için görünür değildir. Ya da HANA büyük örneği farklı kiracılar dağıtılan birimleri diğer HANA büyük örneği damga düzeyi ile doğrudan iletişim kurabilir. Yalnızca HANA büyük örneği biriminde bulunan bir kiracı HANA büyük örneği damga düzeyinde birbiriyle iletişim kurabilir.
Büyük örneği damgasında dağıtılan bir kiracı için bir Azure aboneliği akıllıca faturalama atanır. Ancak, aynı Azure kayıt içinde başka Azure abonelikleri Azure Vnet gelen erişilebilir akıllıca ağ. Başka bir Azure aboneliğiniz aynı Azure bölgesinde dağıtırsanız, ayrılmış bir HANA büyük örneği Kiracı için sormak seçebilirsiniz.

SAP HANA HANA büyük örnekleri ve SAP HANA Azure'de dağıtılan Azure vm'lerinde çalışan çalışan arasında önemli farklılıklar vardır:

- SAP HANA (büyük örnekler) azure'da yok sanallaştırma katmanı yoktur. Temel alınan tam donanım performansını alın.
- Azure farklı olarak, Azure (büyük örnekler) sunucusunda SAP HANA belirli bir müşteriye ayrılmış. Bir sunucu birim veya ana bilgisayarın sabit veya geçici bölümlenmiş hiçbir olasılığı vardır. Sonuç olarak, bir HANA büyük örneği birimi bir bütün olarak bir kiracı ve, size bir müşteri olarak atanmış olarak kullanılır. Bir yeniden başlatma veya kapatma sunucusunun otomatik olarak işletim sistemine ve başka bir sunucu üzerinde dağıtılan SAP HANA neden olmaz. (Türü için SKU'ları sınıfı, bir sunucu sorunlarla karşılaşabilirsiniz ve yeniden dağıtım gereken başka bir sunucu üzerinde gerçekleştirilecek tek özel durumdur.)
- Burada ana bilgisayar İşlemci türleri için en iyi fiyat/performans oranı seçilir, Azure, SAP HANA azure'da (büyük örnekler) için seçilen işlemci yüksek Intel E7v3 ve E7v4 işlemci satırının gerçekleştirme türleridir.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Çalışan bir HANA büyük örneği biriminde birden çok SAP HANA örnekler
HANA büyük örneği birimleri birden fazla etkin SAP HANA örneğinde barındırmak mümkündür. Hala depolama anlık görüntüler ve olağanüstü durum kurtarma özellikleri sağlamak için bu tür bir yapılandırma örneği bir birim gerektirir. Şimdi itibariyle HANA büyük örneği birimler gibi bölünmüştür:

- S72, S72m, S144, S192: artışlarla 256 GB en küçük başlangıç birim 256 GB. 256 GB, 512 GB ve vb. gibi farklı aralıklarla birimi belleğinin en birleştirilebilir.
- S144m ve S192m: 512 GB en küçük birim 256 GB artışlarla. 512 GB, 768 GB ve vb. gibi farklı aralıklarla birimi belleğinin en birleştirilebilir.
- Tür II sınıfı: 2 TB'lık en küçük başlangıç birimiyle 512 GB artışlarla. 512 GB, 1 TB, 1, 5 TB ve vb. gibi farklı aralıklarla birimi belleğinin en birleştirilebilir.

Birden çok SAP HANA örneği çalıştıran bazı örnekler gibi görünebilir:

| SKU | Bellek Boyutu | Depolama Boyutu | Birden çok veritabanı boyutlarıyla |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA örneği<br /> ya da 1 x 512 GB örneği + 1 x 256 GB örneği<br /> ya da 3 x 256 GB örnekleri | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA örnekleri<br />ya da 1 x 512 GB örneği + 1 adet 1 TB örneği<br />ya da 6 x 256 GB örnekleri<br />veya 1x1.5 TB örneği | 
| S192m | 4 TB | 16 TB | 8 x 512 GB örnekleri<br />veya 4 x 1 TB örnekleri<br />ya da 4 x 512 GB örnekleri + 2 x 1 TB örnekleri<br />ya da 4 x 768 GB örnekleri + 2 x 512 GB örnekleri<br />ya da 1 x 4 TB örneği |
| S384xm | 8 TB | 22 TB | 4 x 2 TB örnekleri<br />ya da 2 x 4 TB örnekleri<br />ya 2 x 3 TB örnekleri + 1 x 2 TB örnekleri<br />ya da 2x2.5 TB örnekleri + 1 x 3 TB örnekleri<br />ya da 1 x 8 TB örneği |


Fikir alın. Kesinlikle vardır diğer Çeşitleme de. 

### <a name="using-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA veri katmanlama ve uzantısı düğümleri kullanarak
SAP SAP BW farklı SAP NetWeaver sürümlerini ve SAP BW/4HANA için veri katmanlama modelini destekler. Ayrıntılar için modeli, bu belge ve bu belgede SAP tarafından başvurulan blog bulunabilir veri katmanlama ilgili: [SAP BW/4HANA ve SAP BW ON HANA ile SAP HANA UZANTISI düğümleri](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
HANA büyük örnekleriyle bu SSS ve SAP blog belgelerde ayrıntılı olarak seçeneği 1 yapılandırma SAP HANA uzantısı düğümlerinin kullanabilirsiniz. Seçenek 2 yapılandırmaları ayarlanabilir aşağıdaki HANA büyük örneği SKU'ları ile: S72m, S192, S192m, S384 ve S384m.  
Avantajı adresindeki arayan hemen görünür olmayabilir. Ancak SAP boyutlandırma yönergeleri baktığınızda, bir avantajı seçeneği-1 ve 2 seçeneği SAP HANA uzantısı düğümleri kullanarak görebilirsiniz. Burada bir örnek:

- SAP HANA boyutlandırma yönergeleri, genellikle belleği double veri birimi miktarını gerektirir. Bu nedenle, SAP HANA örneğinizi dinamik verilerle çalışırken, yalnızca % 50 sahip veya daha az bellek verilerle doldurulur. Bellek geri kalanı, SAP HANA işini yapmak için ideal olarak tutulur.
- Anlamına HANA büyük örneği S192 biriminde 2 çalıştıran bir SAP BW veritabanı TB bellek, 1 TB veri birimi olarak yeterlidir.
- Ek bir SAP HANA uzantısı düğümünü seçeneği 1 kullanırsanız, ayrıca bir S192 HANA büyük örneği SKU, onu bir ek veri birimi 2 TB kapasite verirsiniz. Seçenek 2 yapılandırma bile ve ek 4 TB sıcak veri birimi için. Etkin düğüme ile karşılaştırıldığında, 'Normal' uzantısı düğümünde tam bellek kapasitesini seçeneği 1 için depolama ve çift bellek veri hacmi seçeneği 2 SAP HANA uzantısı düğüm yapılandırması için kullanılabilir verileri için kullanılabilir.
- Sonuç olarak, 3 TB kapasite ile verilerinizi ve 1:2 seçenek 1 için yarı ile hot oranını ve veri ve bir seçenek 2 uzantısı düğüm yapılandırması 1:4 oranı 5 TB sonlanır.

Ancak, bellek, daha yüksek olasılığını sıcak veri için istiyoruz, karşılaştırılır daha yüksek veri birimi disk depolama alanında depolanır.


## <a name="operations-model-and-responsibilities"></a>İşlem modeli ve sorumlulukları

SAP HANA azure'da (büyük örnekler) ile sağlanan hizmet Azure Iaas Hizmetleri ile hizalanır. SAP HANA için en iyi duruma getirilmiş yüklü bir işletim sistemi HANA büyük örnekleri örnek alın. Azure Iaas Vm'leri gibi işletim sistemi sağlamlaştırma görevlerinin çoğunu ile ek yazılım yükleme ihtiyacınız HANA yükleme, işletim sistemi ve HANA, ve sizin sorumluluğunuzdadır HANA ve işletim sistemi güncelleştirmek olur. Microsoft işletim sistemi güncelleştirmeleri veya HANA güncelleştirmeleri size zorlamaz.

![SAP HANA (büyük örnekler) azure'da sorumlulukları](./media/hana-overview-architecture/image2-responsibilities.png)

Yukarıdaki diyagramda görüldüğü gibi SAP HANA (büyük örnekler) azure'da çok kiracılı altyapı bir hizmet teklifidir. Ve sonuç olarak, sorumluluk bölümünün OS altyapı sınırında çoğunlukla. Microsoft hizmet satırın işletim sisteminin aşağıda tüm yönlerini sorumludur ve işletim sistemi dahil satır sorumludur. Bu nedenle uyumluluk, güvenlik, uygulama yönetimi, temel ve işletim sistemi yönetimi için en güncel şirket içi yöntemleri kullanan, kullanılacak devam edebilirsiniz. Sistemleri ağınızdaki tüm gelince oldukları gibi görünür.

Ancak, Microsoft en iyi sonuçlar için temel altyapı özellikleri kullanmak için birlikte çalışması için gereken yeri alanları olduklarından bu hizmet SAP HANA için optimize edilmiştir.

Aşağıdaki listede daha fazla ayrıntı her katmanları ve sorumluluklarınızın sağlar:

**Ağ:** tüm iç ağlar için SAP HANA, depolama, örnekleri (için genişleme ve diğer işlevleri), bağlantı yatay arasında bağlantı ve Azure bağlantısı kendi erişimi çalışan büyük örneği damga nerede SAP uygulama katmanı, Azure sanal makinelerinde barındırılır. Ayrıca, olağanüstü durum kurtarma amacıyla çoğaltma için Azure veri merkezleri arasında geniş ağ bağlantısı içerir. Tüm ağları Kiracı tarafından bölümlenir ve QOS uyguladığınız.

**Depolama:** sanallaştırılmış bölümlenmiş anlık görüntü yanı sıra, SAP HANA sunucuları tarafından gerekli tüm birimler için depolama. 

**Sunucular:** SAP HANA kiracılar için atanan DBs çalıştırmak için adanmış fiziksel sunucular. T türü sunucuları sınıf soyutlanır donanım SKU'lar. Sunucular bu tür ile sunucu yapılandırmasını toplanır ve bir fiziksel donanım için başka bir fiziksel donanım taşınabilir profilleri tutulur. Bu tür bir (el ile) taşıma profil işlemler Azure hizmet onarma için biraz karşılaştırılabilir. Böyle bir özellik türü II sınıfı SKU'ları sunucuları sunulmamaktadır.

**SDDC:** yazılım tanımlı varlıklar verileri yönetmek için kullanılan yönetim yazılımı merkezleri. Microsoft, Ölçek, kullanılabilirlik ve performansı artırmak için havuz kaynakları olanak sağlar.

**İşletim sistemi:** (SUSE Linux veya Red Hat Linux) işletim sistemi, seçtiğiniz sunucular üzerinde çalışır. Sağlanan işletim sistemi görüntüleri SAP HANA çalıştıran amacıyla Microsoft'a tek tek Linux satıcısı tarafından sağlanan görüntüleri bağımsızdır. Linux satıcısı belirli SAP HANA iyileştirilmiş görüntüsü için bir aboneliğe sahip olmanız gerekir. Görüntüleri işletim sistemi satıcıyla birlikte kaydetme, sorumlulukları içerir. Microsoft tarafından handover noktadan itibaren ayrıca tüm ek Linux işletim sistemi düzeltme eki uygulama için sorumluluğu size aittir. Bu aynı zamanda düzeltme başarılı bir SAP HANA yüklemesi için gerekli olabilecek ek paketler içerir (SAP'ın HANA yükleme belgelerini ve SAP notlara bakın) ve hangi kullanıcıların SAP HANA belirli Linux satıcı tarafından dahil edilmemiş OS en iyi duruma getirilmiş görüntüler. Müşteri sorumluluğu da arıza / OS ve belirli sunucu donanımı için ilgili sürücülerini iyileştirmesi ilgili işletim sistemi düzeltme eki uygulama içerir. Veya bir güvenlik veya işletim sistemini işlevsel düzeltme eki uygulama. Müşteri'nin sorumluluğu da izleme ve kapasite planlamasına ait:

- CPU kaynak tüketimi
- Bellek tüketimi
- Disk birimleri boş alan, IOPS ve gecikme süresi ile ilgili
- HANA büyük örneği ve SAP uygulama katmanı arasındaki ağ birim trafiği

Altyapının HANA büyük örneklerinin yedekleme ve geri yükleme işletim sistemi birimi için işlevsellik sağlar. Bu işlevselliği de sizin sorumluluğunuzdadır kullanmaktır.

**Ara yazılım:** öncelikle SAP HANA örneği. Yönetim, işlemleri ve izleme sizin sorumluluğunuzdadır. Yedekleme/geri yükleme ve olağanüstü durum kurtarma amacıyla depolama anlık görüntüleri kullanmanıza olanak sağlayan sağlanan işlevi yoktur. Bu özellikler altyapısı tarafından sağlanır. Ancak, sizin Sorumluluklarınız bu özellikler sayesinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarımı, bunları yararlanan ve depolama anlık görüntüleri başarıyla çalıştırılıncaya izleme içerir.

**Veri:** SAP HANA tarafından yönetilen, verilerinizi ve yedekleme gibi diğer verilerin birimlerde bulunan dosya veya dosya paylaşımları. Sizin Sorumluluklarınız boş disk alanı izleme ve birimler üzerinde içeriği yönetme ve disk birimleri ve depolama anlık görüntüleri yedeklerini aktarılmadığı izleme içerir. Ancak, DR sitelere veri çoğaltması başarılı yürütme Microsoft sorumluluğundadır.

**Uygulamalar:** SAP uygulama örnekleri veya SAP olmayan uygulamaları, uygulama katmanı söz konusu uygulamaların durumunda. Dağıtım, yönetim, işlemleri ve bu uygulamaların ilgili CPU kaynak kullanımı, bellek tüketimi, Azure depolama alanı tüketimi ve ağ bant genişliği tüketimi içinde kapasite planlama için izleme, sorumlulukları içerir Azure sanal ağlar, Azure (büyük örnekler) üzerinde SAP HANA Azure sanal ağlara gelen ve giden.

**WAN:** iş yükleri için Azure dağıtımları için şirket içi belirlediğiniz bağlantıları. Tüm müşterilerimizin HANA büyük örneğiyle Azure ExpressRoute bağlantısı kullanın. Bu bağlantı ayarı için sorumlu; bu nedenle bu bağlantı SAP HANA Azure (büyük örnekler) çözüm üzerinde bir parçası değil.

**Arşiv:** depolama hesaplarında kendi yöntemlerini kullanarak verileri kopyalarını arşivlemek tercih edebilirsiniz. Arşivleme yönetimi, uyumluluk, maliyetleri ve işlemler gerektirir. Arşiv kopyalar ve Azure üzerinde yedeklemeler oluşturmak ve bunları uyumlu bir biçimde saklamak için sorumlu.

Bkz: [azure'da (büyük örnekler) SAP HANA SLA](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Boyutlandırma

Boyutlandırma HANA büyük örnekleri için HANA için genel boyutlandırma daha farklı değildir. Mevcut ve sistemler, dağıtılan taşımak diğer RDBMS HANA için SAP sağlayan bir dizi varolan SAP sistemlerinize çalıştırmak rapor istediğiniz. Veritabanı için HANA taşınırsa, bu raporları verileri denetleyin ve HANA örneği için bellek gereksinimleri hesaplayın. Bu raporların nasıl çalıştırılacağı ve bunların en son düzeltme eklerinin/sürümlerini edinme hakkında daha fazla bilgi almak için aşağıdaki SAP notları okuyun:

- [SAP Not #1793345 - HANA üzerinde SAP paketi için boyutlandırma](https://launchpad.support.sap.com/#/notes/1793345)
- [Not #1872170 - Suite HANA ve S/4 HANA boyutlandırma rapor SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP Not #2121330 - SSS: SAP BW rapor boyutlandırma HANA üzerinde](https://launchpad.support.sap.com/#/notes/2121330)
- [Not #1736976 - HANA boyutlandırma rapor BW için SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [HANA üzerinde BW için Not #2296290 - yeni boyutlandırma rapor SAP](https://launchpad.support.sap.com/#/notes/2296290)

Yeşil alan uygulamaları için SAP hızlı Boyutlandırıcı SAP HANA üstünde yazılım uygulamasıdır bellek gereksinimlerini hesaplamak kullanılabilir.

Veri birimi büyüdükçe, şimdi bellek tüketimi unutmayın ve onu gelecekte olmasını neler olduğunu tahmin etmek için istediğiniz şekilde HANA için bellek gereksinimleri artmaktadır. Bellek gereksinimlerine bağlı olarak, ardından, isteğe bağlı HANA büyük örneği SKU birine eşleyebilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu liste, SAP HANA (büyük örnekler) Azure üzerinde çalıştırmak için gereksinimler derler.

**Microsoft Azure:**

- SAP HANA azure'da (büyük örnekler) için bağlı bir Azure aboneliği.
- Microsoft Premier Destek sözleşmenizi. Bkz: [SAP destek Not #2015553 – Microsoft Azure üzerinde SAP: Destek Önkoşullar](https://launchpad.support.sap.com/#/notes/2015553) SAP Azure'da çalıştırma ile ilgili daha ayrıntılı bilgi için. HANA büyük örneği birimleri 384 ve daha fazla CPU ile kullanarak, ayrıca Azure hızlı yanıt (ARR) içerecek şekilde Premier Destek sözleşmenizi genişletmek gerekir.
- Tanıma HANA büyük örneklerinin SAP ile SKU bir boyutlandırma gerçekleştirildikten sonra uygulamanız.

**Ağ bağlantısı:**

- Şirket içi Azure arasında Azure ExpressRoute: şirket içi veri merkeziniz Azure'a bağlanmak için en az 1 GB/sn bağlantı ISS'niz tarafından sipariş emin olun. 

İşletim Sistemi:

- SUSE Linux Enterprise Server 12 SAP uygulamaları için lisans.

> [!NOTE] 
> Microsoft tarafından sunulan işletim sistemi ile SUSE kayıtlı değil ya da bir SMT örneği ile bağlı.

- SUSE Linux Abonelik Yönetim Aracı (Azure VM'de azure'da dağıtılan SMT). SAP HANA azure'da (örnekler büyük kayıtlı ve (olduğundan HANA büyük örnekleri veri merkezi içinde Internet erişimi yok) tarafından SUSE sırasıyla güncelleştirilmiş) için yeteneği sağlar. 
- Red Hat Enterprise Linux 6.7 veya 7.2 SAP HANA için için lisans.

> [!NOTE]
> Microsoft tarafından sunulan işletim sistemi ile Red Hat kayıtlı değil ya da bir Red Hat Abonelik Yöneticisi örneğine bağlı.

- Red Hat Abonelik Yöneticisi Azure bir Azure VM üzerinde dağıtılabilir. Red Hat Abonelik Yöneticisi'ni (örnekler büyük kayıtlı ve hiçbir doğrudan internet içinden erişim Azure büyük örneği damgası üzerinde dağıtılan Kiracı (olduğu gibi) tarafından Red Hat sırasıyla güncelleştirilmiş) azure'da SAP HANA yeteneği sağlar.
- SAP destek sahip olmanızı gerektirir de Linux sağlayıcınız ile bir sözleşme. Bu gereksinim HANA büyük örnekleri veya olgu çözümü tarafından silinmeyen, azure'da çalışma, Linux. Farklı olarak bazı Linux Azure galeri görüntüleri hizmeti ücret HANA büyük örneklerinin çözüm teklife dahil edilmez. Bu, SAP Linux dağıtıcı ile gerekliliklerini destek sözleşmeleri ile ilgili müşteri gibidir.   
   - SUSE Linux için destek sözleşmede gereksinimlerini aramak [SAP Not #1984787 - SUSE LINUX Enterprise Server 12: yükleme notları](https://launchpad.support.sap.com/#/notes/1984787) ve [SAP Not #1056161 - SUSE öncelik SAP uygulamalarıdesteğini](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux için destek içerir ve (güncelleştirmeler HANA büyük örneklerinin işletim sistemlerine. Hizmet doğru aboneliğin düzeyleri olması gerekiyor. Red Hat önerir alınırken bir "için RHEL [SAP çözümleri](https://access.redhat.com/solutions/3082481)" abonelik. 

Farklı Linux sürümleri farklı SAP HANA sürümüyle için destek matrisi denetleyin [SAP Not #2235581](https://launchpad.support.sap.com/#/notes/2235581).


**Veritabanı:**

- Lisansları ve SAP HANA (platform veya enterprise edition) için yazılım yükleme bileşenleri.

**Uygulamalar:**

- Lisansları ve SAP HANA bağlanma ve SAP ilgili tüm SAP uygulamaları için yazılım yükleme bileşenleri sözleşmeleri destekler.
- Lisansları ve yazılım yükleme bileşenleri tüm SAP olmayan uygulamalar için destek sözleşmeleri ilgili ve Azure (büyük örnekler) ortamda SAP HANA bağlantılı olarak kullanılır.

**Yetenekler:**

- Deneyimi ve Azure Iaas ve bileşenleri hakkında bilgi.
- Deneyimi ve Azure SAP iş yükü dağıtma hakkında bilgi.
- SAP HANA yükleme kişisel onaylandı.
- SAP Mimarı becerileri tasarım yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA geçici.

**SAP:**

- Beklenir bir SAP müşterisiyseniz ve bir desteğine sahip SAP ile sözleşme
- Özellikle uygulamalarında için HANA büyük örneği SKU'ları türü II sınıfının, SAP HANA ve büyük ölçekli büyütme donanımda nihai yapılandırmaları sürümlerinde SAP ile incelediğinizden kullanmamanız önerilir.


## <a name="storage"></a>Depolama

SAP HANA azure'da (büyük örnekler) için depolama düzeni SAP HANA kısmında belgelenen yönergeler, önerilen SAP aracılığıyla Azure Hizmet Yönetimi tarafından yapılandırılan [SAP HANA depolama gereksinimleri](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi.

I sınıf türünün HANA büyük örneklerini dört kez bellek toplu depolama birimi olarak gelir. HANA büyük örneği birimlerinin II sınıf türü için depolama dört kat daha fazla olacağını değil. Birimleri HANA işlem günlüğü yedeklemeleri depolamak için amaçlanan bir birim gelir. Daha ayrıntılı olarak Bul [yükleyin ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Depolama ayırma bakımından aşağıdaki tabloya bakın. Tablo kabaca farklı HANA büyük örneği birimleriyle sağlanan farklı birimler için kapasite listeler.

| HANA büyük örneği SKU | hana/veri | hana/günlük | hana ve paylaşılan | Günlük/hana/yedekleme |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1024 GB | 1536 GB | 1024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12.000 GB | 2050 GB | 2050 GB | 2040 GB |
| S384xm | 16.000 GB | 2050 GB | 2050 GB | 2040 GB |
| S576 | 20.000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | 28,000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36,000 GB | 4100 GB | 2050 GB | 4100 GB |


Gerçek Dağıtılmış birimler dağıtım ve birim boyutlarını göstermek için kullanılan aracı biraz göre farklılık gösterebilir.

HANA büyük örneği SKU ayırabilir, birkaç olası bölme parça örnekleri gibi görünür:

| GB bellek bölümü | hana/veri | hana/günlük | hana ve paylaşılan | Günlük/hana/yedekleme |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Bu boyutlar biraz dağıtım ve birimlerinin aramak için kullanılan araçlar göre değişebilir kaba birim numaralarıdır. Ayrıca diğer bölüm boyutu vardır gibi 2,5 TB thinkable. Bu depolama boyutları olarak kullanılan yukarıdaki bölümler için benzer bir formülü olan hesaplanır. Terim 'partitions' işletim sistemi, bellek veya CPU kaynaklarını herhangi şekilde bölümlenmiş olduğunu göstermez. Yalnızca tek bir HANA büyük örneği birimde dağıtmak isteyebilirsiniz farklı HANA örnekleri için depolama bölüm de gösterir. 

Bir müşteri olabilir gibi daha fazla depolama alanı için gerekir, 1 TB birimler ek depolama alanı satın almak için depolama alanı eklemek için olanağına sahip olursunuz. Bu ek depolama alanı ek bir birim eklenebilir veya bir veya daha fazla var olan birimler genişletmek için kullanılabilir. İlk olarak dağıtılan ve genellikle yukarıdaki tablo tarafından belgelenen şekilde birimlerin boyutunu azaltmak mümkün değildir. Ayrıca birimleri adlarını değiştirmek veya adları bağlama mümkün değil. Yukarıda açıklandığı gibi depolama birimleri HANA büyük örneği birimleri NFS4 birimler olarak eklenir.

Bir müşteri olarak yedekleme/geri yükleme ve olağanüstü durum kurtarma amacıyla depolama anlık görüntüleri kullanmayı seçebilirsiniz. Daha fazla ayrıntı ayrıntıları [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure üzerinde](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi
Disklerde depolanan gibi HANA büyük örnekleri için kullanılan depolama, veri saydam bir şifreleme sağlar. HANA büyük örneği birim dağıtım sırasında bu tür bir şifreleme etkin olmasını seçeneğiniz vardır. Şifrelenmiş birimlere zaten dağıtımdan sonra değiştirmek seçebilirsiniz. Şifrelenmemiş taşımak şifrelenmiş birimler için saydamdır ve bir kapalı kalma süresi gerektirmez. 

SKU'ları, LUN üzerinde depolanan önyükleme birimi sınıf türü ile şifrelenir. II sınıf örneklerinin SKU'ları, HANA büyük türü durumunda işletim sistemi yöntemleriyle LUN önyükleme şifrelemeniz gerekir. Daha fazla bilgi için Microsoft Hizmet Yönetimi ekibine başvurun.


## <a name="networking"></a>Ağ

Azure ağ mimarisi, SAP uygulamaları HANA büyük örneklerinde'nın başarılı dağıtımı için kilit bir bileşendir. Genellikle, Azure (büyük örnekler) dağıtımlarda SAP HANA veritabanları, CPU kaynak kullanımı ve bellek kullanımı çeşitli boyutlarda birkaç farklı SAP çözümleriyle birlikte daha büyük bir SAP yatay vardır. Büyük olasılıkla SAP yatay büyük olasılıkla kullanan bir karma olacak şekilde bu SAP sistemleri tüm SAP HANA üzerinde temel alır:

- SAP sistemleri şirket içinde dağıtılabilir. Boyutlarının nedeniyle bu sistemler şu anda Azure'da barındırılamaz; Klasik bir örnek, Microsoft SQL Server (veritabanı) olarak daha fazla CPU gerektirir veya Azure VM'ler sağlayabilir bellek kaynakları üzerinde çalışan bir üretim SAP ERP sistemi olabilir.
- SAP HANA tabanlı SAP sistemleri şirket içi dağıtıldı.
- Azure vm'lerinde dağıtılan SAP sistemleri. Sınama, korumalı alan, geliştirme, bu sistemler olabilir veya üretim (vm'lerinde), kaynak kullanımı ve bellek talebe göre azure'da başarıyla dağıtabilirsiniz SAP NetWeaver tabanlı uygulamalardan herhangi biri için örnekler. Bu sistemler de veritabanlarını SQL Server gibi temel alabilir (bkz [SAP destek Not #1928533 – Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E)) veya SAP HANA (bkz [SAP HANA sertifikalı Iaas platformları](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- SAP HANA (büyük örneği) azure'da Azure büyük örneği Damgalar yararlanan SAP uygulama sunucuları (VM'ler üzerinde) azure'da dağıtılabilir.

Karma (ile dört veya daha fazla farklı dağıtım senaryolarında) SAP yatay tipik olsa da, Azure'da çalışan tam SAP yatay, birçok müşteri durumlar vardır. Microsoft Azure sanal makineleri daha güçlü gelmektedir gibi Azure üzerinde tüm SAP çözümleri taşıma müşteri sayısını artırma.

Azure üzerinde dağıtılan SAP sistemleri bağlamında ağ azure karmaşık değil. Aşağıdaki kurallara göre dayanır:

- Azure sanal ağlar (Vnet'ler) şirket ağına bağlanan Azure expressroute bağlantı hattına bağlı olması gerekir.
- Azure için şirket içi bağlamak, genellikle bir expressroute bağlantı hattı 1 GB/sn veya daha yüksek bant genişliği olması gerekir. Bu en düşük bant genişliği, şirket içi sistemleri ve Azure sanal makinelerini (yanı sıra Azure sistemlerinde bağlantı son kullanıcılar şirket içi) üzerinde çalışan sistemleri arasında veri aktarmak için yeterli bant genişliği sağlar.
- Azure'da tüm SAP sistemlerini birbirleri ile iletişim kurmak için Azure sanal ağlar ayarlanması gerekir.
- Active Directory ve DNS barındırılan şirket içi Azure ExpressRoute aracılığıyla uygulamasına genişletilmiş.


> [!NOTE] 
> Açısından bir fatura, yalnızca tek bir Azure aboneliği yalnızca tek bir kiracı büyük örneği damgasında belirli bir Azure bölgesindeki bağlanabilir ve diğer yandan tek bir büyük örneği damga Kiracı yalnızca bir Azure aboneliğine bağlı olabilir. Bu olgu Azure diğer Faturalanabilir nesneleri farklı değil

SAP HANA birden çok farklı Azure bölgelerinde (büyük örnekler) azure'da dağıtma, büyük örnek damgaya dağıtılması için ayrı bir kiracı sonuçlanır. Ancak, bu örnekler aynı SAP yatay bir parçası olduğu sürece, aynı Azure aboneliği altında her ikisi de çalıştırabilirsiniz. 

> [!IMPORTANT] 
> Yalnızca Azure kaynak yönetimi dağıtımı, SAP HANA azure'da (büyük örnekler) ile desteklenir.

 

### <a name="additional-azure-vnet-information"></a>Ek Azure VNet bilgi

ExpressRoute için bir Azure sanal bağlanmak için bir Azure ağ geçidi oluşturulmalıdır (bkz [ExpressRoute için sanal ağ geçitleri hakkında](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Azure bir ağ geçidi veya ExpressRoute Azure dışında bir altyapı (veya bir Azure büyük örneği damgası) ile Azure Vnet arasında bağlanmak için kullanılır (bkz [PowerShellkullanarakkaynakyöneticisiiçinVNet-VNetbağlantıyapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Bu bağlantılar farklı MS Kurumsal kenarları (MSEE) yönlendiricilerden gelen sürece en fazla dört farklı ExpressRoute bağlantıları Azure ağ geçidine bağlanabilir.  Daha fazla bilgi için bkz: [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Her ikisi de kullanım için Azure bir ağ geçidi sağlar verimlilik farklıdır (bkz [VPN Gateway hakkında](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Bir sanal ağ geçidi ile elde edebilirsiniz en yüksek verimlilik 10 GB/sn bir ExpressRoute bağlantı kullanarak, ' dir. Göz önünde bulundurmanız Azure VNet ve bir sistemde bulunan bir Azure VM arasında dosya kopyalama (tek bir kopya akış olarak) şirket içi, farklı ağ geçidi SKU'ları tam verimini elde değil. Sanal ağ geçidinin tam bant genişliği yararlanmak için birden çok akış kullanın veya tek bir dosyayı paralel akış farklı dosyaları kopyalayın.


### <a name="networking-architecture-for-hana-large-instances"></a>HANA büyük örnekleri için ağ mimarisi
Ağ mimarisi HANA büyük örnekleri için aşağıda gösterildiği gibi dört farklı bölümlerinde ayrılabilir:

- Şirket içi ağ ve Azure ExpressRoute bağlantısı. Bu bölümü müşteriler etki alanıdır ve Azure ExpressRoute üzerinden bağlanır. Bu grafik sağ alt bölümüdür.
- Azure ağı olarak kısaca yukarıda yeniden ağ geçidine sahip Azure Vnet ile açıklanan. Bu, burada uygulamaları gereksinimleri, güvenlik ve uyumluluk gereksinimlerine uygun tasarımları bulmanız gereken bir alandır. HANA büyük örnekleri kullanılarak başka bir sanal ağlar ve Azure ağ geçidi SKU'ları aralarından seçim yapabileceğiniz sayısı bakımından göz önünde bulundurarak noktasıdır. Bu grafik sağ üst bölümüdür.
- Azure içine ExpressRoute teknolojisi aracılığıyla bağlantı HANA büyük örnekleri. Bu bölümü dağıtılan ve Microsoft tarafından işlenir. Tüm yapmanız gereken bazı IP sağlamak için bir müşteri olarak adresi aralıkları ve varlıklarınızı durumlarda HANA büyük ExpressRoute bağlanma dağıtımdan sonra Azure VNet(s) devre (bkz [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- HANA büyük durumlarda ağ, hangi çoğunlukla bir müşteri olarak sizin için saydamdır.

![Azure sanal ağı Azure (büyük örnekler) ve şirket içi SAP HANA bağlı](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

HANA büyük örnekleri kullanın olgu Azure ExpressRoute üzerinden bağlı şirket içi varlıklarınızı almak için gereksinim değiştirmez. Ayrıca hangi ana bilgisayarın HANA örnekleri bağlayan uygulama katmanı barındırılan Azure Vm'leri HANA büyük örneği birimlerinde çalıştırmak bir veya birden çok sanal ağlara sahip gereksinimi değiştirmez. 

SAP dağıtımları saf azure'da fark bulguları ile gelir:

- Müşteri kiracınızın HANA büyük örneği birimleri, Azure VNet(s) başka bir expressroute bağlantı hattı bağlanır. İş yükü koşullarının ayırmak için şirket içi Azure sanal ağlar ExpressRoute bağlantıları ve Azure sanal ağlar ve HANA büyük örnekleri arasında bağlantılar paylaşmıyor aynı yönlendiriciler.
- İş yükü SAP uygulama katmanı HANA örneği arasında çok sayıda küçük istekleri farklı yapısını profilidir ve veri bloğu veri gibi (sonuç kümeleri) SAP HANA uygulama katmana aktarır.
- SAP uygulama mimarisi için ağ gecikmesi daha tipik senaryolar, verileri şirket içi ve Azure arasında alınıp daha duyarlıdır.
- VNet ağ geçidi en az iki ExpressRoute bağlantısı olan ve sanal ağ geçidinin gelen veriler için en yüksek bant genişliği hem bağlantılarını paylaşın.

Ağ gecikmesi Azure Vm'leri ve HANA büyük örneği birimleri arasında deneyimli bir tipik VM VM ağ gidiş dönüş gecikmesi yüksek olabilir. Azure bölgesinde bağımlı, ölçülen değerleri aşağıda ortalama olarak sınıflandırılmış 0.7 ms gidiş dönüş gecikme da aşabilir [SAP Not #1100926 - SSS: ağ performansı](https://launchpad.support.sap.com/#/notes/1100926/E). Yine de müşteriler SAP HANA tabanlı üretim SAP uygulamaları SAP HANA büyük örnekleri üzerinde çok başarıyla dağıtıldı. Dağıtan müşteriler, SAP HANA HANA büyük örneği birimlerini kullanarak SAP uygulamalarını çalıştırarak harika geliştirmeleri bildirdi. Yine de İş süreçlerinizi Azure HANA büyük durumlarda sınamanız gerekir.
 
Azure VM'ler HANA büyük örneği arasında belirleyici ağ gecikmesi sağlamak için Azure VNet ağ geçidi SKU'su seçimi gereklidir. Şirket içi ve Azure sanal makineleri arasındaki trafik düzenlerini, küçük, ancak yüksek ani iletilecek istekleri ve veri birimleri, Azure Vm'leri ve HANA büyük örnekleri arasında trafiği düzeni geliştirebilirsiniz. İyi işlenmiş gibi WINS'e olması için UltraPerformance ağ geçidi SKU'su kullanımını öneririz. HANA büyük örneği SKU'ları türü II sınıfı için UltraPerformance ağ geçidi SKU'su Azure VNet ağ geçidi olarak kullanımını zorunludur.  

> [!IMPORTANT] 
> SAP uygulama ve veritabanı katmanları arasındaki genel ağ trafiğini göz önüne alındığında, yalnızca HighPerformance veya sanal ağlar için UltraPerformance ağ geçidi SKU'ları desteklenir (büyük örnekler) azure'da SAP HANA bağlanmak için. Tür II SKU'ları HANA büyük örneği için yalnızca UltraPerformance ağ geçidi SKU'su Azure VNet ağ geçidi olarak desteklenir.



### <a name="single-sap-system"></a>Tek SAP sistem

Yukarıda gösterilen şirket içi altyapı ExpressRoute aracılığıyla Azure'da bağlı olduğundan ve bir Microsoft Enterprise sınır yönlendiricisi (MSEE) içine expressroute bağlantı hattı bağlanan (bkz [ExpressRoute teknik genel bakış](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Sonra kurulan, bu rota Microsoft Azure omurga bağlanır ve tüm Azure bölgeleri erişilebilir.

> [!NOTE] 
> SAP Windows'un Azure'da çalışan için SAP yatay Azure bölgesinde en yakın MSEE bağlayın. Azure büyük örneği Damgalar Azure Iaas ve büyük örnek Damgalar Azure Vm'lerde arasındaki ağ gecikmesini en aza indirmek için ayrılmış MSEE cihazları üzerinden bağlanır.

SAP uygulama örnekleri, barındırma Azure VM'ler için sanal ağ geçidi, expressroute bağlantı hattına bağlıysa ve aynı Vnet'i büyük örneği damgaları bağlanmakta ayrılmış ayrı bir MSEE yönlendiriciye bağlanır.

Bu, burada SAP uygulama katmanı Azure üzerinde barındırılan ve SAP HANA veritabanına SAP HANA azure'da (büyük örnekler) üzerinde çalışan tek bir SAP sisteminin basit bir örnektir. Sanal ağ geçidi bant genişliği 2 GB/sn veya 10 GB/sn üretilen iş tıkanıklık göstermiyor varsayılır.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Birden çok SAP sistemleri veya büyük SAP sistemleri

SAP HANA Azure (büyük örnekler), onu &#39; bağlanma birden çok SAP sistemleri veya büyük SAP sistemleri dağıtılıyorsa s VNet ağ geçidi verimini varsaymak makul bir ayak bağı. Böyle bir durumda, uygulama katmanları birden fazla Azure Vnet bölmeniz gerekir. Ayrıca gibi durumlarda HANA büyük örneklerine bağlanan özel sanal ağlar oluşturmak için recommendable olabilir:

- NFS paylaşımlarını barındıran Azure VM doğrudan HANA büyük durumlarda HANA örneklerinden yedeklemeleri gerçekleştirmek
- Büyük yedekleri ya da diğer dosyaları HANA büyük örneği birimlerindeki disk alanı Azure'da yönetilen kopyalama.

Konak depolama yönetmek VM'ler büyük dosya etkisi önler veya veri HANA büyük örneklerden Azure'a SAP uygulama katmanı çalıştıran VM'ler görevi gören sanal ağ geçidinde aktarımı ayrı Vnet'ler kullanıyor. 

Daha fazla ölçeklenebilir bir ağ mimarisi için:

- Tek, daha büyük bir SAP uygulama katmanı için birden fazla Azure Vnet yararlanın.
- Dağıtılan, her bir SAP sistemi için ayrı bir Azure sanal ağ aynı Vnet'i altında ayrı alt ağlar bu SAP sistemler birleştirmek için karşılaştırıldığında dağıtın.

 SAP HANA azure'da (büyük örnekler) için daha fazla ölçeklenebilir bir ağ mimarisi:

![SAP uygulama katmanı üzerinde birden fazla Azure Vnet dağıtma](./media/hana-overview-architecture/image4-networking-architecture.png)

Yukarıda gösterildiği gibi birden fazla Azure Vnet üzerinde SAP uygulama katmanı veya bileşenleri, dağıtma, bu Azure Vnet barındırılan uygulamalar arasında iletişim sırasında oluşan kaçınılmaz gecikme yükü kullanıma sunuldu. Varsayılan olarak, bu yapılandırmada MSEE yönlendiriciler farklı sanal ağlar yönlendir Azure VM'ler arasındaki ağ trafiğini yer. Ancak, bu yönlendirme Eylül 2016 itibaren iyileştirilebilir. En iyi duruma getirme ve iki sanal ağlar arasındaki iletişim gecikmesi aşağı Kes eşleme Azure sanal ağlar aynı bölge içinde tarafından yoludur. Bu sanal ağlar farklı Aboneliklerde olsa bile. Azure VNet eşlemesi kullanarak iki farklı Azure vnet'lerdeki sanal makineleri arasındaki iletişimi, birbirleriyle doğrudan iletişim kurmak için Azure ağı omurga kullanabilirsiniz. Böylece sanal makineleri aynı Vnet'i olacaksa gibi benzer gecikme süresine gösteriliyor. Oysa trafik, Azure sanal ağ geçidi üzerinden bağlı IP adresi aralıklarını adresleme VNet tek tek sanal ağ geçidinden yönlendirilir. Makaleyi eşliği Azure VNet ayrıntılarını alabilirsiniz [VNet eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Azure'da yönlendirme

SAP HANA azure'da (büyük örnekler) için üç önemli ağ yönlendirme noktalar vardır:

1. SAP HANA Azure (büyük örnekler) üzerinde yalnızca adanmış ExpressRoute bağlantı ve Azure Vm'leri aracılığıyla erişilebilir; doğrudan şirket içinden. Şirket içinden HANA büyük örneği birimlerine doğrudan erişim, Microsoft tarafından size sunulan gibi SAP HANA büyük örnekleri için kullanılan geçerli Azure ağ mimarisinin geçişli yönlendirme kısıtlamaları nedeniyle hemen mümkün değildir. Bazı yönetim istemcileri ve SAP çözüm şirket içi, çalışan Yöneticisi gibi doğrudan erişim gerektiren herhangi bir uygulama SAP HANA veritabanına bağlanamıyor.

2. İki farklı Azure bölgelerinde olağanüstü durum kurtarma amacıyla dağıtılan HANA büyük örneği birim varsa, aynı geçici yönlendirme kısıtlamaları geçerli olur. Veya diğer bir deyişle, IP adresleri (örneğin BİZE Batı) bir bölgede HANA büyük örneği biriminin (örneğin BİZE Doğu) başka bir bölgede dağıttığınız HANA büyük örneği birimine yönlendirilmez. Bu bölgeler arasında veya HANA büyük örneği birimleri Azure sanal ağlara bağlanma ExpressRoute bağlantı hatları bağlanma çapraz Azure ağ eşlemesi kullanım bağımsızdır. Gösterildiği biraz daha aşağı bu belgeleri. Dağıtılan mimarisiyle gelir, bu kısıtlama, olağanüstü durum kurtarma işlevi HANA sistem çoğaltma hemen kullanımı engelleyecek.

3. SAP HANA Azure (büyük örnekler) birimlerine sahip gönderilen müşteri olarak aralık atanan IP adresini sunucu IP havuzu adresinden (bkz [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ayrıntılar için).  Bu IP adresi, Azure abonelikleri ve Azure (büyük örnekler) üzerinde HANA Azure Vnet bağlandığı ExpressRoute aracılığıyla erişilebilir. Bu sunucu IP adresi aralığı donanım birimine doğrudan atanır ve NAT'ed artık bu değil havuzu dışında atanan IP adresi bu çözüm ilk dağıtımları durumda oluştu. 

> [!NOTE] 
> İlk iki liste öğeleri yukarıda açıklandığı gibi geçici akışındaki kısıtlama üstesinden gerekiyorsa, yönlendirme için ek bileşenleri kullanmak gerekir. Kısıtlama üstesinden gelmek için kullanılabilir bileşenleri olabilir: rota verilerini, gelen ve giden için ters proxy. Örneğin, F5 BIG-IP, NGINX ile trafik, Yöneticisi Azure sanal güvenlik duvarı/trafik yönlendirme çözümü olarak dağıtılabilir.
> Kullanarak [IPTables kuralları](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) HANA büyük örneği birimleri farklı bölgelerde veya şirket içi konumlar ve HANA büyük örneği birimleri arasında yönlendirme etkinleştirmek için bir Linux VM.
> Uygulama ve üçüncü taraf ağ uygulamaları veya IPTables içeren özel çözümler için destek değil, Microsoft tarafından sağlanır unutmayın. Destek kullanılan bileşenin satıcısına veya Tümleştirici tarafından sağlanmalıdır. 

### <a name="internet-connectivity-of-hana-large-instances"></a>Internet bağlantısı HANA büyük örnekleri
HANA büyük örnekleri doğrudan internet bağlantısı yok. Bu örneğin işletim sistemi görüntüsü doğrudan işletim sistemi satıcı ile kaydetmek için yeteneklerini kısıtlıyor. Bu nedenle yerel SLES SMT sunucu veya RHEL Abonelik Yöneticisi ile çalışması gerekebilir

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Azure Vm'leri ve HANA büyük örnekleri arasında veri şifrelemesi
Azure VM'ler HANA büyük örnekleri arasında aktarılan veriler şifrelenmez. Ancak, exchange HANA DBMS yan arasındaki JDBC/ODBC tabanlı uygulamalar için tamamen trafik şifrelemeyi etkinleştirebilirsiniz. Başvuru [SAP tarafından bu belgeleri](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Birden çok bölgede HANA büyük örneği birimlerini kullanma

SAP HANA (büyük örnekler) azure'da dağıtmak için başka bir nedenle olağanüstü durum kurtarma yanı sıra birden fazla Azure bölgesine olabilir. Belki de her bölgelerde farklı vnet'lerdeki dağıtılan VM'ler HANA büyük örneklere erişmek istediğiniz. Farklı HANA büyük örnekleri birimleri atanan IP adreslerini (yani örnekleri kendi ağ geçidi üzerinden doğrudan bağlıdır) Azure Vnet ötesinde yayılmaz olduğundan, yukarıda sunulan VNet tasarım küçük bir değişiklik yoktur: Azure sanal ağ ağ geçidi farklı Msee'ler dışında farklı dört ExpressRoute bağlantı hatları işleyebilir ve büyük örneği Damgalar birine bağlı her bir Vnet'teki başka bir Azure bölgesindeki büyük örneği damga bağlanabilir.

![Farklı Azure bölgelerinde Azure büyük örneği Damgalar bağlı azure sanal ağlar](./media/hana-overview-architecture/image8-multiple-regions.png)

Yukarıdaki şekil hem Azure bölgelerinde (büyük örnekler) azure'da SAP HANA bağlanmak için kullanılan iki farklı ExpressRoute bağlantı hatları için her iki bölgelerde farklı Azure sanal ağlar nasıl bağlandığını gösterir. Dikdörtgen kırmızı satırları yeni sunulan bağlantılardır. Azure sanal ağlar dışında bu bağlantılar ile bu sanal ağlar birini çalıştıran VM'ler her iki bölgelerde dağıtılan farklı HANA büyük örnekleri birimlerinin erişebilir. Yukarıdaki grafik gördüğünüz gibi şirket içi iki ExpressRoute bağlantıları iki Azure bölgeleri sahip olduğunuz varsayılır; Olağanüstü durum kurtarma nedenlerle önerilir.

> [!IMPORTANT] 
> Birden çok ExpressRoute bağlantı hatları kullandıysanız, uygun trafik yönlendirme emin olmak için AS yolu eklenmesini ve yerel tercih BGP ayarları kullanılmalıdır.


