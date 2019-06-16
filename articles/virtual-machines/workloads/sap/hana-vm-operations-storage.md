---
title: SAP HANA Azure sanal makine depolama yapılandırmaları | Microsoft Docs
description: SAP HANA, dağıtılan bunları sahip VM için depolama önerileri.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d062b6fff9693d5bda75edd65b8fe88d834eff57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735519"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>SAP HANA Azure sanal makine depolama alanı yapılandırmaları

Azure, Azure Vm'leri için SAP HANA çalıştırmayı uygun olan farklı depolama türlerini sağlar. SAP HANA dağıtım listesi gibi için kabul edilebilir bir Azure depolama türleri: 

- Standart SSD disk sürücüsü (SSD)
- Premium katı hal sürücülerine (SSD)
- Ultra yüksek SSD genel önizlemeye sunuldu ve üretim SAP uygulamalarıyla henüz desteklenmiyor

Şu disk türleri hakkında bilgi edinmek için bkz [bir disk türü seçin](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types)

Azure, Azure standart ve Premium depolama alanına VHD'ler için iki dağıtım yöntemleri sunar. Genel senaryo veriyorsa yararlanmak [Azure yönetilen disk](https://azure.microsoft.com/services/managed-disks/) dağıtımları. 

Depolama türleri ve IOPS ve depolama aktarım hızının, SLA'ları içeren listeyi gözden geçirip [yönetilen diskler için Azure belgeleri](https://azure.microsoft.com/pricing/details/managed-disks/).

**Öneri: Azure Premium depolama, Azure yazma Hızlandırıcı ile birlikte kullanın ve dağıtım için Azure yönetilen diskler kullanın**

Şirket içi dünyasında, nadiren g/ç alt sistemlerinin ve özellikleri hakkında önemli gerekiyordu. SAP HANA için en az depolama gereksinimlerinin karşılandığından emin olmak gereken gerecin satıcısına neden oldu. Azure altyapı kendiniz gibi bu gereksinimleri bazıları farkında olmalıdır. Bazı istemeden en düşük aktarım hızı özelliklerine gerek výsledek:

- Okuma/yazma etkinleştirileceği **/hana/günlük** 250 MB/sn 1 MB g/ç boyutu
- Etkinleştirme okuma en az 400 MB/sn için etkinlik **/hana/veri** 16 MB ve 64 MB g/ç boyutları için
- En az 250 MB/sn için yazma etkinliği etkinleştirin **/hana/veri** 16 MB ve 64 MB g/ç boyutu

Düşük depolama gecikme süresi gibi SAP HANA DBMS verileri bellek içinde saklamak gibi DBMS sistemler için önemlidir. Kritik depolama genellikle DBMS sistemleri işlem günlüğü yazma yoludur. Ancak aynı zamanda operations kayıt yazma veya kilitlenme kurtarma kritik sonra verileri bellek içinde yükleniyor. Bu nedenle, **zorunlu** için Azure Premium Diskler'den yararlanmaya **/hana/veri** ve **/hana/günlük** birimleri. En düşük aktarım hızı elde etmek için **/hana/günlük** ve **/hana/veri** RAID 0 oluşturmanızı sağlayacak dilediğiniz şekilde SAP tarafından MDADM veya LVM birden çok Azure Premium depolama diskleri kullanarak. Ve RAID birimleri olarak **/hana/veri** ve **/hana/günlük** birimleri. 

**Öneri: Stripe RAID 0 öneri boyutları olarak kullanmaktır:**

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

**Öneri: Bu gözlemlenen GÇ desenlerinden sonucunda SAP HANA, Azure Premium depolama kullanan farklı birimler için önbelleğe alma gibi ayarlanmalıdır:**

- **/ hana/veri** -önbelleksizlik
- **/ hana/günlük** - önbelleksizlik - M serisi (Bu belgenin sonraki bölümlerinde bakın) için özel durumu
- **/ hana/paylaşılan** - okuma önbelleğe alma


