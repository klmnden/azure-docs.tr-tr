---
title: SAP hana (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Docs
description: Yüksek kullanılabilirlik ve (büyük örnekler) Azure üzerinde SAP hana olağanüstü durum kurtarma planı oluştur
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 078ce7e2acd93ecab6b37407f460672d4acb1853
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707336"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA büyük örnekleri azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma 

>[!IMPORTANT]
>Bu belge, yönetim belgeleri SAP HANA veya SAP notları hiçbir yerini almaktadır. Okuyucu düz bir anlayış ve SAP HANA yönetimi ve işlemleri, özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma konuları uzmanlığa sahip beklenmektedir.

Adımlar ve ortamınızdaki ve HANA sürümleri ve sürümler ile yapılan işlemler alıştırma önemlidir. Bu belgede açıklanan bazı işlemler, daha iyi bir genel anlamak için Basitleştirilmiş ve nihai işlemi el kitapları için ayrıntılı adımları olarak kullanılmak üzere tasarlanmamıştır. İşlemi el kitapları yapılandırmalarınız için oluşturmak istiyorsanız, test ve işlemlerinizi çalışma ve belirli yapılandırmalarınız için ilgili işlemleri belge gerekir. 


Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DR), görev açısından kritik SAP HANA (büyük örnekler) Azure sunucu üzerinde çalışan önemli yönleri şunlardır. SAP, sistem entegratörü veya Microsoft ile düzgün şekilde Mimar ve doğru yüksek kullanılabilirlik ve olağanüstü durum kurtarma stratejileri uygulamak için iş önemlidir. Ortamınıza özgü kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi göz önünde bulundurmanız önemlidir.

Microsoft, bazı HANA büyük örnekleri ile SAP HANA yüksek kullanılabilirlik özellikleri destekler. Bu özellikler şunları içerir:

- **Depolama çoğaltma**: Depolama sisteminin özelliği tüm verileri başka bir Azure bölgesindeki başka bir HANA büyük örneği damgasında çoğaltın. SAP HANA, bu yöntem bağımsız olarak çalışır. Bu işlev, HANA büyük örnekler için sunulan varsayılan olağanüstü durum kurtarma mekanizmadır.
- **HANA sistem çoğaltması**: [Tüm SAP HANA veri çoğaltmanın](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) ayrı bir SAP HANA sistem. Kurtarma süresi hedefi düzenli aralıklarla veri çoğaltma ile en aza indirilir. SAP HANA, zaman uyumsuz ve zaman uyumlu, bellek ve zaman uyumlu modda destekler. Zaman uyumlu modda aynı veri merkezinde veya küçüktür 100 km uzaklıkta içinde olan SAP HANA sistemleri için kullanılır. Geçerli tasarım HANA büyük örneği damga ile yalnızca tek bir bölgede yüksek kullanılabilirlik için HANA sistem çoğaltması kullanılabilir. HANA sistem çoğaltması, başka bir Azure bölgesine olağanüstü durum kurtarma yapılandırmaları için üçüncü taraf ters proxy ya da yönlendirme bileşeni gerektirir. 
- **Ana bilgisayar otomatik yük devretme**: Yerel Hata kurtarma çözümü için SAP HANA, HANA sistem çoğaltması için bir alternatiftir. Ana düğüm kullanılamaz duruma gelirse, Ölçek Genişletme modunda bir veya daha fazla bekleme SAP HANA düğümleri yapılandırmak ve SAP HANA otomatik olarak bir bekleme düğüme devreder.

