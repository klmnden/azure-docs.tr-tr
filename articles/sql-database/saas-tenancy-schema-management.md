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
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: ad7434efcead9a250bda9958ade74e798609a25d
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme

[İlk başına Wingtip biletleri SaaS veritabanı Kiracı öğretici](saas-dbpertenant-get-started-deploy.md) nasıl uygulama bir kiracı veritabanı sağlamak ve kataloğa kaydetmek gösterir. Herhangi bir uygulama gibi başına Wingtip biletleri SaaS veritabanı Kiracı uygulama zamanla gelişmesi ve bazen, veritabanı için değişiklik yapılmasını gerektirir. Değişiklikler; yeni veya değiştirilmiş şema, yeni ya da değiştirilmiş başvuru verileri ve en iyi uygulama performansını sağlayan rutin veritabanı bakım görevlerini içerebilir. SaaS uygulamasıyla, bu değişikliklerin oldukça büyük olabilecek bir kiracı veritabanı filosunda eşgüdümlü şekilde dağıtılması gerekir. Bu değişiklikler Kiracı veritabanları gelecekte olmasını sağlama işlemine yapılması ihtiyaç duyar.

Bu öğreticide iki senaryo incelenmektedir: tüm kiracılar için başvuru verisi güncelleştirmelerini dağıtma ve başvuru verilerini içeren tabloda bir dizin döndürme. [Esnek iş](sql-database-elastic-jobs-overview.md) özelliği, tüm kiracılar arasında işlemlerini yürütmek için kullanılır ve *altın* yeni veritabanları için şablon olarak kullanılan Kiracı veritabanı.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]

> * Bir iş hesabı oluşturun
> * Birden çok Kiracı arasında sorgu
> * Tüm kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Başına Wingtip biletleri SaaS veritabanı Kiracı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve başına Wingtip biletleri SaaS veritabanı Kiracı uygulama keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi tamamlamak istiyorsanız konu satırına Esnek İşler Önizlemesi yazarak abonelik kimliğinizi SaaSFeedback@microsoft.com adresine gönderin. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ile ilgili sorular veya destek.*


## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS Şema Yönetimi düzenlerine giriş

Veritabanı başına tek kiracı SaaS düzeni, elde edilen veri yalıtımından birçok farklı şekilde yararlanır, ancak aynı zamanda çok sayıda veritabanının korunması ve yönetilmesi için süreci daha karmaşık hâle getirir. [Esnek işler](sql-database-elastic-jobs-overview.md), SQL veri katmanının yönetimini kolaylaştırır. İşler, bir veritabanı grubuna yönelik kullanıcı etkileşimi veya girişlerinden bağımsız olarak güvenilir bir şekilde görev (T-SQL betikleri) çalıştırmanıza imkan tanır. Bu yöntemi kullanılarak bir uygulamadaki tüm kiracılara şema ve ortak başvuru verisi değişikliklerini dağıtılabilir. Esnek İşler, yeni kiracı oluşturmak amacıyla kullanılan veritabanının *altın* kopyasını tutmak ve her zaman en yeni şemaya ve başvuru verilerine sahip olmasını sağlamak için de kullanılabilir.

