---
title: Azure SQL Elastik Veritabanı İşleri | Microsoft Docs
description: Elastik Veritabanı İşlerini kullanarak bir veya daha fazla Azure SQL veritabanında Transact-SQL (T-SQL) betikleri çalıştırmayı öğrenin
services: sql-database
author: srinia
manager: craigg
ms.service: sql-database
ms.topic: overview
ms.date: 06/14/2018
ms.author: srinia
ms.openlocfilehash: b2cbf7501b3c5006c7504c7af7d70c14035cfc74
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113366"
---
# <a name="manage-groups-of-databases-with-elastic-database-jobs"></a>Elastik Veritabanı İşleriyle veritabanı gruplarını yönetin

**Elastik Veritabanı İşleri**, bir zaman çizelgesine veya istek üzerine çok sayıda veritabanı arasında bir veya daha fazla T-SQL betiğinin paralel olarak çalıştırılmasını sağlar.

**İşleri tüm veritabanı birleşimleri üzerinde çalıştırabilirsiniz**: Bir veya daha fazla tek veritabanı, bir sunucu üzerindeki tüm veritabanları bir elastik havuz veya parça eşlemesi içindeki tüm veritabanları için, herhangi bir veritabanını dahil etme veya hariç tutma esnekliğinden faydalanabilirsiniz. **İşler birden fazla sunucu ve birden fazla havuzda çalışabilir, hatta farklı aboneliklerde bulunan veritabanlarını kullanabilir.** Sunucular ve havuzlar çalışma zamanında dinamik olarak numaralandırıldığından işler, yürütme zamanında hedef grupta bulunan tüm veritabanlarında çalışır.

Aşağıdaki resimde farklı hedef gruplarda iş yürüten bir iş aracısı gösterilmektedir:

![Elastik İş aracısı kavramsal modeli](media/elastic-jobs-overview/conceptual-diagram.png)


## <a name="why-use-elastic-jobs"></a>Neden elastik işleri kullanmalısınız?

### <a name="manage-many-databases"></a>Birden fazla veritabanını yönetme

- Yönetim görevlerini hafta içi her gün, çalışma saatlerinden sonra gibi zamanlarda çalışacak şekilde planlayın.
- Şema değişiklikleri, kimlik bilgileri yönetimi, performans verisi toplama veya kiracı (müşteri) telemetri verilerini toplama gibi görevleri dağıtın. Başvuru verilerini (tüm veritabanlarında bulunan ortak veriler) güncelleştirin.
- Sorgu performansını artırmak için dizinleri yeniden oluşturun. İşleri bir veritabanı koleksiyonunda yoğun saatlerin dışında yenilenecek şekilde yapılandırın.
- Bir veritabanı kümesinden alınan sorgu sonuçlarını düzenli olarak merkezi bir tabloya toplayın. Performans sorguları sürekli yürütülebilir ve ek görevleri tetikleyecek şekilde yapılandırılabilir.

### <a name="collect-data-for-reporting"></a>Raporlama için veri toplama

- Bir Azure SQL veritabanı koleksiyonunda bulunan verileri tek bir hedef tabloda toplayın.
- Çok sayıda veritabanında müşteri telemetri verilerinin toplanması gibi daha uzun süre çalışan veri işleme sorguları çalıştırın. Sonuçlar daha ayrıntılı analiz için tek bir hedef tabloda toplanır.

### <a name="reduce-overhead"></a>Ek yükü azaltma

- Normalde Transact-SQL deyimlerini çalıştırmak veya diğer yönetim görevlerini gerçekleştirmek için veritabanlarına tek tek bağlanmanız gerekir. İşler, hedef gruptaki her bir veritabanında oturum açma görevini üstlenir. Azure SQL veritabanı grubunda çalıştırılacak Transact-SQL betiklerinin tanımlama, bakımını yapma ve sürekliliğini sağlama konusunda da denetim sahibi olursunuz.

### <a name="accounting"></a>Muhasebe

- İşler her veritabanında gerçekleştirilen işlemlerin durumunu günlüğe kaydeder. Hata oluştuğunda otomatik olarak yeniden deneme yapılır.

### <a name="flexibility"></a>Esneklik

- Azure SQL veritabanlarından oluşan özel gruplar belirleyip işin çalıştırılacağı zamanlamayı tanımlayın.