SAP HANA (büyük örnekler) azure'da iki Azure bölgesinde dört coğrafi alanları (ABD, Avustralya, Avrupa ve Japonya) olarak sunulur. HANA büyük örneği Damgalar barındıran iki bölgeleri coğrafi alanda ayrı Adanmış ağ bağlantı hatlarına bağlı. Bunlar depolama anlık görüntüleri çoğaltmak için olağanüstü durum kurtarma yöntemler sağlamak için kullanılır. Çoğaltma, varsayılan olarak oluşturulmuş olmaz ancak olağanüstü durum kurtarma işlevi sipariş müşteriler için ayarlanmış. Depolama çoğaltma HANA büyük örnekler için depolama anlık görüntüleri kullanımını bağlıdır. Farklı bir coğrafi alanda bir DR bölgesinde olarak bir Azure bölgesi seçin mümkün değildir. 

Aşağıdaki tabloda, birleşimler ve şu anda desteklenen yüksek kullanılabilirlik ve olağanüstü durum kurtarma yöntemler gösterilmektedir:

| HANA büyük örnekleri desteklenen bir senaryo | Yüksek kullanılabilirlik seçeneği | Olağanüstü durum kurtarma seçeneği | Açıklamalar |
| --- | --- | --- | --- |
| Tek düğüm | Kullanılamıyor. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu. | |
| Ana bilgisayar otomatik yük devretme: Ölçeği genişletme (ile veya olmadan bekleme)<br /> 1 + 1 dahil olmak üzere | Etkin rolü alma bekleme ile mümkün.<br /> HANA rol anahtar denetler. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltması kullanarak DR eşitleme. | HANA birim kümeleri tüm düğümleri eklenir.<br /> DR sitesi düğümleri sayıları aynı olmalıdır. |
| HANA sistem çoğaltması | Birincil veya ikincil kurulum ile mümkün.<br /> İkincil bir yük devretme durumunda, birincil rol taşır.<br /> Yük devretme, HANA sistem çoğaltması ve işletim sistemi denetler. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltması kullanarak DR eşitleme.<br /> HANA sistem çoğaltması kullanarak DR henüz üçüncü taraflara ait bileşenleri mümkün değildir. | Disk birimleri ayrı kümesi her düğüme eklenmiş.<br /> Üretim sitesinin ikincil çoğaltma yalnızca disk birimlerini DR konuma çoğaltılır.<br /> Birimlerin bir kümesini DR sitede gereklidir. | 

Adanmış bir DR Kurulum, burada DR sitesi HANA büyük örneği biriminde herhangi bir iş yükü veya üretim dışı sistem çalıştırmak için kullanılmayan ' dir. Birim pasif ve yalnızca bir olağanüstü durum yük devretme yürütülürse dağıtılır. Ancak, bu kurulum birçok müşteri için tercih edilen bir seçim değil.

Başvuru [HLI desteklenen senaryoları](hana-supported-scenario.md) Mimarinizi için depolama düzeni ve ethernet ayrıntıları öğrenin.

