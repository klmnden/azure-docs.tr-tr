---
title: Ölçeklendirilen bulut veritabanlarını yönetme | Microsoft Docs
description: Esnek veritabanı iş hizmeti grubu bir betik yürütmek için kullanın.
metakeywords: azure sql database elastic databases
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 5e2c233ec631f6a3e57d2203a9678b42f909a885
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646094"
---
# <a name="managing-scaled-out-cloud-databases"></a>Ölçeklendirilen bulut veritabanlarını yönetme
Ölçeklendirilen parçalı veritabanlarını yönetmek için **esnek veritabanı işleri** özelliği (Önizleme) güvenilir bir şekilde veritabanları dahil olmak üzere, bir grup arasında bir Transact-SQL (T-SQL) komut dosyası yürütme olanak sağlar:

* Özel tanımlı bir koleksiyon veritabanlarının (aşağıda açıklanmıştır)
* tüm veritabanları bir [esnek havuz](sql-database-elastic-pool.md)
* Parça kümesi (kullanılarak oluşturulan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)). 

## <a name="documentation"></a>Belgeler
* [Esnek veritabanı iş bileşenlerini yüklemek](sql-database-elastic-jobs-service-installation.md). 
* [Esnek veritabanı işleri ile çalışmaya başlama](sql-database-elastic-jobs-getting-started.md).
* [PowerShell kullanarak işleri oluşturmak ve yönetmek](sql-database-elastic-jobs-powershell.md).
* [Oluşturma ve Azure SQL veritabanlarını ölçeklendirilmiş yönetme](sql-database-elastic-jobs-getting-started.md)

**Esnek veritabanı iş** şu anda bir müşteri barındırılan Azure bulut geçici ve denir zamanlanmış yönetim görevlerini yürütülmesi sağlayan bir hizmettir **işleri**. İşleri ile kolayca ve güvenilir bir şekilde yönetim işlemlerini gerçekleştirmek için Transact-SQL betiklerini çalıştırarak Azure SQL veritabanları oluşan büyük gruplar yönetin. 

![Esnek veritabanı iş hizmeti][1]

## <a name="why-use-jobs"></a>İşlerini neden kullanılır?
**Yönetme**

Kolayca şema değişiklikleri, kimlik bilgileri yönetimi, başvuru veri güncelleştirmeleri, performans verileri toplama veya Kiracı (müşteri) telemetri koleksiyonu.

**Raporlar**

Koleksiyona tek hedef tablo Azure SQL veritabanları birleşik veriler.

**Ek yükü azaltın**

Normalde, bağımsız olarak Transact-SQL deyimlerini çalıştırın veya diğer yönetim görevlerini gerçekleştirmek için her bir veritabanı için bağlanmanız gerekir. Bir işi hedef grubundaki her veritabanı oturum açmayı görevini işler. Ayrıca tanımlamak, korumak ve bir Azure SQL veritabanları grubu boyunca yürütülecek Transact-SQL betikleri kalır.

**Hesap oluşturma**

İşlerini komut dosyasını çalıştırın ve her veritabanı için yürütme durumu oturum. Hatalar oluştuğunda, otomatik yeniden deneme de alırsınız.

**Esneklik**

Bir işi zamanlamaları tanımlamak ve Azure SQL veritabanlarının özel grupları tanımlayın.

> [!NOTE]
> Azure Portal'da SQL Azure esnek havuzlar için sınırlı işlevleri yalnızca sınırlı sayıda kullanılabilir. Geçerli işlev tam kümesi erişmek için PowerShell API'leri kullanın.
> 
> 

## <a name="applications"></a>Uygulamalar
* Yeni bir şema dağıtımı gibi yönetim görevlerini gerçekleştirin.
* Başvuru verileri ürün bilgileri ortak tüm veritabanları arasında güncelleştirin. Veya her hafta içi günü, saat sonra zamanlamaları otomatik güncelleştirmeler.
* Sorgu performansını artırmak için dizinleri yeniden oluştur. Yeniden derleme veritabanlarının düzenli olarak, bir koleksiyon üzerinden gibi yoğun olmayan saatlerde yürütmek için yapılandırılabilir.
* Sorgu sonuçları veritabanları kümesinden bir devam eden temelinde merkezi bir tabloya toplayın. Performans sorguları sürekli olarak yürütülen ve yürütülecek tetikleyici ek görevleri yapılandırılır.
* Uzun çalışan veri işleme sorguları büyük bir veritabanları kümesi, arasında örneğin müşteri telemetri koleksiyonunu yürütün. Sonuçlar daha fazla çözümleme için bir tek hedef tabloya toplanır.

