---
title: Genel bakış ve SAP HANA azure'da (büyük örnekler) mimarisini | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da dağıtmak nasıl mimari genel bakış.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e3342f3057917202d81359a27accf47ba288b128
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>SAP HANA (büyük örnekler) genel bakış ve Azure üzerinde mimarisi

## <a name="what-is-sap-hana-on-azure-large-instances"></a>SAP HANA azure'da (büyük örnekler) nedir?

SAP HANA azure'da (büyük örnekler), Azure için benzersiz bir çözümdür. Sanal makineleri dağıtmak ve SAP HANA çalıştırmak için sağlamanın yanı sıra, Azure çalıştırın ve için ayrılmış tam sunuculardaki SAP HANA dağıtma olanağı sunar. SAP HANA Azure (büyük örnekler) çözüm üzerinde size atanan konak/sunucu paylaşılmayan tam donanım inşa edilmiştir. Sunucu donanımı işlem/sunucu, ağ ve depolama altyapısı içeren büyük damga olarak katıştırılır. , Bu sertifikalı uyarlanmış HANA veri merkezi tümleştirmesi (TDI) birleşimidir. SAP HANA Azure (büyük örnekler) üzerinde farklı sunucu SKU'ları veya boyutları sunar. Birimleri, 72 CPU ve bellek 768 GB sahip ve CPU ve bellek 20 TB'ye 960 olan birimleri için artar.

Altyapı damga içindeki müşteri yalıtımı, kiracılar olduğu gibi görünüyor gerçekleştirilir:

- **Ağ**: yalıtım müşterilerin müşteri atanan Kiracı başına sanal ağlar altyapı yığınından içinde. Bir kiracı için tek bir müşteri atanır. Bir müşteri, birden çok kiracıya sahip olabilir. Kiracılar aynı müşteriye ait olsa bile ağ yalıtımı kiracıların kiracılar altyapı damga düzeyinde arasındaki ağ iletişimi engelliyor.
- **Depolama bileşenleri**: atanmış depolama birimleri depolama sanal makinelerde yalıtımı. Depolama birimleri, yalnızca bir depolama sanal makineye atanabilir. Bir depolama alanı sanal makine yalnızca tek bir kiracı SAP HANA TDI sertifikalı altyapı yığınında atanır. Sonuç olarak, bir özel ve ilgili Kiracı içinde yalnızca bir depolama alanı sanal makineye atanan depolama birimleri erişilebilir. Bunlar, farklı dağıtılan kiracılar arasında görünmez.
- **Sunucu veya ana bilgisayar**: bir sunucu veya konak birimi müşteriler veya kiracılar arasında paylaşılmaz. Bir sunucu veya bir müşteri'ye dağıtılan konak tek bir kiracı için atanan bir atomik tam işlem birimi değil. *Hayır* bölümleme donanım veya yazılım bölümleme kullanılırsa, neden olabilir, başka bir müşteri ile bir ana bilgisayar veya bir sunucu paylaşımı. Belirli Kiracı depolama sanal makineye atanan depolama birimleri, bu tür bir sunucuya bağlanır. Bir kiracı farklı SKU'ları özel olarak atanan sunucu sayısı birine sahip olabilir.
- Azure (büyük örnekler) altyapısı damga üzerinde bir SAP HANA içinde birçok farklı kiracıların dağıtılır ve diğer karşı ağ, depolama ve işlem düzeyi Kiracı kavramları aracılığıyla yalıtılmış. 


Bu tam sunucu birimleri, SAP HANA yalnızca çalıştırmak için desteklenir. SAP uygulama katmanı veya iş yükü donanımlar orta katman sanal makinelerde çalışır. SAP HANA Azure (büyük örnekler) birimlerde çalıştırmak altyapı Damgalar için Azure Ağ Hizmetleri omurgalarında bağlanır. Bu şekilde, Azure (büyük örnekler) birimlerdeki SAP HANA ve sanal makineler arasında düşük gecikme süreli bağlantısı sağlanır.

Bu belge, Azure (büyük örnekler) üzerinde SAP HANA kapak birkaç belge biridir. Bu belge, temel mimari, sorumlulukları ve çözümü tarafından sağlanan hizmetleri sunar. Çözüme üst düzey yeteneklerini de ele alınmıştır. Ağ ve bağlantısı gibi birçok diğer alanlarda, dört belge ayrıntıya bilgileri kapsar. SAP HANA Azure (büyük örnekler) ile ilgili belgelere SAP NetWeaver yükleme yönlerini veya SAP NetWeaver VM'ler içinde dağıtımları kapsamaz. SAP NetWeaver Azure üzerinde aynı Azure belgelerine kapsayıcıda bulunan ayrı belgelerinde ele alınmıştır. 


Farklı belgeler HANA büyük örneği kılavuzunun aşağıdaki alanlarda kapsar:

