---
title: "Çok kiracılı bir uygulamada Azure SQL Veritabanı şemasını yönetme | Microsoft Docs"
description: "Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: billgib
ms.openlocfilehash: e4b8e38d20ec408869f2228597afdf2f9620515b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Azure SQL Database kullanan çok kiracılı bir uygulama içinde birden çok Kiracı için şema yönetme

[İlk Wingtip biletleri SaaS çok Kiracı veritabanı Öğreticisi](saas-multitenantdb-get-started-deploy.md) nasıl uygulama parçalı çok Kiracı veritabanı sağlayabilir ve kataloğa kaydetmek gösterir. Herhangi bir uygulama gibi Wingtip biletleri SaaS uygulama zamanla gelişmesi ve bazen, veritabanı için değişiklik yapılmasını gerektirir. Değişiklikler; yeni veya değiştirilmiş şema, yeni ya da değiştirilmiş başvuru verileri ve en iyi uygulama performansını sağlayan rutin veritabanı bakım görevlerini içerebilir. SaaS uygulamasıyla, bu değişikliklerin oldukça büyük olabilecek bir kiracı veritabanı filosunda eşgüdümlü şekilde dağıtılması gerekir. Bu değişiklikler Kiracı veritabanları gelecekte olmasını sağlama işlemine yapılması ihtiyaç duyar.

Bu öğreticide iki senaryo incelenmektedir: tüm kiracılar için başvuru verisi güncelleştirmelerini dağıtma ve başvuru verilerini içeren tabloda bir dizin döndürme. [Esnek iş](sql-database-elastic-jobs-overview.md) özelliği, Kiracı veritabanları arasında işlemlerini yürütmek için kullanılır ve *altın* yeni veritabanları için şablon olarak kullanılan Kiracı veritabanı.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]

