---
title: Azure'a VMware olağanüstü durum kurtarma için Azure Site Recovery dağıtım Planlayıcısı raporunu analiz etme | Microsoft Docs
description: Bu makalede Vmware'den azure'a olağanüstü durum kurtarma için Azure Site Recovery dağıtım Planlayıcısı tarafından oluşturulan raporu analiz etme.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 3/20/2019
ms.author: mayg
ms.openlocfilehash: cbea6785239c70a3cdb229d0811497f051224238
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61472609"
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report-for-vmware-disaster-recovery-to-azure"></a>Vmware'den azure'a olağanüstü durum kurtarma için Azure Site Recovery dağıtım Planlayıcısı raporunu analiz etme

Oluşturulan Microsoft Excel raporu şu sayfaları içerir:
## <a name="on-premises-summary"></a>Şirket içi özeti
Şirket içi özeti çalışma sayfası, profili oluşturulmuş VMware ortamına genel bir bakış sağlar.

![VMware ortamının şirket içi özeti](media/site-recovery-vmware-deployment-planner-analyze-report/on-premises-summary-v2a.png)

**Başlangıç tarihi** ve **bitiş tarihi**: Rapor oluşturma için göz önünde bulundurulan profil oluşturma verilerinin başlangıç ve bitiş tarihleri. Varsayılan olarak, başlangıç tarihi profil oluşturmanın başladığı, bitiş tarihi ise profil oluşturmanın durdurulduğu tarihtir. Rapor bu parametrelerle oluşturulduysa bunlar ‘StartDate’ ve ‘EndDate’ değerleri olabilir.

**Profil oluşturma işleminin toplam sayısı**: Raporun oluşturulduğu başlangıç ve bitiş tarihleri arasında profil gün sayısı.

**Uyumlu sanal makinelerin sayısı**: Gerekli ağ bant genişliği, gerekli depolama hesapları, Microsoft Azure çekirdek, yapılandırma sunucusu ve ek işlem sunucularının sayısı hesaplandığı uyumlu sanal makinelerin toplam sayısı.

**Tüm uyumlu sanal makinelerdeki toplam disk sayısına**: Yapılandırma sunucusu ve dağıtımda kullanılacak ek işlem sunucusu sayısına karar vermeye yönelik girdilerden biri kullanılan bir sayı.

**Uyumlu sanal makinedeki disk ortalama sayısı**: Diskler, tüm uyumlu sanal makinelerde hesaplanan ortalama sayısı.

**Ortalama disk boyutu (GB)** : Tüm uyumlu sanal makinelerde hesaplanan ortalama disk boyutudur.

**İstenen RPO (dakika)** : Varsayılan kurtarma noktası hedefi veya gerekli bant genişliğini tahmin etmek için rapor oluşturma sırasında 'DesiredRPO' parametresi için geçirilen değer.

**İstenen bant genişliği (MB/sn)** : Ulaşılabilir RPO'yu tahmin etmek için rapor oluşturma sırasında geçirilen 'Bandwidth' parametresi için değer.

**(GB) günde gözlemlenen tipik veri değişim sıklığı**: Profil oluşturma tüm günlerde gözlemlenen ortalama veri değişim sıklığı. Bu sayı, dağıtımda kullanılacak yapılandırma sunucusu ve ek işlem sunucusu sayısına karar vermeye yönelik girdilerden biri olarak kullanılır.

## <a name="recommendations"></a>Öneriler

>[!Note]
>Doğrudan çoğaltma yönetilen diskler, depolama hesabı sayısı için bir öneri yoksayın.

VMware’den Azure'a dağıtım raporunun öneriler sayfasında, seçilen ve istenen RPO'ya göre aşağıdaki ayrıntılar yer alır:

![VMware'den Azure'a dağıtım raporu için öneriler](media/site-recovery-vmware-deployment-planner-analyze-report/Recommendations-v2a.png)

### <a name="profiled-data"></a>Profili oluşturulan veriler
![Dağıtım planlayıcısında profili oluşturulmuş veriler görünümü](media/site-recovery-vmware-deployment-planner-analyze-report/profiled-data-v2a.png)

**Profili oluşturulmuş veri dönemi**: Profil oluşturma sırasında hangi çalıştırıldığı süre. Varsayılan olarak, rapor oluşturma sırasında StartDate ve EndDate seçenekleri kullanılarak rapor belirli bir süre için oluşturulmadıkça araç, profili oluşturulmuş tüm verileri hesaplamaya dahil eder.

