---
title: Mevcut Azure SQL veri ambarı Gen2'ye için geçirme | Microsoft Docs
description: Var olan bir veri ambarı Gen2'ye ve geçiş için geçirilmesine yönelik yönergeler, bölgeye göre planlayın.
services: sql-data-warehouse
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.topic: article
ms.date: 02/09/2019
ms.openlocfilehash: a3ec74d0b22bfafb4353eca400b389b07a58ba39
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246361"
---
# <a name="upgrade-your-data-warehouse-to-gen2"></a>Veri ambarınız için Gen2'ye yükseltme
Duyurunun tamamını okuyun sürücü giriş düzeyi sorgular Azure SQL veri ambarı için alt bilgi işlem katmanı ekleyerek yoğun işleme yeteneğine sahip bir veri ambarı'nı çalıştırmanın maliyeti aşağı Microsoft yardımcı olma [alt işlem katmanı desteği Gen2](https://azure.microsoft.com/blog/azure-sql-data-warehouse-gen2-now-supports-lower-compute-tiers/). Yeni bir teklif zaten aşağıdaki tabloda belirtildiği bölgelerinde kullanılabilir. Desteklenen bölgeler için mevcut Gen1 veri ambarları için Gen2 üzerinden yükseltilebilir:
- **Otomatik yükseltme işlemi:** Hizmet bir bölgede kullanılabilir hemen sonra otomatik yükseltmeler başlatmayın.  Otomatik yükseltmeler belirli bir bölgede başlattığınızda, tek tek DW yükseltmeleri sırasında seçilen bakım zamanlamanızı gerçekleşir. 
- **2. nesil için kendi kendine yükseltme:** Yükseltme zamanı denetlemek isterseniz, bir kendi kendine yükseltme Gen2'ye gerçekleştirebilirsiniz. Bölgeniz henüz desteklenmiyor, desteklenen ve ardından bir kendi kendine yükseltme Gen 2 gerçekleştirin bir bölgeye, DW geçirebilirsiniz.

## <a name="automated-schedule-and-region-availability-table"></a>Otomatik zamanlamayı ve bölge kullanılabilirliği tablosu
Aşağıdaki tabloda, bölgeye göre daha düşük 2. nesil işlem katmanını kullanıma sunulacak ve otomatik yükseltmeler başlattığınızda özetler. Tarihleri değişikliğe tabi olduğu. Bölgenizde kullanılabilir duruma geldiğinde tekrar görmek için kontrol edin.

\* belirli bir zamanlama bölge için şu anda kullanılamıyor gösterir.

| **Bölge** | **Daha düşük Gen2 kullanılabilir** | **Otomatik yükseltmeler başlayın** |
|:--- |:--- |:--- |
| Avustralya Orta 1 |1 Mart 2019 |15 Haziran 2019 |
| Avustralya Orta 2 |1 Mart 2019 |15 Haziran 2019 |
| Avustralya Doğu |Kullanılabilir |1 Mayıs 2019 |
| Avustralya Güneydoğu |1 Mart 2019 |15 Haziran 2019 |
| Güney Brezilya |\* |\* |
| Orta Kanada |Kullanılabilir |1 Mayıs 2019 |
| Doğu Kanada |\* |\* |
| Orta ABD |Kullanılabilir |1 Mayıs 2019 |
| Çin Doğu |\* |\* |
| Çin Kuzey 1 |\* |\* |
| Doğu Asya |Kullanılabilir |1 Mayıs 2019 |
| Doğu ABD 1 |Kullanılabilir |16 Mart 2019 |
| Doğu ABD 2 |Kullanılabilir |16 Mart 2019 |
| Fransa Orta |1 Mart 2019 |1 Mayıs 2019 |
| Almanya Orta |\* |\* |
| Hindistan Orta |Kullanılabilir |1 Mayıs 2019 |
| Hindistan Güney 1 |1 Mart 2019 |15 Haziran 2019 |
| Japonya Doğu |Kullanılabilir |1 Mayıs 2019 |
| Japonya Batı |Kullanılabilir |15 Haziran 2019 |
| Kore Orta |1 Mart 2019 |1 Mayıs 2019 |
| Kore Güney 1 |1 Mart 2019 |15 Haziran 2019 |
| Orta Kuzey ABD |1 Mart 2019 |15 Haziran 2019 |
| Kuzey Avrupa |Kullanılabilir |16 Mart 2019 |
| Orta Güney ABD |Kullanılabilir |1 Mayıs 2019 |
| Güneydoğu Asya |Kullanılabilir |16 Mart 2019 |
| Birleşik Krallık Güney |1 Mart 2019 |1 Mayıs 2019 |
| UK Batı 1 |1 Mart 2019 |15 Haziran 2019 |
| Batı Orta ABD |\* |\* |
| Batı Avrupa |Kullanılabilir |16 Mart 2019 |
| Batı ABD 1 |1 Mart 2019 |15 Haziran 2019 |
| Batı ABD 2 |Kullanılabilir |16 Mart 2019 |

