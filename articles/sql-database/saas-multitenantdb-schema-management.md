---
title: "Çok kiracılı bir uygulamada Azure SQL Veritabanı şemasını yönetme | Microsoft Docs"
description: "Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 135764a7d89dcf711ff7fe9416850f1af9329479
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Azure SQL Database kullanan çok kiracılı bir uygulama içinde birden çok Kiracı için şema yönetme

Bu öğretici, bulutta bir hizmet (SaaS) uygulaması olarak bir yazılım veritabanlarının olası yoğun Donanma korumada zorluklar inceler. Çözümleri geliştirilen ve uygulama yaşam sırasında uygulanan şema geliştirmeleri yönetmek için gösterilmiştir.

Herhangi bir uygulama geliştikçe değişiklikleri tablo sütunlarını veya diğer şema ya da başvuru verilerini oluşabilecek veya öğeleri performansı ile ilgili. Bir SaaS uygulaması ile bu değişikliklerin mevcut çok sayıda Kiracı veritabanları arasında koordineli bir şekilde dağıtılmalıdır. Ve bu değişiklikler uygulamaya eklenecek gelecekteki Kiracı veritabanları bulunması gerekir. Bu nedenle değişiklikler de yeni veritabanları hazırlar işlemine dahil gerekir.

#### <a name="two-scenarios"></a>İki senaryo

Bu öğretici, aşağıdaki senaryolarda ele:
- Başvuru veri güncelleştirmeleri tüm kiracılar için dağıtın.
- Başvuru verileri içeren tablo dizin retuning.

[Esnek iş](sql-database-elastic-jobs-overview.md) Özelliği Azure SQL Database, Kiracı veritabanları arasında işlemlerini yürütmek için kullanılır. İşlerini altın şablon Kiracı veritabanı üzerinde de çalışır. Bu şablon, yeni veritabanları sağlandığında kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir iş hesabı oluşturun.
> * Birden çok Kiracı arasında sorgu.
> * Tüm Kiracı veritabanları verileri güncelleştirin.
> * Tüm Kiracı veritabanı tablosunda bir dizin oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Wingtip biletleri uygulama zaten dağıtılmış gerekir:
    - Tanıtır ilk öğreticide yönergeler için bkz *Wingtip biletleri* SaaS çok Kiracı veritabanı uygulama:<br />[Dağıtma ve Azure SQL veritabanı kullanan parçalı bir çok kiracılı uygulama keşfedin](saas-multitenantdb-get-started-deploy.md).
        - Dağıtma işlemi beş dakikadan daha kısa bir süre için çalışır.
    - Bilmeniz gereken *parçalı çok kiracılı* Wingtip yüklü sürümü. Sürümleri için *tek başına* ve *veritabanı Kiracı başına* mevcut öğretici desteklemez.

