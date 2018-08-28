---
title: Genel bakış ve SAP hana (büyük örnekler) azure'da bir mimari | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da dağıtma konusunda mimari genel bakış.
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
ms.date: 08/27/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d28b6ea5612a3db539c51d2603c3f12282ca519
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090425"
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>SAP HANA (büyük örnekler) genel bakış ve azure'da mimarisi

## <a name="what-is-sap-hana-on-azure-large-instances"></a>SAP HANA (büyük örnekler) azure'da nedir?

SAP HANA (büyük örnekler) azure'da, Azure için benzersiz bir çözümdür. SAP HANA çalıştırmayı ve dağıtmak için sanal makineler sağlamanın yanı sıra Azure çalıştırın ve size atanmış çıplak sunucularda SAP HANA dağıtma olanağı sunar. SAP HANA (büyük örnekler) Azure çözümü, size atanan konak/sunucu paylaşılmayan çıplak bilgisayar donanım üzerine inşa edilmiştir. Sunucu donanımı işlem/sunucu, ağ ve depolama altyapısı içeren daha büyük damga olarak katıştırılır. Bir birleşimi buna sertifikalı uyarlanmış HANA veri merkezi tümleştirmesi (TDI) var. SAP HANA (büyük örnekler) Azure üzerinde farklı bir sunucuya SKU'ları veya boyutları sunar. Birimleri 36 Intel CPU Çekirdeği ve 768 GB bellek ve en fazla 480 Intel CPU çekirdeği olan birimleri adede kadar ve 24 Git olabilir TB bellekle.

Altyapı damga içindeki müşteri yalıtımı, kiracılar, benzer gerçekleştirilir:

- **Ağ**: yalıtım müşterilerin altyapı yığını aracılığıyla müşteri atanan Kiracı başına sanal ağ içinde. Bir kiracı, tek bir müşteriye atanır. Bir müşteri, birden çok kiracıya sahip olabilir. Kiracılar aynı müşteriye ait olsa bile, kiracılar ağ yalıtımını altyapı damga düzeyi, kiracılar arasında ağ iletişimi engeller.
- **Depolama bileşenleri**: atanmış depolama birimleri olan depolama sanal makineler için yalıtımı. Depolama birimleri, yalnızca bir depolama sanal makineye atanabilir. Bir depolama alanı sanal makine yalnızca tek bir kiracı SAP HANA TDI sertifikalı altyapı yığınında atanır. Sonuç olarak, bir depolama alanı sanal makineye atanan depolama birimleri, bir özel ve ilgili kiracıda yalnızca erişilebilir. Bunlar, farklı dağıtılan kiracılar arasında görünmez.
- **Sunucu veya konak**: sunucu veya konak birimi müşteriler veya kiracılar arasında paylaşılmaz. Bir sunucu veya dağıtılmış bir müşteri için ana, tek bir kiracı için atanmış bir atomik çıplak işlem birimi değil. *Hayır* bölümleme donanım veya yazılım bölümleme kullanılır, başka bir müşteri bir konak ya da bir sunucu paylaşımı içinde neden. Belirli bir kiracıda depolama sanal makineye atanan depolama birimleri, bu tür bir sunucuya bağlanır. Bir kiracı özel olarak atanan farklı SKU'ları çok sayıda sunucu ölçü birine sahip olabilir.
- İçinde bir SAP HANA (büyük örnekler) Azure altyapı damgası üzerinde birçok farklı kiracıların dağıtılır ve ağ, depolama ve işlem düzeyi Kiracı kavramları aracılığıyla birbirine karşı yalıtılmış. 


Bu tam sunucu birimleri, yalnızca SAP HANA çalıştırmayı desteklenir. SAP uygulama katmanında veya iş yükü donanımlar orta katman sanal makinelerde çalıştırır. SAP HANA (büyük örnekler) Azure birimlerde çalıştıran altyapı Damgalar için Azure Ağ Hizmetleri omurgalarından bağlanır. Bu şekilde, SAP HANA (büyük örnekler) Azure birimleri ve sanal makineler arasında düşük gecikme süreli bağlantı sağlanır.

Bu belgede, SAP HANA (büyük örnekler) azure'da kapsayan birkaç belge biridir. Bu belge, temel mimari, sorumlulukları ve çözüm tarafından sağlanan hizmetleri tanıtır. Çözüme üst düzey özellikleri de ele alınmıştır. Ağ ve bağlantı gibi birçok diğer alanlarda, dört belge detaya gitme bilgileri kapsar. Belgeleri (büyük örnekler) Azure üzerinde SAP hana, SAP NetWeaver yükleme yönlerini veya vm'lerde SAP NetWeaver dağıtımlarını ele alınmamıştır. Azure'da SAP NetWeaver aynı Azure belgeleri kapsayıcıda bulunan ayrı belgelerinde ele alınmıştır. 


Farklı belgeleri HANA büyük örneği kılavuzunun aşağıdaki alanları kapsar:

