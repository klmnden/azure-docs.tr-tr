---
title: "Hyper-V’den Azure’a Azure Site Recovery dağıtım planlayıcısı| Microsoft Docs"
description: "Bu makalede Hyper-V'den Azure'a dağıtım senaryosu için Azure Site Recovery dağıtım planlayıcısının oluşturduğu raporun analizi açıklanır."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 714c2074f643d2b168c054c5af467b550f57daba
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report"></a>Azure Site Recovery dağıtım planlayıcısı raporunun analizi
Oluşturulan Microsoft Excel raporu aşağıdaki sayfaları içerir:

## <a name="on-premises-summary"></a>Şirket içi özeti
Şirket içi özeti sayfasında, profili oluşturulan Hyper-V ortamına genel bakış sağlanır![Şirket içi özeti](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-summary-h2a.png)

**Başlangıç Tarihi** ve **Bitiş Tarihi**: Rapor oluşturma için göz önünde bulundurulan profil oluşturma verilerinin başlangıç ve bitiş tarihleri. Varsayılan olarak, başlangıç tarihi profil oluşturmanın başladığı, bitiş tarihi ise profil oluşturmanın durdurulduğu tarihtir. Rapor bu parametrelerle oluşturulduysa bunlar ‘StartDate’ ve ‘EndDate’ değerleri olabilir.

**Profil oluşturulan toplam gün sayısı**: Raporun oluşturulduğu başlangıç ile bitiş tarihleri arasında profil oluşturulan toplam gün sayısı.

**Uyumlu sanal makine sayısı**: Gerekli ağ bant genişliği, gerekli depolama hesabı sayısı ve Microsoft Azure çekirdek sayısının hesaplandığı uyumlu sanal makinelerin toplamı.

**Tüm uyumlu sanal makinelerdeki toplam disk sayısı**: Tüm uyumlu sanal makinelerdeki toplam disk sayısıdır.

**Bir uyumlu sanal makinedeki ortalama disk sayısı**: Tüm uyumlu sanal makinelerde hesaplanan ortalama disk sayısıdır.

**Ortalama disk boyutu (GB)**: Tüm uyumlu sanal makinelerde hesaplanan ortalama disk boyutudur.

**İstenen RPO (dakika)**: Varsayılan kurtarma noktası hedefi veya rapor oluşturma sırasında gerekli bant genişliğini tahmin etmek üzere ‘DesiredRPO’ parametresi için geçirilen değer.

**İstenen bant genişliği (Mb/sn)**: Rapor oluşturma sırasında ulaşılabilir RPO’yu tahmin etmek üzere ‘Bandwidth’ parametresi için geçirdiğiniz değerdir.

**Bir günde gözlemlenen tipik veri değişim sıklığı (GB)**: Profil oluşturulan tüm günlerde gözlemlenen ortalama veri değişim sıklığıdır.

## <a name="recommendations"></a>Öneriler  
Hyper-V'den Azure'a dağıtım raporunun öneriler sayfasında, seçilen ve istenen RPO'ya göre aşağıdaki ayrıntılar yer alır:

![Hyper-V'den Azure'a dağıtım raporu için öneriler](media/site-recovery-hyper-v-deployment-planner-analyze-report/Recommendations-h2a.png)

### <a name="profile-data"></a>Profil verileri
![Profil verileri](media/site-recovery-hyper-v-deployment-planner-analyze-report/profile-data-h2a.png)

**Profili oluşturulmuş veri süresi**: Profil oluşturma işleminin gerçekleştirildiği süre. Varsayılan olarak, araç hesaplamadaki tüm profil verilerini içerir. Rapor oluşturma sırasında StartDate ve EndDate seçeneğini kullandıysanız, belirli bir süre için rapor oluşturulur. 

**Profili oluşturulan Hyper-V sunucularının sayısı**: İçerdikleri sanal makinelerin raporu oluşturulan Hyper-V sunucularının sayısı. Hyper-V sunucularının adını görüntülemek için sayıya tıklayın. Bu işlem Şirket İçi Depolama Gereksinimi sayfası açılır ve bu sayfada tüm sunucular, depolama gereksinimleriyle birlikte listelenir.    

**İstenen RPO**: Dağıtımınıza yönelik kurtarma noktası hedefi. Varsayılan olarak, gerekli ağ bant genişliği 15, 30 ve 60 dakikalık RPO değerleri için hesaplanır. Seçim temel alınarak, etkilenen değerler sayfada güncelleştirilir. Raporu oluştururken DesiredRPOinMin parametresini kullandıysanız, değer İstenen RPO sonucunda gösterilir.

### <a name="profiling-overview"></a>Profil oluşturmaya genel bakış
![Profil oluşturmaya genel bakış](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-h2a.png)

**Profili Oluşturulan Toplam Sanal Makine Sayısı**: Profili oluşturulan verileri bulunan sanal makinelerin toplam sayısı. VMListFile dosyasında profili oluşturulmamış sanal makineler varsa, bu sanal makineler rapor oluşturma işleminde göz önünde bulundurulmaz ve profili oluşturulan toplam sanal makine sayısının dışında tutulur.

**Uyumlu Sanal Makineler**: Azure Site Recovery kullanılarak Azure’da korunabilen sanal makine sayısı. Bu sayı, gerekli ağ bant genişliği, depolama hesabı sayısı ve Azure çekirdek sayısının hesaplandığı uyumlu sanal makinelerin toplamıdır. Her uyumlu VM’nin ayrıntıları "Uyumlu VM’ler" bölümünde bulunabilir.

