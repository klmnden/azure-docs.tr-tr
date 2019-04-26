---
title: Test sonuçları Hyper-V VM çoğaltması için Azure Site Recovery ile ikincil bir siteye VMM bulutlarında | Microsoft Docs
description: Bu makalede, Azure Site RECOVERY'yi kullanarak ikincil bir siteye VMM bulutlarındaki Hyper-V Vm'lerini çoğaltma için performans testleri hakkında bilgi sağlar.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi
ms.openlocfilehash: 7e2f5c344a0fb632956ab5d5b951ee69cff528ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60363603"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>İkincil bir siteye Hyper-V çoğaltma için test sonuçları


Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V Vm'lerini ikincil bir veri merkezine çoğaltma testi performans sonuçları sağlar.

## <a name="test-goals"></a>Test amaçları

Site Recovery, kararlı durum çoğaltma sırasında performansını incelemek için test etme amacı oluştu.

- Kararlı durum çoğaltma Vm'leri ilk çoğaltması tamamlanan ve delta değişikliklerinin eşitleme gerçekleşir.
- Beklenmeyen kesintiler sürece, hangi VM'lerin çoğunda durumda olduğundan kalır, kararlı bir duruma kullanma performansını ölçmek önemlidir.
- Her bir sitedeki VMM sunucusuyla iki şirket içi sitenin sınama dağıtımı oluşmuştur. Bu test dağıtımının birincil site olarak işlev gören Genel merkez ve şube ofis ikincil veya kurtarma bir merkez ofis/şube office dağıtımı, tipik sitedir.

## <a name="what-we-did"></a>Ne yaptık

Ne biz testinde başarısız olan şu şekildedir:

1. VMM şablonları kullanılarak oluşturulan VM'ler.
2. VM'ler çalışmaya ve 12 saatin üzerinde temel performans ölçümlerinin yakalanır.
3. Oluşturulan bulut birincil ve kurtarma VMM sunucuları.
4. Site Recovery, kaynak ve kurtarma arasında bulut eşleştirme dahil olmak üzere yapılandırılmış çoğaltma.
5. Sanal makineler için korumayı etkinleştirmeden ve bunları ilk çoğaltmayı tamamlamak için izin verilir.
6. Birkaç saat sistem sabitleme için beklendi.
7. Performans ölçümleri, 12 burada tüm VM'lerin bir beklenen çoğaltma durumunda bu 12 saat boyunca kaldığını saatten, yakalanan.
8. Delta çoğaltma performans ölçümlerinin yanı sıra temel performans ölçümlerinin arasında ölçülür.


## <a name="primary-server-performance"></a>Birincil sunucu performansı

* Hyper-V (Site Recovery tarafından kullanılan) çoğaltma zaman uyumsuz olarak, bir günlük dosyası, en az depolama ek yükü birincil sunucuda ile yapılan değişiklikleri izler.
* Hyper-V çoğaltma, IOPS, izleme için ek yükü en aza Self tutulan önbellek kullanır. Depoladığı bellekte VHDX yazar ve bunları kurtarma sitesine gönderilen günlük süreden önce günlük dosyasına boşaltır. Yazma önceden belirlenmiş bir sınırına bir disk temizleme de olur.
* Aşağıdaki grafikte, çoğaltma kararlı bir duruma IOPS yükü gösterir. Çoğaltma ek yükü nedeniyle IOPS yaklaşık %5, oldukça düşük olduğu olduğunu görebiliriz.

  ![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V çoğaltma, disk performansını iyileştirmek için birincil sunucuda bellek kullanır. Aşağıdaki grafikte gösterildiği gibi Bellek Yükü birincil kümedeki tüm sunucuların Marjinal. Ek yükü gösterilen Hyper-V sunucusunda yüklü toplam belleği ile karşılaştırıldığında, çoğaltma tarafından kullanılan bellek yüzdesi bellektir.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V çoğaltma, en düşük CPU ek yükü vardır. Grafikte gösterildiği gibi % 2-3 aralığında çoğaltma ek yüktür.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>İkincil sunucu performansı

Hyper-V çoğaltma, depolama işlemlerinin sayısını en iyi duruma getirmek için kurtarma sunucusunda az miktarda bellek kullanır. Kurtarma sunucusunda bellek kullanımı grafiği özetler. Ek yükü gösterilen Hyper-V sunucusunda yüklü toplam belleği ile karşılaştırıldığında, çoğaltma tarafından kullanılan bellek yüzdesi bellektir.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744916.png)

