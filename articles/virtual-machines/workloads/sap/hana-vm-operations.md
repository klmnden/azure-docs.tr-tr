---
title: Azure üzerinde SAP HANA işlemleri | Microsoft Docs
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
ms.openlocfilehash: 2480ad464f2fc716cf68672387a189aeb92f5737
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918841"
---
# <a name="sap-hana-on-azure-operations-guide"></a>Azure işlemler kılavuzu üzerinde SAP HANA
Bu belgede işletim dağıtılan Azure yerel sanal makinelerinde (VM'ler) SAP HANA sistemleri için yönergeler verilmektedir. Bu belge aşağıdaki içeriği için standart bir SAP belgelerindeki değiştirin yönelik değildir:

- [SAP Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP yükleme kılavuzlarını](https://service.sap.com/instguides)
- [SAP notları](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenlerini temel bilgiye ihtiyacınız vardır:

- [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Depolama](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

SAP NetWeaver ve diğer Azure üzerinde SAP bileşenleri hakkında daha fazla bilgi için bkz: [azure'da SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) bölümünü [Azure belgeleri](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Temel Kurulum konuları
Aşağıdaki bölümlerde, Azure Vm'leri üzerinde SAP HANA sistemleri dağıtmak için temel kurulum konuları açıklanmaktadır.

### <a name="connect-into-azure-virtual-machines"></a>Azure sanal makinelerine bağlanmak
Açıklandığı gibi [Planlama Kılavuzu Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), Azure Vm'lerini bağlamak için iki temel yöntem vardır:

- Hızlı bir VM'de veya SAP HANA çalıştırmayı VM'de genel uç noktaları ve internet üzerinden bağlanır.
- Üzerinden bir [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) veya Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Siteden siteye bağlantı VPN veya ExpressRoute aracılığıyla üretim senaryoları için gereklidir. Bu tür bir bağlantı, SAP yazılım kullanıldığı yerin üretim senaryolarına akış üretim dışı senaryolar için de gereklidir. Aşağıdaki görüntüde, siteler arası bağlantı örneği gösterilmektedir:

![Siteler arası bağlantı](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Azure VM türleri seçin
Üretim senaryoları için kullanılabilecek Azure VM türleri listelenen [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Üretim dışı senaryolar için çok çeşitli yerel Azure VM türleri kullanılabilir.

>[!NOTE]
> Üretim dışı senaryolar için listelenen VM türleri kullanın [SAP notu 1928533 #](https://launchpad.support.sap.com/#/notes/1928533). Kullanımı ve Azure Vm'leri için üretim senaryoları için yayımlanan SAP Vm'lerde SAP HANA sertifikalı denetlemek [sertifikalı Iaas platformlar listesine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Azure sanal makineleri kullanarak dağıtın:

- Azure portalı.
- Azure PowerShell cmdlet'leri.
- Azure CLI.

Ayrıca Azure VM Hizmetleri aracılığıyla bir tam yüklü SAP HANA platforma dağıtabilirsiniz [SAP Cloud platform](https://cal.sap.com/). Yükleme işlemi açıklanmıştır [dağıtma SAP S/4HANA veya BW/4hana'yı Azure üzerindeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) veya serbest Otomasyon [burada](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Azure depolama türü seçin
Azure, Azure Vm'leri için SAP HANA çalıştırmayı uygun olan iki depolama türlerini sağlar:

- [Azure standart depolama](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure, Azure standart ve Premium depolama alanına VHD'ler için iki dağıtım yöntemleri sunar. Genel senaryo veriyorsa yararlanmak [Azure yönetilen disk](https://azure.microsoft.com/services/managed-disks/) dağıtımları.

Depolama türleri ve IOPS ve depolama aktarım hızının, SLA'ları içeren listeyi gözden geçirip [yönetilen diskler için Azure belgeleri](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Azure sanal makineler için depolama yapılandırma

Şirket içi SAP HANA gereçlerini satın aldığınız için uzaklığa gibi hiçbir zaman SAP HANA için en az depolama gereksinimlerinin karşılandığından emin olmak gerecin satıcısına gerektiğinden, g/ç alt sistemlerinin ve özellikleri hakkında dikkatli olun gerekiyordu. Azure altyapı kendiniz gibi ayrıca bazı ayrıca aşağıdaki bölümlerde öneririz yapılandırma gereksinimlerini anlamak için bu gereksinimleri farkında olmalıdır. Burada sanal makinelerin yapılandırılmasından çalışmaları için SAP HANA çalıştırmak istediğiniz veya. Bazı istemeden özelliklerine gerek výsledek:

- Okuma/yazma /hana/log en az 1 MB g/ç boyutu 250 MB/sn bir birimde etkinleştir
- En az 400 MB/sn için /hana/data 16 MB ve 64 MB g/ç boyutları için okuma etkinliği etkinleştirin
- En az 250 MB/sn /hana/data 16 MB ve 64 MB g/ç boyutu için yazma etkinliği etkinleştirin

Düşük depolama gecikme süresi gibi SAP HANA, bu verileri bellek içinde saklamak gibi DBMS sistemleri için önemlidir. Kritik depolama genellikle DBMS sistemleri işlem günlüğü yazma yoludur. Ancak aynı zamanda operations kayıt yazma veya kilitlenme kurtarma kritik sonra verileri bellek içinde yükleniyor. Bu nedenle, Azure Premium Diskler'den yararlanmaya /hana/data ve /hana/log birimleri için zorunludur. / Hana/log/hana/veri SAP tarafından istendiği ve en düşük aktarım hızı elde etmek için bir RAID 0'ı oluşturmak gereken MDADM veya LVM birden çok Azure Premium depolama diskleri kullanarak ve RAID birimleri/hana/veri ve/hana/günlük birimleri kullanın. Stripe RAID 0 öneri boyutları olarak kullanmaktır:

- 64K ya da/hana/veriler için 128K
- 32 bin/hana/günlüğü

> [!NOTE]
> Azure Premium ve standart depolama üç görüntü VHD tutmak bu yana RAID birimleri kullanılarak herhangi bir yedeklilik düzeyi yapılandırmanız gerekmez. Yalnızca bir RAID birimine kullanımını yeterli g/ç aktarım hızı birimleri yapılandırmaktır.

Azure VHD'leri bir RAID altında bir dizi biriktirme, IOPS ve depolama aktarım hızı taraftan biriktirici olur. 3 x P30 Azure Premium depolama diskleri üzerinde bir RAID 0 koyarsanız, bu nedenle, bu, üç kez IOPS ve tek bir Azure Premium depolama P30 disk depolama verimliliğini üç kez vermeniz gerekir.

Aşağıdaki önbelleğe alma önerileri, g/ç özelliklerini listeleyen gibi SAP HANA için varsayılarak:

- Zor var. karşı HANA veri dosyalarını okuma her türlü iş yükü Verileri HANA yüklendiğinde büyük ölçekli g/ç HANA örneği Azure VM yeniden başlatma veya yeniden başlatma sonrasında özel durumlardır. Başka bir örneği büyük veri dosyalarını karşı g/ç HANA veritabanı yedeklemeleri olabilir okuyun. Sonuç olarak çoğunlukla Önbellek Okuma beri örneklerinin en mantıklı değildir, tüm veri dosyası birimleri tamamen okunması gerekir.
- Veri dosyalarını karşı yazma, HANA kayıt ve HANA kurtarma tarafından temel artışları yaşanır. Yazma kayıt zaman uyumsuzdur ve tüm kullanıcı işlemleri tutan değil. Yazma kilitlenme Kurtarma sırasında önemli bir performans sistem yeniden hızlı yanıt almak için verilerdir. Bunun yerine olağanüstü durum kurtarma ancak olmalıdır
- HANA Yinele dosyalarından gereken tüm okuma vardır. Özel durumlar, işlem günlüğü yedeklemeleri, kilitlenme kurtarma gerçekleştirirken veya bir HANA örneği yeniden başlatma aşaması büyük g/ç olan.  
- Ana SAP HANA Yinele günlük dosyası karşı yazma yüktür. G/ç olabilir iş yükü doğasına bağlı, 4 KB olarak veya diğer durumlarda g/ç boyutu 1 MB veya daha fazla küçük. Performans kritik SAP HANA Yinele günlük karşı gecikme yazmaktır.
- Tüm yazma işlemlerini güvenilir bir biçimde diskte kalıcı gerekir

Bu gözlemlenen GÇ desenlerinden sonucunda SAP HANA, Azure Premium depolama kullanan farklı birimler için önbelleğe alma gibi ayarlanmalıdır:

- önbelleksizlik/hana/veriler-
- / hana/log - önbelleksizlik - özel durum için M serisi (Bu belgenin sonraki bölümlerinde bakın)
- /hana/Shared - okuma önbelleğe alma


Ayrıca genel VM g/ç aktarım hızı boyutlandırma ya da bir VM için karar aklınızda bulundurun. VM depolama verimliliğini makalesinde genel belgelenen [bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="cost-conscious-azure-storage-configuration"></a>Maliyet bilinçli Azure depolama yapılandırması
Aşağıdaki tabloda, Azure Vm'leri üzerinde SAP HANA ana müşteriler yaygın olarak kullanan VM türlerinin bir yapılandırma gösterilmektedir. SAP HANA için en düşük tüm ölçütleri karşılamayabilir bazı VM türleri olabilir. Ancak şu ana kadar sorunsuz üretim dışı senaryolar için gerçekleştirmek için bu sanal makineler olduğu görülüyor. 

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM'yi veya MDADM Şerit | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 giB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 giB | 1200 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 giB | 2000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 giB | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 giB | 1000 MB/sn | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Daha küçük bir VM ile 3 x P20 oversize birimleri ayarına göre alan öneriler ilgili türler için önerilen disk [SAP TDI depolama teknik incelemesi](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ancak tabloda gösterildiği seçimi, SAP HANA için yeterli disk aktarım hızı çalışıldı. Ayarlamak iki kez bellek birimine temsil eden yedeklemeler tutmak için boyutta, /hana/backup birime değişiklikleri gerekiyorsa çekinmeyin.   
Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını kontrol edin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure Premium depolama VHD'leri sayısını artırmam gerekiyor. Listelenen çok daha fazla VHD ile bir birimi boyutlandırma, Azure sanal makine türünü sınırları dahilinde IOPS ve g/ç aktarım hızı artar. 

> [!NOTE]
> Yukarıdaki yapılandırma öğesinden yararlı değildir [Azure sanal makine tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) Azure Premium depolama ve Azure standart depolama bir karışımını kullanır. Ancak, maliyetleri iyileştirmek için seçimi seçildi.


#### <a name="azure-storage-configuration-to-benefit-for-meeting-single-vm-sla"></a>Tek VM SLA'sını Karşılama konusunda yararlanmak için azure depolama yapılandırması
Yararlı istiyorsanız [Azure sanal makine tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), yalnızca Azure Premium depolama VHD'leri kullanmanız gerekir.

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM'yi veya MDADM Şerit | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E32v3 | 256 giB | 768 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| E64v3 | 443 giB | 1200 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| GS5 | 448 giB | 2000 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M32ls | 256 giB | 500 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M64s | 1000 giB | 1000 MB/sn | 2 x P30 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |


Daha küçük bir VM ile 3 x P20 oversize birimleri ayarına göre alan öneriler ilgili türler için önerilen disk [SAP TDI depolama teknik incelemesi](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ancak tabloda gösterildiği seçimi, SAP HANA için yeterli disk aktarım hızı çalışıldı. Ayarlamak iki kez bellek birimine temsil eden yedeklemeler tutmak için boyutta, /hana/backup birime değişiklikleri gerekiyorsa çekinmeyin.  
Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını kontrol edin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure Premium depolama VHD'leri sayısını artırmam gerekiyor. Listelenen çok daha fazla VHD ile bir birimi boyutlandırma, Azure sanal makine türünü sınırları dahilinde IOPS ve g/ç aktarım hızı artar. 



#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Azure yazma Hızlandırıcı Azure M serisi sanal makineler için depolama çözümü
Azure yazma Hızlandırıcı M serisi VM'ler için özel olarak kullanıma bir işlevdir. Adını belirten gibi Azure Premium Depolama'ya yönelik yazma işlemlerinin g/ç gecikme işlevselliğini amacı artırmaktır. SAP HANA için yazma Hızlandırıcı, yalnızca /hana/log birim karşı kullanılmak üzere varsayılır. Bu nedenle şu ana kadar gösterilen yapılandırmaları değiştirilmesi gerekir. Ana /hana/data /hana/log arasındaki paketlerdeki yalnızca /hana/log birim karşı Azure yazma Hızlandırıcı kullanmak için farklıdır. 

> [!IMPORTANT]
> SAP HANA Azure M serisi sanal makineler için yalnızca /hana/log birimi için Azure yazma Hızlandırıcı ile sertifikasıdır. Sonuç olarak, üretim senaryosu SAP HANA dağıtımları Azure M serisi sanal makinelerde /hana/log birimi için Azure yazma Hızlandırıcı ile yapılandırılması beklenir.  

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Önerilen yapılandırmaları şöyle görünür:

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 giB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 giB | 1000 MB/sn | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını kontrol edin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure Premium depolama VHD'leri sayısını artırmam gerekiyor. Listelenen çok daha fazla VHD ile bir birimi boyutlandırma, Azure sanal makine türünü sınırları dahilinde IOPS ve g/ç aktarım hızı artar.

Azure yazma Hızlandırıcı, yalnızca çalışır birlikte [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). /Hana/log birimin oluşturan bu nedenle en az Azure Premium depolama diskleri yönetilen disklere dağıtılması gerekiyor.

Azure Premium depolama VHD'leri Azure yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlar şu şekildedir:

- Bir M128xx 16 VHD'ler VM
- Bir M64xx VHD'ler 8 VM
- Bir M32xx 4 VHD'ler VM

Makalesinde Azure yazma Hızlandırıcı etkinleştirme hakkında daha ayrıntılı yönergeler bulunabilir [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Ayrıntıları ve kısıtlamaları Azure yazma Hızlandırıcı için aynı belgelerinde bulunabilir.


### <a name="set-up-azure-virtual-networks"></a>Azure sanal ağları ayarlama
VPN veya ExpressRoute aracılığıyla azure'a siteden siteye bağlantı varsa, en az bir tane olmalıdır [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) VPN veya ExpressRoute bağlantı hattına bir sanal ağ geçidinden bağlı. Sanal ağ geçidi, Azure sanal ağ içindeki alt ağ içinde bulunur. SAP HANA yüklemek için sanal ağ içindeki iki ek alt ağlar oluşturun. Bir alt ağ, SAP HANA örnekleri çalıştırmak için sanal makineleri barındırır. Diğer alt ağı, sıçrama kutusu veya yönetim Vm'leri SAP HANA Studio veya diğer yönetim yazılımı barındırmak için çalışır.

SAP HANA çalıştırmayı Vm'leri yüklediğinizde, VM'lerin gerekir:

- Yüklü iki sanal NIC: Yönetim alt ağına bağlanmak için bir NIC ve Azure VM'de SAP HANA örneği şirket içi ağ ya da diğer ağlara bağlanmak için bir NIC.
- Her iki sanal NIC için dağıtılan statik özel IP adresleri.

IP adresleri atamak için farklı yöntemler genel bakış için bkz. [IP adresi türleri ve ayırma yöntemleri azure'da](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

SAP HANA çalıştıran VM'ler için statik IP adresleri atanan çalışması gerekir. Bazı configuration öznitelikleri HANA için IP adresleri başvuru nedenidir.

[Azure ağ güvenlik grupları (Nsg'ler)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) SAP HANA örneği veya Sıçrama kutusu için yönlendirilen trafiği yönlendirmek için kullanılır. SAP HANA alt ağı ve yönetim alt ağı için Nsg ilişkilendirilir.

Aşağıdaki görüntüde bir kaba dağıtım şema genel bir bakış için SAP HANA gösterilmektedir:

![SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking.PNG)


Azure'da SAP HANA siteden siteye bağlantı olmadan dağıtmak için bir genel IP adresi ancak SAP HANA örneği erişin. Sıçrama kutusuna sanal makinenizin çalışmakta olan Azure VM için IP adresi atanmış olmalıdır. Bu temel bir senaryoda, dağıtım ana bilgisayar adlarını çözümlemek için Azure yerleşik DNS hizmetlere dayanır. Genel kullanıma yönelik IP adreslerini kullanıldığı daha karmaşık bir dağıtımda, Azure yerleşik DNS hizmetleri özellikle önemlidir. Azure Nsg'ler, genel kullanıma yönelik IP adreslerine sahip varlıklar Azure alt ağlarla içine bağlanabilir IP adresi aralıkları ve bağlantı noktalarını açma sınırlamak için kullanın. Aşağıdaki görüntüde, SAP HANA siteden siteye bağlantı olmadan dağıtmak için kaba bir şema gösterilmektedir:
  
![Siteden siteye bağlantı olmadan SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Azure Vm'leri üzerinde SAP HANA dağıtma işlemleri
Aşağıdaki bölümlerde Azure Vm'leri üzerinde SAP HANA sistemleri dağıtımıyla ilgili işlemleri bazılarını açıklar.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Yedekleme ve geri yükleme işlemleri Azure vm'lerinde
Aşağıdaki belgeler, yedekleme ve geri yükleme, SAP HANA dağıtım açıklar:

- [SAP HANA yedeklemesine genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA dosya düzeyi yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA depolama anlık görüntü Kıyaslama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Başlatma ve SAP HANA içeren Vm'leri yeniden başlatma
Azure genel bulutunda tanınmış bir özelliğini, yalnızca bilgi işlem, dakika için ücretlendirildiğiniz ' dir. Örneğin, SAP HANA çalıştıran bir sanal makineyi, bu süre boyunca yalnızca depolama maliyetleri için faturalandırılırsınız. İlk dağıtımınızda sanal makineleriniz için statik IP adresleri belirttiğinizde başka bir özellik kullanılabilir. SAP HANA sahip bir VM'yi yeniden başlatma, önceki IP adresleriyle yeniden başlatır. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAP için Uzak destek SAProuter kullanın
Azure ve şirket içi konumlar arasında siteden siteye bağlantınız ve SAP bileşenleri çalıştırıyorsanız, SAProuter büyük olasılıkla zaten çalıştırıyorsunuz. Bu durumda, aşağıdaki öğeler için Uzak destek tamamlayın:

- Sanal makinenin özel ve statik IP adresi barındıran SAP HANA SAProuter yapılandırmasında korur.
- Barındıran 3299 TCP/IP bağlantı noktası üzerinden trafiğe izin verecek şekilde HANA VM alt ağının bir NSG yapılandırın.

Daha sonra azure'a internet üzerinden bağlanıyorsanız ve VM ile SAP HANA için SAP yönlendirici yoksa, bileşen yüklemeniz gerekir. Ayrı bir sanal makinede Yönetim Alt SAProuter yükleyin. Aşağıdaki görüntüde, siteden siteye bağlantı olmadan ve SAProuter SAP HANA dağıtma kaba bir şema gösterilmektedir:

![SAP HANA için bir siteden siteye bağlantı ve SAProuter dağıtım şema kaba](media/hana-vm-operations/hana-simple-networking3.PNG)

Sıçrama kutusu VM'niz değil, ayrı bir sanal makinede SAProuter yüklediğinizden emin olun. Ayrı sanal Makineyi statik bir IP adresi olmalıdır. SAP tarafından barındırılan SAProuter, SAProuter bağlanmak için bir IP adresi için SAP başvurun. (SAP tarafından barındırılan SAProuter karşılık gelen sanal makinenizde yüklediğiniz SAProuter örneğinin içindir.) SAP IP adresinden SAProuter örneğinizin yapılandırmak için kullanın. Yapılandırma ayarlarında TCP bağlantı noktası 3299 yalnızca gerekli bağlantı noktasıdır.

Ayarlama ve SAProuter aracılığıyla uzak destek bağlantıları hakkında daha fazla bilgi için bkz. [SAP belgelerindeki](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Yerel Azure sanal makineler'de SAP HANA ile yüksek kullanılabilirlik
SUSE Linux 12 SP1 çalıştırıyorsanız veya daha sonra STONITH cihazlarla Pacemaker küme kurup olur. Cihazlar, HANA sistem çoğaltması ve otomatik yük devretme ile zaman uyumlu çoğaltma kullanan bir SAP HANA yapılandırması ayarlamak için kullanabilirsiniz. Kurulum yordamı hakkında daha fazla bilgi için bkz: [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
