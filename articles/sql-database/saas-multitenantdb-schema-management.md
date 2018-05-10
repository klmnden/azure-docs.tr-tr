---
title: Çok kiracılı bir uygulamada Azure SQL Veritabanı şemasını yönetme | Microsoft Docs
description: Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme
keywords: sql veritabanı öğreticisi
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: dc70fe43c63d6b77d4a122f196f59fe51573b7aa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="manage-schema-in-a-saas-application-that-uses-sharded-multi-tenant-sql-databases"></a>Parçalı çok Kiracı SQL veritabanı kullanan bir SaaS uygulaması şemada yönetme

Bu öğretici, bir hizmet (SaaS) uygulaması olarak bir yazılım veritabanlarının Donanma korumada zorluklar inceler. Çözümleri veritabanları Donanma arasında şema değişiklikleri fanning gösterilmiştir.

Herhangi bir uygulama gibi Wingtip biletleri SaaS uygulama zamanla gelişmesi ve veritabanı değişiklikleri gerektirir. Değişiklikleri şema veya başvuru veri etkileyebilir veya veritabanı bakım görevlerini uygulayın. Kiracı deseni başına bir veritabanını kullanarak bir SaaS uygulaması ile değişiklikleri Kiracı veritabanları arasında bir olası yoğun Donanma Eşgüdümlü gerekir. Ayrıca, bu değişiklikleri sağlama işlemi oluşturuldukları sırada yeni veritabanları dahil emin olmak için veritabanına eklemeniz gerekir.

#### <a name="two-scenarios"></a>İki senaryo

Bu öğretici, aşağıdaki senaryolarda ele:
- Başvuru veri güncelleştirmeleri tüm kiracılar için dağıtın.
- Başvuru verileri içeren bir tablo üzerinde bir dizini yeniden oluşturma.

[Esnek iş](sql-database-elastic-jobs-overview.md) Özelliği Azure SQL Database, Kiracı veritabanları arasında işlemlerini yürütmek için kullanılır. İşlerini 'şablon' Kiracı veritabanı üzerinde de çalışır. Wingtip biletleri örnek uygulaması, yeni bir kiracı veritabanı sağlamak için bu şablonu veritabanı kopyalanır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir İş Aracısı oluşturun.
> * Bir T-SQL sorgusu birden fazla Kiracı veritabanı yürütün.
> * Tüm Kiracı veritabanları başvuru verileri güncelleştirin.
> * Tüm Kiracı veritabanı tablosunda bir dizin oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Wingtip biletleri çok Kiracı veritabanı uygulama zaten dağıtılmış gerekir:
    - Wingtip biletleri SaaS çok Kiracı veritabanı uygulama tanıtır ilk öğreticide yönergeler için bkz:<br />[Dağıtma ve Azure SQL veritabanı kullanan parçalı bir çok kiracılı uygulama keşfedin](saas-multitenantdb-get-started-deploy.md).
        - Dağıtma işlemi beş dakikadan daha kısa bir süre için çalışır.
    - Bilmeniz gereken *parçalı çok kiracılı* Wingtip yüklü sürümü. Sürümleri için *tek başına* ve *veritabanı Kiracı başına* Bu öğretici desteklemez.

