---
title: SAP hana (büyük örnekler) azure'da bir mimari | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da dağıtma mimarisi.
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
ms.openlocfilehash: 80ef63cdd9de8cb2340fe15d761402bb9f00fae9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60795941"
---
# <a name="sap-hana-large-instances-architecture-on-azure"></a>Azure'da SAP HANA (büyük örnekler) mimarisi

Yüksek düzeyde, SAP HANA (büyük örnekler) Azure çözüm üzerinde bulunan VM'ler SAP uygulama katmanı vardır. Veritabanı katmanı, bir büyük örneği damgasında Azure Iaas bağlı olduğu aynı Azure bölgesinde bulunan SAP TDI yapılandırılmış donanımda bulunur.

> [!NOTE]
> SAP DBMS katmanı ile aynı Azure bölgesinde SAP uygulama katmanı dağıtın. Bu kural, azure'da SAP iş yükleri hakkında yayımlanan bilgi iyi belgelenmiştir. 

Sanallaştırılmamış, çıplak metal, SAP HANA veritabanı için yüksek performanslı sunucusu olan bir SAP TDI sertifikalı donanım yapılandırması, SAP HANA (büyük örnekler) azure'da genel mimarisi sağlar. Ayrıca, özelliği ve kaynakları, ihtiyaçlarınızı karşılamak SAP uygulama katmanı için Azure'nın esnekliği sağlar.

![SAP HANA (büyük örnekler) Azure ile ilgili mimari genel bakış](./media/hana-overview-architecture/image1-architecture.png)

Mimarisini üç bölümlere ayrılmıştır:

- **doğru**: Son kullanıcıların erişebilmesi için verileri farklı uygulamalarını çalıştıran bir şirket içi altyapı merkezleri gösterildiği gibi SAP uygulamaları LOB. İdeal olarak, bu altyapı ile azure'a bağlı sonra şirket [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Merkezi**: Azure Iaas gösterir ve bu durumda, SAP ve SAP HANA DBMS sistemi olarak kullanan diğer uygulamaları barındırmak için sanal makinelerin kullanın. VM'ler bellekle işlevi daha küçük HANA örnekleri kendi uygulama katmanı ile birlikte Vm'lere dağıtılır. Sanal makineler hakkında daha fazla bilgi için bkz. [sanal makineler](https://azure.microsoft.com/services/virtual-machines/).

   Azure Ağ Hizmetleri, SAP sistemlerini diğer uygulamalara sanal ağlar ile birlikte gruplandırmak için kullanılır. Şirket içi sistemleri de (büyük örnekler) Azure üzerinde SAP HANA için bu sanal ağları bağlayın.

   SAP NetWeaver uygulamalar ve Azure'da çalıştırmak için desteklenen veritabanları için bkz. [SAP destek Not #1928533-azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533). Azure'da SAP çözümleri dağıtma hakkında daha fazla bilgi için bkz:

  -  [Windows sanal makinelerinde SAP kullanma](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Azure sanal makinelerinde SAP çözümlerini kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Sol**: SAP HANA TDI sertifikalı donanım Azure büyük örneği damgasında gösterir. HANA büyük örneği birimleri aboneliğinizin sanal ağlara bağlantı şirket içinden azure'a aynı teknolojiyi kullanarak bağlanır.

Azure büyük örnek damgası aşağıdaki bileşenleri birleştirir:

- **Bilgi işlem**: Gerekli hesaplama yeteneği sağlar ve SAP HANA sertifikalı olan Intel Xeon E7-8890v3 veya Intel Xeon E7-8890v4 işlemcilerde tabanlı sunucu.
- **Ağ**: Bilgi işlem, depolama ve LAN bileşenleri eşitliyor birleşik yüksek hızlı ağ fabric.
- **Depolama**: Bir birleşik ağ yapısı erişilen bir depolama altyapısı. Belirli bir SAP HANA, dağıtılan Azure (büyük örnekler) yapılandırması hakkında sağlanan belirli depolama kapasitesi bağlıdır. Daha fazla depolama kapasitesi, ek bir aylık ücret ödemeden kullanılabilir.

Büyük örneği damgasında çok kiracılı altyapısında müşteriler yalıtılmış Kiracı dağıtılır. Kiracı dağıtımı sırasında bir Azure aboneliği içinde Azure kaydınızın adı. Bu Azure aboneliği, HANA büyük örneği karşı faturalandırılır bilgisayardır. Bu kiracıları 1:1 ilişki Azure aboneliğine sahip. Bir ağ için bir Azure bölgesinden farklı Azure aboneliklere ait sanal ağlar farklı bir kiracıda dağıtılmış bir HANA büyük örneği birim erişmek mümkündür. Bu Azure abonelikleri için aynı Azure kayıt ait olmalıdır. 

SAP HANA (büyük örnekler) azure'da vm'lerle olduğu gibi birden çok Azure bölgesinde sunulur. Olağanüstü durum kurtarma özellikleri sunmak için katılım seçebilirsiniz. Farklı büyük örnek Damgalar bir siyasi coğrafi bölge içinde birbirlerine bağlı. Örneğin, HANA büyük örneği damgaları ABD Batı hem de ABD Doğu, olağanüstü durum kurtarma çoğaltması için bir adanmış ağ bağlantısı üzerinden bağlanır. 

Farklı VM türler ile Azure sanal makineler arasında yalnızca seçebileceğiniz gibi farklı SKU'ları, HANA büyük SAP HANA farklı iş yükü türleri için uygun örnek arasından seçim yapabilirsiniz. SAP, bellek için işlemci yuvasında oranları Intel işlemci nesillerde göre değişen iş yükleri için geçerlidir. Aşağıdaki tabloda sunulan SKU türleri gösterilmektedir.

Kullanılabilir SKU'lar bulabilirsiniz [HLI için kullanılabilir SKU'ları](hana-available-skus.md).

**Sonraki adımlar**
- Başvuru [SAP HANA (büyük örnekler) ağ mimarisi](hana-network-architecture.md)