**Sunucu adı**: Adı veya VMware vCenter veya ESXi ana makinelerinin raporu oluşturulan IP adresi.

**İstenen RPO**: Dağıtımınız için kurtarma noktası hedefi. Varsayılan olarak, gerekli ağ bant genişliği 15, 30 ve 60 dakikalık RPO değerleri için hesaplanır. Seçim temel alınarak, etkilenen değerler sayfada güncelleştirilir. Raporu oluştururken *DesiredRPOinMin* parametresini kullandıysanız, değer İstenen RPO sonucunda gösterilir.

### <a name="profiling-overview"></a>Profil oluşturmaya genel bakış

![Dağıtım planlayıcısında profil oluşturma sonuçları](media/site-recovery-vmware-deployment-planner-analyze-report/profiling-overview-v2a.png)

**Toplam sanal makine profili**: Profili oluşturulan verileri kullanılabilir sanal makinelerin toplam sayısı. VMListFile dosyasında profili oluşturulmamış sanal makineler varsa, bu sanal makineler rapor oluşturma işleminde göz önünde bulundurulmaz ve profili oluşturulan toplam sanal makine sayısının dışında tutulur.

**Uyumlu sanal makinelerin**: Site Recovery kullanılarak Azure'da korunabilen sanal makineleri sayısı. Bu sayı, gerekli ağ bant genişliği, depolama hesabı sayısı, Azure çekirdek sayısı ve yapılandırma sunucusu ile ek işlem sunucularının sayısının hesaplandığı uyumlu sanal makinelerin toplamıdır. Her uyumlu VM’nin ayrıntıları "Uyumlu VM’ler" bölümünde bulunabilir.

**Uyumsuz sanal makineler**: Site Recovery ile koruma için uygun olmayan profili oluşturulan sanal makinelerin sayısı. Uyumsuzluğun nedenleri, “Uyumsuz VM’ler” bölümünde belirtilmiştir. VMListFile içinde profili oluşturulmamış bir sanal makinenin adı varsa, bu sanal makineler uyumsuz sanal makine sayısının dışında bırakılır. Bu sanal makineler, “Uyumsuz VM’ler” bölümünün sonunda “Veri bulunamadı” olarak listelenir.

**İstenen RPO**: Dakika cinsinden istenen kurtarma noktası hedefi. Rapor üç RPO değeri için oluşturulur: 15 (varsayılan), 30 ve 60 dakika. Rapordaki bant genişliği önerisi, tablonun sağ üst köşesinde bulunan İstenen RPO açılır listesindeki seçiminize göre değişir. Raporu özel bir değer ile *-DesiredRPO* parametresini kullanarak oluşturduysanız, bu özel değer İstenen RPO açılır listesinde varsayılan olarak gösterilir.

### <a name="required-network-bandwidth-mbps"></a>Gerekli ağ bant genişliği (Mb/sn)

![Dağıtım planlayıcısında gerekli ağ bant genişliği](media/site-recovery-vmware-deployment-planner-analyze-report/required-network-bandwidth-v2a.png)

**Sürenin yüzde 100 RPO karşılamak için:** Önerilen bant genişliğini sürenin yüzde 100 istenen RPO karşılamak için ayrılacak MB/sn cinsinden. Bu bant genişliği miktarı, herhangi bir RPO ihlalini önlemek üzere tüm uyumlu sanal makinelerinizin kararlı durum delta çoğaltması için ayrılmalıdır.

**Süresinin yüzde 90'ünü karşılayacak şekilde**: İstenen RPO süresini yüzde 100 karşılamak için gereken bant genişliğini ayarlayamazsınız, geniş bant fiyatlandırması nedeniyle veya başka bir nedenle, istenen RPO süresini yüzde 90'karşılayabilecek daha düşük bir bant genişliği ayarlamayı seçebilirsiniz. Daha düşük olan bu bant genişliğini ayarlamanın etkilerini anlamak için, raporda beklenen RPO ihlallerinin sayısı ve süresine ilişkin bir ne yapmalı analizi sağlar.

**Elde edilen aktarım hızı:** Depolama hesabının bulunduğu Microsoft Azure bölgesine GetThroughput komutunu çalıştırdıktan sunucudan aktarım. Bu aktarım hızı sayısı, yapılandırma sunucusu veya işlem sunucusu depolama ve ağ özelliklerinin, aracı çalıştırdığınız sunucudaki özelliklerle aynı kalması şartıyla, Site Recovery kullanarak uyumlu VM’leri korurken elde edebileceğiniz tahmini düzeyi gösterir.