- SQL Server Management Studio (SSMS) en son sürümü yüklü olmalıdır. [İndirme ve yükleme SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell yüklenmelidir. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Bu öğretici sınırlı önizlemede Azure SQL veritabanı hizmetinin özelliklerini kullanır ([esnek veritabanı işleri](sql-database-elastic-database-client-library.md)). Bu öğretici yapmak isterseniz, abonelik Kimliğinizi sağlamak *SaaSFeedback@microsoft.com* esnek işleri Önizleme konuyla =. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun *SaaSFeedback@microsoft.com* ile ilgili sorular veya destek.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetimi desenleri giriş

Bu örnekte kullanılan parçalı çok Kiracı veritabanı modeli içeren bir veya daha fazla Kiracı için bir kiracı veritabanı sağlar. Bu örnek etkinleştirme çok Kiracı ve bir kiracı veritabanları, bir karışımını kullanmak için olası araştırır bir *karma* kiracı yönetim modeli. Bu veritabanları yapılan değişiklikleri yönetme karmaşık olabilir. [Esnek iş](sql-database-elastic-jobs-overview.md) veritabanı çok sayıda yönetim kolaylaştırır. İşlerini güvenli ve güvenilir bir şekilde Kiracı veritabanları grubu karşı görevler olarak Transact-SQL betikleri çalıştırmak etkinleştirin. Kullanıcı etkileşimi ve girişinden bağımsız görevlerdir. Bu yöntem, şema veya ortak başvuru verileri, bir uygulamadaki tüm kiracılar arasında değişiklikleri dağıtmak için kullanılabilir. Esnek işler, veritabanının altın şablon kopyasını korumak için de kullanılabilir. Şablon her zaman en son şema sağlayarak, yeni kiracılar oluşturmak için kullanılır ve başvuru verileri kullanılıyor.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Azure SQL veritabanı'nın tümleşik bir özellik sunulmuştur esnek iş yeni bir sürümü var. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Sınırlı önizlemesi şu anda bir iş aracısı ve T-SQL işleri oluşturmak ve yönetmek için oluşturmak için PowerShell kullanarak destekler.
> [!NOTE] 
> Bu öğretici bir sınırlı (esnek veritabanı iş) önizlemede SQL veritabanı hizmetinin özelliklerini kullanır. Bu öğretici yapmak isterseniz, abonelik Kimliğinizi sağlamak SaaSFeedback@microsoft.com esnek işleri Önizleme konuyla =. Aboneliğiniz etkin onay aldıktan sonra indirin ve en son sürüm öncesi işleri cmdlet'leri yükleyin. Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ile ilgili sorular veya destek.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip biletleri SaaS çok Kiracı veritabanı uygulama kaynak koduna ve komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) github'daki. Bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak. 

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Veritabanı ve İş Aracısı'nın yeni bir İş Aracısı oluşturma

Bu öğretici, İş Aracısı ve İş Aracısı veritabanı oluşturmak için PowerShell kullanmanızı gerektirir. SQL aracısı tarafından kullanılan MSDB veritabanı gibi bir İş Aracısı iş tanımları, iş durumunu ve geçmişini depolamak için bir Azure SQL veritabanı kullanır. İş Aracısı oluşturulduktan sonra oluşturabilir ve işlerini hemen izleyin.

1. İçinde **PowerShell ISE**, açık *... \\Modülleri öğrenme\\Şema Yönetimi\\Demo SchemaManagement.ps1*.
2. Betiği çalıştırmak için **F5**'e basın.

*Demo SchemaManagement.ps1* komut çağrıları *dağıtma SchemaManagement.ps1* adlı bir veritabanı oluşturmak için komut dosyası _jobagent_ katalog sunucusunda. Komut dosyası ardından geçirme işi aracısı oluşturur _jobagent_ bir parametre olarak veritabanı.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

#### <a name="prepare"></a>Hazırlama

Her bir kiracının veritabanı salonundan türlerinde bir dizi içerir **VenueTypes** tablo. Her salonundan türü salonundan barındırılan olayları türünü tanımlar. Bu salonundan türleri Kiracı olayları uygulamasında gördüğünüz arka plan görüntüleri karşılık gelir.  Bu alıştırmada, iki ek salonundan türleri eklemek için tüm veritabanları için bir güncelleştirme dağıtın: *Motosikletinizin yarış* ve *yüzme kulübü*. 

İlk olarak, her bir kiracı veritabanı içinde bulunan salonundan türlerini gözden geçirin. SQL Server Management Studio (SSMS) Kiracı veritabanlarından birini bağlanın ve VenueTypes tablosunu inceleyin.  Azure portalında, sorgu Düzenleyicisi'ni bu tabloda ayrıca Sorgulayabileceğiniz veritabanı sayfasından erişilebilir. 

1. SSMS açın ve Kiracı sunucuya bağlanın: *tenants1-dpt -&lt;kullanıcı&gt;. database.windows.net*
1. Onaylamak için *Motosikletinizin yarış* ve *yüzme kulübü* **değil** için şu anda dahil, Gözat *contosoconcerthall* üzerinde veritabanı *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusunda ve sorgu *VenueTypes* tablo.



#### <a name="steps"></a>Adımlar

Güncelleştirmek için bir iş oluşturmak artık **VenueTypes** iki yeni yerini türleri ekleyerek her kiracılar veritabanındaki tablo.

Yeni bir proje oluşturmak için oluşturulmuş işlerin sistem saklı yordamları kümesi kullanmak _jobagent_ veritabanı. Saklı yordamlar İş Aracısı oluşturulduğu zaman oluşturulmuştur.

1. SSMS, Kiracı sunucuya: tenants1-mt -&lt;kullanıcı&gt;. database.windows.net

2. Gözat *tenants1* veritabanı.

3. Sorgu *VenueTypes* doğrulamak için tablo *Motosikletinizin yarış* ve *yüzme kulübü* henüz sonuçlar listesinde değil.

4. Olan katalog sunucusuna bağlanmak *katalog-mt -&lt;kullanıcı&gt;. database.windows.net*.

5. Bağlanmak _jobagent_ katalog sunucusu veritabanında.

6. SSMS, dosyayı açma *... \\Modülleri öğrenme\\Şema Yönetimi\\DeployReferenceData.sql*.

7. Deyim değiştirin: ayarlamak @User = &lt;kullanıcı&gt; ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama dağıtıldığında kullanılan kullanıcı değeri değiştirin.

8. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemle

Aşağıdaki öğeleri inceleyin *DeployReferenceData.sql* komut dosyası:

- **SP\_ekleme\_hedef\_grup** hedef grup adı oluşturur *DemoServerGroup*, ve hedef üyeleri gruba ekler.

- **SP\_ekleme\_hedef\_grup\_üye** aşağıdaki öğeler ekler:
    - A *server* hedef üye türü.
        - Bu *tenants1-mt -&lt;kullanıcı&gt;*  kiracılar veritabanlarını içeren sunucu.
        - Sunucu dahil olmak üzere iş yürütür sırada mevcut Kiracı veritabanlarını içerir.
    - A *veritabanı* hedef şablon veritabanı için üye türü (*basetenantdb*), bulunduğu *katalog-mt -&lt;kullanıcı&gt;*  sunucusu
    - A *veritabanı* hedef eklemek için üye türü *adhocreporting* sonraki öğreticide kullanılan veritabanı.

- **SP\_ekleme\_iş** adlı bir işi oluşturur *başvuru veri dağıtımı*.

- **SP\_ekleme\_iş** VenueTypes başvuru tablosu güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur.

- Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** işi bittiğinde belirlemek için sütun. İş kiracılar veritabanını güncelleştirir ve başvuru tablosu içeren iki ek veritabanları güncelleştirir.

SSMS, Kiracı veritabanına gözatın *tenants1-mt -&lt;kullanıcı&gt;*  sunucu. Sorgu *VenueTypes* doğrulamak için tablo *Motosikletinizin yarış* ve *yüzme kulübü* tabloya şimdi eklenir. Salonundan türleri toplam hata sayısı iki tarafından artış.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırmada, tüm Kiracı veritabanları başvuru tablosunda birincil anahtar dizini yeniden oluşturmak için bir proje oluşturur. Bir dizini yeniden oluşturma yönetici çalışabilecek bir tipik veritabanı yönetim işlemdir çok miktarda veri yükü, performansı artırmak için yükleme sonra.

1. SSMS, bağlanmak _jobagent_ veritabanını *katalog-mt -&lt;kullanıcı&gt;. database.windows.net* sunucu.

2. SSMS, açık *... \\Modülleri öğrenme\\Şema Yönetimi\\OnlineReindex.sql*.

3. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemle

Aşağıdaki öğeleri inceleyin *OnlineReindex.sql* komut dosyası:

* **SP\_ekleme\_iş** adlı yeni bir iş oluşturur *çevrimiçi yeniden dizin oluşturma PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_ekleme\_iş** dizini güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur.

* Kalan görünümlerde betik iş yürütme izleyin. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** işi başarıyla tamamlandı tüm hedef grup üyeleri zaman belirlemek için sütun.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Ölçeği artırılmış bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md)
* [Ölçeği artırılmış bulut veritabanları oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
.
> * Birden çok veritabanı arasında T-SQL işlerini çalıştırmak için bir İş Aracısı oluşturun
> * Tüm Kiracı veritabanları başvuru verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, deneyin [geçici raporlama öğretici](saas-multitenantdb-adhoc-reporting.md) dağıtılmış sorgular arasında Kiracı veritabanları çalıştıran keşfetmek için.

