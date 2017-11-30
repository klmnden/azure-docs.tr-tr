---
title: "Azure üzerinde SAP HANA işlemler | Microsoft Docs"
description: "SAP HANA işlemleri Azure yerel vm'lerde"
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
ms.openlocfilehash: 0328bdc40429e1e82a76f290f5bde39089db0a9d
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA üzerinde Azure işlemler Kılavuzu
Bu kılavuz, Azure sanal makinelerinde dağıtılan SAP HANA sistemleri işletim kılavuzu sağlar. Bu belge herhangi bir standart SAP belgeleri değiştirmek üzere tasarlanmamıştır. SAP kılavuzları ve notları aşağıdaki konumlarda bulunabilir:

- [SAP Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP yükleme kılavuzları](https://service.sap.com/instguides)
- [SAP notuna göz atın](https://sservice.sap.com/notes)

Farklı Azure bileşenlerinin üzerinde temel bilgiye sahip koşuldur:

- [Azure sanal makineler](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Depolama](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

SAP NetWeaver ek belgeler ve Azure ile ilgili diğer SAP bileşenlerini bulunabilir [Azure üzerinde SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) bölümünü [Azure belgelerine](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Temel Kurulum konuları
### <a name="connecting-into-azure"></a>Azure'a bağlanma
[Azure sanal makineleri planlama ve uygulama için SAP NetWeaver] açıklandığı gibi [Planlama Kılavuzu], Azure sanal makinelerine bağlanmak için iki temel yöntem vardır. 

- Internet ve genel Uç noktalara Atlamak VM veya SAP HANA çalışan VM aracılığıyla bağlanma
- Üzerinden bağlanan bir [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) veya Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

Üretim için senaryolar veya senaryolar burada SAP yazılım ile birlikte üretim senaryolarında içine akışı üretim dışı senaryoları, siteden siteye bağlantı VPN ya da ExpressRoute aracılığıyla bu grafikte gösterildiği gibi olması gerekir:

![Siteler arası bağlantı](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choice-of-azure-vm-types"></a>Azure VM türleri seçimi
Üretim senaryoları için kullanılabilecek azure VM türleri Aranan [burada](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Üretim dışı senaryoları için çok çeşitli yerel Azure sanal makineleri seçebilirsiniz. Ancak kendiniz olan VM türlerini kısıtlamalısınız [SAP Not #1928533](https://launchpad.support.sap.com/#/notes/1928533). Azure'da bu VM dağıtımı aracılığıyla gerçekleştirilebilir:

- Azure portalına
- Azure Powershell cmdlet'leri
- Azure CLI

Azure sanal makine Hizmetler aracılığıyla tam yüklü SAP HANA platformuna de dağıtabilirsiniz [SAP bulut platformu](https://cal.sap.com/) belirtildiği gibi [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choice-of-azure-storage"></a>Azure storage seçimi
SAP HANA çalıştıran iki ana depolama türlerini Azure VM'ler için uygun Azure sağlar

- [Standart Azure depolama](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure, Azure standart ve Premium depolama VHD için iki dağıtım yöntemleri sunar. Genel senaryo izin veriliyorsa, yararlanmak için önerilir [Azure yönetilen diski](https://azure.microsoft.com/services/managed-disks/) dağıtımları.

Tam depolama türleri ve bu depolama türlerini geçici SLA, denetlemek [bu belgeleri](https://azure.microsoft.com/pricing/details/managed-disks/)

Azure Premium diskleri /hana/data ve /hana/log birimleri için kullanılması önerilir. Birden çok Premium depolama diskleri LVM RAID oluşturmak ve bu RAID /hana/data olarak ve /hana/log birimler kullanmak için desteklenir.

Müşteriler kadarki SAP HANA Azure vm'lerinde barındırmak için kullandığınız farklı ortak VM türleri için olası bir yapılandırma gibi görünebilir:

| VM SKU | RAM | / hana/veri ve/hana/günlük<br /> LVM veya MDADM şeritli | / hana/paylaşılan | / root birim | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S10 |
| E32v3 | 256GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1TB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1.7 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128ms | 3.8 TB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 5 x S30 |


### <a name="azure-networking"></a>Azure ağı
Bir VPN ya da ExpressRoute siteden siteye bağlantı Azure içine olduğunu varsaydığımızdan en az bir olurdu [Azure VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) VPN ya da ExpressRoute bağlantı hattına sanal bir ağ geçidi üzerinden bağlı. Azure Vnet içindeki bir alt ağdaki sanal ağ geçidi yaşar. HANA yüklemek için sanal ağ içindeki başka bir iki alt ağlar oluşturursunuz. SAP HANA örnekleri ve nihai Jumpbox veya Yönetim sanal makine çalıştıran başka bir alt ağ çalıştıran sanal makine konakları SAP HANA Studio ya da diğer yönetim yazılımı barındırabilen bir alt ağ.
HANA çalışması gerektiğini VM'ler yüklediğinizde, sanal makineleri olmalıdır:

- Bir yönetim alt ağına bağlanan yüklü iki sanal NIC ve bir NIC şirket içi veya diğer ağlara ya da Azure VM'deki SAP HANA örneğine bağlanmak için kullanılır.
- Özel statik IP adresleri için her iki vNICs dağıtılan

Bir genel IP adresi ataması farklı olasılıklar bulunabilir [burada](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Trafik için doğrudan SAP HANA örneği veya jumpbox yönlendirme tarafından yönlendirilen [Azure ağ güvenlik grupları](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg) HANA alt ağı ve yönetim alt ağ için ilişkili olan.

Genel kaba dağıtım şema gibi görünür:

![SAP HANA kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking.PNG)


Bir site siteye (VPN veya Azure içine ExpressRoute) gerek kalmadan SAP HANA azure'da'ni yalnızca dağıtırsanız, SAP HANA örneği Jumpbox VM çalıştıran Azure VM'ye atanan genel bir IP adresi olsa erişir. En basit durumda, ayrıca Azure yerleşik DNS Hizmetleri'ni ana bilgisayar adları çözümlemek amacıyla kullanır. Özellikle, genel kullanıma yönelik IP adresleri kullanarak, Azure ağ güvenlik grupları açık bağlantı noktalarını sınırlandırmak için kullanmak istediğiniz veya IP adresi aralıkları varlıklar genel kullanıma yönelik IP adresleri ile çalışan Azure alt ağlar uygulamasına bağlanmak için verilir. Böyle bir dağıtımda şeması gibi görünebilir:
  
![SAP HANA kaba dağıtım şemasını siteden siteye bağlantı olmadan](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations"></a>İşlemler
### <a name="backup-and-restore-operations-on-azure-vms"></a>Yedekleme ve geri yükleme işlemlerinin Azure vm'lerinde
SAP HANA yedekleme ve geri yükleme olanaklarını bu belgelerde belgelenmiştir:

- [SAP HANA Yedekleme'ye Genel Bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA dosya düzeyi yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA depolama anlık görüntü Kıyaslama](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)



### <a name="start-and-restart-of-vms-containing-sap-hana"></a>Başlangıç ve SAP HANA içeren sanal makineleri yeniden
Azure genel bulut gücü harcama işlem dakika için yalnızca ücretlendirilen olgu biridir. SAP HANA içinde çalışan'yle bir VM'yi kapatın, bu süre boyunca yalnızca depolama maliyetlerini faturalandırılır anlamına gelir. VM ile SAP HANA içinde başladığınızda yeniden VM yeniden başlatma gittiği ve (statik IP adresleriyle dağıtıldıysa) aynı IP adresine sahip olacağını. 


### <a name="saprouter-enabling-sap-remote-support"></a>SAPRouter etkinleştirme SAP uzak desteği
Şirket içi konumları ve Azure arasında SAP bileşenleri zaten çalıştırmadan siteden siteye bağlantınız varsa, zaten SAProuter zaten çalıştırmanızı yüksek oranda olasıdır. Bu durumda, bir şey yok Azure'da dağıtmak SAP HANA örneğiyle yapmanız gerekir. Özel ve statik IP adresi VM barındıran HANA SAPRouter yapılandırmasında korumak ve alt ağ barındırma NSG sahip dışında (bağlantı noktası TCP/IP bağlantı noktası izin verilen 3299 üzerinden trafik) HANA VM uyarlanan.

SAP HANA dağıtıyorsanız ve Internet üzerinden Azure bağlanmak ve SAP HANA ile bir VM çalıştıran Vnet yüklü bir SAP yönlendirici yoksa, aşağıda gösterildiği gibi yönetim alt ağda ayrı bir VM'de SAPRouter yüklemeniz gerekir :


![SAP HANA kaba dağıtım şemasını siteden siteye bağlantı ve SAPRouter olmadan](media/hana-vm-operations/hana-simple-networking3.PNG)

Ayrı bir VM yer alan ve Jumpbox VM'nizi SAPRouter yüklemeniz gerekir. Ayrı VM statik bir IP adresi gerekiyor. SAP barındıran SAPRouter SAPRouter bağlanmak için (karşılık gelen VM yükle SAPRouter örneğini), SAPRouter örneğinizi yapılandırmanız gereken SAP bir IP adresi almak için SAP başvurmanız gerekir. TCP bağlantı noktası 3299 gereken tek bağlantı noktasıdır.
Kurulması ve korunması SAPRouter üzerinden uzaktan destek bağlantılar hakkında daha fazla bilgi için bu kontrol [SAP kaynak](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>SAP HANA Azure yerel vm'lerde ile yüksek kullanılabilirlik
SUSE Linux 12 SP1 çalıştıran ve daha yeni, Pacemaker küme HANA sistem çoğaltma ve otomatik yük devretme ile zaman uyumlu çoğaltma kullanan bir SAP HANA yapılandırması ayarlamak için STONITH aygıtlarla kurabilirsiniz. Kurulum yordamı makalesinde açıklanan [SAP HANA, yüksek kullanılabilirlik'Azure sanal makineler üzerinde](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

 