Birincil sitede yazma işlemlerinin sayısı işlevi g/ç işlemleri kurtarma miktarıdır. Şimdi toplam g/ç işlemleri karşılaştırıldığında kurtarma sitesinde toplam g/ç işlemleri bakmak ve yazma işlemlerini birincil sitede. Toplam IOPS kurtarma olduğunu grafikleri Göster

* Yaklaşık 1,5 katı yazma IOPS değeri birincil.
* Yaklaşık %37 birincil sitede toplam IOPS.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744917.png)

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Ağ kullanımı üzerinde etkisi

275 Mb'ın ağ bant genişliği saniye başına ortalama 5 Gb'ın mevcut bir bant saniyede karşı (ile sıkıştırma) etkin, birincil ve kurtarma düğümler arasında kullanıldı.

![Sonuçlar ağı kullanımı](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM performansı üzerindeki etkisi

Önemli bir husus çoğaltma sanal makineler üzerinde çalışan üretim iş yükleri üzerindeki etkisidir. Birincil sitenin çoğaltma için yeterince sağlandığında iş yükleri üzerinde herhangi etkisi olmamalıdır. Sanal makinelerde çalışan iş yükleri kararlı durum çoğaltma sırasında etkilenmez Hyper-V çoğaltma'nın lightweight izleme mekanizması sağlar. Bu, aşağıdaki grafiklerde gösterilmiştir.

Bu grafik, IOPS ve çoğaltma etkinleştirildikten sonra önce farklı iş yüklerini çalıştıran sanal makineler tarafından gerçekleştirilen gösterir. İkisi arasındaki fark yoktur gözlemleyebilirsiniz.

![Çoğaltma efekti sonuçları](./media/hyper-v-vmm-performance-results/IC744920.png)

Aşağıdaki grafikte, önce farklı iş yüklerini çalıştıran sanal makinelerin ve çoğaltma etkinleştirildikten sonra aktarım hızını gösterir. Bu çoğaltma hiçbir önemli bir etkisi gözlemleyebilirsiniz.

![Sonuçlar çoğaltma etkileri](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Sonuç

Sonuçları, Site Recovery, Hyper-V çoğaltma ile birlikte en büyük bir küme için ek yükü ile iyi ölçeklenebilir açıkça gösterir. Site Recovery basit dağıtım, çoğaltma, yönetim ve izleme sağlar. Hyper-V çoğaltma, başarılı bir çoğaltma ölçeklendirme için gerekli altyapıyı sağlar. 

## <a name="test-environment-details"></a>Test ortamı ayrıntıları

### <a name="primary-site"></a>Birincil site

* Birincil site 470 sanal makineleri çalıştıran, beş Hyper-V sunucuları içeren bir küme sahiptir.
* VM'ler farklı iş yüklerini çalıştırın ve tüm Site Recovery koruması etkinleştirilmiş.
* Depolama düğümü için iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.
* Her küme sunucusu bir Gbps dört ağ kartı (NIC) vardır.
* Bir iSCSI özel ağa bağlı iki ağ kartları ve iki bir dış Kurumsal ağa bağlanır. Dış ağlar birini yalnızca küme iletişimi için ayrılmıştır.

![Birincil donanım gereksinimleri](./media/hyper-v-vmm-performance-results/IC744922.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V kümesindeki sunucular: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |256 128ESTLAB HOST25 sahip |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 \@ 2.20 GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server 2012 veritabanı R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-site"></a>İkincil site

* İkincil sitenin altı düğümlü yük devretme kümesi vardır.
* Depolama düğümü için iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.

![Birincil donanım belirtimi](./media/hyper-v-vmm-performance-results/IC744923.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V kümesindeki sunucular: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel(r) Xeon(R) CPU E5-2630 0 \@ 2.30 GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST17 |128 |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 \@ 2.20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST24 |256 |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 \@ 2.20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server 2012 veritabanı R2 (x 64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Sunucu iş yükleri

* Test amaçları için Kurumsal müşteri senaryolarda yaygın olarak kullanılan iş yüklerini biz Çekildi.
* Kullandığımız [IOMeter](http://www.iometer.org) benzetimi için tabloda özetlenen iş yükü özelliği ile.
* Tüm IOMeter profilleri desenleri iş yükleri için iki katına benzetmek için rastgele bayt yazma yazmak için ayarlanır.

| İş yükü | G/ç boyutu (KB) | % Erişim | % Okuma | Bekleyen g/ç | G/ç deseni |
| --- | --- | --- | --- | --- | --- |
| Dosya Sunucusu |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Rastgele tüm %100 |
| SQL Server (birim 1) SQL Server (2 birim) |864 |100%100% |70%0% |88 |% 100 random100% sıralı |
| Exchange |32 |%100 |67% |8 |rastgele %100 |
| İş istasyonu/VDI |464 |66%34% |70%95% |11 |Her iki rastgele %100 |
| Web dosya sunucusu |4864 |33%34%33% |95%95%95% |888 |Rastgele tüm %75 |

### <a name="vm-configuration"></a>VM yapılandırması

* Birincil küme 470 Vm'lerde.
* VHDX disk bulunduğu tüm VM'ler.
* Tabloda özetlenen iş yükleri çalıştıran VM'ler. Tüm VMM şablonları ile oluşturulmuş.

| İş yükü | # VM'ler | Minimum RAM (GB) | En fazla RAM (GB) | VM başına mantıksal disk boyutu (GB) | Maksimum IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Dosya Sunucusu |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Web sunucusu |149 |.5 |1 |80 |6 |
| TOPLAM |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery ayarları

* Site Recovery, şirket içi-şirket içi koruma için yapılandırıldı
* VMM sunucusu, Hyper-V küme sunucuları ve Vm'leri içeren yapılandırılmış, dört bulut içerir.

| Birincil VMM Bulutu | Korumalı VM'ler | Çoğaltma sıklığı | Ek kurtarma noktaları |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 dakika |None |
| PrimaryCloudRpo30s |47 |30 saniye |None |
| PrimaryCloudRpo30sArp1 |47 |30 saniye |1 |
| PrimaryCloudRpo5m |235 |5 dk. |None |

### <a name="performance-metrics"></a>Performans ölçümleri

Tablo, performans ölçümlerini ve dağıtımında ölçülen sayaçları özetler.

| Ölçüm | Sayaç |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Uygun bellek |\Memory\Available MBayt |
| IOPS |\PhysicalDisk (_Total) \Disk aktarımı/sn |
| VM okuma işlemi (IOPS) / sn |\Hyper-V sanal depolama cihazı (\<VHD >) \Read işlemi/sn |
| VM (IOPS) yazma işlemi/sn |\Hyper-V sanal depolama cihazı (\<VHD >) \Write işlemleri/sn |
| Aktarım hızı VM okuyun |\Hyper-V sanal depolama cihazı (\<VHD >) \Read bayt/sn |
| VM yazma üretimi |\Hyper-V sanal depolama cihazı (\<VHD >) \Write bayt/sn |

## <a name="next-steps"></a>Sonraki adımlar

[Çoğaltmayı ayarlama](hyper-v-vmm-disaster-recovery.md)