> * Bir iş hesabı oluşturun
> * Birden çok Kiracı arasında sorgu
> * Tüm kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS çok Kiracı veritabanı uygulama dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama keşfedin.](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Bu öğretici bir sınırlı (esnek veritabanı iş) önizlemede SQL veritabanı hizmetinin özelliklerini kullanır. Bu öğreticiyi tamamlamak istiyorsanız konu satırına Esnek İşler Önizlemesi yazarak abonelik kimliğinizi SaaSFeedback@microsoft.com adresine gönderin. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ile ilgili sorular veya destek.


## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS Şema Yönetimi düzenlerine giriş

Bu örnekte kullanılan parçalı çok Kiracı veritabanı modeli kiracıların herhangi bir sayı içerecek şekilde kiracılar veritabanı sağlar. Bu örnek bir 'karma' kiracı yönetim modeli etkinleştirme bir çok kiracılı ve tek Kiracı veritabanları bir karışımını kullanmak için olası araştırır. Koruma ve bu veritabanlarını yönetme karmaşıktır. [Esnek işler](sql-database-elastic-jobs-overview.md), SQL veri katmanının yönetimini kolaylaştırır. İşler, bir veritabanı grubuna yönelik kullanıcı etkileşimi veya girişlerinden bağımsız olarak güvenilir bir şekilde görev (T-SQL betikleri) çalıştırmanıza imkan tanır. Bu yöntemi kullanılarak bir uygulamadaki tüm kiracılara şema ve ortak başvuru verisi değişikliklerini dağıtılabilir. Esnek İşler, yeni kiracı oluşturmak amacıyla kullanılan veritabanının *altın* kopyasını tutmak ve her zaman en yeni şemaya ve başvuru verilerine sahip olmasını sağlamak için de kullanılabilir.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Esnek İşler’in artık Azure SQL Veritabanı’nın (ek hizmet veya bileşen gerektirmeyen) tümleşik bir özelliği olan yeni bir sürümü vardır. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Bu sınırlı önizleme, iş hesapları oluşturmak için PowerShell’i ve işleri oluşturmak ve yönetmek için T-SQL’i desteklemektedir.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip biletleri SaaS çok Kiracı veritabanı uygulama kaynak koduna ve komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak. 

## <a name="create-a-job-account-database-and-new-job-account"></a>İş hesabı veritabanı ve yeni bir iş hesabı oluşturma

Bu öğretici, iş hesabı veritabanını ve iş hesabını oluşturmak için PowerShell kullanmanızı gerektirir. MSDB ve SQL Aracısı’nda olduğu gibi, Esnek İşler de iş tanımlarını, iş durumunu ve geçmişi depolamak için Azure SQL veritabanını kullanır. İş hesabı oluşturulduktan sonra, işleri hemen oluşturabilir ve izleyebilirsiniz.

1. İçinde **PowerShell ISE**, açık *... \\Modülleri öğrenme\\Şema Yönetimi\\Demo SchemaManagement.ps1*.
1. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* betiği, katalog sunucusunda **jobaccount** adlı bir *S2* veritabanı oluşturmak için *Deploy-SchemaManagement.ps1* betiğini çağırır. Ardından jobaccount veritabanını bir parametre olarak iş hesabı oluşturma çağrısına aktararak iş hesabını oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

Her Kiracı veritabanı tabloda salonundan türleri kümesi içerir **VenueTypes** salonundan barındırılan olayların türünü tanımlayın. Bu alıştırmada, iki ek salonundan türleri eklemek için tüm veritabanları için bir güncelleştirme dağıtın: *Motosikletinizin yarış* ve *yüzme kulübü*. Bu mekan türleri, kiracı etkinlikleri uygulamasında gördüğünüz arka plan görüntüsüne karşılık gelir.

Mekan Türü açılır menüsüne tıklayın ve yalnızca 10 mekan türü seçeneğinin kullanılabildiğini ve özellikle “Motosiklet Yarışı” ve “Yüzme Kulübü”nün listede yer almadığını doğrulayın.

Şimdi güncelleştirmek için bir iş oluşturalım **VenueTypes** tablo tüm kiracılar veritabanları ve yeni salonundan türlerini ekleyin.

Yeni bir iş oluşturmak için iş hesabı oluşturulduğunda jobaccount veritabanında oluşturulmuş iş grubu sistemi saklı yordamlarını kullanırız.

1. SSMS, Kiracı sunucuya: tenants1-mt -\<kullanıcı\>. database.windows.net
2. Gözat *tenants1* üzerinde veritabanı *tenants1-mt -\<kullanıcı\>. database.windows.net* sunucusunda ve sorgu *VenueTypes* tablosu onaylayın *Motosikletinizin yarış* ve *yüzme kulübü* olan **değil** sonuçlar listesinde.
3. Katalog sunucusuna bağlanma: Katalog-mt -\<kullanıcı\>. database.windows.net
4. Katalog sunucusu jobaccount veritabanında bağlayın.
5. SSMS, Dosya Aç... \\Modülleri öğrenme\\Şema Yönetimi\\DeployReferenceData.sql
6. Deyim değiştirin: ayarlamak @User = &lt;kullanıcı&gt; ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama dağıtıldığında kullanılan kullanıcı değeri değiştirin.
7. Betiği çalıştırmak için **F5**'e basın.

Aşağıdakileri gözlemlemek *DeployReferenceData.sql* komut dosyası:
* **SP\_ekleme\_hedef\_grup** hedef grubu oluşturur ad DemoServerGroup, artık hedef üyeleri gruba ekleyin.
* **SP\_ekleme\_hedef\_grup\_üye** ekler bir *server* hedef sunucu içindeki tüm veritabanları uymak açısından gerekli olduğunu üye türü (Bu tenants1 Not - mt - &lt;kullanıcı&gt; kiracılar veritabanını içeren sunucu) işe dahil edilecek iş yürütme zaman bir *veritabanı* hedef bulunduğu 'Altın' veritabanı (basetenantdb) için üye türü Katalog-mt -&lt;kullanıcı&gt; sunucusu ve son olarak bir *veritabanı* hedef sonraki öğreticide kullanılan adhocreporting veritabanına eklemek için üye türü.
* **SP\_ekleme\_iş** "Başvuru veri dağıtımı" adlı bir işi oluşturur.
* **SP\_ekleme\_iş** VenueTypes başvuru tablosu güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur.
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** zaman işi başarıyla kiracılar veritabanı ve başvuru tablosu içeren iki ek veritabanları tamamlandı belirlemek için sütun.

SSMS, Kiracı veritabanına gözatın *tenants1-mt -&lt;kullanıcı&gt;*  sunucusunda ve sorgu *VenueTypes* doğrulamak için tablo *Motosikletinizinyarış* ve *yüzme kulübü* artık **eklenen* tablosu.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Önceki alıştırmaya benzer şekilde, bu alıştırmada başvuru tablosu birincil anahtarında dizini yeniden oluşturmak için bir iş oluşturulur. Bu, tabloya büyük bir veri yüklendikten sonra bir yöneticinin gerçekleştirebileceği tipik bir veritabanı yönetim işlemidir.


1. SSMS, kataloğunda jobaccount veritabanına bağlanın-mt -&lt;kullanıcı&gt;. database.windows.net sunucu.
2. SSMS içinde Aç... \\Modülleri öğrenme\\Şema Yönetimi\\OnlineReindex.sql.
3. Betiği çalıştırmak için **F5**'e basın.

Aşağıdakileri gözlemlemek *OnlineReindex.sql* komut dosyası:
* **SP\_ekleme\_iş** adlı yeni bir iş oluşturur "çevrimiçi arat PK\_\_VenueTyp\_\_265E44FD7FD4C885".
* **SP\_ekleme\_iş** dizini güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur.
* Kalan görünümlerde betik iş yürütme izleyin. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** zaman işi başarıyla üzerindeki tüm hedef grup üyeleri tamamlandı belirlemek için sütun.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Birden fazla kiracı genelinde sorgulama yapmak için bir iş hesabı oluşturma
> * Tüm kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, deneyin [geçici raporlama Öğreticisi](saas-multitenantdb-adhoc-reporting.md).


## <a name="additional-resources"></a>Ek kaynaklar

<!--* Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Ölçeği artırılmış bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md)
* [Ölçeği artırılmış bulut veritabanları oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)
