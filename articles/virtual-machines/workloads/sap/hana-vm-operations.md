---
title: SAP HANA altyapısı yapılandırmaları ve işlemleri Azure üzerinde | Microsoft Docs
description: Azure sanal makinelerinde dağıtılan SAP HANA sistemleri için işlemler Kılavuzu.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6f60fdced25fdc594c28972f555bb28a9c629f21
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878664"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri
Bu belge, Azure altyapı yapılandırma ve işletim dağıtılan Azure yerel sanal makinelerinde (VM'ler) SAP HANA sistemleri için yönergeler sağlar. Belge ayrıca SAP HANA ölçeklendirme M128s VM SKU için yapılandırma bilgilerini içerir. Bu belge aşağıdaki içeriği için standart bir SAP belgelerindeki değiştirin yönelik değildir:

- [SAP Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP yükleme kılavuzlarını](https://service.sap.com/instguides)
- [SAP notları](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenlerini temel bilgiye ihtiyacınız vardır:

- [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

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
> Üretim dışı senaryolar için listelenen VM türleri kullanın [SAP notu 1928533 #](https://launchpad.support.sap.com/#/notes/1928533). Kullanımı ve Azure Vm'leri için üretim senaryoları için yayımlanan SAP Vm'lerde SAP HANA sertifikalı denetlemek [sertifikalı Iaas platformlar listesine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Azure sanal makineleri kullanarak dağıtın:

- Azure portalı.
- Azure PowerShell cmdlet'leri.
- Azure CLI.

Ayrıca Azure VM Hizmetleri aracılığıyla bir tam yüklü SAP HANA platforma dağıtabilirsiniz [SAP Cloud platform](https://cal.sap.com/). Yükleme işlemi açıklanmıştır [dağıtma SAP S/4HANA veya BW/4hana'yı Azure üzerindeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) veya serbest Otomasyon [burada](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Azure depolama türü seçin
Azure, Azure Vm'leri için SAP HANA çalıştırmayı uygun olan iki depolama türlerini sağlar: Standart sabit disk sürücülerinin (HDD) ve premium katı hal sürücülerine (SSD). Şu disk türleri hakkında bilgi edinmek için bkz: makalemizi [bir disk türü seçin](../../windows/disks-types.md)

- Standart sabit disk sürücülerinin (HDD)
- Premium katı hal sürücülerine (SSD)

Azure, Azure standart ve Premium depolama alanına VHD'ler için iki dağıtım yöntemleri sunar. Genel senaryo veriyorsa yararlanmak [Azure yönetilen disk](https://azure.microsoft.com/services/managed-disks/) dağıtımları.

Depolama türleri ve IOPS ve depolama aktarım hızının, SLA'ları içeren listeyi gözden geçirip [yönetilen diskler için Azure belgeleri](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Azure sanal makineler için depolama yapılandırma

Şu ana kadar hiç g/ç alt sistemlerinin ve özellikleri hakkında dikkatli gerekiyordu. SAP HANA için en az depolama gereksinimlerinin karşılandığından emin olmak gereken gerecin satıcısına neden oldu. Azure altyapı kendiniz gibi ayrıca bazıları bu gereksinimleri bilmeniz gerekir. Ayrıca aşağıdaki bölümlerde önerilen yapılandırma gereksinimlerini anlamak ve. Burada sanal makinelerin yapılandırılmasından çalışmaları için SAP HANA çalıştırmak istediğiniz veya. Bazı istemeden özelliklerine gerek výsledek:

- Üzerinde okuma/yazma toplu etkinleştirme **/hana/günlük** 250 MB/sn en az 1 MB g/ç boyutu
- Etkinleştirme okuma en az 400 MB/sn için etkinlik **/hana/veri** 16 MB ve 64 MB g/ç boyutları için
- En az 250 MB/sn için yazma etkinliği etkinleştirin **/hana/veri** 16 MB ve 64 MB g/ç boyutu

Düşük depolama gecikme süresi gibi SAP HANA DBMS verileri bellek içinde saklamak gibi DBMS sistemler için önemlidir. Kritik depolama genellikle DBMS sistemleri işlem günlüğü yazma yoludur. Ancak aynı zamanda operations kayıt yazma veya kilitlenme kurtarma kritik sonra verileri bellek içinde yükleniyor. Bu nedenle, için Azure Premium Diskler'den yararlanmaya zorunlu **/hana/veri** ve **/hana/günlük** birimleri. En düşük aktarım hızı elde etmek için **/hana/günlük** ve **/hana/veri** RAID 0 oluşturmanızı sağlayacak dilediğiniz şekilde SAP tarafından MDADM veya LVM birden çok Azure Premium depolama diskleri kullanarak. Ve RAID birimleri olarak **/hana/veri** ve **/hana/günlük** birimleri. Stripe RAID 0 öneri boyutları olarak kullanmaktır:

- 64 KB veya 128 KB   **/hana/veri**
- 32 KB   **/hana/günlük**

> [!NOTE]
> Azure Premium ve standart depolama üç görüntü VHD tutmak bu yana RAID birimleri kullanılarak herhangi bir yedeklilik düzeyi yapılandırmanız gerekmez. Yalnızca bir RAID birimine kullanımını yeterli g/ç aktarım hızı birimleri yapılandırmaktır.

Azure VHD'leri bir RAID altında bir dizi biriktirme, IOPS ve depolama aktarım hızı taraftan biriktirici olur. 3 x P30 Azure Premium depolama diskleri üzerinde bir RAID 0 koyarsanız, bu nedenle, bu, üç kez IOPS ve tek bir Azure Premium depolama P30 disk depolama verimliliğini üç kez vermeniz gerekir.

Aşağıdaki önbelleğe alma önerileri, g/ç özelliklerini listeleyen gibi SAP HANA için varsayılarak:

- Zor var. karşı HANA veri dosyalarını okuma her türlü iş yükü Büyük ölçekli g/ç HANA veriler yüklendiğinde veya HANA örneği yeniden başlatma işleminden sonra özel durumlardır. Başka bir örneği büyük veri dosyalarını karşı g/ç HANA veritabanı yedeklemeleri olabilir okuyun. Sonuç olarak çoğunlukla Önbellek Okuma beri örneklerinin en mantıklı değildir, tüm veri dosyası birimleri tamamen okunması gerekir.
- Veri dosyalarını karşı yazma, HANA kayıt ve HANA kurtarma tarafından temel artışları yaşanır. Yazma kayıt zaman uyumsuzdur ve tüm kullanıcı işlemleri tutan değil. Yazma kilitlenme Kurtarma sırasında önemli bir performans sistem yeniden hızlı yanıt almak için verilerdir. Bunun yerine olağanüstü durum kurtarma ancak olmalıdır
- HANA Yinele dosyalarından gereken tüm okuma vardır. Özel durumlar, işlem günlüğü yedeklemeleri, kilitlenme kurtarma gerçekleştirirken veya bir HANA örneği yeniden başlatma aşaması büyük g/ç olan.  
- Ana SAP HANA Yinele günlük dosyası karşı yazma yüktür. G/ç olabilir iş yükü doğasına bağlı, 4 KB olarak veya diğer durumlarda g/ç boyutu 1 MB veya daha fazla küçük. Performans kritik SAP HANA Yinele günlük karşı gecikme yazmaktır.
- Tüm yazma işlemlerini güvenilir bir biçimde diskte kalıcı gerekir

Bu gözlemlenen GÇ desenlerinden sonucunda SAP HANA, Azure Premium depolama kullanan farklı birimler için önbelleğe alma gibi ayarlanmalıdır:

- **/ hana/veri** -önbelleksizlik
- **/ hana/günlük** - önbelleksizlik - M serisi (Bu belgenin sonraki bölümlerinde bakın) için özel durumu
- **/ hana/paylaşılan** - okuma önbelleğe alma


Ayrıca genel VM g/ç aktarım hızı boyutlandırma ya da bir VM için karar aklınızda bulundurun. VM depolama verimliliğini makalesinde genel belgelenen [bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="linux-io-scheduler-mode"></a>Linux g/ç Zamanlayıcısını modu
Linux, birkaç farklı g/ç planlama modu vardır. Linux satıcılar ve SAP ortak öneri olan liste kutusundan disk birimleri için g/ç Zamanlayıcı modu ayarlamak için **cfq** moduna **noop** modu. Ayrıntılar başvurulan [SAP notu #1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Azure yazma Hızlandırıcı Azure M serisi sanal makineler için depolama çözümü
Azure yazma Hızlandırıcı Azure M serisi VM'ler için özel olarak kullanıma bir işlevdir. Adını belirten gibi Azure Premium Depolama'ya yönelik yazma işlemlerinin g/ç gecikme işlevselliğini amacı artırmaktır. SAP HANA için yazma Hızlandırıcı karşı kullanılmak üzere beklenen **/hana/günlük** yalnızca birim. Bu nedenle şu ana kadar gösterilen yapılandırmaları değiştirilmesi gerekir. Paketlerdeki arasında ana değişikliktir **/hana/veri** ve **/hana/günlük** karşı Azure yazma Hızlandırıcı kullanmak için **/hana/günlük** yalnızca birim. 

> [!IMPORTANT]
> Yalnızca Azure yazma Hızlandırıcı ile SAP HANA sertifika Azure M serisi sanal makineler için olduğundan **/hana/günlük** birim. Sonuç olarak, üretim senaryosu Azure M serisi sanal makinelerde SAP HANA dağıtımları için Azure yazma Hızlandırıcı ile yapılandırılması beklenir **/hana/günlük** birim.  

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Önerilen yapılandırmaları şöyle görünür:

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 giB | 1000 MB/sn | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını kontrol edin. İş yükü için daha yüksek birimleri gerektiriyorsa **/hana/veri** ve **/hana/günlük**, Azure Premium depolama VHD'leri sayısını artırmanız gerekir. Listelenen çok daha fazla VHD ile bir birimi boyutlandırma, Azure sanal makine türünü sınırları dahilinde IOPS ve g/ç aktarım hızı artar.

Azure yazma Hızlandırıcı, yalnızca çalışır birlikte [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Bu nedenle en az Azure Premium depolama diskleri oluşturan **/hana/günlük** birim yönetilen diskler olarak dağıtılması gerekiyor.

Azure Premium depolama VHD'leri Azure yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlar şu şekildedir:

- Bir M128xx 16 VHD'ler VM
- Bir M64xx VHD'ler 8 VM
- Bir M32xx 4 VHD'ler VM

Makalesinde Azure yazma Hızlandırıcı etkinleştirme hakkında daha ayrıntılı yönergeler bulunabilir [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Ayrıntıları ve kısıtlamaları Azure yazma Hızlandırıcı için aynı belgelerinde bulunabilir.


#### <a name="cost-conscious-azure-storage-configuration"></a>Maliyet bilinçli Azure depolama yapılandırması
Aşağıdaki tabloda, Azure Vm'leri üzerinde SAP HANA ana müşteriler yaygın olarak kullanan VM türlerinin bir yapılandırma gösterilmektedir. SAP HANA için en düşük tüm ölçütleri karşılamıyor olabilir veya SAP HANA ile SAP tarafından resmi olarak desteklenmez bazı VM türleri olabilir. Ancak şu ana kadar sorunsuz üretim dışı senaryolar için gerçekleştirmek için bu sanal makineler olduğu görülüyor. 

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM'yi veya MDADM Şerit | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 giB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 432 giB | 1200 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 giB | 1000 MB/sn | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Daha küçük bir VM ile 3 x P20 oversize birimleri ayarına göre alan öneriler ilgili türler için önerilen disk [SAP TDI depolama teknik incelemesi](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ancak tabloda gösterildiği seçimi, SAP HANA için yeterli disk aktarım hızı çalışıldı. Değişiklikleri gerekiyorsa **/hana/yedekleme** iki kez bellek birimine temsil eden yedeklemeler tutmak için boyutta, birim ayarlamak ücretsiz gönderebilirsiniz.   
Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını kontrol edin. İş yükü için daha yüksek birimleri gerektiriyorsa **/hana/veri** ve **/hana/günlük**, Azure Premium depolama VHD'leri sayısını artırmanız gerekir. Listelenen çok daha fazla VHD ile bir birimi boyutlandırma, Azure sanal makine türünü sınırları dahilinde IOPS ve g/ç aktarım hızı artar. 

> [!NOTE]
> Yukarıdaki yapılandırma öğesinden yararlı değildir [Azure sanal makine tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) Azure Premium depolama ve Azure standart depolama bir karışımını kullanır. Ancak, maliyetleri iyileştirmek için seçimi seçildi. Yukarıda Azure Standard Storage (VM yapılandırması Azure tek VM SLA ile uyumlu hale getirmek için Sxx) olarak listelenen tüm diskler için Premium depolama seçmeniz gerekir.


> [!NOTE]
> Belirtilen disk yapılandırması önerileri SAP altyapısını sağlayıcıları ifade en düşük gereksinimler hedeflediğiniz. Gerçek müşteri dağıtımları ve iş yükü senaryoları, burada bu önerileri yine de yeterli özellikleri sağlamadı durumlarda karşılaşıldı. Bunlar burada bir müşteri HANA yeniden başlatmadan sonra verileri daha hızlı bir şekilde yeniden yüklenmesi gereken durumlar olabilir veya burada depolama yapılandırmaları gerekli daha yüksek bant yedekleme. Dahil edilen diğer durumlarda **/hana/günlük** burada 5000 IOPS değil belirli iş yükü için yeterli. Bu nedenle bu önerileri bir başlangıç olarak işaret ve uyum Al iş yükü gereksinimlerine göre.
>  

### <a name="set-up-azure-virtual-networks"></a>Azure sanal ağları ayarlama
VPN veya ExpressRoute aracılığıyla azure'a siteden siteye bağlantı varsa, VPN veya ExpressRoute bağlantı hattına sanal ağ geçidi üzerinden bağlı en az bir Azure sanal ağınızın olması gerekir. Basit dağıtımlarda, sanal ağ geçidi, SAP HANA örnekleri de barındıran Azure sanal ağı (VNet) bir alt ağ içinde dağıtılabilir. SAP HANA yüklemek için Azure sanal ağ içindeki iki ek alt ağlar oluşturun. Bir alt ağ, SAP HANA örnekleri çalıştırmak için sanal makineleri barındırır. Diğer alt ağı, sıçrama kutusu veya yönetim Vm'leri SAP HANA Studio, diğer yönetim yazılımı veya uygulama yazılımınızı barındırmak için çalışır.

> [!IMPORTANT]
> İşlevsellik, ancak daha fazla dışında önemli performans nedeniyle dışında yapılandırmak için desteklenmez [Azure ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) DBMS katmanı bir SAP NetWeaver SAP uygulama arasındaki iletişim yolunun içinde Hybris veya S/4HANA, SAP sistemine bağlı. SAP uygulama katmanı ve DBMS katman arasındaki iletişim, doğrudan bir tane olması gerekiyor. Kısıtlama içermemesi [Azure ASG ve NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview) ASG ve NSG kuralları bir doğrudan iletişime izin vermek sürece. Burada nva'ları desteklenmez başka senaryolar şunlardır açıklandığı gibi Linux Pacemaker küme düğümlerini ve SBD cihazları temsil eden bir Azure VM'ler arasında iletişimi yollarda [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik SAP uygulamaları için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse). Veya iletişim yollarını arasında Azure VM ve Windows Server SOFS açıklandığı kadar ayarlamak [SAP ASCS/SCS örneği ile Azure dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). İletişim yolları can nva'larını kolayca ağ gecikme süresi iki iletişim iş ortakları arasında çift, aktarım hızı SAP uygulama katmanı ve DBMS katman arasında kritik yollarda kısıtlayabilirsiniz. Müşterilerle gözlemlenen bazı senaryolarda, nva'ları Pacemaker Linux kümeleri SBD cihazını bir NVA aracılığıyla iletişim kurmak için Linux Pacemaker düğümler arasındaki iletişimler gerektiğinde başarısız olmasına neden olabilir.  
> 

> [!IMPORTANT]
> Olan başka bir tasarım **değil** desteklenir SAP uygulama katmanı ve DBMS katman ayrımı olmayan farklı Azure sanal ağlarına [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) birbiriyle. SAP uygulama katmanında ve farklı Azure sanal ağları kullanarak yerine bir Azure sanal ağ içindeki alt ağlara DBMS katman ayırmanız önerilir. İki sanal ağ olmayan öneriye ve bunun yerine iki katmanı farklı bir sanal ağa ayırmak karar verirseniz, olması gereken [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Unutmayın, ağ trafiğini iki [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağları olan aktarım maliyetlerinin konu. SAP uygulama katmanında ve DBMS katman arasında alınıp verilen terabayta kadar büyük veri hacmi ile önemli maliyetleri, toplanabilir SAP uygulama katmanında ve DBMS katman iki eşlenen Azure sanal ağları arasında ayrılmış. 

SAP HANA çalıştırmayı Vm'leri yüklediğinizde, VM'lerin gerekir:

- Yüklü iki sanal NIC: Yönetim alt ağına bağlanmak için bir NIC ve Azure VM'de SAP HANA örneği şirket içi ağ ya da diğer ağlara bağlanmak için bir NIC.
- Her iki sanal NIC için dağıtılan statik özel IP adresleri.

> [!NOTE]
> Azure yol aracılığıyla statik IP adresleri atamasını bireysel Vnıc'ler için. Konuk işletim sistemi içinde statik IP adresleri için bir Vnıc atamanız gerekir değil. Olgu üzerinde bazı Azure Hizmetleri gibi Azure Backup hizmeti kullanan, en azından birincil Vnıc DHCP ve statik IP adresleri için ayarlanır. Ayrıca bkz [sanal makine yedekleme sorunlarını giderme Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Bir VM'ye birden fazla statik IP adresleri atamak gerekiyorsa, bir VM'ye çoklu Vnıcs atamanız gerekir.
>
>

Ancak, enduring dağıtımları için Azure'da bir sanal veri merkezi ağ mimarisi oluşturmanız gerekir. Bu mimari, şirket içi ayrı bir Azure Vnet'e bağlanır Azure VNet ağ geçidinin ayrılması önerir. Bu ayrı sanal ağ, şirket içi veya İnternet'e bırakır tüm trafiği barındırmamalısınız. Bu yaklaşım, yazılım denetleme ve sanal veri merkezi, Azure'da bu ayrı hub sanal ağında girdiği günlük trafiği dağıtmanıza olanak tanır. Bu nedenle içinde - ve giden trafiği Azure dağıtımınıza ilişkili tüm yazılım ve yapılandırmalar barındıran bir VNet gerekir.

Makaleleri [Azure sanal veri merkezi: Ağ perspektifi](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) ve [Azure sanal veri merkezi ve kurumsal denetim düzlemi](https://docs.microsoft.com/azure/architecture/vdc/) sanal veri merkezi yaklaşımıyla ve ilgili Azure sanal ağ tasarımı hakkında daha fazla bilgi verin.


>[!NOTE]
>Bir merkez sanal ağı ve bağlı bileşen, VNet kullanarak arasında akan trafik [Azure VNet eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) sahibi olan ek [maliyetleri](https://azure.microsoft.com/pricing/details/virtual-network/). Bu maliyetleri temelinde, katı bir merkez ve uç ağ tasarımı çalıştıran ve birden çok çalışan arasında ödün yapmayı düşünün gerekebilir [Azure ExpressRoute ağ geçitleri](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) VNet eşlemesi atlamak için 'uçlara yönelik' bağlanın. Ancak, Azure ExpressRoute ağ geçitleri ek tanıtmak [maliyetleri](https://azure.microsoft.com/pricing/details/vpn-gateway/) de. Ayrıca, ağ trafiğini günlüğe kaydetme, denetleme ve izleme için kullandığınız üçüncü taraf yazılımları için ek maliyetler karşılaşabilirsiniz. Bağımlı bir tarafı ve maliyetleri ek Azure ExpressRoute ağ geçitleri ve ek yazılım lisansları ile oluşturulan sanal ağ eşlemesi aracılığıyla veri değişimi için maliyetleri, bir sanal ağ içinde mikro segmentasyona için yalıtım birim olarak alt ağlar'ı kullanarak karar verebilir Vnet yerine.


IP adresleri atamak için farklı yöntemler genel bakış için bkz. [IP adresi türleri ve ayırma yöntemleri azure'da](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

SAP HANA çalıştıran VM'ler için statik IP adresleri atanan çalışması gerekir. Bazı configuration öznitelikleri HANA için IP adresleri başvuru nedenidir.

[Azure ağ güvenlik grupları (Nsg'ler)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) SAP HANA örneği veya Sıçrama kutusu için yönlendirilen trafiği yönlendirmek için kullanılır. Nsg'ler ve sonunda [uygulama güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) SAP HANA alt ağı ve yönetim alt ağı ilişkilendirilir.

Aşağıdaki görüntüde, SAP HANA için bir merkez ve uç VNet mimarisi aşağıdaki kaba dağıtım şema genel bir bakış gösterilmektedir:

![SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking.PNG)

Azure'da SAP HANA siteden siteye bağlantı olmadan dağıtmak için yine de genel internet'ten SAP HANA örneği koruyun ve ileri bir proxy'nin arkasındayken Gizle istiyor. Bu temel bir senaryoda, dağıtım ana bilgisayar adlarını çözümlemek için Azure yerleşik DNS hizmetlere dayanır. Genel kullanıma yönelik IP adreslerini kullanıldığı daha karmaşık bir dağıtımda, Azure yerleşik DNS hizmetleri özellikle önemlidir. Azure Nsg'ler kullanın ve [Azure nva'ları](https://azure.microsoft.com/solutions/network-appliances/) denetim, azure'da Azure sanal ağ Mimarinizi içine internet'ten yönlendirme izlemek için. Aşağıdaki görüntüde, SAP HANA bir merkez ve uç VNet mimarisi siteden siteye bağlantı olmadan dağıtmak için kaba bir şema gösterilmektedir:
  
![Siteden siteye bağlantı olmadan SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Azure nva'ları denetleme ve izleme erişimi Internet'ten hub'ı kullanın ve VNet mimarisi uç konusunda başka bir açıklama makalesinde bulunabilir [yüksek oranda kullanılabilir ağ sanal gereçlerini dağıtma](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>SAP HANA genişletmek için Azure altyapısını yapılandırma
Microsoft, bir SAP HANA genişletme yapılandırma için sertifikalı bir M serisi VM SKU sahiptir. VM türü M128s bir ölçek genişletme için en fazla 16 düğümlerinin sertifikalı. Azure vm'lerde SAP HANA genişleme sertifikaları için değişiklikleri, iade [sertifikalı Iaas platformlar listesine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Azure sanal makinelerinde genişleme yapılandırmaları dağıtmak için en düşük işletim sistemi sürümleri şunlardır:

- SUSE Linux 12 SP3
- Red hat Linux 7.4

16 düğüm genişleme sertifikasını

- Ana düğümün bir düğümüdür
- En fazla 15 düğüme olan çalışan düğümleri

>[!NOTE]
>Azure VM ölçek genişletme dağıtımlarda bekleme düğümü kullanmak kaybetme riski yoktur
>

Bir bekleme düğümünü yapılandırmak yükleyememeyle nedenini iki aşamalıdır şunlardır:

- Azure, bu noktada hiçbir yerel NFS hizmeti vardır. Sonuç olarak NFS paylaşımlarını üçüncü taraf işlevselliği yardımıyla yapılandırılması gerekir.
- Üçüncü taraf NFS yapılandırmaları hiçbiri, Azure'da dağıtılan çözümleri ile SAP HANA için depolama gecikmesi ölçütlerini karşılayan mümkün değil.

Sonuç olarak, **/hana/veri** ve **/hana/günlük** birimleri paylaşılamaz. Tek düğümlerin bu birimlerin kullanılmadığı bir SAP HANA bekleme düğümü genişleme yapılandırmasında kullanımını engeller.

Sonuç olarak görünmesi için bir genişletme yapılandırma tek bir düğüm için temel tasarım geçiyor:

![Tek bir düğüm genişleme temelleri](media/hana-vm-operations/scale-out-basics.PNG)

SAP HANA genişleme için bir VM düğümünden temel yapılandırmasını aşağıdaki gibi görünür:

- İçin **/hana/paylaşılan**, SUSE Linux üzerinde 12 SP3 tabanlı yüksek oranda kullanılabilir bir NFS küme oluşturmak. Bu küme barındıracak **/hana/paylaşılan** NFS paylaşımlar genişletme yapılandırma ve SAP NetWeaver veya BW/4hana'yı merkezi hizmetlerinizi. Bu tür bir yapılandırma oluşturmak için belgeleri makalesinde bulunur [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- Diğer tüm disk birimi **değil** olan ve farklı düğümler arasında paylaşılan **değil** NFS tabanlı. Yükleme yapılandırmalar ve adımlar için paylaşılmayan ile ölçek genişletme HANA yüklemeler **/hana/veri** ve **/hana/günlük** aşağıya bu belgedeki sağlanır.

>[!NOTE]
>Yüksek oranda kullanılabilir bir NFS küme grafikleri şimdiye görüntülendiği şekilde SUSE Linux ile yalnızca desteklenir. Red Hat üzerinde alan yüksek oranda kullanılabilir bir NFS çözümü daha sonra önerilecektir.

Düğümler için birimleri boyutlandırma aynıdır ölçek büyütme dışında **/hana/paylaşılan**. M128s VM SKU için önerilen boyutlar ve türlerinin şöyle görünür:

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü sağlayıp sağlamadığını kontrol edin. İş yükü için daha yüksek birimleri gerektiriyorsa **/hana/veri** ve **/hana/günlük**, Azure Premium depolama VHD'leri sayısını artırmanız gerekir. Listelenen çok daha fazla VHD ile bir birimi boyutlandırma, Azure sanal makine türünü sınırları dahilinde IOPS ve g/ç aktarım hızı artar. Ayrıca Azure yazma Hızlandırıcı diskleri oluşturan uygulama **/hana/günlük** birim.
 
Belgedeki [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), boyutu tanımlayan bir formül adlı **/hana/paylaşılan** birimi için tek çalışan düğüme dört çalışan düğümü başına bellek boyutu olarak ölçeği genişletme.

SAP HANA genişleme sertifikalı M128s Azure VM kabaca 2 TB belleğe sahip olması varsayıldığında, SAP önerileri gibi özetlenebilir:

- Bir ana düğümü ve dört alt düğüm, en fazla **/hana/paylaşılan** birim boyutu 2 TB olması gerekir. 
- Bir ana düğümü ve beş ve sekiz çalışan düğümü boyutu **/hana/paylaşılan** 4 TB olmalıdır. 
- Bir ana düğümü ve 9-12 çalışan düğümü için 6 TB boyutunu **/hana/paylaşılan** gerekli olacaktır. 
- Bir ana düğümü ve 12 arasında 15 çalışan düğümü kullanarak, işiniz sağlamak için gerekli bir **/hana/paylaşılan** , 8 TB boyutunda birimi.

Genişleme SAP HANA VM tek düğüm yapılandırması grafik görüntülenen bir önemli tasarım VNet veya alt ağ yapılandırmasını daha iyi olur. SAP HANA düğümler arasında iletişime geçilen istemci/uygulama yönelik trafiği ayrımı önerir. Grafikler, gösterildiği gibi bu hedef VM'ye bağlı iki farklı Vnıc'ler sağlanarak elde edilir. Her iki Vnıc'ler farklı alt ağlarda, iki farklı IP adresine sahip. Ardından yönlendirme kuralları ile Nsg veya kullanıcı tanımlı yollar kullanarak trafik akışını denetler.

Özellikle Azure'da yok anlamına gelir ve yöntemleri belirli Vnıc'ler kotalar ve hizmet kalitesi zorlamak için vardır. Sonuç olarak, istemci/uygulama yönelik ve düğüm içi iletişimin ayrımı başka bir trafik akışı önceliğini fırsatı açılmaz. Bunun yerine bir ölçü genişleme yapılandırmaları düğüm içi iletişimin koruma güvenlik ayrımı kalır.  

>[!IMPORTANT]
>SAP istemci/uygulama tarafından ağ trafiği ve bu belgede açıklanan şekilde düğüm içi trafik ayırma önerir. Bu nedenle bir mimari son grafikleri gösterildiği yerinde koyarak kesinlikle önerilir.
>

Bir ağ açısından bakıldığında, en düşük gerekli ağ mimarisi gibi görünür:

![Tek bir düğüm genişleme temelleri](media/hana-vm-operations/scale-out-networking-overview.PNG)

Şu ana kadar desteklenen bir ana düğüme ek 15 çalışan limitlerdir.

Bir depolama açısından bakıldığında, depolama mimarisi gibi görünür:


![Tek bir düğüm genişleme temelleri](media/hana-vm-operations/scale-out-storage-overview.PNG)

**/Hana/paylaşılan** birim üzerinde yüksek oranda kullanılabilir NFS Paylaşım Yapılandırması bulunur. 'Yerel olarak' diğer tüm sürücüleri ise tek VM'ler için bağlanır. 

### <a name="highly-available-nfs-share"></a>Yüksek oranda kullanılabilir bir NFS paylaşım
Yüksek oranda kullanılabilir bir NFS küme şu ana kadar yalnızca SUSE Linux ile çalışmaktadır. Belge [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) kurulacağını açıklar. SAP HANA örnekleri çalıştıran azure sanal ağ dışındaki diğer bir HANA yapılandırmaları ile NFS küme paylaşmayın, aynı sanal ağda yükleyin. Kendi alt ağına yükleyin ve tüm rastgele trafiğini alt ağa erişebildiğinden emin olun. Bunun yerine o alt ağa trafiği yürütün VM'nin IP adreslerine trafiği sınırlamak istediğinize **/hana/paylaşılan** birim.

Yönlendirmek bir HANA genişleme VM için Vnıc ilgili **/hana/paylaşılan** trafik, öneriler şunlardır:

- Trafiği beri **/hana/paylaşılan** olan Orta, yönlendirme, en düşük yapılandırmayı istemci ağa atanır Vnıc aracılığıyla
- Sonuç olarak, trafiği için **/hana/paylaşılan**, üçüncü bir alt ağ SAP HANA genişletme yapılandırma ve bu alt ağda barındırılır Ata üçüncü bir Vnıc dağıttığınız sanal ağ dağıtma. İlişkili IP adresi ve üçüncü Vnıc NFS paylaşımına trafiği için kullanın. Ardından, ayrı bir erişim ve yönlendirme kuralları uygulayabilirsiniz.

>[!IMPORTANT]
>SAP HANA, dağıtılan bir genişleme biçimde olan Vm'leri yüksek oranda kullanılabilir NFS arasındaki ağ trafiğini hiçbir koşulda yönlendirilir aracılığıyla bir [NVA](https://azure.microsoft.com/solutions/network-appliances/) veya benzer sanal cihazlar. Oysa Azure Nsg'ler cihaz yok. Nva'ları veya benzer sanal Gereçleri detoured emin olmak için yönlendirme kuralları kontrol yüksek oranda kullanılabilir bir NFS paylaşımına SAP HANA çalıştırmayı Vm'lerden eriştiğinizde.
> 

Yüksek oranda kullanılabilir bir NFS küme SAP HANA yapılandırmaları arasında paylaşmak istiyorsanız, bu HANA yapılandırmaları aynı Vnet'te taşıyın. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>SAP HANA genişleme n Azure yükleme
Yapılandırmayı genişletmek SAP yükleme, kaba adımları tamamlamanız gerekir:

- Yeni dağıtma veya yeni bir Azure sanal ağ altyapısını uyarlama
- Azure Premium depolama yönetilen birimleri yeni Vm'leri dağıtma
- Yeni bir dağıtımı veya mevcut yüksek oranda kullanılabilir bir NFS küme uyum
- Örneğin, sanal makineler arasında düğüm içi iletişimin aracılığıyla yönlendirilir değil, emin olmak için ağ yönlendirme uyum bir [NVA](https://azure.microsoft.com/solutions/network-appliances/). Aynı sanal makineleri yüksek oranda kullanılabilir bir NFS küme arasındaki trafiği için geçerlidir.
- SAP HANA ana düğüme yükleyin.
- SAP HANA ana düğümünün yapılandırma parametreleri uyum
- SAP HANA çalışan düğümü yükleme işlemine devam

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>SAP HANA yüklemesinde genişletme yapılandırma
Azure VM altyapınızı dağıtılır ve diğer tüm hazırlıkları tamamladıktan gibi SAP HANA genişleme yapılandırmaları Bu adımlarda yüklemeniz gerekir:

- SAP HANA ana düğüm SAP'nin belgelere göre yükleyin
- **Yüklemeden sonra global.ini dosyasını değiştirin ve parametre eklemek gereken ' basepath_shared = Hayır ' global.ini için**. Bu parametre, genişletme 'shared' olmadan çalıştırmak SAP HANA etkinleştirecek **/hana/veri** ve **/hana/günlük** düğümler arasında birimleri. Ayrıntılar bölümünde belgelenmiştir [SAP notu #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- SAP HANA örneği global.ini parametresi değiştirdikten sonra yeniden başlatın
- Ek çalışan düğümleri ekleyin. Ayrıca bkz: <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>. SAP HANA yüklemesi veya daha sonra örnek, kullanarak, yerel hdblcm sırasında düğümler arası iletişim için iç ağ belirtin. Daha ayrıntılı belgeler için Ayrıca bkz: [SAP notu #2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Bu Kurulum yordamı yüklediğiniz genişletme yapılandırma gittiğini çalıştırmak için paylaşılmayan disk kullanmak **/hana/veri** ve **/hana/günlük**. Oysa **/hana/paylaşılan** birim yerleştirilecek üzerinde yüksek oranda kullanılabilir NFS paylaşım.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA dinamik katmanlama 2.0 için Azure sanal makineleri

M serisi Azure sanal makineler'de SAP HANA sertifikaları yanı sıra SAP HANA dinamik katmanlama 2.0 ayrıca Microsoft Azure üzerinde desteklenir (SAP HANA dinamik katmanlama belgelerine bağlantılar aşağıya bakın). Ürünü yüklemek veya bu işletim herhangi bir fark olsa da, örneğin, SAP HANA Kokpit içinde bir Azure sanal makinesi aracılığıyla azure'da resmi destek için zorunlu olan birkaç önemli öğe yok. Aşağıdaki önemli noktalara aşağıda açıklanmıştır. Makale boyunca kısaltması "DT 2.0" dinamik katmanlama 2.0 tam adı yerine kullanılacaktır.

SAP HANA dinamik katmanlama 2.0, SAP BW veya S4HANA tarafından desteklenmiyor. Ana kullanım örnekleri şu anda yerel HANA uygulamalardır.


### <a name="overview"></a>Genel Bakış

Aşağıdaki resimde, Microsoft Azure'da DT 2.0 desteği ile ilgili genel bir bakış sağlar. Resmi bir sertifika ile uyum sağlamak için izlenmesi gereken bir dizi zorunlu gereksinim vardır:

- DT 2.0 adanmış bir Azure sanal makinesinde yüklü olmalıdır. SAP HANA çalıştığı aynı VM'de çalışmayabilir
- SAP HANA ve DT 2.0 Vm'leri aynı Azure sanal ağ içinde dağıtılmalıdır
- DT 2.0 sanal makineleri ve SAP HANA etkin Azure hızlandırılmış ağ ile dağıtılması gerekir
- Azure Premium depolama DT 2.0 Vm'leri için depolama türü olmalıdır
- Birden çok Azure diskleri DT 2.0 VM'ye bağlı olması gerekir
- Yazılım RAID oluşturmak için gerekli / şeritli birim (lvm veya mdadm aracılığıyla da) Azure disklerde şeritleme kullanma

Aşağıdaki bölümlerde daha fazla ayrıntı açıklanacaktır.

![SAP HANA DT 2.0 mimarisine genel bakış](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Azure VM'de SAP HANA DT 2.0 için ayrılmış

Azure Iaas üzerinde DT 2.0 yalnızca adanmış bir VM üzerinde desteklenir. DT 2.0 HANA örneği çalıştığı aynı Azure sanal makinesinde çalıştırmak için izin verilmiyor. SAP HANA DT 2.0 çalıştırmak için ilk iki VM türleri kullanılabilir:

- M64-32ms 
- E32sv3 

VM türü tanımı bakın [burada](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Maliyet tasarrufu için "sıcak" veri boşaltma olan DT 2.0 temel fikri verilen karşılık gelen VM boyutlarını kullanın mantıklıdır. Olası birleşimlerin ilgili olsa katı kural yoktur. Bu, belirli müşteri iş yüküne bağlıdır.

Önerilen yapılandırmaları olacaktır:

| SAP HANA VM türü | DT 2.0 VM türü |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Tüm desteklenen DT 2.0 Vm'leri (M64-32ms ve E32sv3) ile SAP HANA sertifikalı M serisi VM'ler olası birleşimleridir.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure ağ ve SAP HANA DT 2.0

DT 2.0 adanmış bir VM üzerinde yüklemek için DT 2.0 VM ve SAP HANA VM en az 10 GB arasında ağ aktarım hızı gerekir. Bu nedenle aynı Azure sanal ağ içindeki tüm sanal makineler ve Azure hızlandırılmış ağ iletişimi etkinleştirmek için zorunludur.

Azure hızlandırılmış ağ hakkında ek bilgi [burada](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

### <a name="vm-storage-for-sap-hana-dt-20"></a>SAP HANA DT 2.0 için VM depolama

DT 2.0 en iyi uygulama kılavuzunu göre disk g/ç aktarım hızı fiziksel çekirdek başına en az 50 MB/sn olmalıdır. DT 2.0 için bir desteklenen iki Azure VM türleri belirtimi bakarak maksimum disk g/ç aktarım hızı sınırı VM için görürsünüz:

- E32sv3    :   768 MB/sn (fiziksel çekirdek başına 48 MB/sn oranını başka bir deyişle, önbelleğe alınmamış)
- M64-32ms  :  1000 MB/sn (yani 62.5 MB/sn başına fiziksel çekirdek oranı önbelleğe alınmamış)

Birden çok Azure diski DT 2.0 VM ve VM başına disk aktarım hızı sınırını elde etmek için işletim sistemi düzeyinde yazılım RAID (şeritleme) oluşturmak için gereklidir. Tek bir Azure diskinin VM sınırını bu bağlamda ulaşmak için aktarım hızı sağlayamaz. Azure Premium depolama DT 2.0 çalıştırmak için zorunludur. 

- Kullanılabilir Azure disk türleri hakkında daha fazla bilgi bulunabilir [burada](../../windows/disks-types.md)
- Yazılım RAID mdadm yoluyla oluşturma hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
- En fazla aktarım hızı bulunabilir şeritli birim oluşturmak için LVM'yi yapılandırma hakkında ayrıntılı bilgiler [burada](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)

Boyut gereksinimlerine bağlı olarak, bir VM'nin en çok aktarım hızı ulaşmak için farklı seçenekler vardır. VM aktarım hızı üst sınırı elde etmek her DT 2.0 VM türü için olası veri birimi disk yapılandırmaları şunlardır. E32sv3 VM iş yükleri küçük bir giriş düzeyi olarak düşünülmelidir. Bunu hızlı değil, kapatmalısınız durumunda yeterince bunu M64-32ms VM'ye yeniden boyutlandırmak gerekli olabilir.
Kadar bellek M64-32ms VM olduğu gibi g/ç yük okuma açısından yoğun iş yükleri için özellikle sınırına ulaşmadığınız değil. Bu nedenle daha az diskler kümesi bağlı olarak müşteri belirli iş yükü için yeterli olabilir. Ancak, güvenli disk olması için aşağıdaki yapılandırmalardan en yüksek aktarım güvence altına almak için seçilmiştir:


| VM SKU | Disk yapılandırması 1 | Disk yapılandırması 2 | Disk yapılandırması 3 | Disk yapılandırması 4 | 5 disk yapılandırması | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 16 TB -> | 4 x P40 8 TB -> | 5 x P30 -> 5 TB | 7 x P20 3,5 TB'a -> | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 2,5 TB -> | 6 x P15, 1,5 TB'ı -> | 


Özellikle iş yükü okuma yoğun olması durumunda "veritabanı yazılımını veri hacimleri için önerildiği şekilde salt okunur" Azure ana bilgisayar önbelleğini etkinleştirmek için g/ç performansı artırma. İşlem için günlük Azure ana bilgisayar diski önbellek "hiçbiri" olmalıdır ancak. 

Günlük birimi boyutu ile ilgili önerilen bir başlangıç noktası bir buluşsal yönteme veri boyutunun % 15 ' dir. Günlük birimi oluşturulmasını, maliyet ve işleme gereksinimlerine bağlı olarak farklı Azure disk türleri kullanarak gerçekleştirilebilir. Günlüğü yüksek g/ç aktarım hızı birimi gereklidir.  Kullanarak VM olması durumunda M64-32ms önemle tavsiye edilir etkinleştirme türü [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). Azure yazma Hızlandırıcı, işlem günlüğü (yalnızca M serisi için kullanılabilir) için en yüksek disk yazma gecikmesi sağlar. En fazla disk sayısı VM türü başına rağmen gibi dikkate alınması gereken bazı öğeler vardır. Yazma hızlandırıcı hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)


Günlük birimi boyutlandırma ile ilgili bazı örnekler şunlardır:

| veri birimi boyutunu ve disk türü | Günlük birimi ve disk yapılandırma 1 yazın. | Günlük birimi ve disk yapılandırma 2 yazın |
| --- | --- | --- |
| 4 x P50 16 TB -> | 5 x P20 2,5 TB -> | 3 x P30 -> 3 TB |
| 6 x P15, 1,5 TB'ı -> | 4 x P6 256 GB -> | 1 x P15 256 GB -> |


SAP HANA ölçek genişletme için gibi SAP HANA VM ve DT 2.0 VM arasında paylaşılmak üzere /hana/shared dizini vardır. SAP HANA genişleme kullanarak olduğu gibi aynı mimariye yüksek oranda kullanılabilir bir NFS sunucusu önerildiği gibi davranan Vm'leri ayrılmış. Paylaşılan bir yedekleme birimi sağlamak amacıyla aynı tasarım kullanılabilir. Ancak HA gerekli veya yalnızca bir yedekleme sunucusu olarak davranacak şekilde yeterli depolama kapasitesine adanmış bir VM kullanmak için yeterli ise, müşterinin kadar olacaktır.



### <a name="links-to-dt-20-documentation"></a>DT 2.0 belgelerine bağlantılar 

- [SAP HANA dinamik katmanlama yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA dinamik katmanlama ilgili öğreticiler ve kaynaklar](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA Dynamic Tiering PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [SAP HANA 2.0 SPS 02 dinamik katmanlama geliştirmeleri](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




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
SUSE Linux Enterprise Server SAP uygulamaları 12 SP1 veya üzeri işletim sistemini kullanıyorsanız STONITH cihazlarla Pacemaker kümesi oluşturabilirsiniz. Cihazlar, HANA sistem çoğaltması ve otomatik yük devretme ile zaman uyumlu çoğaltma kullanan bir SAP HANA yapılandırması ayarlamak için kullanabilirsiniz. Kurulum yordamı hakkında daha fazla bilgi için bkz: [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
