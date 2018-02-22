---
title: "Azure Site Recovery ile ikincil siteye VMM bulutlarındaki Hyper-V sanal makineleri çoğaltma için sonuçları test | Microsoft Docs"
description: "Bu makale, Azure Site RECOVERY'yi kullanarak ikincil bir siteye VMM bulutlarındaki Hyper-V Vm'lerini çoğaltma için performans testleri hakkında bilgi sağlar."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/13/2018
ms.author: raynew
ms.openlocfilehash: e15f435a3f32b8908b5b93bccc6c57710ab589bc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>İkincil bir siteye Hyper-V çoğaltma için test sonuçları

Bu makale performans System Center Virtual Machine Manager (VMM) bulutlarında, Hyper-V Vm'lerini ikincil bir veri merkezine çoğaltırken testi sonuçlarını sağlar.

## <a name="test-goals"></a>Test amaçları

Site Recovery kararlı durum çoğaltma sırasında nasıl gerçekleştireceğini incelemek için sınama amacı oluştu.

- Kararlı durum çoğaltma VM'ler başlangıç çoğaltmasını tamamlamış olmanız ve delta değişiklikler eşitleniyor oluşur.
- Beklenmeyen kesintiler sürece kararlı durum kullanarak performansını ölçmek, hangi çoğu VM'ler durumda olduğu için kalır, önemlidir.
- Her sitede bir VMM sunucusuyla iki şirket içi sitenin sınama dağıtımı içermektedir. Bu test dağıtımını birincil site olarak işlev gören Genel merkez ve şube ofis ikincil veya kurtarma sitesi olarak baş office/şube office dağıtımını normaldir.

## <a name="what-we-did"></a>Ne yaptığımız

Ne biz testte başarılı olmadı aşağıda verilmiştir:

1. VMM şablonları kullanarak oluşturulan VM'ler.
2. Sanal makineleri başlatıldı ve temel performans ölçümlerini üzerinde 12 saat yakalanan.
3. Oluşturulan bulut birincil ve kurtarma VMM sunucuları.
4. Site Recovery, kaynak ve kurtarma arasında bulut eşleme dahil olmak üzere yapılandırılmış çoğaltmasında.
5. VM'ler için korumayı ve ilk çoğaltmayı tamamlamasını izin.
6. Birkaç saat sistem sabitlemeyi beklendi.
7. Performans ölçümleri üzerinde 12 burada tüm sanal makineleri bu 12 saat için beklenen çoğaltma durumunda kalan saat, yakalanan.
8. Delta temel performans ölçümlerini ve çoğaltma performans ölçümleri arasında ölçülür.


## <a name="primary-server-performance"></a>Birincil sunucu performansı

* Hyper-V (Site Recovery tarafından kullanılır) çoğaltma zaman uyumsuz olarak, birincil sunucu üzerindeki ek yükü en düşük depolama ile bir günlük dosyasına değişiklikleri izler.
* Hyper-V çoğaltma IOPS yükü izleme için en aza indirmek için otomatik olarak tutulan önbellek kullanır. Depoladığı bellekte VHDX yazar ve bunları günlük kurtarma siteye gönderilir süreden önce günlük dosyasına kaydeder. Yazma önceden belirlenmiş bir sınırına bir disk temizleme de olur.
* Grafik çoğaltma kararlı durum IOPS yükü gösterir. Çoğaltma ek yükü nedeniyle IOPS yaklaşık % 5, oldukça düşük olduğu olduğunu görebiliriz.

  ![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V çoğaltma, disk performansı iyileştirmek için birincil sunucuda bellek kullanır. Aşağıdaki grafikte gösterildiği gibi birincil kümedeki tüm sunucuların Bellek Yükü Marjinal olur. Ek yükü gösterilen Hyper-V sunucusunda yüklü toplam bellek ile karşılaştırıldığında, çoğaltma tarafından kullanılan bellek yüzdesi bellektir.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V çoğaltma minimum CPU yüke sahiptir. Grafikte gösterildiği gibi çoğaltma ek yükünü % 2-3 aralığında olur.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>İkincil sunucu performansı

Hyper-V çoğaltma depolama işlemlerinin sayısını en iyi duruma getirmek için kurtarma sunucusunda az miktarda bellek kullanır. Kurtarma sunucusunda bellek kullanımı grafiği özetler. Ek yükü gösterilen Hyper-V sunucusunda yüklü toplam bellek ile karşılaştırıldığında, çoğaltma tarafından kullanılan bellek yüzdesi bellektir.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744916.png)

Kurtarma sitesinde g/ç işlemleri miktarı, birincil site yazma işlemlerinin sayısı bir işlevdir. Şimdi toplam g/ç işlemlerinin toplam g/ç işlemleri karşılaştırıldığında kurtarma sitesinde bakın ve yazma işlemlerini birincil sitede. Grafikleri toplam IOPS kurtarma sitesinde olmadığını gösterir

* Yaklaşık 1,5 katı yazma IOPS birincil.
* Birincil sitede toplam IOPS yaklaşık % 37'si.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744917.png)

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Ağ kullanımı etkisi

275 MB ağ bant genişliğinin saniyedeki ortalama, birincil ve kurtarma düğümleri arasında (sıkıştırma), bir var olan 5 Gb bant genişliği saniye başına karşı kullanıldı.