Çoğaltma için, önerilen bant genişliğini sürenin yüzde 100’ünü karşılayacak şekilde ayarlamanız gerekir. Bant genişliğini ayarladıktan sonra araç tarafından ulaşıldığı bildirilen aktarım hızında herhangi bir artış görmezseniz aşağıdakileri yapın:

1. Site Recovery aktarım hızını sınırlayan herhangi bir ağ Hizmet Kalitesi (QoS) olup olmadığını denetleyin.

2. Ağ gecikme süresini en aza indirmek için, Site Recovery kasanızın desteklenen en yakın fiziksel Microsoft Azure bölgesinde olup olmadığını denetleyin.

3. Donanımı iyileştirip iyileştiremeyeceğinizi (örneğin, HDD’den SSD’ye) belirlemek için yerel depolama özelliklerini denetleyin.

4. [Çoğaltma için kullanılan ağ bant genişliği miktarını artırmak](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth) üzere işlem sunucusundaki Site Recovery ayarlarını değiştirin.

Aracı korunan sanal makinelere zaten sahip olan bir yapılandırma sunucusu veya işlem sunucusu üzerinde çalıştırıyorsanız, aracı birkaç kez çalıştırın. Elde edilen aktarım hızı sayısı, zamanın o noktasında işlenen değişim hızı miktarına bağlı olarak değişir.