**Uyumsuz Sanal Makineler**: Azure Site Recovery ile koruma için uygun olmayan profili oluşturulmuş sanal makine sayısı. Uyumsuzluğun nedenleri, “Uyumsuz VM’ler” bölümünde belirtilmiştir. VMListFile içinde profili oluşturulmamış bir sanal makinenin adı varsa, bu sanal makineler uyumsuz sanal makine sayısının dışında bırakılır. Bu sanal makineler, “Uyumsuz VM’ler” bölümünün sonunda “Veri bulunamadı” olarak listelenir.

**İstenen RPO**: Dakika cinsinden istediğiniz kurtarma noktası hedefi. Rapor üç RPO değeri için oluşturulur: 15 (varsayılan), 30 ve 60 dakika. Rapordaki bant genişliği önerisi, tablonun sağ üst köşesinde gösterilen İstenen RPO açılır listesindeki seçiminize göre değişir. Raporu özel bir değer ile -DesiredRPO parametresini kullanarak oluşturduysanız, bu özel değer İstenen RPO açılır listesinde varsayılan olarak gösterilir.

### <a name="required-network-bandwidth-mbps"></a>Gerekli ağ bant genişliği (Mb/sn)
![Gerekli ağ bant genişliği](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-network-bandwidth-h2a.png)

**Yüzde 100 RPO süresini karşılamak için:** İstediğiniz yüzde 100 RPO süresini karşılamak için Mb/sn cinsinden ayrılacak önerilen bant genişliğidir. Bu bant genişliği miktarı, herhangi bir RPO ihlalini önlemek üzere tüm uyumlu sanal makinelerinizin kararlı durum delta çoğaltması için ayrılmalıdır.

**Yüzde 90 RPO süresini karşılamak için**: Geniş bant fiyatlandırması veya istediğiniz yüzde 100 RPO süresini karşılamak için gereken bant genişliğini ayarlayamamanız durumunda başka bir nedenle, istediğiniz yüzde 90 RPO süresini karşılayabilen daha düşük bir bant genişliği ayarlamayı seçebilirsiniz. Daha düşük olan bu bant genişliğini ayarlamanın etkilerini anlamak için, raporda beklenen RPO ihlallerinin sayısı ve süresine ilişkin bir ne yapmalı analizi sağlar.

**Elde Edilen Aktarım Hızı**: Depolama hesabının bulunduğu Azure bölgesine GetThroughput komutunu gönderdiğiniz sunucudan aktarım hızıdır. Bu aktarım hızı sayısı, Hyper-V sunucu depolama ve ağ özelliklerinin, aracı çalıştırdığınız sunucudaki özelliklerle aynı kalması şartıyla, Azure Site Recovery kullanarak uyumlu sanal makineleri korurken elde edebileceğiniz tahmini düzeyi gösterir.

