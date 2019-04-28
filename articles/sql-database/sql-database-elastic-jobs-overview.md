---
title: Ölçeği genişletilen bulut veritabanlarını yönetme | Microsoft Docs
description: Veritabanlarından oluşan bir grupta bir betik yürütmek için elastik veritabanı iş hizmetini kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 8abb2e3ac4f62a3ea51cc686bbf23260fccc4077
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61475686"
---
# <a name="managing-scaled-out-cloud-databases"></a>Ölçeği genişletilen bulut veritabanlarını yönetme

[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]

**Elastik veritabanı işleri** bir müşteri barındırılan Azure bulut çağrılan geçici ve zamanlanmış yönetim görevlerinin yürütülmesini sağlayan bir hizmettir **işleri**. İşlerle kolayca ve güvenilir bir şekilde yönetim işlemlerini gerçekleştirmek için Transact-SQL betikleri çalıştırarak Azure SQL veritabanı büyük gruplarını yönetin.

Ölçeği genişletilen parçalı veritabanlarını yönetmek için **elastik veritabanı işleri** özelliği (Önizleme) dahil olmak üzere, veritabanlarından oluşan bir grupta bir Transact-SQL (T-SQL) komut dosyası güvenilir bir şekilde yürütülecek sağlar:

- Özel tanımlı bir koleksiyon veritabanlarının (aşağıda açıklanmıştır)
- tüm veritabanları bir [elastik havuz](sql-database-elastic-pool.md)
- bir parça kümesi (kullanılarak oluşturulan [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)).