## <a name="automatic-upgrade-process"></a>Otomatik yükseltme işlemi

16 Mart 2019 başlangıç Gen1 örnekleriniz için otomatik yükseltmeler zamanlama başlayacağız. Veri ambarının kullanılabilirliğine herhangi beklenmeyen kesintiler önlemek için otomatik yükseltmeler sırasında bakım zamanlamanızı zamanlanacak. Zamanlama hakkında daha fazla bilgi için bkz. [bakım zamanlaması görüntüleyin](viewing-maintenance-schedule.md)

Biz, veri ambarı yeniden başlatmanız gibi yükseltme işlemi bağlantısı (yaklaşık 5 dakika) kısa bir bırakma içerir.  Veri ambarınız yeniden başlatıldıktan sonra ancak kullanım için tam olarak kullanılabilir olacaktır, arka planda veri dosyalarını yükseltmek yükseltme işlemi devam ederken, aslında performansta düşüş karşılaşabilirsiniz. Performans düşüşü için toplam süreyi, veri dosyalarının boyutuna bağlı olarak değişir.

Çalıştırarak veri dosyası yükseltme sürecini hızlandırabilir [Alter Index yeniden](sql-data-warehouse-tables-index.md) yeniden başladıktan sonra daha büyük bir SLO ve kaynak sınıfı kullanarak tüm birincil columnstore tabloları.

> [!NOTE]
> ALTER INDEX yeniden çevrimdışı bir işlemdir ve tabloları yeniden tamamlanana kadar kullanılamaz.

## <a name="self-upgrade-to-gen2"></a>Kendi kendine için Gen2'ye yükseltme

İsteğe bağlı olarak, şimdi yükseltmek istiyorsanız, var olan bir Gen1 veri ambarı üzerinde aşağıdaki adımları izleyerek kendi kendine yükseltme seçebilirsiniz. Bu seçeneği seçerseniz, bu bölgede otomatik yükseltme işlemine başlamadan önce yükseltme Self'tamamlamanız gerekir. Bu, herhangi bir çakışmaya neden otomatik yükseltmeleri riskini önlemek sağlar.

Kendi kendine yükseltme yapılırken iki seçenek vardır.  Ya da, geçerli veri ambarı yerinde yükseltme yapabilirsiniz veya bir Gen1 veri ambarı Gen2 örneğine geri yükleyebilirsiniz.

- [Yerinde yükseltme](upgrade-to-latest-generation.md) -bu seçenek mevcut Gen1 veri ambarınız için Gen2'ye yükseltir. Biz, veri ambarı yeniden başlatmanız gibi yükseltme işlemi bağlantısı (yaklaşık 5 dakika) kısa bir bırakma içerir.  Veri ambarınız yeniden başlatıldıktan sonra tam olarak kullanılmaya hazır olacaktır. Herhangi bir nedenle, bir Gen2 örneğinden geri Gen1 için geri yüklemeniz gerekirse, açık bir [destek isteği](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket).
- [Geri yükleme noktasından yükseltme](sql-data-warehouse-restore.md) - geçerli Gen1 veri ambarınıza bir kullanıcı tanımlı bir geri yükleme noktası oluşturma ve sonra da doğrudan bir Gen2 örneğine geri yükleyin. Var olan Gen1 veri ambarı yerinde kalır. Geri yükleme tamamlandıktan sonra Gen2 veri Ambarınızı tamamen kullanılabilir olacaktır.  Geri yüklenen Gen2 örneğinde tüm sınama ve doğrulama işlemleri gerçekleştirdikten sonra özgün Gen1 örneği silinebilir.
    - 1. Adım: Azure portalından [bir kullanıcı tanımlı bir geri yükleme noktası oluşturma](sql-data-warehouse-restore.md#create-a-user-defined-restore-point-using-the-azure-portal).
    - 2. Adım: Öğesinden geri yüklenirken kullanıcı tanımlı bir geri yükleme noktası, "performans düzeyi" tercih edilen Gen2 katmanınızı ayarlayın.

Arka planda veri dosyalarını yükseltmek yükseltme işlemi devam ederken, bir süre içinde performans düşüşü karşılaşabilirsiniz. Performans düşüşü için toplam süreyi, veri dosyalarının boyutuna bağlı olarak değişir.

Arka plan veri geçişi işlemini hızlandırmak için hemen veri taşıma çalıştırarak zorlayabilirsiniz [Alter Index yeniden](sql-data-warehouse-tables-index.md) tüm birincil columnstore tabloları, daha büyük bir SLO ve kaynak sınıfı sorgulama.

> [!NOTE]
> ALTER INDEX yeniden çevrimdışı bir işlemdir ve tabloları yeniden tamamlanana kadar kullanılamaz.

Veri Ambarınızda herhangi bir sorunla karşılaşırsanız oluşturma bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) ve "Gen2'ye yükseltme" olası nedeni olarak başvuru.