> [!NOTE]
> [SAP HANA MCOD dağıtımları](https://launchpad.support.sap.com/#/notes/1681092) (birden çok HANA örneklerine bir birimi) senaryo iş yöntemleri HA ve DR ile planlamanızda tabloda listelendiği gibi. HANA sistem çoğaltması SLES üzerinde Pacemaker tabanlı bir otomatik yük devretme kümesi ile bir özel durum kullanılır. Böyle bir durumda, yalnızca birim başına bir HANA örneği destekler. İçin [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000) dağıtımları, yalnızca depolama tabanlı olmayan HA ve DR yöntemler çalışır birden fazla Kiracı dağıtılması durumunda. Dağıtılan bir kiracıyla listelenen tüm geçerli yöntemlerdir.  

Çok amaçlı bir DR Kurulum, DR sitesi HANA büyük örneği biriminde bir üretim dışı iş yükü çalıştığı ' dir. Olağanüstü durum, üretim dışı sistemi Kapat durumunda (ek) birim depolama çoğaltılan kümeleri bağlayın ve üretim HANA örneği başlatın. HANA büyük örneği olağanüstü durum kurtarma işlevlerini kullanan müşterilerin çoğu bu yapılandırmayı kullanın. 


Aşağıdaki makalelerde SAP, SAP HANA yüksek kullanılabilirliği hakkında daha fazla bilgi bulabilirsiniz: 

- [SAP HANA yüksek kullanılabilirlik teknik incelemesi](https://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA Yönetim Kılavuzu](https://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP HANA sistem çoğaltması üzerinde SAP HANA Academy videosunda](https://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP destek Not #1999880 – SAP HANA sistem çoğaltması hakkında SSS](https://apps.support.sap.com/sap/support/knowledge/preview/en/1999880)
- [Yedekleme desteği Not #2165547 – geri SAP HANA SAP ve SAP HANA sistem çoğaltma ortamı içinde geri yükleme](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [Donanım değişimi en az/sıfır kapalı kalma süresi için destek Not #1984882 – SAP HANA sistem çoğaltması kullanarak SAP](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>HANA büyük örnekleri ile olağanüstü durum kurtarma için ağ konuları

HANA büyük örnekleri olağanüstü durum kurtarma işlevlerini yararlanmak için tasarım iki Azure bölgesi için ağ bağlantısı gerekir. Şirket içi, ana Azure bölgesinde Azure ExpressRoute bağlantı hattı bağlantısı ve şirket içi, olağanüstü durum kurtarma bölgesindeki başka bir bağlantı hattı bağlantı gerekir. Bu ölçü, içinde bir sorun var. Microsoft Kurumsal kenar yönlendirici (MSEE) konumu dahil olmak üzere bir Azure bölgesinde bir durum kapsar.

İkinci bir ölçü başka bir bölgede HANA büyük örnekleri bağlanan bir ExpressRoute bağlantı hattı için bir bölgede (büyük örnekler) Azure üzerinde SAP hana'ya bağlanan tüm Azure sanal ağları bağlanabilirsiniz. Bu *çapraz bağlanma*, bir Azure üzerinde çalışan hizmetleri sanal ağ, bölge 1, bölge 2 ve tersine HANA büyük örneği birimleri bağlanabilir. Bu ölçü, Azure ile şirket içi konumunuza bağlanan tek MSEE konumu çevrimdışı bir durumda yöneliktir.

Aşağıdaki grafikte, olağanüstü durum kurtarma çalışmaları için esnek bir yapılandırma gösterilmektedir:

![Olağanüstü durum kurtarma için en uygun yapılandırma](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>HANA büyük örnekleri depolama çoğaltması, olağanüstü durum kurtarma için diğer gereksinimleri

HANA büyük örnekleri ile bir olağanüstü durum kurtarma kurulumu için yukarıdaki gereksinimlere ek olarak, şunları yapmalısınız:

- SAP HANA (büyük örnekler) Azure SKU'sku ' ları üretim aynı boyutta üzerinde sıralayabilir ve bunları olağanüstü durum kurtarma bölgesinde dağıtabilirsiniz. Geçerli müşteri dağıtımlarında, bu örnekler, üretim dışı HANA örnekleri çalıştırmak için kullanılır. Bu yapılandırmalar olarak ifade edilir *çok amaçlı DR kurulumları*.   
- DR sitesinde ek depolama alanı, her biri olağanüstü durum kurtarma sitesini kurtarmak istediğiniz (büyük örnekler) Azure SKU'ları SAP HANA için sipariş. Ek depolama alanı satın alma depolama birimleri ayırmanıza olanak tanır. Depolama çoğaltma Azure bölgesine olağanüstü durum kurtarma, üretim ortamında Azure bölgesini hedef birimler ayırabilirsiniz.
- Durumda, burada birincil HSR ayarladıktan ve DR sitesine göre depolama çoğaltma kurulumu, bu nedenle hem birincil hem de Kurtarma sitesinde ek depolama alanı satın almanız gerekir ve ikincil düğüm veri DR sitesine çoğaltılan.

  **Sonraki adımlar**
- Başvuru [yedekleme ve geri yükleme](hana-backup-restore.md).













