---
title: "Test sonuçları Azure Site Recovery ile siteler arasında Hyper-V çoğaltma için | Microsoft Docs"
description: "Bu makale, Azure Site RECOVERY'yi kullanarak şirket içi çoğaltma, Hyper-V sanal makineleri için performans testleri hakkında bilgi sağlar."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: f25bbca86fdbb480a4db7623d4ee8d296415a4be
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="test-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Şirket içi Site Recovery ile şirket içi Hyper-V çoğaltma için test sonuçları

Microsoft Azure Site Recovery, düzenlemek ve sanal makineleri ve fiziksel sunucuların Azure'a veya ikincil veri merkezine çoğaltmayı yönetmek için kullanabilirsiniz. Bu makale, Hyper-V sanal makineleri iki arasında çoğaltma şirket içi veri merkezi zaman yaptığımız performansı test sonuçlarını sağlar.

## <a name="test-goals"></a>Test amaçları

Azure Site Recovery kararlı durum çoğaltma sırasında nasıl gerçekleştireceğini incelemek için sınama amacı oluştu. Sanal makinelerin başlangıç çoğaltmasını tamamlamış olmanız ve delta değişiklikler eşitleniyor kararlı durum çoğaltma oluşur. Beklenmeyen kesintiler sürece, çoğu sanal makine kalması durumunda olduğundan kararlı durum kullanarak performansını ölçmek önemlidir.

İki şirket içi sitenin her sitedeki VMM sunucusu ile test dağıtımını içermektedir. Bu test dağıtımını birincil site ve şube ofis ikincil veya kurtarma sitesi olarak davranan merkez ofis ile baş office/şube office dağıtımını normaldir.

## <a name="what-we-did"></a>Ne yaptığımız

Ne biz testte başarılı olmadı aşağıda verilmiştir:

1. VMM şablonları kullanarak sanal makineleri oluşturulur.
2. Sanal makineler ve yakalama temel performans ölçümlerini üzerinde 12 saat başlatıldı.
3. Birincil ve kurtarma VMM sunucularında oluşturulan bulut.
4. Azure Site kurtarma, kaynak ve kurtarma bulut eşleme dahil olmak üzere yapılandırılmış bulut koruma.
5. Sanal makineler için koruma etkin ve ilk çoğaltmayı tamamlamasını sağlar.
6. Birkaç saat sistem sabitlemeyi beklendi.
7. Performans ölçümleri, tüm sanal makineler için bu 12 saat içinde beklenen çoğaltma durumunda kalan sağlama üzerinde 12 saat yakalandı.
8. Temel performans ölçümlerini ve çoğaltma performans ölçümleri arasındaki delta ölçün.


## <a name="primary-server-performance"></a>Birincil sunucu performansı

* Hyper-V çoğaltma zaman uyumsuz olarak değişiklikleri minimum depolama ek yükü ile bir günlük dosyasına birincil sunucuda izler.
* Hyper-V çoğaltma IOPS yükü izleme için en aza indirmek için otomatik olarak tutulan önbellek kullanır. Depoladığı bellekte VHDX yazar ve bunları günlük kurtarma siteye gönderilir süreden önce günlük dosyasına kaydeder. Yazma önceden belirlenmiş bir sınırına bir disk temizleme de olur.
* Grafik çoğaltma kararlı durum IOPS yükü gösterir. Çoğaltma ek yükü nedeniyle IOPS yaklaşık oldukça düşük olan %5 olduğunu görebiliriz.

