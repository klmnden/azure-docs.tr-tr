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
ms.openlocfilehash: acccc553c5b63b2acd0f9793b0397b25145449dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60477475"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri
Bu makalede, nasıl Azure altyapısını yapılandırma ve dağıtılan Azure yerel sanal makinelerinde (VM'ler) SAP HANA sistemleri çalıştırılacağı hakkında yönergeler sağlanır. Bu makale, SAP HANA ölçeklendirme M128s VM SKU için yapılandırma bilgilerini de içerir. Bu makalede, aşağıdaki içeriği için standart bir SAP belgelerindeki değiştirmek için tasarlanmamıştır:

- [SAP Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP yükleme kılavuzlarını](https://service.sap.com/instguides)
- [SAP notları](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenlerini temel bilgiye ihtiyacınız vardır:

- [Azure Sanal Makineler](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Depolama](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

SAP NetWeaver ve diğer Azure üzerinde SAP bileşenleri hakkında daha fazla bilgi için bkz: [azure'da SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) bölümünü [Azure belgeleri](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Temel Kurulum konuları
Aşağıdaki bölümlerde, Azure Vm'leri üzerinde SAP HANA sistemleri dağıtmak için temel kurulum konuları açıklanmaktadır.

### <a name="connect-to-azure-virtual-machines"></a>Azure sanal makinelere bağlanın
Açıklandığı gibi [Planlama Kılavuzu Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), iki temel yöntem Azure Vm'lerine bağlanmak için kullanılır:

- Bir Sıçrama kutusu VM'de veya SAP HANA çalıştırmayı VM'de genel uç noktaları ve internet üzerinden bağlanır.
- Üzerinden bir [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) veya Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Siteden siteye bağlantı VPN veya ExpressRoute aracılığıyla üretim senaryoları için gereklidir. Bu tür bir bağlantı, SAP yazılım kullanıldığı üretim senaryolarına akış üretim dışı senaryolar için de gereklidir. Aşağıdaki görüntüde, siteler arası bağlantı örneği gösterilmektedir:

![Siteler arası bağlantı](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Azure VM türleri seçin
Üretim senaryolarında kullanılması için Azure VM türleri listelenen [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Üretim dışı senaryolar için çok çeşitli yerel Azure VM türleri kullanılabilir.

>[!NOTE]
> Üretim dışı senaryolar için listelenen VM türleri kullanın [SAP notu #1928533](https://launchpad.support.sap.com/#/notes/1928533). Yayımlanan SAP Vm'lerde SAP HANA sertifikalı Azure Vm'leri için kullanımını üretim senaryoları için kontrol edin [Iaas platformlar listesine sertifikalı](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Azure'da VM'ler dağıtmak için kullanın:

- Azure portalı.
- Azure PowerShell cmdlet'leri.
- Azure CLI.

Ayrıca Azure VM Hizmetleri aracılığıyla bir tam yüklü SAP HANA platforma dağıtabilirsiniz [SAP cloud platform](https://cal.sap.com/). Yükleme işlemi hakkında daha fazla bilgi için bkz. [dağıtma SAP S/4HANA veya BW/4hana'yı Azure üzerindeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Yayımlanan Otomasyonu hakkında daha fazla bilgi için bkz: [GitHub makalede](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-an-azure-storage-type"></a>Bir Azure depolama türü seçin
Azure, Azure Vm'leri için SAP HANA çalıştırma uygun olan iki depolama türlerini sağlar:  

- Standart sabit disk sürücülerinin (HDD'ler)
- Premium katı hal sürücüleri (SSD'ler)

Şu disk türleri hakkında bilgi edinmek için [bir disk türü seçin](../../windows/disks-types.md).

Azure standart Azure depolama ve premium depolama VHD'ler için iki dağıtım yöntemleri sunar. Genel senaryo veriyorsa yararlanmak [Azure yönetilen disk](https://azure.microsoft.com/services/managed-disks/) dağıtımları.

Depolama türleri ve IOPS ve depolama aktarım hızının, SLA'ları listesi için bkz: [yönetilen diskler için Azure belgeleri](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configure-the-storage-for-azure-virtual-machines"></a>Azure sanal makineler için depolama yapılandırma

Gerecin satıcısına SAP HANA için en az depolama gereksinimlerinin karşılandığından emin yapıldığı için g/ç alt sistemlerinin ve yeteneklerini muhtemelen hiçbir zaman ilgilenilmeli. Azure altyapısı kendiniz oluşturduğunuzda, bu gereksinimleri bazıları dikkat etmeniz gerekir. Ayrıca, aşağıdaki bölümlerde önerilen yapılandırma gereksinimlerini anlamanız gerekir. Sanal makineler yapılandırdığınız durumlarda, çalıştırmak için SAP HANA istediğiniz gerekebilir:

- Okuma/yazma birimde en az 1 MB'lık g/ç boyutu 250 MB/sn /hana/log etkinleştirin.
- En az 400 MB/sn için /hana/data 16 MB ve 64 MB'lık g/ç boyutları için okuma etkinliği etkinleştirin.
- En az 250 MB/sn /hana/data 16 MB ve 64 MB'lık g/ç boyutu için yazma etkinliği etkinleştirin.

DBMS, SAP HANA gibi verileri bellek içinde tutar olsa bile, düşük depolama gecikmesi DBMS sistemleri için önemlidir. Kritik depolama genellikle DBMS sistemleri işlem günlüğü yazma yoludur. Ancak kayıt yazma veya kilitlenme kurtarma ayrıca kritik olabilir sonra verileri bellek içinde yükleme işlemleri gibi. 

Azure premium diskler kullanımı /hana/data ve /hana/log birimleri için zorunludur. En düşük aktarım hızı /hana/log ve SAP tarafından istendiği /hana/data elde etmek için bir RAID 0 mdadm veya mantıksal birim Yöneticisi (LVM) birden çok Azure premium depolama disklerini kullanarak oluşturun. Ayrıca /hana/log birimleri ve RAID birimleri /hana/data olarak kullanın. RAID 0 aşağıdaki stripe boyutları kullanmanızı öneririz:

- 64 KB veya 128 KB/hana/veriler için
- 32 KB/hana/günlüğü

> [!NOTE]
> Azure premium ve standart depolama üç görüntü VHD sürekli değiştiğinden RAID birimleri kullanarak herhangi bir yedeklilik düzeyi yapılandırmanız gerekmez. Bir RAID birimine tamamen yeterli g/ç aktarım hızı birimleri yapılandırmak için kullanılır.

Azure VHD'leri bir RAID altında bir dizi biriktirme bir IOPS ve depolama aktarım hızı taraftan biriktirici. RAID 0 3 x P30 Azure premium depolama disklerini koyarsanız, bu, üç kez IOPS ve üç kez tek Azure premium depolama P30 disk depolama verimliliğini sunar.

Aşağıdaki önbelleğe alma önerileri SAP HANA için g/ç özelliklerini varsayın:

- Karşı HANA veri dosyalarını okuma gereken tüm iş yüklerini yoktur. G/ç büyük ölçekli veri HANA yüklendiğinde veya HANA örneği yeniden başlatma işleminden sonra özel durumlardır. Başka bir örneği büyük veri dosyalarını karşı g/ç HANA veritabanı yedeklemeleri olabilir okuyun. Çoğu durumda, tüm veri dosyası birimleri tamamen okunması gerekir çünkü sonuç olarak, okuma önbelleğini mantıklı değildir.
- Veri dosyalarını karşı yazma, HANA kayıt ve HANA kurtarma tarafından temel artışları yaşanır. Kayıt yazma zaman uyumsuzdur ve tüm kullanıcı işlemleri tutmak değil. Yazma kilitlenme Kurtarma sırasında önemli bir performans sistem yeniden hızlı yanıt almak için verilerdir. Bunun yerine olağanüstü durum kurtarma olmalıdır.
- HANA Yinele dosyalarından gereken tüm okuma vardır. İşlem günlüğü yedeklemeleri, kurtarma veya bir HANA örneği yeniden başlatma aşaması gerçekleştirdiğinizde özel durumlar büyük g/ç ' dir.  
- Ana SAP HANA Yinele günlük dosyası karşı yazma yüktür. İş yükü doğasına bağlı olarak, g/ç olabilir 4 KB olarak veya diğer durumlarda, g/ç küçük boyutu 1 MB veya daha fazla. Performans kritik SAP HANA Yinele günlük karşı gecikme yazmaktır.
- Tüm yazma işlemlerini güvenilir bir biçimde diskte kalıcı gerekir.

Bu gözlemlenen GÇ desenlerinden sonucunda SAP HANA, Azure premium depolama kullanan farklı birimler için önbelleğe almayı ayarlayın:

- **/ hana/veri**: Önbellek yok.
- **/ hana/günlük**: Yok, M serisi VM'ler (daha sonra bu makaleye bakın) için bir özel durum ile önbelleğe alma.
- **/ hana/paylaşılan**: Önbelleğe alma okuyun.


Ayrıca, boyutu veya VM üzerinde karar genel VM g/ç performansını göz önünde bulundurun. VM depolama verimliliğini genel belgelenen [bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="linux-io-scheduler-mode"></a>Linux g/ç Zamanlayıcı modu
Linux, birkaç farklı g/ç planlama modu vardır. Liste kutusundan disk birimleri için g/ç Zamanlayıcı modu ayarlamak için Linux satıcılar ve SAP ortak öneri olarak **cfq** moduna **noop** modu. Daha fazla bilgi için [SAP notu #1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-write-accelerator-for-azure-m-series-virtual-machines"></a>Depolama çözümü için Azure yazma Hızlandırıcı ile M serisi sanal makineler
 Yazma Hızlandırıcı Azure M serisi VM'ler için özel olarak kullanıma sunuluyor bir Azure özelliğidir. Azure premium Depolama'ya yönelik yazma işlemlerinin g/ç gecikme işlevselliğini amacı artırmaktır. SAP HANA için yazma Hızlandırıcı, yalnızca /hana/log birim karşı kullanılmak üzere varsayılır. Bu nedenle, şu ana kadar gösterilen yapılandırmaları değiştirilmelidir. Ana /hana/data /hana/log arasındaki paketlerdeki yalnızca /hana/log birim karşı yazma Hızlandırıcı kullanmak için farklıdır. 

> [!IMPORTANT]
> SAP HANA sertifikaları M serisi sanal makineler, yazma Hızlandırıcı/hana/günlük birimi için kullanımda. Sonuç olarak, Azure üzerinde SAP HANA dağıtımları üretim senaryosu oturum birim M serisi sanal makineler için/hana/yazma Hızlandırıcı ile yapılandırılmış olması beklenir.

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Aşağıdaki tablo, önerilen yapılandırmaları gösterir.

| VM SKU | RAM | Maksimum VM g/ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1.000 giB | 1000 MB/sn | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1,750 giB | 1000 MB/sn | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2.000 giB | 2.000 MB/sn |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3.800 giB | 2.000 MB/sn | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını kontrol edin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure premium depolama VHD'ler sayısını artırın. Bir toplu listelenen arttıkça IOPS değerinden daha fazla VHD ile ve Azure sanal makine türünü sınırları dahilinde g/ç aktarım hızı boyutlandırma.

Yazma Hızlandırıcı, yalnızca çalışır birlikte [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Az, /hana/log birimin oluşturan Azure premium depolama disklerini yönetilen disklere dağıtılması gerekir.

Azure premium depolama yazma Hızlandırıcı destekleyebilir VM başına VHD'ler sınırı yoktur. Geçerli sınırlar şu şekildedir:

- Bir M128xx 16 VHD'ler VM.
- Bir M64xx 8 VHD'ler VM.
- Bir M32xx 4 VHD'ler VM.

Yazma hızlandırıcıyı etkinleştirme hakkında daha fazla bilgi için bkz. [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Daha fazla bilgi ve yazma Hızlandırıcı için kısıtlamalar için aynı belgelerine bakın.


#### <a name="cost-conscious-azure-storage-configuration"></a>Maliyet açısından temkinli davranan Azure depolama yapılandırması
Aşağıdaki tabloda, Azure Vm'leri üzerinde SAP HANA ana müşteriler yaygın olarak kullanan VM türlerinin bir yapılandırma gösterilmektedir. SAP HANA için tüm minimum ölçütlerine uymayan bazı VM türleri olabilir. Ayrıca olabilir bazı VM türleri ile SAP HANA, SAP tarafından resmi olarak desteklenmez. Şu ana kadar bu sanal makineler, üretim dışı senaryolar için ince gerçekleştirdiniz. 

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| VM SKU | RAM | Maksimum VM g/ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM'yi veya mdadm Şerit | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 giB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 432 giB | 1.200 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2.000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1.000 giB | 1000 MB/sn | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1,750 giB | 1000 MB/sn | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2.000 giB | 2.000 MB/sn |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3.800 giB | 2.000 MB/sn | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Daha küçük bir VM içinde verilen alan öneriler in regard to birimleri ile 3 x P20 oversize türleri için önerilen diskler [SAP TDI depolama incelemeyi](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Tabloda gösterilen seçenek, SAP HANA için yeterli disk aktarım hızı çalışıldı. Değiştirmeniz gerekiyorsa **/hana/yedekleme** iki kez bellek birimine temsil eden yedeklemeler tutmak boyutta, birim ayarlamalar yapmak ücretsiz gönderebilirsiniz. 

Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını kontrol edin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure premium depolama VHD'ler sayısını artırın. Bir toplu listelenen arttıkça IOPS değerinden daha fazla VHD ile ve Azure sanal makine türünü sınırları dahilinde g/ç aktarım hızı boyutlandırma. 

> [!NOTE]
> Önceki yapılandırmaların yararlı olmayan [Azure sanal makine tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) çünkü Azure premium depolama ve Azure standart depolama bir karışımını kullanır. Seçimi maliyetleri iyileştirmek için seçilmiştir. Tüm diskler için Premium depolama, Azure standart depolama VM yapılandırması Azure tek VM SLA ile uyumlu hale getirmek için (Sxx) olarak listelenen tabloda seçin.


> [!NOTE]
> Disk yapılandırma önerileri SAP, altyapı sağlayıcıları sağlayan en düşük gereksinimleri hedefleyin. Bu öneriler, gerçek müşteri dağıtımları ve iş yükü senaryoları, bazı durumlarda yeterli özellikleri sağlamadı. Örneğin, bir müşteri, HANA yeniden başlatmadan sonra verileri daha hızlı bir şekilde yeniden yüklenmesi gereken ya da daha yüksek bant genişliği depolama için gerekli olan yedekleme yapılandırmalar. Diğer durumlarda /hana/log, burada 5.000 IOPS belirli iş yükü için yeterli olmayan içerir. Bu önerileri bir başlangıç noktası olarak kullanın ve bunları iş yükü gereksinimlerine göre uyarlayın.
>  

### <a name="set-up-azure-virtual-networks"></a>Azure sanal ağları ayarlama
Siteden siteye VPN aracılığıyla Azure veya Azure ExpressRoute bağlantısı varsa, VPN veya ExpressRoute bağlantı hattına sanal ağ geçidi üzerinden bağlı en az bir Azure sanal ağınızın olması gerekir. Basit dağıtımlarda, SAP HANA örnekleri çok barındıran Azure sanal ağ alt ağı sanal ağ geçidi dağıtılabilir. 

SAP HANA yüklemek için Azure sanal ağ içindeki iki ek alt ağlar oluşturun. Bir alt ağ, SAP HANA örnekleri çalıştırmak için sanal makineleri barındırır. Diğer alt ağı, sıçrama kutusu veya yönetim Vm'leri konağa SAP HANA Studio, diğer yönetim yazılımı veya uygulama yazılımınızı çalıştırır.

> [!IMPORTANT]
> Yapılandırma [Azure ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) SAP uygulama, bir SAP NetWeaver - DBMS katman arasındaki iletişim yolunun, Hybris veya SAP S/4HANA tabanlı sistem için desteklenmiyor. Bu kısıtlama, işlevi ve Performans nedeniyle olur. Doğrudan bir SAP uygulama katmanı ve DBMS katman arasındaki iletişim yolunun olmalıdır. Kısıtlama içermez [uygulama güvenlik grubu (ASG) ve ağ güvenlik grubu (NSG) kurallarını](https://docs.microsoft.com/azure/virtual-network/security-overview) doğrudan iletişim yolunun ASG ve NSG kuralları izin veriyorsa. 
>
> İçindeki ağ sanal Gereçleri desteklendiği olmayan diğer senaryolar şunlardır: 
>
> - Linux Pacemaker temsil eden bir Azure Vm'leri arasında iletişim yolları, düğümleri ve SBD cihazları açıklandığı gibi küme [SUSE Linux Enterprise Server SAP uygulamaları için Azure vm'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse).
> - Azure sanal makinelerini ve Windows Server genişleme dosya sunucusu arasındaki iletişim yolları ayarlayın açıklandığı kadar [SAP ASCS/SCS örneği ile Azure dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). 
>
> Ağ sanal Gereçleri iletişim yolları, iki iletişim iş ortakları arasında kolayca çift ağ gecikmesi olabilir. Bunlar, SAP uygulama katmanı ve DBMS katmanı arasında kritik yollarda aktarım hızı da kısıtlayabilirsiniz. Bazı müşteri senaryolarda, ağ sanal Gereçleri Pacemaker Linux kümeleri başarısız olmasına neden olabilir. Linux Pacemaker düğümler arasındaki iletişimler SBD cihazını ağ sanal Gereci iletişim kurduğu durumlarda bunlar.  
> 

> [!IMPORTANT]
> Başka bir tasarım 's *değil* desteklenir SAP uygulama katmanı ve DBMS katman ayrımı olmayan farklı Azure sanal ağlarına [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) birbiriyle. Farklı Azure sanal ağları kullanarak yerine bir Azure sanal ağ içindeki alt ağlar SAP uygulama katmanında ve DBMS katman ayırmak önerilir. 
>
>Değil öneriye ve bunun yerine iki katmanı farklı sanal ağlara ayırmak karar verirseniz, iki sanal ağ olmalıdır [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 
>
> Unutmayın, ağ trafiğini iki [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağları olan aktarım maliyetleri tabidir. Birçok terabaytlarca oluşan büyük veri hacmi SAP uygulama katmanında ve DBMS katman arasında paylaşılmaz. SAP uygulama katmanında ve DBMS katmanı iki eşlenen Azure sanal ağları arasında arkadaşlarından, önemli maliyetleri birikebilir. 

SAP HANA çalıştırmayı Vm'leri yüklediğinizde, VM'lerin gerekir:

- İki sanal NIC yüklü. Bir NIC, yönetim alt ağına bağlanır. Başka bir NIC Azure VM'de SAP HANA örneği şirket içi ağ ya da diğer ağlara bağlanır.
- Her iki sanal NIC için dağıtılan statik özel IP adresleri.

> [!NOTE]
> Azure aracılığıyla statik IP adresleri atayarak bunları tek tek sanal Nıc'lere atayın anlamına gelir. Sanal bir NIC'ye konuk işletim sistemi içinde statik IP adresleri atamayın Olgu üzerinde bazı Azure Hizmetleri gibi Azure Backup kullanan, en azından birincil sanal NIC'yi DHCP ve statik IP adresleri için ayarlanır. Daha fazla bilgi için [sanal makine yedekleme sorunlarını giderme Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Bir VM'ye birden fazla statik IP adresleri atamak için birden çok sanal NIC bir VM'ye atayın.
>
>

Enduring dağıtımları için Azure'da bir sanal veri merkezi ağ mimarisi oluşturun. Bu mimari, şirket içi ayrı bir Azure sanal ağa bağlanan bir Azure sanal ağ geçidi ayrımı önerir. Bu ayrı bir sanal ağ, şirket içi veya İnternet'e bırakır tüm trafiği barındırır. Bu yaklaşımı kullanarak denetlemek için yazılım ve sanal veri merkezi, Azure'da bu ayrı hub sanal ağında girdiği trafiğini günlüğe kaydetme dağıtabilirsiniz. Bu şekilde barındıran tüm yazılım ve yapılandırmalar gelen ve giden trafiği Azure dağıtımınız için ilişkili bir sanal ağa sahip.


Sanal veri merkezi yaklaşımıyla ve ilgili Azure sanal ağ tasarımı ile ilgili daha fazla bilgi için bkz: 

- [Azure sanal veri merkezi: Ağ perspektifi](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter).
- [Azure sanal veri merkezi ve kurumsal denetim düzlemi](https://docs.microsoft.com/azure/architecture/vdc/).


>[!NOTE]
>Kullanarak bir hub sanal ağ ile bir uç sanal ağ arasında akan trafik [Azure sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) tabi ek [maliyetleri](https://azure.microsoft.com/pricing/details/virtual-network/). Bu maliyetleri temelinde, çalışan bir katı merkez-uç ağ tasarımı ve birden çok çalışan arasında ödün yapmanız gerekebilir [Azure ExpressRoute ağ geçitleri](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) uçlara yönelik sanal ağ eşlemesi atlamak için bağlanın. 
>
> Azure ExpressRoute ağ geçitleri tanıtmak ek [maliyetleri](https://azure.microsoft.com/pricing/details/vpn-gateway/) çok. Ayrıca, oturum açın, Denetim ve ağ trafiğini izlemek için kullandığınız üçüncü taraf yazılımları için ek maliyetler karşılaşabilirsiniz. Sanal Ağ eşlemesi ek ExpressRoute ağ geçitleri ve yazılım lisansları ile oluşturulan maliyetleri karşılaştırması aracılığıyla veri değişimi maliyetlerini göz önünde bulundurun. Sanal ağlar'yerine yalıtım birimleri olarak alt ağlar'ı kullanarak bir sanal ağ içinde mikro segmentasyona üzerinde karar verebilirsiniz.


IP adresleri atamak için kullanabileceğiniz farklı yöntemlere genel bakış için bkz. [IP adresi türleri ve ayırma yöntemleri azure'da](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

SAP HANA çalıştıran VM'ler için atanmış olan bir statik IP adresleri ile çalışır. Bazı configuration öznitelikleri HANA için IP adresleri başvuru nedenidir.

[Azure Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) SAP HANA örneği veya Sıçrama kutusu yönlendirilen trafiği yönlendirmek için kullanılır. Nsg'ler ve sonunda [uygulama güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) SAP HANA alt ağı ve yönetim alt ağı ile ilişkili.

Aşağıdaki görüntüde, SAP HANA için bir merkez ve uç sanal ağ mimarisini izleyen kaba dağıtım şema genel bir bakış gösterilmektedir:

![SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking.PNG)

Azure'da SAP HANA siteden siteye bağlantı olmadan dağıtmak için genel internet'ten SAP HANA örneği korumak ve ileri bir proxy'nin arkasındayken gizle. Bu temel bir senaryoda, dağıtım ana bilgisayar adlarını çözümlemek için Azure yerleşik DNS hizmetlere dayanır. Genel kullanıma yönelik IP adreslerini kullanıldığı daha karmaşık bir dağıtımda, Azure yerleşik DNS hizmetleri özellikle önemlidir. Azure Nsg'ler kullanın ve [Azure ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) azure'da Azure sanal ağ Mimarinizi içine internet'ten yönlendirme izlemek için. 

Aşağıdaki resimde, bir sanal ağ merkez ve uç mimarisi siteden siteye bağlantı olmadan SAP HANA dağıtmayı öğrenmek için kaba bir şema gösterilmektedir:
  
![Siteden siteye bağlantı olmadan SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Merkez ve uç sanal ağ mimarisini olmaksızın internet'ten erişimi denetleme ve izleme Azure ağ sanal Gereçleri kullanma başka bir açıklaması için bkz [yüksekorandakullanılabilirağsanalgereçlerinidağıtma](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configure-azure-infrastructure-for-sap-hana-scale-out"></a>SAP HANA genişletmek için Azure altyapısını yapılandırma
Microsoft, bir SAP HANA genişletme yapılandırma için sertifikalı bir M serisi VM SKU sahiptir. VM türü M128s bir ölçek genişletme için en fazla 16 düğümlerinin sertifikalıdır. Azure vm'lerde SAP HANA genişleme sertifikaları değişiklikler için bkz: [Iaas platformlar listesine sertifikalı](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Azure sanal makinelerinde genişleme yapılandırmaları dağıtmak için kullanılan en düşük işletim sistemi sürümleri şunlardır:

- SUSE Linux 12 SP3.
- Red Hat Linux 7.4.

16 düğümlü ölçek genişletme sertifika için:

- Ana düğüm bir düğümdür.
- Çalışan düğümü en fazla 15 düğüme var.

>[!NOTE]
>Azure VM ölçek genişletme dağıtımlarda, bekleme düğümü kullanmak mümkün değildir.
>

Neden bir bekleme düğüm yapılandıramazsınız iki nedeni vardır:

- Azure, bu noktada hiçbir yerel NFS hizmeti vardır. Sonuç olarak, NFS paylaşımlarını üçüncü taraf işlevselliği Yardımı ile yapılandırılması gerekir.
- Azure'da dağıtılan çözümleri ile üçüncü taraf NFS yapılandırmaları hiçbiri için SAP HANA depolama gecikme süresi ölçütleri gerçekleştirebilir.

Sonuç olarak, /hana/data ve /hana/log birimleri paylaşılamaz. Tek düğümlerin bu birimlerin kullanılmadığı bir SAP HANA bekleme düğümü genişleme yapılandırmasında kullanımını engeller.

Aşağıdaki resimde, bir genişleme yapılandırmasında tek bir düğüm için temel tasarım gösterilmektedir:

![Tek bir düğüm genişleme temelleri](media/hana-vm-operations/scale-out-basics.PNG)

SAP HANA genişleme için bir VM düğümünden temel yapılandırmasını aşağıdaki gibi görünür:

- /Hana/Shared için SUSE Linux üzerinde 12 SP3 tabanlı yüksek oranda kullanılabilir bir NFS küme kullanıma oluşturun. Bu küme ölçeği genişletme yapılandırma ve SAP NetWeaver veya BW/4hana'yı merkezi hizmetlerinizi /hana/shared NFS paylaşımlarını barındırır. Bu tür bir yapılandırma derleme hakkında daha fazla bilgi için bkz: [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
- Diğer tüm disk birimleri farklı düğümler arasında paylaşılmaz ve NFS tabanlı değildir. Bu makalenin sonraki bölümlerinde, yükleme yapılandırmalar ve adımlar paylaşılmayan /hana/data ve /hana/log ile ölçek genişletme HANA yüklemeler için sağlanır.

>[!NOTE]
>Yüksek oranda kullanılabilir bir NFS küme grafikleri şimdiye görüntülendiği şekilde SUSE Linux ile yalnızca desteklenir. Red Hat üzerinde alan yüksek oranda kullanılabilir bir NFS çözümü daha sonra önerilecektir.

Düğümler için birimleri boyutlandırma /hana/shared dışında ölçek büyütme aynıdır. Aşağıdaki tabloda, M128s VM SKU için önerilen boyutları ve türleri gösterilmektedir.

| VM SKU | RAM | Maksimum VM g/ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2.000 giB | 2.000 MB/sn |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını kontrol edin. İş yükü /hana/data ve /hana/log için daha yüksek birimleri gerektiriyorsa, Azure premium depolama VHD'ler sayısını artırın. Bir toplu listelenen arttıkça IOPS değerinden daha fazla VHD ile ve Azure sanal makine türünü sınırları dahilinde g/ç aktarım hızı boyutlandırma. Ayrıca yazma Hızlandırıcı /hana/log birimin form diskler için geçerlidir.
 

Ölçek genişletme için/hana/paylaşılan biriminin boyutu tek bir çalışan düğümü dört çalışan düğümü başına bellek boyutu olarak tanımlayan bir formülü için bkz: [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

SAP HANA genişleme sertifikalı M128s Azure VM kabaca 2 TB bellek ile ele varsayılarak, SAP öneriler olarak özetlenmiştir:

- Bir ana düğümü ve dört alt düğüm kadar /hana/shared birimin boyutu 2 TB'tır.
- Bir ana düğümü ve beş ve sekiz çalışan düğümleri için /hana/shared biriminin boyutu 4 TB'tır.
- Bir ana düğümü ve 9-12 çalışan düğümleri için /hana/shared birimin boyutunu 6 TB'tır.
- Bir ana düğümü ve 12-15 çalışan düğümleri için /hana/shared birimin boyutu 8 TB'tır.

Grafik Ölçeği genişletme SAP HANA VM tek düğüm yapılandırması görüntülenen diğer önemli tasarım alt ağ yapılandırması ile sanal ağdır. SAP HANA düğümler arasında iletişime geçilen istemci ve uygulama yönelik trafiği ayrımı önerir. 

Bu hedefe ulaşmak için kullanılan grafikler, gösterildiği gibi iki farklı sanal NIC, VM'e ekleyin. Her iki sanal NIC farklı alt ağlarda ve iki farklı IP adresine sahiptir. Ardından Nsg'ler veya kullanıcı tanımlı yollar kullanarak yönlendirme kuralları ile trafik akışını denetleme.

Özellikle Azure'da yok anlamına gelir ve yöntemlerini belirli bir sanal NIC kotalar ve hizmet kalitesi zorlamak için vardır. Sonuç olarak, istemci ve uygulama yönelik ve düğüm içi iletişimin ayrımı başka bir trafik akışı önceliğini fırsatı açmaz. Bunun yerine, bir ölçü genişleme yapılandırmaları düğüm içi iletişimin koruma güvenlik ayrımı kalır.  

>[!IMPORTANT]
>SAP ağ trafiğini bu makalede anlatıldığı gibi istemci ve uygulama yan ve düğüm içi trafiği ayrı önerir. Önceki grafikleri gösterildiği yere bir mimari koymak, öneririz.
>

Aşağıdaki görüntüde, ağ bir açısından en düşük gerekli ağ mimarisi gösterilmektedir:

![Tek bir düğüm genişleme temelleri](media/hana-vm-operations/scale-out-networking-overview.PNG)

Şu ana kadar desteklenen bir ana düğüm yanı sıra 15 çalışan düğümleri limitlerdir.

Aşağıdaki resimde, bir depolama açısından depolama mimarisi gösterilmektedir:


![Tek bir düğüm genişleme temelleri](media/hana-vm-operations/scale-out-storage-overview.PNG)

/Hana/shared birim üzerinde yüksek oranda kullanılabilir NFS Paylaşım Yapılandırması bulunur. Diğer tüm sürücüleri tek VM'ler için yerel olarak bağlanır. 

### <a name="highly-available-nfs-share"></a>Yüksek oranda kullanılabilir bir NFS paylaşım
Şu ana kadar yüksek oranda kullanılabilir bir NFS küme yalnızca SUSE Linux ile çalışmaktadır. Kurulum için bilgi [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs). SAP HANA örnekleri çalıştıran Azure sanal ağ dışındaki diğer bir HANA yapılandırmaları ile NFS küme paylaşmayın, aynı sanal ağda yükleyin. Kendi alt ağına yükleyin ve tüm rastgele trafiğini alt ağa erişebildiğinden emin olun. Bunun yerine, bu alt ağa trafiğin /hana/shared birime yürütülen VM IP adreslerini trafiği sınırlayın.

/Hana/shared trafiği yönlendiren bir HANA genişleme VM'nin sanal NIC'ye ilgili, öneriler şunlardır:

- /Hana/shared trafiği Orta olduğundan, yönlendirme, en düşük yapılandırmayı istemci ağa atanan sanal NIC'yi aracılığıyla.
- Sonuç olarak, üçüncü bir alt SAP HANA genişletme yapılandırma olduğu dağıttığınızda sanal ağ trafiği için/hana/paylaşılan, dağıtın. Bu alt ağda barındırılır üçüncü bir sanal NIC atayın. Üçüncü sanal NIC ve ilişkili IP adresi NFS paylaşımına trafiği için kullanın. Ardından, ayrı bir erişim ve yönlendirme kuralları uygulayabilirsiniz.

>[!IMPORTANT]
>SAP HANA, dağıtılan bir genişleme biçimde olan Vm'leri yüksek oranda kullanılabilir bir NFS hiçbir koşulda arasındaki ağ trafiğini yönlendirilir aracılığıyla bir [ağ sanal Gereci](https://azure.microsoft.com/solutions/network-appliances/) veya benzer sanal cihazlar. Azure Nsg yok gibi cihazlardır. SAP HANA çalıştıran sanal makineler yüksek oranda kullanılabilir NFS eriştiklerinde ağ sanal Gereçleri veya benzer sanal Gereçleri detoured emin emin olmak için yönlendirme kuralları paylaşmak denetleyin.
> 

Yüksek oranda kullanılabilir bir NFS küme SAP HANA yapılandırmaları arasında paylaşmak istiyorsanız, bu HANA yapılandırmaları aynı sanal ağa taşıyın. 
 

### <a name="install-an-sap-hana-scale-out-in-azure"></a>Azure'da bir SAP HANA genişleme yükleme
Bir ölçek genişletme SAP yapılandırmasını yüklemek için aşağıdaki genel adımları izleyin:

- Yeni dağıtın veya yeni bir Azure sanal ağ altyapısını uyarlama.
- Premium Azure tarafından yönetilen depolama birimi kullanarak yeni VM'ler dağıtın.
- Yeni bir dağıtın veya mevcut yüksek oranda kullanılabilir bir NFS küme uyarlayabilirsiniz.
- Örneğin, sanal makineler arasında düğüm içi iletişimin üzerinden yönlendirilmesini değil, emin olmak için ağ yönlendirme uyum bir [ağ sanal Gereci](https://azure.microsoft.com/solutions/network-appliances/). Aynı sanal makineleri ve yüksek oranda kullanılabilir bir NFS küme arasındaki trafiği için geçerlidir.
- SAP HANA ana düğüme yükleyin.
- SAP HANA ana düğümünün yapılandırma parametreleri uyarlayın.
- SAP HANA çalışan düğümü yükleme işlemine devam edin.

#### <a name="install-sap-hana-in-a-scale-out-configuration"></a>Bir ölçek genişletme yapılandırmasında SAP HANA yükleyin
Azure VM altyapınızı dağıtılır ve diğer tüm hazırlıkları tamamlandı. Şimdi, SAP HANA genişleme yapılandırmaları yüklemek için aşağıdaki adımları izleyin:

- SAP HANA ana düğüm SAP'nin belgelere göre yükleyin.
- *Yüklemeden sonra global.ini dosyasını değiştirin ve parametre Ekle ' basepath_shared = Hayır ' global.ini için*. Bu parametre düğümler arasında paylaşılan /hana/data ve /hana/log birimleri olmadan genişleme çalıştırmak SAP HANA sağlar. Daha fazla bilgi için [SAP notu #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- SAP HANA örneği global.ini parametresi değiştirdikten sonra yeniden başlatın.
- Ek çalışan düğümleri ekleyin. Daha fazla bilgi için [SAP HANA Yönetim Kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html). Yükleme sırasında veya daha sonra Örneğin, yerel hdblcm kullanarak SAP HANA düğümler arası iletişim için iç ağ belirtin. Daha fazla bilgi için [SAP notu #2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Bu Kurulum yordamı yüklediğiniz genişletme yapılandırma /hana/data ve /hana/log çalıştırmak için paylaşılmayan disk kullanır. / Hana/paylaşılan birimi üzerinde yüksek oranda kullanılabilir yerleştirildiğinde NFS paylaşım.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>Azure sanal makineleri için SAP HANA dinamik katmanlama 2.0

M serisi Azure sanal makineler'de SAP HANA sertifikaları yanı sıra SAP HANA dinamik katmanlama 2.0 (DT 2.0), Microsoft Azure'da da desteklenir. SAP HANA dinamik katmanlama belgelerine bağlantılar için "DT için bu makalenin sonraki bölümlerinde 2.0 belgelerine bağlantılar" bölümüne bakın. Ürünü yüklemek veya bir Azure VM içindeki SAP HANA Kokpit aracılığıyla, örneğin, işletim fark yoktur ancak azure'da resmi destek için birkaç önemli öğe zorunludur. Aşağıdaki önemli noktalara aşağıdaki bölümlerde açıklanmıştır. 

SAP HANA DT 2.0, SAP BW veya S4HANA tarafından desteklenmiyor. Ana kullanım örnekleri şu anda yerel HANA uygulamalardır.


### <a name="overview"></a>Genel Bakış

Aşağıdaki görüntüde, Microsoft Azure'da DT 2.0 Desteği'ne genel bakış sağlar. Resmi bir sertifika ile uyum sağlamak için zorunlu bu gereksinimleri izleyin:

- DT 2.0 adanmış bir Azure sanal makinesinde yüklü olmalıdır. SAP HANA çalıştığı aynı VM'de çalışmayabilir.
- SAP HANA ve DT 2.0 Vm'leri aynı Azure sanal ağ içinde dağıtılması gerekir.
- SAP HANA ve DT 2.0 Vm'leri Azure hızlandırılmış etkin ağ ile dağıtılması gerekir.
- Azure premium depolama DT 2.0 Vm'leri için depolama türü olmalıdır.
- Birden çok Azure diskleri DT 2.0 VM'ye bağlı olmalıdır.
- Bir yazılım oluşturma RAID veya şeritli birim LVM veya mdadm, üzerinden gerekli Azure disklerde şeritleme kullanarak.

Aşağıdaki bölümlerde daha fazla açıklama sağlayın.

![SAP HANA DT 2.0 mimarisine genel bakış](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Azure VM'de SAP HANA DT 2.0 için ayrılmış

Azure Iaas üzerinde DT 2.0 yalnızca adanmış bir VM üzerinde desteklenir. DT 2.0 HANA örneği çalıştığı aynı Azure sanal makinesinde çalıştırmak için izin verilmiyor. Başlangıçta, SAP HANA DT 2.0 çalıştırmak için iki VM türleri kullanılabilir:

- M64-32ms 
- E32sv3 

VM türü açıklamaları için bkz. [bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

Maliyetlerden tasarruf etmek için normal veri boşaltma hakkında olan DT 2.0 temel fikri verilen karşılık gelen VM boyutlarını kullanın mantıklıdır. Olası birleşimlerin için katı kural yoktur. Bu, belirli müşteri iş yüküne bağlıdır.

Aşağıdaki tablo, önerilen yapılandırmaları gösterir.

| SAP HANA VM türü | DT 2.0 VM türü |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


SAP HANA sertifikalı M serisi VM'ler M64-32ms ve E32sv3, gibi desteklenen DT 2.0 VM'ler ile tüm birleşimlerini mümkündür.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure ağ ve SAP HANA DT 2.0

DT 2.0 adanmış bir VM üzerinde yüklemek için DT 2.0 VM en az 10 GB ile SAP HANA VM arasındaki ağ aktarım hızı gerekir. Sonuç olarak, aynı Azure sanal ağ içindeki tüm sanal makineler yerleştirin ve Azure hızlandırılmış ağ iletişimi etkinleştirmek için zorunludur.

Azure hızlandırılmış ağ hakkında daha fazla bilgi için bkz: [hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

### <a name="vm-storage-for-sap-hana-dt-20"></a>SAP HANA DT 2.0 için VM depolama

DT 2.0 iyi göre disk g/ç aktarım hızı en az 50 MB/sn başına fiziksel çekirdek kılavuzdur. DT 2.0 için desteklenen iki Azure VM türleri belirtimi bakarak maksimum disk g/ç aktarım hızı sınırı VM için verilmiştir:

- **E32sv3**:   768 MB/önbelleğe alınmamış, fiziksel çekirdek başına 48 MB/sn oranını yani sn
- **M64-32ms**:  1000 MB/önbelleğe alınmamış, 62.5 MB/sn başına fiziksel çekirdek oranı yani sn

DT 2.0 VM'ye birden çok Azure diskleri ekleme ve VM başına disk aktarım hızı üst sınırı elde etmek için işletim sistemi düzeyinde şeritleme kullanarak yazılım RAID oluşturma gereklidir. Tek bir Azure diskinin VM sınırına ulaşması için bir işlem hacmi sağlayamaz. Azure premium depolama DT 2.0 çalıştırmak için zorunludur. 

- Kullanılabilir Azure disk türleri hakkında daha fazla bilgi için bkz. [Azure Iaas Windows Vm'leri için disk türü seçin](../../windows/disks-types.md).
- Yazılım RAID mdadm aracılığıyla oluşturma hakkında daha fazla bilgi için bkz. [bir Linux'ta yazılım RAID yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid).
- En fazla aktarım hızı için şeritli birim oluşturmak için LVM'yi yapılandırma hakkında daha fazla bilgi için bkz. [azure'da bir Linux VM'de LVM'yi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm).

Boyut gereksinimlerine bağlı olarak, bir VM'nin en yüksek aktarım ulaşmak için farklı seçenekler vardır. VM aktarım hızı üst sınırı elde etmek her DT 2.0 VM türü için olası veri birimi disk yapılandırmaları şunlardır. E32sv3 VM iş yükleri küçük bir giriş düzeyi olarak kabul edilir. Yeterince hızlı değilse M64-32ms VM'ye yeniden boyutlandırmak gerekli olabilir.

Büyük miktarda bellek M64-32ms VM olduğu için g/ç yük okuma açısından yoğun iş yükleri için özellikle sınırına ulaşmadığınız değil. Daha az sayıda disk kümesi içinde yeterli müşteriye özgü iş yüküne bağlı olabilir. Aşağıdaki disk yapılandırmaları, güvenli olması için en fazla aktarım hızı garanti seçilmiştir.


| VM SKU | Disk yapılandırması 1 | Disk yapılandırması 2 | Disk yapılandırması 3 | Disk yapılandırması 4 | 5 disk yapılandırması | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 16 TB -> | 4 x P40 8 TB -> | 5 x P30 -> 5 TB | 7 x P20 3,5 TB'a -> | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 2,5 TB -> | 6 x P15, 1,5 TB'ı -> | 


"Salt okunur" Azure ana bilgisayar önbelleği ayarı veritabanı yazılımını veri hacimleri için önerildiği şekilde kapatılması, özellikle okuma açısından yoğun iş yükü, g/ç performansını artırmak. İşlem günlüğü, "none". Azure konak disk önbelleği olmalıdır 

Günlük birimi boyutu için önerdiğimiz bir buluşsal yöntem, verilerin boyutunun yüzde 15 başlangıçtır. Günlük birimi oluşturmak için maliyet ve işleme gereksinimlerine bağlı olarak farklı Azure disk türleri kullanın. Günlük birimi için yüksek g/ç aktarım hızı gereklidir. 

VM kullanıyorsanız M64-32ms yazıp, etkinleştirmenizi öneririz [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). Yazma Hızlandırıcı, işlem günlüğü için en yüksek disk yazma gecikmesi sağlayan bir Azure özelliğidir. Yalnızca M serisi için kullanılabilir. Ancak, VM türü başına disk sayısı gibi dikkate alınması gereken bazı öğeler vardır. Yazma hızlandırıcı hakkında daha fazla bilgi için bkz. [yazma hızlandırıcıyı etkinleştirme](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


Aşağıdaki tabloda günlük birimi boyutunu yardımcı olması için bazı örnekler gösterilmektedir.

| veri birimi boyutunu ve disk türü | Günlük birimi ve disk yapılandırma 1 yazın. | Günlük birimi ve disk yapılandırma 2 yazın |
| --- | --- | --- |
| 4 x P50 16 TB -> | 5 x P20 2,5 TB -> | 3 x P30 -> 3 TB |
| 6 x P15, 1,5 TB'ı -> | 4 x P6 256 GB -> | 1 x P15 256 GB -> |


Benzer şekilde, SAP HANA genişleme, /hana/shared dizini DT 2.0 VM ve SAP HANA VM arasında paylaşılabilir olmalıdır. Yüksek oranda kullanılabilir bir NFS sunucusu olarak davranacak özel VM'ler kullanarak SAP HANA genişleme olduğu gibi aynı mimarisini kullanmanızı öneririz. Paylaşılan bir yedekleme birimi sağlamak için aynı tasarım kullanın. Ancak, yüksek kullanılabilirlik gerekiyorsa, ya da bir yedekleme sunucusu olarak davranacak şekilde yeterli depolama kapasitesine adanmış bir VM kullanmak için yeterli ise ilişkin karar size aittir.



### <a name="links-to-dt-20-documentation"></a>DT 2.0 belgelerine bağlantılar 

- [SAP HANA dinamik katmanlama yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA dinamik katmanlama ilgili öğreticiler ve kaynaklar](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA dinamik katmanlama kavram kanıtı](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
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
Azure ve şirket içi konumlar arasında siteden siteye bağlantı varsa ve SAP bileşenleri çalıştırıyorsanız, büyük olasılıkla zaten SAProuter çalıştırıyorsunuz. Bu durumda, uzak desteği için aşağıdaki adımları izleyin:

- Sanal makinenin özel ve statik IP adresi barındıran SAP HANA SAProuter yapılandırmasında korur.
- Barındıran 3299 TCP/IP bağlantı noktası üzerinden trafiğe izin verecek şekilde HANA VM alt ağının bir NSG yapılandırın.

Azure'a internet üzerinden bağlanmak ve VM ile SAP HANA için SAP yönlendirici yoksa, bileşenini yükleyin. Ayrı bir sanal makinede Yönetim Alt SAProuter yükleyin. 

Aşağıdaki görüntüde, siteden siteye bağlantı olmadan ve SAProuter SAP HANA dağıtma kaba bir şema gösterilmektedir:

![SAP HANA için bir siteden siteye bağlantı ve SAProuter dağıtım şema kaba](media/hana-vm-operations/hana-simple-networking3.PNG)

Sıçrama kutusu VM'niz değil, ayrı bir sanal makinede SAProuter yüklediğinizden emin olun. Ayrı sanal Makineyi statik bir IP adresi olmalıdır. SAP tarafından barındırılan SAProuter, SAProuter bağlanmak için bir IP adresi için SAP başvurun. SAP tarafından barındırılan SAProuter karşılık gelen sanal makinenizde yüklediğiniz SAProuter örneğinin ' dir. SAP IP adresinden SAProuter örneğinizin yapılandırmak için kullanın. Yapılandırma ayarlarında TCP bağlantı noktası 3299 yalnızca gerekli bağlantı noktasıdır.

Ayarlama ve SAProuter aracılığıyla uzak destek bağlantıları hakkında daha fazla bilgi için bkz. [SAP belgelerindeki](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Yerel Azure sanal makineler'de SAP HANA ile yüksek kullanılabilirlik
SAP uygulamalarını 12 SP1 için SUSE Linux Enterprise Server çalıştırırsanız, STONITH cihazlarla Pacemaker kümesi oluşturabilirsiniz. Cihazlar, HANA sistem çoğaltması ve otomatik yük devretme ile zaman uyumlu çoğaltma kullanan bir SAP HANA yapılandırması ayarlamak için kullanın. Kurulum yordamı hakkında daha fazla bilgi için bkz: [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