- [SAP HANA (büyük örnekler) genel bakış ve azure'da mimarisi](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) sorun giderme ve Azure'da izleme](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [STONITH kullanarak yüksek kullanılabilirlik SUSE ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [İşletim sistemi yedekleme ve geri yükleme Type II SKU'lara yönelik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Tanımlar

Mimari ve Teknik Dağıtım Kılavuzu, birkaç ortak tanımlara yaygın olarak kullanılır. Aşağıdaki terimler ve anlamlarını dikkat edin:

- **Iaas**: hizmet olarak altyapı.
- **PaaS**: hizmet olarak Platform.
- **SaaS**: hizmet olarak yazılım.
- **SAP bileşen**: ERP merkezi bileşeni (ECC), Business Warehouse (BW), çözüm Yöneticisi ya da Enterprise Portal (EP) gibi tek bir SAP uygulama. SAP bileşenleri geleneksel ABAP veya Java teknolojileri ya da bir olmayan-NetWeaver tabanlı uygulama iş nesneleri gibi temel alabilir.
- **SAP ortamı**: bir veya daha fazla SAP bileşenleri mantıksal olarak gruplanmış geliştirme, kalite güvencesi, eğitim, olağanüstü durum kurtarma veya üretim gibi bir iş işlevi gerçekleştirmek için.
- **SAP ortamı**: tamamı SAP varlıkları, BT yatay ifade eder. SAP ortamı, tüm üretim ve üretim dışı ortamlar içerir.
- **SAP sistemine**: DBMS katmanı ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sistemi, bir SAP BW test sistemi ve bir SAP CRM üretim sistemine birleşimi. Azure dağıtımlar, bu iki şirket içi ile Azure arasında bölünen desteklemez. Azure'da dağıtılan şirket içinde dağıtılabilir veya onun bir SAP sistemiyle değil. Azure veya şirket içine bir SAP ortamının farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtabilir ve sistemleri, SAP CRM üretim sistemi şirket içi dağıtırken, Azure'da test etme. (Büyük örnekler) Azure üzerinde SAP HANA, SAP sistemlerini vm'lerde SAP uygulama katmanı ve SAP hana (büyük örnekler) Azure damgası üzerinde bir birim ilgili SAP HANA örneğinde ana bilgisayar içindir.
- **Büyük örneği damgasında**: SAP HANA TDI sertifikalı ve azure'da SAP HANA örnekleri çalıştırmak için adanmış bir donanım altyapısı yığını.
- **SAP HANA (büyük örnekler) azure'da:** büyük örnek Damgalar farklı Azure bölgelerindeki dağıtılmış SAP HANA TDI sertifikalı donanımlarda HANA çalıştırmak Azure teklifi için resmi ad örnekler. İlgili dönem *HANA büyük örneği* kısaltması olduğundan *SAP HANA (büyük örnekler) azure'da* ve bu teknik Dağıtım Kılavuzu'nda yaygın olarak kullanılır.
- **Şirketler arası**: siteden siteye, çok siteli veya Azure ile şirket içi veri merkezleri arasında Azure ExpressRoute bağlantısı olan bir Azure aboneliğine VM'ler dağıtıldığı bir senaryo açıklanır. Azure ortak belgeler, bu tür dağıtımlar şirketler arası senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Azure Active Directory/OpenLDAP ve şirket içi DNS Azure'a genişletmek için nedenidir. Şirket içi yatay Azure aboneliklerini Azure varlıkları için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. 

   Şirket içi etki alanının etki alanı kullanıcıları, sunuculara erişmek ve Hizmetleri (DBMS hizmetler gibi) bu sanal makineler üzerinde çalıştırın. Şirket içi sanal makineler arasında iletişim ve ad çözümlemesini dağıtılır ve Azure tarafından dağıtılan Vm'leri mümkündür. Bu senaryo, çoğu SAP varlıklar dağıtıldığı yol, tipik bir durumdur. Daha fazla bilgi için [planlamak ve tasarlamak için Azure VPN ağ geçidi](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [Azure portalını kullanarak siteden siteye bağlantı ile sanal ağ oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Kiracı**: HANA büyük örneği damgasında dağıtılan bir müşteri olarak yalıtılmış bir *Kiracı.* Bir kiracı, ağ, depolama ve bilgi işlem katmanını diğer kiracılardan içinde yalıtılır. Farklı kiracıların atanan depolama ve işlem birimleri birbirine göremez veya birbirleri ile iletişim kurmak HANA büyük örnek damgası düzeyine. Bir müşteri farklı kiracıların dağıtımlarını sahip olmayı seçebilirsiniz. Daha sonra HANA büyük örnek damgası düzeyinde kiracılar arasında hiçbir iletişim yok.
- **SKU kategori**: HANA büyük örneği için aşağıdaki iki kategoriden SKU ile sunulur:
    - **Ben sınıf türü**: S72, S72m, S144, S144m, S192, S192m ve S192xm
    - **Türü II sınıfı**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm ve S960m


Çeşitli ek kaynaklara bulutta, SAP iş yükünü dağıtmak nasıl kullanılabilir. Azure'da SAP HANA dağıtımı yürütmek planlıyorsanız, deneyimli ile ve Azure Iaas ilkeleri ve SAP iş yüklerini Azure ıaas dağıtımını farkında olması gerekir. Devam etmeden önce bkz [kullanım SAP çözümlerini Azure sanal makinelerinde](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) daha fazla bilgi için. 

## <a name="certification"></a>Sertifika

NetWeaver sertifika yanı sıra, SAP, SAP hana, SAP HANA Azure Iaas gibi belirli altyapıları desteklemek özel bir sertifika gerektirir.

Temel, derece SAP HANA sertifika ve NetWeaver'lu SAP notuna [SAP notu #1928533-azure'da SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533).

SAP HANA (büyük örnekler) Azure birimlerdeki sertifika kayıtlarını bulunabilir [SAP HANA sertifikalı ve Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) site. 

SAP HANA (büyük örnekler) Azure türleri, SAP HANA sertifikalı Iaas platformları site içinde başvurulan Microsoft sağlar ve müşteriler SAP büyük SAP Business Suite, SAP BW, s/4 HANA, BW/4HANA veya diğer SAP HANA iş yüklerini azure'da dağıtma yeteneği. Çözüm, SAP HANA sertifikalı adanmış donanım damgada bağlıdır ([SAP HANA veri merkezi tümleştirmesi – TDI uyarlanmış](https://scn.sap.com/docs/DOC-63140)). SAP HANA TDI yapılandırılmış bir çözüm çalıştırırsanız, tüm SAP HANA tabanlı uygulamalar (örneğin, SAP hana'da SAP Business Suite, SAP BW on HANA SAP S4/HANA'ya ve BW4/HANA) donanım altyapısında çalışır.

SAP HANA Vm'lerinde çalışan için karşılaştırıldığında, bu çözümü bir avantajı vardır. Bu, çok daha yüksek bellek hacimleri için sağlar. Bu çözümü etkinleştirmek için aşağıdaki önemli noktalar anlamanız gerekir:

- SAP uygulama katmanı ve SAP olmayan uygulamalar her zamanki Azure donanım damga olarak barındırılan sanal makineleri çalıştırın.
- Müşteri şirket altyapısı, veri merkezleri ve uygulama dağıtımları (önerilen) ExpressRoute aracılığıyla bulut platformu veya sanal özel ağ (VPN) bağlı. Ayrıca Active Directory ve DNS Azure'a genişletilir.
- SAP HANA veritabanı örneği HANA iş yükü için SAP HANA (büyük örnekler) Azure üzerinde çalışır. Vm'lerde çalışan yazılım HANA büyük örneği çalıştıran HANA örneği ile etkileşim kurabilmesi büyük örneği damgasında Azure ağı ile bağlı.
- Donanım (büyük örnekler) Azure üzerinde SAP HANA'ın bir Iaas ile SUSE Linux Enterprise Server ya da Red Hat Enterprise Linux makineleridir sağlanan adanmış donanım ' dir. Sanal makineler gibi daha fazla güncelleştirmeler ve Bakım işletim sistemi için olduğundan sizin sorumluluğunuzdadır.
- HANA veya SAP HANA HANA büyük örneği birimlerde çalıştırmak için gereken tüm ek bileşenleri sizin sorumluluğunuzdur. Tüm ilgili devam eden işlemler ve Azure üzerinde SAP HANA yönetimini de sizin sorumluluğunuzdadır.
- Burada açıklanan çözümlerinin yanı sıra, Azure aboneliğinizdeki SAP HANA (büyük örnekler) azure'da bağlandığı diğer bileşenleri yükleyebilirsiniz. İletişimi ile veya doğrudan SAP HANA etkinleştiren bileşenleri, atlama sunucuları gibi RDP sunucuları, SAP HANA Studio SAP BI senaryoları için SAP Data Services veritabanı ya da ağ izleme çözümleri verilebilir.
- Azure olduğu gibi HANA büyük örneği, yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevleri için destek sunar.

## <a name="architecture"></a>Mimari

Yüksek düzeyde, SAP HANA (büyük örnekler) Azure çözüm üzerinde bulunan VM'ler SAP uygulama katmanı vardır. Veritabanı katmanı, bir büyük örneği damgasında Azure Iaas bağlı olduğu aynı Azure bölgesinde bulunan SAP TDI yapılandırılmış donanımda bulunur.

> [!NOTE]
> SAP DBMS katmanı ile aynı Azure bölgesinde SAP uygulama katmanı dağıtın. Bu kural, azure'da SAP iş yükleri hakkında yayımlanan bilgi iyi belgelenmiştir. 

Sanallaştırılmamış, çıplak metal, SAP HANA veritabanı için yüksek performanslı sunucusu olan bir SAP TDI sertifikalı donanım yapılandırması, SAP HANA (büyük örnekler) azure'da genel mimarisi sağlar. Ayrıca, özelliği ve kaynakları, ihtiyaçlarınızı karşılamak SAP uygulama katmanı için Azure'nın esnekliği sağlar.

![SAP HANA (büyük örnekler) Azure ile ilgili mimari genel bakış](./media/hana-overview-architecture/image1-architecture.png)

Mimarisini üç bölümlere ayrılmıştır:

- **Sağ**: verilerde farklı uygulamalarını çalıştıran bir şirket içi altyapı merkezleri SAP gibi LOB uygulamalarını son kullanıcılara erişebilmesi gösterir. İdeal olarak, bu altyapı ile azure'a bağlı sonra şirket [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Merkezi**: Azure Iaas gösterir ve bu durumda, SAP ve SAP HANA DBMS sistemi olarak kullanan diğer uygulamaları barındırmak için sanal makinelerin kullanın. VM'ler bellekle işlevi daha küçük HANA örnekleri kendi uygulama katmanı ile birlikte Vm'lere dağıtılır. Sanal makineler hakkında daha fazla bilgi için bkz. [sanal makineler](https://azure.microsoft.com/services/virtual-machines/).

   Azure Ağ Hizmetleri, SAP sistemlerini diğer uygulamalara sanal ağlar ile birlikte gruplandırmak için kullanılır. Şirket içi sistemleri de (büyük örnekler) Azure üzerinde SAP HANA için bu sanal ağları bağlayın.

   SAP NetWeaver uygulamalar ve Azure'da çalıştırmak için desteklenen veritabanları için bkz. [SAP destek Not #1928533-azure'da SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533). Azure'da SAP çözümleri dağıtma hakkında daha fazla bilgi için bkz:

  -  [Windows sanal makinelerinde SAP kullanma](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Azure sanal makinelerinde SAP çözümlerini kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Sol**: SAP HANA TDI sertifikalı donanım Azure büyük örneği damgasında gösterir. HANA büyük örneği birimleri aboneliğinizin sanal ağlara bağlantı şirket içinden azure'a aynı teknolojiyi kullanarak bağlanır.

Azure büyük örnek damgası aşağıdaki bileşenleri birleştirir:

- **Bilgi işlem**: gerekli hesaplama yeteneği sağlar ve SAP HANA sertifikalı olan Intel Xeon E7-8890v3 veya Intel Xeon E7-8890v4 işlemcilerde bağlı sunucuları.
- **Ağ**: A birleşik bilgi işlem, depolama ve LAN bileşenleri eşitliyor yüksek hızlı ağ fabric.
- **Depolama**: bir birleşik ağ yapısı erişilen bir depolama altyapısı. Belirli bir SAP HANA, dağıtılan Azure (büyük örnekler) yapılandırması hakkında sağlanan belirli depolama kapasitesi bağlıdır. Daha fazla depolama kapasitesi, ek bir aylık ücret ödemeden kullanılabilir.

Büyük örneği damgasında çok kiracılı altyapısında müşteriler yalıtılmış Kiracı dağıtılır. Kiracı dağıtımı sırasında bir Azure aboneliği içinde Azure kaydınızın adı. Bu Azure aboneliği, HANA büyük örneği karşı faturalandırılır bilgisayardır. Bu kiracıları 1:1 ilişki Azure aboneliğine sahip. Bir ağ için bir Azure bölgesinden farklı Azure aboneliklere ait sanal ağlar farklı bir kiracıda dağıtılmış bir HANA büyük örneği birim erişmek mümkündür. Bu Azure abonelikleri için aynı Azure kayıt ait olmalıdır. 

SAP HANA (büyük örnekler) azure'da vm'lerle olduğu gibi birden çok Azure bölgesinde sunulur. Olağanüstü durum kurtarma özellikleri sunmak için katılım seçebilirsiniz. Farklı büyük örnek Damgalar bir siyasi coğrafi bölge içinde birbirlerine bağlı. Örneğin, HANA büyük örneği damgaları ABD Batı hem de ABD Doğu, olağanüstü durum kurtarma çoğaltması için bir adanmış ağ bağlantısı üzerinden bağlanır. 

Farklı VM türler ile Azure sanal makineler arasında yalnızca seçebileceğiniz gibi farklı SKU'ları, HANA büyük SAP HANA farklı iş yükü türleri için uygun örnek arasından seçim yapabilirsiniz. SAP, bellek için işlemci yuvasında oranları Intel işlemci nesillerde göre değişen iş yükleri için geçerlidir. Aşağıdaki tabloda sunulan SKU türleri gösterilmektedir.

SAP HANA (büyük örnekler) Azure hizmeti üzerinde çeşitli yapılandırmalarda ABD Batı ve ABD Doğu, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa, Kuzey Avrupa, Japonya Doğu ve Japonya Batı, Azure bölgelerinde kullanılabilir.

[SAP HANA sertifikalı ve SKU'ları, HANA büyük örnekleri](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) gibi listeleyin:

| SAP çözümü | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| OLAP için İyileştirildi: SAP BW, BW/4hana'yı<br /> veya genel OLAP iş yükü için SAP HANA | Azure S72 üzerinde SAP HANA<br /> – 2 x Intel® Xeon® İşlemci E7-8890 v3<br /> 36 CPU Çekirdeği ve 72 CPU iş parçacıkları |  768 GB |  3 TB | Kullanılabilir |
| --- | Azure S144 üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v3<br /> 72 CPU Çekirdeği ve 144 CPU iş parçacıkları |  1,5 TB |  6 TB | Artık önerilmez |
| --- | Azure S192 üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v4<br /> 96 CPU Çekirdeği ve 192 CPU iş parçacıkları |  2.0 TB |  8 TB | Kullanılabilir |
| --- | Azure S384 üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  4.0 TB |  16 TB | Kullanılabilir |
| OLTP için iyileştirilmiştir: SAP Business Suite<br /> SAP HANA veya S/4HANA (OLTP)<br /> Genel OLTP | Azure S72m üzerinde SAP HANA<br /> – 2 x Intel® Xeon® İşlemci E7-8890 v3<br /> 36 CPU Çekirdeği ve 72 CPU iş parçacıkları |  1,5 TB |  6 TB | Kullanılabilir |
|---| Azure S144m üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v3<br /> 72 CPU Çekirdeği ve 144 CPU iş parçacıkları |  3.0 TB |  12 TB | Artık önerilmez |
|---| Azure S192m üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v4<br /> 96 CPU Çekirdeği ve 192 CPU iş parçacıkları  |  4.0 TB |  16 TB | Kullanılabilir |
|---| Azure S384m üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  6.0 TB |  18 TB | Kullanılabilir |
|---| Azure S384xm üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  8.0 TB |  22 TB |  Kullanılabilir |
|---| Azure S576m üzerinde SAP HANA<br /> – 12 x Intel® Xeon® İşlemci E7-8890 v4<br /> 288 CPU Çekirdeği ve 576 CPU iş parçacıkları |  12.0 TB |  28 TB | Kullanılabilir |
|---| Azure S768m üzerinde SAP HANA<br /> – 16 x Intel® Xeon® İşlemci E7-8890 v4<br /> 384 CPU Çekirdeği ve 768 CPU iş parçacıkları |  16,0 TB |  36 TB | Kullanılabilir |
|---| Azure S960m üzerinde SAP HANA<br /> -20 x Intel® Xeon® İşlemci E7-8890 v4<br /> 480 CPU Çekirdeği ve 960 CPU iş parçacıkları |  20.0 TB |  46 TB | Kullanılabilir |


SAP HANA TDIv5 altında SAP müşteriye özgü boyutlandırma ve içinde onaylı olarak listelenen değil sunucu yapılandırmaları için yol açabilecek müşteriye özgü projeleri sağlar:

- [SAP HANA sertifikalı cihazları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [SAP HANA sertifikalı ve Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

İçinde çok sayıda durumlarda, bu müşteriye özgü sunucu yapılandırmaları ile SAP sertifikalı sunucu birimlerini daha fazla bellek taşır. SAP kullanmaya çalışırken, müşteriler SAP destek almak ve müşteriye özgü ölçekli sunucu yapılandırmalarını onaylamak için olanağına sahip olursunuz. Azure'da HANA büyük örneği aşağıdaki standart SKU'lar kullanılabilir ve Microsoft gibi TDIv5 müşteriye özgü boyutlandırma projeler için liste fiyatı.


| Olabilir özgün SKU <br /> bellekte genişletilmiş | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| S192m Genişletilebilir | Azure S192xm üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v4<br /> 96 CPU Çekirdeği ve 192 CPU iş parçacıkları |  6.0 TB |  16 TB | Kullanılabilir |
| S384xm Genişletilebilir | Azure S384xxm üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  12.0 TB |  28 TB | Kullanılabilir |
| S576m Genişletilebilir | Azure S576xm üzerinde SAP HANA<br /> – 12 x Intel® Xeon® İşlemci E7-8890 v4<br /> 288 CPU Çekirdeği ve 576 CPU iş parçacıkları |  18.0 TB |  41 TB | Kullanılabilir |
| S768m Genişletilebilir | Azure S768xm üzerinde SAP HANA<br /> – 16 x Intel® Xeon® İşlemci E7-8890 v4<br /> 384 CPU Çekirdeği ve 768 CPU iş parçacıkları |  24.0 TB |  56 TB | Kullanılabilir |

- CPU çekirdekleri toplam olmayan-hyper-Threading Teknolojisi CPU çekirdek işlemcileri sunucusu birimi toplamı =.
- CPU iş parçacıklarını hiper iş parçacıklı CPU çekirdeği sunucusu birimi işlemcilerin toplamı tarafından sağlanan işlem iş parçacıklarının toplam =. Çoğu birimleri Hyper-Threading teknolojisini kullanmak için varsayılan olarak yapılandırılır.
- Tedarikçi önerilerine S768m göre S768xm ve S960m SAP HANA çalıştırmak için Hyper-Threading kullanmak için yapılandırılmamış.


Seçilen belirli yapılandırmalar, iş yükü, CPU kaynaklarını ve istenen bellek bağımlıdır. OLTP iş yükünün OLAP iş yükü için iyileştirilmiştir SKU'ları kullanmak mümkündür. 

Müşteriye özgü boyutlandırma projeleri için birim dışında teklifler için temel donanım SAP HANA TDI sertifikalı. Donanım iki farklı sınıflardaki SKU'ları halinde bölmek:

- S72 S72m, S144, S144m, S192, S192m ve S192xm, "ı sınıf türü olarak" adlandırılan SKU.
- S384, S384m, S384xm, S384xxm, S576m, S576xm S768m, S768xm ve denir S960m, "Type II sınıfı" SKU.

Eksiksiz bir HANA büyük örnek damgası için tek bir müşteriye özel olarak ayrılmış değil&#39;s kullanın. Bu olgu bölmelerin de Azure'da dağıtılan bir ağ yapısı bağlı işlem ve depolama kaynakları için geçerlidir. Azure gibi HANA büyük örneği altyapı, farklı müşteri dağıtır &quot;kiracılar&quot; birbirlerinden aşağıdaki üç düzeyden birinde yalıtılmış olan:

- **Ağ**: HANA büyük örneği damgasında içindeki sanal ağlar arasında yalıtım.
- **Depolama**: depolama sanal makineler, atanan depolama birimleri ve depolama birimleri kiracılar arasında yalıtmak için yalıtımı.
- **İşlem**: sunucu birimlerin atamasını tek bir kiracı için ayrılmış. Hayır sabit veya geçici sunucu ölçü bölümleme. Hiçbir tek bir sunucu veya konak birimi kiracılar arasında paylaştırma. 

HANA büyük örneği birimleri arasında farklı kiracıların dağıtımları birbirine görünmez. HANA büyük örneği birimleri farklı kiracıda dağıtılan diğer HANA büyük örnek damgası düzeyinde ile doğrudan iletişim kuramaz. Yalnızca tek bir kiracı içindeki HANA büyük örneği birimleri HANA büyük örnek damgası düzeyine birbiriyle iletişim kurabilir.

Büyük örneği damgasında dağıtılan bir kiracıda bir Azure aboneliği için faturalandırma için atanır. Bir ağ için diğer Azure aboneliklerine aynı Azure kayıt içinde olan sanal ağlardan erişilebilir. Aynı Azure bölgesindeki başka bir Azure aboneliğiyle dağıtırsanız, ayrılmış bir HANA büyük örneği kiracısı için sorulacak seçebilirsiniz.

HANA büyük örneği üzerinde SAP HANA ve Azure'da dağıtılan VM'ler üzerinde çalışan SAP HANA çalıştırma arasındaki önemli farklar vardır:

- SAP hana (büyük örnekler) azure'da sanallaştırma katmanı yoktur. Temel alınan çıplak bilgisayar donanım performansını sahip olursunuz.
- Azure, SAP HANA (büyük örnekler) Azure sunucusunda belirli bir müşteriye ayrılmış. Sunucusu birimi veya ana bilgisayar sabit veya geçici bölümlenmiş, kaybetme riski yoktur. Sonuç olarak, HANA büyük örneği birim bir bütün olarak bir kiracı ve, size atanmış olarak kullanılır. Bir yeniden başlatma veya kapatma sunucusunun işletim sistemi ve başka bir sunucuya dağıtılmasını SAP HANA için otomatik olarak yol değil. (Türünün miyim SKU'ları sınıfı, tek özel durum bir sunucu karşılaştığı sorunları ve yeniden dağıtma işlemi başka bir sunucuda gerçekleştirilmesi gerekir.)
- Burada ana bilgisayar İşlemci türleri için en iyi fiyat/performans oranını seçilir, Azure (büyük örnekler) Azure üzerinde SAP HANA için seçilen işlemci Intel E7v3 ve E7v4 işlemci satırının en yüksek gerçekleştirme türleridir.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Bir HANA büyük örneği biriminde birden çok SAP HANA örnekleri çalıştırma
HANA büyük örneği birimi birden fazla etkin SAP HANA örneğinde barındırmak mümkündür. Depolama anlık görüntüleri ve olağanüstü durum kurtarma özellikleri sağlamak için bu tür bir yapılandırma örneği başına bir birim gerektirir. Şu anda, HANA büyük örneği birimleri gibi alt bölümlere ayrılabilir:

- **S72, S72m, S144, S192**: en düşük başlangıç birim 256 GB ile 256 GB'lık artışlarla. 256 GB ile 512 GB gibi farklı aralıklarla en büyük bellek birimi birleştirilebilir.
- **S144m ve S192m**: en küçük birim 512 GB ile 256 GB'lık artışlarla. 512 GB ve 768 GB gibi farklı aralıklarla en büyük bellek birimi birleştirilebilir.
- **Türü II sınıfı**: en düşük başlangıç birim 2 TB ile 512 GB'lık artışlarla. 512 GB, 1 TB ve 1,5 TB'lık gibi farklı aralıklarla en büyük bellek birimi birleştirilebilir.

Birden çok SAP HANA örnekleri çalışan bazı örnekler aşağıdaki gibi görünebilir.

| SKU | Bellek boyutu | Depolama boyutu | Birden fazla veritabanıyla, boyutları |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA örneği<br /> ya da 1 x 512 GB örneğine + 1 x 256 GB örneği<br /> veya 3 x 256 GB örnekleri | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA örnekleri<br />ya da 1 x 512 GB örneğine + 1 x 1 TB örneği<br />veya 6 x 256 GB örnekleri<br />veya 1x1.5 TB örneği | 
| S192m | 4 TB | 16 TB | 8 x 512 GB örnekleri<br />veya 4 x 1 TB örnekleri<br />ya da 4 x 512 GB örnekleri + 2 x 1 TB örnekleri<br />ya da 4 x 768 GB örnekleri + 2 x 512 GB örnekleri<br />ya da 1 x 4 TB örneği |
| S384xm | 8 TB | 22 TB | 4 x 2 TB örnekleri<br />veya 2 x 4 TB örnekleri<br />ya 2 x 3 TB örnekleri + 1 x 2 TB örnekleri<br />veya örnekleri 2x2.5 TB + 1 x 3 TB örnekleri<br />ya da 1 x 8 TB örneği |


Diğer farklılıklar da vardır. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA veri katmanlama ve uzantı düğümleri kullanma
SAP, SAP NetWeaver farklı sürümleri, SAP BW ve SAP BW/4HANA için bir veri katmanlama modelini destekler. SAP belge veri katmanlama modeli hakkında daha fazla bilgi için bkz. [SAP BW/4hana'yı ve SAP BW on HANA ve SAP HANA uzantısı düğümlerle](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
HANA büyük örnek sayesinde, SAP HANA uzantısı düğümün seçeneği 1 yapılandırması SSS ve SAP blog belgelerinde açıklandığı gibi kullanabilirsiniz. Seçenek 2 yapılandırmalar ayarlanabilir aşağıdaki HANA büyük örneği SKU'ları ile: S72m, S192 S192m S384 ve S384m. 

Adresindeki belgelere baktığınızda avantajı hemen görünmeyebilir. Ancak SAP boyutlandırma yönergeleri baktığınızda, SAP HANA uzantısı düğümleri seçeneği-1 ve 2 seçeneğini kullanarak bir avantajı görebilirsiniz. Örnekler şunlardır:

- SAP HANA boyutlandırma yönergeleri, genellikle bellek olarak double veri hacminin miktarı gerekir. Sık erişimli verileri, SAP HANA örneği çalıştırdığınızda, yüzde 50'yalnızca sahip olduğunuz veya daha az bellek veri ile doldurulan. Bellek geri kalanında, SAP HANA işlerini yapmak için ideal olarak tutulur.
- Bellek, çalışan bir SAP BW veritabanı 2 TB ile bir HANA büyük örneği S192 biriminde anlamına yalnızca 1 TB veri birimi olarak gerekir.
- Seçenek 1 ek bir SAP HANA uzantısı düğümüne kullanırsanız, ayrıca bir S192 HANA büyük örneği SKU, bu veri hacmi için ek bir 2 TB kapasite sağlar. Seçenek 2 Yapılandırması'nda bir ek 4 TB için normal veri hacmi alın. Sık erişimli düğüme karşılaştırıldığında, "sıcak" uzantısı düğümünün tam bellek kapasitesi seçeneği 1 için verileri depolamak için kullanılabilir. İki bellek, veri hacmi seçeneği 2 SAP HANA uzantısı düğüm yapılandırması için kullanılabilir.
- 3 TB kapasite ile verilerinize ve-1. seçenek 1:2 sıcak ile sık oranını edersiniz. 5 TB veri ve 1:4 oranı seçeneği 2 uzantı düğüm yapılandırması ile var.

Bellek kıyasla daha yüksek veri hacmini için soran sıcak veri disk depolama alanında depolanır olasılığı yüksek olan.


## <a name="operations-model-and-responsibilities"></a>İşlem modeli ve sorumlulukları

SAP HANA (büyük örnekler) azure'da ile sağlanan hizmeti, Azure Iaas Hizmetleri ile hizalanır. SAP HANA için en iyi duruma getirilmiş yüklü bir işletim sistemi bir HANA büyük örneği örneğini sahip olursunuz. Azure Iaas Vm'lerle, işletim sistemini sağlamlaştırma, ek yazılım yüklemeniz, HANA yükleme, işletim sistemi ve HANA işletim ve işletim sistemi ve HANA güncelleştirme görevlerin çoğunu gibidir sizin sorumluluğunuzdadır. Microsoft işletim sistemi güncelleştirmeleri veya HANA güncelleştirmeleri size zorlamaz.

![SAP hana (büyük örnekler) azure'da sorumlulukları](./media/hana-overview-architecture/image2-responsibilities.png)

Diyagramda gösterildiği gibi SAP HANA (büyük örnekler) azure'da Iaas sunan çok kiracılı ' dir. Çoğunlukla, sorumluluk bölümünün OS altyapısı sınırında ' dir. Microsoft, hizmet işletim sisteminin çizginin altındaki tüm yönlerini sorumludur. Bu satırın yukarısında hizmetin tüm boyutlarından sorumlu olursunuz. İşletim sistemi sizin sorumluluğunuzdur. Uyumluluk, güvenlik, uygulama yönetimi, temel ve işletim sistemi yönetim için en güncel şirket içi yöntemler uyguluyor kullanmaya devam edebilirsiniz. Sistemleri, ağınızdaki tüm ilgili oldukları gibi görünür.

Bu hizmet, alanları, en iyi sonuçlar için temel alınan altyapı özellikleri kullanmak için Microsoft ile çalışmak için gereken şekilde SAP HANA için optimize edilmiştir.

Aşağıdaki listede her katmanları ve sizin Sorumluluklarınız daha fazla ayrıntı sağlar:

**Ağ**: çalışan SAP HANA büyük örneği damgasında tüm iç ağlar. Sizin sorumluluğunuzdadır, VM'ler depolama, örnekleri (için ölçek genişletme ve diğer işlevler), yatay bağlantısı ve SAP uygulama katmanı barındırıldığı Azure bağlantısı arasında bağlantı erişimi içerir. Ayrıca, olağanüstü durum kurtarma amacıyla çoğaltma için Azure veri merkezleri arasında WAN bağlantısı içerir. Tüm ağlar, Kiracı tarafından bölümlenir ve uygulanan hizmet kalitesi sahip.

**Depolama**: depolama anlık görüntüleri yanı sıra, SAP HANA sunucuları tarafından gerekli olan tüm birimler için sanallaştırılmış bölümlenmiş. 

**Sunucuları**: kiracılar için atanan SAP HANA veritabanlarını çalıştırmak için ayrılmış fiziksel sunuculara. Sunucuları türü miyim sınıfı soyutlanır donanım SKU'lar. Bu sunucu türleri ile sunucu yapılandırmasını toplanır ve başka bir fiziksel donanıma taşınabilir bir fiziksel donanım profilleri, saklanır. Böyle bir (el ile) taşıma profili işlem tarafından Azure hizmet onarımı için biraz karşılaştırılabilir. Type II sınıfı SKU'ları sunucularında, böyle bir özellik sunmamaktadır.

**SDDC**: verileri yönetmek için kullanılan yönetim yazılımı, yazılım tanımlı varlıklar olarak ortalar. Microsoft, Ölçek, kullanılabilirlik ve performansı artırmak için kaynakları havuza sağlar.

**İşletim sistemi**: (SUSE Linux veya Red Hat Linux) seçtiğiniz işletim sistemi çalıştıran sunucularda. İle sağlanan işletim sistemi görüntüleri, SAP HANA çalıştırmak için tek tek Linux satıcı tarafından Microsoft'a sağlanmadı. SAP HANA için iyileştirilmiş görüntü için Linux satıcıyla aboneliğiniz olmalıdır. Görüntüleri işletim sistemi satıcıyla birlikte kaydetmek için sorumlu olursunuz. 

Microsoft tarafından devreden MultiPath noktasından herhangi ek Linux işletim sistemi düzeltme eki uygulama için sorumlu olursunuz. Bu düzeltme eki uygulama olabilecek başarılı bir SAP HANA yüklemesi için gerekli ve belirli kullanıcıların SAP HANA Linux satıcı tarafından dahil olmayan ek paketleri içeren işletim sistemi görüntüleri en iyi duruma getirilmiş. (Daha fazla bilgi için SAP'nin SAP notları ve HANA yükleme belgelerine bakın.) 

Arızasını veya işletim sistemi iyileştirme ve belirli sunucu donanımı göre sürücülerini owing to işletim sistemi yaması için sorumlu olursunuz. Ayrıca güvenlik veya işlev işletim sistemi düzeltme eki için sizin sorumluluğunuzdadır. 

Sizin sorumluluğunuzdadır, izleme ve kapasite planlama de içerir:

- CPU kaynak kullanımı.
- Bellek tüketimi.
- Disk birimleri boş alan, IOPS ve gecikme süresi için ilgili.
- HANA büyük örneği ile SAP uygulama katmanı arasında ağ birim trafiği.

HANA büyük örneği temel altyapısını yedekleme ve geri yükleme, işletim sistemi birimi için işlevsellik sağlar. Bu işlevleri kullanarak da sizin sorumluluğunuzdur.

**Ara yazılım**: SAP HANA örneği, öncelikli olarak. Yönetim, işlemler ve izleme sizin sorumluluğunuzdadır. Sağlanan işlevselliği, yedekleme ve geri yükleme, olağanüstü durum kurtarma amacıyla depolama anlık görüntüleri kullanmak için kullanabilirsiniz. Bu özellikler altyapısı tarafından sağlanır. Sizin Sorumluluklarınız de yüksek kullanılabilirlik veya depolama anlık görüntüleri başarıyla gerçekleştirilip gerçekleştirilmediğini belirlemek için izleme ve bunları yararlanarak bu özelliklere sahip, olağanüstü durum kurtarma tasarımı içerir.

**Veri**: SAP HANA tarafından yönetilen veri ve yedeklemeler gibi diğer veri birimlerinde bulunan dosya veya dosya paylaşımları. Sizin Sorumluluklarınız birimlerde içeriği yönetme ve boş disk alanı izleme içerir. Ayrıca yedeklemeler, birimlerin ve depolama anlık görüntüleri başarıyla yürütüldü izlemekten sorumlu olursunuz. Olağanüstü durum kurtarma sitelerinde veri çoğaltmasının başarılı yürütme Microsoft sorumluluğundadır.

**Uygulamalar:** SAP uygulama örnekleri veya SAP olmayan uygulamalarınızı, söz konusu uygulamaların uygulama katmanı söz konusu olduğunda. Sizin Sorumluluklarınız dağıtım, yönetim, işlemler ve bu uygulamaların izlenmesi içerir. CPU kaynak tüketimi, bellek tüketimi, Azure depolama tüketimi ve sanal ağ içinde ağ bant genişliği tüketimi, kapasite planlaması için sorumlu olursunuz. Siz de Kapasite (büyük örnekler) Azure üzerinde SAP HANA için kaynak tüketimi sanal ağlardan planlama sorumlu olursunuz.

**WAN**: iş yükleri için Azure dağıtımları için şirket içinden bilgisayarlarıyla bağlantıları. HANA büyük örneği olan tüm müşterileri, Azure ExpressRoute için bağlantı kullanın. Bu bağlantının SAP HANA (büyük örnekler) Azure çözüm üzerinde bir parçası değil. Bu bağlantının kurulumu için sorumlu olursunuz.

**Arşiv**: verilerin kopyaları depolama hesaplarında kendi yöntemlerini kullanarak arşivlemek tercih edebilirsiniz. Arşivleme, yönetim, uyumluluk, maliyetleri ve işlemleri gerektirir. Arşiv kopyalarını oluşturma ve yedekleri Azure ve bunları uyumlu bir şekilde depolamak için sorumlu olursunuz.

Bkz: [(büyük örnekler) Azure üzerinde SAP HANA için SLA](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Boyutlandırma

Boyutlandırma HANA büyük örneği için HANA için genel olarak boyutlandırma değerinden farklı değildir. Mevcut ve taşımak için diğer RDBMS HANA için SAP sağlayan çok sayıda mevcut SAP sistemlerinde çalışan bir rapor istediğiniz sistemlerini dağıtılabilir. Veritabanı için HANA taşınırsa, bu raporlar verileri kontrol edip HANA örneği için bellek gereksinimleri hesaplayın. Bu raporları çalıştırmak ve bunların en son düzeltme eklerinin veya sürümler elde etme hakkında daha fazla bilgi için aşağıdaki SAP notları okuyun:

- [SAP notu #1793345 - SAP Suite on HANA ve boyutlandırma](https://launchpad.support.sap.com/#/notes/1793345)
- [Not #1872170 - Suite HANA ve s/4 HANA boyutlandırma rapor üzerinde SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP notu #2121330 - SSS: SAP BW on HANA ve rapor boyutlandırma](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP BW on HANA ve boyutlandırma raporu #1736976 - Not](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP notu #2296290 - yeni rapor, BW on HANA ve için boyutlandırma](https://launchpad.support.sap.com/#/notes/2296290)

Yeşil alan uygulamalar için hızlı Boyutlandırıcı SAP HANA SAP yazılımlarını uygulamasını bellek gereksinimlerini hesaplamak kullanılabilir.

Veri hacmi büyüdükçe, HANA için bellek gereksinimleri artırın. Bu gelecekte olacak şekilde neler olduğunu tahmin yardımcı olması için geçerli bellek tüketimi dikkat edin. Bellek gereksinimlerine bağlı olarak, ardından, isteğe bağlı bir HANA büyük örneği için başka bir SKU eşleyebilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu liste, SAP HANA (büyük örnekler) Azure üzerinde çalıştırmak için gereksinimler birleştirir.

**Microsoft Azure**

- SAP hana (büyük örnekler) azure'da bağlı bir Azure aboneliği.
- Microsoft Premier Destek Sözleşmesi. Azure'da SAP çalıştırma ile ilgili ayrıntılı bilgi için bkz: [SAP destek Not #2015553 – Microsoft Azure üzerinde SAP: Destek önkoşulları](https://launchpad.support.sap.com/#/notes/2015553). 384 ve daha fazla CPU HANA büyük örneği birimleri kullanmayı da Premier genişletmek gerekirse Azure hızlı yanıt'ı dahil etmek için destek sözleşmesi.
- Farkındalık, HANA büyük örneği SAP boyutlandırma alıştırmaya gerçekleştirdikten sonra ihtiyacınız SKU.

**Ağ bağlantısı**

- Şirket içi arasında Azure ExpressRoute: şirket içi veri merkezinizi Azure'a bağlanmak için en az 1 GB/sn bağlantı ISS'niz tarafından sipariş emin olun. 

**İşletim sistemi**

- Lisanslar SAP uygulamaları için SUSE Linux Enterprise Server 12.

   > [!NOTE] 
   > Microsoft tarafından sunulan işletim sistemi SUSE ile kayıtlı değil. Bu abonelik yönetimini örneğine bağlı değil.

- SUSE Linux abonelik yönetimi bir VM'de azure'da dağıtılan aracı. Bu araç, kayıtlı ve sırasıyla SUSE tarafından güncelleştirildi (büyük örnekler) Azure üzerinde SAP HANA için yeteneği sağlar. (HANA büyük örnek veri merkezinde internet erişimi yoktur.) 
- Red Hat Enterprise Linux 6.7 veya SAP HANA için 7.x için lisans.

   > [!NOTE]
   > Microsoft tarafından sunulan işletim sistemi, Red Hat ile kayıtlı değil. Bu, bir Red Hat Abonelik Yöneticisi örneğine bağlı değil.

- Red Hat Abonelik Yöneticisi azure'da bir VM üzerinde dağıtılabilir. Red Hat Abonelik Yöneticisi, kayıtlı ve Red Hat sırasıyla güncelleştirildi (büyük örnekler) Azure üzerinde SAP HANA için yeteneği sağlar. (Hiçbir doğrudan internet erişimini Azure büyük örnek damgası üzerinde dağıtılan kiracıda yoktur.)
- SAP destek sahip olmanızı gerektirir de Linux sağlayıcınız ile sözleşme. Bu gereksinim, HANA büyük örneği ya da Linux Azure üzerinde çalışmasına olgu çözüm kaldırılmaz. Bazı Linux Azure galeri görüntüleri hizmet ücretinin benzemez *değil* HANA büyük örneği çözüm teklife dahil edilen. Bunu SAP Linux dağıtımcısı gerekliliklerini destek sözleşmeleri ile ilgili sizin sorumluluğunuzdur. 
   - SUSE Linux için destek sözleşmeleri gereksinimlerini arayın [SAP notu #1984787 - SUSE Linux Enterprise Server 12: yükleme notları](https://launchpad.support.sap.com/#/notes/1984787) ve [SAP notu #1056161 - SUSE öncelikli destek için SAP uygulamaları](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux için destek içerir ve hizmet güncelleştirmeleri işletim sistemlerini HANA büyük örneği için doğru abonelik düzeylerini olması gerekir. [SAP çözümlerini] için Red Hat önerir Red Hat Enterprise Linux (https://access.redhat.com/solutions/3082481 abonelik. 

Farklı SAP HANA sürümleriyle farklı Linux sürümleri için destek matrisi bkz [SAP notu #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Başvurmak için uyumluluk matrisi HLI bellenim/DRIVER sürümleri ve işletim sistemi [HLI için işletim sistemi yükseltme](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Bu noktada Type II birimleri yalnızca SLES 12 SP2 işletim sistemi sürümü desteklenmiyor. 


**Veritabanı**

- Lisanslar ve yazılım yükleme bileşenleri için SAP HANA (platform veya enterprise edition).

**Uygulamalar**

- Lisanslar ve yazılım yükleme bileşenleri ilgili SAP ve SAP HANA'ya bağlanan tüm SAP uygulamaları için sözleşmeler destekler.
- Lisansları ve herhangi bir SAP olmayan uygulamalar için yazılım yükleme bileşenleri ile ilgili SAP HANA (büyük örnekler) Azure ortamlarında kullanılan ve destek sözleşmeleri ilgili.

**Beceriler**

- Deneyiminizi ve Azure Iaas ve bileşenlerinin bilgisine sahip.
- Deneyiminizi ve bir SAP iş yükünü azure'a dağıtma konusunda bilgi.
- SAP HANA yüklemesi personel sertifikalıdır.
- SAP HANA geçici olarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarım için Mimarı becerileri SAP.

**SAP**

- Beklenir SAP müşterisi olduğunuz ve bir destek sözleşmesi ile SAP.
- İçin özellikle uygulamaları HANA büyük örneği SKU'ları Type II sınıfının SAP ile SAP HANA ve büyük ölçekli ölçek büyütme donanımda nihai yapılandırmalar sürümlerinde başvurun.


## <a name="storage"></a>Depolama

Depolama alanı düzenini (büyük örnekler) Azure üzerinde SAP HANA için SAP HANA tarafından önerilen yönergeleri SAP Klasik dağıtım modeliyle yapılandırılır. Yönergeleri bölümünde belgelendirilen [SAP HANA depolama gereksinimlerini](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi.

Ben sınıf türünün HANA büyük örneği dört kez bellek toplu depolama birimi olarak gelir. HANA büyük örneği birimleri Type II sınıfı için depolama dört kat daha fazla değildir. Birimleri tasarlanmıştır HANA işlem günlüğü yedeklemeleri depolamak için bir birim gelir. Daha fazla bilgi için [yüklemek ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Depolama tahsisi açısından aşağıdaki tabloya bakın. Tablo farklı HANA büyük örneği birimleri ile sağlanan farklı birimler için kaba kapasite listeler.

| HANA büyük örneği SKU | hana/veri | hana/günlük | hana ve paylaşılan | hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S192 | 4.608 GB | 1.024 GB | 1.536 GB | 1.024 GB |
| S192m | 11,520 GB | 1.536 GB | 1,792 GB | 1.536 GB |
| S192xm |  11,520 GB |  1.536 GB |  1,792 GB |  1.536 GB |
| S384 | 11,520 GB | 1.536 GB | 1,792 GB | 1.536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2040 GB |
| S384xm | 16.000 BİN GB | 2.050 GB | 2.050 GB | 2040 GB |
| S384xxm |  20.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S576m | 20.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S576xm | 31.744 GB | 4.096 GB | 2.048 GB | 4.096 GB |
| S768m | 28,000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S768xm | 40.960 GB | 6.144 GB | 4.096 GB | 6.144 GB |
| S960m | 36,000 GB | 4,100 GB | 2.050 GB | 4,100 GB |


Gerçek dağıtılan birimleri, dağıtım ve birim boyutları göstermek için kullanılan bir aracı bağlı olarak değişebilir.

Bir HANA büyük örneği SKU bölüyorsanız, olası bölme parçaları birkaç örnek gibi görünmelidir:

| GB cinsinden bellek bölümü | hana/veri | hana/günlük | hana ve paylaşılan | hana/log/yedekleme |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1,792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Bu boyutları dağıtım ve hacimlerinde aramak için kullanılan araçları biraz göre değişebilir kaba birim sayılardır. Ayrıca var. 2,5 TB gibi diğer bölüm boyutları Önceki bölümleri için kullanılan benzer bir formül ile bu depolama boyutu hesaplanır. "Bölümler" terimi, işletim sistemi, bellek veya CPU kaynaklarının herhangi bir yolla bölümlenir anlamına gelmez. Bu, tek bir HANA büyük örneği birim dağıtmak isteyebilirsiniz farklı HANA örnekleri için depolama bölümleri gösterir. 

Daha fazla depolama alanı gerekebilir. Ek depolama alanı 1 TB birimler satın alarak depolama ekleyebilirsiniz. Bu ek depolama alanı, ek bir birim eklenebilir. Ayrıca, bir veya daha fazla var olan birimler genişletmek için de kullanılabilir. İlk olarak dağıtılan ve genellikle önceki tablolarda tarafından belgelenen şekilde birimlerin boyutunu azaltmak mümkün değildir. Birim adlarını değiştirme veya takma adları mümkün değildir. Daha önce açıklanan depolama birimleri, HANA büyük örneği birimine NFS4 birimleri olarak eklenir.

Depolama anlık görüntüleri yedekleme ve geri yükleme, olağanüstü durum kurtarma amacıyla kullanabilirsiniz. Daha fazla bilgi için [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Başvuru [HLI desteklenen senaryoları](hana-supported-scenario.md) senaryonuz için depolama düzeni Ayrıntılar için.

### <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi
Disklere depolanan gibi verilerin saydam bir şifrelenmesi HANA büyük örneği için kullanılan depolama alanı sağlar. Bir HANA büyük örneği birim dağıtıldığında, bu tür şifrelemeyi etkinleştirebilirsiniz. Dağıtım gerçekleştirildikten sonra şifrelenmiş birimlere da değiştirebilirsiniz. Şifrelenmemiş taşımak şifrelenmiş birimlere saydamdır ve kapalı kalma süresi gerektirmez. 

Ben türüyle sınıfı SKU'ları bir LUN üzerinde depolanan önyükleme birimi şifrelenir. SKU'ları, HANA büyük örneği Type II sınıfı için işletim sistemi yöntemleriyle LUN önyükleme şifrelemeniz gerekir. Daha fazla bilgi için Microsoft Hizmet Yönetimi ekibine başvurun.


## <a name="networking"></a>Ağ

Azure Ağ Hizmetleri mimarisi, SAP HANA büyük örnek uygulamaları başarılı dağıtımı önemli bir bileşenidir. Genellikle, SAP HANA (büyük örnekler) Azure dağıtımlarda, farklı boyutlarda veritabanları, kaynak tüketimine CPU ve bellek kullanımı ile birçok farklı SAP çözümleri ile daha geniş bir SAP ortamının sahiptir. Bu tüm SAP sistemlerini üzerinde SAP HANA dayalı olasıdır. SAP altyapınızı kullanan karma olabilir:

- SAP sistemlerini şirket içinde dağıtılabilir. Kendi boyutları nedeniyle bu sistemleri şu an Azure'da barındırılamaz. SQL Server (veritabanı) olarak çalışır ve Vm'leri sunabileceğinden daha fazla CPU veya bellek kaynağı gerektiren SAP ERP sistemi bir ürün örneğidir.
- SAP HANA tabanlı SAP sistemlerini şirket içinde dağıtılabilir.
- Vm'lerde SAP sistemlerini dağıtılmış. Geliştirme, test, korumalı alan, bu sistemler olabilir veya üretim kaynak tüketimi ve bellek talebe göre Azure'da (VM'ler) içinde başarıyla dağıtabilirsiniz SAP NetWeaver tabanlı uygulamalardan herhangi biri için örnekler. Bu sistemler de veritabanlarını SQL Server gibi temel alabilir. Daha fazla bilgi için [SAP destek Not #1928533-azure'da SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E). Ve bu sistemler gibi SAP HANA veritabanları temel alabilir. Daha fazla bilgi için [SAP HANA sertifikalı ve Iaas platformları](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
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

 

### <a name="additional-virtual-network-information"></a>Ek sanal ağ bilgileri

ExpressRoute için sanal bir ağa bağlanmak için bir Azure ağ geçidi oluşturulmalıdır. Daha fazla bilgi için [ExpressRoute için sanal ağ geçitleri hakkında](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 

Azure dışındaki bir altyapı ya da bir Azure büyük örneği damgasında ExpressRoute ile bir Azure ağ geçidi kullanılabilir. Bir Azure ağ geçidi arasında sanal ağları bağlamak için de kullanılabilir. Daha fazla bilgi için [ağ ağ bağlantısı için Resource Manager PowerShell kullanarak yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu bağlantılar farklı Microsoft Kurumsal kenar yönlendiricilerine gelen sürece en fazla dört farklı ExpressRoute bağlantıları Azure ağ geçidine bağlanabilir. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Her ikisi de kullanım örnekleri için bir Azure ağ geçidi sağlar aktarım hızı farklılık gösterir. Daha fazla bilgi için [VPN Gateway hakkında](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bir ExpressRoute bağlantısı kullanarak 10 GB/sn ile bir sanal ağ geçidi ulaşabileceği en yüksek aktarım var. Bir sanal ağda bulunan bir VM ile bir sistem arasında dosya kopyalama şirket içi (bir tek kopya akış) olarak farklı ağ geçidi SKU'ları tam verimini elde etmez. Sanal ağ geçidi tam bant yararlanmak için birden çok akışı kullanın. Veya tek bir dosyayı paralel akışlarındaki farklı dosya kopyalamanız gerekir.


### <a name="networking-architecture-for-hana-large-instance"></a>HANA büyük örneği için ağ mimarisi
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

Karşılaşılan ağ gecikme sürelerini VM'ler ve HANA büyük örneği arasında birimleri bir tipik VM VM ağ gidiş dönüş gecikmesi yüksek olabilir. Bağımlı Azure bölgesi, ölçülen değerleri aşağıda ortalama olarak sınıflandırılan 0,7 ms gidiş dönüş gecikmesi da aşabilir [SAP notu #1100926 - SSS: ağ performansı](https://launchpad.support.sap.com/#/notes/1100926/E). Bununla birlikte, müşteriler SAP HANA büyük örneği başarıyla üzerinde SAP HANA tabanlı üretim SAP uygulamaları dağıtın. Raporu harika iyileştirmeleri HANA büyük örneği birimleri kullanarak SAP HANA'da SAP uygulamalarını çalıştırarak dağıtan müşteriler. Azure HANA büyük örneği İş süreçlerinizi kapsamlı olarak test emin olun.
 
VM'ler ve HANA büyük örneği arasındaki belirleyici ağ gecikme süresi sağlamak için seçtiğiniz sanal ağ geçidi SKU'su gereklidir. Şirket içi VM'ler arasındaki trafiği desenlerinden farklı olarak, HANA büyük örneği ile VM'ler arasındaki trafik desenini aktarılacak istekleri ve veri birimlerini küçük ancak yüksek ani artışlara geliştirebilirsiniz. İyi tür artışlarını işlemek için UltraPerformance ağ geçidi SKU'SUNUN kullanılmasını öneririz. HANA büyük örneği SKU'ları Type II sınıfı için bir sanal ağ geçidi olarak UltraPerformance ağ geçidi SKU'sunu kullanımını zorunludur.

> [!IMPORTANT] 
> SAP uygulama ve veritabanı katmanları arasındaki genel ağ trafiğini göz önünde bulundurulduğunda, yalnızca HighPerformance veya UltraPerformance ağ geçidi SKU'ları sanal ağlar için SAP HANA (büyük örnekler) azure'da bağlamak için desteklenir. HANA büyük örneği Type II SKU'lara için bir sanal ağ geçidi yalnızca UltraPerformance ağ geçidi SKU'sunu desteklenir.



### <a name="single-sap-system"></a>Tek SAP sistemine

Daha önce gösterilen şirket içi altyapınızı Azure'a ExpressRoute aracılığıyla bağlanır. ExpressRoute bağlantı hattı, bir enterprise sınır yönlendiricisi bağlar. Daha fazla bilgi için [Expressroute'a teknik genel bakış](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Rota gerçekleştikten sonra Azure omurgası bağlanır ve tüm Azure bölgelerinde erişilebilir.

> [!NOTE] 
> Azure'da SAP ortamlarını çalıştırmak için SAP ortamı bir Azure bölgesinde en yakın enterprise sınır yönlendiricisi bağlayın. Azure büyük örnek Damgalar Azure Iaas ve büyük örnek Damgalar VM'ler arasındaki ağ gecikmesini en aza indirmek için adanmış Kurumsal edge yönlendirici cihazları aracılığıyla bağlanır.

SAP uygulama örneklerini barındıran sanal makineler için sanal ağ geçidi ExpressRoute işlem hattına bağlı. Büyük örnek damga bağlanmaya yönelik adanmış bir ayrı enterprise sınır yönlendiricisi aynı sanal ağa bağlanır.

Bu sistem, tek bir SAP sistemiyle basit bir örnektir. SAP uygulama katmanı, Azure'da barındırılır. SAP HANA (büyük örnekler) Azure üzerinde SAP HANA veritabanı çalışır. Sanal ağ ağ geçidi bant genişliğini 2 GB/sn veya 10 GB/sn aktarım hızının bir performans sorunu temsil etmez varsayılır.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Birden çok SAP sistemlerini veya büyük SAP sistemlerini

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


### <a name="routing-in-azure"></a>Azure'da yollar

Üç ağ yönlendirme konuları (büyük örnekler) Azure üzerinde SAP HANA için önemlidir:

* SAP HANA (büyük örnekler) Azure üzerinde Vm'leri ve şirket içi doğrudan özel ExpressRoute bağlantısı üzerinden erişilebilir. Şirket içi HANA büyük örneği birimleri, doğrudan erişim için Microsoft tarafından sunulan hemen mümkün değildir. SAP HANA büyük örneği için kullanılan geçerli Azure ağ mimarisi geçişli yönlendirme kısıtlamaları kaynaklanır. SAP HANA veritabanına bağlanılamıyor, bazı yönetim istemcileri ve doğrudan erişim, SAP çözümü şirket içinde çalışan Yöneticisi gibi tüm uygulamalar.

* Olağanüstü durum kurtarma için iki farklı Azure bölgelerindeki dağıtılan HANA büyük örneği birimi varsa aynı geçici yönlendirme kısıtlamaları geçerli olur. Diğer bir deyişle, IP adresleri (örneğin, ABD Batı) bir bölgedeki bir HANA büyük örneği birimin başka bir bölgede (örneğin, ABD Doğu) dağıtılan bir HANA büyük örneği birim yönlendirilmeyen. Bu kısıtlama, bölgeler arasında eşleme veya çapraz bağlanma HANA büyük örneği birimleri için sanal ağları birbirine bağlama ExpressRoute bağlantı hatları Azure ağ kullanımını bağımsızdır. Şekil "Kullanımı HANA büyük örneği birimleri birden çok bölgede." bölümünde bir grafik gösterimi için bkz. Dağıtılmış mimari ile birlikte sunulur, bu kısıtlama, HANA sistem çoğaltması olağanüstü durum kurtarma işlevi olarak anında kullanımını engeller.

* SAP HANA (büyük örnekler) Azure birimlerdeki gönderdiğiniz sunucu IP havuzu adres aralığı atanan bir IP adresi vardır. Daha fazla bilgi için [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu IP adresi, Azure aboneliği ve azure'da HANA (büyük örnekler) için sanal ağları birbirine bağlayan ExpressRoute aracılığıyla erişilebilir. Sunucu IP havuzu adres aralığı donanım birimi doğrudan atanır / IP adresi atanır. Sahip *değil* Bu çözüm ilk dağıtımları durumda olduğu gibi NAT artık atanmış. 

> [!NOTE] 
> İlk iki liste öğelerini açıklandığı gibi geçici akışındaki kısıtlamayı çözmek için ek bileşenler yönlendirme için kullanın. Kısıtlamayı çözmek için kullanılan bileşenleri olabilir:

> * Bir ters ve ondan veri yönlendirmek için proxy. Örneğin, F5 BIG-IP, NGINX ile trafiği bir sanal güvenlik duvarı/trafik yönlendirme çözümü olarak azure'da dağıtılan Yöneticisi.
> * Kullanarak [IPTables kurallarını](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) HANA büyük örneği birimleri farklı bölgelerde veya şirket içi konumlar ve HANA büyük örneği birimleri arasında yönlendirme etkinleştirmek için bir Linux VM'de.

> Uygulama ve üçüncü taraf içeren özel çözümler için destek ağ Gereçleri veya IPTables Microsoft tarafından sağlanmayan unutmayın. Destek, kullanılan bileşenin bir satıcı veya entegratörü tarafından sağlanmalıdır. 

### <a name="internet-connectivity-of-hana-large-instance"></a>Internet bağlantısı HANA büyük örneği
HANA büyük örneği mu *değil* doğrudan internet bağlantısına sahip. Örnek olarak, bu sınırlama, işletim sistemi görüntüsü işletim sistemi satıcıyla birlikte doğrudan kaydetme olanağı kısıtlayabilir. SUSE Linux Enterprise Server abonelik yönetimi aracı yerel sunucu ya da Red Hat Enterprise Linux Abonelik Yöneticisi ile çalışmak gerekebilir.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>VM'ler ve HANA büyük örnek arasındaki veri şifrelemesi
VM'ler ve HANA büyük örneği arasında aktarılan veriler şifrelenmez. Ancak, yalnızca exchange için JDBC/ODBC tabanlı uygulamalar ve HANA DBMS yan arasında trafiği şifreleme etkinleştirebilirsiniz. Daha fazla bilgi için [SAP tarafından bu belgeleri](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>Birden çok bölgede HANA büyük örneği birimleri kullanın

SAP HANA (büyük örnekler) dışında birden çok Azure bölgesine olağanüstü durum kurtarma için azure'a dağıtmak için neden olabilir. Belki de her bölgelerdeki farklı sanal ağlarda bulunan dağıtılan VM'lerin HANA büyük örneği erişmek istediğiniz. Farklı bir HANA büyük örneği birimleri için atanan IP adreslerini, kendi ağ geçidi örneklerine üzerinden doğrudan bağlı olan sanal ağların ötesinde yayılmaz. Sonuç olarak, küçük bir değişiklik için sanal ağ tasarımı sunulmuştur. Bir sanal ağ geçidi farklı bir kurumsal kenar yönlendiricilerine dışında dört farklı ExpressRoute devreleri başa çıkabilir. Büyük örnek Damgalar birine bağlı her bir sanal ağ başka bir Azure bölgesinde büyük örneği damgasında bağlanabilir.

![Farklı Azure bölgelerindeki Azure büyük örnek Damgalar bağlı olan sanal ağı](./media/hana-overview-architecture/image8-multiple-regions.png)

Şekil nasıl iki bölgeleri farklı sanal ağlarda (büyük örnekler) Azure üzerinde SAP hana'ya bağlanmak için kullanılan iki farklı ExpressRoute devreleri için Azure her iki bölgede de bağlandığını gösterir. Yeni çıkan dikdörtgen kırmızı çizgiler bağlantılardır. Bu sanal ağlardan birine çalıştırılan sanal makinelerin sanal ağların dışında bu bağlantıları ile her iki bölgede dağıtılan farklı HANA büyük örneği birimlerinin erişebilirsiniz. Şekilde gösterildiği gibi iki Azure bölgesi için iki ExpressRoute bağlantıları şirket içinde sahip olduğunuz varsayılır. Bu düzenleme, olağanüstü durum kurtarma nedenlerle önerilir.

> [!IMPORTANT] 
> Birden çok ExpressRoute bağlantı hattına kullandıysanız, doğru yönlendirmeyi, trafiği emin olmak için AS yolu eklenmesini ve yerel tercih BGP ayarları kullanılmalıdır.