![Birincil sonuçlar](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V çoğaltma disk performansı iyileştirmek için birincil sunucuda bellek kullanır. Aşağıdaki grafikte gösterildiği gibi birincil kümedeki tüm sunucuların Bellek Yükü Marjinal olur. Ek yükü gösterilen Hyper-V sunucusunda yüklü toplam bellek karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesi bellektir.

![Birincil sonuçlar](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V çoğaltma minimum CPU yüke sahiptir. Grafikte gösterildiği gibi çoğaltma ek yükünü % 2-3 aralığında olur.

![Birincil sonuçlar](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

## <a name="secondary-recovery-server-performance"></a>İkincil (Kurtarma) sunucusu performansı

Hyper-V çoğaltma depolama işlemlerinin sayısını en iyi duruma getirmek için kurtarma sunucusunda az miktarda bellek kullanır. Kurtarma sunucusunda bellek kullanımı grafiği özetler. Ek yükü gösterilen Hyper-V sunucusunda yüklü toplam bellek karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesi bellektir.

![İkincil sonuçlar](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Kurtarma sitesinde g/ç işlemleri miktarı, birincil site yazma işlemlerinin sayısı bir işlevdir. Şimdi toplam g/ç işlemlerinin toplam g/ç işlemleri karşılaştırıldığında kurtarma sitesinde bakın ve yazma işlemlerini birincil sitede. Grafikleri toplam IOPS kurtarma sitesinde olmadığını gösterir

* Yaklaşık 1,5 katı yazma IOPS birincil.
* Birincil sitede toplam IOPS yaklaşık % 37'si.

![İkincil sonuçlar](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![İkincil sonuçlar](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

## <a name="effect-on-network-utilization"></a>Ağ kullanımı etkisi

Ağ bant genişliğinin Saniyedeki 275 Mb ortalama bir var olan 5 Gb bant genişliği saniye başına karşı birincil ve kurtarma düğümleri arasında (sıkıştırma) kullanıldı.

![Sonuçlar ağı kullanımı](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM performans üzerindeki etkisi

Önemli bir çoğaltma sanal makinelerde çalışan üretim iş yükleri üzerindeki etkisini konudur. Birincil site çoğaltma için yeterli sağlandığında, iş yükleri üzerinde hiçbir etkisi olması döndürmemelidir. Hyper-V çoğaltma'nın basit mekanizması izleme sanal makinelerde çalışan iş yükleri kararlı durum çoğaltma sırasında etkilenmez sağlar. Bu aşağıdaki grafiklerde gösterilmiştir.

Bu grafik, farklı iş yükleri önce çalışan sanal makineler tarafından ve çoğaltma etkinleştirildikten sonra gerçekleştirilen IOPS gösterir. İkisi arasındaki fark olduğunu görebilirsiniz.

![Çoğaltma efekti sonuçları](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Aşağıdaki grafikte farklı iş yükleri önce çalışan sanal makineler ve çoğaltma etkinleştirildikten sonra işleme gösterir. Çoğaltmanın önemli bir etkisi yoktur görebilirsiniz.

![Sonuçlar çoğaltma etkileri](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

## <a name="conclusion"></a>Sonuç

Azure Site Recovery, Hyper-V çoğaltma ile birlikte ek yükü büyük bir küme için en az ile iyi ölçeklenir sonuçları açıkça gösterir.  Azure Site Recovery, basit dağıtım, çoğaltma, yönetim ve izleme sağlar. Hyper-V çoğaltma, başarılı çoğaltma ölçekleme için gerekli altyapıyı sağlar. En iyi dağıtım planlama için karşıdan önerdiğimiz [Hyper-V çoğaltma kapasite Planlayıcısı](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Sınama ortamı ayrıntıları

### <a name="primary-site"></a>Birincil site

* Birincil site 470 sanal makineleri çalıştıran beş Hyper-V sunucuları içeren bir küme var.
* Farklı iş yükleri sanal makineleri çalıştırmak ve tüm Azure Site Recovery koruması etkin sahip.
* Depolama küme düğümü için iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.
* Her küme sunucusu bir Gbps dört ağ kartı (NIC) sahiptir.
* Bir iSCSI özel ağına bağlı iki ağ kartları ve iki bir dış Kurumsal ağa bağlanır. Dış ağlara birini yalnızca küme iletişimi için ayrılmıştır.

![Birincil donanım gereksinimleri](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümede Hyper-V sunucuları: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |256 128ESTLAB HOST25 sahip |Dell ™ PowerEdge ™ R820 |Intel(r) Xeon(R) CPU E5-4620 0 @ 2.20GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server 2012 veritabanı R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-recovery-site"></a>İkincil (Kurtarma) sitesi

* İkincil sitenin altı düğümlü yük devretme kümesi vardır.
* Depolama küme düğümü için iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.

![Birincil donanım belirtimi](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

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

* Birincil kümesinde 470 sanal makineler.
* Tüm sanal makinelerle VHDX disk.
* Tabloda özetlenen iş yüklerini çalıştıran sanal makineler. Tüm VMM şablonları ile oluşturulmuş.

| İş yükü | # VM'ler | Minimum RAM (GB) | En fazla RAM (GB) | VM başına mantıksal disk boyutu (GB) | Maksimum IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Dosya Sunucusu |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Web sunucusu |149 |.5 |1 |80 |6 |
| TOPLAM |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery ayarları

* Azure Site Recovery, şirket içinden şirket içine koruma için yapılandırıldı
* VMM sunucusu, Hyper-V küme sunucuları ve sanal makinelerinin içeren yapılandırılan, dört Bulutlar sahip.

| Birincil VMM Bulutu | Buluttaki korunan sanal makineler | Çoğaltma sıklığı | Ek kurtarma noktaları |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 dakika |Hiçbiri |
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

[İki şirket içi VMM siteler arasında çoğaltmayı ayarlama](site-recovery-vmm-to-vmm.md)