![Sonuçlar ağı kullanımı](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM performans üzerindeki etkisi

Önemli bir çoğaltma sanal makinelerde çalışan üretim iş yükleri üzerindeki etkisini konudur. Birincil site çoğaltma için yeterli sağlandığında, iş yükleri üzerinde hiçbir etkisi olması döndürmemelidir. Hyper-V çoğaltma'nın basit mekanizması izleme sanal makinelerde çalışan iş yükleri kararlı durum çoğaltma sırasında etkilenmez sağlar. Bu aşağıdaki grafiklerde gösterilmiştir.

Bu grafik IOPS farklı iş yükleri, önce çalışan sanal makineler tarafından ve çoğaltma etkinleştirildikten sonra gerçekleştirilen gösterir. İkisi arasındaki fark olduğunu görebilirsiniz.

![Çoğaltma efekti sonuçları](./media/hyper-v-vmm-performance-results/IC744920.png)

Aşağıdaki grafikte önce farklı iş yüklerini çalıştıran sanal makinelerin ve çoğaltma etkinleştirildikten sonra işleme gösterir. Çoğaltmanın önemli bir etkisi yoktur görebilirsiniz.

![Sonuçlar çoğaltma etkileri](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Sonuç

Site Recovery, Hyper-V çoğaltma ile birlikte ek yükü büyük bir küme için en az ile iyi ölçeklenir sonuçları açıkça gösterir. Site Recovery, basit dağıtım, çoğaltma, yönetim ve izleme sağlar. Hyper-V çoğaltma, başarılı çoğaltma ölçekleme için gerekli altyapıyı sağlar. 

## <a name="test-environment-details"></a>Sınama ortamı ayrıntıları

### <a name="primary-site"></a>Birincil site

* Birincil site 470 sanal makineleri çalıştıran beş Hyper-V sunucuları içeren bir küme var.
* Farklı iş yüklerini sanal makineleri çalıştırmak ve tüm Site Recovery koruması etkin sahip.
* Depolama küme düğümü için iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.
* Her küme sunucusu bir Gbps dört ağ kartı (NIC) sahiptir.
* Bir iSCSI özel ağına bağlı iki ağ kartları ve iki bir dış Kurumsal ağa bağlanır. Dış ağlara birini yalnızca küme iletişimi için ayrılmıştır.

![Birincil donanım gereksinimleri](./media/hyper-v-vmm-performance-results/IC744922.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümede Hyper-V sunucuları: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |256 128ESTLAB HOST25 sahip |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 @ 2.20GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server 2012 veritabanı R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-site"></a>İkincil site

* İkincil sitenin altı düğümlü yük devretme kümesi vardır.
* Depolama küme düğümü için iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.

![Birincil donanım belirtimi](./media/hyper-v-vmm-performance-results/IC744923.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümede Hyper-V sunucuları: <br />ESTLAB-HOST07<br />ESTLAB HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel(r) Xeon(R) CPU E5-2630 0 @ 2.30GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST17 |128 |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 @ 2.20GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST24 |256 |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 @ 2.20GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server 2012 veritabanı R2 (x 64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Sunucu iş yükleri

* Test amaçları için Kurumsal müşteri senaryolarda yaygın olarak kullanılan iş yükleri Çekildi.
* Kullanırız [IOMeter](http://www.iometer.org) benzetimi için tablodaki özetlenen iş yükü özelliği ile.
* Tüm IOMeter profilleri modellerini iş yükleri için en kötü durum benzetmek için rastgele bayt yazma yazmak için ayarlanır.

| İş yükü | G/ç boyutu (KB) | % Erişim | %Read | Bekleyen g/ç | G/ç düzeni |
| --- | --- | --- | --- | --- | --- |
| Dosya Sunucusu |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |% Rastgele tüm 100 |
| SQL Server (birim 1) SQL Server (2 birimi) |864 |100%100% |70%0% |88 |% 100 random100% sıralı |
| Exchange |32 |100% |67% |8 |% 100 rastgele |
| İş istasyonu/VDI |464 |66%34% |70%95% |11 |Her iki % 100 rastgele |
| Web dosya sunucusu |4864 |33%34%33% |95%95%95% |888 |Tüm %75 rastgele |

### <a name="vm-configuration"></a>VM yapılandırması

* Birincil kümedeki sanal makinelerin 470.
* Tüm sanal makineleri VHDX disk ile.
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

* Site Recovery, şirket içinden şirket içine koruma için yapılandırıldı
* VMM sunucusu, Hyper-V Küme sunucularını ve Vm'leri içeren yapılandırılan, dört Bulutlar sahip.

| Birincil VMM Bulutu | Korumalı VM'ler | Çoğaltma sıklığı | Ek kurtarma noktaları |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 dakika |None |
| PrimaryCloudRpo30s |47 |30 saniye |Hiçbiri |
| PrimaryCloudRpo30sArp1 |47 |30 saniye |1 |
| PrimaryCloudRpo5m |235 |5 dakika |Hiçbiri |

### <a name="performance-metrics"></a>Performans ölçümleri

Tablo performans ölçümleri ve dağıtımında ölçülen sayaçları özetler.

| Ölçüm | Sayaç |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Kullanılabilir bellek |\Memory\Available MBayt |
| IOPS |\PhysicalDisk (_Total) \Disk aktarımı/sn |
| VM okuma (IOPS) işlemleri/sn |\Hyper-V sanal depolama aygıtı (<VHD>) \Read işlemi/sn |
| VM yazma (IOPS) işlemi/sn |\Hyper-V sanal depolama aygıtı (<VHD>) \Write Operations/S |
| VM üretilen işi okuma |\Hyper-V sanal depolama aygıtı (<VHD>) \Read bayt/sn |
| VM yazma üretimi |\Hyper-V sanal depolama aygıtı (<VHD>) \Write bayt/sn |

## <a name="next-steps"></a>Sonraki adımlar

[Çoğaltmayı ayarlama](hyper-v-vmm-disaster-recovery.md)