Tüm kurumsal Site Recovery dağıtımları için [ExpressRoute](https://aka.ms/expressroute) kullanılması önerilir.

### <a name="required-storage-accounts"></a>Gerekli depolama hesapları
Aşağıdaki grafikte tüm uyumlu sanal makineleri korumak için gereken depolama hesaplarının (standart ve premium) toplam sayısı gösterilmektedir. Her bir VM için kullanılacak depolama hesabını öğrenmek için "VM depolama yerleşimi" bölümüne bakın.

![Dağıtım planlayıcısında gerekli depolama hesapları](media/site-recovery-vmware-deployment-planner-analyze-report/required-storage-accounts-v2a.png)

### <a name="required-number-of-azure-cores"></a>Gerekli Azure çekirdek sayısı
Bu sonuç, tüm uyumlu sanal makinelerin yük devretme işlemi ya da yük devretme testi öncesinde ayarlanması gereken toplam çekirdek sayısıdır. Abonelikte çok az sayıda çekirdek varsa, yük devretme testi veya yük devretme sırasında Azure Site Recovery, sanal makineleri oluşturamaz.

![Dağıtım planlayıcısında gereken Azure çekirdek sayısı](media/site-recovery-vmware-deployment-planner-analyze-report/required-cores-v2a.png)

### <a name="required-on-premises-infrastructure"></a>Gerekli şirket içi altyapısı
Bu sayı, tüm uyumlu sanal makineleri korumak için yapılandırılması gereken yapılandırma sunucusu ve ek işlem sunucularının toplam sayısıdır. [Yapılandırma sunucusu için desteklenen boyut önerilerine](https://aka.ms/asr-v2a-on-prem-components) bağlı olarak, araç ek sunucular önerebilir. Öneri, günlük değişim sıklığı veya en fazla korunan VM sayısından (VM başına ortalama üç disk olduğu varsayılarak) yapılandırma sunucusu veya ek işlem sunucusunda ilk gerçekleşen olaya göre yapılır. Günlük toplam değişim sıklığı ve toplam korunan disk sayısına ilişkin ayrıntıları “Şirket içi özeti” bölümünde bulabilirsiniz.

![Dağıtım planlayıcısında gerekli şirket içi altyapı](media/site-recovery-vmware-deployment-planner-analyze-report/required-on-premises-components-v2a.png)

### <a name="what-if-analysis"></a>Benzetim analiz
Bu analiz, sürenin yalnızca yüzde 90’ını karşılamasını istediğiniz RPO için daha düşük bir bant genişliği ayarladığınızda profil oluşturma sırasında kaç tane ihlal oluşabileceğini özetler. Belirli bir günde bir veya daha fazla RPO ihlali ortaya çıkabilir. Graf, günün yoğun RPO değerini gösterir.
Bu analizi temel alarak, belirtilen düşük bant genişliği ile tüm günlerdeki RPO ihlali sayısının ve bir gündeki en yoğun RPO gerçekleşme zamanının kabul edilebilir olup olmadığına karar verebilirsiniz. Değer kabul edilebilir düzeydeyse, çoğaltma için düşük bant genişliği ayırabilirsiniz; aksi takdirde, istenilen yüzde 100 RPO süresini karşılamak üzere önerilen yüksek bant genişliğini ayırmanız gerekir.

![Dağıtım planlayıcısında benzetim analizi](media/site-recovery-vmware-deployment-planner-analyze-report/what-if-analysis-v2a.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>İlk çoğaltma için önerilen VM toplu iş boyutu
Bu bölümde, ayarlanmakta olan sürenin %100 RPO değerini karşılamak amacıyla ilk çoğaltmayı önerilen bant genişliği ile 72 saat içinde tamamlamak için paralel olarak korunması gereken sanal makine sayısı önerilmektedir. Bu değer yapılandırılabilir bir değerdir. Rapor oluşturma zamanında bu değeri değiştirmek için *GoalToCompleteIR* parametresini kullanın.

Buradaki grafikte, tüm uyumlu makinelerde algılanan ortalama sanal makine boyutuna göre 72 saat içinde ilk çoğaltmayı tamamlamak için bir bant genişliği değer aralığı ve hesaplanmış sanal makine toplu iş boyutu sayısı gösterilmektedir.

Genel önizleme sürümünde rapor, bir toplu işe hangi sanal makinelerin dahil edilmesi gerektiğini belirtmez. Her bir sanal makinenin boyutunu bulmak ve bir toplu işe yönelik sanal makinelerinizi seçmek ya da bilinen iş yükü özelliklerine göre seçim yapmak için, “Uyumlu VM’ler” bölümünde gösterilen disk boyutunu kullanabilirsiniz. İlk çoğaltmayı tamamlama süresi, gerçek VM disk boyutu, kullanılan disk alanı ve kullanılabilir ağ aktarım hızı ile doğru orantılı bir şekilde değişir.

![Önerilen VM toplu iş boyutu](media/site-recovery-vmware-deployment-planner-analyze-report/ir-batching-v2a.png)

### <a name="cost-estimation"></a>Maliyet tahmini
Grafta, seçtiğiniz hedef bölgede ve rapor oluşturma için belirttiğiniz para biriminde Azure'a tahmini toplam olağanüstü durum kurtarma (DR) maliyetinin özet görünümü gösterilir.

![Maliyet tahmini özeti](media/site-recovery-vmware-deployment-planner-analyze-report/cost-estimation-summary-v2a.png)

Bu özet Azure Site Recovery kullanarak tüm uyumlu sanal makinelerinizi Azure'da koruduğunuzda ödemeniz gereken depolama, bilgi işlem, ağ ve lisansın maliyetini anlamanıza yardımcı olur. Maliyet, tüm profili oluşturulan sanal makineler için değil uyumlu sanal makineler için hesaplanır.  

Aylık veya yıllık maliyeti görüntüleyebilirsiniz. [Desteklenen hedef bölgeler](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-target-regions) ve [desteklenen para birimleri](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-currencies) hakkında daha fazla bilgi edinin.

**Bileşenlere göre maliyet** toplam DR maliyeti dört bileşene bölünür: İşlem, depolama, ağ ve Azure Site Recovery lisans maliyeti. Maliyet, şirket içi siteyle Azure arasında ve yapılandırılan işlem, depolama (premium ve standart) ExpressRoute/VPN ve Azure Site Recovery lisansı için çoğaltma sırasında ve DR tatbikatında tahakkuk ettirilecek tüketim temelinde hesaplanır.

**Durumlara göre maliyet** Toplam olağanüstü durum kurtarma (DR) maliyeti, iki farklı duruma göre kategorilere ayrılır: Çoğaltma ve DR tatbikatı.

**Çoğaltma maliyeti**:  Çoğaltma sırasında tahakkuk ettirilen maliyet. Depolama, ağ ve Azure Site Recovery lisansı maliyetini kapsar.

**DR Tatbikatı maliyeti**: Yük devretme testi sırasında tahakkuk ettirilen maliyet. Azure Site Recovery, yük devretme testi sırasında sanal makineleri çalıştırır. DR tatbikatı maliyeti, çalıştırılan sanal makinelerin işlem ve depolama maliyetini kapsar.

**Ay/Yıl başına Azure depolama maliyeti** Çoğaltma ve DR tatbikatının premium ve standart depolaması için tahakkuk ettirilecek toplam depolama maliyetini gösterir.
[Maliyetini Tahmini](site-recovery-vmware-deployment-planner-cost-estimation.md) sayfasında VM başına ayrıntılı maliyet analizini görüntüleyebilirsiniz.

### <a name="growth-factor-and-percentile-values-used"></a>Kullanılan büyüme faktörü ve yüzdelik değerler
Tablonun alt kısmındaki bu bölümde, profili oluşturulan sanal makinelerin tüm performans sayaçları için kullanılan yüzdelik dilim değeri (varsayılan değer yüzde 95’lik dilim) ve tüm hesaplamalarda kullanılan büyüme faktörü (varsayılan değer yüzde 30) gösterilmektedir.

![Kullanılan büyüme faktörü ve yüzdelik değerler](media/site-recovery-vmware-deployment-planner-analyze-report/growth-factor-v2a.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Girdi olarak kullanılabilir bant genişliği ile ilgili öneriler

![Girdi olarak kullanılabilir bant genişliği ile ilgili öneriler](media/site-recovery-vmware-deployment-planner-analyze-report/profiling-overview-bandwidth-input-v2a.png)

Site Recovery çoğaltması için x MB/sn’den fazla bant genişliği ayarlayamayacağınızı bildiğiniz bir durumla karşılaşabilirsiniz. Araç, kullanılabilir bant genişliğini girmenize (rapor oluşturma sırasında -Bandwidth parametresini kullanarak) ve dakika cinsinden ulaşılabilir RPO değerini almanıza olanak tanır. Bu ulaşılabilir RPO değeriyle ek bant genişliği ayarlamanızın gerekli olup olmadığına veya bu RPO ile bir olağanüstü durum kurtarma çözümüne sahip olmak isteyip istemediğinize karar verebilirsiniz.

![500 MB/sn bant genişliği için elde edilebilen RPO](media/site-recovery-vmware-deployment-planner-analyze-report/achievable-rpo-v2a.png)

## <a name="vm-storage-placement"></a>VM-depolama yerleşimi

>[!Note]
>Doğrudan çoğaltma yönetilen diskler, depolama hesabı sayısı hakkında endişelenmeniz gerekmez. Depolama için yalnızca öneri (standart veya Premium) depolama türü kullanın. Aynı türde, yönetilen diskler için geçerlidir.

![VM-depolama yerleşimi](media/site-recovery-vmware-deployment-planner-analyze-report/vm-storage-placement-v2a.png)

**Disk depolama türü**: İçinde bahsedilen tüm ilgili sanal makineleri çoğaltmak için kullanılan herhangi bir standart veya premium depolama hesabı **yerleştirilecek VM'ler** sütun.

**Önerilen ön ek**: Depolama hesabını adlandırmak için kullanılabilecek önerilen üç karakterli önek. Kendi ön ekinizi kullanabilirsiniz, ancak aracın önerisi [depolama hesapları için bölüm adlandırma kuralına](https://aka.ms/storage-performance-checklist) uygundur.

**Önerilen hesap adı**: Önerilen ön eki ekledikten sonra depolama hesabı adı. Köşeli ayraç (< ve >) içindeki adı özel girdinizle değiştirin.

**Kayıt depolama hesabı**: Tüm çoğaltma kayıtları standart bir depolama hesabında depolanır. Premium depolama hesabına çoğaltılan sanal makineler için günlük depolamaya yönelik ek bir standart depolama hesabı oluşturun. Tek bir standart kayıt depolama hesabı, birden fazla premium çoğaltma depolama hesabı tarafından kullanılabilir. Standart depolama hesaplarına çoğaltılan sanal makineler, günlüklerle aynı depolama hesabını kullanır.

**Önerilen günlük hesabı adı**: Önerilen ön eki ekledikten sonra depolama günlük hesabı adı. Köşeli ayraç (< ve >) içindeki adı özel girdinizle değiştirin.

**Yerleştirme özeti**: Toplam sanal makine yükünün özetini depolama hesabına çoğaltma zaman yükleyin ve yük devretme veya yük devretme testi. Buna depolama hesabıyla eşlenmiş toplam sanal makine sayısı, bu depolama hesabına yerleştirilen tüm sanal makinelerdeki toplam okuma/yazma IOPS sayısı, toplam yazma (çoğaltma) IOPS sayısı, tüm disklerde ayarlanan toplam boyut ve toplam disk sayısı dahildir.

**Yerleştirilecek sanal makineler**: En iyi performans ve kullanım için belirli bir depolama hesabına yerleştirilmesi gereken tüm sanal makinelerin listesi.

## <a name="compatible-vms"></a>Uyumlu VM’ler
![Uyumlu VM'lerin Excel elektronik tablosu](media/site-recovery-vmware-deployment-planner-analyze-report/compatible-vms-v2a.png)

**VM adı**: VM adı veya bir rapor oluşturulurken VMListFile içinde kullanılan IP adresi. Bu sütunda ayrıca sanal makinelere bağlanan diskler (VMDK) listelenir. Yinelenen adlara veya IP adreslerine sahip vCenter sanal makinelerini birbirinden ayırt etmek için, adlar ESXi ana bilgisayar adını içerir. Listelenen ESXi ana bilgisayarı, profil oluşturma sırasında araç keşfettiğinde VM’in yerleştirildiği yerdir.

**VM uyumluluğu**: Değerler **Evet** ve **Evet**\*. **Evet** \* için sanal makine için uygun olduğu örneklere [premium SSD](../virtual-machines/windows/disks-types.md). Burada, profili oluşturulmuş yüksek değişim sıklığı veya IOPS diski P20 ya da P30 kategorisine uyar, ancak diskin boyutu diskin bir P10 veya P20 ile eşlenmesine neden olur. Depolama hesabı, bir diskin boyutuna göre hangi premium depolama disk türüne eşleneceğine karar verir. Örneğin:
* <128 GB bir P10’dur.
* 128 GB ile 256 GB arası P15'tir
* 256 GB ile 512 GB arası P20'dir.
* 512 GB ile 1024 GB arası P30'dur.
* 1025 GB ile 2048 GB arası P40'dır.
* 2049 GB ile 4095 GB arası P50'dir.

Örneğin, diskin iş yükü özellikleri diski P20 veya P30 kategorisine koyarken boyutu nedeniyle daha düşük bir premium depolama disk türüne eşleniyorsa, araç bu VM’yi **Evet**\* olarak işaretler. Araç ayrıca kaynak disk boyutunu önerilen premium depolama disk türüne uyacak şekilde değiştirmenizi veya hedef disk türünü yük devretme sonrasını değiştirmenizi önerir.

**Depolama türü**: Standart veya premium.

**Önerilen ön ek**: Üç karakterli depolama hesabı ön ekidir.

**Depolama hesabı**: Önerilen depolama hesabı ön ekini kullanan ad.

**Okuma/yazma IOPS (büyüme faktörü ile) en üst seviyeye**: Yoğun iş yükü okuma/yazma IOPS disk üzerinde (varsayılan, 95'lik dilim), gelecekteki büyüme faktörünü de (varsayılan değer yüzde 30). Sanal makinenin en yoğun okuma/yazma IOPS değeri profil oluşturma işleminin her dakikası boyunca tek disklerin okuma/yazma IOPS değerinin toplamı olduğundan, bir sanal makinenin toplam okuma/yazma IOPS değeri her zaman sanal makinenin ayrı disklerinin okuma/yazma IOPS değerine eşit değildir.

**En yoğun veri değişim sıklığı (büyüme faktörü ile) MB/sn cinsinden**: En yüksek erime oranı disk üzerinde (varsayılan, 95'lik dilim), gelecekteki büyüme faktörünü de (varsayılan değer yüzde 30). Sanal makinenin en yoğun veri değişim sıklığı, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin veri değişim sıklığı toplamının tepe noktası olduğundan, sanal makinenin toplam veri değişim sıklığı her zaman sanal makinedeki ayrı disklerin veri değişim sıklığının toplamı olmayacaktır.

**Azure VM boyutu**: Azure Cloud Services sanal makine boyutu bu şirket içi sanal makine için eşlenen ideal. Eşleme, şirket içi sanal makinenin belleğine, disk/çekirdek/ağ arabirimi sayısına ve okuma/yazma IOPS değerine bağlıdır. Her zaman şirket içi VM özelliklerinin tümüyle eşleşen en düşük Azure VM boyutunun kullanılması önerilir.

**Disk sayısı**: VM üzerinde sanal makine disklerinin (Vmdk) toplam sayısı.

**Disk boyutu (GB)** : Sanal Makinenin tüm disklerinin toplam kurulum boyutu. Araç ayrıca sanal makinedeki ayrı diskler için disk boyutunu gösterir.

**Çekirdek**: Sanal makine üzerindeki CPU çekirdeği sayısı.

**Bellek (MB)** : VM üzerindeki RAM.

**NIC**: VM üzerindeki NIC sayısı.

**Önyükleme türü**: Sanal Makinenin önyükleme türü. BIOS veya EFI olabilir.  Azure Site Recovery şu anda önyükleme diskindeki bölüm sayısı 4’ten az ve önyükleme kesimi boyutu 512 bayt olmak şartıyla Windows Server EFI VM’lerini (Windows Server 2012, 2012 R2 ve 2016) desteklemektedir. EFI VM'lerini korumak için Azure Site Recovery hareketlilik hizmeti sürümü 9.13 veya üstü olmalıdır. EFI VM'ler için yalnızca yük devretme desteklenir. Yeniden çalışma desteklenmez.  

**İşletim sistemi türü**: Sanal Makinenin işletim sistemi türü var. VM oluşturulurken VMware vSphere’den seçilen şablona bağlı olarak Windows veya Linux ya da başka bir işletim sistemi olabilir.  

## <a name="incompatible-vms"></a>Uyumsuz VM’ler

![Uyumsuz VM'lerin Excel elektronik tablosu
](media/site-recovery-vmware-deployment-planner-analyze-report/incompatible-vms-v2a.png)

**VM adı**: VM adı veya bir rapor oluşturulurken VMListFile içinde kullanılan IP adresi. Bu sütunda ayrıca sanal makinelere bağlanan VMDK’lar listelenir. Yinelenen adlara veya IP adreslerine sahip vCenter sanal makinelerini birbirinden ayırt etmek için, adlar ESXi ana bilgisayar adını içerir. Listelenen ESXi ana bilgisayarı, profil oluşturma sırasında araç keşfettiğinde VM’in yerleştirildiği yerdir.

**VM uyumluluğu**: Neden belirli bir VM'nin Site Recovery ile kullanım için uyumlu olmadığını gösterir. Sanal makinenin her uyumsuz diski için, yayımlanan [depolama sınırlarına](https://aka.ms/azure-storage-scalbility-performance) göre nedenler aşağıdakilerden biri olabilir:

* Disk boyutu > 4095 GB’dir. Azure Depolama şu anda 4095 GB’den büyük veri diski boyutlarını desteklememektedir.

* İşletim sistemi diski >2048 GB'dir. Azure Depolama şu anda 2048 GB’den büyük işletim sistemi diski boyutunu desteklememektedir.

* Toplam VM boyutu (çoğaltma + TFO), desteklenen depolama hesabı boyut sınırını (35 TB) aşıyor. Bu uyumsuzluk genellikle sanal makine içindeki tek bir diskin standart depolama için desteklenen Azure veya Site Recovery sınırlarını aşan bir performans özelliği olduğunda gerçekleşir. Bu tür bir örnek, sanal makineyi premium depolama bölgesine iter. Ancak, bir premium depolama hesabı için desteklenen en büyük boyut 35 TB’dir ve tek bir korunan sanal makine birden fazla depolama hesabında korunamaz. Ayrıca, korunan bir sanal makine üzerinde yük devretme testi yürütüldüğünde, test ile çoğaltma aynı depolama hesabında devam eder. Bu örnekte, çoğaltmanın ilerlemesi ve yük devretme testinin paralel olarak başarılı olması için disk boyutunun 2 katını ayarlayın.

* Kaynak IOPS, depolama IOPS için disk başına desteklenen 7500 sınırını aşıyor.

* Kaynak IOPS, depolama IOPS için VM başına desteklenen 80.000 limitini aşıyor.

* Ortalama veri değişim sıklığı, disk için ortalama G/Ç’ye yönelik 10 MB/sn’lik Site Recovery veri değişim sıklığı limitini aşıyor.

* Ortalama veri değişim sıklığı, VM için ortalama G/Ç’ye yönelik 25 MB/sn’lik (tüm disk değişim sıklığının toplamı) Site Recovery veri değişim sıklığı limitini aşıyor.

* Sanal makine üzerindeki tüm disklerde bulunan en yüksek veri değişim sıklığı, VM başına 54 MB/sn’lik desteklenen Site Recovery en yüksek veri değişim sıklığı sınırını aşıyor.

* Algılanan ortalama yazma IOPS değeri, disk için 840 olan desteklenen Site Recovery IOPS limitini aşıyor.

* Hesaplanan anlık görüntü depolama alanı, 10 TB’lik desteklenen anlık görüntü depolama limitini aşıyor.

* Günlük toplam veri değişim sıklığı, bir İşlem Sunucusu tarafından desteklenen günlük 2 TB sınırını aşıyor.


**Okuma/yazma IOPS (büyüme faktörü ile) en üst seviyeye**: Disk üzerindeki en yoğun iş yükü IOPS değeridir (varsayılan değer 95. yüzdebirlik) gelecekteki büyüme faktörünü de (varsayılan değer yüzde 30). Sanal makinenin en yoğun okuma/yazma IOPS değeri profil oluşturma işleminin her dakikası boyunca tek disklerin okuma/yazma IOPS değerinin toplamı olduğundan, bir sanal makinenin toplam okuma/yazma IOPS değeri her zaman sanal makinenin ayrı disklerinin okuma/yazma IOPS değerine eşit değildir.

**En yoğun veri değişim sıklığı (büyüme faktörü ile) MB/sn cinsinden**: En yüksek erime (95. yüzdebirlik varsayılan) disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer yüzde 30) içeren oranı. Sanal makinenin en yoğun veri değişim sıklığı, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin veri değişim sıklığı toplamının tepe noktası olduğundan, sanal makinenin toplam veri değişim sıklığı her zaman sanal makinedeki ayrı disklerin veri değişim sıklığının toplamı olmayacaktır.

**Disk sayısı**: Vmdk sanal makine üzerindeki toplam sayısı.

**Disk boyutu (GB)** : Sanal Makinenin tüm disklerinin toplam kurulum boyutu. Araç ayrıca sanal makinedeki ayrı diskler için disk boyutunu gösterir.

**Çekirdek**: Sanal makine üzerindeki CPU çekirdeği sayısı.

**Bellek (MB)** : VM üzerindeki RAM miktarı.

**NIC**: VM üzerindeki NIC sayısı.

**Önyükleme türü**: Sanal Makinenin önyükleme türü. BIOS veya EFI olabilir.  Azure Site Recovery şu anda önyükleme diskindeki bölüm sayısı 4’ten az ve önyükleme kesimi boyutu 512 bayt olmak şartıyla Windows Server EFI VM’lerini (Windows Server 2012, 2012 R2 ve 2016) desteklemektedir. EFI VM'lerini korumak için Azure Site Recovery hareketlilik hizmeti sürümü 9.13 veya üstü olmalıdır. EFI VM'ler için yalnızca yük devretme desteklenir. Yeniden çalışma desteklenmez.

**İşletim sistemi türü**:  Sanal Makinenin işletim sistemi türü var. VM oluşturulurken VMware vSphere’den seçilen şablona bağlı olarak Windows veya Linux ya da başka bir işletim sistemi olabilir.

## <a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri
Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu limitler yaptığımız testleri temel alsa da mümkün olan tüm uygulama G/Ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir. En iyi sonuçlar için, uygulamanın gerçek performans görüntüsünü elde etmek üzere, dağıtım planlamasından sonra bile yük devretme testi kullanılarak her zaman kapsamlı uygulama testleri gerçekleştirilmesi önerilir.

**Çoğaltma depolama hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günlük toplam kaynak disk veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB  | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |  Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük | 20 MB/sn | Disk başına 1684 GB

**Kaynak veri değişim sıklığı** | **Üst Sınır**
---|---
VM başına ortalama veri değişim sıklığı| 25 MB/sn
VM üzerindeki tüm disklerde en yüksek veri değişim sıklığı | 54 MB/sn
İşlem Sunucusu tarafından desteklenen günlük en fazla veri değişim sıklığı | 2 TB

Bunlar yüzde 30 G/Ç çakışmasını varsayan ortalama sayılardır. Site Recovery; çakışma oranı, büyük yazma boyutları ve gerçek iş yükü G/Ç davranışına göre daha yüksek aktarım hızını işleyebilir. Yukarıdaki sayılar yaklaşık beş dakikalık tipik bir kapsamı varsayar. Diğer bir deyişle, veriler karşıya yüklendikten sonra işlenir ve beş dakika içinde kurtarma noktası oluşturulur.


## <a name="cost-estimation"></a>Maliyet tahmini
[Maliyet tahmini](site-recovery-vmware-deployment-planner-cost-estimation.md) hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar
[Maliyet tahmini](site-recovery-vmware-deployment-planner-cost-estimation.md) hakkında daha fazla bilgi edinin.
