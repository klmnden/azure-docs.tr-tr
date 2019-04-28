---
title: Tek kiracılı bir uygulamada Azure SQL veritabanı şemasını yönetme | Microsoft Docs
description: Azure SQL veritabanı kullanan tek kiracılı bir uygulama birden fazla Kiracı için şemayı yönetme
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 09/19/2018
ms.openlocfilehash: b2aa3eb6a117bbbdcf9c4aa44161dc25ddea2f1a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61484389"
---
# <a name="manage-schema-in-a-saas-application-using-the-database-per-tenant-pattern-with-azure-sql-database"></a>Bir SaaS uygulamasında Kiracı başına veritabanı düzenini Azure SQL veritabanı ile kullanarak şemayı yönetme
 
Bir veritabanı uygulaması geliştikçe değişiklikleri kaçınılmaz olarak veritabanı şeması veya başvuru verileriyle yapılması gerekir.  Ayrıca, veritabanı bakım görevlerini düzenli aralıklarla gereklidir. Veritabanı başına Kiracı düzeni kullanan bir uygulamayı yönetme, Kiracı veritabanlarını filosundan bu değişiklikleri veya bakım görevlerini uygulamak gerektirir.

Bu öğreticide, tüm kiracılar için başvuru verisi güncelleştirmelerini dağıtma iki senaryo incelenmektedir ve başvuru verilerini içeren tabloda bir dizini yeniden oluşturma. [Esnek işler](sql-database-elastic-jobs-overview.md) özelliği, tüm Kiracı veritabanlarında ve yeni Kiracı veritabanları oluşturmak için kullanılan şablon veritabanında bu eylemler yürütmek için kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * İş Aracısı oluşturma
> * T-SQL işleri, tüm Kiracı veritabanlarında çalışmasına neden
> * Başvuru tüm Kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip bilet SaaS her Kiracı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Kiracı uygulama başına Wingtip bilet SaaS veritabanı keşfedin](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Bu öğreticide SQL veritabanı hizmetinin sınırlı Önizleme (elastik veritabanı işleri) özellikleri kullanılır. Bu öğreticiyi uygulamak istiyorsanız, abonelik Kimliğinizi sağlamanız SaaSFeedback@microsoft.com konuyla esnek işler önizlemesi yazarak. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ilgili sorular veya destek için.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetimi düzenlerine giriş

Kiracı deseni başına veritabanı Kiracı verileri etkili bir şekilde ayırır, ancak yönetmek ve korumak için veritabanlarının sayısını artırır. [Esnek işler](sql-database-elastic-jobs-overview.md) SQL veritabanları yönetimini kolaylaştırır. İşleri güvenli ve güvenilir bir şekilde veritabanlarından oluşan bir grupta karşı görev (T-SQL betikleri) çalıştırmanıza olanak sağlar. İşleri, uygulamadaki tüm Kiracı veritabanlarında şema ve ortak başvuru verisi değişikliklerini dağıtabilirsiniz. Esnek işler de korumak için kullanılabilir bir *şablon* her zaman, yeni kiracılar oluşturmak için kullanılan veritabanı en yeni şemaya ve başvuru verilerine sahip.

![ekran](media/saas-tenancy-schema-management/schema-management-dpt.png)


## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Artık Azure SQL veritabanı'nın tümleşik bir özelliği olan esnek işler, yeni bir sürümü var. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Bu sınırlı önizlemesi şu anda İş Aracısı ve T-SQL işleri oluşturmak ve yönetmek için oluşturmak için PowerShell kullanarak destekler.

> [!NOTE]
> Bu öğreticide SQL veritabanı hizmetinin sınırlı Önizleme (elastik veritabanı işleri) özellikleri kullanılır. Bu öğreticiyi uygulamak istiyorsanız, abonelik Kimliğinizi sağlamanız SaaSFeedback@microsoft.com konuyla esnek işler önizlemesi yazarak. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ilgili sorular veya destek için.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip bilet SaaS veritabanı başına Kiracı uygulama betiklerini alma

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>İş Aracısı veritabanı ve yeni iş Aracısı oluşturma

Bu öğretici, bir iş aracısı ve yedekleme İş Aracısı veritabanı oluşturmak için PowerShell kullanmanızı gerektirir. İş Aracısı veritabanı, iş tanımlarını, iş durumunu ve geçmişini tutar. İş Aracısı ve veritabanı oluşturulduktan sonra oluşturma ve işleri hemen izleyin.

1. **PowerShell ıse'de**açın... \\Öğrenme modülleri\\Şema Yönetimi\\*Demo-SchemaManagement.ps1*.
1. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* komut çağrıları *Deploy-SchemaManagement.ps1* adlı bir SQL veritabanı oluşturma betiği *osagent* katalog sunucusunda. Ardından, veritabanını bir parametre olarak kullanan iş aracısı oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

Wingtip bilet uygulamada, her Kiracı veritabanı desteklenen mekan türü kümesi içerir. Her mekan türü barındırılabilir ve bu uygulamada kullanılan arka plan resmi belirleyen olayları tanımlayan bir belirli mekan türü değil. Bu uygulamayı yeni tür olay desteklemek eklenen güncelleştirilmiş ve yeni mekan türlerini bu başvuru verileri olmalıdır.  Bu alıştırmada, iki ek mekan türünü eklemek için tüm Kiracı veritabanlarına yönelik bir güncelleştirme dağıtın: *Motosiklet yarışı* ve *yüzme kulübü*.

İlk olarak, her bir kiracı veritabanında bulunan mekan türlerini gözden geçirin. Kiracı veritabanlarını SQL Server Management Studio (SSMS) birine bağlanın ve VenueTypes tabloyu inceleyin.  Ayrıca, Azure portalındaki sorgu Düzenleyicisi'nde bu tabloda sorgulayabilirsiniz veritabanı sayfasından erişilebilir. 

1. SSMS'yi açın ve Kiracı sunucuya: *tenants1-dpt -&lt;kullanıcı&gt;. database.windows.net*
1. Onaylamak için *motosiklet yarışı* ve *yüzme kulübü* **değil** Gözat şu anda dahil, _contosoconcerthall_ veritabanına *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusu ve sorgu *VenueTypes* tablo.

Şimdi güncelleştirmek için bir iş oluşturalım *VenueTypes* yeni mekan türlerini eklemek için tüm Kiracı veritabanlarında tablo.

Yeni bir proje oluşturmak için işleri sistem saklı yordamlarını oluşturduğunuzdan, bir dizi kullanın. _jobagent_ veritabanı İş Aracısı oluşturulduğu zaman.

1. SSMS'de, katalog sunucusuna: *Kataloğu-dpt -&lt;kullanıcı&gt;. database.windows.net* sunucusu 
1. SSMS'de, dosyasını aç... \\Öğrenme modülleri\\Şema Yönetimi\\deployreferencedata.SQL öğesini
1. Deyimi değiştirin: AYARLAMA @wtpUser = &lt;kullanıcı&gt; ve Wingtip bilet SaaS her Kiracı veritabanı uygulamasını dağıtırken kullandığınız kullanıcı değerini yerine koy
1. Bağlı olduğunuzdan emin olun _jobagent_ veritabanı ve ENTER tuşuna **F5** betiği çalıştırmak için

Aşağıdaki öğeleri inceleyin *deployreferencedata.SQL öğesini* betiği:
* **SP\_ekleme\_hedef\_grubu** hedef grubu adı Demoservergroup'u oluşturur.
* **SP\_ekleme\_hedef\_grubu\_üye** hedef veritabanları kümesini tanımlamak için kullanılır.  İlk _tenants1-dpt -&lt;kullanıcı&gt;_  sunucu eklenir.  Bir hedef sunucuya eklemeyi veritabanları bu sunucu projeye eklenecek iş yürütme zamanında neden olur. Ardından _basetenantdb_ veritabanı ve *adhocreporting* veritabanı (bir sonraki öğreticide kullanılan), hedefler olarak eklenir.
* **SP\_ekleme\_iş** adlı bir iş oluşturur _başvuru verileri dağıtımı_.
* **SP\_ekleme\_jobstep** VenueTypes başvuru tablosunu güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur.
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değerini gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** tüm hedef veritabanlarına işi bittiğinde belirlemek için sütun.

Betik tamamladıktan sonra güncelleştirilen başvuru verileri doğrulayabilirsiniz.  SSMS'de, Gözat *contosoconcerthall* veritabanına *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusu ve sorgu *VenueTypes* tablo.  Bu maddeyi *motosiklet yarışı* ve *yüzme kulübü* **olan** artık sunar.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırmada başvuru tablosu birincil anahtarında dizini yeniden oluşturmak için bir işi kullanır.  Büyük miktarlarda veri yüklendikten sonra yapılabilir bir tipik veritabanı bakım işlemi budur.

Aynı iş “sistem” saklı yordamlarını kullanarak bir iş oluşturun.

1. SSMS'yi açın ve bağlanma _Kataloğu-dpt -&lt;kullanıcı&gt;. database.windows.net_ sunucusu
1. Dosyayı açmak _... \\Öğrenme modülleri\\Şema Yönetimi\\OnlineReindex.sql_
1. Sağ tıklayın, bağlantıyı seçin ve bağlanma _Kataloğu-dpt -&lt;kullanıcı&gt;. database.windows.net_ henüz bağlı değilseniz sunucusu
1. Bağlı olduğunuzdan emin olun _jobagent_ veritabanı ve ENTER tuşuna **F5** betiği çalıştırmak için

Aşağıdaki öğeleri inceleyin _OnlineReindex.sql_ betiği:
* **SP\_ekleme\_iş** adında yeni bir iş oluşturur "Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885"
* **SP\_ekleme\_jobstep** dizini güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur
* Betikteki kalan görünümler, işin yürütülüşünü izler. Durum değerini gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** işi başarıyla tamamlandı tüm hedef grup üyeleri üzerinde ne zaman belirlemek için sütun.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * Birden çok veritabanı T-SQL işleri çalıştırmak için bir İş Aracısı oluşturma
> * Başvuru tüm Kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, deneyin [öğretici Ad hoc raporlama](saas-tenancy-cross-tenant-reporting.md) dağıtılmış sorgular kiracıda veritabanları çalıştıran keşfetmek için.


## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip bilet SaaS her Kiracı veritabanı uygulama dağıtımına dayalı ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Ölçeği artırılmış bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md)
* [Ölçeği artırılmış bulut veritabanları oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)