## <a name="elastic-database-jobs-end-to-end"></a>Esnek veritabanı iş: uçtan uca
1. Yükleme **esnek veritabanı işleri** bileşenleri. Daha fazla bilgi için bkz: [esnek veritabanı yükleme işleri](sql-database-elastic-jobs-service-installation.md). Yükleme başarısız olursa bkz [nasıl kaldırılacağı](sql-database-elastic-jobs-uninstall.md).
2. Daha fazla işlevsellik, örneğin zamanlamaları ekleme ve/veya sonuç kümelerini toplama özel tanımlı veritabanı koleksiyonları oluşturma erişmek için PowerShell API'leri kullanın. Basit yükleme ve yürütme karşı sınırlı işlerin oluşturma/izleme portalı kullanmak bir **esnek havuz**. 
3. İş yürütme için şifrelenmiş kimlik bilgileri oluşturun ve [grubundaki her veritabanı kullanıcı (veya rol) eklemek](sql-database-security-overview.md).
4. Idempotent grubundaki her veritabanı karşı çalıştırabilirsiniz T-SQL komut dosyası oluşturun. 
5. Azure portalını kullanarak işleri oluşturmak için aşağıdaki adımları izleyin: [oluşturma ve esnek veritabanı işlerini yönetme](sql-database-elastic-jobs-create-and-manage.md). 
6. Veya PowerShell betikleri kullanma: [oluşturma ve PowerShell (Önizleme) kullanarak bir SQL Database esnek veritabanı işlerini yönetmek](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotent komut dosyaları
Komut dosyalarını olmalıdır [ıdempotent](https://en.wikipedia.org/wiki/Idempotence). Basitçe, betik başarılı ve tekrar çalıştırırsanız, aynı sonucu oluşur "ıdempotent" anlamına gelir. Bir komut dosyası geçici ağ sorunları nedeniyle başarısız olabilir. Bu durumda, iş otomatik olarak hazır kaç kez desisting önce komut dosyası çalıştırarak yeniden deneyecek. Idempotent komut dosyası başarıyla iki kez çalıştırılmış olsa bile aynı sonucu verir. 

Basit bir yöntem, oluşturmadan önce bir nesne varlığı test etmektir.  

    IF NOT EXIST (some_object)
    -- Create the object 
    -- If it exists, drop the object before recreating it.

Benzer şekilde, bir komut dosyası için mantıksal olarak test ederek başarıyla yürütülecek kurabiliyor olması gerekir ve ilgili koşulları countering bulur.

## <a name="failures-and-logs"></a>Hataları ve günlükleri
Bir komut dosyası birden çok denemeden sonra başarısız olursa, iş hata günlüklerini ve devam eder. (Bir çalışma grubundaki tüm veritabanlarının karşı anlamına gelir), bir iş sona erdikten sonra başarısız girişim listesini kontrol edebilirsiniz. Günlükleri hatalı komut dosyaları hata ayıklamak için ayrıntıları sağlar. 

## <a name="group-types-and-creation"></a>Grup türleri ve oluşturma
İki tür Grup vardır: 

1. Parça kümeleri
2. Özel gruplar

Parça kümesi grupları kullanılarak oluşturulan [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md). Parça kümesi grubu oluşturduğunuzda, veritabanları eklenen veya gruptan otomatik olarak kaldırılır. Örneğin, parça eşlemeye eklediğinizde, yeni bir parça otomatik olarak grubunda olacaktır. Bir iş grubu karşı çalıştırabilirsiniz.

Özel gruplar, diğer yandan aracılığı tanımlanır. Açıkça ekleyin veya özel gruplarından veritabanlarını kaldırmanız gerekir. Gruptaki bir veritabanı kesilirse, işin bir nihai karşılanamamasına veritabanında komut dosyasını çalıştırmak dener. Şu anda Azure portalını kullanarak oluşturduğunuz özel gruplar gruplarıdır. 

## <a name="components-and-pricing"></a>Bileşenleri ve fiyatlandırma
Aşağıdaki bileşenler yönetici işleri geçici olarak yürütülmesini sağlar bir Azure bulut hizmeti oluşturmak için birlikte çalışır. Bileşenler yüklenir ve aboneliğinizde Kurulum sırasında otomatik olarak yapılandırılır. Tüm bunlar otomatik olarak oluşturulan aynı ada sahip gibi hizmetleri tanımlayabilirsiniz. Ad benzersiz olduğundan ve "21 rastgele oluşturulmuş karakterle devam etmelidir önek edj" oluşur.

* **Azure bulut hizmeti**: esnek veritabanı işleri (Önizleme) yürütme istenen görevleri gerçekleştirmek için müşteri tarafından barındırılan Azure bulut hizmeti olarak teslim. Portalından hizmete dağıtılan ve Microsoft Azure aboneliğiniz barındırılır. Varsayılan en az iki çalışan rolleri yüksek kullanılabilirlik için hizmeti çalıştığında dağıtıldı. Her çalışan rolü (ElasticDatabaseJobWorker) varsayılan boyutunu A0 örneği üzerinde çalışır. Fiyatlandırma için bkz: [fiyatlandırma bulut Hizmetleri](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Azure SQL veritabanı**: olarak bilinen bir Azure SQL veritabanı hizmetinin kullandığı **denetim veritabanı** tüm iş meta verileri depolamak için. Varsayılan hizmet katmanına bir S0 ' dir. Fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
* **Azure Service Bus**: bir Azure hizmet veri yolu, Azure bulut hizmeti içinde iş eşgüdümünü içindir. Bkz: [hizmet veri yolu fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).
* **Azure depolama**: bir Azure depolama hesabı, bir sorunu daha fazla hata ayıklama gerektiren gerektiğinde, tanılama çıktıları günlüğü depolamak için kullanılır (bkz [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](../cloud-services/cloud-services-dotnet-diagnostics.md)). Fiyatlandırma için bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Esnek veritabanı işleri nasıl çalışır
1. Bir Azure SQL veritabanı belirlenmiş bir **denetim veritabanı** tüm meta verileri ve Durum verilerini depolar.
2. Denetim veritabanı tarafından erişilen **iş hizmeti** başlatın ve işleri yürütmek için izleyebilirsiniz.
3. İki farklı roller denetim veritabanı ile iletişim kurar: 
   * Denetleyicisi: yeni iş görevleri oluşturarak istenen iş ve yeniden deneme başarısız olan işler gerçekleştirilecek görevler hangi işlerin gerektiren belirler.
   * İş görevi yürütmede: İş görevleri gerçekleştirir.

### <a name="job-task-types"></a>İş görev türleri
İşlerini yürütme taşımak iş görevleri birden çok tür vardır:

* ShardMapRefresh: parça parça kullanılan tüm veritabanları belirlemek eşlemek sorgular
* ScriptSplit: komut dosyası toplu işleri 'Git' deyime arasında böler.
* ExpandJob: alt işleri her veritabanı için bir grubu hedefleyen bir işten oluşturur
* ScriptExecution: bir komut dosyası tanımlanmış kimlik bilgilerini kullanarak belirli bir veritabanında yürütür
* Dacpac: bir DACPAC belirli kimlik bilgilerini kullanarak belirli bir veritabanına uygular.

## <a name="end-to-end-job-execution-work-flow"></a>Uçtan uca iş yürütme iş akışı
1. Portal veya PowerShell API kullanarak, bir iş eklenir **denetim veritabanı**. İş belirli kimlik bilgileri kullanılarak veritabanlarının bir gruba göre bir Transact-SQL betiği yürütülmesini istiyor.
2. Denetleyici yeni işi tanımlar. İş görevleri oluşturulur ve betik bölme ve grubun veritabanları yenilemek için yürütüldü. Son olarak, yeni bir iş oluşturulur ve iş'i genişletin ve yeni alt her bir alt iş grubundaki tek bir veritabanının karşı Transact-SQL betiği çalıştırmak için burada belirtilen işi oluşturmak için yürütüldü.
3. Denetleyici oluşturulan alt işlerin tanımlar. Her iş için denetleyici oluşturur ve bir veritabanında betik yürütmek için bir iş görevini tetikler. 
4. Tüm iş görevleri tamamladıktan sonra denetleyici işleri tamamlanmış durumuna güncelleştirir. 
   İş yürütme sırasında herhangi bir noktada, PowerShell API'si iş yürütme geçerli durumunu görüntülemek için kullanılabilir. PowerShell API'ları tarafından döndürülen tüm saatler UTC olarak temsil edilir. İsterseniz, bir işi durdurmak için iptal isteği başlatılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
[Bileşenleri yüklemek](sql-database-elastic-jobs-service-installation.md), ardından [oluşturun ve veritabanlarının grubundaki her veritabanı için bir oturum açma ekleyin](sql-database-manage-logins.md). Daha fazla iş oluşturulması ve Yönetimi anlamak için bkz: [esnek veritabanı işleri oluşturmaya ve yönetmeye](sql-database-elastic-jobs-create-and-manage.md). Ayrıca bkz. [esnek veritabanı işlerine Başlarken](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->