## <a name="elastic-job-components"></a>Elastik İş bileşenleri

|Bileşen  | Açıklama (ek ayrıntılar tablonun altındadır) |
|---------|---------|
|[**Elastik İş aracısı**](#elastic-job-agent) |  İşleri çalıştırmak ve yönetmek için oluşturduğunuz Azure kaynağıdır.   |
|[**İş veritabanı**](#job-database)    |    İş aracısının işle ilgili veriler, iş tanımları gibi bilgileri depolamak için kullandığı bir Azure SQL veritabanıdır.      |
|[**Hedef grup**](#target-group)      |  Bir işin çalıştırılacağı sunucu, havuz, veritabanı ve parça eşlemesi kümesidir.       |
|[**İş**](#job)  |  İş, bir veya daha fazla [iş adımından](#job-step) oluşan çalışma birimidir. İş adımları çalıştırılacak T-SQL betiğinin yanı sıra betiğin yürütülmesi için gerekli olan diğer ayrıntıları belirtir.  |


### <a name="elastic-job-agent"></a>Elastik İş aracısı

Elastik İş aracısı; işlerin oluşturulması, çalıştırılması ve yönetilmesi için kullanılan Azure kaynağıdır. Elastik İş aracısı, portalda oluşturduğunuz bir Azure kaynağıdır ([PowerShell](elastic-jobs-powershell.md) ve REST de desteklenir). 

**Elastik İş aracısı** oluşturmak için bir SQL veritabanı gerekir. Aracı, mevcut veritabanını [*İş veritabanı*](#job-database) olarak yapılandırır.

Elastik İş aracısı ücretsizdir. İş veritabanı, sıradan SQL veritabanları ile aynı şekilde faturalandırılır.

### <a name="job-database"></a>İş veritabanı

*İş veritabanı*, işleri tanımlamanın yanı sıra iş yürütme durumunu ve geçmişini takip etmek için kullanılır. *İş veritabanı* ayrıca aracı meta verilerini, günlükleri, sonuçları, iş tanımlarını depolamak için kullanılır ve ayrıca T-SQL kullanarak işlerin oluşturulması, çalıştırılması ve yönetilmesi için birçok faydalı saklı yordam ve farklı veritabanı nesnesi içerir.

Geçerli önizleme için Elastik İş aracısı oluşturma amacıyla bir Azure SQL veritabanı (S0 veya üzeri) gerekir.

*İş veritabanının* yeni olması şart değildir ancak temiz, boş, S0 veya üzeri hizmet katmanında olması gerekir. *İş veritabanı* için önerilen hizmet katmanı S1 veya üzeridir ancak bu durum iş adımı sayısı, yineleme sayısı ve işlerin çalıştırılma sıklığı gibi performans ihtiyaçlarına göre değişiklik gösterir. Örneğin bir S0 veritabanı, bir saatte birkaç iş çalıştıran bir iş aracısı için yeterli olurken dakikada bir iş çalıştırmak için yeterli performansı sunmayabilir ve bu durumda daha yüksek bir hizmet katmanının kullanılması daha iyi olacaktır.


#### <a name="job-database-permissions"></a>İş veritabanı izinleri

İş aracısı oluşturma sırasında *İş veritabanında* bir şema, tablolar ve *jobs_reader* adlı bir rol oluşturulur. Rol, aşağıdaki izinle oluşturulur ve yöneticilere iş izleme için daha ayrıntılı erişim denetimi sunmak üzere tasarlanmıştır:


|Rol adı  |'jobs' şeması izinleri  |'jobs_internal' şeması izinleri  |
|---------|---------|---------|
|**jobs_reader**     |    SELECT     |    None     |

> [!IMPORTANT]
> Veritabanı yöneticisi olarak *İş veritabanına* erişim izni vermeden önce güvenlik durumunu gözden geçirin. İş oluşturma veya düzenleme izinlerine sahip olan kötü niyetli bir kullanıcı, kendi denetimindeki bir veritabanına bağlanmak için kayıtlı kimlik bilgisini kullanan bir iş oluşturarak veya düzenleyerek ilgili kimlik bilgisinin parolasını belirleyebilir.



### <a name="target-group"></a>Hedef grup

*Hedef grup*, işin üzerinde çalışacağı veritabanı kümesini tanımlar. Hedef grup içinde aşağıdaki bileşenlerden birden fazlası veya birleşimi bulunabilir:

- **Azure SQL sunucusu**: Sunucu belirtilirse işin yürütülmesi sırasında sunucuda mevcut olan tüm veritabanları gruba dahil edilir. İş yürütülmeden önce grubun numaralandırılması ve güncelleştirilmesi için asıl veritabanı kimlik bilgisinin sağlanması gerekir.
- **Elastik havuz**: Elastik havuz belirtilirse işin yürütülmesi sırasında elastik havuzda mevcut olan tüm veritabanları gruba dahil edilir. Sunucuda olduğu gibi iş yürütülmeden önce grubun güncelleştirilmesi için asıl veritabanı kimlik bilgisinin sağlanması gerekir.
- **Tek veritabanı**: Gruba eklenmek üzere bir veya daha fazla tek veritabanı belirtin.
- **Parça eşlemesi**: Bir parça eşlemesinin veritabanlarıdır.

> [!TIP]
> İşin yürütülmesi sırasında *dinamik numaralandırma*, sunucuları veya havuzları içeren hedef gruplardaki veritabanı kümesini yeniden değerlendirir. Dinamik numaralandırma, **işlerin, yürütüldükleri sırada sunucuda veya havuzda mevcut olan tüm veritabanlarında çalıştırılmasını sağlar**. Veritabanı listesinin çalışma zamanında yeniden değerlendirilmesi özellikle havuz veya sunucu üyeliğinin sık değiştiği senaryolar için kullanışlıdır.


Havuzlar ve tek veritabanları gruba dahil edilebilir veya gruptan hariç tutulabilir. Bu sayede istenen veritabanlarını içeren bir hedef grup oluşturulabilir. Örneğin hedef gruba bir sunucuyu ekleyebilir ancak bir elastik havuzdaki belirli veritabanlarını (veya bir havuzun tamamını) hariç tutabilirsiniz.

Hedef grupta birden fazla abonelikte ve bölgede bulunan veritabanları mevcut olabilir. Birden fazla bölgenin söz konusu olduğu yürütme işlemlerinin aynı bölgedeki yürütmelere kıyasla daha yüksek gecikme süresine sahip olacağını unutmayın.


### <a name="job"></a>İş

*İş*, bir plan dahilinde veya tek seferlik olarak yürütülen çalışma birimidir. Bir işte bir veya daha fazla *iş adımı* bulunur.

#### <a name="job-step"></a>İş adımı

Her işte yürütülecek bir T-SQL betiği, bu T-SQL betiğinin çalıştırılacağı bir veya daha fazla hedef grup ve iş aracısının hedef veritabanına bağlanması için gereken kimlik bilgileri belirtilir. İşin her adımı özelleştirilebilir zaman aşımı ve yeniden deneme ilkelerine sahiptir ve her adımda isteğe bağlı çıkış parametreleri belirtilebilir.

#### <a name="job-output"></a>İş çıktısı

İş adımlarının her bir hedef veritabanında elde ettiği sonuç ayrıntılı olarak kaydedilir ve betik çıktısı, belirtilen bir tabloya aktarılabilir. Bir işin döndürdüğü verilerin kaydedileceği bir veritabanı belirtebilirsiniz.

#### <a name="job-history"></a>İş geçmişi

İş yürütme geçmişi, *İş veritabanında* kaydedilir. Sistem temizleme işlemi 45 günden daha eski olan yürütme geçmişi verilerini siler. 45 günden daha yeni olan geçmişi kaldırmak için *İş veritabanında* **sp_purge_history** saklı yordamını çağırın.

## <a name="workflow-to-create-configure-and-manage-jobs"></a>İşleri oluşturma, yapılandırma ve yönetme iş akışı

### <a name="create-and-configure-the-agent"></a>Aracıyı oluşturma ve yapılandırma

1. Boş bir S0 veya üzeri SQL veritabanı oluşturun ya da tanımlayın. Bu, Elastik İş aracısı oluşturma işlemi sırasında *İş veritabanı* olarak kullanılacaktır.
2. [Portalda](https://portal.azure.com/#create/Microsoft.SQLElasticJobAgent) veya [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent) kullanarak bir Elastik İş aracısı oluşturun.

   ![Elastik İş aracısı oluşturma](media/elastic-jobs-overview/create-elastic-job-agent.png)

### <a name="create-run-and-manage-jobs"></a>İşleri oluşturma, çalıştırma ve yönetme

1. [PowerShell](elastic-jobs-powershell.md#create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets) veya [T-SQL](elastic-jobs-tsql.md#create-a-credential-for-job-execution) kullanarak *İş veritabanında* iş yürütme kimlik bilgisi oluşturun.
2. [PowerShell](elastic-jobs-powershell.md#define-the-target-databases-you-want-to-run-the-job-against) veya [T-SQL](elastic-jobs-tsql.md#create-a-target-group-servers) kullanarak hedef grubu (işi çalıştırmak istediğiniz veritabanları) tanımlayın.
3. İşin çalışacağı her veritabanında bir iş aracısı kimlik bilgisi oluşturun [(kullanıcıyı (veya rolü) gruptaki her bir veritabanına ekleyin)](https://docs.microsoft.com/azure/sql-database/sql-database-control-access). Örnek için bkz. [PowerShell öğreticisi](elastic-jobs-powershell.md#create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets).
4. [PowerShell](elastic-jobs-powershell.md#create-a-job) veya [T-SQL](elastic-jobs-tsql.md#deploy-new-schema-to-many-databases) kullanarak bir iş oluşturun.
5. [PowerShell](elastic-jobs-powershell.md#create-a-job-step) veya [T-SQL](elastic-jobs-tsql.md#deploy-new-schema-to-many-databases) kullanarak iş adımlarını ekleyin.
6. [PowerShell](elastic-jobs-powershell.md#run-the-job) veya [T-SQL](elastic-jobs-tsql.md#begin-ad-hoc-execution-of-a-job) kullanarak bir işi çalıştırın.
7. Portal, [PowerShell](elastic-jobs-powershell.md#monitor-status-of-job-executions) veya [T-SQL](elastic-jobs-tsql.md#monitor-job-execution-status) kullanarak iş yürütme durumunu izleyin.

   ![Portal](media/elastic-jobs-overview/elastic-job-executions-overview.png)

## <a name="credentials-for-running-jobs"></a>İşleri çalıştırmak için kullanılan kimlik bilgileri

İşler, yürütme sırasında hedef grup tarafından belirtilen veritabanlarına bağlanmak için [veritabanı kapsamlı kimlik bilgilerini](/sql/t-sql/statements/create-database-scoped-credential-transact-sql) kullanır. Hedef grupta sunucular veya havuzlar varsa bu veritabanı kapsamlı kimlik bilgileri, kullanılabilir durumdaki veritabanlarını numaralandırmak amacıyla asıl veritabanına bağlanmak için kullanılır.

Bir işi çalıştırmak için uygun kimlik bilgilerinin ayarlanması kafa karışıklığına neden olabileceğinden aşağıdaki noktaları göz önünde bulundurun:

- Veritabanı kapsamlı kimlik bilgileri *İş veritabanında* oluşturulmalıdır.
- **İşin başarıyla tamamlanması için hedef veritabanlarının tümünde [yeterli izinlere](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) sahip bir oturum açma adı olmalıdır**  (aşağıdaki şemada jobuser).
- Kimlik bilgilerinin her işte yeniden kullanılması beklenir ve kimlik bilgisi parolaları şifrelenerek iş nesnelerine salt okunur erişime sahip olan kullanıcılardan gizlenir.

Aşağıdaki resim, uygun iş kimlik bilgilerinin anlaşılması ve ayarlanması konusunda yardımcı olmak üzere tasarlanmıştır. **Kullanıcıyı işin çalıştırılacağı her veritabanında (tüm *hedef kullanıcı veritabanlarında*) oluşturulması gerektiğini unutmayın**.

![Elastik İşler kimlik bilgileri](media/elastic-jobs-overview/job-credentials.png)

## <a name="security-best-practices"></a>En iyi güvenlik uygulamaları

Elastik İşlerle çalışırken dikkat etmeniz gereken en iyi deneyimlerin bazıları:

- API2lerin kullanımını güvenilir kişilerle sınırlayın.
- Kimlik bilgileri iş adımını gerçekleştirmek için gerekli olan en düşük ayrıcalıklara sahip olmalıdır. Ek bilgi için bkz. [SQL Server Yetkilendirme ve İzinler](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server).
- Sunucu ve/veya hedef grup üyesi kullanırken, asıl veritabanında işi yürütmeden önce sunucuların ve/veya havuzların veritabanı listelerini genişletmek için kullanılan veritabanı görüntüleme/listeleme haklarına sahip ayrı bir kimlik bilgisinin oluşturulması önerilir.



## <a name="agent-performance-capacity-and-limitations"></a>Aracı performansı, kapasitesi ve sınırlamaları

Elastik İşler, uzun süren işlerin tamamlanması sırasında en az düzeyde işlem kaynağı kullanır.

Hedef veritabanı grubunun boyutuna ve bir işin istenen yürütme süresine (eşzamanlı çalışan sayısı) bağlı olarak aracı için gerekli olan işlem süresi ve *İş veritabanı* performansı değişiklik gösterir (hedef ve iş sayısı ne kadar yüksek olursa gereken işlem zamanı o kadar fazla olur).

Önizleme şu an için 100 eşzamanlı işle sınırlıdır.

### <a name="prevent-jobs-from-reducing-target-database-performance"></a>İşlerin hedef veritabanının performansını düşürmesini engelleme

Bir SQL elastik havuzundaki veritabanları üzerinde iş çalıştırılması sırasında kaynakların aşırı yüklenmesini önlemek için işler aynı anda üzerinde çalışılabilecek veritabanı sayısını sınırlayacak şekilde yapılandırılabilir.

##  <a name="differences-between-elastic-jobs-and-sql-server-agent"></a>Elastik İşler ile SQL Server Agent arasındaki farklar

SQL Server Agent (şirket içi ve SQL Veritabanı Yönetilen Örneği kapsamında mevcuttur) ile Azure SQL Veritabanı Elastik İş aracısı (SQL Veritabanı ve SQL Veri Ambarı) arasında birkaç fark vardır.


|  |Elastik İşler  |SQL Server Agent |
|---------|---------|---------|
|Kapsam     |  İş aracısıyla aynı Azure bulutunda herhangi bir sayıda Azure SQL Veritabanı ve/veya veri ambarı. Hedefler farklı mantıksal sunucularda, aboneliklerde ve/veya bölgelerde olabilir. <br><br>Hedef gruplar tek veritabanı veya veri ambarlarının yanı sıra bir sunucu, havuz veya parça eşlemesi içindeki tüm veritabanları (iş zamanında dinamik olarak numaralandırılır) olabilir. | SQL aracısıyla aynı SQL Server örneğindeki tek bir veritabanı. |
|Desteklenen API’ler ve Araçlar     |  Portal, PowerShell, T-SQL, Azure Resource Manager      |   T-SQL, SQL Server Management Studio (SSMS)     |





## <a name="best-practices-for-creating-jobs"></a>İş oluşturmak için en iyi deneyimler

### <a name="idempotent-scripts"></a>Bir kez etkili betikler
Bir işin T-SQL betiklerinin [bir kez etkili](https://en.wikipedia.org/wiki/Idempotence) olması gerekir. **Bir kez etkili**, betiğin başarılı olması ve tekrar çalıştırılması durumunda aynı sonucun ortaya çıkması anlamına gelir. Bir betik, geçici ağ sorunları nedeniyle başarısız olabilir. Bu durumda iş, betiği atlamadan önce otomatik olarak önceden belirtilen sayıda yeniden deneme gerçekleştirir. Bir kez etkili betik, iki kez (veya daha fazla) çalıştırılsa dahi aynı sonucu verir.

Basit bir yöntem, bir nesneyi oluşturmadan önce mevcut olup olmadığını test etmektir.


```sql
IF NOT EXIST (some_object)
    -- Create the object
    -- If it exists, drop the object before recreating it.
```

Benzer şekilde bir betiğin mantıksal olarak test ederek ve bulduğu sonuçlara göre kendini ayarlayarak başarılı şekilde yürütülebilmesi gerekir.



## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell’i kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-powershell.md)
- [Transact-SQL (T-SQL) kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-tsql.md)