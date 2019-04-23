---
title: Azure Lsv2 serisi sanal makineler - depolama performansı en iyi duruma getirme | Microsoft Docs
description: Çözümünüzün Lsv2 serisi sanal makinelerde performansını nasıl iyileştirebileceğinizi öğrenin.
services: virtual-machines-linux
author: laurenhughes
manager: jeconnoc
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: joelpell
ms.openlocfilehash: 7be86c8934b8766217f9fca432327d254204f0c4
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014552"
---
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>Lsv2 serisi sanal makinelerde performansını iyileştirme

Lsv2 serisi sanal makineler, çok çeşitli uygulamalar ve sektörler arasında yüksek g/ç ve yerel depolama aktarım hızı gereken iş yükleri çeşitli destekler.  Lsv2 serisi, büyük veri, SQL, NoSQL veritabanları, veri ambarlama ve Cassandra, MongoDB, Cloudera dahil olmak üzere büyük işlem veritabanları için idealdir ve Redis.

Tasarım Lsv2 serisi sanal makinelerin (VM'ler) işlemci, bellek, NVMe cihazları ve VM'ler arasında en iyi performansı sağlamak için AMD EPYC™ 7551 işlemci en üst düzeye çıkarır. Donanım performansı en üst düzeye ek olarak Lsv2 serisi VM'ler, Linux işletim sistemlerinin gereksinimleri için donanım ve yazılım ile daha iyi performans ile çalışacak şekilde tasarlanmıştır.

Yazılım ve donanım ayarlama, en iyi duruma getirilmiş sürümünde sonuçlandı [Canonical Ubuntu 18.04 ve 16.04](https://azuremarketplace.microsoft.com/marketplace/apps/Canonical.UbuntuServer?tab=Overview), içinde NVMe cihazlarda en yüksek performansı destekleyen Azure marketi, erken aralık 2018'de yayınlandı Lsv2 serisi VM'ler.

Bu makalede ipuçları ve iş yüklerini ve uygulamaları sağlamak için öneriler Vm'lere tasarlanan en yüksek performansı elde edin. Daha fazla en iyi duruma getirilmiş Lsv2 görüntüleri Azure Marketi'nde eklendikçe bu sayfadaki bilgiler sürekli olarak güncelleştirilir.

## <a name="amd-eypc-chipset-architecture"></a>AMD EYPC™ yonga mimarisi

Lsv2 serisi VM'ler üzerinde Zen mikro mimarisi tabanlı AMD EYPC™ server işlemcileri kullanın. AMD geliştirilen sonsuz doku (varsa) için EYPC™ olarak ölçeklenebilir-zar-package ve birden çok paket iletişimleri için kullanılabilecek kendi NUMA modeli için bağlantı. QPI'yi (hızlı yolu bağlantı) ve Intel modern zar tek parça işlemciler üzerinde kullanılan bir UPI (Ultra yolu bağlantı) ile karşılaştırıldığında, AMD'ın çok NUMA küçük zar mimarisi her iki performans avantajlarının Getir zorluklarının yanı sıra. Gerçek bellek bant genişliği ve gecikme kısıtlamaları etkisini çalışan iş yüklerini türüne bağlı olarak değişir.

## <a name="tips-to-maximize-performance"></a>Performansı en üst düzeye çıkarmak için ipuçları

* İş yükünüz için özel bir Linux GuestOS yüklüyorsanız, hızlandırılmış ağ unutmayın **OFF** varsayılan olarak. Hızlandırılmış Ağ'ı etkinleştirmek istiyorsanız, en iyi performans için VM oluşturma zamanında etkinleştirin.

* Lsv2 serisi VM'ler destekleyen donanım sekiz g/ç sırası çiftleri (QP) s ile NVMe cihazları kullanır. Tüm NVMe aygıt g/ç sırası aslında bir çiftidir: gönderme sırası ve bir tamamlanma sırası. NVMe sürücü dağıtarak ben bu sekiz g/ç QPs kullanımını iyileştirmek için ayarlanmış / Ç'lerde hepsini bir kez deneme olarak zamanlayın. En yüksek performans sağlamak için eşleştirmek için cihaz başına sekiz iş çalıştırın.

* NVMe yönetici komutlarını (örneğin, NVMe akıllı bilgisi sorgu, vb.) NVMe g/ç komutlarla etkin iş yükleri sırasında karıştırmayın. Lsv2 NVMe cihazları herhangi bir NVMe yönetici komut Beklemede olduğunda, "yavaş moduna" anahtarları Hyper-V NVMe doğrudan teknoloji tarafından desteklenir. Lsv2 kullanıcılar bu durumda NVMe g/ç performansı bırak çarpıcı performansın arttığını görürsünüz.

* Lsv2 kullanıcılar, NUMA benzeşimi uygulamalarını için karar vermek için veri sürücüleri için VM ve raporlanan cihaz NUMA bilgileri (tüm 0) dayanarak doğrulamamalısınız. Daha iyi performans için önerilen yol, iş yüklerini CPU'lar arasında mümkünse yayılan sağlamaktır.

* 1024 (vs. desteklenen en büyük sıra derinliğidir Lsv2 VM NVMe cihazın g/ç sırası çifti başına Amazon ı3 QD 32 sınırı). Lsv2 kullanıcılar 1024 veya daha düşük performansı düşürebilir sırası tam koşullar tetikleme önlemek için sıra derinliği (Sentetik) Kıyaslama iş yüklerinin sınırlamanız gerekir.

## <a name="utilizing-local-nvme-storage"></a>Yerel NVMe depolama kullanma

Yerel 1.92 TB NVMe disk tüm Lsv2 VM'ler üzerinde geçici depolamadır. Başarılı standart yeniden başlatma VM'nin sırasında yerel NVMe diskteki veriler korunur. VM yeniden, edilemez veya silinmesi durumunda veri NVMe kalıcı olmaz. VM veya sistem durumu için üzerinde çalıştığı donanım başka bir soruna neden olursa veri kalıcı olmaz. Bu durumda, eski ana bilgisayardaki tüm veriler güvenli bir şekilde silinir.

Ayrıca durumlar olacaktır bir planlı bakım işlemi sırasında farklı bir konak makinesi için örneğin, taşınacak VM ihtiyacı olduğunda. Planlı bakım işlemleri ve bazı donanım hataları beklenen ile [zamanlanmış olaylar](scheduled-events.md). Zamanlanmış olaylar, herhangi bir tahmini Bakım ve kurtarma işlemleri için güncelleştirilmiş kalmak için kullanılmalıdır.

Boş yerel disk içeren yeni bir konağa yeniden oluşturulması VM planlanan bakım olayı gerektiren durumlarda verileri (yeniden verilerle güvenli bir şekilde silinebilir eski konaktaki) eşitlenmesi gerekir. Bu durum, Lsv2 serisi VM'ler dinamik geçişi şu anda yerel NVMe disk üzerindeki desteklemeyen kaynaklanır.

Planlı bakım için iki mod vardır.

### <a name="standard-vm-customer-controlled-maintenance"></a>Standart VM müşteri tarafından denetlenen bakım

- VM, 30 günlük penceresi sırasında güncelleştirilmiş bir ana bilgisayara taşınır.
- Olay öncesinde yedeklenirken verilerin önerilen şekilde Lsv2 yerel depolama veri kaybı, olabilir.

### <a name="automatic-maintenance"></a>Otomatik bakım

- Müşteri, müşteri tarafından denetlenen bakım yürütülemezse veya Acil durum yordamları güvenlik sıfır gün olay gibi olması durumunda gerçekleşir.
- Müşteri verilerini korumak kullanılmaya ancak küçük bir VM dondurma veya yeniden başlatma riski yoktur.
- Olay öncesinde yedeklenirken verilerin önerilen şekilde Lsv2 yerel depolama veri kaybı, olabilir.

Herhangi bir gelecek hizmet olayı için denetlenen bakım işlemi güncelleştirmesi sizin için en uygun bir zaman seçmek için kullanın. Olay önce premium depolama, verilerinizi yedekleyebilirsiniz. Bir bakım olayı tamamlandıktan sonra verilerinizi yenilenmiş Lsv2 Vm'leri yerel NVMe depolama birimine geri dönebilirsiniz.

Yerel NVMe disk üzerindeki verileri korumak senaryolar şunlardır:

- VM çalışır ve iyi durumda.
- VM (veya Azure tarafından) yerinde yeniden.
- VM (ayırmayı kaldırma olmadan durduruldu) duraklatıldı.
- Çoğu bakım işlemi planlı Bakım.

Güvenli bir şekilde müşteri korunacak verileri silme senaryolar şunlardır:

- VM yeniden, durduruldu (serbest bırakıldı), veya (sizin tarafınızdan) silindi.
- VM kötüleşir ve hizmete sahip başka bir düğüme bir donanım sorunu nedeniyle onarımı.
- Planlı bakım işlemleri bakım az sayıda sanal Makinenin bakım için başka bir konağa ayrılabilecek gerektirir.

Yerel depolama alanındaki verileri yedekleme seçenekleri hakkında daha fazla bilgi için bkz. [Azure Iaas diskler için yedekleme ve olağanüstü durum kurtarma](backup-and-disaster-recovery-for-azure-iaas-disks.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

* **Lsv2 serisi Vm'leri dağıtma nasıl başlarım?**  
   Diğer herhangi bir VM gibi çok kullanma [portalı](quick-create-portal.md), [Azure CLI](quick-create-cli.md), veya [PowerShell](quick-create-powershell.md) bir VM oluşturmak için.

* **Tek NVMe disk hatası tüm VM'lerin ana bilgisayarda başarısız olmasına neden olur?**  
   Bir disk arızası donanım düğümde algılanırsa, donanım bir hatalı durumda. Bu durumda, düğümdeki tüm VM'ler otomatik olarak serbest ve sağlam bir düğüme taşınır. Lsv2 serisi VM'ler için başarısız olan düğümdeki müşteri verileri de güvenli bir şekilde silinir ve yeni düğüm üzerinde müşteri tarafından oluşturulması gerekir bu anlamına gelir. Bunlar başka bir düğüme aktarılır gibi dinamik geçiş Lsv2 üzerinde kullanılabilir olmadan önce belirtildiği gibi, başarısız olan düğümde verileri proaktif bir şekilde de Vm'lerle taşınır.

* **Tüm performans rq_affinity ayarlamalar gerekiyor mu?**  
   Rq_affinity küçük bir ayarlama mutlak en büyük giriş/çıkış işlemi (IOPS) saniyede kullanırken ayarıdır. Diğer her şey iyi çalışmaya başladığında, rq_affinity fark aktarılmayacağını görmek için 0 olarak ayarlayın. daha sonra deneyin.

* **Blk_mq ayarları değiştirmeniz gerekiyor mu?**  
   RHEL/CentOS 7.x NVMe cihazları için otomatik olarak blk mq kullanır. Hiçbir yapılandırma değişikliği veya ayarları gereklidir. Scsi_mod.use_blk_mq ayarı için SCSI amaçlıdır ve NVMe cihazları Konuk sanal SCSI cihazları olarak görünür çünkü Lsv2 Önizleme sırasında kullanıldı. Şu anda, ilgisiz SCSI blk mq ayarı, bu nedenle NVMe cihazları NVMe cihazları olarak görülebilir.

* **"Fio" değiştirmek gerekiyor mu?**  
   Maksimum IOPS ölçü aracı L64v2 ve L80v2 VM boyutları 'fio' gibi bir performans elde etmek için "rq_affinity" her NVMe cihazda 0 olarak ayarlayın.  Örneğin, bu komut satırı 10 tüm NVMe cihazları için sıfır "rq_affinity" L80v2 VM'deki ayarlanır:

   ```console
   for i in `seq 0 9`; do echo 0 >/sys/block/nvme${i}n1/queue/rq_affinity; done
   ```

   Ayrıca, her biri ile yok, hiçbir dosya sistemleri, hiçbir RAID bölümleme ham NVMe cihazları için doğrudan g/ç yapılır zaman 0 yapılandırma, vb. en iyi performansı elde edilen unutmayın. Test oturumu başlatmadan önce yapılandırma çalıştırarak bilinen bir yeni ve temiz durumda olduğundan emin olun `blkdiscard` her NVMe cihazları.
   
## <a name="next-steps"></a>Sonraki adımlar

* Tüm özelliklerini görmek [Vm'leri için depolama performansını en iyi duruma getirilmiş](sizes-storage.md) azure'da