Parçalı veritabanını kavramı hakkında daha fazla bilgi için bkz. [bir SQL Server veritabanı parçalama](https://blog.pythian.com/sharding-sql-server-database/).

## <a name="documentation"></a>Belgeler

- [Elastik veritabanı iş bileşenleri](sql-database-elastic-jobs-service-installation.md).
- [Elastik veritabanı işleriyle çalışmaya başlama](sql-database-elastic-jobs-getting-started.md).
- [PowerShell kullanarak iş oluşturma ve yönetme](sql-database-elastic-jobs-powershell.md).
- [Oluşturma ve Azure SQL veritabanlarına ölçeklendirilmiş yönetme](sql-database-elastic-jobs-getting-started.md)

![Elastik veritabanı iş hizmeti][1]

## <a name="why-use-jobs"></a>Neden işlerini kullanma

### <a name="manage"></a>Yönetme

Şema değişiklikleri kolayca yapın, kimlik yönetimi, başvuru verilerini güncelleştirme, performans verileri toplama veya Kiracı (müşteri) telemetrisi toplama.

### <a name="reports"></a>Reports

Bir Azure SQL veritabanı koleksiyonunda bulunan verileri tek bir hedef tabloda toplayın.

### <a name="reduce-overhead"></a>Ek yükü azaltma

Normalde Transact-SQL deyimlerini çalıştırmak veya diğer yönetim görevlerini gerçekleştirmek için veritabanlarına tek tek bağlanmanız gerekir. İşler, hedef gruptaki her bir veritabanında oturum açma görevini üstlenir. Azure SQL veritabanı grubunda çalıştırılacak Transact-SQL betiklerinin tanımlama, bakımını yapma ve sürekliliğini sağlama konusunda da denetim sahibi olursunuz.

### <a name="accounting"></a>Muhasebe

İşler betiği çalıştırın ve her veritabanı için yürütme durumunu oturum açın. Hata oluştuğunda otomatik olarak yeniden deneme yapılır.

### <a name="flexibility"></a>Esneklik

Azure SQL veritabanlarından oluşan özel gruplar belirleyip işin çalıştırılacağı zamanlamayı tanımlayın.

> [!NOTE]
> Azure portalında SQL Azure elastik havuzlar için sınırlı işlevleri azaltılmış bir dizi kullanılabilir. Geçerli işlev tam kümesi erişmek için PowerShell API'lerini kullanın.

## <a name="applications"></a>Uygulamalar

- Yeni bir şema dağıtımı gibi yönetim görevlerini gerçekleştirin.
- Başvuru veri ürün bilgileri ortak tüm veritabanlarında güncelleştirin. Veya, hafta içi her gün, saat sonra zamanlamaları otomatik güncelleştirmeler.
- Sorgu performansını artırmak için dizinleri yeniden oluşturun. Yeniden oluşturma arasında yinelenen bir düzende veritabanları koleksiyonunu gibi yoğun olmayan saatlerde yürütmek için yapılandırılabilir.
- Bir veritabanı kümesinden alınan sorgu sonuçlarını düzenli olarak merkezi bir tabloya toplayın. Performans sorguları sürekli yürütülebilir ve yürütülecek ek görevleri tetikleyecek şekilde yapılandırılabilir.
- Çok sayıda veritabanında müşteri telemetri verilerinin toplanması gibi daha uzun süre çalışan veri işleme sorguları çalıştırın. Sonuçlar daha ayrıntılı analiz için tek bir hedef tabloda toplanır.

## <a name="elastic-database-jobs-end-to-end"></a>Elastik veritabanı işleri: uçtan uca

1. Yükleme **elastik veritabanı işleri** bileşenleri. Daha fazla bilgi için [yükleme elastik veritabanı işleri](sql-database-elastic-jobs-service-installation.md). Yükleme başarısız olursa bkz [nasıl kaldırılacağı](sql-database-elastic-jobs-uninstall.md).
2. Daha fazla işlevsellik zamanlamaları ekleme ve/veya sonuç kümelerini bir araya getirmek üzere veritabanı özel tanımlı koleksiyonlar, örneğin oluşturma, erişim için PowerShell API'lerini kullanın. Basit yükleme ve yürütme karşı sınırlı iş oluşturma/izlemek için portalı kullanma bir **elastik havuz**.
3. İş yürütme için şifrelenmiş kimlik bilgileri oluşturun ve [grubundaki her veritabanı kullanıcı (veya rol) eklemek](sql-database-security-overview.md).
4. Bir kez etkili gruptaki her bir veritabanında çalıştırın T-SQL betiği oluşturun.
5. Azure portalını kullanarak işleri oluşturmak için aşağıdaki adımları izleyin: [Elastik veritabanı işleri oluşturmaya ve yönetmeye](sql-database-elastic-jobs-create-and-manage.md).
6. Veya PowerShell betikleri kullanın: [PowerShell (Önizleme) kullanarak SQL veritabanı elastik veritabanı işleri oluşturmak ve yönetmek](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Bir kez etkili betikler

Betikleri olmalıdır [ıdempotent](https://en.wikipedia.org/wiki/Idempotence). Basit bir deyişle, betik başarılı ve yeniden çalıştırın, aynı sonucu oluşur "etkili" anlamına gelir. Bir betik, geçici ağ sorunları nedeniyle başarısız olabilir. Bu durumda iş, betiği atlamadan önce otomatik olarak önceden belirtilen sayıda yeniden deneme gerçekleştirir. Bir kez etkili betiği başarıyla iki kez çalıştırılmış olsa bile aynı sonucu verir.

Basit bir yöntem, bir nesneyi oluşturmadan önce mevcut olup olmadığını test etmektir.  

```sql
    IF NOT EXIST (some_object)
    -- Create the object
```

Benzer şekilde bir betiğin mantıksal olarak test ederek ve bulduğu sonuçlara göre kendini ayarlayarak başarılı şekilde yürütülebilmesi gerekir.

## <a name="failures-and-logs"></a>Hataları ve günlükleri

Bir komut dosyası birden çok denemeden sonra başarısız olursa, iş hata günlüklerini ve devam eder. (Bir çalışma grubundaki tüm veritabanlarında anlamına gelir), bir işi sona erdikten sonra başarısız oturum açma girişimlerinin listesini kontrol edebilirsiniz. Günlükleri hatalı betiklerinde hata ayıklamak için ayrıntıları sağlayın.

## <a name="group-types-and-creation"></a>Grup türleri ve oluşturma

İki tür Grup vardır:

1. Parça kümeleri
2. Özel gruplar

Parça kümesi grupları kullanılarak oluşturulur [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md). Bir parça kümesi grubu oluşturduğunuzda, veritabanları eklenen veya gruptan otomatik olarak kaldırılır. Örneğin, parça eşlemesine eklediğinizde, yeni bir parça otomatik olarak gruptaki olacaktır. Bir iş grubu karşı çalıştırabilirsiniz.

Özel gruplar, diğer taraftan, aracılığı tanımlanır. Açıkça ekleyin veya özel gruplarından veritabanlarını kaldırmanız gerekir. Bir veritabanı grubu kesilirse, iş betiği bir nihai hatasıyla sonuçlanan veritabanında çalıştırın dener. Şu anda Azure portalını kullanarak oluşturduğunuz özel gruplar gruplarıdır.

## <a name="components-and-pricing"></a>Bileşenleri ve fiyatlandırma

Aşağıdaki bileşenler yönetim işlemleri geçici olarak yürütülmesini sağlayan bir Azure bulut hizmeti oluşturmak için birlikte çalışır. Bileşenler yüklenir ve aboneliğinizdeki kurulumu sırasında otomatik olarak yapılandırılır. Otomatik olarak oluşturulan aynı ada sahip oldukları tüm gibi hizmetleri tanımlayabilirsiniz. Ad benzersiz olduğu ve "21 rastgele oluşturulmuş karakterlerinin izlediği önek edj" oluşur.

- Azure Cloud Service

  Elastik veritabanı işleri (Önizleme) yürütülmesi gereken görevleri gerçekleştirmek için bir müşteri barındırılan Azure bulut hizmeti olarak sunulur. Portaldan hizmetin dağıtılmış ve barındırılan Microsoft Azure aboneliğiniz. Varsayılan hizmeti çalıştığında en yüksek kullanılabilirlik için iki çalışan rolleri ile dağıtılabilir. Her bir çalışan rolü (ElasticDatabaseJobWorker) varsayılan boyutu A0 örneği üzerinde çalışır. Fiyatlandırma için bkz [bulut Hizmetleri fiyatlandırması](https://azure.microsoft.com/pricing/details/cloud-services/).

- Azure SQL Veritabanı

  Hizmet olarak bilinen bir Azure SQL veritabanı kullanan **denetimi veritabanı** tüm iş meta verileri depolamak için. Varsayılan hizmet katmanına bir S0 ' dir. Fiyatlandırma için bkz [SQL veritabanı fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/).

- Azure Service Bus

  Bir Azure Service Bus, Azure bulut hizmeti içindeki iş eşgüdümünü içindir. Bkz: [hizmet veri yolu fiyatlandırması](https://azure.microsoft.com/pricing/details/service-bus/).

- Azure Storage

  Bir Azure depolama hesabı gerektiren bir sorun daha derin hata ayıklama olay içeren tanılama çıkışı günlüğünü depolamak için kullanılır (bkz [Azure bulut Hizmetleri ve sanal Makineler'de tanılamayı etkinleştirme](../cloud-services/cloud-services-dotnet-diagnostics.md)). Fiyatlandırma için bkz [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Elastik veritabanı işleri nasıl çalışır

1. Azure SQL veritabanı atanmış bir **denetimi veritabanı** tüm meta verileri ve Durum verilerini depolar.
2. Denetim veritabanı tarafından erişilen **iş hizmeti** başlatın ve işleri yürütmek için izleyebilirsiniz.
3. İki farklı roller denetimi veritabanıyla iletişim kurar:
   - Denetleyici: Yeni iş görevleri oluşturarak yeniden denemeleri başarısız olan işler ve istenen iş gerçekleştirilecek görevler hangi işlerin gerektiren belirler.
   - İş görevi yürütmede: İş görevleri yürütür.

### <a name="job-task-types"></a>Proje Görev türleri

İş yürütme taşıyan iş görevleri birden çok tür vardır:

- ShardMapRefresh

  Parça eşleme parçaları kullanılan tüm veritabanları belirlemek için sorgular
- ScriptSplit

  Betik 'Git' deyimlerini gruplayın arasında böler
- ExpandJob

  Alt işleri her veritabanı için veritabanı grubu hedefleyen bir işten oluşturur.
- ScriptExecution

  Tanımlanan kimlik bilgilerini kullanarak belirli bir veritabanında bir betik yürütür
- Dacpac

  DACPAC belirli kimlik bilgilerini kullanarak belirli bir veritabanı için geçerlidir.

## <a name="end-to-end-job-execution-work-flow"></a>Uçtan uca iş yürütme iş akışı

1. Portal veya PowerShell API'si kullanarak, bir işi eklendiği **denetimi veritabanı**. Belirli kimlik bilgilerini kullanarak veritabanlarından oluşan bir grupta bir Transact-SQL betiği iş istekleri yürütülemedi.
2. Denetleyici, yeni iş tanımlar. İş görevleri oluşturulur ve kod bölme ve grubun veritabanları yenilemek için yürütüldü. Son olarak, yeni bir iş oluşturulur ve iş'i genişletin ve yeni alt grupta tek bir veritabanında Transact-SQL betiği yürütmek için her bir alt iş burada belirtilen işleri oluşturmak için çalıştırılır.
3. Denetleyici oluşturulan alt işlerin tanımlar. Her iş için denetleyici oluşturur ve bir veritabanında bir betik yürütmek için bir iş görevini tetikler.
4. Tüm iş görevleri tamamladıktan sonra denetleyici işleri tamamlandı durumuna güncelleştirir.
   İş yürütme sırasında herhangi bir noktada PowerShell API'si, iş yürütme geçerli durumunu görüntülemek için kullanılabilir. PowerShell API'ları tarafından döndürülen tüm saatler UTC olarak gösterilir. İsterseniz, bir iptal isteğine bir işi durdurmak için başlatılabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Bileşenleri yüklemek](sql-database-elastic-jobs-service-installation.md), ardından [oluşturma ve her veritabanında bir veritabanı grubu için bir oturum açma ekleme](sql-database-manage-logins.md). Daha fazla iş oluşturulması ve Yönetimi anlamak için bkz [elastik veritabanı işleri oluşturmaya ve yönetmeye](sql-database-elastic-jobs-create-and-manage.md). Ayrıca bkz: [elastik veritabanı işleriyle çalışmaya başlama](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->