Ayrıca genel VM g/ç aktarım hızı boyutlandırma ya da bir VM için karar aklınızda bulundurun. VM depolama verimliliğini makalesinde genel belgelenen [bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

## <a name="linux-io-scheduler-mode"></a>Linux g/ç Zamanlayıcısını modu
Linux, birkaç farklı g/ç planlama modu vardır. Linux satıcılar ve SAP ortak öneri olan disk birimlerden g/ç Zamanlayıcı modunu yapılandırmak için **cfq** moduna **noop** modu. Ayrıntılar başvurulan [SAP notu #1984798](https://launchpad.support.sap.com/#/notes/1984787). 


## <a name="production-storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Azure M serisi sanal makineler için Azure yazma Hızlandırıcı ile üretim depolama çözümü
Azure yazma Hızlandırıcı Azure M serisi VM'ler için özel olarak kullanıma bir işlevdir. Adını belirten gibi Azure Premium Depolama'ya yönelik yazma işlemlerinin g/ç gecikme işlevselliğini amacı artırmaktır. SAP HANA için yazma Hızlandırıcı karşı kullanılmak üzere beklenen **/hana/günlük** yalnızca birim. Bu nedenle, **/hana/veri** ve **/hana/günlük** destekleyen Azure yazma Hızlandırıcı ile farklı birimler **/hana/günlük** yalnızca birim. 

> [!IMPORTANT]
> Yalnızca Azure yazma Hızlandırıcı ile SAP HANA sertifika Azure M serisi sanal makineler için olduğundan **/hana/günlük** birim. Sonuç olarak, üretim senaryosu Azure M serisi sanal makinelerde SAP HANA dağıtımları için Azure yazma Hızlandırıcı ile yapılandırılması beklenir **/hana/günlük** birim.  

> [!NOTE]
> Üretim senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

**Öneri: Üretim senaryoları için önerilen yapılandırmalar şöyle görünür:**

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri | / hana/günlük | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 giB | 1000 MB/sn | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 2 x P40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P40 |
| M208s_v2 | 2850 GiB | 1000 MB/sn | 4 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P40 |
| M208ms_v2 | 5700 GiB | 1000 MB/sn | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416s_v2 | 5700 GiB | 2000 MB/sn | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416ms_v2 | 11400 giB | 2000 MB/sn | 8 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P50 |

M416xx_v2 VM türleri henüz Microsoft tarafından ortak olarak kullanılabilir duruma getirilmez. Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını kontrol edin. İş yükü için daha yüksek birimleri gerektiriyorsa **/hana/veri** ve **/hana/günlük**, Azure Premium depolama VHD'leri sayısını artırmanız gerekir. Bir toplu listelenen arttıkça IOPS değerinden daha fazla VHD ile ve Azure sanal makine türünü sınırları dahilinde g/ç aktarım hızı boyutlandırma.

Azure yazma Hızlandırıcı, yalnızca çalışır birlikte [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/). Bu nedenle en az Azure Premium depolama diskleri oluşturan **/hana/günlük** birim yönetilen diskler olarak dağıtılması gerekiyor.

Azure Premium depolama VHD'leri Azure yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlar şu şekildedir:

- M128xx ve M416xx bir VM 16 VHD'ler
- M64xx ve M208xx bir VM için 8 VHD'ler
- Bir M32xx 4 VHD'ler VM

Makalesinde Azure yazma Hızlandırıcı etkinleştirme hakkında daha ayrıntılı yönergeler bulunabilir [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Ayrıntıları ve kısıtlamaları Azure yazma Hızlandırıcı için aynı belgelerinde bulunabilir.

**Öneri: Yazma Hızlandırıcı /hana/log birimin oluşturan diskler için kullanmanız gerekir**


## <a name="cost-conscious-azure-storage-configuration"></a>Maliyet bilinçli Azure depolama yapılandırması
Aşağıdaki tabloda, Azure Vm'leri üzerinde SAP HANA ana müşterileri de VM türleri bir yapılandırılmasını gösterir. SAP HANA için tüm en az depolama ölçütleri karşılamıyor olabilir veya SAP HANA ile SAP tarafından resmi olarak desteklenmez bazı VM türleri olabilir. Ancak şu ana kadar sorunsuz üretim dışı senaryolar için gerçekleştirmek için bu sanal makineler olduğu görülüyor. **/Hana/veri** ve **/hana/günlük** tek bir birim için birleştirilir. Sonuç olarak Azure yazma Hızlandırıcı kullanımı bir sınırlama ıops'ye hale gelebilir.

> [!NOTE]
> Desteklenen SAP senaryoları için belirli bir sanal makine türü için SAP HANA SAP tarafından desteklenip desteklenmediğini kontrol [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

> [!NOTE]
> Maliyet bilinçli bir çözümü, eski önerileri farklıdır sunucudan taşımak için [Azure standart HDD diskleri](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) daha iyi gerçekleştirmek için ve daha güvenilir [Azure standart SSD disk](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd)


| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri ve/hana/günlük<br /> LVM'yi veya MDADM Şerit | / hana/paylaşılan | / root birimi | / usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 giB | 768 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E16v3 | 128 GiB | 384 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E32v3 | 256 GiB | 768 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| E64v3 | 432 giB | 1200 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| GS5 | 448 GiB | 2000 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| M32ts | 192 giB | 500 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M32ls | 256 GiB | 500 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M64ls | 512 GiB | 1000 MB/sn | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 |1 x E30 |
| M64s | 1000 giB | 1000 MB/sn | 2 x P30 | 1 x E30 | 1 x E6 | 1 x E6 |2 x E30 |
| M64ms | 1750 giB | 1000 MB/sn | 3 x P30 | 1 x E30 | 1 x E6 | 1 x E6 | 3 x E30 |
| M128s | 2000 giB | 2000 MB/sn |3 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E40 |
| M128ms | 3800 giB | 2000 MB/sn | 5 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E50 |
| M208s_v2 | 2850 GiB | 1000 MB/sn | 4 x P30 | 1 x E30 | 1 x E10 | 1 x E6 |  3 x E40 |
| M208ms_v2 | 5700 GiB | 1000 MB/sn | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416s_v2 | 5700 GiB | 2000 MB/sn | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416ms_v2 | 11400 giB | 2000 MB/sn | 8 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E50 |


M416xx_v2 VM türleri henüz Microsoft tarafından ortak olarak kullanılabilir duruma getirilmez. Daha küçük bir VM ile 3 x P20 oversize birimleri ayarına göre alan öneriler ilgili türler için önerilen disk [SAP TDI depolama teknik incelemesi](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ancak tabloda gösterildiği seçimi, SAP HANA için yeterli disk aktarım hızı çalışıldı. Değişiklikleri gerekiyorsa **/hana/yedekleme** iki kez bellek birimine temsil eden yedeklemeler tutmak için boyutta, birim ayarlamak ücretsiz gönderebilirsiniz.   
Farklı önerilen birimler için depolama verimliliğini çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını kontrol edin. İş yükü için daha yüksek birimleri gerektiriyorsa **/hana/veri** ve **/hana/günlük**, Azure Premium depolama VHD'leri sayısını artırmanız gerekir. Bir toplu listelenen arttıkça IOPS değerinden daha fazla VHD ile ve Azure sanal makine türünü sınırları dahilinde g/ç aktarım hızı boyutlandırma. 

> [!NOTE]
> Yukarıdaki yapılandırma öğesinden yararlı değildir [Azure sanal makine tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) Azure Premium depolama ve Azure standart depolama bir karışımını kullanır. Ancak, maliyetleri iyileştirmek için seçimi seçildi. Yukarıda Azure Standard Storage (VM yapılandırması Azure tek VM SLA ile uyumlu hale getirmek için Sxx) olarak listelenen tüm diskler için Premium depolama seçmeniz gerekir.


> [!NOTE]
> Belirtilen disk yapılandırması önerileri SAP altyapısını sağlayıcıları ifade en düşük gereksinimler hedeflediğiniz. Gerçek müşteri dağıtımları ve iş yükü senaryoları, burada bu önerileri yine de yeterli özellikleri sağlamadı durumlarda karşılaşıldı. Bunlar burada bir müşteri HANA yeniden başlatmadan sonra verileri daha hızlı bir şekilde yeniden yüklenmesi gereken durumlar olabilir veya burada depolama yapılandırmaları gerekli daha yüksek bant yedekleme. Dahil edilen diğer durumlarda **/hana/günlük** burada 5000 IOPS değil belirli iş yükü için yeterli. Bu nedenle bu önerileri bir başlangıç olarak işaret ve uyum Al iş yükü gereksinimlerine göre.
>  

## <a name="azure-ultra-ssd-storage-configuration-for-sap-hana"></a>SAP HANA için Azure Ultra SSD depolama yapılandırması
Microsoft adlı yeni bir Azure depolama türü ile tanışın sürecinde [Azure Ultra SSD](https://azure.microsoft.com/updates/ultra-ssd-new-azure-managed-disks-offering-for-your-most-latency-sensitive-workloads/). Şu ana kadar sunulan Azure depolama ve Ultra SSD arasında büyük fark, disk özellikleri için disk boyutunu artık bağlandığına değil ise. Bir müşteri olarak bu özellikler için Ultra yüksek SSD tanımlayabilirsiniz:

- 4 GiB 65.536 Gib'a yeniden arasında değişen bir diskin boyutu
- 100 IOPS IOPS aralığından 160K IOPS için (en fazla VM türleri de bağlıdır)
- Depolama performansını 2.000 MB/sn için 300 MB/sn

Ayrıntılar için makalesi Ara [Duyurusu Ultra SSD – yeni nesil Azure diskleri teknoloji (Önizleme)](https://azure.microsoft.com/blog/announcing-ultra-ssd-the-next-generation-of-azure-disks-technology-preview/)

UltraSSD, boyutu, IOPS ve disk aktarım hızı aralığı karşıladığı tek bir diske tanımlamak için olanağını sağlar. IOPS ve depolama aktarım hızı gereksinimlerini karşılamak birimleri oluşturmak için mantıksal birim yöneticileri LVM veya Azure Premium depolama üzerinde MDADM gibi kullanmak yerine. Yapılandırma karma UltraSSD ve Premium depolama çalıştırabilirsiniz. Sonuç olarak, performans kritik /hana/data ve /hana/log birimleri UltraSSD kullanımını sınırlamak ve Azure Premium depolama ile diğer birimleri kapsar

| VM SKU | RAM | En çok, VM G/Ç<br /> Aktarım hızı | / hana/veri hacmi | / hana/veri g/ç aktarım hızı | / hana/veri IOPS | / hana/günlük birimi | / hana/günlük g/ç aktarım hızı | / hana/günlük IOPS |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/sn | 250 GB | 500 MB/sn | 7500 | 256 GB | 500 MB/sn  | 2000 |
| M32ls | 256 GiB | 500 MB/sn | 300 GB | 500 MB/sn | 7500 | 256 GB | 500 MB/sn  | 2000 |
| M64ls | 512 GiB | 1000 MB/sn | 600 GB | 500 MB/sn | 7500 | 512 GB | 500 MB/sn  | 2000 |
| M64s | 1000 giB | 1000 MB/sn |  1200 GB | 500 MB/sn | 7500 | 512 GB | 500 MB/sn  | 2000 |
| M64ms | 1750 giB | 1000 MB/sn | 2100 GB | 500 MB/sn | 7500 | 512 GB | 500 MB/sn  | 2000 |
| M128s | 2000 giB | 2\.000 MB/sn |2400 GB | 1200 MB/sn |9000 | 512 GB | 800 MB/sn  | 2000 | 
| M128ms | 3800 giB | 2\.000 MB/sn | 4800 GB | 1200 MB/sn |9000 | 512 GB | 800 MB/sn  | 2000 | 
| M208s_v2 | 2850 GiB | 1000 MB/sn | 3500 GB | 1000 MB/sn | 9000 | 512 GB | 500 MB/sn  | 2000 | 
| M208ms_v2 | 5700 GiB | 1000 MB/sn | 7200 GB | 1000 MB/sn | 9000 | 512 GB | 500 MB/sn  | 2000 | 
| M416s_v2 | 5700 GiB | 2\.000 MB/sn | 7200 GB | 1500 M / sn | 9000 | 512 GB | 800 MB/sn  | 2000 | 
| M416ms_v2 | 11400 giB | 2\.000 MB/sn | 14400 GB | 1500 MB/sn | 9000 | 512 GB | 800 MB/sn  | 2000 |   

M416xx_v2 VM türleri henüz Microsoft tarafından ortak olarak kullanılabilir duruma getirilmez. Listelenen değerler bir başlangıç noktası olacak şekilde tasarlanmıştır ve gerçek taleplerini karşı değerlendirilmesi gerekir. Azure Ultra SSD ile avantajı, IOPS ve aktarım hızı için değerler sanal makineyi gerek kalmadan uyarlanabilir veya iş yükü durdurma sisteme uygulanacak olmasıdır.   

> [!NOTE]
> Şu ana kadar depolama anlık görüntüleri UltraSSD depolama ile kullanılamaz. Bu VM anlık görüntüleri Azure yedekleme Services ile kullanımını engeller.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz.

- [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).