Tüm kurumsal Azure Site Recovery dağıtımları için [ExpressRoute](https://aka.ms/expressroute) kullanılması önerilir.

### <a name="required-storage-accounts"></a>Gerekli depolama hesapları
Aşağıdaki grafikte tüm uyumlu sanal makineleri korumak için gereken depolama hesaplarının (standart ve premium) toplam sayısı gösterilmektedir. Her bir VM için kullanılacak depolama hesabını öğrenmek için "VM depolama yerleşimi" bölümüne bakın.

![Gerekli depolama hesapları](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-storage-accounts-h2a.png)

### <a name="required-number-of-azure-cores"></a>Gerekli Azure çekirdek sayısı
Bu sonuç, tüm uyumlu sanal makinelerin yük devretme işlemi ya da yük devretme testi öncesinde ayarlanması gereken toplam çekirdek sayısıdır. Abonelikte çok az sayıda çekirdek varsa, yük devretme testi veya yük devretme sırasında Azure Site Recovery, sanal makineleri oluşturamaz.
![Gerekli Azure çekirdek sayısı](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-number-of-azure-cores-h2a.png)


### <a name="additional-on-premises-storage-requirement"></a>Ek şirket içi depolama gereksinimleri

Sanal makine çoğaltmasının üretim uygulamalarınızda istenmeyen kapalı kalma durumlarına neden olmayacağından emin olacak şekilde başarılı bir ilk çoğaltma ve değişiklik çoğaltması sağlamak için Hyper-V sunucularında gereken toplam boş depolama alanı. Her birim gereksiniminin ayrıntıları, [şirket içi depolama gereksinimleri](#on-premises-storage-requirement) bölümünde sağlanmıştır. 

Çoğaltmada neden boş alan gerektiğini anlamak için, [Şirket için depolama gereksinimleri](#why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication) bölümüne bakın.

![şirket içi depolama gereksinimleri ve kopyalama sıklığı](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-and-copy-frequency-h2a.png)

### <a name="maximum-copy-frequency"></a>En yüksek kopyalama sıklığı
İstenen RPO'yu elde etmek üzere çoğaltma için önerilen en yüksek kopyalama sıklığı ayarlanmalıdır. Varsayılan değer 5 dakikadır. Daha iyi bir RPO elde etmek için 30 saniyelik kopyalama sıklığı ayarlayabilirsiniz.

### <a name="what-if-analysis"></a>Benzetim analiz
![Benzetim analizi](media/site-recovery-hyper-v-deployment-planner-analyze-report/what-if-analysis-h2a.png) Bu analiz, sürenin yalnızca %90’ını karşılamasını istediğiniz RPO için daha düşük bir bant genişliği ayarladığınızda profil oluşturma sırasında kaç tane ihlal oluşabileceğini özetler. Belirli bir günde bir veya daha fazla RPO ihlali ortaya çıkabilir. Graf, günün yoğun RPO değerini gösterir. Bu analizi temel alarak, belirtilen düşük bant genişliği ile tüm günlerdeki RPO ihlali sayısının ve bir gündeki en yoğun RPO gerçekleşme zamanının kabul edilebilir olup olmadığına karar verebilirsiniz. Değer kabul edilebilir düzeydeyse, çoğaltma için düşük bant genişliği ayırabilirsiniz; aksi takdirde, istenilen yüzde 100 RPO süresini karşılamak için önerilen yüksek bant genişliğini ayırmanız gerekir. 

### <a name="recommendation-for-successful-initial-replication"></a>Başarılı ilk çoğaltma önerisi
Bu bölümde, sanal makinelerin korunacağı toplu iş sayısına ve ilk çoğaltmayı (IR) başarıyla tamamlamak için gereken en düşük bant genişliğine yönelik önerilerimizi bulabilirsiniz. 

![IR toplu işlemesi](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-h2a.png)

Sanal makine belirtilen toplu iş sırasıyla korunmalıdır. Her toplu işin belirli bir sanal makine listesi vardır. Toplu İş 1 sanal makineleri Toplu İş 2'den önce korunmalıdır. Toplu İş 2 sanal makineleri Toplu İş 3 sanal makinelerinden önce korunmalıdır ve bu şekilde devam edilmelidir. Toplu İş 1 sanal makinelerinin ilk çoğaltması tamamlanınca, Toplu İş 2 sanal makinelerinin çoğaltmasını etkinleştirebilirsiniz. Benzer biçimde, Toplu İş 2 sanal makinelerinin ilk çoğaltması tamamlanınca, Toplu İş 3 sanal makinelerinin çoğaltmasını etkinleştirebilirsiniz. Toplu iş sırasına uyulmazsa, daha sonra korunan sanal makinelerin ilk çoğaltması için yeterli bant genişliği kalmayabilir. Sonuçta, sanal makineler ilk çoğaltmayı hiç tamamlayamaz veya korunan az sayıda sanal makine yeniden eşitleme moduna geçebilir. Seçilen RPO için IR toplu işleme sayfasında, her toplu işe hangi sanal makinelerin dahil edilmesi gerektiğine ilişkin ayrıntılı bilgi bulunur.

Buradaki grafta, belirlenen toplu iş sırasına göre toplu işlerde ilk çoğaltma ve değişiklik çoğaltması için bant genişliği dağılımı gösterilir. İlk toplu işin sanal makinelerini korurken, ilk çoğaltma için bant genişliğinin tamamı kullanılabilir. İlk toplu işin ilk çoğaltması bittiğinde, bant genişliğinin bir bölümü değişiklik çoğaltması için gerekecektir. Kalan bant genişliği ikinci toplu işteki sanal makinelerin ilk çoğaltmasında kullanılabilir. Toplu İş 2 çubuğu, Toplu İş 1 sanal makineleri için gereken değişiklik çoğaltması bant genişliğini ve Toplu İş 2 sanal makinelerinin ilk çoğaltması için kullanılabilir bant genişliğini gösterir. Benzer biçimde, Toplu İş 3 çubuğu önceki toplu işlerin (Toplu İş 1 ve Toplu İş 2 sanal makineleri) değişiklik çoğaltması için gereken bant genişliğini ve Toplu İş 3'ün ilk çoğaltması için kullanılabilir bant genişliğini gösterir ve bu şekilde devam eder.  Tüm toplu işlerin ilk çoğaltmaları bitince, son çubuk tüm korumalı sanal makinelerin değişiklik çoğaltması için gereken bant genişliğini gösterir. 

**Neden ilk çoğaltma toplu işlemesine ihtiyacım var?**
İlk çoğaltmayı tamamlama süresi VM disk boyutuna, kullanılan disk alanına ve kullanılabilir ağ aktarım hızına bağlıdır. Seçilen RPO için IR toplu işlemesi sayfasında ayrıntılar sağlanır.

### <a name="cost-estimation"></a>Maliyet tahmini
Grafta, seçtiğiniz hedef bölgede ve rapor oluşturma için belirttiğiniz para biriminde Azure'a tahmini toplam olağanüstü durum kurtarma (DR) maliyeti gösterilir.
![Maliyet tahmini özeti](media/site-recovery-hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png) Bu özet Azure Site Recovery kullanarak tüm uyumlu sanal makinelerinizi Azure'da koruduğunuzda ödemeniz gereken depolama, bilgi işlem, ağ ve lisansın maliyetini anlamanıza yardımcı olur. Maliyet, tüm profili oluşturulan sanal makineler için değil uyumlu sanal makineler için hesaplanır.  
 
Aylık veya yıllık maliyeti görüntüleyebilirsiniz. [Desteklenen hedef bölgeler](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) ve [desteklenen para birimleri](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies) hakkında daha fazla bilgi edinin.

**Bileşenlere göre maliyet** Toplam DR dört bileşene bölünür: İşlem, Depolama, Ağ ve Azure Site Recovery lisansı maliyeti. Maliyet, şirket içi siteyle Azure arasında ve yapılandırılan işlem, depolama (premium ve standart) ExpressRoute/VPN ve Azure Site Recovery lisansı için çoğaltma sırasında ve DR tatbikatında tahakkuk ettirilecek tüketim temelinde hesaplanır.

**Durumlara göre maliyet** Toplam olağanüstü durum kurtarma (DR) maliyeti, iki farklı duruma göre kategorilere ayrılır: Çoğaltma ve DR tatbikatı. 

**Çoğaltma maliyeti**: Çoğaltma sırasında tahakkuk ettirilen maliyet. Depolama, ağ ve Azure Site Recovery lisansı maliyetini kapsar. 

**DR Tatbikatı maliyeti**: Yük devretme testi sırasında tahakkuk ettirilen maliyet. Azure Site Recovery, yük devretme testi sırasında sanal makineleri çalıştırır. DR tatbikatı maliyeti, çalıştırılan sanal makinelerin işlem ve depolama maliyetini kapsar. 

**Ay/Yıl başına Azure depolama maliyeti** Çoğaltma ve DR tatbikatının premium ve standart depolaması için tahakkuk ettirilecek toplam depolama maliyetini gösterir.
[Maliyetini Tahmini](site-recovery-hyper-v-deployment-planner-cost-estimation.md) sayfasında VM başına ayrıntılı maliyet analizini görüntüleyebilirsiniz.

### <a name="growth-factor-and-percentile-values-used"></a>Kullanılan büyüme faktörü ve yüzdelik değerler
Tablonun alt kısmındaki bu bölümde, profili oluşturulan sanal makinelerin tüm performans sayaçları için kullanılan yüzdelik dilim değeri (varsayılan değer yüzde 95’lik dilim) ve tüm hesaplamalarda kullanılan büyüme faktörü (varsayılan değer yüzde 30) gösterilmektedir.

![Kullanılan büyüme faktörü ve yüzdelik değerler](media/site-recovery-hyper-v-deployment-planner-run/growth-factor-max-percentile-value.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Girdi olarak kullanılabilir bant genişliği ile ilgili öneriler
![Bant genişliği girdisiyle profil oluşturmaya genel bakış](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-bandwidth-input-h2a.png)

Azure Site Recovery çoğaltması için x MB/sn’den fazla bant genişliği ayarlayamayacağınızı bildiğiniz bir durumla karşılaşabilirsiniz. Araç, kullanılabilir bant genişliğini girmenize (rapor oluşturma sırasında -Bandwidth parametresini kullanarak) ve dakika cinsinden ulaşılabilir RPO değerini almanıza olanak tanır. Bu ulaşılabilir RPO değeriyle ek bant genişliği sağlamanızın gerekli olup olmadığına veya bu RPO ile bir olağanüstü durum kurtarma çözümüne sahip olmak isteyip istemediğinize karar verebilirsiniz.

![Ulaşılabilir RPO](media/site-recovery-hyper-v-deployment-planner-analyze-report/achivable-rpo-h2a.PNG)

## <a name="vm-storage-placement-recommendation"></a>VM-Depolama yerleştirme önerisi 

![VM-Depolama yerleşimi](media/site-recovery-hyper-v-deployment-planner-analyze-report/vm-storage-placement-h2a.png)

**Disk Depolama Türü**: **Yerleştirilecek VM’ler** sütununda bahsedilen tüm ilgili sanal makineleri çoğaltmak için kullanılan standart veya premium depolama hesabıdır.

**Önerilen Ön Ek**: depolama hesabını adlandırmak için kullanılabilecek, üç karakterli bir önerilen ön ektir. Kendi ön ekinizi kullanabilirsiniz, ancak aracın önerisi [depolama hesapları için bölüm adlandırma kuralına](https://aka.ms/storage-performance-checklist) uygundur.

**Önerilen Hesap Adı**: Önerilen ön eki ekledikten sonra depolama hesabı adı. Köşeli ayraç (< ve >) içindeki adı özel girdinizle değiştirin.

**Kayıt Depolama Hesabı:** Tüm çoğaltma kayıtları standart bir depolama hesabında depolanır. Premium depolama hesabına çoğaltılan sanal makineler için günlük depolamaya yönelik ek bir standart depolama hesabı oluşturun. Tek bir standart kayıt depolama hesabı, birden fazla premium çoğaltma depolama hesabı tarafından kullanılabilir. Standart depolama hesaplarına çoğaltılan sanal makineler, kayıtlarla aynı depolama hesabını kullanır.

**Önerilen Günlük Hesabı Adı**: Önerilen ön eki ekledikten sonra depolama günlük hesabı adı. Köşeli ayraç (< ve >) içindeki adı özel girdinizle değiştirin.

**Yerleştirme Özeti**: Çoğaltma ve yük devretme testi veya yük devretme sırasında depolama hesabındaki toplam sanal makine yükünün özeti. Buna depolama hesabıyla eşlenmiş toplam sanal makine sayısı, bu depolama hesabına yerleştirilen tüm sanal makinelerdeki toplam okuma/yazma IOPS sayısı, toplam yazma (çoğaltma) IOPS sayısı, tüm disklerde ayarlanan toplam boyut ve toplam disk sayısı dahildir.

**Yerleştirilecek Sanal Makineler**: En iyi performans ve kullanım için belirli bir depolama hesabına yerleştirilmesi gereken tüm sanal makinelerin listesi.

## <a name="compatible-vms"></a>Uyumlu VM’ler
Azure Site Recovery dağıtım planlayıcısı tarafından oluşturulan Microsoft Excel raporu, "Uyumlu VM'ler" sayfasında tüm uyumlu sanal makinelerin ayrıntılarını sağlar.

![Uyumlu VM’ler](media/site-recovery-hyper-v-deployment-planner-analyze-report/compatible-vms-h2a.png)

**VM Adı**: Rapor oluşturulurken VMListFile içinde kullanılan VM adı. Bu sütunda ayrıca sanal makinelere bağlanan diskler de (VHD) listelenir. Adlar, profil oluşturma sırasında aracın sanal makineleri bulduğu Hyper-V konak adlarını içerir.

**VM Uyumluluğu**: Değerler **Evet** ve **Evet**\* şeklindedir. **Evet**\* değer, VM’nin [Azure Premium Depolama](https://aka.ms/premium-storage-workload) için uygun olduğu örneklere yöneliktir. Burada, profili oluşturulan yüksek değişim sıklığı veya IOPS diski, diske eşlenen boyuttan daha büyük bir premium disk boyutuna sığar. Depolama hesabı, bir diskin boyutuna göre hangi premium depolama disk türüne eşleneceğine karar verir. 
* <128 GB bir P10’dur.
* 128 GB ile 256 GB arası P15'tir
* 256 GB ile 512 GB arası P20'dir.
* 512 GB ile 1024 GB arası P30'dur.
* 1025 GB ile 2048 GB arası P40'dır.
* 2049 GB ile 4095 GB arası P50'dir.

Örneğin, diskin iş yükü özellikleri diski P20 veya P30 kategorisine koyarken boyutu nedeniyle daha düşük bir premium depolama disk türüne eşleniyorsa, araç bu VM’yi **Evet**\* olarak işaretler. Araç ayrıca kaynak disk boyutunu önerilen premium depolama disk türüne uyacak şekilde değiştirmenizi veya hedef disk türünü yük devretme sonrasını değiştirmenizi önerir.

**Depolama Türü**: Standart veya Premium.

**Önerilen Ön Ek**: Üç karakterli depolama hesabı ön ekidir.

**Depolama Hesabı**: Önerilen depolama hesabı ön ekini kullanan ad.

**En Yoğun Okuma/Yazma IOPS (Büyüme Faktörü ile)**: Disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yoğun iş yükü okuma/yazma IOPS değeridir (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun okuma/yazma IOPS değeri profil oluşturma işleminin her dakikası boyunca tek tek disklerin okuma/yazma IOPS değerinin toplamı olduğundan, bir sanal makinenin toplam okuma/yazma IOPS değeri her zaman sanal makinenin tek tek disklerinin okuma/yazma IOPS değerine eşit değildir.

**MB/sn Cinsinden En Yoğun Veri Değişim Sıklığı (Büyüme Faktörü ile)**: Disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yüksek erime oranıdır (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun veri değişim sıklığı, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin veri değişim sıklığı toplamının tepe noktası olduğundan, sanal makinenin toplam veri değişim sıklığı her zaman sanal makinedeki ayrı disklerin veri değişim sıklığının toplamı olmayacaktır.

**Azure VM Boyutu**: Bu şirket içi sanal makine için eşlenen ideal Azure Cloud Services makine boyutudur. Eşleme, şirket içi sanal makinenin belleğine, disk/çekirdek/ağ arabirimi sayısına ve okuma/yazma IOPS değerine bağlıdır. Her zaman şirket içi VM özelliklerinin tümüyle eşleşen en düşük Azure VM boyutunun kullanılması önerilir.

**Disk Sayısı**: Sanal makine üzerindeki disklerin (VHD) toplam sayısı.

**Disk boyutu (GB)**: Sanal makinenin tüm disklerinin toplam boyutu. Araç ayrıca sanal makinedeki ayrı diskler için disk boyutunu gösterir.

**Çekirdek**: Sanal makine üzerindeki CPU çekirdeği sayısı.

**Bellek (MB)**: VM üzerindeki RAM.

**NIC**: VM üzerindeki NIC sayısı.

**Önyükleme Türü**: Sanal makinenin önyükleme türü. BIOS veya EFI olabilir.

## <a name="incompatible-vms"></a>Uyumsuz VM’ler
Azure Site Recovery dağıtım planlayıcısı tarafından oluşturulan Microsoft Excel raporu, "Uyumsuz VM'ler" sayfasında tüm uyumsuz sanal makinelerin ayrıntılarını sağlar.

![Uyumsuz VM’ler](media/site-recovery-hyper-v-deployment-planner-analyze-report/incompatible-vms-h2a.png)

**VM Adı**: Rapor oluşturulurken VMListFile içinde kullanılan VM adı. Bu sütunda ayrıca sanal makinelere bağlanan diskler de (VHD) listelenir. Adlar, profil oluşturma sırasında aracın sanal makineleri bulduğu Hyper-V konak adlarını içerir.

**VM Uyumluluğu**: Belirli bir sanal makinenin, Azure Site Recovery ile kullanım için neden uyumlu olmadığını gösterir. Sanal makinenin her uyumsuz diski için, yayımlanan [depolama sınırlarına](https://aka.ms/azure-storage-scalbility-performance) göre nedenler aşağıdakilerden biri olabilir:

* Disk boyutu > 4095 GB’dir. Azure Depolama şu anda 4095 GB’den büyük veri diski boyutlarını desteklememektedir.

* 1. Nesil (BIOS önyükleme türü) sanal makinesi için işletim sistemi diski > 2047 GB.  Azure Site Recovery, 1. nesil sanal makinelerde 2047 GB'den büyük işletim sistemi disk boyutunu desteklemez.

* 2. Nesil (EFI önyükleme türü) sanal makinesi için işletim sistemi diski > 300 GB. Azure Site Recovery, 2. nesil sanal makinelerde 300 GB'den büyük işletim sistemi disk boyutunu desteklemez.

* Şu “” [] ` karakterlerini içeren VM adları desteklenmez.  Araç, adlarında bu karakterlerden biri bulunan sanal makinelerin profil verilerini alamaz. 

* VHD iki veya daha çok sanal makine tarafından paylaşılıyor. Azure VHD'si paylaşılan sanal makineleri desteklemez.

* Sanal Fiber Kanalı olan sanal makineler desteklenmez. Azure Site Recovery, Sanal Fiber Kanalı olan sanal makineleri desteklemez.

* Hyper-V kümesinde çoğaltma aracısı yok. Azure Site Recovery, küme için Hyper-V çoğaltma aracısı yapılandırılmadıysa, Hyper-V kümesindeki sanal makineyi desteklemez.

* Sanal makine yüksek kullanılabilirliğe sahip değil. Azure Site Recovery, VHD'leri küme diski yerine yerel diskte depolanmış olan Hyper-V küme düğümünün sanal makinesini desteklemez. 

* Toplam VM boyutu (çoğaltma + TFO), desteklenen premium depolama hesabı boyut sınırını (35 TB) aşıyor. Bu uyumsuzluk genellikle sanal makine içindeki tek bir diskin standart depolama için desteklenen Azure veya Azure Site Recovery sınırlarını aşan bir performans özelliği olduğunda gerçekleşir. Bu tür bir örnek, sanal makineyi premium depolama bölgesine iter. Ancak, bir premium depolama hesabı için desteklenen en büyük boyut 35 TB’dir ve tek bir korunan sanal makine birden fazla depolama hesabında korunamaz. Ayrıca, korunan bir sanal makine üzerinde yük devretme testi yürütüldüğünde, yük devretme testi için yönetilmeyen disk yapılandırıldıysa, test ile çoğaltma aynı depolama hesabında devam eder. Bu örnekte, çoğaltmayla aynı miktarda ek depolama alanı gerekir. Bu sayede, çoğaltmanın ilerlemesiyle yük devretme testinin başarıya ulaşması paralel gider. Yük devretme testi için yönetilen disk yapılandırıldıysa, yük devretme testi sanal makinesi için ek alanın hesaba katılması gerekmez.

* Kaynak IOPS, depolama IOPS için disk başına desteklenen 7500 sınırını aşıyor.

* Kaynak IOPS, depolama IOPS için VM başına desteklenen 80.000 limitini aşıyor.

* Ortalama veri değişim sıklığı, disk için ortalama G/Ç’ye yönelik 10 Mb/sn’lik Azure Site Recovery veri değişim sıklığı sınırını aşıyor.

* Algılanan ortalama yazma IOPS değeri, disk için 840 olan desteklenen Azure Site Recovery IOPS sınırını aşıyor.

* Hesaplanan anlık görüntü depolama alanı, 10 TB’lik desteklenen anlık görüntü depolama limitini aşıyor.

**En Yoğun Okuma/Yazma IOPS (Büyüme Faktörü ile)**: Disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yoğun iş yükü IOPS değeridir (varsayılan değer yüzde 95'lik dilim). Sanal makinenin en yoğun okuma/yazma IOPS değeri profil oluşturma işleminin her dakikası boyunca tek disklerin okuma/yazma IOPS değerinin toplamı olduğundan, bir sanal makinenin toplam okuma/yazma IOPS değeri her zaman sanal makinenin ayrı disklerinin okuma/yazma IOPS değerine eşit değildir.

**MB/sn Cinsinden En Yoğun Veri Değişim Sıklığı (Büyüme Faktörü ile)**: Disk üzerinde gelecekteki büyüme faktörünü de (varsayılan değer %30) içeren en yüksek erime oranıdır (varsayılan değer yüzde 95’lik dilim). Sanal makinenin en yoğun veri değişim sıklığı, profil oluşturma döneminin her dakikasında içindeki ayrı disklerin veri değişim sıklığı toplamının tepe noktası olduğundan, sanal makinenin toplam veri değişim sıklığı her zaman sanal makinedeki ayrı disklerin veri değişim sıklığının toplamı olmayacaktır.

**Disk Sayısı**: Sanal makine üzerindeki toplam VHD sayısı.

**Disk boyutu (GB)**: Sanal makinenin tüm disklerinin toplam kurulum boyutu. Araç ayrıca sanal makinedeki ayrı diskler için disk boyutunu gösterir.

**Çekirdek**: Sanal makine üzerindeki CPU çekirdeği sayısı.

**Bellek (MB)**: VM üzerindeki RAM miktarı.

**NIC**: VM üzerindeki NIC sayısı.

**Önyükleme Türü**: Sanal makinenin önyükleme türü. BIOS veya EFI olabilir.

## <a name="azure-site-recovery-limits"></a>Azure Site Recovery limitleri
Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu limitler yaptığımız testleri temel alsa da mümkün olan tüm uygulama G/Ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir. En iyi sonuçlar için, uygulamanın gerçek performans görüntüsünü elde etmek üzere, dağıtım planlamasından sonra bile yük devretme testi kullanılarak her zaman kapsamlı uygulama testleri gerçekleştirilmesi önerilir.
 
**Çoğaltma depolama hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günlük toplam kaynak disk veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB | 2 Mb/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB  | 2 Mb/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 Mb/sn |  Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 Mb/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 Mb/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |10 Mb/sn | Disk başına 842 GB

Bu sınırlar yüzde 30 G/Ç çakışmasını varsayan ortalama sayılardır. Azure Site Recovery; çakışma oranı, büyük yazma boyutları ve gerçek iş yükü G/Ç davranışına göre daha yüksek aktarım hızını işleyebilir. Yukarıdaki sayılar yaklaşık beş dakikalık tipik bir kapsamı varsayar. Diğer bir deyişle, veriler karşıya yüklendikten sonra işlenir ve beş dakika içinde kurtarma noktası oluşturulur.

## <a name="on-premises-storage-requirement"></a>Şirket içi depolama gereksinimi

Çalışma sayfasında başarılı bir ilk çoğaltma ve değişiklik çoğaltması için Hyper-V sunucularının (VHD'lerin durduğu) her birimindeki toplam boş depolama alanı gereksinimi sağlanır. Çoğaltmanın üretim uygulamalarınızda istenmeyen kapalı kalma durumlarına neden olmaması için, çoğaltmayı etkinleştirmeden önce birimlere gerekli depolama alanını ekleyin. 

Azure Site Recovery dağıtım planlayıcısı, çoğaltmada kullanılan VHD'lerin boyutu ve ağ bant genişliği temelinde en uygun depolama alanı gereksinimini belirler.

![şirket içi depolama gereksinimi](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-requirement-h2a.png)

### <a name="why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication"></a>Çoğaltma için neden Hyper-V sunucusunda boş alana ihtiyacım var?
* Sanal makinenin çoğaltmasını etkinleştirdiğinizde, Azure Site Recovery ilk çoğaltma (IR) için sanal makinenin her VHD'sinin anlık görüntüsünü alır. İlk çoğaltma devam ederken, uygulama tarafından disklere yeni değişiklikler yazılır. Azure Site Recovery bu değişiklikleri günlük dosyalarında izler ve bu da ek depolama alanı gerektirir.  İlk çoğaltma tamamlanana kadar günlük dosyaları yerel olarak depolanır. Günlük dosyaları ve anlık görüntü (AVHDX) için yeterli alan yoksa, çoğaltma yeniden eşitleme moduna geçer ve hiçbir zaman tamamlanmaz. En kötü durumda, ilk çoğaltma için VHD boyutunun %100'ü kadar ek boş alana ihtiyacınız olur.
* İlk çoğaltma bittikten sonra değişiklik çoğaltması başlatılır. Azure Site Recovery bu değişiklikleri günlük dosyalarında izler; bu dosyalar sanal makinenin VHD'lerinin durduğu birimde depolanır. Bu günlük dosyaları, yapılandırılan kopyalama sıklığında Azure'a çoğaltılır. Kullanılabilir ağ bant genişliğine göre, günlük dosyalarının Azure'a çoğaltılması biraz zaman alır. Günlük dosyalarını depolamak için yeterli boş alan yoksa, çoğaltma duraklatılır ve sanal makinenin çoğaltma durumu yeniden eşitleme gerekiyor durumuna geçer.
* Günlük dosyalarını Azure'a göndermek için ağ bant genişliği yeterli değilse, günlük dosyaları birimde birikir. En kötü durum senaryosunda, günlük dosyalarının boyutu VHD boyutunun %50'sini aştığında, sanal makinenin çoğaltması yeniden eşitleme gerekiyor durumuna geçer. En kötü durumda, değişiklik çoğaltması için VHD boyutunun %50'si kadar ek boş alana ihtiyacınız olur.

**Hyper-V konağı**: Profili oluşturulan Hyper-V sunucularının listesi. Sunucu bir Hyper-V kümesinin parçasıysa, tüm küme düğümleri birlikte gruplandırılır.

**Birim (VHD yolu)**: VHD/VHDX'lerin bulunduğu Hyper-V konağının her birimi. 

**Kullanılabilir boş alan (GB)**: Birimde kullanılabilir olan boş alan.

**Birimde gereken toplam depolama alanı (GB)**: Başarılı bir ilk çoğaltma ve değişiklik çoğaltması için birimde bulunması gereken toplam boş depolama alanı. 

**Başarılı bir çoğaltma için birimde sağlanacak toplam ek depolama alanı**: Başarılı bir ilk çoğaltma ve değişiklik çoğaltması için birimde sağlanması gereken toplam ek alan için öneride bulunur.

## <a name="initial-replication-batching"></a>İlk çoğaltma toplu işlemesi 

### <a name="why-do-i-need-initial-replication-ir-batching"></a>Neden ilk çoğaltma (IR) toplu işlemesine ihtiyacım var?
Sanal makinelerin tümü aynı anda korunursa, boş depolama alanı gereksinimi çok daha fazla olabilir ve yeterli depolama yoksa, sanal makinelerin çoğaltması yeniden eşitleme moduna geçer. Aynı zamanda, tüm sanal makinelerin ilk çoğaltmasını başarıyla tamamlamak için gereken ağ bant genişliği çok daha yüksek olabilir. 

### <a name="initial-replication-batching-for-a-selected-rpo"></a>Seçilen RPO için ilk çoğaltma toplu işlemesi
Bu çalışma sayfası ilk çoğaltma (IR) için her toplu işin ayrıntılı görünümünü sağlar. Her RPO için, ayrı bir IR toplu işleme sayfası oluşturulur. 

Her birim için şirket içi depolama gereksinimleri önerisine uyduğunuzda, çoğaltmanız gereken ana bilgiler, paralel olarak korunabilecek sanal makinelerin listesidir. Bu sanal makineler bir toplu işte birlikte gruplandırılır. Birden çok toplu iş olabilir. Sanal makineleri verilen toplu iş sırasına göre korumalısınız. İlk olarak Toplu İş 1 sanal makinelerini koruyun ve ilk çoğaltma tamamlanınca Toplu İş 2 sanal makinelerini koruyun ve bu şekilde devam edin. Bu sayfada toplu iş listesini ve bunlara karşılık gelen sanal makineleri görebilirsiniz. 

![IR toplu işleme ayrıntıları1](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo-h2a.png)
![IR toplu işleme ayrıntıları2](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo2-h2a.png)

### <a name="each-batch-provides-the-following-information"></a>Her toplu iş şu bilgileri sağlar 
**Hyper-V konağı**: Korunacak sanal makinenin Hyper-V konağı.
Sanal makine: Korunacak olan sanal makine. 

**Açıklamalar**: Sanal makinenin herhangi bir birimi için herhangi bir eylem gerekiyorsa, burada açıklama sağlanır.  Örneğin birimde yeterli boş alan yoksa, açıklamada Bu sanal makineyi korumak için depolama alanı ekleyin ifadesi yer alır.

**Birim(VHD yolu)**: Sanal makinenin VHD'lerinin durduğu birimin adı. Birimdeki kullanılabilir boş alan (GB): Birimde sanal makine için kullanılabilen boş disk alanı. Birimlerdeki kullanılabilir boş alan hesaplanırken, VHD'leri aynı birimde yer alan önceki toplu işlerin sanal makineleri tarafından değişiklik çoğaltması için kullanılan disk alanını hesaba katar.  

Örneğin, VM1, VM2 ve VM3 sanal makinelerinin E:\VHDyolu yolunda bulunduğunu varsayalım. Çoğaltma öncesinde, birimdeki boş alan 500 GB'dir. VM1 Toplu İş 1, VM2 Toplu İş 2 ve VM3 de Toplu İş 3'ün parçasıdır.  VM1 için, kullanılabilir boş alan 500 GB olur. VM2 için, kullanılabilir boş alan 500 – VM1'in değişiklik çoğaltması için gereken disk alanı olur.  VM1'in değişiklik çoğaltması için 300 GB gerektiğini varsayarsak, VM2'nin kullanılabilir boş alanı 500 GB – 300 GB = 200 GB olacaktır.  Benzer biçimde, VM2'ye değişiklik çoğaltması için 300 GB gerektiğini varsayalım. VM3 için kullanılabilir boş alan 200 GB - 300 GB = -100 GB olur.

**İlk çoğaltma için birimde gereken depolama alanı (GB)**: Sanal makinenin ilk çoğaltması için birimde gereken boş depolama alanı.

**Değişiklik çoğaltması için birimde gereken depolama alanı (GB)**: Sanal makinenin değişiklik çoğaltmasısı için birimde gereken boş depolama alanı.

**Çoğaltmanın başarısız olmasını önlemek için eksiklik temelinde gereken ek depolama alanı (GB)**: Sanal makine için birimde gereken ek depolama alanı.  Bu, ilk çoğaltma ile değişiklik çoğaltmasının en yüksek depolama alanı gereksinimi - birimdeki kullanılabilir boş alana eşittir.

**İlk çoğaltma için gereken en düşük bant değişliği (Mb/sn)**: Sanal makinenin ilk çoğaltması için gereken en düşük bant genişliği.

**Değişiklik çoğaltması için gereken en düşük bant genişliği (Mb/sn)**: Sanal makinenin değişiklik çoğaltması için gereken en düşük bant genişliği.

### <a name="network-utilization-details-for-each-batch"></a>Her toplu iş için ağ kullanım ayrıntıları 
Her toplu iş tablosunda, toplu işin ağ kullanımının özeti sağlanır.

**Toplu İş için kullanılabilir bant genişliği**: Önceki toplu işin değişiklik çoğaltması bant genişliği dikkate alındıktan sonra, toplu iş için kullanılabilir bant genişliği.

**Toplu işin ilk çoğaltması için kullanılabilir yaklaşık bant genişliği**: Toplu işteki sanal makinelerin ilk çoğaltması için kullanılabilir bant genişliği. 

**Toplu işin değişiklik çoğaltması için kullanılan yaklaşık bant genişliği**: Toplu işteki sanal makinelerin değişiklik çoğaltması için gereken bant genişliği. 

**Toplu İş için tahmini İlk Çoğaltma süresi (SS:DD)**: Saat:Dakika cinsinden tahmini ilk çoğaltma süresi.

## <a name="cost-estimation"></a>Maliyet tahmini
[Maliyet tahmini](site-recovery-hyper-v-deployment-planner-cost-estimation.md) hakkında daha fazla bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar
[Maliyet tahmini](site-recovery-hyper-v-deployment-planner-cost-estimation.md) hakkında daha fazla bilgi edinin.