- SQL Server Management Studio (SSMS) en son sürümü yüklü olmalıdır. [İndirme ve yükleme SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell yüklenmelidir. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Bu öğretici sınırlı önizlemede Azure SQL veritabanı hizmetinin özelliklerini kullanır ([esnek veritabanı işleri](sql-database-elastic-database-client-library.md)). Bu öğretici yapmak isterseniz, abonelik Kimliğinizi sağlamak  *SaaSFeedback@microsoft.com*  esnek işleri Önizleme konuyla =. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun  *SaaSFeedback@microsoft.com*  ile ilgili sorular veya destek.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetimi desenleri giriş

Bu örnekte kullanılan parçalı çok Kiracı veritabanı modeli içeren bir veya daha fazla Kiracı için bir kiracı veritabanı sağlar. Bu örnek etkinleştirme çok Kiracı ve bir kiracı veritabanları, bir karışımını kullanmak için olası araştırır bir *karma* kiracı yönetim modeli. Bu veritabanları yönetme karmaşıktır. [Esnek işler](sql-database-elastic-jobs-overview.md), SQL veri katmanının yönetimini kolaylaştırır. İşlerini güvenli ve güvenilir bir şekilde Kiracı veritabanları grubu karşı görevler olarak Transact-SQL betikleri çalıştırmak etkinleştirin. Kullanıcı etkileşimi ve girişinden bağımsız görevlerdir. Bu yöntem, şema veya ortak başvuru verileri, bir uygulamadaki tüm kiracılar arasında değişiklikleri dağıtmak için kullanılabilir. Esnek işler, veritabanının altın şablon kopyasını korumak için de kullanılabilir. Şablon her zaman en son şema sağlayarak, yeni kiracılar oluşturmak için kullanılır ve başvuru verileri kullanılıyor.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Azure SQL veritabanı'nın tümleşik bir özellik sunulmuştur esnek iş yeni bir sürümü var. Tümleşik tarafından hiçbir ek hizmet veya bileşenleri gerektirir demek isteriz. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Sınırlı önizlemesi şu anda iş hesaplarını oluşturmak için PowerShell ve işleri oluşturmak ve yönetmek için T-SQL desteklemektedir.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip biletleri SaaS çok Kiracı veritabanı uygulama kaynak koduna ve komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) github'daki. Bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak. 

## <a name="create-a-job-account-database-and-new-job-account"></a>İş hesabı veritabanı ve yeni bir iş hesabı oluşturma

Bu öğretici iş hesabı veritabanı ve iş hesabı oluşturmak için PowerShell kullanmanızı gerektirir. SQL aracısı tarafından kullanılan MSDB veritabanı gibi esnek işler iş tanımları, iş durumunu ve geçmişini depolamak için bir Azure SQL veritabanı kullanır. İş hesabı oluşturulduktan sonra oluşturun ve işlerini hemen izleyin.

1. İçinde **PowerShell ISE**, açık *... \\Modülleri öğrenme\\Şema Yönetimi\\Demo SchemaManagement.ps1*.
2. Betiği çalıştırmak için **F5**'e basın.

*Demo SchemaManagement.ps1* komut çağrıları *dağıtma SchemaManagement.ps1* bir katman oluşturmak için komut dosyası *S2* adlı veritabanı **jobaccount** katalog sunucusunda. Komut dosyası sonra jobaccount veritabanı için iş hesabı oluşturma çağrısı bir parametre olarak Geçiren iş hesabını oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

#### <a name="prepare"></a>Hazırlama

Her Kiracı veritabanı salonundan türlerinde bir dizi içerir **VenueTypes** tablo. Salonundan türlerini salonundan barındırılan olayların türünü tanımlayın. Bu alıştırmada, iki ek salonundan türleri eklemek için tüm veritabanları için bir güncelleştirme dağıtın: *Motosikletinizin yarış* ve *yüzme kulübü*. Bu mekan türleri, kiracı etkinlikleri uygulamasında gördüğünüz arka plan görüntüsüne karşılık gelir.

Yeni başvuru verileri dağıtmadan önce zaten mevcut salonundan türleri sayısını not edin, 10 olması olabilir. Take Not Wingtip web kullanıcı Arabirimi için aşağıdaki bağlantıyı tıklatarak ve ardından **Salonundan türü** açılır menü:
- http://Events.Wingtip-MT.<USER>. trafficmanager.net

Şimdi özgün salonundan türleri sayımını. Özellikle, unutmayın *Motosikletinizin yarış* ya da *yüzme kulübü* henüz mevcut.

#### <a name="steps"></a>Adımlar

Güncelleştirmek için bir iş oluşturmak artık **VenueTypes** iki yeni yerini türleri ekleyerek her kiracılar veritabanındaki tablo.

Yeni bir proje oluşturmak için oluşturulmuş işlerin sistem saklı yordamları kümesi kullanmak *jobaccount* veritabanı. İş hesabı oluşturduğunuzda yordamlarda oluşturulmuştur.

1. SSMS, Kiracı sunucuya: tenants1-mt -\<kullanıcı\>. database.windows.net

2. Gözat *tenants1* üzerinde veritabanı *tenants1-mt -\<kullanıcı\>. database.windows.net* sunucu.

3. Sorgu *VenueTypes* doğrulamak için tablo *Motosikletinizin yarış* ve *yüzme kulübü* henüz sonuçlar listesinde değil.

4. Olan katalog sunucusuna bağlanmak *katalog-mt -\<kullanıcı\>. database.windows.net*.

5. Bağlanmak *jobaccount* katalog sunucusu veritabanında.

6. SSMS, dosyayı açma *... \\Modülleri öğrenme\\Şema Yönetimi\\DeployReferenceData.sql*.

7. Deyim değiştirin: ayarlamak @User = &lt;kullanıcı&gt; ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama dağıtıldığında kullanılan kullanıcı değeri değiştirin.

8. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemle

Aşağıdaki öğeleri inceleyin *DeployReferenceData.sql* komut dosyası:

- **SP\_ekleme\_hedef\_grup** hedef grup adı oluşturur *DemoServerGroup*, ve hedef üyeleri gruba ekler.

- **SP\_ekleme\_hedef\_grup\_üye** aşağıdaki öğeler ekler:
    - A *server* hedef üye türü.
        - Bu *tenants1-mt -&lt;kullanıcı&gt;*  kiracılar veritabanlarını içeren sunucu.
        - Böylece iş yürütüldüğünde sunucudaki tüm veritabanları işine eklenir.
    - A *veritabanı* hedef altın veritabanı için üye türü (*basetenantdb*), bulunduğu *katalog-mt -&lt;kullanıcı&gt;*  sunucusu
    - A *veritabanı* hedef eklemek için üye türü *adhocreporting* sonraki öğreticide kullanılan veritabanı.

- **SP\_ekleme\_iş** adlı bir işi oluşturur *başvuru veri dağıtımı*.

- **SP\_ekleme\_iş** VenueTypes başvuru tablosu güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur.

- Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** zaman işi başarıyla tamamlandı belirlemek için sütun. İş kiracılar veritabanını güncelleştirir ve başvuru tablosu içeren iki ek veritabanları güncelleştirir.

SSMS, Kiracı veritabanına gözatın *tenants1-mt -&lt;kullanıcı&gt;*  sunucu. Sorgu *VenueTypes* doğrulamak için tablo *Motosikletinizin yarış* ve *yüzme kulübü* tabloya şimdi eklenir. Salonundan türleri toplam hata sayısı iki tarafından artış.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırmada, önceki alıştırmada benzer. Bu alıştırmada başvuru tablosunda birincil anahtar dizini yeniden oluşturmak için bir proje oluşturur. Bir dizini yeniden oluşturma yönetici tabloya, büyük veri yükleme işleminden sonra çalışabilecek bir tipik veritabanı yönetim işlemdir performansını artırmak için.

1. SSMS, bağlanmak *jobaccount* veritabanını *katalog-mt -&lt;kullanıcı&gt;. database.windows.net* sunucu.

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
> - Birden çok Kiracı arasında sorgulamak için bir iş hesabı oluşturun.
> - Tüm Kiracı veritabanları verileri güncelleştirin.
> - Tüm Kiracı veritabanı tablosunda bir dizin oluşturun.

Ardından, deneyin [geçici raporlama Öğreticisi](saas-multitenantdb-adhoc-reporting.md).