Daha fazla bilgi için [yükseltmek için 2. nesil](upgrade-to-latest-generation.md).

## <a name="migration-frequently-asked-questions"></a>Geçiş hakkında sık sorulan sorular

**S: Gen2 Gen1 aynı maliyeti?**
- C: Evet.

**S: Yükseltmeler nasıl my Otomasyon betikleri etkiler mi?**
- C: Bir hizmet düzeyi amacı başvuran tüm otomasyon betiği Gen2'ye denk karşılık gelecek şekilde değiştirilmelidir.  Ayrıntıları görmek [burada](upgrade-to-latest-generation.md#sign-in-to-the-azure-portal).

**S: Nasıl kendi kendine yükseltme normalde kadar sürer?**
- C: Yerinde yükseltme veya geri yükleme noktasından yükseltin.  
   - Yerinde yükseltme, veri ambarınızın anlık olarak duraklatma ve sürdürme neden olur.  Veri ambarı çevrimiçi durumdayken arka plan işlemi devam eder.  
   - Yükseltme tam geri yükleme işleminde geçer çünkü bir geri yükleme noktası yükseltiyorsanız uzun sürer.

**S: Otomatik yükseltme ne kadar sürer?**
- C: Gerçek kapalı kalma süresi yükseltme için yalnızca, duraklatma ve sürdürme 5-10 dakika arasında hizmet için gereken zamanı gelmiştir. Kısa kapalı kalma süresini sonra bir arka plan işlemi, depolama geçişi çalışır. Arka plan işlemi süreyi veri Ambarınızı boyutuna bağlıdır.

**S: Ne zaman bu otomatik yükseltme gerçekleşir?**
- C: Bakım zamanlaması sırasında. Seçilen bakım zamanlamanızı yararlanarak işletmenizi aksamasıyla en aza indirirsiniz.

**S: Yükseltme işlemi arka planda takılmış gibi görünüyor. varsa ne yapmalıyım?**
 - C: Columnstore tabloları'bir reindex tetiklersiniz. Tablosu, ancak bu işlem sırasında çevrimdışı olacağını unutmayın.

**S: Hizmet düzeyi hedefi varsa ne Gen2 yok üzerinde Gen1 mı var?**
- C: DW600 veya DW1200 Gen1 üzerinde çalıştırıyorsanız, 2. nesil daha fazla bellek, kaynakları ve Gen1 daha yüksek performans sağlar. bu yana DW500c veya DW1000c sırasıyla kullanmak için önerilir.

**S: Coğrafi yedekleme devre dışı bırakabilirim?**
- C: Hayır. Coğrafi yedekleme, verilerinizi korumak için bir kurumsal özellik bir bölge kullanılamaz hale gelmesi durumunda kullanılabilirlik ambar ' dir. Açık bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) daha fazla ilgili endişeleriniz varsa.

**S: T-SQL söz dizimini Gen1 ve 2. nesil arasında bir fark var mı?**
- C: Gen2'ye Gen1 öğesinden T-SQL dil sözdiziminde bir değişiklik yoktur.

**S: Gen2 bakım Windows destekliyor mu?**
- C: Evet.

**S: My bölge yükseltildikten sonra yeni bir Gen1 örneği oluşturabilir kalacağım?**
- C: Hayır. Bir bölge yükseltildikten sonra yeni Gen1 örnekleri oluşturma devre dışı bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Yükseltme adımları](upgrade-to-latest-generation.md)
- [Bakım pencereleri](maintenance-scheduling.md)
- [Kaynak sistem durumu İzleyicisi](https://docs.microsoft.com/azure/service-health/resource-health-overview)
- [Bir Geçişe başlamadan önce gözden geçirin](upgrade-to-latest-generation.md#before-you-begin)
- [Yerinde yükseltme ve geri yükleme noktasından yükseltme](upgrade-to-latest-generation.md)
- [Bir kullanıcı tanımlı bir geri yükleme noktası oluştur](sql-data-warehouse-restore.md#restore-through-the-azure-portal)
- [2. nesil için geri yüklemeyi öğreneceksiniz](sql-data-warehouse-restore.md#restore-an-active-or-paused-database-using-the-azure-portal)
- [SQL veri ambarı destek isteği açın](https://go.microsoft.com/fwlink/?linkid=857950)