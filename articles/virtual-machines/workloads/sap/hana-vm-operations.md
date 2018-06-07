---
title: Azure üzerinde SAP HANA işlemler | Microsoft Docs
description: Azure sanal makinelerinde dağıtılan SAP HANA sistemleri için işlemler Kılavuzu.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: juergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61369fbf864db28ee0a9415bbb87dca2a185ed43
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809684"
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA üzerinde Azure işlemler Kılavuzu
Bu belgede işletim Azure yerel sanal makinelerde (VM'ler) dağıtılan SAP HANA sistemleri için yönergeler sağlanmaktadır. Bu belge aşağıdaki içeriği içerir standart SAP belgeleri değiştirmek üzere tasarlanmamıştır:

- [SAP Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP yükleme kılavuzları](https://service.sap.com/instguides)
- [SAP notları](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Önkoşullar
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
> Üretim dışı senaryoları için listelenen VM türlerini kullanan [SAP Not #1928533](https://launchpad.support.sap.com/#/notes/1928533). SAP HANA yayımlanan SAP Vm'lerde sertifikalı kullanımını Azure VM'ler için üretim senaryoları için kontrol [sertifikalı Iaas platformlar listesi](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Azure'da Vm'leri kullanarak dağıtın:

- Azure portalı.
- Azure PowerShell cmdlet'leri.
- Azure CLI.

Ayrıca, bir tam yüklü SAP HANA platformda Azure VM Hizmetler aracılığıyla dağıtabilirsiniz [SAP bulut platformu](https://cal.sap.com/). Yükleme işlemi açıklanan [dağıtmak SAP S/4HANA veya BW/4HANA Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) veya yayımlanan otomasyon ile [burada](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Azure depolama türü seçin
Azure SAP HANA çalışan Azure VM'ler için uygun olan iki tür depolama sağlar:

- [Standart Azure depolama](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure, Azure standart ve Premium depolama VHD için iki dağıtım yöntemleri sunar. Genel senaryo izin veriliyorsa, yararlanmak [Azure disk yönetilen](https://azure.microsoft.com/services/managed-disks/) dağıtımları.

Depolama türlerini ve SLA'ları IOPS ve depolama iş çıkarma listesi için gözden [yönetilen diskler için Azure belgelerine](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Azure sanal makineler için depolama yapılandırma

SAP HANA cihazları şirket içi için satın aldığınız şekilde uzaklığa kadar hiçbir zaman Gereci satıcı minimum depolama gereksinimleri için SAP HANA karşılandığından emin olmak gerektiğinden g/ç alt sistemleri ve özelliklerini hakkında önemli gerekiyordu. Azure altyapısı kendiniz oluşturmak gibi aynı zamanda bazı ayrıca aşağıdaki bölümlerde önerdiğimiz yapılandırma gereksinimlerini anlamak için bu gereksinimleri farkında olmalıdır. Sanal makineler yapılandırmakta olduğunuz nerede durumları için SAP HANA çalıştırmak istediğiniz veya. Bazı sorulur özelliklerine gerek sonuçta:

- 250 MB/sn en az 1 MB g/ç boyutlarıyla /hana/log okuma/yazma birimde etkinleştir
- En az 400 MB/sn 16 MB ile 64 MB g/ç boyutları için /hana/data için okuma etkinlik etkinleştir
- En az 250 MB/sn /hana/data 16 MB ile 64 MB g/ç boyutu için yazma etkinliği etkinleştir

SAP HANA gibi bu bellekteki verileri tutmak gibi düşük o depolama gecikme DBMS sistemler için kritik öneme sahiptir. Kritik depolama genellikle işlem günlük yazma DBMS sistemlerinin yoludur. Ancak aynı zamanda operations kayıt yazmak veya veri bellek içi kilitlenme kurtarma kritik sonra yükleme. Bu nedenle, /hana/data ve /hana/log birimleri için Azure Premium diskleri kullanabilmeniz için zorunludur. / Hana/log/hana/verileri SAP tarafından istenen olarak ve minimum verime ulaşmak için bir RAID 0 yapı gerekir MDADM veya LVM birden çok Azure Premium Storage diskleri kullanarak ve RAID birimleri/hana/veri ve/hana/günlük birimleri kullanabilirsiniz. Şerit RAID 0 öneri boyutları olarak kullanmaktır:

- 64K veya/hana/veriler için 128K
- / Hana/günlüğü için 32K

> [!NOTE]
> Azure Premium ve standart depolama VHD üç görüntüleri tutmak bu yana RAID birimleri kullanılarak herhangi bir artıklık düzeyi yapılandırmak gerekmez. Yalnızca bir RAID birimi kullanımını yeterli g/ç işleme sağlamak birimleri yapılandırmaktır.

Azure VHD'ler bir RAID altında bir dizi biriktirme, bir IOPS ve depolama verimliliği taraftan biriktirici olur. 3 x P30 Azure Premium Storage diskler üzerinde RAID 0 yerleştirirseniz, bu nedenle, bu, üç kez IOPS ve tek bir Azure Premium Storage P30 diskinin depolama verimliliğini üç kez vermesi gerekir.

Premium depolama /hana/data ve /hana/log için kullanılan diskler üzerinde önbelleğe alma yapılandırmayın. Bu birimlerin oluşturma tüm diskleri 'Yok' olarak ayarlanan bu disklerin önbelleğe alma olması gerekir.

Ayrıca genel VM g/ç işleme boyutlandırma veya bir VM için karar aklınızda bulundurun. VM depolama verimliliğini makalesinde genel belgelenen [bellek için iyileştirilmiş sanal makine boyutlarını](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="cost-conscious-azure-storage-configuration"></a>Maliyet bilinçli Azure depolama yapılandırması
Aşağıdaki tabloda konak SAP HANA Azure vm'lerinde müşteriler sık kullandığınız VM türleri yapılandırmasını gösterir. SAP HANA için en az tüm ölçüt karşılamayabilir bazı VM türleri olabilir. Ancak şu ana kadar bu VM'lerin üretim dışı senaryoları için ince ayar yapmak gibi görünüyor. 

> [!NOTE]
> Üretim senaryoları için SAP içinde tarafından belirli bir VM türü için SAP HANA desteklenip desteklenmediğini kontrol [IAAS SAP belgelerine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM veya MDADM şeritli | / hana/paylaşılan | / root birim | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 Gib'den | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 Gib'den | 1200 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 Gib'den | 2000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 Gib'den | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 Gib'den | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 Gib'den | 1000 MB/sn | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 Gib'den | 1000 MB/sn | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 Gib'den | 2000 MB/sn |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 Gib'den | 2000 MB/sn | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Daha küçük VM ile 3 x P20 oversize birimleri ayarına göre alan önerileri ilgili türleri için önerilen disk [SAP TDI depolama teknik](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ancak tabloda gösterilen seçimi SAP HANA için yeterli disk verimliliği sağlayın çalışıldı. İki kez bellek birim temsil yedeklemeleri tutmak için boyuta sahip, /hana/backup birime değişiklikleri gerekiyorsa ayarlamak çekinmeyin.   
Farklı önerilen birimler için depolama üretilen işini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını denetleyin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure Premium Storage VHD'ler sayısını artırmak gerekir. Listelenen olandan daha fazla VHD sahip bir birim boyutlandırma Azure sanal makine türü sınırları içinde IOPS ve g/ç verimliliği artırır. 

> [!NOTE]
> Yukarıdaki yapılandırmaları gelen yararlı değildir [Azure sanal makine tek bir VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) bir karışımını Azure Premium Storage ve Azure Standard Storage kullanır. Ancak, seçim maliyetleri en iyi duruma getirmek için seçildi.


#### <a name="azure-storage-configuration-to-benefit-for-meeting-single-vm-sla"></a>Tek VM SLA toplantı yararlanmak için azure depolama yapılandırması
Yararlı istiyorsanız [Azure sanal makine tek bir VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), Azure Premium Storage VHD'ler özel olarak kullanmanız gerekir.

> [!NOTE]
> Üretim senaryoları için SAP içinde tarafından belirli bir VM türü için SAP HANA desteklenip desteklenmediğini kontrol [IAAS SAP belgelerine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM veya MDADM şeritli | / hana/paylaşılan | / root birim | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E32v3 | 256 Gib'den | 768 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| E64v3 | 443 Gib'den | 1200 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| GS5 | 448 Gib'den | 2000 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M32ts | 192 Gib'den | 500 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M32ls | 256 Gib'den | 500 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M64s | 1000 Gib'den | 1000 MB/sn | 2 x P30 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 Gib'den | 1000 MB/sn | 3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 Gib'den | 2000 MB/sn |3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 Gib'den | 2000 MB/sn | 5 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |


Daha küçük VM ile 3 x P20 oversize birimleri ayarına göre alan önerileri ilgili türleri için önerilen disk [SAP TDI depolama teknik](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ancak tabloda gösterilen seçimi SAP HANA için yeterli disk verimliliği sağlayın çalışıldı. İki kez bellek birim temsil yedeklemeleri tutmak için boyuta sahip, /hana/backup birime değişiklikleri gerekiyorsa ayarlamak çekinmeyin.  
Farklı önerilen birimler için depolama üretilen işini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını denetleyin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure Premium Storage VHD'ler sayısını artırmak gerekir. Listelenen olandan daha fazla VHD sahip bir birim boyutlandırma Azure sanal makine türü sınırları içinde IOPS ve g/ç verimliliği artırır. 



#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Azure yazma Hızlandırıcı Azure M-serisi sanal makineler için bir depolama çözümü
Azure yazma Hızlandırıcı M-serisi VM'ler için özel olarak alınır bir işlevdir. Adını belirten, işlevselliği amacı Azure Premium Storage'a karşı yazma g/ç gecikmesi artırmak için aynıdır. SAP HANA için yazma Hızlandırıcı yalnızca /hana/log birim karşı kullanılan beklenir. Bu nedenle kadarki gösterilen yapılandırmalarının değiştirilmesi gerekebilir. Ana /hana/data /hana/log arasındaki paketlerdeki yalnızca /hana/log birim karşı Azure yazma Hızlandırıcı kullanmak için farklıdır. 

> [!IMPORTANT]
> SAP HANA sertifika Azure M-serisi sanal makineler için Azure yazma Hızlandırıcı /hana/log birim için özel olarak sahip olur. Sonuç olarak, üretim senaryo SAP HANA dağıtımları Azure M-serisi sanal makinelerde Azure yazma Hızlandırıcı /hana/log biriminin aşağıdakilerle yapılandırılması beklenir.  

> [!NOTE]
> Üretim senaryoları için SAP içinde tarafından belirli bir VM türü için SAP HANA desteklenip desteklenmediğini kontrol [IAAS SAP belgelerine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Önerilen yapılandırmaları gibi görünür:

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / hana/paylaşılan | / root birim | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 Gib'den | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 Gib'den | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 Gib'den | 1000 MB/sn | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 Gib'den | 1000 MB/sn | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 Gib'den | 2000 MB/sn |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 Gib'den | 2000 MB/sn | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Farklı önerilen birimler için depolama üretilen işini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını denetleyin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure Premium Storage VHD'ler sayısını artırmak gerekir. Listelenen olandan daha fazla VHD sahip bir birim boyutlandırma Azure sanal makine türü sınırları içinde IOPS ve g/ç verimliliği artırır.

Azure yazma Hızlandırıcı yalnızca çalışır birlikte [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/). /Hana/log birim oluşturan böylece en az Azure Premium Storage diskin yönetilen diskleri olarak dağıtılması gerekir.

Azure Premium Storage VHD'ler Azure yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlarını şunlardır:

- Bir M128xx için 16 VHD'leri VM
- Bir M64xx için 8 VHD'leri VM
- Bir M32xx için 4 VHD'leri VM

Azure yazma Hızlandırıcı etkinleştirme hakkında daha ayrıntılı yönergeler makalesinde bulunabilir [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Ayrıntılar ve Azure yazma Hızlandırıcı sınırlamaları aynı belgelerinde bulunabilir.


### <a name="set-up-azure-virtual-networks"></a>Azure sanal ağları ayarlayın
Azure VPN ya da ExpressRoute aracılığıyla içine siteden siteye bağlantı varsa, en az bir olmalıdır [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) VPN ya da ExpressRoute bağlantı hattına sanal bir ağ geçidi üzerinden bağlı. Azure sanal ağındaki bir alt ağdaki sanal ağ geçidi yaşar. SAP HANA yüklemek için sanal ağ içindeki iki ek alt ağlar oluşturun. Bir alt ağ SAP HANA örnekleri çalıştırmak için sanal makineleri barındırır. Diğer alt Jumpbox veya yönetim VM'ler SAP HANA Studio ya da diğer yönetim yazılımı barındırmak için çalışır.

SAP HANA çalıştırmak için sanal makineleri yüklediğinizde, sanal makineleri gerekir:

- Yüklü iki sanal NIC: yönetimi alt ağına bağlanmak için bir NIC ve Azure VM SAP HANA örneğinde şirket içi ağ veya diğer ağlara bağlanmak için bir NIC.
- Her iki sanal NIC için dağıtılan statik özel IP adresleri.

IP adresleri atama için farklı yöntemler genel bakış için bkz: [IP adresi türleri ve Azure ayırma yöntemleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

SAP HANA çalıştıran VM'ler için atanan statik IP adresleri ile çalışması gerekir. Bazı yapılandırma öznitelikler HANA için IP adreslerini başvuru nedenidir.

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
SUSE Linux 12 SP1 çalıştırıyorsanız veya daha sonra STONITH aygıtlarla Pacemaker küme kurup varsa. Zaman uyumlu çoğaltma HANA sistem çoğaltma ve otomatik yük devretme kullanan bir SAP HANA yapılandırması ayarlamak için cihazları kullanın. Kurulum yordamının hakkında daha fazla bilgi için bkz: [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
