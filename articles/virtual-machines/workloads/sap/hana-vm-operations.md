---
title: "Azure üzerinde SAP HANA işlemler | Microsoft Docs"
description: "Azure sanal makinelerinde dağıtılan SAP HANA sistemleri için işlemler Kılavuzu."
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: juergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2017
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8ddfd5e2ee57d79fecacdc648af9264b6c95240
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA üzerinde Azure işlemler Kılavuzu
Bu belgede işletim Azure yerel sanal makinelerde (VM'ler) dağıtılan SAP HANA sistemleri için yönergeler sağlanmaktadır. Bu belge aşağıdaki içeriği içerir standart SAP belgeleri değiştirmek üzere tasarlanmamıştır:

- [SAP Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP yükleme kılavuzları](https://service.sap.com/instguides)
- [SAP notları](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenlerin temel bilgi gerekir:

- [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Depolama](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

SAP NetWeaver ve Azure ile ilgili diğer SAP bileşenleri hakkında daha fazla bilgi için bkz: [Azure üzerinde SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) bölümünü [Azure belgelerine](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Temel Kurulum konuları
Aşağıdaki bölümlerde Azure vm'lerinde SAP HANA sistemlerini dağıtmak için temel kurulum konuları açıklanmaktadır.

### <a name="connect-into-azure-virtual-machines"></a>Azure sanal makinelerine bağlanmak
Açıklandığı gibi [Planlama Kılavuzu Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), Azure sanal makinelerini bağlamak için iki temel yöntem vardır:

- SAP HANA çalışan VM veya atlama VM üzerinde ortak uç noktaları ve Internet üzerinden bağlanır.
- Üzerinden bir [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) veya Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Siteden siteye bağlantı VPN ya da ExpressRoute aracılığıyla üretim senaryoları için gereklidir. Bu tür bir bağlantı SAP yazılım burada kullanılıyor üretim senaryolarına akışı üretim dışı senaryoları için de gereklidir. Aşağıdaki resimde, siteler arası bağlantı örneği gösterilmiştir:

![Siteler arası bağlantı](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Azure VM türlerini seçin
Üretim senaryoları için kullanılabilecek Azure VM türleri listelenen [IAAS SAP belgelerine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Üretim dışı senaryoları için çok çeşitli yerel Azure VM türleri kullanılabilir.

>[!NOTE]
>Üretim dışı senaryoları için listelenen VM türlerini kullanan [SAP Not #1928533](https://launchpad.support.sap.com/#/notes/1928533).

Azure'da Vm'leri kullanarak dağıtın:

- Azure portalı.
- Azure PowerShell cmdlet'leri.
- Azure CLI.

Ayrıca, bir tam yüklü SAP HANA platformda Azure VM Hizmetler aracılığıyla dağıtabilirsiniz [SAP bulut platformu](https://cal.sap.com/). Yükleme işlemi açıklanan [dağıtmak SAP S/4HANA veya BW/4HANA Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choose-azure-storage-type"></a>Azure depolama türü seçin
Azure SAP HANA çalışan Azure VM'ler için uygun olan iki tür depolama sağlar:

- [Standart Azure depolama](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure, Azure standart ve Premium depolama VHD için iki dağıtım yöntemleri sunar. Genel senaryo izin veriliyorsa, yararlanmak [Azure disk yönetilen](https://azure.microsoft.com/services/managed-disks/) dağıtımları.

Depolama türlerini ve SLA'ları listesi için gözden [yönetilen diskler için Azure belgelerine](https://azure.microsoft.com/pricing/details/managed-disks/).

Azure Premium diskleri /hana/data ve /hana/log birimleri için önerilir. Bir LVM RAID birden çok Premium depolama diski oluşturun ve RAID /hana/data olarak ve /hana/log birimler kullanın.

Aşağıdaki tabloda müşteriler sık kullandığınız VM türleri bir ana bilgisayara SAP HANA Azure vm'lerinde gösterir:

| VM SKU | RAM | / hana/veri ve/hana/günlük<br /> LVM veya MDADM şeritli | / hana/paylaşılan | / root birim | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S10 |
| E32v3 | 256 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1 TB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1.7 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128ms | 3.8 TB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 5 x S30 |


### <a name="set-up-azure-virtual-networks"></a>Azure sanal ağları ayarlayın
Azure VPN ya da ExpressRoute aracılığıyla içine siteden siteye bağlantı varsa, en az bir olmalıdır [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) VPN ya da ExpressRoute bağlantı hattına sanal bir ağ geçidi üzerinden bağlı. Azure sanal ağındaki bir alt ağdaki sanal ağ geçidi yaşar. SAP HANA yüklemek için sanal ağ içindeki iki ek alt ağlar oluşturun. Bir alt ağ SAP HANA örnekleri çalıştırmak için sanal makineleri barındırır. Diğer alt Jumpbox veya yönetim VM'ler SAP HANA Studio ya da diğer yönetim yazılımı barındırmak için çalışır.

SAP HANA çalıştırmak için sanal makineleri yüklediğinizde, sanal makineleri gerekir:

- Yüklü iki sanal NIC: yönetimi alt ağına bağlanmak için bir NIC ve Azure VM SAP HANA örneğinde şirket içi ağ veya diğer ağlara bağlanmak için bir NIC.
- Her iki sanal NIC için dağıtılan statik özel IP adresleri.

IP adresleri atama için farklı yöntemler genel bakış için bkz: [IP adresi türleri ve Azure ayırma yöntemleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

[Azure ağ güvenlik grupları (Nsg'ler)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) SAP HANA örneği veya Jumpbox yönlendirilen trafiği yönlendirmek için kullanılır. SAP HANA alt ağı ve yönetim alt ağ için Nsg'ler ilişkilendirilir.

Aşağıdaki resimde bir kaba dağıtım şema genel bir bakış için SAP HANA gösterir:

![SAP HANA kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking.PNG)


SAP HANA azure'da siteden siteye bağlantı dağıtmak için bir ortak IP adresi olsa SAP HANA örneği erişin. IP adresi Jumpbox VM çalıştıran Azure VM'ye atanması gerekir. Temel Bu senaryoda, ana bilgisayar adları çözümlemek için Azure yerleşik DNS hizmetleri üzerinde dağıtım kullanır. Genel kullanıma yönelik IP adreslerini kullanıldığı daha karmaşık bir dağıtımda, Azure yerleşik DNS hizmetleri özellikle önemlidir. Azure Nsg'ler açık bağlantı noktalarını ve genel kullanıma yönelik IP adreslerine sahip varlıklar Azure alt ağlarla içinde bağlanıp IP adres aralıklarını sınırlamak için kullanın. Aşağıdaki resimde bir siteden siteye bağlantı SAP HANA dağıtmak için bir kaba şema gösterilmektedir:
  
![SAP HANA kaba dağıtım şemasını bir siteden siteye bağlantı olmadan](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>SAP HANA Azure vm'lerinde dağıtmak için işlemler
Aşağıdaki bölümlerde Azure vm'lerinde SAP HANA sistemleri dağıtımıyla ilgili işlemleri bazıları açıklanmaktadır.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Yedekleme ve geri yükleme işlemleri Azure vm'lerinde
Aşağıdaki belgeler ve SAP HANA dağıtımınızı geri açıklanmaktadır:

- [SAP HANA yedeklemesine genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA dosya düzeyinde yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA depolama anlık görüntü Kıyaslama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Başlangıç ve SAP HANA içeren sanal makineleri yeniden başlatın
Belirgin Azure genel bulutunda yalnızca, bilgi işlem dakika için ücret ödersiniz özelliğidir. Örneğin, SAP HANA çalışmakta olan bir VM kapatma, bu süre boyunca yalnızca depolama maliyetleri için fatura. İlk dağıtımınızda Vm'leriniz için statik IP adresleri belirttiğinizde başka bir özellik kullanılabilir. SAP HANA sahip bir VM yeniden başlattığınızda, VM önceki IP adresleriyle yeniden başlatır. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAP uzak desteği SAProuter kullanın
Şirket içi konumları ve Azure arasında bir siteden siteye bağlantı varsa ve SAP bileşenleri çalıştırıyorsanız, SAProuter büyük olasılıkla zaten kullanıyorsunuz. Bu durumda, uzak desteği için aşağıdaki öğeleri tamamlayın:

- Özel ve statik IP adresi VM barındıran SAP HANA SAProuter yapılandırmasında korur.
- NSG 3299 TCP/IP bağlantı noktası üzerinden trafiğe izin verecek şekilde HANA VM barındıran alt ağın yapılandırın.

Internet üzerinden Azure bağlanacağınız ve SAP HANA ile VM için bir SAP yönlendirici yoksa, bileşen yüklemeniz gerekir. Yönetim alt ağda ayrı bir VM'de SAProuter yükleyin. Aşağıdaki resimde bir siteden siteye bağlantı olmadan ve SAProuter SAP HANA dağıtmak için bir kaba şema gösterilmektedir:

![Bir siteden siteye bağlantı ve SAProuter SAP HANA için dağıtım şema kaba](media/hana-vm-operations/hana-simple-networking3.PNG)

Ayrı bir VM yer alan ve Jumpbox VM'nizi SAProuter yüklediğinizden emin olun. Ayrı VM statik bir IP adresi olmalıdır. SAP tarafından barındırılan SAProuter, SAProuter bağlanmak için bir IP adresi için SAP başvurun. (SAP tarafından barındırılan SAProuter karşılık gelen VM üzerinde yüklediğiniz SAProuter örneğinin olur.) SAP IP adresinden SAProuter örneğinizi yapılandırmak için kullanın. Yapılandırma ayarları, yalnızca gerekli bağlantı noktası TCP bağlantı noktası 3299 ' dir.

Kurmak ve bakımını yapmak SAProuter üzerinden uzaktan destek bağlantılar hakkında daha fazla bilgi için bkz: [SAP belgelerine](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>SAP HANA Azure yerel vm'lerde ile yüksek kullanılabilirlik
SUSE Linux 12 SP1 çalıştırıyorsanız veya daha sonra STONITH aygıtlarla Pacemaker küme kurup varsa. Zaman uyumlu çoğaltma HANA sistem çoğaltma ve otomatik yük devretme kullanan bir SAP HANA yapılandırması ayarlamak için cihazları kullanın. Kurulum yordamının hakkında daha fazla bilgi için bkz: [SAP HANA yüksek kullanılabilirlik Azure sanal makinelerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).
