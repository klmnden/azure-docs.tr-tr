---
title: SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Docs
description: Yüksek kullanılabilirlik ve Azure (büyük örnekler) üzerinde SAP HANA olağanüstü durum kurtarma planı oluşturun
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2445713aa5d6a839950ca0fe9567133c06d1ffa
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062250"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA büyük örnekleri azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma 

>[!IMPORTANT]
>Bu belge SAP HANA yönetim belgeleri veya SAP notları hiçbir yerini alır. Okuyucu düz anlayış ve SAP HANA yönetim ve yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma konuları ile özellikle işlemleri uzmanlığa sahip beklenir. Bu belgede, SAP HANA Studio'dan ekran görüntüleri gösterilir. İçerik, yapı ve SAP Yönetim Araçları ve araçları kendilerini ekranlar yapısını SAP HANA yayın için yayın ' değiştirebilirsiniz.

Adımlar ve ortamınızda ve HANA sürümleri ve sürümler ile gerçekleştirilen işlemler çalışma önemlidir. Bu belgede açıklanan bazı işlemleri daha iyi bir genel anlamak için Basitleştirilmiş ve son işlemi el kitapları için ayrıntılı adımlar kullanılacak amaçlanmamıştır. İşlemi el yapılandırmalarınızı için kitapları oluşturmak istiyorsanız, test ve süreçlerinizi uygulamak ve belirli yapılandırmalarınız ilgili işlemleri belge gerekir. 


Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DR) kritik SAP HANA Azure (büyük örnekler) sunucuda çalışan önemli yönleri şunlardır. SAP, sistem Tümleştirici ve Microsoft ile düzgün şekilde mimari ve sağ yüksek kullanılabilirlik ve olağanüstü durum kurtarma stratejilerini uygulamak için iş önemlidir. Ortamınıza özgü kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi dikkate almak önemlidir.

Microsoft, HANA büyük örnekleri ile bazı SAP HANA yüksek kullanılabilirlik özelliklerini destekler. Bu özellikler şunları içerir:

- **Depolama çoğaltma**: depolama sistemin başka bir Azure bölgesindeki başka bir HANA büyük örneği damga tüm veri çoğaltma olanağı. SAP HANA bu yöntem bağımsız olarak çalışır. Bu işlev, HANA büyük örnekleri için sunulan varsayılan olağanüstü durum kurtarma mekanizmadır.
- **HANA sistem çoğaltma**: [SAP HANA tüm veri çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) için ayrı bir SAP HANA sistemi. Kurtarma süresi hedefi düzenli aralıklarla veri çoğaltma ile en aza indirilir. SAP HANA zaman uyumsuz ve zaman uyumlu bellek içi ve zaman uyumlu modda destekler. Zaman uyumlu modda aynı veri merkezinde veya küçüktür 100 km birbirinden içinde olan SAP HANA sistemler için kullanılır. Geçerli tasarım HANA büyük örneği Damgalar ile HANA sistem çoğaltma tek bir bölge içinde yüksek kullanılabilirlik için kullanılabilir. HANA sistem çoğaltma, başka bir Azure bölgesi olağanüstü durum kurtarma yapılandırmaları için üçüncü taraf ters proxy ya da yönlendirme bileşeni gerektirir. 
- **Konak otomatik yük devretme**: HANA sistem çoğaltma için bir alternatif SAP HANA için bir yerel arıza kurtarma çözümü. Ana düğüm kullanılamaz duruma gelirse, bir veya daha fazla bekleme SAP HANA düğümleri genişleme modunda yapılandırın ve SAP HANA otomatik olarak bir bekleme düğüme yöneltilir.

SAP HANA (büyük örnekler) azure'da dört jeopolitik alanlarda (ABD, Avustralya, Avrupa ve Japonya) iki Azure bölgelerindeki sunulur. HANA büyük örneği Damgalar barındıran iki bölgede coğrafi alanda ayrı Adanmış ağ bağlantı hatları için bağlanır. Bunlar depolama anlık görüntüleri çoğaltmak için olağanüstü durum kurtarma yöntemleri sağlamak için kullanılır. Çoğaltma varsayılan olarak kurulmaz ancak olağanüstü durum kurtarma işlevini sipariş müşteriler için ayarlanmadı. Depolama çoğaltma HANA büyük örnekleri için depolama anlık görüntüleri kullanımını bağımlıdır. Farklı bir jeopolitik alanında olan bir DR bölge olarak Azure bölgesini seçmek mümkün değildir. 

Aşağıdaki tabloda şu anda desteklenen yüksek kullanılabilirlik ve olağanüstü durum kurtarma yöntemleri ve birleşimleri gösterilmektedir:

| HANA büyük durumlarda desteklenen senaryosu | Yüksek kullanılabilirlik seçeneği | Olağanüstü durum kurtarma seçeneği | Yorumlar |
| --- | --- | --- | --- |
| Tek düğüm | Mevcut değil. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu. | |
| Konak otomatik yük devretme: genişletme (ile veya bekleme olmadan)<br /> 1 + 1 dahil | Etkin rolü alma bekleme ile mümkün.<br /> HANA rol anahtarı denetler. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltma kullanarak DR eşitleme. | HANA birim kümeleri tüm düğümlere bağlı.<br /> DR sitesi düğümleri aynı sayıda olmalıdır. |
| HANA sistem çoğaltma | Birincil veya ikincil kurulumu ile mümkün.<br /> İkincil bir yük devretme durumunda birincil rolü taşır.<br /> Yük devretme denetim HANA sistem çoğaltma ve işletim sistemi. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltma kullanarak DR eşitleme.<br /> HANA sistem çoğaltma kullanarak DR henüz olmayan üçüncü taraf bileşenler mümkün değildir. | Disk birimleri ayrı kümesi her düğüme bağlı.<br /> Yalnızca disk birimleri üretim sitedeki ikincil çoğaltmasının DR konuma çoğaltılan.<br /> Bir dizi birimleri DR sitede gereklidir. | 

Ayrılmış bir DR Kurulum burada DR sitesi HANA büyük örneği biriminde herhangi bir iş yükü veya üretim dışı sistem çalıştırmak için kullanılmayan ' dir. Birim pasif ve yalnızca bir olağanüstü durum yük devretme yürütülürse dağıtılır. Ancak, bu kurulum birçok müşteri için tercih edilen bir seçenek değil.

Başvuran [HLI desteklenen senaryoları](hana-supported-scenario.md) Mimarinizi için depolama düzeni ve ethernet ayrıntıları öğrenin.