- [SAP HANA (büyük örnekler) genel bakış ve Azure üzerinde mimarisi](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) sorun giderme ve Azure üzerinde izleme](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [STONITH kullanarak yüksek kullanılabilirlik SUSE içinde ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [İşletim sistemi yedekleme ve geri yükleme türü II SKU'ları için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Tanımlar

Birkaç ortak tanımları mimari ve teknik dağıtım kılavuzu yaygın olarak kullanılır. Aşağıdaki terimler ve anlamlarını dikkat edin:

- **Iaas**: hizmet olarak altyapı.
- **PaaS**: hizmet olarak Platform.
- **SaaS**: hizmet olarak yazılım.
- **SAP bileşen**: ERP merkezi bileşeni (ECC), iş ambar (BW), çözüm Yöneticisi veya Enterprise Portal (EP) gibi tek bir SAP uygulamayı. SAP bileşenleri geleneksel ABAP veya Java teknolojiler ya da olmayan-tabanlı NetWeaver uygulama iş nesneleri gibi temel alabilir.
- **SAP ortamı**: SAP bileşenlerden bir veya daha fazla mantıksal olarak gruplandırılmış geliştirme, kalite, eğitim, olağanüstü durum kurtarma veya üretim gibi bir iş işlevi gerçekleştirmek için.
- **SAP yatay**: BT yatay tüm SAP varlıkları başvuruyor. SAP yatay tüm üretim ve üretim dışı ortamlar içerir.
- **SAP sistem**: DBMS katman ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sistemi, bir SAP BW test sistemini ve SAP CRM üretim sistem birleşimi. Azure dağıtımları, bu iki şirket içi ve Azure arasında bölünen desteklemez. Bir SAP dağıtılan şirket içi veya, Azure üzerinde dağıtılan sistemidir. Azure veya şirket içi SAP yatay farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtın ve SAP CRM üretim sistem şirket içi dağıtırken sistemleri Azure'da test edin. Azure'da (büyük örnekler) SAP HANA için VM'ler SAP sistemlerinde SAP uygulama katmanı ve Azure (büyük örnekler) damgasında SAP HANA biriminde ilgili SAP HANA örneğinde ana yöneliktir.
- **Büyük örneği damga**: SAP HANA TDI onaylı ve Azure içindeki SAP HANA örnekleri çalıştırmak için adanmış bir donanım altyapı yığını.
- **SAP HANA azure'da (büyük örnekler):** büyük örneği Damgalar farklı Azure bölgelerinde dağıtılan SAP HANA TDI onaylı donanıma HANA çalıştırmak Azure teklifin resmi adı örnekler içinde. İlgili terim *HANA büyük örneği* kısaltması olan *azure'da (büyük örnekler) SAP HANA* ve bu teknik Dağıtım Kılavuzu'nda yaygın olarak kullanılır.
- **Şirket içi**: Burada VM'ler dağıtıldığı siteden siteye, çok siteli veya Azure ile şirket içi veri merkezleri arasında Azure ExpressRoute bağlantısı bulunan bir Azure aboneliği için bir senaryo açıklanmaktadır. Ortak Azure belgelerine dağıtımları bu tür şirket içi senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Azure Active Directory/OpenLDAP ve şirket içi DNS Azure'da genişletmek için nedenidir. Şirket içi yatay Azure abonelikleri Azure varlıklar için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. 

   Şirket içi etki alanının etki alanı kullanıcıları sunuculara erişmek ve bu vm'lerde (örneğin, DBMS Hizmetleri) Hizmetleri çalıştırın. Şirket içi iletişim ve ad çözümleme VM'ler arasında dağıtılır ve Azure tarafından dağıtılan VM'ler mümkündür. Bu senaryo, çoğu SAP varlıklar dağıtılmış sanal yolu normaldir. Daha fazla bilgi için bkz: [Plan ve tasarım Azure VPN ağ geçidi için](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [Azure portalını kullanarak bir sanal ağ ile bir siteden siteye bağlantı oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Kiracı**: HANA büyük örneği damga içinde dağıtılan bir müşteri içine yalıtılmış bir *Kiracı.* Bir kiracı ağı, depolama ve bilgi işlem katmanını diğer kiracıdan yalıtılmış. Farklı kiracıların atanan depolama ve işlem birimleri birbirine bakın veya HANA büyük örneği damga düzeyinde birbirleriyle iletişim. Müşteri dağıtımları farklı kiracıların uygulamasına sahip olmayı seçebilirsiniz. Daha sonra HANA büyük örneği damga düzeyinde kiracılar arasında iletişim yoktur.
- **SKU kategori**: HANA büyük örneği için aşağıdaki iki kategoriden SKU sunulur:
    - **I sınıf türü**: S72, S72m, S144, S144m, S192 ve S192m
    - **Tür II sınıfı**: S384, S384m, S384xm, S576m, S768m ve S960m


Ek kaynaklar çeşitli bulutta bir SAP iş yükü dağıtmak nasıl kullanılabilir. SAP HANA dağıtımını Azure'da yürütme planlıyorsanız, deneyimli ile ve Azure Iaas ilkeleri ve Azure Iaas iş yükünü SAP dağıtımının farkında olmanız gerekir. Devam etmeden önce bkz [kullanım SAP çözümleri Azure sanal makinelerinde](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) daha fazla bilgi için. 

## <a name="certification"></a>Sertifika

SAP NetWeaver sertifika yanı sıra Azure Iaas gibi belirli altyapıları kullanan, SAP HANA desteklemek SAP HANA için özel bir sertifika gerektirir.

SAP NetWeaver ve bir derece SAP HANA sertifika için Not çekirdeği [SAP Not #1928533 – Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533).

[SAP Not #2316233 - Microsoft azure'da (büyük örnekler) SAP HANA](https://launchpad.support.sap.com/#/notes/2316233/E) de önemlidir. Bu kılavuzda açıklanan çözüm kapsar. Ayrıca, SAP HANA Azure GS5 VM türünü çalıştırmak için desteklenir. Bu durumda bilgi sitesinde yayımlanır [SAP Web sitesi](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

SAP HANA SAP Not #2316233 başvurulan Azure (büyük örnekler) çözüm üzerinde Microsoft ve SAP müşteriler büyük SAP Business Suite, SAP BW, S/4 HANA, BW/4HANA veya diğer SAP HANA iş yüklerini azure'a dağıtma yeteneği sağlar. SAP HANA sertifikalı ayrılmış donanım damgası tabanlı çözümün ([SAP HANA hazırlanmış bir veri merkezi tümleştirmesi – TDI](https://scn.sap.com/docs/DOC-63140)). SAP HANA TDI yapılandırılmış çözümünü çalıştırırsanız, tüm SAP HANA tabanlı uygulamalar (örneğin, SAP HANA üzerinde SAP Business Suite, SAP HANA, S4/HANA ve BW4/HANA SAP BW) donanım altyapısı üzerinde çalışır.

SAP HANA Vm'lerde çalışan ile karşılaştırıldığında, bu çözümü bir avantajı vardır. Çok büyük bellek birimleri için sağlar. Bu çözüm etkinleştirmek için aşağıdaki unsur anlamanız gerekir:

- SAP uygulama katmanı ve SAP olmayan uygulamalar her zamanki Azure donanım Damgalar barındırılan sanal makineleri çalıştırın.
- Müşteri altyapı, veri merkezleri, şirket içi ve uygulama dağıtımları (önerilen) ExpressRoute aracılığıyla bulut platformu veya sanal özel ağ (VPN) bağlı. Ayrıca Active Directory ve DNS Azure'da genişletilmiştir.
- SAP HANA veritabanı örneği HANA iş yükü için SAP HANA Azure (büyük örnekler) üzerinde çalışır. Vm'lerde çalışan yazılımı HANA büyük örneğinde HANA örneği ile etkileşim kurabilmesi büyük örneği damga Azure ağ uygulamasına bağlandı.
- SAP HANA Azure (büyük örnekler) ile ilgili donanım SUSE Linux Enterprise Server veya Red Hat Enterprise Linux önceden yüklenmiş olan bir Iaas sağlanan ayrılmış donanım ' dir. Sanal makineler gibi daha fazla güncelleştirmeleri ve işletim sistemi bakım olduğu sizin sorumluluğunuzdadır.
- HANA veya SAP HANA HANA büyük örneği birimler üzerinde çalıştırmak için gereken diğer ek bileşenleri yüklemesi, sorumluluğundadır. Tüm ilgili devam eden işlemler ve Azure üzerinde SAP HANA yönetimini de sizin sorumluluğunuzdadır.
- Burada açıklanan çözümler yanı sıra, Azure (büyük örnekler) üzerinde SAP HANA bağlandığı Azure aboneliğinizde diğer bileşenleri yükleyebilirsiniz. Örnek ile veya doğrudan SAP HANA iletişimi etkinleştir bileşenleri, atlama sunucuları gibi RDP sunucuları, SAP HANA Studio SAP BI senaryoları için SAP veri Hizmetleri veritabanı veya izleme çözümü ağ verilebilir.
- Azure olduğu gibi HANA büyük örneği yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevi için destek sunar.

## <a name="architecture"></a>Mimari

Yüksek düzeyde, Azure (büyük örnekler) çözüm üzerinde SAP HANA Vm'lerde bulunan SAP uygulama katmanı vardır. Veritabanı katmanı büyük örneği damga Azure Iaas bağlı aynı Azure bölgesinde bulunan SAP TDI yapılandırılmış donanımda bulunur.

> [!NOTE]
> SAP uygulama katmanı aynı Azure bölgesinde SAP DBMS katmanı olarak dağıtın. Bu kural, SAP iş yükleri hakkında yayımlanan bilgi Azure üzerinde de belgelenmiştir. 

SAP HANA Azure (büyük örnekler) ile ilgili genel mimarisi bir sanallaştırılmamış, çıplak metal, SAP HANA veritabanı için yüksek performanslı sunucusu bir SAP TDI sertifikalı donanım yapılandırmasını sağlar. Ayrıca, Azure esneklik için gereksinimlerinizi karşılayacak şekilde SAP uygulama katmanı ölçek kaynakları ve özelliği de sağlar.

![SAP HANA Azure (büyük örnekler) ile ilgili mimari genel bakış](./media/hana-overview-architecture/image1-architecture.png)

Gösterilen mimarisi üç bölümlere ayrılmıştır:

- **Sağ**: son kullanıcılar, SAP gibi iş KOLU uygulamaları erişebilmesi için verileri farklı uygulamaları çalıştıran bir şirket içi altyapı merkezleri gösterir. İdeal olarak, bu altyapı sonra Azure ile bağlı şirket içi [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Merkezi**: Azure Iaas gösterir ve bu durumda, SAP veya SAP HANA DBMS sistem olarak kullanan diğer uygulamalar barındırmak için Vm'leri kullanın. VM'ler sağlamak bellekle işlev küçük HANA örnekleri Vm'lerde kendi uygulama katmanı ile birlikte dağıtılır. Sanal makineler hakkında daha fazla bilgi için bkz: [sanal makineleri](https://azure.microsoft.com/services/virtual-machines/).

   Azure Ağ Hizmetleri SAP sistemleri diğer uygulamalara sanal ağlar ile birlikte gruplandırmak için kullanılır. Şirket içi sistemlere de Azure (büyük örnekler) üzerinde SAP HANA için bu sanal ağlara bağlanabilir.

   SAP NetWeaver uygulamalar ve Azure'da çalışması için desteklenen veritabanları için bkz: [SAP destek Not #1928533 – Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533). Azure üzerinde SAP çözümlerini dağıtma konusunda daha fazla belgeler için bkz:

  -  [Windows sanal makinelerde SAP kullanın](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Azure sanal makinelerde SAP çözümleri kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Sol**: Azure büyük örneği damgaya SAP HANA TDI sertifikalı donanım gösterir. HANA büyük örneği birimleri aboneliğinizi sanal ağlar için şirket içi bağlantı Azure içine olarak aynı teknolojisini kullanarak bağlanır.

Azure büyük örneği damga aşağıdaki bileşenleri birleştirir:

- **Bilgi işlem**: gerekli hesaplama yeteneği sağlamak ve SAP HANA sertifikalı olan Intel Xeon E7-8890v3 veya Intel Xeon E7-8890v4 işlemcilerini esas alan sunucuları.
- **Ağ**: A birleşik bilgi işlem, depolama ve LAN bileşenleri bağlantılar yüksek hızlı ağ yapısı.
- **Depolama**: birleşik ağ yapısı erişilen bir depolama altyapısını. Sağlanan belirli depolama kapasitesi dağıtılan Azure (büyük örnekler) yapılandırması hakkında belirli SAP HANA bağlıdır. Daha fazla depolama kapasitesi, ek bir aylık ücret kullanılabilir.

Büyük örneği damga çok kiracılı altyapısı içinde müşteriler yalıtılmış Kiracı dağıtılır. Kiracı dağıtımı sırasında Azure kaydınızı içinde bir Azure aboneliği adı. Bu Azure aboneliği HANA büyük örneği karşı faturalandırılır adrestir. Bu kiracılar Azure aboneliği 1:1 ilişkisi. Bir ağ için Azure farklı aboneliklere ait farklı sanal ağlardan bir Azure bölgesindeki bir kiracı dağıtılmış bir HANA büyük örneği birim erişim mümkündür. Bu Azure abonelikleri aynı Azure kayıt ait olmalıdır. 

VMs olduğu gibi Azure (büyük örnekler) üzerinde SAP HANA birden çok Azure bölgelerinde sunulur. Olağanüstü durum kurtarma özellikleri sunmak için kabul seçebilirsiniz. Farklı büyük örneği Damgalar bir siyasi coğrafi bölge içinde birbirlerine bağlı. Örneğin, HANA büyük örneği damgaları Batı ABD ve BİZE Doğu, olağanüstü durum kurtarma çoğaltma için ayrılmış bir ağ bağlantısı üzerinden bağlanır. 

Farklı VM türler ile Azure sanal makineler arasında yalnızca seçebileceğiniz gibi farklı SKU'ları HANA büyük, SAP HANA farklı iş yükü türleri için özel olarak hazırlanmış örneğini arasından seçim yapabilirsiniz. SAP bellek için işlemci yuvası oranları üzerinde Intel işlemci nesli göre değişen iş yükleri için geçerlidir. Aşağıdaki tabloda sunulan SKU türleri gösterilmektedir.

Temmuz 2017'ten itibaren SAP HANA Azure (büyük örnekler) ile ilgili çeşitli yapılandırmalarda BİZE Batı ABD Doğu, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa ve Kuzey Avrupa Azure bölgelerde kullanılabilir.

| SAP çözümü | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| OLAP için en iyi duruma getirilmiş: SAP BW, BW/4HANA<br /> veya SAP HANA Genel OLAP iş yükü için | SAP HANA Azure S72 üzerinde<br /> – 2 x Intel® Xeon İşlemci E7 8890 v3<br /> 36 CPU çekirdekleri ve 72 CPU iş parçacıkları |  768 GB |  3 TB | Kullanılabilir |
| --- | SAP HANA Azure S144 üzerinde<br /> -4 x Intel® Xeon İşlemci E7 8890 v3<br /> 72 CPU çekirdekleri ve 144 CPU iş parçacıkları |  1,5 TB |  6 TB | Artık sunulmuyor |
| --- | SAP HANA Azure S192 üzerinde<br /> -4 x Intel® Xeon İşlemci E7 8890 v4<br /> 96 CPU çekirdekleri ve 192 CPU iş parçacıkları |  2.0 TB |  8 TB | Kullanılabilir |
| --- | SAP HANA Azure S384 üzerinde<br /> – 8 x Intel® Xeon İşlemci E7 8890 v4<br /> 192 CPU çekirdekleri ve 384 CPU iş parçacıkları |  4.0 TB |  16 TB | Kullanılabilir |
| OLTP için en iyi duruma getirilmiş: SAP Business paketi<br /> SAP HANA veya S/4HANA (OLTP)<br /> Genel OLTP | SAP HANA Azure S72m üzerinde<br /> – 2 x Intel® Xeon İşlemci E7 8890 v3<br /> 36 CPU çekirdekleri ve 72 CPU iş parçacıkları |  1,5 TB |  6 TB | Kullanılabilir |
|---| SAP HANA Azure S144m üzerinde<br /> -4 x Intel® Xeon İşlemci E7 8890 v3<br /> 72 CPU çekirdekleri ve 144 CPU iş parçacıkları |  3.0 TB |  12 TB | Artık sunulmuyor |
|---| SAP HANA Azure S192m üzerinde<br /> -4 x Intel® Xeon İşlemci E7 8890 v4<br /> 96 CPU çekirdekleri ve 192 CPU iş parçacıkları  |  4.0 TB |  16 TB | Kullanılabilir |
|---| SAP HANA Azure S384m üzerinde<br /> – 8 x Intel® Xeon İşlemci E7 8890 v4<br /> 192 CPU çekirdekleri ve 384 CPU iş parçacıkları |  6.0 TB |  18 TB | Kullanılabilir |
|---| SAP HANA Azure S384xm üzerinde<br /> – 8 x Intel® Xeon İşlemci E7 8890 v4<br /> 192 CPU çekirdekleri ve 384 CPU iş parçacıkları |  8.0 TB |  22 TB |  Kullanılabilir |
|---| SAP HANA Azure S576m üzerinde<br /> – 12 x Intel® Xeon İşlemci E7 8890 v4<br /> 288 CPU çekirdekleri ve 576 CPU iş parçacıkları |  12.0 TB |  28 TB | Kullanılabilir |
|---| SAP HANA Azure S768m üzerinde<br /> – 16 x Intel® Xeon İşlemci E7 8890 v4<br /> 384 CPU çekirdekleri ve 768 CPU iş parçacıkları |  16,0 TB |  36 TB | Kullanılabilir |
|---| SAP HANA Azure S960m üzerinde<br /> – 20 x Intel® Xeon İşlemci E7 8890 v4<br /> 480 CPU çekirdekleri ve 960 CPU iş parçacıkları |  20.0 TB |  46 TB | Kullanılabilir |

- CPU çekirdekleri = olmayan-hiper iş parçacıklı CPU çekirdeği sunucusu birimi işlemcileri toplamı toplamı.
- CPU iş parçacıkları = hiper iş parçacıklı CPU çekirdeği sunucusu birimi işlemcileri toplamı tarafından sağlanan işlem iş parçacıklarının toplamı. Tüm birimlerin varsayılan olarak Hyper-Threading Teknolojisi kullanacak şekilde yapılandırılır.


Seçilen belirli yapılandırmalar, iş yükü, CPU kaynaklarını ve istenen bellek bağımlıdır. OLAP iş yükü için en iyi duruma getirilir SKU'ları kullanmak OLTP iş yükü için mümkündür. 

Tüm teklifleri için temel donanım SAP HANA TDI onaylı. Donanım farklı iki sınıf içine SKU'ları bölün:

- S72, S72m, S144, S144m, S192 ve S192m, "ı sınıf türü olarak" adlandırılan SKU.
- S384, S384m, S384xm, S576m, S768m ve denir S960m "Tür II sınıfı" SKU.

Tam HANA büyük örneği damga yalnızca tek bir müşteri için ayrılan değil&#39;s kullanın. Azure üzerinde de dağıtılan ağ yapısı bağlı işlem ve depolama kaynaklarını raflarının olgunun uygular. Azure gibi HANA büyük örneği altyapı, farklı müşteri dağıtır &quot;kiracılar&quot; birbirlerinden aşağıdaki üç düzeyin yalıtılmış olan:

- **Ağ**: HANA büyük örneği damga içindeki sanal ağlar için yalıtımı.
- **Depolama**: atanan depolama birimleri ve depolama birimleri kiracılar arasında yalıtmak depolama sanal makineler için yalıtımı.
- **İşlem**: sunucu birimlerini atamasının tek bir kiracı için ayrılmış. Hayır sabit veya yumuşak sunucu ölçü bölümleme. Hiçbir tek bir sunucu veya konak birimi kiracılar arasında paylaştırma. 

Farklı kiracılar arasında HANA büyük örneği birimleri dağıtımları birbirlerine görünmez. Farklı kiracılar dağıtılan HANA büyük örneği birimleri diğer HANA büyük örneği damga düzeyi ile doğrudan iletişim kuramaz. Yalnızca bir kiracı içindeki HANA büyük örneği birimleri HANA büyük örneği damga düzeyinde birbirleri ile iletişim kurabilir.

Büyük örneği damgasında dağıtılan bir kiracı fatura amaçlar için bir Azure aboneliğine atanır. Bir ağ için başka Azure abonelikleri aynı Azure kayıt içinde sanal ağlardan erişilebilir. Başka bir Azure aboneliğiniz aynı Azure bölgesinde dağıtırsanız, ayrılmış bir HANA büyük örneği Kiracı için sormak seçebilirsiniz.

SAP HANA HANA büyük örneğinde ve SAP HANA Azure'da dağıtılan VM'ler üzerinde çalışan çalıştıran arasında önemli farklılıklar vardır:

- SAP HANA (büyük örnekler) azure'da yok sanallaştırma katmanı yoktur. Temel alınan tam donanım performansını alın.
- Azure farklı olarak, Azure (büyük örnekler) sunucusunda SAP HANA belirli bir müşteriye ayrılmış. Bir sunucu birim veya ana bilgisayar yazılım veya sabit bölündüğünü hiçbir olasılığı vardır. Sonuç olarak, bir HANA büyük örneği birimi bir bütün olarak bir kiracı ve, size atanan olarak kullanılır. Bir yeniden başlatma veya kapatma sunucunun işletim sistemine ve başka bir sunucu üzerinde dağıtılan SAP HANA otomatik olarak yol değil. (Türü için SKU'ları sınıfı, bir sunucu karşılaştığı sorunları ve yeniden dağıtım gereken başka bir sunucu üzerinde gerçekleştirilecek tek özel durumdur.)
- Burada ana bilgisayar İşlemci türleri için en iyi fiyat/performans oranı seçilir, Azure, SAP HANA azure'da (büyük örnekler) için seçilen işlemci yüksek Intel E7v3 ve E7v4 işlemci satırının gerçekleştirme türleridir.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Bir HANA büyük örneği biriminde birden çok SAP HANA örneklerinde Çalıştır
HANA büyük örneği birimleri birden fazla etkin SAP HANA örneğinde barındırmak mümkündür. Depolama anlık görüntüler ve olağanüstü durum kurtarma özellikleri sağlamak için bu tür bir yapılandırma örneği bir birim gerektirir. Şu anda HANA büyük örneği birimler gibi bölünmüştür:

- **S72, S72m, S144, S192**: 256 GB en küçük başlangıç birim 256 GB artışlarla. 256 GB ve 512 GB gibi farklı aralıklarla birimi belleğinin en birleştirilebilir.
- **S144m ve S192m**: 512 GB en küçük birim 256 GB artışlarla. 512 GB ve 768 GB gibi farklı aralıklarla birimi belleğinin en birleştirilebilir.
- **Tür II sınıfı**: 2 TB'lık en küçük başlangıç birimiyle 512 GB artışlarla. 512 GB, 1 TB ve 1,5 TB gibi farklı aralıklarla birimi belleğinin en birleştirilebilir.

Birden çok SAP HANA örneği çalıştıran bazı örnekler aşağıdaki gibi görünebilir.

| SKU | Bellek boyutu | Depolama boyutu | Birden çok veritabanı boyutlarıyla |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA örneği<br /> ya da 1 x 512 GB örneği + 1 x 256 GB örneği<br /> ya da 3 x 256 GB örnekleri | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA örnekleri<br />ya da 1 x 512 GB örneği + 1 adet 1 TB örneği<br />ya da 6 x 256 GB örnekleri<br />veya 1x1.5 TB örneği | 
| S192m | 4 TB | 16 TB | 8 x 512 GB örnekleri<br />veya 4 x 1 TB örnekleri<br />ya da 4 x 512 GB örnekleri + 2 x 1 TB örnekleri<br />ya da 4 x 768 GB örnekleri + 2 x 512 GB örnekleri<br />ya da 1 x 4 TB örneği |
| S384xm | 8 TB | 22 TB | 4 x 2 TB örnekleri<br />ya da 2 x 4 TB örnekleri<br />ya 2 x 3 TB örnekleri + 1 x 2 TB örnekleri<br />ya da 2x2.5 TB örnekleri + 1 x 3 TB örnekleri<br />ya da 1 x 8 TB örneği |


Diğer Çeşitleme de vardır. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA veri katmanlama ve uzantı düğümlerini kullanın
SAP SAP BW farklı SAP NetWeaver sürümlerini ve SAP BW/4HANA için veri katmanı modelini destekler. SAP belgenin veri katmanlama modeli hakkında daha fazla bilgi için bkz: [SAP BW/4HANA ve SAP BW SAP HANA uzantısı düğümlerle HANA üzerinde](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
HANA büyük örneğiyle SSS ve SAP blog belgelerinde açıklandığı gibi SAP HANA uzantısı düğümünün seçeneği 1 yapılandırmasını kullanabilirsiniz. Seçenek 2 yapılandırmaları ayarlanabilir aşağıdaki HANA büyük örneği SKU'ları ile: S72m, S192, S192m, S384 ve S384m. 

Belgelerine bakın, avantajı hemen görünür olmayabilir. Ancak SAP boyutlandırma yönergeleri baktığınızda seçeneği-1 ve 2 seçeneği SAP HANA uzantısı düğümleri kullanarak bir avantajı görebilirsiniz. Örnekler şunlardır:

- SAP HANA boyutlandırma yönergeleri, genellikle belleği double veri birimi miktarını gerektirir. SAP HANA örneğinizi dinamik veri ile çalıştırdığınızda, yüzde 50'yalnızca sahip veya daha az bellek verilerle doldurulur. Bellek geri kalanı, SAP HANA işini yapmak için ideal olarak tutulur.
- Anlamına HANA büyük örneği S192 biriminde 2 çalıştıran bir SAP BW veritabanı TB bellek, 1 TB veri birimi olarak yeterlidir.
- Seçenek 1 ek bir SAP HANA uzantısı düğümünü kullanırsanız, ayrıca bir S192 HANA büyük örneği SKU, onu bir ek veri birimi 2 TB kapasite sağlar. Seçenek 2 yapılandırmasında sıcak veri birimi için ek bir 4 TB alın. Etkin düğüme ile karşılaştırıldığında, "sıcak" uzantısı düğümünde tam bellek kapasitesini seçeneği-1 için verileri depolamak için kullanılabilir. Double bellek, veri birimi seçeneği 2 SAP HANA uzantısı düğüm yapılandırması için kullanılabilir.
- 3 TB kapasite ile verilerinizi ve 1:2 seçenek 1 için yarı ile hot oranını sonlanır. Veri ve 1:4 oranını seçeneği 2 uzantı düğüm yapılandırması ile 5 TB var.

Disk depolaması için istiyoruz sıcak veri depolanır yüksek olasılığını belleği ile karşılaştırıldığında daha yüksek veri birimi var.


## <a name="operations-model-and-responsibilities"></a>İşlem modeli ve sorumlulukları

SAP HANA azure'da (büyük örnekler) ile sağlanan hizmet Azure Iaas Hizmetleri ile hizalanır. SAP HANA için en iyi duruma getirilmiş yüklü bir işletim sistemi HANA büyük örneğiyle örneğini alır. Azure Iaas VM'ler ile OS sağlamlaştırma, ek yazılım yükleme, HANA yükleme, işletim sistemi ve HANA ve işletim sistemi ve HANA güncelleştirme görevlerin çoğunu olduğundan sizin sorumluluğunuzdadır. Microsoft işletim sistemi güncelleştirmeleri veya HANA güncelleştirmeleri üzerinde zorlayabilir değil.

![SAP HANA (büyük örnekler) azure'da sorumlulukları](./media/hana-overview-architecture/image2-responsibilities.png)

Aşağıdaki çizimde gösterildiği gibi Azure (büyük örnekler) üzerinde SAP HANA Iaas sunan bir çok kiracılı ' dir. Çoğunlukla, sorumluluk bölümünün OS altyapı sınırında ' dir. Microsoft, işletim sistemi satırının altında hizmet tüm yönlerini sorumludur. Hizmet satırın yukarısında tüm yönlerini siz sorumlusunuz. İşletim sistemi, sorumluluğundadır. En güncel şirket içi yöntemleri, uygulamadığınız uyumluluk, güvenlik, uygulama yönetimi, temel ve işletim sistemi yönetimi için kullanmaya devam edebilirsiniz. Sistemleri ağınızdaki tüm gelince oldukları gibi görünür.

Bu hizmet, alanları burada en iyi sonuçlar için temel altyapı özellikleri kullanmak için Microsoft ile çalışmak için gereken şekilde SAP HANA için optimize edilmiştir.

Aşağıdaki listede daha fazla ayrıntı her katmanları ve sorumluluklarınızın sağlar:

**Ağ**: tüm iç ağlar için SAP HANA çalışan büyük örneği Damga. Sizin sorumluluğunuzdadır depolama, örnekleri (için genişleme ve diğer işlevleri), yatay bağlanma ve bağlantıyı SAP uygulama katmanı barındırıldığı Azure arasında bağlantı erişim Vm'lerde içerir. Ayrıca, olağanüstü durum kurtarma amacıyla çoğaltma için Azure veri merkezleri arasında geniş ağ bağlantısı içerir. Tüm ağları Kiracı tarafından bölümlenmiş ve uygulanan hizmet kalitesi vardır.

**Depolama**: sanallaştırılmış bölümlenmiş anlık görüntü yanı sıra, SAP HANA sunucuları tarafından gerekli tüm birimler için depolama. 

**Sunucuları**: kiracılar için atanan SAP HANA DBs çalıştırmak için adanmış fiziksel sunucular. T türü sunucuları sınıf soyutlanır donanım SKU'lar. Sunucular bu tür ile sunucu yapılandırmasını toplanır ve bir fiziksel donanım için başka bir fiziksel donanım taşınabilir profilleri tutulur. Bu tür bir (el ile) taşıma profil işlemler Azure hizmet onarma için biraz karşılaştırılabilir. Böyle bir özellik türü II sınıfı SKU'ları sunucuları sunmamaktadır.

**SDDC**: yazılım tanımlı varlıklar verileri yönetmek için kullanılan yönetim yazılımı merkezleri. Microsoft, Ölçek, kullanılabilirlik ve performansı artırmak için havuz kaynakları olanak sağlar.

**İşletim sistemi**: (SUSE Linux veya Red Hat Linux) seçtiğiniz işletim sistemi çalıştıran sunucularda. İle birlikte verilen işletim sistemi görüntüleri SAP HANA çalıştırmak için Microsoft'a tek tek Linux satıcısı tarafından sağlanan. Linux satıcısı belirli SAP HANA iyileştirilmiş görüntüsü için bir aboneliğinizin olması gerekir. İşletim sistemi satıcıyla görüntüleri kaydetmekten. 

Microsoft tarafından handover noktasından herhangi başka Linux işletim sistemi düzeltme eki uygulama için sorumluluğu size aittir. Bu düzeltme eki uygulama, olabilir başarılı bir SAP HANA yüklemesi için gerekli ve bunların SAP HANA belirli Linux satıcı tarafından dahil doğru ek paketleri dahil işletim sistemi görüntülerini en iyi duruma getirilmiş. (Daha fazla bilgi için SAP'ın HANA yükleme belgelerini ve SAP notlarına bakın.) 

İşletim sistemi arızası veya işletim sisteminin en iyi duruma getirme ve belirli sunucu donanımı göre sürücülerini owing to düzeltme eki uygulama için sorumlu. Ayrıca güvenlik veya işletim sistemini işlevsel düzeltme eki uygulama için sorumlu değildir. 

Sizin sorumluluğunuzdadır de izleme ve kapasite planlama içerir:

- CPU kaynak kullanımı.
- Bellek tüketimi.
- Disk birimleri boş alan, IOPS ve gecikme süresi ile ilgili.
- HANA büyük örneği ve SAP uygulama katmanı arasındaki ağ birim trafiği.

Altyapının HANA büyük örneğinin yedekleme ve geri yükleme işletim sistemi birimi için işlevsellik sağlar. Bu işlevselliği de sizin sorumluluğunuzdadır kullanmaktır.

**Ara yazılım**: öncelikle SAP HANA örneği. Yönetim, işlemleri ve izleme sizin sorumluluğunuzdadır. Yedekleme ve geri yükleme ve olağanüstü durum kurtarma amacıyla depolama anlık görüntüleri kullanmak için sağlanan işlevselliği kullanabilirsiniz. Bu özellikler altyapısı tarafından sağlanır. Sizin Sorumluluklarınız aynı zamanda yüksek kullanılabilirlik veya depolama anlık görüntüleri başarıyla gerçekleştirilip gerçekleştirilmediğini belirlemek için bunları yararlanan ve izleme bu özellikler, olağanüstü durum kurtarma tasarımı içerir.

**Veri**: SAP HANA tarafından yönetilen, verilerinizi ve yedekleme gibi diğer verilerin birimlerde bulunan dosya veya dosya paylaşımları. Sizin Sorumluluklarınız boş disk alanı izleme ve birimler üzerinde içeriği yönetme içerir. Ayrıca disk birimleri ve depolama anlık görüntüleri yedeklerini aktarılmadığı izlemekten sorumlu değildir. Olağanüstü durum kurtarma sitelerinde veri çoğaltmasının başarılı yürütme Microsoft sorumluluğundadır.

**Uygulamalar:** SAP uygulama örnekleri veya SAP olmayan uygulamaları, uygulama katmanı söz konusu uygulamaların söz konusu olduğunda. Sizin Sorumluluklarınız dağıtım, yönetim, işlemleri ve bu uygulamaları izleme içerir. CPU kaynak kullanımı, bellek tüketimi, Azure depolama alanı tüketimi ve sanal ağ içindeki ağ bant genişliği tüketimi için kapasite planlaması sorumlu. Ayrıca sanal ağlardan SAP HANA kaynak tüketimi (büyük örnekler) Azure üzerinde planlama kapasite sorumlu değildir.

**WAN**: iş yükleri için Azure dağıtımları için şirket içi belirlediğiniz bağlantıları. Tüm müşteriler HANA büyük örneğiyle Azure ExpressRoute bağlantısı kullanın. Bu bağlantı, Azure (büyük örnekler) çözüm üzerinde SAP HANA bir parçası değil. Bu bağlantı ayarı için sorumlu.

**Arşiv**: depolama hesaplarında kendi yöntemleri kullanarak veri kopyasının arşivlemek tercih edebilirsiniz. Arşivleme yönetimi, uyumluluk, maliyetleri ve işlemler gerektirir. Oluşturma arşiv kopyalar ve Azure ile uyumlu bir şekilde depolamak yedeklemelerin sorumlu.

Bkz: [azure'da (büyük örnekler) SAP HANA SLA](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Boyutlandırma

Boyutlandırma HANA büyük örneğinin HANA için genel boyutlandırma daha farklı değildir. Mevcut ve taşımak diğer RDBMS HANA için SAP sağlayan bir dizi varolan SAP sistemlerinize çalıştırmak rapor istediğiniz sistemleri dağıtılabilir. Veritabanı için HANA taşınırsa, bu raporları verileri denetleyin ve HANA örneği için bellek gereksinimleri hesaplayın. Bu raporları çalıştırmak ve bunların en son düzeltme ekleri veya sürümleri elde hakkında daha fazla bilgi için aşağıdaki SAP notları okuyun:

- [SAP Not #1793345 - HANA üzerinde SAP paketi için boyutlandırma](https://launchpad.support.sap.com/#/notes/1793345)
- [Not #1872170 - Suite HANA ve S/4 HANA boyutlandırma rapor SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP Not #2121330 - SSS: SAP BW rapor boyutlandırma HANA üzerinde](https://launchpad.support.sap.com/#/notes/2121330)
- [Not #1736976 - HANA boyutlandırma rapor BW için SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP HANA üzerinde BW için yeni rapor boyutlandırma #2296290 - Not](https://launchpad.support.sap.com/#/notes/2296290)

Yeşil alan uygulamaları için SAP hızlı Boyutlandırıcı SAP HANA üstünde yazılım uygulamasıdır bellek gereksinimlerini hesaplamak kullanılabilir.

Veri birimi büyüdükçe HANA yönelik bellek gereksinimleri artırın. Ne, gelecekte olacağını tahmin yardımcı olması için geçerli bellek kullanımına dikkat edin. Bellek gereksinimlerine bağlı olarak, ardından, isteğe bağlı HANA büyük örneği SKU birine eşleyebilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu liste, SAP HANA (büyük örnekler) Azure üzerinde çalıştırmak için gereksinimler derler.

**Microsoft Azure**

- SAP HANA azure'da (büyük örnekler) için bağlı bir Azure aboneliği.
- Microsoft Premier Destek sözleşmenizi. SAP Azure'da çalıştırma ile ilgili ayrıntılı bilgi için bkz: [SAP destek Not #2015553 – Microsoft Azure üzerinde SAP: Destek Önkoşullar](https://launchpad.support.sap.com/#/notes/2015553). 384 ve daha fazla CPU HANA büyük örneği birimler kullanırsanız, Premier genişletmek etmeniz destek sözleşmesi Azure hızlı yanıt içerir.
- HANA büyük örneği SAP boyutlandırma alıştırmaya gerçekleştirdikten sonra ihtiyacınız SKU'ları tanıma.

**Ağ bağlantısı**

- Şirket içi Azure arasında ExpressRoute: şirket içi veri Merkezinize Azure'a bağlanmak için en az bir 1 Gbps bağlantı ISS'niz tarafından sipariş emin olun. 

**İşletim Sistemi**

- SUSE Linux Enterprise Server 12 SAP uygulamaları için lisans.

   > [!NOTE] 
   > Microsoft tarafından sunulan işletim sistemi ile SUSE kayıtlı değil. Bir abonelik yönetim aracı örneğine bağlı değil.

- SUSE Linux Abonelik Yönetim Azure VM üzerinde dağıtılmış aracı. Bu araç SAP HANA azure'da (örnekler büyük kayıtlı ve sırasıyla SUSE tarafından güncelleştirilmiş) özelliği sunar. (HANA büyük örnek veri merkezi içinde internet erişimi yoktur.) 
- Red Hat Enterprise Linux 6.7 veya 7.2 SAP HANA için için lisans.

   > [!NOTE]
   > Microsoft tarafından sunulan işletim sistemi ile Red Hat kayıtlı değil. Red Hat Abonelik Yöneticisi örneğine bağlı değil.

- Red Hat Abonelik Yöneticisi Azure VM üzerinde dağıtılabilir. Red Hat Abonelik Yöneticisi SAP HANA azure'da (örnekler büyük kayıtlı ve sırasıyla Red Hat tarafından güncelleştirilmiş) özelliği sunar. (Hiçbir doğrudan internet içinden erişim Azure büyük örneği damgası üzerinde dağıtılan Kiracı yoktur.)
- SAP destek sahip olmanızı gerektirir de Linux sağlayıcınız ile bir sözleşme. Bu gereksinim çözüm HANA büyük örneği ya da Linux Azure'da çalışması olgu tarafından kaldırılmaz. Bazı Linux Azure galeri görüntüleri hizmeti ücret benzemez *değil* HANA büyük örnek çözümü teklifinize dahil. SAP Linux dağıtıcı ile gerekliliklerini destek sözleşmeleri ilgili sizin sorumluluğunuzdadır değil. 
   - SUSE Linux için destek sözleşmeleri gereksinimlerini aramak [SAP Not #1984787 - SUSE Linux Enterprise Server 12: yükleme notları](https://launchpad.support.sap.com/#/notes/1984787) ve [SAP Not #1056161 - SUSE öncelik SAP uygulamalarıdesteği](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux için destek içerir ve hizmet güncelleştirmeleri HANA büyük örneğinin işletim sistemleri için doğru aboneliğin düzeyleri olması gerekiyor. Red Hat önerir Red Hat Enterprise Linux [SAP çözümleri] (https://access.redhat.com/solutions/3082481 abonelik. 

Farklı Linux sürümleri ile farklı SAP HANA sürümleri için destek matrisi bkz [SAP Not #2235581](https://launchpad.support.sap.com/#/notes/2235581).

İçin uyumluluk matrisi HLI bellenim/sürücü sürümleri ve işletim sistemine başvurmak [HLI için işletim sistemi yükseltme](os-upgrade-hana-large-instance.md).


**Veritabanı**

- Lisansları ve SAP HANA (platform veya enterprise edition) için yazılım yükleme bileşenleri.

**Uygulamalar**

- Lisansları ve SAP HANA ve ilgili SAP bağlanmak tüm SAP uygulamaları için yazılım yükleme bileşenleri sözleşmeleri destekler.
- Lisansları ve yazılım yükleme bileşenleri tüm SAP olmayan uygulamalar için destek sözleşmeleri ilgili ve Azure (büyük örnekler) ortamda SAP HANA bağlantılı olarak kullanılır.

**Yetenekler**

- Deneyiminizi ve Azure Iaas ve bileşenlerinin bilgisine sahip.
- Deneyiminizi ve Azure bir SAP iş yükü dağıtma konusunda bilgi.
- SAP HANA yükleme personel onaylandı.
- SAP HANA geçici yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarlamak için Mimarı becerileri SAP.

**SAP**

- Beklenir SAP Müşteri yapıyorsanız ve bir desteğine sahip SAP ile sözleşme.
- SAP HANA ve büyük ölçekli büyütme donanımda nihai yapılandırmaları sürümlerinde SAP ile için özellikle uygulamaları HANA büyük örneği SKU'ları türü II sınıfının başvurun.


## <a name="storage"></a>Depolama

SAP HANA azure'da (büyük örnekler) için depolama düzeni Klasik dağıtım modeli yönergeleri önerilen SAP aracılığıyla SAP HANA tarafından yapılandırılır. Yönergeleri belgelenmiştir [SAP HANA depolama gereksinimleri](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi.

I sınıf türü HANA büyük örneği dört kez bellek toplu depolama birimi olarak gelir. HANA büyük örneği birimleri türü II sınıfı için depolama dört kat daha fazla değil. Birimleri HANA işlem günlüğü yedeklemeleri depolamak için amaçlanan bir birim gelir. Daha fazla bilgi için bkz: [yükleyin ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Depolama ayırma bakımından aşağıdaki tabloya bakın. Tablo farklı HANA büyük örneği birimleriyle sağlanan farklı birimler için kaba kapasitesi listeler.

| HANA büyük örneği SKU | hana/veri | hana/günlük | hana ve paylaşılan | Günlük/hana/yedekleme |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S192 | 4.608 GB | 1.024 GB | 1.536 GB | 1.024 GB |
| S192m | 11,520 GB | 1.536 GB | 1,792 GB | 1.536 GB |
| S384 | 11,520 GB | 1.536 GB | 1,792 GB | 1.536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2,040 GB |
| S384xm | 16.000 GB | 2.050 GB | 2.050 GB | 2,040 GB |
| S576m | 20.000 GB | 3,100 GB | 2.050 GB | 3,100 GB |
| S768m | 28,000 GB | 3,100 GB | 2.050 GB | 3,100 GB |
| S960m | 36,000 GB | 4,100 GB | 2.050 GB | 4,100 GB |


Gerçek Dağıtılmış birimler dağıtım ve birim boyutlarını göstermek için kullanılan bir aracı göre değişebilir.

HANA büyük örneği SKU ayırabilir, olası bölme parçaları birkaç örnek gibi görünmelidir:

| GB bellek bölümü | hana/veri | hana/günlük | hana ve paylaşılan | Günlük/hana/yedekleme |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1,792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Bu boyutlar biraz dağıtım ve birimlerinin aramak için kullanılan araçlar göre değişebilir kaba birim numaralarıdır. Ayrıca diğer bölüm boyutu vardır, 2.5 TB gibi. Bu depolama boyutları, önceki bölümler için kullanılan benzer bir formülü olan hesaplanır. "Bölüm" terimi, işletim sistemi, bellek veya CPU kaynaklarını herhangi bir şekilde bölümlenir anlamına gelmez. Depolama bölümleri tek tek HANA büyük örneği biriminde dağıtmak isteyebilirsiniz farklı HANA örnekleri için gösterir. 

Daha fazla depolama alanı gerekebilir. 1 TB birimler ek depolama alanı satın alarak depolama ekleyebilirsiniz. Bu ek depolama alanı ek bir birim eklenebilir. Ayrıca, bir veya daha fazla var olan birimler genişletmek için de kullanılabilir. İlk olarak dağıtılan ve genellikle önceki tabloları tarafından belirtildiği gibi birimlerin boyutunu azaltmak mümkün değildir. Birimleri adlarını değiştirmek veya adları bağlama mümkün değildir. Daha önce açıklanan depolama birimleri HANA büyük örneği birimleri NFS4 birimler olarak eklenir.

Depolama anlık görüntüleri yedekleme ve geri yükleme ve olağanüstü durum kurtarma amacıyla kullanabilirsiniz. Daha fazla bilgi için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi
Disklerde depolanan gibi HANA büyük örnek için kullanılan depolama, veri saydam bir şifreleme sağlar. HANA büyük örneği birim dağıtıldığında, bu tür bir şifreleme etkinleştirebilirsiniz. Dağıtım gerçekleştikten sonra şifrelenmiş birimlere de değiştirebilirsiniz. Şifrelenmemiş taşımak şifrelenmiş birimler için saydamdır ve kapalı kalma süresi gerektirmez. 

T türü ile sınıf SKU'ları LUN depolanmış önyükleme birimi şifrelenir. SKU'ları HANA büyük örneğini türü II sınıfı için işletim sistemi yöntemleriyle LUN önyükleme şifrelemeniz gerekir. Daha fazla bilgi için Microsoft Hizmet Yönetimi ekibine başvurun.


## <a name="networking"></a>Ağ

Azure Ağ Hizmetleri mimarisi başarılı uygulamaları dağıtma işlemini SAP HANA büyük örneğinde anahtar bir bileşenidir. Genellikle, Azure (büyük örnekler) dağıtımlarda SAP HANA veritabanları, CPU kaynak kullanımı ve bellek kullanımı çeşitli boyutlarda birkaç farklı SAP çözümleriyle birlikte daha büyük bir SAP yatay vardır. SAP HANA tabanlı bu tüm SAP sistemlerde olasıdır. SAP yatay kullanan bir karma olabilir:

- SAP sistemleri şirket içinde dağıtılabilir. Boyutlarının nedeniyle bu sistemler şu anda Azure'da barındırılamaz. Üretim, SQL Server (veritabanı) olarak çalışır ve VM'ler sunabileceğinden daha fazla CPU veya bellek kaynakları gerektiren SAP ERP sistem örneğidir.
- SAP HANA tabanlı SAP sistemleri şirket içi dağıtıldı.
- Sanal makineleri dağıtılan SAP sistemleri. Bu sistemler sınama, korumalı alan, geliştirme, olabilir veya üretim (vm'lerinde), kaynak kullanımı ve bellek talebe göre azure'da başarıyla dağıtabilirsiniz SAP NetWeaver tabanlı uygulamalardan herhangi biri için örnekler. Bu sistemler de veritabanlarını SQL Server gibi temel alabilir. Daha fazla bilgi için bkz: [SAP destek Not #1928533 – Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E). Ve bu sistemler veritabanları SAP HANA gibi temel alabilir. Daha fazla bilgi için bkz: [SAP HANA sertifikalı Iaas platformları](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- SAP HANA (büyük örnekler) azure'da Azure büyük örneği Damgalar yararlanan SAP uygulama sunucuları (VM'ler üzerinde) azure'da dağıtılabilir.

SAP yatay dört veya daha fazla farklı dağıtım senaryoları ile karma normaldir. Ayrıca birçok müşteri durumlar vardır, Azure'da çalışması tam SAP Windows'un. VM'ler daha güçlü dönüşürken, Azure üzerinde tüm SAP çözümleri taşıma müşteriler sayısını artırır.

Azure üzerinde dağıtılan SAP sistemleri bağlamında ağ azure karmaşık değil. Aşağıdaki kurallara göre dayanır:

- Bir şirket ağına bağlanan expressroute bağlantı hattı için Azure sanal ağları yeniden bağlanmanız gerekir.
- Şirket içi genellikle Azure'a bağlanan bir expressroute bağlantı hattı 1 GB/sn veya daha yüksek bant genişliği olması gerekir. Bu en düşük bant genişliği, şirket içi sistemleri ve sanal makineler çalıştıran sistemleri arasında veri aktarımı için yeterli bant genişliği sağlar. Ayrıca bağlantı için yeterli bant genişliği şirket içi kullanıcıları Azure sistemlere sağlar.
- Azure'da tüm SAP sistemlerini birbirleri ile iletişim kurmak için sanal ağlarda ayarlanması gerekir.
- Active Directory ve DNS barındırılan şirket içi AD'den Azure ExpressRoute aracılığıyla uygulamasına genişletilmiş.


> [!NOTE] 
> Bir faturalandırma açısından bakıldığında, belirli bir Azure bölgesindeki büyük örneği damgasında yalnızca bir kiracı için yalnızca bir Azure aboneliği bağlanabilir. Buna karşılık, tek bir büyük örneği damga Kiracı yalnızca bir Azure aboneliğine bağlanabilir. Bu gereksinim, Azure Faturalanabilir diğer nesneleri ile tutarlıdır.

SAP HANA Azure (büyük örnekler) üzerinde birden çok farklı Azure bölgelerinde dağıtılırsa, ayrı bir kiracı büyük örneği damgaya dağıtılır. Bu örnekler aynı SAP yatay bir parçası olduğu sürece, aynı Azure aboneliği altında her ikisi de çalıştırabilirsiniz. 

> [!IMPORTANT] 
> Yalnızca Azure Resource Manager dağıtımı, SAP HANA azure'da (büyük örnekler) ile desteklenir.

 

### <a name="additional-virtual-network-information"></a>Ek sanal ağ bilgileri

ExpressRoute için bir sanal ağa bağlanmak için bir Azure ağ geçidi oluşturulmalıdır. Daha fazla bilgi için bkz: [ExpressRoute için sanal ağ geçitleri hakkında](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 

Azure bir ağ geçidi, Azure dışında bir altyapı ya da bir Azure büyük örneği damga ExpressRoute ile kullanılabilir. Azure bir ağ geçidi, sanal ağlar arasında bağlanmak için de kullanılabilir. Daha fazla bilgi için bkz: [PowerShell kullanarak bir ağ ağ bağlantısı kaynak yöneticisi için yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu bağlantılar farklı Microsoft Kurumsal kenar yönlendiricilerden gelen sürece en fazla dört farklı ExpressRoute bağlantıları Azure ağ geçidine bağlanabilir. Daha fazla bilgi için bkz: [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Her ikisi de kullanım için Azure bir ağ geçidi sağlar verimlilik farklıdır. Daha fazla bilgi için bkz: [VPN Gateway hakkında](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bir sanal ağ geçidi ile elde edebilirsiniz en yüksek verimlilik 10 GB/sn hesaplamak için ExpressRoute bağlantısı değeri kullanmaktır. Bir sanal ağda bulunan bir VM ile bir sistem arasında dosya kopyalama şirket içi (olarak tek bir kopya akışı), farklı ağ geçidi SKU'ları tam verimini elde değil. Sanal ağ geçidinin tam bant genişliği yararlanmak için birden çok akış kullanın. Veya tek bir dosyayı paralel akış farklı dosyaları kopyalamanız gerekir.


### <a name="networking-architecture-for-hana-large-instance"></a>HANA büyük örneği için ağ mimarisi
Ağ mimarisi HANA büyük örneği için dört farklı bölümlere ayrılabilir:

- Şirket içi ağ ve Azure ExpressRoute bağlantısı. Bu bölümü, Müşteri'nin etki alanıdır ve Azure ExpressRoute üzerinden bağlanır. Aşağıdaki şekilde sağ alt köşedeki bakın.
- Daha önce ağ geçitleri yeniden olan sanal ağlar ile açıklandığı gibi azure ağ hizmetleri. Bu bölümü, uygulama gereksinimleri, güvenlik ve uyumluluk gereksinimlerine uygun tasarımları bulmanız gereken nerede bir alandır. HANA büyük örneği kullanıp kullanmayacağınızı aralarından seçim yapabileceğiniz sanal ağlar ve Azure ağ geçidi SKU'ları sayısı bakımından dikkate alınması gereken başka bir noktadır. Sağ üst şekildeki bakın.
- Bağlantılara HANA büyük örneğinin ExpressRoute teknolojisi aracılığıyla Azure. Bu bölümü dağıtılan ve Microsoft tarafından işlenir. Yapmanız gereken tek şey bazı IP adres aralıklarını varlıklarınızı HANA büyük örneğinde dağıtımdan sonra expressroute bağlantı hattı sanal ağlara bağlanma sağlayın. Daha fazla bilgi için bkz: [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Ağ iletişimi HANA büyük örneğinde, çoğunlukla sizin için saydamdır.

![SAP HANA Azure (büyük örnekler) ve şirket içi bağlı sanal ağ](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

HANA büyük örneği kullandığından Azure ExpressRoute aracılığıyla şirket içi varlıklarınızı bağlanmalısınız gereksinim değiştirmez. Gereksiniminin HANA büyük örneği birimlerinde barındırılan HANA örneklerinin bağlandığı uygulama katmanı ana bilgisayar, sanal makineleri çalıştıran bir veya birden çok sanal ağlar da değiştirmez. 

Azure SAP dağıtımlarda farklılıklar şunlardır:

- Müşteri kiracınızın HANA büyük örneği birimleri sanal ağlarınıza başka bir expressroute bağlantı hattı bağlanır. İş yükü koşullarının ayırmak için şirket içi sanal ağlar ExpressRoute bağlantıları ve sanal ağlar ve HANA büyük örnek arasındaki bağlantılar için aynı yönlendiriciler paylaşmayın.
- SAP uygulama katmanı HANA büyük örneği arasında iş yükünü profili çok sayıda küçük istekleri içeren farklı bir doğası tarafından kullanılır ve verileri (sonuç kümeleri) SAP HANA uygulama katmana aktarır gibi WINS'e.
- SAP uygulama mimarisi ağ gecikmesi için daha fazla şirket içi ve Azure arasında veri değişimi burada tipik senaryolar daha duyarlıdır.
- Sanal ağ geçidi en az iki ExpressRoute bağlantıları vardır. Sanal ağ geçidinin gelen veriler için en yüksek bant genişliği hem bağlantılarını paylaşın.

Yaşadı ağ gecikmesi VM'ler ve HANA büyük örneği arasında birimler bir tipik VM VM ağ gidiş dönüş gecikmesi yüksek olabilir. Azure bölgesinde bağımlı, değerlerin ölçülen ortalama olarak aşağıda sınıflandırılmış 0,7 ms gidiş dönüş gecikme da aşabilir [SAP Not #1100926 - SSS: ağ performansı](https://launchpad.support.sap.com/#/notes/1100926/E). Bununla birlikte, müşterilerin başarıyla örneğinde SAP HANA büyük SAP HANA tabanlı üretim SAP uygulamaları dağıtın. SAP HANA HANA büyük örneği birimleri kullanarak SAP uygulamalarını çalıştırarak rapor harika geliştirmeleri dağıtan müşteriler. İş süreçlerinizi Azure HANA büyük örneğinde sınamanız emin olun.
 
Belirleyici ağ gecikmesi VM'ler HANA büyük örneği arasında sağlamak için sanal ağ geçidi SKU temel seçimdir. Şirket içi ve VM'ler arasındaki trafiği desenlerini VM'ler HANA büyük örneği arasında trafiği düzeni küçük, ancak yüksek ani iletilecek istekleri ve veri birimleri, geliştirebilirsiniz. Bu tür WINS'e iyi işlemek için yüksek oranda UltraPerformance ağ geçidi SKU'su kullanılmasını öneririz. HANA büyük örneği SKU'ları türü II sınıfı için bir sanal ağ geçidi olarak UltraPerformance ağ geçidi SKU'su kullanımını zorunludur.

> [!IMPORTANT] 
> SAP uygulama ve veritabanı katmanları arasındaki genel ağ trafiğini göz önüne alındığında, yalnızca HighPerformance veya sanal ağlar için UltraPerformance ağ geçidi SKU'ları Azure (büyük örnekler) üzerinde SAP HANA bağlanmak için desteklenir. HANA büyük örneği türü II için SKU'ları, yalnızca UltraPerformance ağ geçidi SKU'su bir sanal ağ geçidi desteklenir.



### <a name="single-sap-system"></a>Tek SAP sistem

Daha önce gösterilen şirket içi altyapı ExpressRoute aracılığıyla Azure'da bağlandı. Expressroute bağlantı hattı enterprise sınır yönlendiricisi bağlanır. Daha fazla bilgi için bkz: [ExpressRoute teknik genel bakış](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Rota kurulduktan sonra Azure omurga bağlanır ve tüm Azure bölgeleri erişilebilir.

> [!NOTE] 
> SAP Windows'un Azure'da çalışması için enterprise sınır yönlendiricisi SAP yatay Azure bölgesinde en yakın bağlayın. Azure büyük örneği Damgalar Azure Iaas ve büyük örnek Damgalar VM'ler arasındaki ağ gecikmesini en aza indirmek için ayrılmış enterprise sınır yönlendiricisi cihazları üzerinden bağlanır.

SAP uygulama örnekleri barındırmak VM'ler için sanal ağ geçidi için expressroute bağlantı hattı bağlandı. Aynı sanal ağda ayrı enterprise sınır yönlendiricisi büyük örneği damgaları bağlanmakta ayrılmış bağlı.

Bu sistem, tek bir SAP sistem basit bir örnektir. SAP uygulama katmanı Azure'da barındırılır. SAP HANA veritabanına SAP HANA Azure (büyük örnekler) üzerinde çalışır. 2 GB/sn veya 10 GB/sn üretilen iş sanal ağın ağ geçidi bant tıkanıklık temsil etmez varsayılır.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Birden çok SAP sistemleri veya büyük SAP sistemleri

SAP HANA azure'da (büyük örnekler) bağlanmak için birden çok SAP sistemleri veya büyük SAP sistemleri dağıtılırsa, sanal ağ geçidi verimini bir ayak bağı. Böyle bir durumda, uygulama katmanları birden çok sanal ağ bölün. Ayrıca gibi durumlarda HANA büyük örneğine bağlanan özel bir sanal ağ oluşturabilirsiniz:

- NFS paylaşımlarını barındıran Azure VM doğrudan HANA büyük örneğinde HANA örneklerinden yedeklemeleri gerçekleştirmek.
- Büyük yedekleri ya da diğer dosyaları HANA büyük örneği birimlerindeki disk alanı Azure'da yönetilen kopyalama.

Ana bilgisayar depolama yönetin VM'ler için ayrı bir sanal ağ kullanın. Bu düzenlemenin büyük dosya veya veri aktarımını HANA büyük örneğinden Azure SAP uygulama katmanı çalıştıran VM'ler hizmet sanal ağ geçidi etkilerini önler. 

Daha fazla ölçeklenebilir bir ağ mimarisi için:

- Tek, daha büyük bir SAP uygulama katmanı için birden çok sanal ağlar yararlanın.
- Dağıtılan, her bir SAP sistemi için ayrı bir sanal ağ, aynı sanal ağda altında ayrı alt ağlar bu SAP sistemler birleştirmek için karşılaştırıldığında dağıtın.

 SAP HANA azure'da (büyük örnekler) için daha fazla ölçeklenebilir bir ağ mimarisi:

![SAP uygulama katmanı birden çok sanal ağ üzerinden dağıtma](./media/hana-overview-architecture/image4-networking-architecture.png)

Şekil SAP uygulama katmanı ya da birden çok sanal ağlar üzerinden dağıtılabilir bileşenleri gösterilir. Bu yapılandırma, bu sanal ağlarda bulunan uygulamalar arasında iletişim sırasında oluşan kaçınılmaz gecikme yükü kullanıma sunuldu. Varsayılan olarak, bu yapılandırmada enterprise sınır yönlendiricileri farklı sanal ağlar yönlendir bulunan VM'ler arasındaki ağ trafiğini. Eylül 2016 itibaren bu yönlendirme iyileştirilebilir. 

En iyi duruma getirme ve iki sanal ağ arasındaki iletişim gecikmesi aşağı Kes eşleme sanal ağlar aynı bölge içinde tarafından yoludur. Bu yöntem, bu sanal ağlar farklı Aboneliklerde olsa bile çalışır. Sanal Ağ eşlemesi ile iki farklı sanal ağlar, sanal makineleri arasındaki iletişimi, doğrudan birbirleri ile iletişim kurmak için Azure ağı omurga kullanabilirsiniz. Sanal makineleri aynı sanal ağda olması durumunda gibi gecikme süresini gösterir. Azure sanal ağ geçidinden bağlandığınızdan IP adres aralıklarını adresleri trafik sanal ağın tek tek sanal ağ geçidinden yönlendirilir. 

Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Azure'da yönlendirme

Üç ağ yönlendirme noktalar, SAP HANA azure'da (büyük örnekler) için önemlidir:

* SAP HANA Azure (büyük örnekler) üzerinde yalnızca VM'ler ve şirket içi doğrudan adanmış ExpressRoute bağlantısı üzerinden erişilebilir. Şirket içi HANA büyük örneği birimlerine doğrudan erişim size Microsoft tarafından sunulan gibi hemen mümkün değildir. SAP HANA büyük örnek için kullanılan geçerli Azure ağ mimarisini geçişli yönlendirme kısıtlamaları kaynaklanır. Bazı yönetim istemcileri ve doğrudan erişim, SAP çözüm şirket içi, çalışan Yöneticisi gibi herhangi bir uygulama SAP HANA veritabanına bağlanamıyor.

* İki farklı Azure bölgelerinde olağanüstü durum kurtarma için dağıtılan HANA büyük örneği birim varsa, aynı geçici yönlendirme kısıtlamaları geçerli olur. Diğer bir deyişle, IP adresleri (örneğin, ABD Batı ') bir bölgede HANA büyük örneği biriminin (örneğin, ABD Doğu) başka bir bölgeye dağıtılmış bir HANA büyük örneği birim yönlendirilir değil. Bu kısıtlama, bölgeler arasında eşliği ya da çapraz bağlanma HANA büyük örneği birimleri sanal ağlara bağlanmak ExpressRoute bağlantı hatları Azure ağ kullanımını bağımsızdır. Grafik gösterimi "Birden çok bölgelerde kullanım HANA büyük örneği birimler." bölümündeki şekle bakın Dağıtılan mimarisiyle gelir, bu kısıtlama olağanüstü durum kurtarma işlevi olarak HANA sistem çoğaltma hemen kullanımını engeller.

* SAP HANA Azure (büyük örnekler) birimlerdeki gönderdiğiniz sunucu IP havuzu adres aralığı atanmış bir IP adresi vardır. Daha fazla bilgi için bkz: [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu IP adresi, Azure abonelikleri ve HANA (büyük örnekler) azure'da sanal ağlar bağlandığı ExpressRoute aracılığıyla erişilebilir. Sunucu IP havuzu adres aralığı doğrudan donanım birime atanan dışı IP adresi atanır. Bunun *değil* Bu çözüm ilk dağıtımları durumda olduğu gibi NAT artık, atanmış. 

> [!NOTE] 
> İlk iki liste öğeleri açıklandığı gibi geçici akışındaki kısıtlama üstesinden gelmek için yönlendirme için ek bileşenler kullanın. Kısıtlama üstesinden gelmek için kullanılabilir bileşenleri olabilir:

> * Bir ters ve ondan veri yönlendirmek için proxy. Örneğin, F5 BIG-IP, NGINX ile trafik, Yöneticisi Azure sanal güvenlik duvarı/trafik yönlendirme çözümü olarak dağıtılabilir.
> * Kullanarak [IPTables kuralları](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) HANA büyük örneği birimleri farklı bölgelerde veya şirket içi konumlar ve HANA büyük örneği birimleri arasında yönlendirme etkinleştirmek için bir Linux VM.

> Uygulama ve üçüncü taraf içeren özel çözümler için destek cihazları ağ veya IPTables Microsoft tarafından sağlanmayan unutmayın. Destek kullanılan bileşenin satıcısına veya Tümleştirici tarafından sağlanmalıdır. 

### <a name="internet-connectivity-of-hana-large-instance"></a>HANA büyük örneğinin Internet bağlantısı
HANA büyük örneği mu *değil* doğrudan Internet bağlantısına sahip. Örnek olarak, bu sınırlama, işletim sistemi görüntüsü doğrudan işletim sistemi satıcı ile kaydetmek için yeteneğini kısıtla. Yerel SUSE Linux Enterprise Server Abonelik Yönetim Aracı sunucu veya Red Hat Enterprise Linux Abonelik Yöneticisi ile çalışmak gerekebilir.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>Sanal makineleri HANA büyük örneği arasında veri şifrelemesi
HANA büyük örneği ve VM'ler arasında aktarılan veriler şifrelenmez. Ancak, tamamen exchange için HANA DBMS yan JDBC/ODBC tabanlı uygulamalar arasındaki trafiği şifrelemeyi etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [SAP tarafından bu belgeleri](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>Birden çok bölgede HANA büyük örneği birimleri kullanın

SAP HANA bölgelerdeki birden çok Azure dışında olağanüstü durum kurtarma (büyük örnekler) azure'da dağıtmak için neden olabilir. Belki de her bölgelerde farklı sanal ağlarda dağıtılan VM'ler HANA büyük örneği erişim sağlamak istiyorsunuz. Farklı HANA büyük örneği birimleri atanan IP adreslerini örnekleri kendi ağ geçidi üzerinden doğrudan bağlı sanal ağlar ötesinde yayılmaz. Sonuç olarak, küçük bir değişiklik için sanal ağ tasarımı sunulmuştur. Bir sanal ağ geçidi başka bir kurumsal sınır yönlendiricileri dışında farklı dört ExpressRoute bağlantı hatları işleyebilir. Büyük örneği Damgalar birine bağlı her bir sanal ağı başka bir Azure bölgesindeki büyük örneği damga bağlanabilir.

![Farklı Azure bölgelerinde Azure büyük örneği Damgalar bağlı sanal ağ](./media/hana-overview-architecture/image8-multiple-regions.png)

Şekil nasıl hem bölgelerde farklı sanal ağlar Azure (büyük örnekler) üzerinde SAP HANA bağlanmak için kullanılan iki farklı ExpressRoute bağlantı hatları için hem Azure bölgelerinde bağlandığını gösterir. Dikdörtgen kırmızı satırları yeni sunulan bağlantılardır. Sanal ağlar dışında bu bağlantılar ile bu sanal ağlardan birini çalıştıran VM'ler her iki bölgelerde dağıtılan farklı HANA büyük örneği birimlerinin erişebilir. Aşağıdaki şekilde gösterildiği gibi şirket içi iki ExpressRoute bağlantıları iki Azure bölgeleri sahip olduğunuz varsayılır. Bu düzenleme, olağanüstü durum kurtarma nedenlerle önerilir.

> [!IMPORTANT] 
> Birden çok ExpressRoute bağlantı hatları kullandıysanız, uygun trafik yönlendirme emin olmak için AS yolu eklenmesini ve yerel tercih BGP ayarları kullanılmalıdır.


