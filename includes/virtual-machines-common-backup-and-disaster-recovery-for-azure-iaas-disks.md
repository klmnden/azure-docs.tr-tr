---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: d242b2815d59676432beb878bbc955a9f39de0f1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188368"
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Azure Iaas diskler için yedekleme ve olağanüstü durum kurtarma

Bu makalede, yedekleme ve olağanüstü durum kurtarma (DR Iaas sanal makineleri (VM'ler) ve Azure disklerinin) planlama açıklanmaktadır. Bu belge, yönetilen ve yönetilmeyen diskler kapsar.

İlk olarak, yerel arızalarına karşı korumanıza yardımcı olur Azure platformundaki yerleşik hataya dayanıklılık özelliklerini kapsar. Ardından tamamen yerleşik özellikleri tarafından kapsanan olağanüstü durum senaryoları ele alır. Ayrıca iş yükü senaryoları farklı yedekleme ve DR dikkat edilecek noktalar burada uygulayabileceğiniz çeşitli örneklerini göstereceğiz. Biz sonra DR, Iaas diskleri için olası çözümleri gözden geçirin.

## <a name="introduction"></a>Giriş

Azure platformu, müşterilerin yerelleştirilmiş donanım hatalarına karşı korumaya yardımcı olmak için yedeklik ve hataya dayanıklılık için çeşitli yöntemler kullanır. Bu sunucu üzerindeki bir sanal disk için verilerin bir kısmını ya da bir SSD veya HDD hatalarının depolayan bir Azure depolama sunucusu makinesindeki sorunlar yerel hataları içerebilir. Normal işlemler sırasında gibi yalıtılmış bir donanım bileşeni hataları oluşabilir.

Azure platformu, bu hatalara karşı dayanıklı olacak şekilde tasarlanmıştır. Önemli bir olağanüstü hataları veya birçok depolama sunucusu veya bile tüm veri merkezi inaccessibility neden olabilir. Yerelleştirilmiş hatalardan normalde korunan VM'ler ve diskleri de iş yükünüz gibi VM ve diskleri etkileyen önemli bir olağanüstü durum, bölge çapında yıkıcı arızalarına karşı korumak ek adımlar gereklidir.

Platform hataları olasılığını ek olarak, bir müşteri uygulama veya veri ile ilgili sorunlar oluşabilir. Örneğin, uygulamanızın yeni bir sürümünü yanlışlıkla ayırmak neden verilere değişiklik. Bu durumda, uygulama ve veri bilinen son iyi duruma içeren bir önceki sürüme geri almak isteyebilirsiniz. Bu, düzenli yedeklemeler bakım gerektirir.

Bölgesel bir olağanüstü durum kurtarma için farklı bir bölgeye Iaas sanal makine disklerinizi yedeklemeniz gerekir.

Şimdi yedekleme ve kurtarma seçenekleri baktığımızda önce yerelleştirilmiş hata işleme için birkaç yöntem kullanılabilir bilgilerin üzerinden geçelim.

### <a name="azure-iaas-resiliency"></a>Azure Iaas dayanıklılık

*Dayanıklılık* donanım bileşenlerinin oluşan normal hata toleransı ifade eder. Dayanıklılık, hatalardan kurtularak çalışmaya devam yeteneğidir. Bu hataları önleme, ancak kapalı kalma süresi veya veri kaybı engelleyen bir yolla hataları ele alma hakkında değil. Dayanıklılığın hedefi, bir hatanın ardından uygulamayı tam çalışır duruma geri döndürmektir. Azure sanal makineleri ve diskleri için yaygın donanım hatalarına dayanıklı olacak şekilde tasarlanmıştır. Azure Iaas platform bu dayanıklılık nasıl sağladığını adresindeki bakalım.

Bir sanal makine ağırlıklı olarak iki bölümden oluşur: bir işlem sunucusu ve kalıcı diskler. Her ikisi de bir sanal makine hataya dayanıklılık etkiler.

Sanal makinenizin barındırıldığı Azure işlem ana bilgisayar sunucusu nadir olarak rastlanıyor bir donanım hatası oluşursa Azure VM'yi başka bir sunucuya otomatik olarak geri yüklemek için tasarlanmıştır. Bu senaryo, bilgisayar yeniden başlatılmadan ve VM göründüğünde, geri süre sonra. Azure, otomatik olarak gibi donanım hataları algılar ve VM müşteri olabildiğince çabuk kullanılabilir sağlamaya yardımcı olmak için kurtarma işlemleri yürütür.

Iaas diskleri ile ilgili veri kalıcı depolama platformu kritik önemlidir. Azure müşterileri Iaas üzerinde çalışan iş önemli uygulamalara sahiptir ve bunlar veri kalıcılığı bağlıdır. Yerel olarak depolanan verilerin üç yedekli kopyalar ile bu Iaas diskleri için Azure tasarımları koruma. Bu kopyalar, yerel arızalarına karşı yüksek bir dayanıklılık düzeyi sağlayın. Diskinizin tutan donanım bileşenlerinden biri başarısız olursa, disk isteklerini desteklemek için iki ek kopyalarını olduğundan, sanal Makinenizin etkilenmez. Bu durum, ince, bir disk Destek birimine iki farklı donanım bileşenlerini (Bu nadir görülen bir durumdur) aynı anda başarısız olsa bile çalışır. 

Her zaman üç kopyaya korumak, üç birini kopyalar, Azure depolama veri arka planda yeni bir kopyasını otomatik olarak çoğaltılır emin olmak için kullanılabilir hale gelir. Bu nedenle, RAID için hataya dayanıklılık ile Azure disklerini kullanmak için gerekli olmamalıdır. Basit bir RAID 0 yapılandırma gerekirse, daha büyük birimleri oluşturmak diskleri bölmek için yeterli olmalıdır.

Bu mimari nedeniyle Azure Kurumsal düzeyde tutarlı bir şekilde teslim dayanıklılık düzeyi sunmak için Iaas diskleri, endüstri lideri ile sıfır yüzde [değer yıllık hata oranı](https://en.wikipedia.org/wiki/Annualized_failure_rate).

Yerelleştirilmiş donanım hatalarını işlemde barındırmak veya depolama platform bazen sonuçlanır kapsamına giren sanal makinenin geçici olarak kullanım dışı kalması için [Azure SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/) VM kullanılabilirlik. Azure, Azure premium SSD kullanan tek sanal makine örnekleri için de bir sektör lideri SLA sağlar.

Bir disk veya sanal makine geçici olarak kullanım dışı kalması nedeniyle kapalı kalma süresi'nden uygulama iş yüklerini korumak için müşteriler kullanabilir [kullanılabilirlik kümeleri](../articles/virtual-machines/windows/manage-availability.md). Bir kullanılabilirlik kümesinde iki veya daha fazla sanal makineyi yedeklilik için uygulama sağlar. Azure daha sonra bu Vm'leri ve diskleri farklı güç, ağ ve sunucu bileşenleri ile ayrı hata etki alanları oluşturur.

Bu ayrı hata etki alanları nedeniyle yerel donanım hatalarına genellikle kümedeki birden çok sanal makineler aynı anda etkilemez. Ayrı hata etki alanlarına sahip, uygulamanız için yüksek kullanılabilirlik sağlar. Bu, yüksek kullanılabilirlik gerektiğinde kullanılabilirlik kümelerini kullanmak için iyi bir uygulama dikkate almıştır. Sonraki bölümde, olağanüstü durum kurtarma yönlerini kapsar.

### <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Olağanüstü durum kurtarma nadir ancak ana, olaylar yeteneğidir. Bu olaylar, tüm bir bölgeyi etkileyen bir hizmet kesintisi gibi geçici olmayan ve geniş ölçekli hataları içerir. Olağanüstü durum kurtarma, veri yedekleme ve arşivleme dahildir ve bir veritabanını bir yedekten geri yükleme gibi el ile müdahaleleri de içerebilir.

Büyük ölçekli kesintiler önemli bir olağanüstü durum neden olursa Azure platformun yerelleştirilmiş arızalarına karşı yerleşik koruma VM'ler/diskleri tam olarak korumaz. Bu büyük ölçekli kesintiler gibi yıkıcı olaylara içeren bir veri merkezine ulaşılırsa kasırga, deprem, Ateş tarafından veya büyük ölçekli donanım birim hatası olması durumunda. Ayrıca, uygulama veya veri sorunları nedeniyle hataları karşılaşabilirsiniz.

Iaas iş yüklerinizi kesintilere karşı korumaya yardımcı olmak için yedeklilik için planlama ve kurtarmayı etkinleştirmek üzere yedeklemelere sahip. Olağanüstü durum kurtarma için farklı bir coğrafi konumdaki birincil siteyi uzağa yedeklemelisiniz. Bu yaklaşım, özel olarak yedekleme VM veya disklerin özgün etkilenen aynı olay tarafından etkilenmez sağlamaya yardımcı olur. Daha fazla bilgi için [Azure uygulamaları için olağanüstü durum kurtarma](/azure/architecture/resiliency/disaster-recovery-azure-applications).

DR konularınızdan aşağıdaki özellikleri içerebilir:

- Yüksek Kullanılabilirlik: Uygulamanın önemli bir kapalı kalma süresi olmadan, sağlam bir durumda çalışmaya devam etmesini yeteneği. Tarafından *sağlıklı duruma*, bu durum uygulamanın yanıt veriyor olması, ve kullanıcıların uygulamaya bağlanmak ve etkileşime anlamına gelir. Belirli görev açısından kritik uygulama ve veritabanlarının bile platform hatası olduğunda her zaman kullanılabilir olacak şekilde gerekli olabilir. Bu iş yükleri için yedeklilik, verilerin yanı sıra, uygulama için planlama gerekebilir.

- Veri dayanıklılığı: Bazı durumlarda, ana göz önünde bulundurarak bir olağanüstü durumda veri korunmasını sağlamaktır. Bu nedenle, verilerinizi farklı bir site yedeklemesini gerekebilir. Bu tür iş yükleri için tam yedeklilik için uygulama, ancak yalnızca normal yedekleme disklerin gerekmeyebilir.

## <a name="backup-and-dr-scenarios"></a>Yedekleme ve olağanüstü durum kurtarma senaryoları

Uygulama iş yükü senaryoları biri bazı tipik örnekler ve olağanüstü durum kurtarma için planlama konuları göz atalım.

### <a name="scenario-1-major-database-solutions"></a>Senaryo 1: Ana veritabanı çözümleri

Bir üretim veritabanı sunucusu, SQL Server veya yüksek kullanılabilirliği destekleyen, Oracle gibi göz önünde bulundurun. Kritik üretim uygulamaları ve kullanıcılar bu veritabanına bağlıdır. Bu sistem için olağanüstü durum kurtarma planı, aşağıdaki gereksinimleri desteklemesi gerekebilir:

- Verilerin korunduğundan ve kurtarılabilir olması gerekir.
- Sunucusunun kullanılabilir olması gerekir.

Olağanüstü durum kurtarma planı, yedek olarak farklı bir bölgede veritabanının bir kopyasını bakımını gerektirebilir. Sunucu kullanılabilirliği ve veri kurtarma için gereksinimlerine bağlı olarak, verilerin düzenli çevrimdışı yedeklemeler için bir aktif-aktif veya Aktif-Pasif kopya sitesinden çözüm aralığında. İlişkisel veritabanları, SQL Server ve Oracle gibi çoğaltma için çeşitli seçenekler sağlar. SQL Server için kullanmak [SQL Server AlwaysOn Kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) yüksek kullanılabilirlik için.

MongoDB gibi NoSQL veritabanları da destek [çoğaltmaları](https://docs.mongodb.com/manual/replication/) yedeklilik için. Yüksek kullanılabilirlik için çoğaltma kullanılır.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Senaryo 2: Artık bir VM kümesi

Yedeklilik sağlamak ve Yük Dengeleme bir VM kümesi tarafından işlenen bir iş yükünü göz önünde bulundurun. Bir bölgede dağıtılan bir Cassandra kümesi bir örnektir. Bu tür bir mimari zaten bu bölgede yedekliliği yüksek düzeyde sağlar. Ancak, iş yükü bir bölge düzeyinde arızasına karşı korumak için küme iki bölgesine yayılan veya başka bir bölgeye düzenli yedeklemeler yapma düşünmelisiniz.

### <a name="scenario-3-iaas-application-workload"></a>Senaryo 3: Iaas uygulaması iş yükü

Iaas uygulama iş yükünün göz atalım. Örneğin, bu uygulama, bir Azure sanal makinesinde çalışan bir tipik bir üretim iş yüklerini olabilir. Bir web sunucusu veya dosya sunucusu içeriği ve bir sitenin diğer kaynakları tutan olabilir. Ayrıca, kendi veri, kaynaklar ve uygulama durumu VM disklerinde depolanan bir sanal makine üzerinde çalışan bir özel olarak geliştirilmiş iş uygulaması da olabilir. Bu durumda, düzenli yedeklemeler dikkate aldığınızdan emin olmak önemlidir. Yedekleme sıklığı, VM iş yükü doğası hakkında temel almalıdır. Örneğin, uygulamayı her gün çalıştırır ve verileri değiştiren, yedekleme saatte gerçekleştirilmelidir.

Toplanan raporları başka kaynaklardan veri çeker ve bir raporlama sunucusu başka bir örnektir. Bu VM veya diskleri kaybı raporların kaybına neden olabilir. Ancak, raporlama işlemi yeniden çalıştırın ve çıktıyı yeniden mümkün olabilir. Bu durumda, Raporlama sunucusu ile bir olağanüstü durum isabet olsa bile, veri kaybı gerçekten yok. Sonuç olarak, Raporlama sunucusu verilerin bir kısmını kaybetme dayanıklılık daha yüksek bir düzeyde olabilir. Bu durumda, daha az sık sık maliyetlerini azaltmak için bir seçenek sahip.

### <a name="scenario-4-iaas-application-data-issues"></a>Senaryo 4: Iaas uygulama veri sorunları

Diğer bir olasılık Iaas uygulama veri sorunlardır. Hesaplar, korur ve fiyatlandırma bilgileri gibi kritik ticari veri hizmet veren bir uygulamayı düşünün. Uygulamanızı yeni bir sürümü yanlış fiyatlandırma hesaplanan ve platform tarafından sunulan mevcut ticaret verileri bozuk bir yazılım hata vardı. Aşağıda, uygulama ve verilerin önceki sürümüne geri dönmek için en iyi eylem planını verilmiştir. Bunu etkinleştirmek için sisteminizin düzenli yedeklemeler yararlanın.

## <a name="disaster-recovery-solution-azure-backup"></a>Olağanüstü durum kurtarma çözümü: Azure Backup 

[Azure yedekleme](https://azure.microsoft.com/services/backup/) yedekleme ve DR için kullanılır ve çalışır [yönetilen diskler](../articles/virtual-machines/windows/managed-disks-overview.md) yönetilmeyen diskler yanı sıra. Bir yedekleme işi, zaman tabanlı yedeklemeler, kolay VM geri yükleme ve yedek bekletme ilkeleri oluşturabilirsiniz.

Kullanırsanız [premium SSD](../articles/virtual-machines/windows/disks-types.md), [yönetilen diskler](../articles/virtual-machines/windows/managed-disks-overview.md), ya da diğer disk türlerinde [yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) seçeneği önemlidir düzenli aralıklarla DR yedeklemeleri yapmak özellikle. Azure yedekleme, Kurtarma Hizmetleri kasasında uzun süreli saklama için verileri depolar. Seçin [coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy-grs.md) seçeneği yedekleme kurtarma Hizmetleri kasası. Bu seçenek yedeklemeleri bölgesel felaketlere karşı koruma için farklı bir Azure bölgesinde çoğaltılmasını sağlar.

Yönetilmeyen diskler için Iaas diskleri için yerel olarak yedekli depolama türü kullanır, ancak Azure Backup ile kurtarma Hizmetleri kasası için coğrafi olarak yedekli depolama seçeneği etkin olduğundan emin olun.

> [!NOTE]
> Kullanırsanız [coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy-grs.md) veya [okuma erişimli coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) yönetilmeyen diskleriniz için yine seçeneğine ihtiyacınız tutarlı anlık görüntüler için yedekleme ve olağanüstü durum kurtarma. Hangisini [Azure Backup](https://azure.microsoft.com/services/backup/) veya [tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots).

 Aşağıdaki tabloda, çözümlerin DR için kullanılabilir bir özetidir.

| Senaryo | Otomatik olarak çoğaltma | DR çözümü |
| --- | --- | --- |
| Premium SSD diskleri | Yerel ([yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilen diskler | Yerel ([yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilmeyen yerel olarak yedekli depolama diskleri | Yerel ([yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilmeyen coğrafi olarak yedekli depolama diskleri | Çapraz bölge ([coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy-grs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots) |
| Yönetilmeyen okuma erişimli coğrafi olarak yedekli depolama diskleri | Çapraz bölge ([okuma erişimli coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots) |

Yüksek kullanılabilirlik en iyi bir kullanılabilirlik kümesinde yanı sıra Azure Backup, yönetilen diskleri kullanarak karşılanır. Yönetilmeyen diskler kullanıyorsanız, Azure Backup DR için kullanabilirsiniz. Azure Yedekleme'yi bulamıyorsanız, ardından alma [tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots)bir sonraki bölümde açıklandığı gibi yedekleme için alternatif bir çözümdür ve olağanüstü durum kurtarma.

Yüksek kullanılabilirlik, yedekleme ve DR uygulamanın veya altyapının düzeylerinde seçimlerinizi şu şekilde temsil edilebilir:

| Düzey |   Yüksek kullanılabilirlik   | Yedekleme ve DR |
| --- | --- | --- |
| Uygulama | SQL Server AlwaysOn | Azure Backup |
| Altyapı    | Kullanılabilirlik kümesi  | Coğrafi olarak yedekli depolama ile tutarlı anlık görüntüler |

### <a name="using-azure-backup"></a>Azure Backup'ı kullanma 

[Azure yedekleme](../articles/backup/backup-azure-vms-introduction.md) Vm'lerinizi Windows çalıştıran yedekleyebilir veya Linux için Azure kurtarma Hizmetleri kasası. Yedekleme ve geri yükleme iş açısından kritik verilerin, verileri oluşturan uygulama çalışırken, iş açısından kritik verilerin yedeklenmesi gereken olgusu karmaşık. 

Bu sorunu gidermek için Azure Backup Microsoft iş yükleri için uygulamayla tutarlı yedeklemeler yapılmasını sağlar. Veri depolama için doğru yazıldığından emin olmak için birim gölge hizmeti kullanır. Linux için birim gölge hizmeti eşdeğer işlevleri olmadığından Linux VM'ler için yalnızca dosyayla tutarlı yedekleme olasılığı vardır.

![Azure Backup akış][1]

Azure Backup, zamanlanan saatte bir yedekleme işi başlattığında, yedekleme uzantısını zaman içinde nokta anlık görüntüsünü almak için VM'e yüklenen tetikler. Bir anlık görüntüsü kapatabilirler gerek kalmadan sanal makinenin diskleri tutarlı bir anlık görüntüsünü almak için birim gölge hizmeti ile koordinasyon halinde alınır. VM yedekleme uzantısına tüm yazma işlemlerini önce tüm diskler tutarlı bir anlık görüntüsünü aktarır. Anlık görüntüyü aldıktan sonra verileri Azure Backup tarafından yedekleme kasasına aktarılır. Yedekleme işlemi daha verimli hale getirmek için hizmete tanımlar ve yalnızca son yedeklemeden sonra değiştirilen veri bloklarını aktarır.

Geri yüklemek için Azure Backup aracılığıyla kullanılabilir yedekler görüntüleyebilir ve ardından geri yükleme başlatın. Oluşturma ve Azure yedeklemelerini geri [Azure portalında](https://portal.azure.com/)tarafından [PowerShell kullanarak](../articles/backup/backup-azure-vms-automation.md), kullanarak veya [Azure CLI](/cli/azure/).

### <a name="steps-to-enable-a-backup"></a>Bir yedeklemeyi etkinleştirme adımları

Sanal makinelerinizin yedeklerini kullanarak etkinleştirmek için aşağıdaki adımları kullanın [Azure portalında](https://portal.azure.com/). Gerçek senaryonuza bağlı olarak bazı çeşitlemeyi yoktur. Başvurmak [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) tüm ayrıntılar için belgeleri. Azure Backup ayrıca [yönetilen diskleri olan sanal makineleri destekler](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Kurtarma Hizmetleri kasası için bir VM oluşturun:

    a. İçinde [Azure portalında](https://portal.azure.com/), Gözat **tüm kaynakları** ve bulma **kurtarma Hizmetleri kasaları**.

    b. Üzerinde **kurtarma Hizmetleri kasaları** menüsünü tıklatın **Ekle** ve VM ile aynı bölgede yeni bir kasa oluşturmak için aşağıdaki adımları izleyin. Örneğin, VM'niz Batı ABD bölgesinde ise, Batı ABD için kasayı seçin.

1.  Yeni oluşturduğunuz kasa için depolama çoğaltma doğrulayın. Altında kasa erişim **kurtarma Hizmetleri kasaları** gidin **özellikleri** > **yedekleme yapılandırması** > **güncelleştirme** . Olun **coğrafi olarak yedekli depolama** seçeneği, varsayılan olarak seçilidir. Bu seçenek, kasanız otomatik olarak ikincil veri merkezine çoğaltılır sağlar. Örneğin, kasanıza Batı ABD, Doğu ABD için otomatik olarak çoğaltılır.

1.  Yedekleme ilkesini yapılandırın ve VM'yi aynı kullanıcı Arabiriminden seçin.

1.  Backup Aracısı sanal makinede yüklü olduğundan emin olun. Bir Azure Galerisi görüntüsünü kullanarak VM'nizi oluşturulursa, Backup Aracısı zaten yüklüdür. (Diğer bir deyişle, özel bir görüntü kullanırsanız) Aksi durumda, yönergeleri kullanın [VM Aracısı, bir sanal makineye yükleme](../articles/backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent).

1.  VM yedekleme hizmeti işlev için ağ bağlantısı izin verdiğinden emin olun. Yönergelerini izleyin [ağ bağlantısı](../articles/backup/backup-azure-arm-vms-prepare.md#establish-network-connectivity).

1.  Önceki adımları tamamladıktan sonra belirtilen yedekleme ilkesinde olarak düzenli aralıklarla yedekleme çalıştırır. Gerekirse, ilk yedekleme Azure portalında kasa panosundan el ile tetikleyebilirsiniz.

Azure Backup komut dosyalarını kullanarak otomatik hale getirmek için bkz [VM yedeklemesi için PowerShell cmdlet'leri](../articles/backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Kurtarma adımları

Onarmak ya da bir VM'yi yeniden oluşturmak ihtiyacınız varsa, herhangi bir kasadaki yedekleme kurtarma noktalarının VM geri yükleyebilirsiniz. Birkaç kurtarma gerçekleştirmek için farklı seçenekler vardır:

-   Yedeklenen VM'niz zaman içinde nokta bir temsili olarak yeni bir VM oluşturabilirsiniz.

-   Diskleri geri yükleyebilir ve sonra özelleştirebilir ve geri yüklenen VM yeniden oluşturmak için VM şablonu kullanın.

Daha fazla bilgi için yönergelerine bakın [sanal makineleri geri yükleme için Azure portalını kullanma](../articles/backup/backup-azure-arm-restore-vms.md). Bu belgede, ayrıca yedeklenen sanal makineleri, birincil veri merkezinde olağanüstü bir durum olursa, yedekleme kasanız coğrafi olarak yedekli kullanarak eşleştirilmiş bir veri merkezine geri yüklemek için belirli adımlar açıklanmaktadır. Bu durumda, Azure Backup geri yüklenen sanal makine oluşturmak için ikincil bölgeden işlem hizmetini kullanır.

PowerShell için de kullanabilirsiniz [yeni VM'den oluşturma diskleri geri](../articles/backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternatif çözüm için: Tutarlı anlık görüntüler

Azure Yedekleme'yi bulamıyorsanız, anlık görüntüleri kullanılarak yedekleme kendi mekanizması uygulayabilirsiniz. Bir VM tarafından kullanılan tüm diskler için tutarlı anlık görüntüleri oluşturmak ve ardından bu anlık görüntülerin başka bir bölgeye çoğaltma karmaşık. Bu nedenle, özel bir çözüm oluşturmaya değerinden daha iyi bir seçenek olarak Yedekleme hizmetini kullanarak Azure dikkate alır.

Okuma erişimli coğrafi olarak yedekli depolama/coğrafi olarak yedekli depolama için diskleri kullanıyorsanız, anlık görüntüler otomatik olarak ikincil veri merkezine çoğaltılır. Yerel olarak yedekli depolama için diskleri kullanırsanız, kendinizi veri çoğaltmak gerekir. Daha fazla bilgi için [artımlı anlık görüntülerle Azure yönetilmeyen VM disklerini yedekleme](../articles/virtual-machines/windows/incremental-snapshots.md).

Bir nesne belirli bir noktada bir temsilini zamanlı anlık görüntüsüdür. Anlık görüntüsünü, isteğe bağlı olarak artımlı veri boyutu için ayrı tutma fatura doğurur. Daha fazla bilgi için [blob anlık görüntüsü oluşturma](../articles/storage/blobs/storage-blob-snapshots.md).

### <a name="create-snapshots-while-the-vm-is-running"></a>VM çalışırken anlık görüntüleri oluşturma

Herhangi bir zamanda bir anlık görüntü alabilirse VM'nin çalışıyor olması durumunda da yine de veri diskleri akıtılan yoktur. Anlık görüntüleri, uçuş modunda olan kısmi işlemleri içerebilir. Ayrıca, ilgili birden çok disk varsa, farklı disklerin anlık görüntüleri farklı zamanlarda oluşmuş olabilir. Bu senaryolar için anlık görüntüleri eşgüdümlü olmayan neden olabilir. Bu eksikliği koordinasyon değişiklik yedekleme sırasında yapıldıysa, dosya bozuk olabilir Bölüştürülmüş birimler için özellikle sorunludur.

Yedekleme işlemi, bu durumu önlemek için aşağıdaki adımları uygulamanız gerekir:

1.  Tüm diskler dondurun.

1.  Bekleyen tüm yazma işlemlerini boşaltmaya.

1.  [Blob anlık görüntüsü oluşturma](../articles/storage/blobs/storage-blob-snapshots.md) tüm diskler için.

SQL Server gibi bazı Windows uygulamaları, uygulamayla tutarlı yedeklemeler oluşturmak için birim gölge hizmeti aracılığıyla eşgüdümlü bir yedekleme mekanizması sağlar. Linux üzerinde gibi bir araç kullanabilirsiniz *fsfreeze* diskleri koordine için. Bu araç, dosya tutarlı yedekler, ancak değil uygulamayla tutarlı anlık görüntüleri sağlar. Kullanmayı düşünmelisiniz. Bu nedenle bu işlemi karmaşıktır [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) veya bu yordamı zaten uygulayan bir üçüncü taraf yedekleme çözümü.

Bir koleksiyondaki tüm VM belirli bir zaman içinde nokta görünümünü temsil eden VM diskleri için Eşgüdümlü anlık bir önceki işlemin sonuçlanır. VM için bir yedekleme geri yükleme noktası budur. Düzenli yedeklemeleri oluşturmak için zamanlanan aralıklarda işlemini tekrar edebilirsiniz. Bkz [yedeklerini başka bir bölgeye kopyalayın](#copy-the-snapshots-to-another-region) anlık görüntüleri DR için başka bir bölgeye kopyalamak adımlar.

### <a name="create-snapshots-while-the-vm-is-offline"></a>VM'yi çevrimdışı durumdayken anlık görüntüleri oluşturma

Tutarlı yedekler oluşturmak için başka bir seçenek, sanal makineyi ve her disk blob anlık oluşturmaktır. BLOB anlık görüntülerini almadan çalışan bir VM anlık görüntülerini koordine daha kolaydır, ancak bu işlem birkaç dakika kapalı kalma süresi gerekir.

1. VM'yi kapatın.

1. Yalnızca birkaç saniye sürer her sanal sabit sürücü blobun anlık görüntüsünü oluşturun.

    Bir anlık görüntüsünü oluşturmak için kullanabileceğiniz [PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure Storage REST API'sini](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](/cli/azure/), ya da Azure depolama istemci kitaplıklarından birini gibi [ .NET için depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/hh488361.aspx).

1. Kapalı kalma süresi sona VM'yi başlatın. Genellikle, işlem birkaç dakika içinde tamamlanır.

Bu işlem, bir VM için bir yedekleme geri yükleme noktası sağlayan tüm diskleri için tutarlı anlık görüntüler koleksiyonunu verir.

### <a name="copy-the-snapshots-to-another-region"></a>Başka bir bölgeye anlık görüntüleri kopyalama

Tek başına anlık görüntü oluşturmaya DR için yeterli olmayabilir. Ayrıca, başka bir bölgeye anlık görüntüsü yedekleri çoğaltılması gerekir.

Ardından diskleriniz için coğrafi olarak yedekli depolama veya okuma erişimli coğrafi olarak yedekli depolama kullanıyorsanız, anlık görüntüleri ikincil bölgeye otomatik olarak çoğaltılır. Birkaç dakika önce çoğaltma gecikmesi olabilir. Anlık görüntü çoğaltma sonlandırmadan önce birincil veri merkezinde arıza yaparsa, ikincil veri merkezlerinden anlık görüntüleri erişemez. Bu olasılığını küçüktür.

> [!NOTE]
> Depolama hesabına sahip bir coğrafi olarak yedekli depolama veya okuma erişimli coğrafi olarak yedekli diskler yalnızca VM felaketlere karşı korumaz. Ayrıca, Eşgüdümlü anlık görüntüleri oluşturmak veya Azure Yedekleme'yi gerekir. Bu, bir VM için tutarlı bir duruma kurtarmak için gereklidir.

Yerel olarak yedekli depolamayı kullanıyorsanız, anlık görüntü oluşturduktan hemen sonra farklı bir depolama hesabı için anlık görüntüleri kopyalamanız gerekir. Kopyalama hedefi Kopyala uzak bir bölgede olduğu kaynaklanan bir yerel olarak yedekli depolama hesabı farklı bir bölgede olabilir. Anlık görüntü, bir okuma erişimli coğrafi olarak yedekli depolama hesabıyla aynı bölgede de kopyalayabilirsiniz. Bu durumda, anlık görüntü gevşek uzak ikincil bölgeye çoğaltılır. Yedekleme buduyor birincil sitedeki kopyaladıktan sonra korumalı ve çoğaltma tamamlandığında.

Artımlı anlık görüntüleriniz DR için verimli bir şekilde kopyalamak için yönergeleri gözden geçirin. [artımlı anlık görüntülerle Azure yönetilmeyen VM disklerini yedekleme](../articles/virtual-machines/windows/incremental-snapshots.md).

![Artımlı anlık görüntülerle Azure yönetilmeyen VM disklerini yedekleme][2]

### <a name="recovery-from-snapshots"></a>Anlık görüntü kurtarma

Anlık görüntü almak için yeni bir blob oluşturmak üzere kopyalayın. Anlık görüntü birincil hesabından kopyalıyorsanız, anlık görüntü üzerinde anlık görüntü, temel bir blob kopyalayabilirsiniz. Bu işlem, disk anlık görüntüye geri döner. Bu işlem anlık görüntüsü yükseltiliyor olarak bilinir. Okuma erişimli coğrafi olarak yedekli depolama hesabı, söz konusu olduğunda, ikincil bir hesabı anlık görüntüsü yedeği kopyalamak için birincil bir hesap kopyalamanız gerekir. Anlık görüntü tarafından kopyalayabilirsiniz [PowerShell kullanarak](../articles/storage/common/storage-powershell-guide-full.md) veya AzCopy yardımcı programını kullanarak. Daha fazla bilgi için [AzCopy komut satırı yardımcı programı ile veri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).

Birden çok disklere sahip VM'ler için aynı Eşgüdümlü geri yükleme noktası parçası olan tüm anlık görüntüleri kopyalamanız gerekir. Anlık görüntüleri için yazılabilir bir VHD bloblarını kopyaladıktan sonra VM için şablon kullanılarak sanal makinenizin yeniden oluşturmak için BLOB'ları kullanabilirsiniz.

## <a name="other-options"></a>Diğer Seçenekler

### <a name="sql-server"></a>SQL Server

Bir VM'de çalışan SQL Server, SQL Server veritabanınızı Azure Blob depolama alanına veya bir dosya için başka bir yedekleme paylaşmak için kendi yerleşik özellikler vardır. Depolama hesabı coğrafi olarak yedekli depolama veya okuma erişimli coğrafi olarak yedekli depolama, daha önce açıklandığı gibi aynı kısıtlamalara olağanüstü bir durumda, bu yedekler depolama hesabının ikincil veri merkezinde erişebilirsiniz. Daha fazla bilgi için [yedekleme ve geri yükleme, SQL Server için Azure sanal makineler'de](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Yedekleme ve geri yükleme, ek olarak [SQL Server AlwaysOn Kullanılabilirlik gruplarını](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) veritabanlarının ikincil çoğaltmaları koruyabilir. Bu özelliği, olağanüstü durum kurtarma süresini önemli ölçüde azaltır.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Bu makalede, yedekleme veya olağanüstü durum kurtarma ve o yedeklerin nasıl kullanılacağını desteklemek üzere, Vm'lerinizi ve disklerini anlık görüntülerini veya verilerinizi kurtarmak için anlık görüntüleri almak nasıl ele. Azure Resource Manager modeli ile birçok kişi ile Azure Vm'lerini ve diğer altyapıları oluşturmak için şablonları kullanın. Her zaman aynı yapılandırmaya sahip bir VM oluşturmak için bir şablon kullanabilirsiniz. Sanal makinelerinizi oluşturmak için özel görüntüler kullanıyorsanız, aynı zamanda görüntülerinizi depolamak için bir okuma erişimli coğrafi olarak yedekli depolama hesabı kullanarak korunduğundan emin olmalısınız.

Sonuç olarak, yedekleme işleminize iki şey bir birleşimi olabilir:

- (Diskler) verileri yedekleyin.
- Yapılandırmayı (şablonları ve özel görüntüler) yedekleyin.

Bağlı seçtiğiniz yedekleme seçeneği olarak hem verileri hem de Yapılandırma Yedekleme işlemini gerekebilir veya yedekleme hizmeti tüm bu sizin için işleyebilir.

## <a name="appendix-understanding-the-impact-of-data-redundancy"></a>Ek: Veri yedekliği etkisini anlama

Azure depolama hesapları için olağanüstü durum kurtarma ile ilgili göz önünde bulundurmanız gereken veri yedekliği üç tür vardır: yerel olarak yedekli, coğrafi olarak yedekli veya coğrafi olarak yedekli okuma erişimi. 

Yerel olarak yedekli depolama, aynı veri merkezinde verilerin üç kopyasını tutar. VM verileri yazdığında, aynı olduklarından bilmesi başarı çağırana döndürülmeden önce tüm üç kopya güncelleştirilir. Tüm üç kopyasını aynı anda etkilenen olası olduğundan, disk yerel hatalardan korunur. Yerel olarak yedekli depolama söz konusu olduğunda yoktur hiçbir coğrafi yedeklilik, disk veri merkezi veya depolama biriminin tamamını etkileyen yıkıcı arızasına karşı korumalı değildir.

Coğrafi olarak yedekli depolama ve okuma erişimli coğrafi olarak yedekli depolama ile verilerinizin üç kopyasını tarafından seçilir birincil bölgesinde saklanır. Azure tarafından ayarlanan karşılık gelen bir ikincil bölgede verilerinizin üç kopya korunur. Örneğin, Batı ABD bölgesinde veri deposu, veri Doğu ABD olarak çoğaltılır. Kopyalama bekletme zaman uyumsuz olarak yapılır ve arasındaki birincil ve ikincil siteler için güncelleştirmeleri kısa bir gecikme yoktur. İkincil sitede disklerin çoğaltmaları tutarlı bir disk başına temelinde (gecikme ile), ancak birden çok etkin diskleri kopyalarını birbirleriyle eşitlenmiş olmayabilir. Birden çok diskte tutarlı çoğaltmalar için tutarlı anlık görüntüleri gereklidir.

Coğrafi olarak yedekli depolama ve okuma erişimli coğrafi olarak yedekli depolama arasındaki temel fark, okuma erişimli coğrafi olarak yedekli depolama ile ikincil kopya herhangi bir zamanda okuyabildiğini ' dir. Birincil bölgedeki veriler erişilemez işleyen bir sorun varsa, Azure ekibi erişimi geri yüklemek için her türlü çabayı yapar. Birincil çalışmadığında, okuma erişimli coğrafi olarak yedekli depolama etkin varsa, ikincil veri merkezinde verilere erişebilir. Birincil erişilemediği durumda çoğaltmadan okuma planlıyorsanız, bu nedenle, ardından okuma erişimli coğrafi olarak yedekli depolama kabul edilmelidir.

Önemli bir kesinti olmasını ettik, Azure ekibi bir coğrafi olarak yük devretme tetiklemek ve ikincil depolama alanına işaret edecek şekilde birincil DNS girişlerini değiştirin. Bu noktada, coğrafi olarak yedekli depolama veya okuma erişimli coğrafi olarak yedekli depolama etkin varsa, kullanılan ikincil bölgedeki veri erişebilirsiniz. Diğer bir deyişle, coğrafi olarak yedekli depolama, depolama hesabıdır ve bir sorun, yalnızca bir coğrafi olarak yük devretme ise ikincil depolama erişebilirsiniz.

Daha fazla bilgi için [bir Azure depolama kesinti oluşursa yapmanız gerekenler](../articles/storage/common/storage-disaster-recovery-guidance.md).

>[!NOTE] 
>Microsoft, bir yük devretme gerçekleştikten olup olmadığını denetler. Bireysel müşteriler tarafından karar değil, böylece her depolama hesabı, yük devretme denetlenmez. Belirli bir depolama hesabı veya sanal makine diskleri için olağanüstü durum kurtarma uygulamak için bu makalede daha önce açıklanan olan tekniklerle kullanmanız gerekir.

[1]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png