![ekran](media/saas-tenancy-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Esnek İşler’in artık Azure SQL Veritabanı’nın (ek hizmet veya bileşen gerektirmeyen) tümleşik bir özelliği olan yeni bir sürümü vardır. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Bu sınırlı önizleme, iş hesapları oluşturmak için PowerShell’i ve işleri oluşturmak ve yönetmek için T-SQL’i desteklemektedir.

> [!NOTE]
> *Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi tamamlamak istiyorsanız konu satırına Esnek İşler Önizlemesi yazarak abonelik kimliğinizi SaaSFeedback@microsoft.com adresine gönderin. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ile ilgili sorular veya destek.*

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama komut dosyaları alma

Başına Wingtip biletleri SaaS veritabanı Kiracı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) github depo. [Başına Wingtip biletleri SaaS veritabanı Kiracı komut dosyalarını karşıdan yüklemek için adımları](saas-dbpertenant-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-a-job-account-database-and-new-job-account"></a>İş hesabı veritabanı ve yeni bir iş hesabı oluşturma

Bu öğretici, iş hesabı veritabanını ve iş hesabını oluşturmak için PowerShell kullanmanızı gerektirir. MSDB ve SQL Aracısı’nda olduğu gibi, Esnek İşler de iş tanımlarını, iş durumunu ve geçmişi depolamak için Azure SQL veritabanını kullanır. İş hesabı oluşturulduktan sonra, işleri hemen oluşturabilir ve izleyebilirsiniz.

1. **PowerShell ıse'de**açın... \\Modülleri öğrenme\\Şema Yönetimi\\*Demo SchemaManagement.ps1*.
1. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* betiği, katalog sunucusunda **jobaccount** adlı bir *S2* veritabanı oluşturmak için *Deploy-SchemaManagement.ps1* betiğini çağırır. Ardından jobaccount veritabanını bir parametre olarak iş hesabı oluşturma çağrısına aktararak iş hesabını oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

Her kiracı veritabanı, bir mekanda düzenlenen etkinliklerin türünü tanımlamak için bir mekan türü kümesi içerir. Bu alıştırmada, şu iki ek mekan türünü eklemek için tüm kiracı veritabanlarına yönelik bir güncelleştirmeyi dağıtacaksınız: *Motosiklet Yarışı* ve *Yüzme Kulübü*. Bu mekan türleri, kiracı etkinlikleri uygulamasında gördüğünüz arka plan görüntüsüne karşılık gelir.

Mekan Türü açılır menüsüne tıklayın ve yalnızca 10 mekan türü seçeneğinin kullanılabildiğini ve özellikle “Motosiklet Yarışı” ve “Yüzme Kulübü”nün listede yer almadığını doğrulayın.

Şimdi tüm kiracı veritabanlarında *VenueTypes* tablosunu güncelleştirmek ve yeni mekan türlerini eklemek için bir iş oluşturalım.

Yeni bir iş oluşturmak için iş hesabı oluşturulduğunda jobaccount veritabanında oluşturulmuş iş grubu sistemi saklı yordamlarını kullanırız.

1. SSMS açın ve katalog sunucusuna bağlanma: Katalog-dpt -\<kullanıcı\>. database.windows.net sunucu
1. Ayrıca Kiracı sunucuya: tenants1-dpt -\<kullanıcı\>. database.windows.net
1. Gözat *contosoconcerthall* üzerinde veritabanı *tenants1-dpt -\<kullanıcı\>*  sunucusunda ve sorgu *VenueTypes* doğrulamak için tablo *Motosikletinizin yarış* ve *yüzme kulübü* **değil** sonuçlar listesinde.
1. SSMS, Dosya Aç... \\Modülleri öğrenme\\Şema Yönetimi\\DeployReferenceData.sql
1. Deyim değiştirin: AYARLAMAK @wtpUser = &lt;kullanıcı&gt; ve başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtıldığında kullanılan kullanıcı değerini değiştirin
1. jobaccount veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için **F5**’e basın

* **sp\_add\_target\_group**, hedef grubu adı DemoServerGroup’u oluşturur, artık hedef üyeleri eklememiz gerekiyor.
* **SP\_ekleme\_hedef\_grup\_üye** ekler bir *server* hedef sunucu içindeki tüm veritabanları uymak açısından gerekli olduğunu üye türü (Bu tenants1 Not - dpt - &lt;Kullanıcı&gt; server Kiracı veritabanları içeren) işi aynı anda yürütme işinde ikinci ekleme eklenmesi gereken bir *veritabanı* hedef üye türü, özellikle 'Altın' Kataloğu'nda bulunan veritabanı (basetenantdb)-dpt -&lt;kullanıcı&gt; sunucusu ve son olarak başka bir *veritabanı* hedef sonraki bir kullanılan adhocanalytics veritabanına eklemek için grubu üye türü öğretici.
* **sp\_add\_job**, “Başvuru Verileri Dağıtımı” adında bir iş oluşturur
* **SP\_ekleme\_iş** VenueTypes başvuru tablosu güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** zaman işi başarıyla tüm Kiracı veritabanları ve başvuru tablosu içeren iki ek veritabanları tamamlandı belirlemek için sütun.

1. SSMS, Gözat *contosoconcerthall* üzerinde veritabanı *tenants1-dpt -\<kullanıcı\>*  sunucusunda ve sorgu *VenueTypes* tablosu onaylayın *Motosikletinizin yarış* ve *yüzme kulübü* **olan** artık sonuçlar listesinde.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Önceki alıştırmaya benzer şekilde, bu alıştırmada başvuru tablosu birincil anahtarında dizini yeniden oluşturmak için bir iş oluşturulur. Bu, tabloya büyük bir veri yüklendikten sonra bir yöneticinin gerçekleştirebileceği tipik bir veritabanı yönetim işlemidir.

Aynı iş “sistem” saklı yordamlarını kullanarak bir iş oluşturun.

1. SSMS açın ve kataloğa bağlanması-dpt -&lt;kullanıcı&gt;. database.windows.net sunucu
1. …\\Öğrenme Modülleri\\Şema Yönetimi\\OnlineReindex.sql dosyasını açın
1. Sağ tıklayın, bağlantıyı seçin ve kataloğa bağlanması-dpt -&lt;kullanıcı&gt;. database.windows.net sunucusu zaten bağlıysa,
1. jobaccount veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için F5’e basın

* sp\_add\_job, “Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885” adlı yeni bir iş oluşturur
* sp\_add\_jobstep, dizini güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur




## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Birden fazla kiracı genelinde sorgulama yapmak için bir iş hesabı oluşturma
> * Tüm kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

[Geçici analizler öğreticisi](saas-tenancy-adhoc-analytics.md)


## <a name="additional-resources"></a>Ek kaynaklar

* [Derleme başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtımı sırasında ek öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Ölçeği artırılmış bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md)
* [Ölçeği artırılmış bulut veritabanları oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)