> [!NOTE]
> [SAP HANA MCOD dağıtımları](https://launchpad.support.sap.com/#/notes/1681092) (birden çok HANA örnekleri tek bir birim üzerinde) senaryoları iş HA ve DR yöntemleri üst üste getirme tabloda listelendiği gibi. Bir özel durum HANA sistem çoğaltma üzerinde Pacemaker dayalı bir otomatik yük devretme kümesi ile kullanılır. Böyle bir durumda, yalnızca birim başına bir HANA örneği destekler. İçin [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000) dağıtımları, yalnızca depolama tabanlı olmayan HA ve DR yöntemleri iş birden fazla Kiracı dağıtılırsa. Dağıtılan bir kiracı ile listelenen tüm yöntemleri geçerli değil.  

Çok amaçlı DR Kurulum, DR sitesi HANA büyük örneği biriminde üretim dışı iş yükü çalıştığı ' dir. Üretim dışı sistemi Kapat olağanüstü durum halinde depolama çoğaltılan (ek) birim kümeleri bağlayın ve üretim HANA örneği başlatın. HANA büyük örneği olağanüstü durum kurtarma işlevini kullanan müşterilerin çoğu, bu yapılandırma kullanın. 


Aşağıdaki SAP makalelerinde SAP HANA yüksek kullanılabilirlik hakkında daha fazla bilgi bulabilirsiniz: 

- [SAP HANA yüksek kullanılabilirlik Teknik İnceleme](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA Yönetim Kılavuzu](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP HANA sistem çoğaltma SAP HANA Akademi Video](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP destek Not #1999880 – SAP HANA sistem çoğaltma hakkında SSS](https://apps.support.sap.com/sap/support/knowledge/preview/en/1999880)
- [Yukarı destek Not #2165547 – SAP HANA geri SAP ve SAP HANA sistem çoğaltma ortamında geri yükleme](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [En az/sıfır kapalı kalma süresi ile donanım Exchange için destek Not #1984882 – kullanarak SAP HANA sistem çoğaltma SAP](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Olağanüstü durum kurtarma HANA büyük örneğiyle ağ konuları

HANA büyük örneklerinin olağanüstü durum kurtarma işlevsellikten yararlanmak için ağ bağlantısı iki Azure bölgeleri tasarlamanız gerekir. Şirket içi ana Azure bölgenizde Azure ExpressRoute bağlantı hattı bağlantısı ve şirket içi olağanüstü durum kurtarma bölgenizi kadar olan başka bir bağlantı hattı bağlantı gerekir. Bu ölçü, içinde bir sorun var. bir Microsoft Enterprise sınır yönlendiricisi (MSEE) konum dahil olmak üzere bir Azure bölgesinde bir durum kapsar.

İkinci bir ölçü bir bölgede başka bir bölgede HANA büyük örnekleri bağlayan bir expressroute bağlantı hattı için Azure (büyük örnekler) üzerinde SAP HANA bağlanmak tüm Azure sanal ağları bağlanabilir. Bu *arası bağlantı*, bir Azure üzerinde çalışan hizmetleri bölge 1'deki sanal ağ HANA büyük örneği birimlerinde bölge 2 ve diğer yönden bağlanabilir. Bu ölçü Azure ile şirket içi konumunuz bağlanan yalnızca biri MSEE konumları çevrimdışı bir durumda giderir.

Aşağıdaki grafikte bir olağanüstü durum kurtarma durumları için esnek yapılandırması gösterilmektedir:

![Olağanüstü durum kurtarma için en uygun yapılandırma](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>HANA büyük örnekleri depolama çoğaltması olağanüstü durum kurtarma için diğer gereksinimleri

Bir olağanüstü durum kurtarma Kurulumu HANA büyük örneğiyle için yukarıdaki gereksinimlerine ek olarak şunları yapmalısınız:

- SAP HANA SKU'ları üretim aynı boyutta, Azure (büyük örnekler) SKU'ları üzerinde sipariş ve olağanüstü durum kurtarma bölgede dağıtın. Geçerli müşteri dağıtımlarında, bu örnekler üretim dışı HANA örnekleri çalıştırmak için kullanılır. Bu yapılandırmalar denir *çok amaçlı DR kurulumları*.   
- DR sitesinde ek depolama alanı, SAP HANA olağanüstü durum kurtarma sitesini kurtarmak istediğiniz Azure (büyük örnekler) SKU üzerinde her sipariş edin. Ek depolama alanı satın alma, depolama birimlerini ayırmak olanak tanır. Olağanüstü durum kurtarma Azure bölgesi, üretime Azure bölgesi depolama çoğaltmayı hedefi olan birimler ayırabilirsiniz.
- Burada birincil HSR ayarladıktan ve DR siteye göre depolama çoğaltma kurulumu, durumda, bu nedenle hem birincil hem de DR sitesinde ek depolama alanı satın almanız gerekir ve ikincil düğümü veri DR sitesi için çoğaltılmış.

 

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

En önemli yönlerinden işletim veritabanlarına geri dönülemez olaylarından korumak için biridir. Bu olaylar nedenini herhangi bir şeyin doğal afetler basit kullanıcı hataları olabilir.

Zaman içinde bir noktaya geri yüklemenizi yeteneği olan bir veritabanını, yedekleme (gibi birisi kritik verileri silmeden önce), mümkün olduğunca yakın önce kesintisi olduğu şekilde bir duruma geri yükleme sağlar.

En iyi sonuçlar için yedeklemeler iki tür gerçekleştirilmesi gerekir:

- Veritabanı Yedeklemeleri: tam, artımlı ya da değişiklik yedekleme
- İşlem günlüğü yedeklemeleri

Bir uygulama düzeyinde gerçekleştirilen tam veritabanı yedeklemeleri yanı sıra depolama anlık görüntüleri ile yedeklemelerini gerçekleştirebilir. Depolama anlık görüntüleri, işlem günlüğü yedeklemeleri değiştirmeyin. İşlem günlüğü yedeklemeleri, veritabanını zaman içinde belirli bir noktaya geri yüklemenizi veya zaten yürütülen işlemler günlüklerinden boşaltmak için önemli kalır. Ancak, depolama anlık görüntüleri, veritabanının İleri alma görüntüsünü hızlı bir şekilde sağlayarak kurtarma hızlandırabilir. 

SAP HANA Azure (büyük örnekler) üzerinde iki yedekleme ve geri yükleme seçenekleri sunar:

- Yazılanları (DIY). Yeterli disk alanı olduğundan emin olmak için hesaplamak sonra tam veritabanı ve günlük yedekleri aşağıdaki disk yedekleme yöntemlerden birini kullanarak gerçekleştirin. Ya da doğrudan HANA büyük örneği birimlerine veya ağ dosya paylaşımları'için (NFS), bir Azure sanal makine (VM) ayarlanır bağlı birimleri yedekleyebilirsiniz. İkinci durumda, müşterilerin azure'da bir Linux VM ayarlama, Azure Storage VM'e ekleyin ve bu VM'de yapılandırılmış bir NFS sunucusu üzerinden depolama paylaşmak. Doğrudan HANA büyük örneği birimlerine ekleme birimleri karşı yedekleme yapıyorsanız, (Azure depolama birimine dayalı NFS paylaşımlarını dışarı aktaran bir Azure VM ayarladıktan sonra) yedeklemeler için Azure storage hesabı kopyalamanız gerekir. Bir Azure yedekleme kasası veya Azure soğuk depolama de kullanabilirsiniz. 

   Başka bir seçenek bir Azure depolama hesabı kopyalandıktan sonra yedekleri depolamak için bir üçüncü taraf veri koruma aracını kullanmaktır. Dıy yedekleme seçeneği de uyumluluk ve denetleme amaçları için uzun süreler depolamak için gereken verileri için gerekli olabilir. Her durumda, bir VM ve Azure Storage ile temsil edilen NFS paylaşımlarını içine yedeklemeleri kopyalanır.

- Altyapı yedekleme ve geri yükleme işlevselliği. Ayrıca, yedekleme kullanabilirsiniz ve SAP HANA Azure (büyük örnekler) üzerinde temel alınan alt yapısını sağlayan işlevselliği geri yükleyin. Bu seçenek, yedekleme ve hızlı geri yükleme gereksinimini karşılar. Bu bölümde rest HANA büyük örnekleriyle sunulan yedekleme ve geri yükleme işlevselliği giderir. Bu bölümde ayrıca ilişki yedekleme kapsayan ve geri yükleme sahip için olağanüstü durum kurtarma işlevi HANA büyük örnekleri tarafından sunulan.

>   [!NOTE]
>   Altyapının HANA büyük örnekleri tarafından kullanılan anlık görüntü teknolojisi SAP HANA anlık görüntü bir bağımlılığa sahiptir. Bu noktada, SAP HANA anlık görüntüleri SAP HANA çok müşterili veritabanı kapsayıcıların birden çok Kiracı ile birlikte çalışmaz. Yalnızca bir kiracı dağıtılır, SAP HANA anlık görüntüleri iş ve bu yöntem kullanılabilir.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>SAP HANA depolama anlık görüntüleri (büyük örnekler) Azure üzerinde kullanma

SAP HANA Azure (büyük örnekler) üzerinde temel alınan depolama altyapısını birimlerin depolama anlık görüntüleri destekler. Yedekleme ve geri yükleme birimlerin desteklenir, aşağıdaki noktaları ile:

- Tam veritabanı yedeklemeleri yerine depolama birim anlık görüntülerini üzerinde düzenli aralıklarla alınır.
- Bir anlık görüntü /hana/data ve /hana/shared (/usr/sap içerir) üzerinden tetiklendiğinde birimler, anlık görüntü teknoloji depolama anlık görüntü yürütülmeden önce anlık görüntü bir SAP HANA başlatır. Bu SAP HANA anlık görüntü Kurulum son günlük geri yüklemeler için depolama anlık görüntü kurtarma işleminden sonra noktasıdır. HANA anlık görüntü başarılı olması etkin bir HANA örneği gerekir.  HSR senaryosunda, depolama anlık HANA anlık görüntü nerede gerçekleştirilemiyor geçerli ikincil düğümde desteklenmiyor.
- SAP HANA anlık görüntü depolama anlık görüntü başarıyla yürütüldükten sonra silinir.
- İşlem günlüğü yedeklemeleri sık gerçekleştirilen ve /hana/logbackups birim veya Azure'da depolanır. Anlık ayrı olarak almak için işlem günlüğü yedeklemeleri içeren /hana/logbackups birimi tetikleyebilir. Bu durumda, bir HANA anlık görüntüsü yürütülecek gerekmez.
- Bir veritabanı belirli bir noktaya zamanında geri yüklemeniz gerekiyorsa, bu Microsoft Azure desteği (bir üretim kesinti) veya SAP HANA belirli depolama anlık görüntüye geri Azure Hizmet Yönetimi isteyin. Bir planlı bir korumalı alan sistem geri yükleme özgün durumuna örneğidir.
- Depolama anlık görüntüsüne dahil SAP HANA anlık görüntü yürütülen ve depolama anlık görüntü alındıktan sonra depolanan işlem günlüğü yedeklemeleri uygulamak için uzaklık bir noktadır.
- Bu işlem günlüğü yedeklemeleri, veritabanını zaman içinde geri belirli bir noktaya geri yüklemenizi alınır.

Depolama anlık görüntüleri birimlerin üç sınıfları hedefleme gerçekleştirebilirsiniz:

- Birleştirilmiş bir anlık görüntü/hana/verileri ve (/ usr/sap içerir) /hana/shared üzerinde. Bu anlık görüntü depolama anlık görüntü için hazırlık olarak bir SAP HANA anlık görüntü oluşturulmasını gerektirir. SAP HANA anlık görüntü veritabanı açısından bir depolama tutarlı bir durumda olduğundan emin olur. Üzerinde ve geri yüklemek için başka bir deyişle ayarlamak için bir noktası işlem çıkar.
- Ayrı bir anlık görüntü/hana/logbackups üzerinden.
- Bir işletim sistemi bölümü.

En son anlık görüntü komut dosyaları ve belgelerini almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="storage-snapshot-considerations"></a>Depolama anlık görüntü konuları

>[!NOTE]
>Depolama anlık görüntüleri HANA büyük örneği birimlerine ayrılan depolama alanını kullanabilir. Aşağıdaki depolama anlık görüntüler ve kaç tane depolama anlık görüntüleri tutmak için zamanlama yönlerini dikkate almanız gerekir. 

SAP HANA Azure (büyük örnekler) üzerinde depolama anlık görüntüleri belirli mekanikleri şunları içerir:

- Belirli bir depolama anlık görüntü (noktasında zaman geçen süre) çok az depolama kullanır.
- Veri içerik değişikliklerini ve dosya depolama biriminde değiştirmek SAP HANA veri içeriği anlık görüntü veri değişikliklerini yanı sıra, özgün blok içeriği depolamak gerekir.
- Sonuç olarak, depolama anlık görüntü boyutunu artırır. Uzun süre anlık görüntü var, daha büyük depolama anlık görüntüsü olur.
- SAP HANA veritabanına birimin depolama anlık görüntü alanı kullanımını daha büyük bir depolama anlık ömrü boyunca yapılan daha fazla değişiklik.

SAP HANA azure'da (büyük örnekler) SAP HANA veri ve günlük birimlerin sabit birim boyutlarını birlikte gönderilir. Bu birimlerin anlık görüntüleri gerçekleştirme birim alanınızı eats. Depolama anlık görüntüleri zamanlamak ne zaman belirlemeniz gerekir. Ayrıca yanı sıra depolama birimleri alanı tüketimini izlemek depoladığınız anlık görüntü sayısını yönetmek gerekir. Masses veri almak veya diğer önemli değişiklikler HANA veritabanına gerçekleştirdiğinizde, depolama anlık görüntüleri devre dışı bırakabilirsiniz. 


Aşağıdaki bölümlerde genel öneriler dahil olmak üzere, bu anlık görüntüleri gerçekleştirmeye yönelik bilgileri sağlayın:

- Donanım 255 anlık görüntüler birim başına karşılayabilir rağmen bu sayının altına de Kal istiyor. 250 veya daha az önerilir.
- Depolama anlık görüntüleri gerçekleştirmeden önce izlemek ve boş alan izler.
- Boş disk alanına dayalı depolama anlık görüntü sayısını azaltın. Güvenliğinizi anlık görüntü sayısını azaltın veya birimleri genişletebilirsiniz. Ek depolama alanı 1 terabayttan küçük birimler cinsinden sıralayabilirsiniz.
- SAP HANA SAP platform Geçiş Araçları (R3load) ile veri taşıma veya SAP HANA veritabanlarını yedeklerden geri yükleme gibi etkinlikleri sırasında /hana/data birimdeki depolama anlık görüntüleri devre dışı bırakın. 
- Sırasında daha büyük düzenlemelere SAP HANA tablo depolama anlık görüntüleri, mümkünse kaçınılmalıdır.
- Depolama anlık görüntüleri, olağanüstü durum kurtarma özelliklerini SAP HANA (büyük örnekler) Azure üzerinde yararlanabilmek için bir önkoşuldur.

### <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Self Servis depolama anlık görüntüleri kullanma önkoşulları

Anlık görüntü komut dosyası başarıyla çalıştığından emin olmak için Perl HANA büyük örnekleri sunucuda Linux işletim sisteminde yüklendiğinden emin olun. Perl HANA büyük örneği biriminde önceden yüklenmiş olarak gelir. Perl sürümünü denetlemek için aşağıdaki komutu kullanın:

`perl -v`

![Ortak anahtar şu komutu çalıştırarak kopyalanır.](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


### <a name="set-up-storage-snapshots"></a>Depolama anlık görüntüler ayarlayın

Depolama anlık görüntüleri HANA büyük örneğiyle ayarlamak için aşağıdaki adımları izleyin:
1. Perl HANA büyük örnekleri sunucuda Linux işletim sisteminde yüklü olduğundan emin olun.
2. Değiştirme/etc/ssh/ssh\_satır eklemek için yapılandırma _Mac'ler hmac-sha1_.
3. Varsa çalıştırıyorsanız, her bir SAP HANA örnek ana düğüm üzerinde bir SAP HANA yedekleme kullanıcı hesabı oluşturun.
4. SAP HANA HDB istemci tüm SAP HANA büyük örnekleri sunucularına yükleyin.
5. Her bölge üzerinde ilk SAP HANA büyük örnekleri sunucu, anlık görüntü oluşturma denetimleri temel alınan depolama altyapısını erişmek için ortak bir anahtar oluşturun.
6. Yapılandırma dosyasından ve komut dosyalarını kopyalama [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) konumunu **hdbsql** SAP HANA yükleme.
7. Değiştirme *HANABackupDetails.txt* uygun müşteri belirtimleri gerektiğinde dosya.

En son anlık görüntü komut dosyaları ve belgelerini almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="consideration-for-mcod-scenarios"></a>Göz önünde bulundurarak MCOD senaryoları için
Çalıştırıyorsanız, bir [MCOD senaryo](https://launchpad.support.sap.com/#/notes/1681092) bir HANA büyük örneği biriminde birden çok SAP HANA örnekleriyle her SAP HANA örnekleri için sağlanmış ayrı bir depolama birimleri vardır. Self Servis anlık görüntü Otomasyon geçerli sürümünde her HANA örneği sistem kimliği (SID) ayrı anlık görüntü başlatamaz. İşlevselliği sunucunun (Bu makalenin sonraki bölümlerinde bakın) yapılandırma dosyasında kayıtlı SAP HANA örnekleri için denetimleri sunar ve eşzamanlı bir anlık görüntü birimi kayıtlı tüm örneklerini birimlerin yürütür.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>1. adım: SAP HANA HDB istemci yükleme

SAP HANA Azure (büyük örnekler) üzerinde yüklü Linux işletim sistemi, SAP HANA depolama anlık görüntüleri yedekleme ve olağanüstü durum kurtarma amacıyla yürütmek için gereken komut dosyaları ve klasörleri içerir. Daha yeni sürümlerde denetle [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Komut dosyalarını en son sürümünü 3.x ' dir. Farklı komut dosyaları farklı ikincil sürümleri içinde aynı ana sürümüne sahip olabilir.

>[!IMPORTANT]
>Sürüm 2.1 sürümüne taşırken 3.x komut dosyalarının yapılandırma dosyasının ve bazı söz dizimi yapısını değiştirildiğine dikkat edin. Belirli bölümlerde belirtme konusuna bakın. 

SAP HANA yüklerken HANA büyük örneği birimlerde SAP HANA HDB istemciyi yüklemek üzere sizin sorumluluğunuzdadır değil.

### <a name="step-2-change-the-etcsshsshconfig"></a>2. adım: / etc/değiştirmek ssh/ssh\_yapılandırma

Değişiklik `/etc/ssh/ssh_config` ekleyerek _Mac'ler hmac-sha1_ satır aşağıda gösterildiği gibi:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>3. adım: bir ortak anahtar oluşturma

Depolama anlık görüntü arabirimleri HANA büyük örneği kiracınızın erişimi etkinleştirmek için bir ortak anahtar ile bir oturum açma yordamı yapmanız gerekir. Kiracınızda Azure (büyük örnekler) sunucuda ilk SAP HANA üzerinde depolama altyapısını erişmek için kullanılacak ortak bir anahtar oluşturun. Ortak anahtarı bir parola depolama anlık görüntü arabirimleri oturum açmak için gerekli değil sağlar. Bir ortak anahtar oluşturma ayrıca parola kimlik bilgileri korumak gerekmez anlamına gelir. SAP HANA büyük örnekleri sunucuda Linux içinde ortak anahtarı oluşturmak için aşağıdaki komutu yürütün:
```
  ssh-keygen –t dsa –b 1024
```
Yeni konumu **_/root/.ssh/id\_dsa.pub**. Gerçek bir parola girmeyin, aksi takdirde her, oturum açtığınızda parolayı girmek için gereklidir. Bunun yerine, seçin **Enter** iki kez oturum açmak için "parola girin" gereksinimini kaldırmak için.

Ortak anahtar klasörlere değiştirerek beklendiği gibi giderilmiştir emin olun **/root/.ssh/** ve ardından yürütme `ls` komutu. Anahtar mevcut değilse, aşağıdaki komutu çalıştırarak kopyalayabilirsiniz:

![Ortak anahtar şu komutu çalıştırarak kopyalanır.](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Bu noktada, Azure Hizmet Yönetimi SAP HANA başvurun ve ortak anahtar ile verin. Hizmet temsilcisi HANA büyük örneği kiracınız için yontulmuş temel alınan depolama altyapısında kaydetmek için ortak anahtarı kullanır.

### <a name="step-4-create-an-sap-hana-user-account"></a>4. adım: bir SAP HANA kullanıcı hesabı oluşturma

SAP HANA anlık görüntü oluşturmaya başlatmak için SAP HANA içinde depolama anlık görüntü betiklerini bir kullanıcı hesabı oluşturmanız gerekir. SAP HANA Studio'dan SAP HANA kullanıcı hesabı bu amaçla oluşturun. Kullanıcı için MDC SYSTEMDB SID veritabanı altında değildir ve oluşturulmalıdır. Tek kapsayıcı ortamında kullanıcı Kiracı veritabanı altında kurulur. Bu hesabı aşağıdaki ayrıcalıklara sahip olmalıdır: **yedekleme yönetici** ve **katalog okuma**. Bu örnekte, kullanıcı adı olan **SCADMIN**. HANA Studio'da oluşturulan kullanıcı hesabı adı büyük/küçük harf duyarlıdır. Seçtiğinizden emin olun **Hayır** kendi sonraki oturum açma parolasını değiştirmek kullanıcı gerektirme.

![Bir kullanıcı HANA Studio'da oluşturma](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

Bir birim üzerinde birden çok SAP HANA örneğiyle MCOD dağıtımları kullanırsanız, her SAP HANA örneği için bu adımı yineleyin gerekir.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>5. adım: SAP HANA kullanıcı hesabı yetkilendirme

Bu adımda, komut dosyaları parolaları çalışma zamanında göndermek gerekmemesi oluşturduğunuz, SAP HANA kullanıcı hesabı yetkilendirin. SAP HANA komutu `hdbuserstore` bir veya daha fazla SAP HANA düğümde depolanan bir SAP HANA kullanıcı anahtarı oluşturulmasını sağlar. Kullanıcı anahtarı parolalardan komut dosyası işlemi içinde yönetmek zorunda kalmadan SAP HANA kullanıcı erişimi sağlar. Komut dosyası işlemi, bu makalenin sonraki bölümlerinde ele alınmıştır.

>[!IMPORTANT]
>Yürütülecek komut dosyalarını planlanan kullanıcı altında aşağıdaki komutu çalıştırın. Aksi takdirde, komut dosyası düzgün çalışamaz.

Girin `hdbuserstore` gibi komut:

**MDC HANA Kurulumu**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**MDC HANA Kurulumu**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

Aşağıdaki örnekte, kullanıcının olduğu **SCADMIN01**, konak adı **lhanad01**, ve örnek numarasını **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Birden çok SAP HANA örnekleri tek bir birim üzerinde bulunan bir HANA MCOD dağıtım kullanırsanız, her SAP HANA örneği ve ilişkili yedekleme kullanıcı birimi adımını yinelemeniz gerekir.

Bir SAP HANA genişleme yapılandırmanız varsa, tüm komut dosyası tek bir sunucudan yönetmesi gerekir. Bu örnekte, SAP HANA anahtar **SCADMIN01** her ana bilgisayarın hangi ana anahtarıyla ilgili gösterir şekilde değiştirilmesi gerekir. SAP HANA yedekleme hesabı HANA DB örneği sayısıyla düzeltmek. Bu anahtar, kendisine atanmış ve genişleme yapılandırmaları için Yedekleme kullanıcı tüm SAP HANA örnekleri için erişim haklarına sahip olmalıdır ana bilgisayardaki yönetici ayrıcalıklarına sahip olmalıdır. Üç genişleme düğümü varsayılarak adlara sahip olması **lhanad01**, **lhanad02**, ve **lhanad03**, komut dizisi şöyle görünür:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>6. adım: anlık görüntü komut dosyalarını almak, anlık görüntüleri yapılandırma ve test yapılandırması ve bağlantı

Gelen komut dosyalarını en son sürümünü indirme [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). İndirilmiş betikler ve metin dosyası için çalışma dizini kopyalayın **hdbsql**. Geçerli HANA yüklemeler için bu dizin aşağıdaki biçimdedir: /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl
azure_hana_dr_failover.pl
azure_hana_test_dr_failover.pl 
HANABackupCustomerDetails.txt 
``` 

Perl betikleri ile ilgilenirken: 

- Hiçbir zaman komut dosyalarını, Microsoft Operations tarafından belirtilmedikçe değiştirin.
- Komut dosyası veya bir parametre dosyası değiştirmek isteyip istemediğiniz sorulduğunda her zaman "VI" gibi Linux metin düzenleyicisi ve değil Not Defteri gibi bir Windows düzenleyicisi kullanın. Bir Windows Düzenleyicisi'ni kullanarak dosya biçimi bozulmasına neden olabilir.
- Her zaman en son betiklerini kullanın. Github'dan en son sürümünü indirebilirsiniz.
- Komut dosyaları aynı sürümü arasında yatay kullanın.
- Komut dosyalarını test edin ve gerekli parametreleri ve komut dosyası çıkışını üretim sisteminde kullanmadan önce doğrudan rahat alın.
- Microsoft Operations tarafından sağlanan sunucusunun bağlama noktası adı değişmez. Bu komut dosyaları için başarılı bir yürütme kullanılabilir olması için bu standart bağlama noktalarını kullanır.


Dosya ve farklı komut dosyaları amacı aşağıdaki gibidir:

- **Azure\_hana\_backup.pl**: Bu komut dosyası Linux Cron zamanlama yardımcı programı ile depolama anlık görüntüleri HANA veri ve paylaşılan birimler, / hana/logbackups birim ya da işletim sistemi üzerinde yürütülecek şekilde zamanlanır.
- **Azure\_hana\_çoğaltma\_status.pl**: Bu komut dosyası üretim site çoğaltma durumu olağanüstü durum kurtarma sitesine geçici temel ayrıntıları sağlar. Çoğaltma yer aldığı ve öğelerin boyutu, gösterir emin olmak için komut dosyası izleyicileri çoğaltılmış olduğunu. Ayrıca Kılavuzu bir çoğaltma çok uzun sürüyorsa veya bağlantı kapalı olduğunda sağlar.
- **Azure\_hana\_anlık görüntü\_details.pl**: Bu komut, ortamınızda bulunan tüm anlık görüntüleri, birim başına hakkında temel ayrıntılar listesini sağlar. Bu komut, birincil sunucuda veya sunucu birim olağanüstü durum kurtarma konumu olarak çalıştırılabilir. Komut dosyası anlık görüntüleri içeren her bir birim ayrıntılarıyla aşağıdaki bilgileri sağlar:
   * Bir birimdeki toplam anlık görüntü boyutu
   * Her anlık görüntü o birimdeki aşağıdaki Ayrıntılar: 
      - Anlık görüntü adı 
      - Oluşturma zamanı 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme ilgiliyse bu anlık görüntü ile ilişkili kimliği
- **Azure\_hana\_anlık görüntü\_delete.pl**: Bu komut dosyasını bir depolama anlık görüntü veya anlık görüntü kümesi siler. SAP HANA yedekleme kimliği HANA Studio'da bulunan olarak ya da depolama anlık görüntü adı kullanabilirsiniz. Şu anda, yedekleme kimliği yalnızca HANA veri/günlük/paylaşılan birimler için oluşturulan anlık görüntülerin bağlıdır. Aksi takdirde, girilen anlık görüntü kimliği eşleşen tüm anlık görüntüleri aradığı anlık görüntü kimliği girilirse,  
- **testHANAConnection.pl**: Bu komut dosyası SAP HANA örneği bağlantısını test eder ve depolama anlık görüntüler ayarlamak için gereklidir.
- **testStorageSnapshotConnection.pl**: Bu komut dosyası iki amaca sahiptir. İlk olarak, betikleri çalıştırır HANA büyük örneği birim atanan depolama sanal makine ve depolama anlık görüntü arabirimi, HANA büyük örneklerinin erişim sahip olmasını sağlar. İkinci amacı test ettiğiniz HANA örneği için geçici bir anlık görüntü oluşturmaktır. Bu komut dosyası her HANA örneği için yedekleme betikleri beklendiği gibi çalışmasını sağlamak için bir sunucu üzerinde çalıştırmanız gerekir.
- **removeTestStorageSnapshot.pl**: Bu komut dosyası komut dosyası ile oluşturulan test anlık görüntüsü siler **testStorageSnapshotConnection.pl**.
- **Azure\_hana\_dr\_failover.pl**: Bu komut dosyası başka bir bölgeye DR yük devretme başlatır. Yük istediğiniz birime veya komut dosyası DR bölgede HANA büyük örneği biriminde yürütülmesi gerekiyor. Bu komut dosyasını ikincil tarafa birincil tarafındaki depolama çoğaltma durdurur, DR birimlerde en son anlık görüntü geri yükler ve bağlama için DR birimlerini sağlar.
- **Azure\_hana\_test\_dr\_failover.pl**: Bu komut dosyası, DR siteye bir yük devretme testi gerçekleştirir. Azure_hana_dr_failover.pl betik bu yürütme depolama çoğaltmayı birincil ikincil kesmez. Bunun yerine, çoğaltılan depolama birimlerinin klonlar DR tarafında oluşturulur ve kopyalanan birimleri bağlama sağlanır. 
- **HANABackupCustomerDetails.txt**: Bu dosya, SAP HANA yapılandırmanızı uyum değişiklik yapmanız değiştirilebilir yapılandırma dosyadır. *HANABackupCustomerDetails.txt* depolama anlık görüntüleri çalışan komut için Denetim ve yapılandırma dosyası bir dosyadır. Dosya amaçlıdır ve kurulum için ayarlayın. Aldığınız **depolama yedekleme adı** ve **depolama IP adresi** SAP HANA örneklerinizi dağıtırken Azure Hizmet Yönetimi'nden. Sıralama ya da bu dosyasındaki değişkenleri hiçbirini aralığı dizisi değiştiremezsiniz. Bunu yaparsanız, komut dosyaları düzgün çalışmaz. Ayrıca, büyütme düğüm veya ana düğümünün IP adresi (genişleme varsa) SAP HANA Azure Hizmet Yönetimi alırsınız. SAP HANA yükleme sırasında edinmeye HANA örnek numarasını da bildiğiniz. Şimdi, yapılandırma dosyasını bir yedek adı eklemeniz gerekir.

HANA büyük örneği birim ve sunucu IP adresi sunucu adını doldurduktan sonra büyütme veya genişleme dağıtımı için yapılandırma dosyasını aşağıdaki gibi görünür. Her SAP HANA yedeklemek veya kurtarmak istediğiniz SID için tüm gerekli alanları doldurun.

Bir süre için gerekli bir alan önünde "#" ekleyerek yedeklenecek istemediğiniz örneklerinin satırları çıkışı açıklama getirebilirsiniz. SAP HANA yedeklemek veya belirli bu örneğe kurtarmak için gerekli ise, bir sunucuda bulunan hepsinin girmeniz gerekmez. Tüm alanlar için biçimi tutulması gerekir veya tüm betikler bir hata iletisi throw ve komut dosyası sona erer. Son SAP HANA örneğinin sonra kullanmadığınızı herhangi SID bilgileri ayrıntıları gerekli ek satırları silebilirsiniz. Tüm satırları gerekir doldurulmuş, derleme dışı bırakıldı veya silinmiş.

>[!IMPORTANT]
>Dosyanın yapısı değiştirilmiş sürüm 2.1 sürümünden geçiş ile 3.x. 3.x sürüm kodları kullanmak istiyorsanız, yapılandırma dosyası yapısı uyarlamanız gerekir. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

HANA büyük örneği biriminde yapılandırdığınız her örneği için ya da yapılandırmayı genişletmek için verileri gibi tanımlamanız gerekir:

    
```
######***SID #1 Information***#####
SID1: h01
###Provided by Microsoft Operations###
SID1 Storage Backup Name: clt1h01backup
SID1 Storage IP Address: 172.18.18.11
######     Customer Provided    ######
SID1 HANA instance number: 00
SID1 HANA HDBuserstore Name: SCADMINH01
```
Genişleme ve HANA sistem çoğaltma yapılandırmalar için bu yapılandırma her düğümlerin yineleyin. Bu ölçü, hata durumlarda çalışmaya yedeklemeleri ve nihai depolama çoğaltma devam emin olur.   

Tüm yapılandırma verilerini içine yerleştirin sonra *HANABackupCustomerDetails.txt* dosya, yapılandırmaları HANA örnek verileri doğru olup olmadığını denetleyin. Komut dosyası kullanma `testHANAConnection.pl`, bir SAP HANA büyütme veya genişleme yapılandırmasını bağımsız olduğu.

```
testHANAConnection.pl
```

Bir SAP HANA genişleme yapılandırmanız varsa, ana HANA örneği gerekli HANA sunucuları ve örneklerine erişimi olduğundan emin olun. Test komut dosyasına bir parametre yok ancak verilerinizi eklemelisiniz *HANABackupCustomerDetails.txt* komut dosyasının düzgün çalışması yapılandırma dosyası. Yalnızca kabuk komutu hata kodları döndürülür, bu komut dosyası hata denetlemek için her örnek onun mümkün değildir. Betik bazı yararlı açıklamaları, iki kez kontrol etmek için buna rağmen sağlar.

Komut dosyasını çalıştırmak için aşağıdaki komutu girin:
```
 ./testHANAConnection.pl
```
Komut dosyasını başarıyla HANA örneğinin durumunu elde ederse HANA bağlantısı başarılı bir ileti görüntüler.


Put içine verileri temel alan depolama bağlantısı denetlemek için sonraki adım olan *HANABackupCustomerDetails.txt* yapılandırma dosyasını ve test anlık görüntü yürütün. Yürütmeden önce `azure_hana_backup.pl` komut dosyası, bu test çalıştırması gerekir. Bir birim anlık görüntü yok içeriyorsa, boş bir birimdir olup olmadığını veya anlık görüntü ayrıntıları almak için bir SSH hatası ise belirlemek mümkün değildir. Bu nedenle, iki adım komut dosyasını çalıştırır:

- Kiracının sanal makine depolama ve arabirimleri anlık görüntüleri yürütülecek komut dosyaları için erişilebilir olduğunu doğrular.
- HANA örneği tarafından her birim için bir test veya kukla, anlık görüntü oluşturur.

Bu nedenle, HANA örneği bağımsız değişken olarak dahil edilir. Yürütme başarısız olursa, depolama bağlantısı denetlenirken hata sağlamak mümkün değil. Olsa bile hata denetimi, betik yararlı ipuçları sağlar.

1. Bu test gerçekleştirmek için komut dizisi üzerinde yürütün:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   Depolama kullanıcı adı ve depolama IP adresi için HANA büyük örneği birim handover belirtildi.

2. Test betiğini çalıştırın:
   ```
    ./testStorageSnapshotConnection.pl
   ```

Depolama birimine yapılandırılmış veri ile önceki kurulum adımlarını sağlanan ortak anahtar kullanarak oturum açmak komut dosyası çalıştığında *HANABackupCustomerDetails.txt* dosya. Oturum açma başarılı olursa, aşağıdaki içeriği gösterilir:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Depolama konsola bağlanırken sorunlar meydana gelirse, çıktı şu şekildedir:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Bir başarılı oturum açma depolama sanal makine arabirimlerine sonra komut dosyasını Aşama 2 ile devam eder ve bir test anlık görüntüsü oluşturur. Çıkış, SAP HANA üç düğümlü genişleme yapılandırılmasını için burada gösterilir:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Test anlık görüntü ile komut dosyası başarıyla çalıştırıldı, gerçek depolama anlık görüntüleri yapılandırma ile devam edebilirsiniz. Başarılı olmazsa, ileriye doğru taşımadan önce sorunları araştırın. İlk gerçek anlık görüntüleri tümü tamamlanıncaya kadar test anlık görüntü geçici kalmalı.


### <a name="step-7-perform-snapshots"></a>7. adım: anlık görüntüler gerçekleştirin

Hazırlık adımları tamamladığınızda, gerçek depolama anlık görüntü yapılandırması yapılandırmaya başlayabilirsiniz. SAP HANA ölçek büyütme ve ölçek genişletme yapılandırmalarla zamanlanması için komut dosyası çalışır. Dönemsel ve normal yedekleme betik yürütme işlemi için komut dosyası cron yardımcı programını kullanarak zamanlayın. 

Anlık görüntü yedeklerini üç tür oluşturabilirsiniz:
- **HANA**: içinde/hana/verileri içeren birimlerin ve/hana paylaşılan (/usr/sap de içeren) / ele alınmıştır koordineli bir anlık görüntü tarafından birleşik anlık görüntü yedekleme. Tek dosya geri yükleme, bu anlık görüntüden mümkündür.
- **Günlükleri**: / hana/logbackups birim anlık görüntü yedeğini. Bu depolama anlık görüntü yürütmek için hiç HANA anlık görüntü tetiklenir. Bu depolama birimi, SAP HANA işlem günlüğü yedeklemeleri içerecek şekilde tasarlanmıştır. Bunlar daha sık Günlük büyüme kısıtlamak ve olası veri kaybını önlemek için gerçekleştirilir. Tek dosya geri yükleme, bu anlık görüntüden mümkündür. Altında 3 dakika sıklığını azaltın yok.
- **Önyükleme**: bir birimin anlık görüntüsünü HANA büyük örneğinin önyükleme mantıksal birim numarası (LUN) içeren. Bu anlık görüntü yedekleme yalnızca Type ı SKU'ları, HANA büyük örnekleriyle mümkündür. LUN önyükleme içeren birim anlık görüntüden geri yüklemeler tek dosyalı gerçekleştiremezsiniz.


>[!NOTE]
> Bu üç tür hareket MCOD dağıtımlarını desteklemek sürüm 3.x kodları değiştirilen anlık görüntüler için çağrı sözdizimi. Bir örneği HANA SID'si artık belirtmek için gerek yoktur. Bir birim SAP HANA örneklerini yapılandırma dosyasında yapılandırılmış olduğundan emin olmanız gerekir *HANABackupCustomerDetails.txt*.

>[!NOTE]
> Komut dosyası ilk kez çalıştırdığınızda, çoklu SID ortamda bazı beklenmeyen hatalar gösterebilir. Komut dosyası yeniden çalıştırma sorunu giderir.



Depolama anlık görüntüleri komut dosyası yürütme yeni çağrı sözdizimi *azure_hana_backup.pl* şöyle görünür:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

Parametrelerin ayrıntılarını aşağıdaki gibidir: 

- İlk parametre anlık görüntü yedekleme türünü belirtir. İzin verilen değerler: **hana**, **günlükleri**, ve **önyükleme**. 
- Parametre **<HANA Large Instance Type>** yalnızca önyükleme birimi yedeklemeler için gereklidir. "TypeI" veya "TypeII" iki geçerli değerlerle HANA büyük örneği biriminde bağımlı. Biriminiz ne tür çıkışı bulmak için bkz. [SAP HANA (büyük örnekler) genel bakış ve Azure ile ilgili mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- Parametre **< snapshot_prefix >** bir anlık görüntü veya anlık görüntü türü için yedekleme etiketi. İki amacı vardır: biridir sizin için bir ad vermek bu anlık görüntüler hakkında nelerdir bilmesi. Komut dosyası için ikinci amaçtır *azure\_hana\_backup.pl* bu özel etiketin altında korunur depolama anlık görüntü sayısını belirlemek için. Aynı türde iki depolama anlık görüntü yedekleme zamanlarsanız (gibi **hana**), iki farklı etiketleri ve 30 anlık görüntüleri saklanması her biri için 60 depolama birimlerin anlık görüntüleri etkilenen ile sona tanımlayın. Yalnızca alfa sayısal ("A-Z, a-z, 0-9"), alt çizgi ("_") ve tire ("-") karakterlere izin verilir. 
- Parametre **< snapshot_frequency >** için gelecek gelişmeler ayrılmıştır ve herhangi bir etkisi yoktur. "3 dakika" türü yedeklerini yürütülürken ayarlamak **günlük**ve "bir yedekleme türleri yürütülürken 15 dakika".
- Parametre **<number of snapshots retained>** aynı anlık görüntü önek (etiketi) ile anlık görüntü sayısını tanımlayarak anlık görüntü saklama dolaylı olarak tanımlar. Bu parametre, cron aracılığıyla zamanlanmış yürütmeleri için önemlidir. Aynı snapshot_prefix ile anlık görüntü sayısı bu parametresi tarafından verilen sayıyı aşarsa, yeni bir depolama anlık görüntü yürütmeden önce eski anlık görüntü silinir.

Bir genişleme söz konusu olduğunda, betik tüm HANA sunucuların erişebildiğinden emin olmak için ek denetimi yapar. Komut dosyası, ayrıca bir SAP HANA anlık görüntüsünü oluşturmadan önce dönüş örneklerinin uygun durumu HANA hepsinin denetler. SAP HANA anlık görüntü depolama anlık görüntü tarafından izlenir.

Betik yürütme işlemi `azure_hana_backup.pl` aşağıdaki üç aşamaya anlık görüntü depolama oluşturur:

1. SAP HANA anlık görüntü yürütür
2. Bir depolama anlık görüntü yürütür
3. Depolama anlık görüntü çalışmaya başlamadan önce oluşturulan SAP HANA anlık görüntü kaldırır

Betik yürütmek için kopyalanmıştır HDB yürütülebilir klasöründen çağırın. 

Saklama dönemi komut yürüttüğünüzde, parametre olarak gönderilen anlık görüntü sayısı ile yönetilir. Depolama anlık görüntüleri tarafından kapsanan süreyi, yürütme ve komut dosyası yürütülürken bir parametre olarak gönderilen anlık görüntü sayısını döneminin bir işlevdir. Tutulur anlık görüntü sayısını betik çağrısı bir parametresi olarak adlı sayıyı aşarsa, yeni bir anlık görüntü yürütülmeden önce eski depolama anlık görüntü aynı etiketinin silinir. Çağrının son parametre sayısı tutulur anlık görüntü sayısını denetlemek için kullanabileceğiniz olarak sayı, verin. Bu numara ile aynı zamanda, dolaylı olarak, anlık görüntüleri için kullanılan disk alanı denetleyebilirsiniz. 

> [!NOTE]
>Etiketi değiştirmek hemen sayım yeniden başlatır. Anlık görüntülerin yanlışlıkla silinmez şekilde etiketleme, katı olması gerekir.

### <a name="snapshot-strategies"></a>Anlık görüntü stratejileri
Farklı türleri için anlık görüntü sıklığı, HANA büyük örneği olağanüstü durum kurtarma işlevini kullanmasına bağlı olarak değişir. Bu işlev özel öneriler depolama anlık görüntü sıklığı ve yürütme dönemleri gerektirebilir depolama anlık görüntüleri kullanır. 

Önemli noktalar ve izleyin önerileri bunu yapmanızı varsayılır *değil* HANA büyük örnekleri sunar olağanüstü durum kurtarma işlevini kullanın. Bunun yerine, depolama anlık görüntüleri yedeklemeler ve son 30 gün için zaman içinde nokta kurtarma sağlamak için kullanın. Anlık görüntüler ve alan sayısı sınırlamaları verildiğinde, müşterilerin aşağıdaki gereksinimleri göz önünde bulundurduğunuzdan:

- Zaman içinde nokta kurtarma için kurtarma süresi.
- Kullanılan alan.
- Kurtarma noktası ve kurtarma zamanı hedeflerine olası bir olağanüstü durum kurtarma için.
- Diskleri karşı HANA tam veritabanı yedeklemeleri Son Yürütme. Tam veritabanı yedeklemesi her diskleri karşı veya **backint** arabirimi gerçekleştirilir, depolama anlık görüntüleri yürütülmesi başarısız olur. Tam veritabanı yedeklemeleri depolama anlık görüntüleri üstünde yürütme planlıyorsanız, depolama anlık görüntüleri yürütülmesi bu süre boyunca devre dışı bırakıldığından emin olun.
- Anlık görüntüler (250 için sınırlı) birim başına sayısı.


Olağanüstü Durum Kurtarma işlevi HANA büyük örneklerinin kullanmayan müşteriler için anlık görüntü dönemi daha az sıklıkta belirtir. Böyle durumlarda, müşteriler birleşik anlık görüntüleri /hana/data ve (/usr/sap içerir) /hana/shared 12 veya 24 saat dönemlerini gerçekleştirmek ve bunlar bir ay için anlık görüntüleri tutmak. Aynı günlük yedekleme birimi anlık görüntüleri ile geçerlidir. Ancak, SAP HANA işlem günlüğü yedeklemeleri günlük yedekleme birimi karşı yürütülmesi 15 dakikalık dönem için 5 dakika içinde gerçekleşir.

Zamanlanmış depolama anlık görüntüleri en iyi cron kullanılarak gerçekleştirilir. Tüm yedekleme ve olağanüstü durum kurtarma gereksinimleri için aynı komut dosyasını kullanın ve yedekleme kez istenen komut dosyası girişleri çeşitli eşleşecek şekilde değiştirin. Bu anlık görüntüleri tüm farklı cron kendi yürütme süresi bağlı olarak, zamanlanan: saat, 12 saatlik, günlük veya haftalık. 

/ Etc/crontab cron zamanlamada örneği verilmiştir:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
Önceki örnekte, yok/hana/verileri içeren birimlerin ve (/ usr/sap içerir) /hana/shared kapsayan bir saatlik birleşik anlık görüntü konumları. Bu tür bir anlık görüntü için daha hızlı bir zaman içinde nokta kurtarma son iki gün içinde kullanın. Ayrıca, bu birimlerde günlük bir anlık görüntü yok. Bu nedenle, kapsamı iki gün saatlik anlık görüntüleri artı kapsam dört hafta tarafından günlük anlık görüntüleri var. Ayrıca, işlem günlüğü yedekleme toplu günlük yedek desteklenir. Bu yedeklemeler de dört hafta boyunca tutulur. Crontab üçüncü satırda gördüğünüz gibi HANA işlem günlüğü yedeklemesi her 5 dakikada bir yürütülecek şekilde zamanlanır. Depolama anlık görüntüleri yürütme farklı cron işlerin başlangıç saatleri dağılır, böylece bu anlık görüntülerin belirli bir noktada aynı anda zamanında yürütülmez. 

Aşağıdaki örnekte, / hana/veri ve/hana paylaşılan / (dahil olmak üzere/usr/sap) konumları saatlik düzenli olarak içeren birimlere kapsayan birleşik bir anlık görüntü gerçekleştirin. Bu anlık görüntüler için iki gün tutun. İşlem günlük yedekleme birimlerin anlık görüntüleri 5 dakikalık temelinde yürütülür ve 4 saat boyunca tutulur. Olarak daha önce HANA işlem günlük dosyasının yedeğini her 5 dakikada bir yürütülecek şekilde zamanlanır. İşlem günlüğü yedeklemesi başlatıldıktan sonra işlem günlüğü yedekleme biriminin anlık görüntü ile 2 dakikalık bir gecikmeyle gerçekleştirilir. Bu 2 dakika içinde SAP HANA işlem günlüğü yedeklemesi normal koşullarda tamamlanmalıdır. Olarak önce önyükleme içeren birimi LUN günde bir kez yedekleme depolama anlık görüntü tarafından yedeklenir ve dört hafta boyunca tutulur.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

Aşağıdaki grafikte LUN önyükleme hariç önceki örnekte, dizileri gösterilmektedir:

![Yedeklemeler ve anlık görüntüleri arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA veritabanına yapılan değişiklikleri belgelemek için /hana/log birim karşı normal yazma işlemleri gerçekleştirir. Düzenli aralıklarla, SAP HANA /hana/data birime bir kayıt noktası yazar. Belirtilen olarak crontab, SAP HANA işlem günlüğü yedeklemesi her 5 dakikada bir çalıştırılır. SAP HANA anlık görüntü /hana/data ve /hana/shared birimlerinin birleşik depolama anlık görüntü tetikleme sonucunda her saat yürütülür de görebilirsiniz. Birleştirilmiş depolama anlık görüntü HANA anlık görüntü başarılı olduktan sonra yürütülür. Crontab belirtildiği gibi depolama anlık görüntü /hana/logbackup birimde her 5 dakikada bir, yaklaşık 2 dakika HANA işlem günlüğü yedeklemesi sonra yürütülür.

> 

>[!IMPORTANT]
> SAP HANA yedeklemeler için depolama anlık görüntüleri kullanımını, yalnızca anlık görüntüleri SAP HANA işlem günlüğü yedeklemeleri ile birlikte gerçekleştirildiğinde faydalıdır. Bu işlem günlüğü yedeklemeleri dönemleri depolama anlık görüntüleri arasında kapak gerekir. 

Kullanıcılara taahhüdü 30 günlük bir zaman içinde nokta kurtarma oluşturmadıysanız, için gerekir:

- Olağanüstü durumlarda /hana/data ve 30 gün önce yapılmışsa /hana/shared üzerinde anlık görüntü birleşik bir depolama alanına erişebilir.
- Herhangi bir birleşik depolama anlık görüntüyü arasındaki süreyi kapsar bitişik işlem günlüğü yedeklemeleri vardır. Bu nedenle, en eski birimin anlık görüntüsünü işlem günlük yedekleme 30 günden daha eski olması gerekir. Azure depolama alanında bulunan başka bir NFS paylaşımına işlem günlüğü yedeklemeleri kopyalarsanız, bu durumda değil. Bu durumda, eski işlem günlüğü yedeklemeleri, NFS paylaşımından çekme.

Depolama anlık görüntüler ve işlem günlüğü yedeklemeleri nihai depolama çoğaltması yararlanmak için işlem günlüğü yedeklemeleri SAP HANA yazacağı konumunu değiştirmeniz gerekir. Bu değişiklik HANA Studio'da yapabilirsiniz. SAP HANA tam günlük kesimleri otomatik olarak yedekler de, bir günlük yedekleme aralığını belirleyici olacak şekilde belirtmeniz gerekir. Olağanüstü durum kurtarma seçeneğini kullandığınızda, genellikle günlük yedeklerini belirleyici bir süresi olan yürütmek istediğiniz çünkü bu özellikle doğrudur. Aşağıdaki durumlarda, 15 dakika günlük yedekleme aralığı ayarlanır.

![SAP HANA yedekleme zamanlaması SAP HANA Studio'da günlüğe kaydeder](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Ayrıca, her 15 dakikadan daha sık yedeklemeler de seçebilirsiniz. Daha sık ayar genellikle olağanüstü durum kurtarma işlevselliğini HANA büyük örnekleri ile birlikte kullanılır. Bazı müşteriler, her 5 dakikada bir işlem günlüğü yedeklemeleri gerçekleştirin.  

Veritabanı hiçbir zaman yedeklendiğinden, son yedekleme kataloğu içinde bulunması gereken tek bir yedekleme girişi oluşturmak için bir dosya tabanlı veritabanı yedeklemesi gerçekleştirmeyi adımdır. Aksi takdirde, SAP HANA belirtilen günlük Yedeklemelerinizin başlatamaz.

![Tek bir yedekleme girişi oluşturmak için dosya tabanlı bir yedek alın](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


İlk başarılı depolama anlık görüntüleri yürütülen sonra 6. adımda yürütüldüğü test anlık görüntü silebilirsiniz. Bunu yapmak için komut dosyasını çalıştırmak `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl
```

Komut dosyası çıktısı örneği verilmiştir:
```
Checking Snapshot Status for h80
**********************Checking access to Storage**********************
Storage Snapshot Access successful.
**********************Getting list of volumes that match HANA instance specified**********************
Collecting set of volumes hosting HANA matching pattern *h80* ...
Volume show completed successfully.
Adding volume hana_data_h80_mnt00001_t020_vol to the snapshot list.
Adding volume hana_log_backups_h80_t020_vol to the snapshot list.
Adding volume hana_shared_h80_t020_vol to the snapshot list.
**********************Adding list of snapshots to volume list**********************
Collecting set of snapshots for each volume hosting HANA matching pattern *h80* ...
**********************Displaying Snapshots by Volume**********************
hana_data_h80_mnt00001_t020_vol
Test_HANA_Snapshot.2018-02-06_1753.3
Test_HANA_Snapshot.2018-02-06_1815.2
….
Command completed successfully.
Exiting with return code: 0
Command completed successfully.
```


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Sayısını ve boyutunu disk birimi anlık görüntü izleme

Belirli bir depolama biriminde sayısı anlık görüntüler ve bu anlık görüntülerin depolama kullanımını izleyebilirsiniz. `ls` Değil Göster komutu anlık görüntü dizini ya da dosyaları. Ancak, Linux işletim sistemi komutu `du` aynı birimlerde depolandığından, bu depolama anlık görüntülerin ayrıntılarını gösterir. Komut aşağıdaki seçenekler kullanılabilir:

- `du –sh .snapshot`: Bu seçenek, anlık görüntü dizini içindeki tüm anlık görüntüleri toplam sağlar.
- `du –sh --max-depth=1`: Bu seçenek kaydedilir tüm anlık görüntüleri listeler **.snapshot** klasörü ve her anlık görüntü boyutu.
- `du –hc`: Bu seçenek tarafından tüm anlık görüntüleri kullanılan toplam boyutu sağlar.

Birimler üzerinde tüm depolama alınan ve depolanan anlık görüntüleri kullanma değil emin olmak için bu komutları kullanın.

>[!NOTE]
>LUN önyükleme anlık görüntüleri önceki komutlarla görünür değildir.

### <a name="getting-details-of-snapshots"></a>Anlık görüntü ayrıntıları alınıyor
Anlık görüntü daha fazla bilgi edinmek için de komut dosyasını kullanabilirsiniz `azure_hana_snapshot_details.pl`. Olağanüstü durum kurtarma konumunda etkin sunucunun ise bu komut dosyası iki konumdan birinde çalıştırabilirsiniz. Komut dosyası anlık görüntüleri içeren her bir birim ayrıntılarıyla aşağıdaki çıkış sağlar: 
   * Bir birimdeki toplam anlık görüntü boyutu
   * Her anlık görüntü o birimdeki aşağıdaki Ayrıntılar: 
      - Anlık görüntü adı 
      - Oluşturma zamanı 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme ilgiliyse bu anlık görüntü ile ilişkili kimliği

Komut dosyası yürütme sözdizimi örneği verilmiştir:

```
./azure_hana_snapshot_details.pl 
```

Komut dosyası HANA yedekleme Kimliği almak çalışacağından, SAP HANA örneğine bağlanması gerekir. Yapılandırma dosyası bu gerektiriyor *HANABackupCustomerDetails.txt* doğru şekilde ayarlanacak. İki anlık görüntü üzerinde bir birim bir çıktı aşağıdakine benzeyebilir:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-a-storage-snapshot"></a>Dosya düzeyinde depolama anlık görüntü geri yükleme
Anlık Görüntü türleri için **hana** ve **günlükleri**, anlık görüntü doğrudan birimlerin erişebilirsiniz **.snapshot** dizini. Her anlık görüntü için bir alt yoktur. Anlık görüntüden o alt gerçek dizin yapıda noktasında olduğu durumda, her dosya kopyalayabilirsiniz. Komut dosyası geçerli sürümünde yoktur **Hayır** (Self Servis DR parçası DR sitede yük devretme sırasında komut dosyaları gibi anlık görüntü geri yükleme gerçekleştirilebilir rağmen) anlık görüntü geri yükleme için bir Self Servis sağlanan komut dosyasını geri yükleyin. Var olan kullanılabilir anlık görüntülerden istenen anlık görüntü geri yüklemek için bir hizmet isteği açarak Microsoft işletim ekibi başvurmanız gerekir.

>[!NOTE]
>Tek dosya geri yükleme LUN HANA büyük örneği birimlerinin türünü bağımsız önyükleme anlık görüntüler için çalışmaz. **.Snapshot** directory önyükleme LUN gösterilmez. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Bir sunucu üzerinde anlık görüntü sayısını azaltır

Daha önce açıklandığı gibi belirli etiketleri depoladığınız anlık görüntü sayısını azaltabilirsiniz. Son iki bir anlık görüntü başlatmak için komut etiketi ve korumak istediğiniz anlık görüntü sayısını parametreleridir.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

Önceki örnekte, anlık görüntü etiketidir **dailyhana** ve korunacak bu etiketle anlık görüntü sayısını **28**. Disk alanı tüketimini yanıt olarak depolanan anlık görüntü sayısını azaltmak isteyebilirsiniz. Son parametre kümesi komut dosyasını çalıştırmak için 15'e, örneğin, anlık görüntü sayısını azaltmak için kolay yoludur **15**:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Bu ayar betik çalıştırırsanız, yeni depolama anlık görüntüsü de dahil olmak üzere anlık görüntü sayısını 15'tir. 15 en son anlık görüntüleri korunur ve 15 eski anlık görüntü silinir.

 >[!NOTE]
 > Birden fazla 1 saat öncesine anlık görüntüler varsa bu komut dosyası anlık görüntü sayısını azaltır. Komut dosyası değerinden 1 saat öncesine anlık görüntüleri silmez. Bu kısıtlamalar sunulan isteğe bağlı olağanüstü durum kurtarma işlevselliği ilişkilidir.

Yedek etiketli anlık görüntü kümesi korumak istiyorsanız, **hanadaily** sözdizimi örneklerde, komut dosyasıyla yürütebilir **0** bekletme sayı olarak. Etiket eşleşen tüm anlık görüntüleri sonra kaldırılır. Ancak, tüm anlık görüntüleri kaldırma HANA büyük örnekleri olağanüstü durum kurtarma işlevini yeteneklerini etkileyebilir.

Betik kullanmak için belirli anlık görüntüleri silmek için ikinci bir seçenek olan `azure_hana_snapshot_delete.pl`. Bu komut dosyası, bir anlık görüntü veya HANA Studio'da ya da anlık görüntü adı aracılığıyla bulunan olarak HANA yedekleme kimliği kullanarak ya da anlık görüntü kümesini silmek için tasarlanmıştır. Şu anda, yedekleme kimliği yalnızca için oluşturulan anlık görüntülerin bağlıdır **hana** anlık görüntü türünde. Yedekleme türü anlık görüntü **günlükleri** ve **önyükleme** SAP HANA anlık görüntü gerçekleştirmeyin ve bu nedenle bu anlık görüntüler için bulunacak yedekleme hiç kimlik yok. Anlık görüntü adı girilirse, tüm anlık görüntüleri girilen anlık görüntü adı ile eşleşmesi farklı birimlerde arar. 

Komut dosyasının çağrı sözdizimini kullanarak HANA örneği SID'si belirtmek zorunda komut dosyasını çağırın:

```
./azure_hana_snapshot_delete.pl <SID>

```

Kullanıcı olarak komut dosyası yürütme **kök**.

Bir anlık görüntü seçerseniz, her anlık görüntü tek tek silebilirsiniz. Anlık görüntü içeren birimi ilk sağlayın ve sonra anlık görüntü adı sağlayın. Anlık görüntü o birim varsa ve birden fazla 1 saat eski silinir. Birim adları ve anlık görüntü adları yürüterek bulabileceğiniz `azure_hana_snapshot_details` komut dosyası. 

>[!IMPORTANT]
>Yalnızca anlık görüntü üzerinde var olan verilerin ise silmekte, anlık görüntüsü silindikten sonra veri sonsuza dek kaybolur.

   

### <a name="recover-to-the-most-recent-hana-snapshot"></a>En son HANA anlık görüntüye Kurtar

Bir üretim aşağı senaryosunda, Microsoft Azure desteğiyle müşterilerde olarak depolama anlık görüntüden kurtarma işlemi başlatılabilir. Veriler bir üretim sisteminde silinir ve üretim veritabanını geri yüklemek için bunu almak için tek yolu ise yüksek aciliyet sağlasa da konusudur.

Farklı bir durumda bir zaman içinde nokta kurtarma düşük aciliyet olabilir ve önceden gün planlı. Yüksek öncelikli bayrağı oluşturma yerine, bu kurtarma SAP HANA ile Azure Hizmet Yönetimi planlayabilirsiniz. Örneğin, yeni bir geliştirme paketi uygulayarak SAP yazılım yükseltmeyi planlama. Ardından geliştirme paket yükseltmeden önce durumunu temsil eden bir anlık görüntü geri dönmek gerekir.

İstek göndermeden önce hazırlamanız gerekir. SAP HANA Azure Hizmet Yönetimi ekipteki isteği işlemek ve geri yüklenen birimleri sağlayın. Daha sonra anlık görüntü tabanlı HANA veritabanını geri yükleyin. 

Aşağıdaki isteği hazırlamak nasıl gösterir:

>[!NOTE]
>Kullanıcı arabirimi, kullanmakta olduğunuz SAP HANA yayın bağlı olarak aşağıdaki ekran görüntüleri kadar değişiklik gösterebilir.

1. Anlık görüntüyü geri yüklemek için karar verin. Aksi takdirde toplamasını sürece yalnızca hana/veri birimi geri yüklendi. 

2. HANA örneği kapatın.

 ![HANA örneği Kapat](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Veri birimleri, her HANA veritabanına düğümde çıkarın. Veri birimleri işletim sistemine bağlı olduğundan, anlık görüntü geri yükleme başarısız olur.
 ![Veri birimleri, her HANA veritabanına düğümde çıkarın](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Bir Azure destek isteği açın ve belirli bir anlık görüntüye geri yükleme hakkında yönergeler içerir.

 - Geri yükleme sırasında: Azure Hizmet Yönetimi SAP HANA düzenleme, doğrulama ve doğru depolama anlık görüntü geri yüklendiğinde onay emin olmak için bir konferans katılmayı isteyebilir. 

 - Geri yükleme sonrasında: Azure Hizmet Yönetimi SAP HANA bildirir, size, depolama anlık görüntü geri olduğunda.

5. Geri yükleme işlemi tamamlandıktan sonra tüm veri birimleri yeniden bağlayın.

 ![Tüm veri birimleri yeniden bağlayın](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. SAP HANA Studio aracılığıyla HANA veritabanına bağlandığınızda bunlar otomatik olarak değil oluşturduysanız SAP HANA Studio içinde kurtarma seçeneklerini seçin. Aşağıdaki örnek, son HANA anlık görüntüye geri yüklemeyi gösterir. Bir depolama anlık bir HANA anlık görüntü katıştırır. En son depolama anlık görüntüye geri yüklerseniz, en son HANA anlık görüntü olmalıdır. (Daha eski bir depolama anlık görüntüye geri yüklerseniz, depolama anlık görüntünün alındığı zamana dayalı HANA anlık görüntü bulmanız gerekir.)

 ![SAP HANA Studio içinde kurtarma seçeneklerini seçin](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Seçin **belirli veri yedekleme veya depolama anlık veritabanını kurtarma**.

 ![Kurtarma türünü belirtin penceresi](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Seçin **belirt yedekleme kataloğu olmadan**.

 ![Yedekleme konumu belirtin penceresi](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. İçinde **hedef türü** listesinde **anlık görüntü**.

 ![Yedekleme kurtarma penceresine belirtin](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Seçin **son** kurtarma işlemini başlatmak üzere.

 ![Kurtarma işlemini başlatmak için "bitir" seçin](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. HANA veritabanına geri ve depolama anlık görüntüsüne dahil HANA anlık kurtarıldı.

 ![HANA veritabanına geri ve HANA anlık görüntüye kurtarıldı](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>En son durumunu kurtarma

Aşağıdaki işlem, depolama anlık görüntüsüne dahil HANA anlık görüntü geri yükler. Bunun ardından işlem günlüğü yedeklemeleri veritabanını en son durumuna depolama anlık görüntü geri yüklemeden önce geri yükler.

>[!IMPORTANT]
>Devam etmeden önce işlem günlüğü yedeklemeleri tam ve bitişik zincirine sahip olduğunuzdan emin olun. Bu yedeklemeler veritabanının geçerli durumu geri yükleyemezsiniz.

1. Adım 1-6'tamamlamak [en son HANA anlık görüntüye kurtarmak](#recovering-to-the-most-recent-hana-snapshot).

2. Seçin **en son durumuna veritabanını kurtarma**.

 !["En son durumuna veritabanını Kurtar" seçin](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. En son HANA günlük yedeklerini konumunu belirtin. Konumun tüm HANA işlem günlüğü yedeklemeleri HANA anlık görüntüden en son durumuna içermesi gerekir.

 ![En son HANA günlük yedeklerini konumunu belirtin](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Veritabanını kurtarmak bir taban olarak bir yedek seçin. Bu örnekte, ekran görüntüsü HANA anlık görüntü depolama anlık görüntüsüne dahil HANA anlık görüntüsüdür. 

 ![Veritabanını kurtarmak bir taban olarak bir yedek seçin.](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Clear **kullanım değişim yedeklemeleri** HANA anlık görüntü saati ve en son durum farkları yoksa, kutuyu.

 ![Hiçbir farkları varsa "Kullanım değişim yedeklemeleri" onay kutusunu temizleyin](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Özet ekranında seçin **son** geri yükleme yordamı başlatmak için.

 ![Özet ekranında "Bitir"'yi tıklatın](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Başka bir noktaya geri
Arasındaki (depolama anlık görüntüsüne dahil) HANA anlık görüntü HANA anlık görüntü zaman içinde nokta kurtarmadan daha sonraki bir zamanda bir noktaya kurtarmak için aşağıdaki adımları gerçekleştirin:

1. Kurtarmayı gerçekleştirmek istediğiniz zaman HANA anlık görüntüden tüm işlem günlüğü yedeklemeleri sahip olduğunuzdan emin olun.
2. Altında yordama başlamadan [en son durumunu kurtarmak](#recovering-to-the-most-recent-state).
3. Yordamın 2 içinde adımda **kurtarma türünü belirtin** penceresinde, seçin **zamanında aşağıdaki noktası veritabanını kurtarma**ve ardından noktası süresini belirtin. 
4. 3-6 adımlarını tamamlayın.

### <a name="monitor-the-execution-of-snapshots"></a>Anlık görüntü yürütülmesi izlenemedi

HANA büyük örnekleri depolama anlık görüntüleri kullanma gibi aynı zamanda bu anlık görüntülerin yürütülmesi izlemeniz gerekir. Bir depolama anlık görüntü yürütür komut dosyası çıkışını bir dosyaya yazar ve Perl betikleri aynı konuma kaydeder. Her Depolama anlık görüntü için ayrı bir dosyaya yazılır. Her dosya çıktısı, anlık görüntü betiği yürüten çeşitli aşamaları gösterilmektedir:

1. Bir anlık görüntü oluşturmak için gereken birimleri bulur.
2. Bu birimlerden alınan anlık görüntü bulur.
3. Belirttiğiniz anlık görüntü sayısını eşleşecek şekilde nihai varolan anlık görüntüleri siler.
4. SAP HANA anlık görüntüsü oluşturur.
5. Depolama anlık görüntüsü birimleri oluşturur.
6. SAP HANA anlık görüntü siler.
7. En son anlık görüntüye yeniden adlandırır **.0**.

En önemli tanımlanan betik cab bu bölümü parçasıdır:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Komut dosyası HANA anlık görüntü oluşturma kayıtları nasıl bu örnekten görebilirsiniz. Genişleme durumda da, bu işlem ana düğümde başlatılır. Ana düğüm SAP HANA anlık görüntü çalışan düğümlerinin her biri zaman uyumlu oluşturulmasını başlatır. Depolama anlık görüntü sonra alınır. Depolama anlık görüntüleri başarılı yürütme sonrasında HANA anlık görüntü silinir. Ana düğümden HANA anlık görüntü silme işlemi başlatılır.


## <a name="disaster-recovery-principles"></a>Olağanüstü durum kurtarma ilkeleri
HANA büyük örnekleri farklı Azure bölgelerinde HANA büyük örneği Damgalar arasında bir olağanüstü durum kurtarma işlevi sunar. Örneğin, Azure ABD Batı bölgede HANA büyük örneği birimleri dağıtırsanız, BİZE Doğu bölgede olağanüstü durum kurtarma birimi olarak HANA büyük örneği birimlerini kullanabilirsiniz. DR bölgede başka bir HANA büyük örneği birim için ödeme yapmak gerektirdiği için daha önce belirtildiği gibi olağanüstü durum kurtarma otomatik olarak yapılandırılmamış. Olağanüstü durum kurtarma Kurulumu ölçek büyütme ve bunun yanı sıra ölçek genişletme ayarları için çalışır. 

Şu ana kadar dağıtılan senaryolarda, müşterilerin yüklü HANA örneğini kullanan üretim dışı sistemlerinin çalıştırmak için DR bölgede birimi kullanın. HANA büyük örneği birim üretim için kullanılan SKU aynı sku'sunun olması gerekir. Aşağıdaki resim Azure üretim bölgesinde sunucusu birimi arasında hangi disk yapılandırmasını gösterir ve olağanüstü durum kurtarma bölge gibi görünüyor:

![DR kurulum yapılandırma açısından disk](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Bu genel bakış grafikte gösterildiği gibi daha sonra disk birimleri ikinci bir set sipariş gerekir. Hedef disk birimleri olağanüstü durum kurtarma birimleri üretim örneğinde üretim birimler olarak aynı boyuttadır. Bu disk birimleri olağanüstü durum kurtarma sitesini HANA büyük örneği sunucu biriminde ile ilişkilendirilmiş. Aşağıdaki birimler üretim bölgesi DR siteye çoğaltılır:

- / hana/veri
- / hana/logbackups 
- /hana/shared (/ usr/sap içerir)

SAP HANA işlem günlüğü, bu birimlerdeki geri yükleme yapılır biçimde gerekli olmadığı için /hana/log birim çoğaltılmaz. 

Olağanüstü durum kurtarma işlevini temelini depolama çoğaltma işlevselliği HANA büyük örneği altyapısı tarafından sunulan sunmuştur. Depolama tarafında kullanılan işlev değişiklikleri için depolama birimi durum gibi zaman uyumsuz bir biçimde çoğaltma değişiklikleri sabit akışı yoktur. Bunun yerine, bu birimlerin anlık görüntüleri düzenli olarak oluşturulan olgu güvenen bir mekanizmadır. Zaten çoğaltılmış bir anlık görüntü ve henüz çoğaltılmaz yeni bir anlık görüntü arasındaki aralığı, ardından hedef disk birimleri olağanüstü durum kurtarma siteye aktarılır.  Bu anlık görüntüleri birimlerinde depolanır ve olağanüstü durum kurtarma yük devretme durumunda, bu birimlerde geri yüklenmesi gerekiyor.  

Veri miktarını anlık görüntüleri arasındaki farkları daha küçük olur önce ilk birimin tam veri aktarımını olmalıdır. Sonuç olarak, DR sitesi birimlerin her biri üretim sitede gerçekleştirilen birim anlık görüntülerini içerir. Sonuç olarak, üretim sistem geri alma olmadan kayıp verileri kurtarmak için bir önceki durumuna almak için bu DR sistem kullanabilirsiniz.

Bir HANA büyük örneği biriminde birden çok bağımsız SAP HANA örneğiyle MCOD dağıtımlar söz konusu olduğunda, tüm SAP HANA örnekleri için DR yan çoğaltılan depolama aldığınızdan emin beklenir.

Burada HANA sistemi çoğaltması yüksek kullanılabilirlik işlevselliği üretim sitenizdeki kullanmak ve DR sitesi için depolama tabanlı çoğaltma kullanmak durumlarda, her iki düğümün birincil siteden DR örneğine birimleri çoğaltılır. Hem birincil hem de DR için ikincil Çoğaltmada uyum sağlamak için DR sitesinde ek depolama alanı (aynı boyutta birincil düğüm itibariyle) satın almanız gerekir. 



>[!NOTE]
>HANA büyük örneği depolama çoğaltma işlevselliği yansıtma ve depolama anlık görüntüleri çoğaltılıyor. Depolama anlık görüntüleri içinde sunulduğu şekilde gerçekleştirme varsa [yedekleme ve geri yükleme](#backup-and-restore) bölümünde bu makalede, olamaz herhangi siteye çoğaltma için olağanüstü durum kurtarma. Depolama anlık görüntü yürütme, depolama siteye çoğaltma için olağanüstü durum kurtarma için bir önkoşuldur.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Olağanüstü durum kurtarma senaryosunda hazırlama
Bu senaryoda HANA büyük örneklerinde Azure bölgesi üretimde çalışan bir üretim sistemi vardır. Adımları için o HANA sistem SID'si "PRD" olduğunu ve DR Azure bölgesindeki HANA büyük örnekleri üzerinde çalışan bir üretim dışı sistem sahip varsayalım. İkincisi, SID'si "TST." olduğunu varsayalım Aşağıdaki resimde, bu yapılandırma gösterilmektedir:

![DR Kurulum başlangıcı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Sunucu örneği zaten varsa, SAP HANA Azure Hizmet Yönetimi ekler üzerinde ek depolama alanı birim kümesi ile birimleri ek kümesi TST çalıştırmakta olduğunuz HANA büyük örneği birimine üretim çoğaltma için hedef olarak sipariş edilen HANA örneği. Bu amaç için üretim HANA örneğinizi SID'si sağlamanız gerekir. SAP HANA Azure hizmet yönetimi üzerinde bu birimlerin eki onayladıktan sonra bu birimlerin HANA büyük örneği birimine bağlama gerekir.

![DR Kurulum sonraki adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

Sonraki adım, DR Azure bölgesindeki TST HANA örneği çalıştırdığı HANA büyük örneği biriminde ikinci SAP HANA örneğini yüklemenizi içindir. Yeni yüklenen SAP HANA örneği aynı SID olmalıdır. Oluşturulan kullanıcılar için aynı kullanıcı kimliği ve üretim örneğinin grup kimliği olması gerekir. Yükleme başarılı olursa, gerekir:

- 2. adım, bu makalenin önceki bölümlerinde açıklanan depolama anlık görüntü hazırlama yürütün.
- Henüz yapmadıysanız, bir ortak anahtar HANA büyük örneği birim DR birim için oluşturun. 3. adım, bu makalenin önceki bölümlerinde açıklanan depolama anlık görüntü hazırlama bakın.
- Bakımını *HANABackupCustomerDetails.txt* yeni HANA örneği ve test mi depolama alanına bağlantısı aşağıdaki düzgün çalışır.  
- DR Azure bölgesindeki HANA büyük örneği biriminde yeni yüklenen SAP HANA örneğini durdurun.
- Bu PRD birimler çıkarın ve SAP HANA Azure Hizmet Yönetimi başvurun. Bunlar depolama çoğaltma hedefi çalışması sırasında erişilebilir olamayacağı için birimlerin birimine bağlı kalın olamaz.  

![Çoğaltma kurmadan önce DR Kurulum adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

İşlemler ekibinin PRD Azure bölgesi üretimde ve PRD birimler DR Azure bölgesindeki arasında çoğaltma ilişki kurar.

>[!IMPORTANT]
>Çoğaltılmış SAP HANA veritabanına olağanüstü durum kurtarma sitedeki tutarlı bir duruma geri yüklemek gerekli olmadığından /hana/log birim çoğaltılmaz.

Ardından, ayarlayın veya RPO ve RTO olağanüstü durumda almak için depolama anlık görüntü yedekleme zamanlamasını ayarlayın. Kurtarma noktası hedefi en aza indirmek için aşağıdaki çoğaltma aralıkları HANA büyük örneği hizmetinde ayarlayın:
- Birleştirilmiş bir anlık görüntü tarafından kapsanan birimlerin (anlık görüntü türünde **hana**), 15 dakikada bir olağanüstü durum kurtarma sitedeki eşdeğer depolama birimi hedefler için çoğaltmak için ayarlama.
- İşlem günlüğü yedekleme biriminin (anlık görüntü türünde **günlükleri**), 3 dakikada bir olağanüstü durum kurtarma sitedeki eşdeğer depolama birimi hedefler için çoğaltmak için ayarlama.

Kurtarma noktası hedefi en aza indirmek için şunu ayarlayın:
- Gerçekleştirmek bir **hana** türü depolama anlık görüntü (bkz: "7. adım: anlık görüntüleri") 30 dakikada 1 saat.
- SAP HANA işlem günlüğü yedeklemeleri her 5 dakikada bir gerçekleştirin.
- Gerçekleştirmek bir **günlükleri** 5-15 dakikada bir anlık görüntü depolama yazın. Bu aralık süresi ile bir RPO yaklaşık 15-25 dakika elde edin.

Bu kurulum, işlem günlüğü yedeklemeleri, depolama anlık görüntüler ve HANA işlem çoğaltma sırası yedekleme birim ve/hana/verileri günlüğe ve (/ usr/sap içerir) /hana/shared Bu grafikte gösterilen veriler gibi görünmelidir:

 ![Bir işlem günlük yedek anlık görüntüsü ve bir zaman ekseni ek yansıtma arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Olağanüstü durum kurtarma durumunda bile daha iyi bir RPO'su elde etmek için HANA işlem günlüğü yedeklemeleri SAP HANA Azure (büyük örnekler) ile ilgili diğer Azure bölgesine kopyalayabilirsiniz. Bu ek RPO azaltma elde etmek için aşağıdaki adımları gerçekleştirin:

1. Geri HANA Hareketi oluşturan gibi bir sıklıkla /hana/logbackups için olabildiğince oturum açın.
2. NFS paylaşım barındırılan Azure sanal makineler için işlem günlüğü yedeklemeleri kopyalamak için rsync kullanın. Sanal makineleri Azure üretim bölgesinde ve DR bölgelerdeki Azure Sanal Ağları için geçerlidir. Üretim HANA büyük örnekleri Azure'a bağlanılıyor hattına iki Azure sanal ağlar bağlanmanız gerekir. Grafikleri görmek [ağ HANA büyük örneğiyle olağanüstü durum kurtarma değerlendirmeleri](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) bölümü. 
3. İşlem günlüğü yedeklemeleri VM'deki bölgede NFS bağlı Canlı depolama verildi.
4. Bir olağanüstü durum yük devretme durumda /hana/logbackups birimde daha kısa bir süre önce geçen işlem günlüğü yedeklemeleri NFS üzerinde olağanüstü durum kurtarma sitede paylaşmak bulduğunuz işlem günlüğü yedeklemeleri ek niteliğindedir. 
5. DR bölgesine üzerinden kaydedilebilir son yedeğini geri yüklemek için bir işlem günlüğü yedeklemesi başlatın.

Çoğaltma ilişkisi Kurulum HANA büyük örneği operations onaylayın ve yürütme depolama anlık görüntüsü yedekleri başlatmak, veri çoğaltma başlar.

![Çoğaltma kurmadan önce DR Kurulum adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Çoğaltma ilerledikçe, DR Azure bölgelerindeki PRD birimlerde anlık görüntüleri geri yüklenmez gibi. Bunlar yalnızca depolanır. Böyle bir durumda birimlerin bağlı olduğundan, DR Azure bölgesindeki sunucu biriminde PRD SAP HANA örnek yüklendikten sonra bu birimlerin uri'siyle neden kaldırdı durumu temsil eder. Bunlar ayrıca henüz geri yüklenmemiş depolama yedeklemeleri temsil eder.

Bir yük devretme durumunda depolama anlık görüntüsü en son yerine daha eski bir depolama anlık görüntü geri yüklemek seçebilirsiniz.

## <a name="disaster-recovery-failover-procedure"></a>Olağanüstü durum kurtarma yük devretme yordamı
DR siteye devretmek yaparken dikkate alınması gereken iki durum vardır:

- SAP HANA veritabanına veri son durumuna geri dönmek için gerekir. Bu durumda, Microsoft'a başvurun gerek olmadan yük devretme gerçekleştirmek bir Self Servis komut dosyası yok. Ancak, yeniden çalışma için Microsoft ile çalışması gerekir.
- Son çoğaltılmış anlık görüntü depolama anlık görüntüye geri yüklemek istediğiniz. Bu durumda, Microsoft ile çalışmak için gerekir. 

>[!NOTE]
>DR birimini temsil eder HANA büyük örneği biriminde yürütülmek üzere aşağıdaki adımları gerekir. 
 
En son çoğaltılan depolama anlık görüntü geri yüklemek için aşağıdaki adımları gerçekleştirin: 

1. Üretim dışı örneğinde HANA çalıştırdığınız HANA büyük örnekleri olağanüstü durum kurtarma birimi kapatın. Önceden yüklenmiş bir etkinliği olmayan HANA üretim örneği olduğundan budur.
2. SAP HANA işlemleri çalışır durumda olduğundan emin olun. Bu denetim için aşağıdaki komutu kullanın: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. Çıktı göstermesi gerekir **hdbdaemon** işlem durdurulmuş bir duruma ve diğer hiçbir HANA işlem çalışan veya başlatılmış bir durumda.
3. DR sitesi HANA büyük örneği biriminde komut dosyası yürütme *azure_hana_dr_failover.pl*. Komut dosyasını bir SAP HANA geri yüklenmesi için SID istiyor. İstendiğinde, bir veya yalnızca yazın çoğaltılmış ve tutulur SAP HANA SID *HANABackupCustomerDetails.txt* DR sitedeki HANA büyük örneği birimindeki dosya. 

      SAP HANA birden devredilir sahip olmak istiyorsanız, komut dosyasını birkaç kez çalıştırmanız gerekir. İstendiğinde, SAP HANA SID yük devri ve geri yüklemek istediğiniz yazın. Tamamlanma, komut dosyası bağlama noktaları HANA büyük örneği birimine eklenen birimlerin listesini gösterir. Bu liste, geri yüklenen DR birimleri de içerir.

4. Olağanüstü durum kurtarma sitede HANA büyük örneği birimine Linux işletim sistemi komutlarını kullanarak geri yüklenen olağanüstü durum kurtarma birimlerini bağlayın. 
6. Etkinliği olmayan SAP HANA üretim örneği başlatın.
7. RPO süresini azaltmak için işlem günlüğü yedekleme günlükleri kopyalamak seçerseniz, bu işlem günlüğü yedeklemeleri yeni takılı DR/hana/logbackups dizine birleştirmeniz gerekir. Var olan yedekleri üzerine yazmayın. Bir depolama anlık görüntüsü en son çoğaltma ile çoğaltılmamış yeni yedeklerini kopyalayın.
8. DR Azure bölgesindeki /hana/shared/PRD birime çoğaltılmamış anlık görüntüleri dışında tek dosyaları da geri yükleyebilirsiniz. 

Gerçek çoğaltma ilişkisi etkilemeden DR yük devretmeyi de test edebilirsiniz. Yük devretme testi gerçekleştirmek için 1 ve 2 yukarıdaki adımları uygulayın ve ardından aşağıdaki 3. adıma geçin.

>[!IMPORTANT]
>Yapmak *değil* üretim işlemler işlemi boyunca DR sitesi oluşturduğunuz örneğinde çalışan **bir yük devretme sınaması** adım 3'te sunulan komut dosyası. Bu komut birincil siteye hiçbir ilişki olan birimleri kümesi oluşturur. Sonuç olarak, birincil siteye eşitleme işlemi *değil* mümkün. 

Yük devretme sınaması için adım 3:

DR sitesi HANA büyük örneği biriminde komut dosyası yürütme **azure_hana_test_dr_failover.pl**. Bu komut dosyası *değil* DR sitesi ile birincil site arasındaki çoğaltma ilişkisini durduruluyor. Bunun yerine, bu komut dosyası DR depolama birimleri kopyalama değil. Kopyalama işlemi başarılı olduktan sonra kopyalanan birimleri en son anlık görüntü durumuna geri ve DR birimine bağlanır. Komut dosyasını bir SAP HANA geri yüklenmesi için SID istiyor. Bir veya yalnızca türü çoğaltılmış ve tutulur SAP HANA SID *HANABackupCustomerDetails.txt* DR sitedeki HANA büyük örneği birimindeki dosya. 

Test etmek için birden fazla SAP HANA örneğe sahip olmak istiyorsanız, komut dosyasını birkaç kez çalıştırmanız gerekir. İstendiğinde, yük devretme için test etmek istediğiniz örneği SAP HANA SID'si yazın. Tamamlandıktan sonra komut dosyasını bağlama noktaları HANA büyük örneği birimine eklenen birimlerin bir listesini gösterir. Bu liste, kopyalanan DR birimleri de içerir.

4. adıma ilerleyin.

   >[!NOTE]
   >Saat önce silindi ve daha önceki bir anlık görüntüye ayarlanacak DR birimleri gereken bazı veriler kurtarması için DR sitesi yük ihtiyacınız varsa, bu yordamı uygular. 

4. Üretim dışı örneğinde HANA çalıştırdığınız HANA büyük örnekleri olağanüstü durum kurtarma birimi kapatın. Önceden yüklenmiş bir etkinliği olmayan HANA üretim örneği olduğundan budur.
5. SAP HANA işlemleri çalışır durumda olduğundan emin olun. Bu denetim için aşağıdaki komutu kullanın: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. Çıktı göstermesi gerekir **hdbdaemon** işlem durdurulmuş bir duruma ve diğer hiçbir HANA işlem çalışan veya başlatılmış bir durumda.
6. Hangi anlık görüntü adı veya SAP HANA yedekleme kimliği geri olağanüstü durum kurtarma sitesi sahip istediğinize karar verin. Gerçek bir olağanüstü durum kurtarma durumlarda, bu anlık görüntü genellikle en son anlık görüntüsüdür. Kayıp verileri kurtarmanız gerekiyorsa, önceki bir anlık görüntüsünü seçin.
7. Yüksek öncelikli destek isteği aracılığıyla Azure desteği ile iletişim kurun. Bu anlık görüntü (adı ile tarih anlık görüntü) veya DR sitesinde HANA yedekleme kimliği geri yüklemek için isteyin. Operations yan/hana/veri hacmi yalnızca geri yükleyen varsayılandır. / Hana/logbackups birimleri de sahip olmak istiyorsanız, özellikle, durum gerekir. */Hana/shared birime geri yüklemeyin.* Bunun yerine, global.ini dışı gibi belirli dosyaları seçmelisiniz **.snapshot** dizin ve alt dizinlerinde, / hana/yeniden bağlayın sonra paylaşılan birim için PRD. 

   Operations tarafında aşağıdaki adımlardan oluşur:

   a. Olağanüstü durum kurtarma birimlere üretim birimden anlık görüntü çoğaltma durduruldu. Kesinti üretim sitesinde olağanüstü durum kurtarma yordamı gerçekleştirmek gereken neden ise bu kesintisi zaten oluşmuş.
   
   b. Depolama adı anlık görüntü veya olağanüstü durum kurtarma birimlerde geri seçtiğiniz yedekleme kimlikli anlık görüntüsünü alın.
   
   c. Geri yüklendikten sonra olağanüstü durum kurtarma bölgede HANA büyük örneği birimlerine takılması olağanüstü durum kurtarma birimler kullanılabilir.
      
8. Olağanüstü durum kurtarma birimlerini olağanüstü durum kurtarma sitedeki HANA büyük örneği birimine bağlayın. 
9. Etkinliği olmayan SAP HANA üretim örneği başlatın.
10. RPO süresini azaltmak için işlem günlüğü yedekleme günlükleri kopyalamak seçerseniz, bu işlem günlüğü yedeklemeleri yeni takılı DR/hana/logbackups dizine birleştirmeniz gerekir. Var olan yedekleri üzerine yazmayın. Bir depolama anlık görüntüsü en son çoğaltma ile çoğaltılmamış yeni yedeklerini kopyalayın.
11. DR Azure bölgesindeki /hana/shared/PRD birime çoğaltılmamış anlık görüntüleri dışında tek dosyaları da geri yükleyebilirsiniz.

Geri yüklenen depolama anlık görüntü ve kullanılabilir işlem günlüğü yedeklemeleri göre SAP HANA üretim örneği kurtarma adımları dizisini içerir:

1. Yedekleme konumu değiştirme **/hana/logbackups** SAP HANA Studio'yu kullanarak.
   ![DR Kurtarma yedekleme konumunu değiştirme](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

2. SAP HANA yedek dosya konumlarını tarayan ve geri yüklemek için en son işlem günlüğü yedeklemesi önerir. Tarama aşağıdaki belirir gibi bir ekran kadar birkaç dakika sürebilir: ![DR kurtarma için işlem günlüğü yedeklemeleri listesi](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

3. Varsayılan ayarlardan bazıları ayarlayın:

      - Clear **değişim yedeklemeleri kullanmak**.
      - Seçin **başlatılamadı günlüğü alanı**.

   ![Başlatılamadı günlüğü alanı ayarlama](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

4. **Son**’u seçin.

   ![DR geri yükleme işlemini tamamladıktan](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Bir ilerleme penceresi, burada gösterildiği gibi görünmelidir. Olağanüstü durum kurtarma geri yüklemeye üç düğümlü genişleme SAP HANA yapılandırmasının örnektir göz önünde bulundurun.

![Geri yükleme durumu](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Geri yükleme sırasında askıda görünüyorsa **son** ekran ve yok ilerleme ekranı göster, SAP HANA hepsinin çalışan düğümleri üzerinde çalıştığını doğrulayın. Gerekirse, SAP HANA örnekleri el ile başlatın.


### <a name="failback-from-a-dr-to-a-production-site"></a>Bir üretim siteye bir DR gelen yeniden çalışma
Bir üretim siteye geri DR başarısız olabilir. Olağanüstü durum kurtarma siteye yük devretme sorunları Azure bölgesi üretimde ve kaybolan verileri kurtarmak için gerekmez kaynaklanmıştır bir senaryo bakalım. SAP üretim yükünüzü biraz olağanüstü durum kurtarma sitede çalışıyor olması. Üretim sitesini sorunlarını çözümlendi olarak üretim sitede yeniden çalıştırmak istiyor. Veri kaybı yapılamıyor çünkü üretim sitesini adımla geri çeşitli adımları ve Azure işlemleri ekipteki SAP HANA Kapat işbirliği içerir. Bu sorun çözümlendikten sonra üretim sitesine geri Eşitlemeyi başlatmak için işlemler ekibinin tetiklemek için hazır.

Bu, uygulamanız gereken adımlar dizisi oluşur:

1. SAP HANA Azure işlemleri ekipteki şimdi üretim durumu temsil eden olağanüstü durum kurtarma depolama birimlerini üretim depolama birimlerden eşitlemek için tetikleyici alır. Bu durumda üretim sitesini HANA büyük örneği biriminde kapatılır.
2. SAP HANA Azure işlemleri ekipteki çoğaltma izler ve bunu size bildiren önce aynı duruma emin olmanızı sağlar.
3. Olağanüstü durum kurtarma sitede üretim HANA örneği kullanan uygulamaları kapatın. Ardından bir HANA işlem günlüğü yedeklemesi gerçekleştirin. Ardından, olağanüstü durum kurtarma sitesini HANA büyük örneği birimlerinde çalışan HANA örneğini durdurun.
4. Olağanüstü durum kurtarma sitedeki HANA büyük örneği biriminde HANA örneği kapattığınızda, işletim ekibi el ile disk birimleri yeniden eşitler.
5. SAP HANA Azure işlemleri ekipteki HANA büyük örneği birim üretim sitenin yeniden başlatır ve bunu size aktarır. SAP HANA örneğinin HANA büyük örneği birim başlangıç sırasında bir kapanma durumunda olduğundan emin olun.
6. İçin olağanüstü durum kurtarma sitesi üzerinden ve daha önce başarısız olduğunda yaptığınız gibi aynı veritabanı geri yükleme adımlarını gerçekleştirin.

### <a name="monitor-disaster-recovery-replication"></a>Olağanüstü durum kurtarma çoğaltmayı izleme

Komut dosyası yürütme, depolama çoğaltma ilerleme durumunu izleyebilirsiniz `azure_hana_replication_status.pl`. Bu betiği beklendiği gibi olağanüstü durum kurtarma konumunda geçerli olarak çalışan bir birimi çalıştırmanız gerekir. Komut dosyası çoğaltma etkin olup bağımsız olarak çalışır. Komut dosyası, her bir olağanüstü durum kurtarma konumu kiracınızda HANA büyük örneği birim için çalıştırılabilir. Önyükleme birimi hakkındaki ayrıntıları almak için kullanılamaz.

Bu komut ile komut dosyası çağırın:
```
./replication_status.pl <HANA SID>
```

Çıkış, aşağıdaki bölümlere birim aşağı ayrılır:  

- Bağlantı durumu
- Geçerli çoğaltma etkinliği
- Çoğaltılan son anlık görüntü 
- En son anlık görüntü boyutu
- Geçerli gecikme süresi arasında anlık görüntüler (son tamamlanan anlık görüntü çoğaltma arasındaki ve şimdi)

Bağlantı durumu olarak gösterir **etkin** konumları arasındaki bağlantı çalışmıyor veya şu anda devam eden yük devretme olayından sürece. Çoğaltma etkinliği herhangi bir veri şu anda çoğaltılmakta olan veya boş olup olmadığını ya da diğer etkinlikleri şu anda bağlantısını gerçekleştiği giderir. Çoğaltılan son anlık görüntü yalnızca olarak görünmesi gereken `snapmirror…`. Son anlık görüntünün boyutu sonra görüntülenir. Son olarak, gecikme süresi gösterilir. Çoğaltma tamamlandığında zamanlanmış çoğaltma süresini gecikme süresini temsil eder. Çoğaltma başlatıldı olsa bile bir gecikme süresi veri çoğaltması, özellikle ilk çoğaltma bir saatten daha büyük olabilir. Gecikme süresi, devam eden çoğaltma tamamlanana kadar artmaya devam eder.

Çıktının bir örnek verilmiştir:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












