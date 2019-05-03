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
ms.date: 04/03/2019
ms.openlocfilehash: a5d93a77652f540fde44b33963b13df04b45ecee
ms.sourcegitcommit: 60606c5e9a20b2906f6b6e3a3ddbcb6c826962d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64987936"
---
# <a name="upgrade-your-data-warehouse-to-gen2"></a>Veri ambarınız için Gen2'ye yükseltme

Microsoft, sürücü giriş düzeyi bir veri ambarı'nı çalıştırmanın maliyeti aşağı yardımcı oluyor.  Alt sorgular zorlu işleyebilmesini katmanları Azure SQL veri ambarı için kullanılabilir işlem. Duyurunun tamamını okuyun [alt işlem katmanı desteği Gen2](https://azure.microsoft.com/blog/azure-sql-data-warehouse-gen2-now-supports-lower-compute-tiers/). Yeni bir teklif, aşağıdaki tabloda belirtildiği bölgelerinde kullanılabilir. Desteklenen bölgeler için mevcut Gen1 veri ambarları için Gen2 üzerinden yükseltilebilir:

- **Otomatik yükseltme işlemi:** Hizmet bir bölgede kullanılabilir hemen sonra otomatik yükseltmeler başlamaz.  Otomatik yükseltmeler belirli bir bölgede başlattığınızda, tek tek DW yükseltmeleri sırasında seçilen bakım zamanlamanızı gerçekleşir.
- [**2. nesil için kendi kendine yükseltme:**](#self-upgrade-to-gen2) 2. nesil için kendi kendine yükseltme yaparak yükseltme zamanı denetleyebilirsiniz. Bölgeniz henüz desteklenmiyor, desteklenen bir bölgede bir Gen2 örneğine doğrudan bir geri yükleme noktasından geri yükleyebilirsiniz.

## <a name="automated-schedule-and-region-availability-table"></a>Otomatik zamanlamayı ve bölge kullanılabilirliği tablosu

Aşağıdaki tabloda, bölgeye göre daha düşük 2. nesil işlem katmanını kullanıma sunulacak ve otomatik yükseltmeler başlattığınızda özetler. Tarihleri değişikliğe tabi olduğu. Bölgenizde kullanılabilir duruma geldiğinde tekrar görmek için kontrol edin.

\* belirli bir zamanlama bölge için şu anda kullanılamıyor gösterir.

| **Bölge** | **Daha düşük Gen2 kullanılabilir** | **Otomatik yükseltmeler başlayın** |
|:--- |:--- |:--- |
| Avustralya Doğu |Kullanılabilir |1 Haziran 2019 |
| Avustralya Güneydoğu |Kullanılabilir |1 Mayıs 2019 |
| Güney Brezilya |Kullanılabilir |1 Haziran 2019 |
| Orta Kanada |Kullanılabilir |1 Haziran 2019 |
| Doğu Kanada |\* |\* |
| Orta ABD |Kullanılabilir |1 Haziran 2019 |
| Çin Doğu |\* |\* |
| Çin Doğu 2 |\* |\* |
| Çin Kuzey |\* |\* |
| Çin Kuzey 2 |\* |\* |
| Doğu Asya |Kullanılabilir |1 Haziran 2019 |
| Doğu ABD |Kullanılabilir |1 Haziran 2019 |
| Doğu ABD 2 |Kullanılabilir |1 Haziran 2019 |
| Fransa Orta |\* |1 Haziran 2019 |
| Almanya Orta |\* |\* |
| Almanya Orta Batı |1 Eylül 2019|2 Ocak 2020 |
| Hindistan Orta |Kullanılabilir |1 Haziran 2019 |
| Hindistan Güney |Kullanılabilir |1 Haziran 2019 |
| Japonya Doğu |Kullanılabilir |1 Haziran 2019 |
| Japonya Batı |Kullanılabilir |1 Mayıs 2019 |
| Kore Orta |Kullanılabilir |1 Haziran 2019 |
| Kore Güney |Kullanılabilir |1 Mayıs 2019 |
| Orta Kuzey ABD |Kullanılabilir |1 Mayıs 2019 |
| Kuzey Avrupa |Kullanılabilir |1 Haziran 2019 |
| Orta Güney ABD |Kullanılabilir |1 Haziran 2019 |
| Güneydoğu Asya |Kullanılabilir |1 Haziran 2019 |
| Birleşik Krallık Güney |Kullanılabilir, 2019 |1 Haziran 2019 |
| Birleşik Krallık Batı |\*|\* |
| Batı Orta ABD |2 Eylül 2019 |2 Ocak 2020|
| Batı Avrupa |Kullanılabilir |1 Haziran 2019 |
| Batı ABD |Kullanılabilir |1 Haziran 2019 |
| Batı ABD 2 |Kullanılabilir |1 Haziran 2019 |

## <a name="automatic-upgrade-process"></a>Otomatik yükseltme işlemi

Yukarıdaki kullanılabilirlik grafiğinde bağlı olarak, biz otomatik yükseltmeler Gen1 örnekleriniz için zamanlama. Veri ambarının kullanılabilirliğine herhangi beklenmeyen kesintiler önlemek için otomatik yükseltmeler sırasında bakım zamanlamanızı zamanlanacak. Gen2'ye otomatik yükseltilen bölgede yeni bir Gen1 örneği oluşturma özelliği devre dışı bırakılır. Zamanlama hakkında daha fazla bilgi için bkz. [bakım zamanlaması görüntüleyin](viewing-maintenance-schedule.md)

Biz, veri ambarı yeniden başlatmanız gibi yükseltme işlemi bağlantısı (yaklaşık 5 dakika) kısa bir bırakma içerir.  Veri ambarınız yeniden başlatıldıktan sonra tam olarak kullanılmaya hazır olacaktır. Ancak, arka planda veri dosyalarını yükseltmek yükseltme işlemi devam ederken, aslında performansta düşüş karşılaşabilirsiniz. Performans düşüşü için toplam süreyi, veri dosyalarının boyutuna bağlı olarak değişir.

Çalıştırarak veri dosyası yükseltme sürecini hızlandırabilir [Alter Index yeniden](sql-data-warehouse-tables-index.md) yeniden başladıktan sonra daha büyük bir SLO ve kaynak sınıfı kullanarak tüm birincil columnstore tabloları.

> [!NOTE]
> ALTER INDEX yeniden çevrimdışı bir işlemdir ve tabloları yeniden tamamlanana kadar kullanılamaz.

## <a name="self-upgrade-to-gen2"></a>Kendi kendine için Gen2'ye yükseltme

Var olan bir Gen1 veri ambarı üzerinde aşağıdaki adımları izleyerek kendi kendine yükseltmek seçebilirsiniz. Kendi kendine yükseltmeyi seçerseniz bölgenizde otomatik yükseltme işlemine başlamadan önce tamamlamalısınız. Bunun yapılması, çakışmaya neden otomatik yükseltmeleri riskini önlemek sağlar.

Kendi kendine yükseltme yapılırken iki seçenek vardır.  Ya da, geçerli veri ambarı yerinde yükseltme yapabilirsiniz veya bir Gen1 veri ambarı Gen2 örneğine geri yükleyebilirsiniz.

- [Yerinde yükseltme](upgrade-to-latest-generation.md) -bu seçenek mevcut Gen1 veri ambarınız için Gen2'ye yükseltir. Biz, veri ambarı yeniden başlatmanız gibi yükseltme işlemi bağlantısı (yaklaşık 5 dakika) kısa bir bırakma içerir.  Veri ambarınız yeniden başlatıldıktan sonra tam olarak kullanılmaya hazır olacaktır. Yükseltme sırasında sorunlarla karşılaşırsanız, açık bir [destek isteği](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) ve "Gen2'ye yükseltme" olası nedeni olarak başvuru.
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

- Y: Evet.

**S: Yükseltmeler nasıl my Otomasyon betikleri etkiler mi?**

- Y: Bir hizmet düzeyi amacı başvuran tüm otomasyon betiği Gen2'ye denk karşılık gelecek şekilde değiştirilmelidir.  Ayrıntıları görmek [burada](upgrade-to-latest-generation.md#sign-in-to-the-azure-portal).

**S: Nasıl kendi kendine yükseltme normalde kadar sürer?**

- Y: Yerinde yükseltme veya geri yükleme noktasından yükseltin.  
   - Yerinde yükseltme, veri ambarınızın anlık olarak duraklatma ve sürdürme neden olur.  Veri ambarı çevrimiçi durumdayken arka plan işlemi devam eder.  
   - Yükseltme tam geri yükleme işleminde geçer çünkü bir geri yükleme noktası yükseltiyorsanız uzun sürer.

**S: Otomatik yükseltme ne kadar sürer?**

- Y: Gerçek kapalı kalma süresi yükseltme için yalnızca, duraklatma ve sürdürme 5-10 dakika arasında hizmet için gereken zamanı gelmiştir. Kısa kapalı kalma süresini sonra bir arka plan işlemi, depolama geçişi çalışır. Arka plan işlemi süreyi veri Ambarınızı boyutuna bağlıdır.

**S: Ne zaman bu otomatik yükseltme gerçekleşir?**

- Y: Bakım zamanlaması sırasında. Seçilen bakım zamanlamanızı yararlanarak işletmenizi aksamasıyla en aza indirirsiniz.

**S: Yükseltme işlemi arka planda takılmış gibi görünüyor. varsa ne yapmalıyım?**

 - Y: Columnstore tabloları'bir reindex tetiklersiniz. Tablosu, ancak bu işlem sırasında çevrimdışı olacağını unutmayın.

**S: Hizmet düzeyi hedefi varsa ne Gen2 yok üzerinde Gen1 mı var?**
- Y: DW600 veya DW1200 Gen1 üzerinde çalıştırıyorsanız, 2. nesil daha fazla bellek, kaynakları ve Gen1 daha yüksek performans sağlar. bu yana DW500c veya DW1000c sırasıyla kullanmak için önerilir.

**S: Coğrafi yedekleme devre dışı bırakabilirim?**
- Y: Hayır. Coğrafi yedekleme, verilerinizi korumak için bir kurumsal özellik bir bölge kullanılamaz hale gelmesi durumunda kullanılabilirlik ambar ' dir. Açık bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) daha fazla ilgili endişeleriniz varsa.

**S: T-SQL söz dizimini Gen1 ve 2. nesil arasında bir fark var mı?**

- Y: Gen2'ye Gen1 öğesinden T-SQL dil sözdiziminde bir değişiklik yoktur.

**S: Gen2 bakım Windows destekliyor mu?**

- Y: Evet.

**S: My bölge yükseltildikten sonra yeni bir Gen1 örneği oluşturabilir kalacağım?**

- Y: Hayır. Bir bölge yükseltildikten sonra yeni Gen1 örnekleri oluşturma devre dışı bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Yükseltme adımları](upgrade-to-latest-generation.md)
- [Bakım pencereleri](maintenance-scheduling.md)
- [Kaynak sistem durumu İzleyicisi](https://docs.microsoft.com/azure/service-health/resource-health-overview)
- [Bir Geçişe başlamadan önce gözden geçirin](upgrade-to-latest-generation.md#before-you-begin)
- [Yerinde yükseltme ve geri yükleme noktasından yükseltme](upgrade-to-latest-generation.md)
- [Bir kullanıcı tanımlı bir geri yükleme noktası oluştur](sql-data-warehouse-restore.md#restore-through-the-azure-portal)
- [2. nesil için geri yüklemeyi öğreneceksiniz](sql-data-warehouse-restore.md#restore-an-active-or-paused-database-using-the-azure-portal)
- [SQL veri ambarı destek isteği açın](https://go.microsoft.com/fwlink/?linkid=857950)
