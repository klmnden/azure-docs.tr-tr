---
title: "Birden çok kiracı için Şemayı yönetme (Azure SQL Veritabanı’nı kullanan örnek SaaS uygulaması) | Microsoft Belgeleri"
description: "Azure SQL Veritabanı’nı kullanan SaaS uygulamasında birden fazla kiracı için Şemayı yönetme"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 226cda254934fae30410e54148d5cc527e1c7881
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="manage-schema-for-multiple-tenants-in-the-wtp-saas-application"></a>WTP SaaS uygulamasında birden fazla kiracı için şemayı yönetme

WTP Uygulamasına giriş öğreticisinde, WTP uygulamasının kiracı veritabanına ilk şemasını sağlaması ve bunu kataloğa kaydetmesi gösterilmiştir. Herhangi bir uygulamada olduğu gibi, WTP uygulaması zamanla gelişecek ve bazen veritabanında değişiklikler yapılmasını gerektirecektir. Değişiklikler; yeni veya değiştirilmiş şema, yeni ya da değiştirilmiş başvuru verileri ve en iyi uygulama performansını sağlayan rutin veritabanı bakım görevlerini içerebilir. SaaS uygulamasıyla, bu değişikliklerin oldukça büyük olabilecek bir kiracı veritabanı filosunda eşgüdümlü şekilde dağıtılması gerekir. Değişiklikler, gelecekteki kiracı veritabanları için sağlama işlemine de eklenmelidir.

Bu öğreticide iki senaryo incelenmektedir: tüm kiracılar için başvuru verisi güncelleştirmelerini dağıtma ve başvuru verilerini içeren tabloda bir dizin döndürme. [Esnek işler](sql-database-elastic-jobs-overview.md) özelliği, tüm kiracılar genelinde bu işlemleri ve yeni veritabanlarının şablonu olarak kullanılan *altın* kiracı veritabanını yürütmek amacıyla kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Birden fazla kiracı genelinde sorgulama yapmak için bir iş hesabı oluşturma
> * Tüm kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* WTP uygulamasının dağıtıldığından. Beş dakikadan daha kısa sürede dağıtmak için [WTP SaaS uygulamasını dağıtma ve keşfetme](sql-database-saas-tutorial.md) bölümünü inceleyin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi tamamlamak istiyorsanız konu satırına Esnek İşler Önizlemesi yazarak abonelik kimliğinizi SaaSFeedback@microsoft.com adresine gönderin. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu sınırlı bir önizleme olduğundan, ilgili sorular veya destek için SaaSFeedback@microsoft.com ile iletişim kurmanız gerekir.*


## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS Şema Yönetimi düzenlerine giriş

Veritabanı başına tek kiracı SaaS düzeni, elde edilen veri yalıtımından birçok farklı şekilde yararlanır, ancak aynı zamanda çok sayıda veritabanının korunması ve yönetilmesi için süreci daha karmaşık hâle getirir. [Esnek işler](sql-database-elastic-jobs-overview.md), SQL veri katmanının yönetimini kolaylaştırır. İşler, bir veritabanı grubuna yönelik kullanıcı etkileşimi veya girişlerinden bağımsız olarak güvenilir bir şekilde görev (T-SQL betikleri) çalıştırmanıza imkan tanır. Bu yöntemi kullanılarak bir uygulamadaki tüm kiracılara şema ve ortak başvuru verisi değişikliklerini dağıtılabilir. Esnek İşler, yeni kiracı oluşturmak amacıyla kullanılan veritabanının *altın* kopyasını tutmak ve her zaman en yeni şemaya ve başvuru verilerine sahip olmasını sağlamak için de kullanılabilir.

![ekran](media/sql-database-saas-tutorial-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Esnek İşler’in artık Azure SQL Veritabanı’nın (ek hizmet veya bileşen gerektirmeyen) tümleşik bir özelliği olan yeni bir sürümü vardır. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Bu sınırlı önizleme, iş hesapları oluşturmak için PowerShell’i ve işleri oluşturmak ve yönetmek için T-SQL’i desteklemektedir.

> [!NOTE]
> *Bu öğretici, SQL Veritabanı hizmetinin sınırlı önizleme (Elastik Veritabanı işleri) olarak sunulan özelliklerini kullanır. Bu öğreticiyi tamamlamak istiyorsanız konu satırına Esnek İşler Önizlemesi yazarak abonelik kimliğinizi SaaSFeedback@microsoft.com adresine gönderin. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu sınırlı bir önizleme olduğundan, ilgili sorular veya destek için SaaSFeedback@microsoft.com ile iletişim kurmanız gerekir.*

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="create-a-job-account-database-and-new-job-account"></a>İş hesabı veritabanı ve yeni bir iş hesabı oluşturma

Bu öğretici, iş hesabı veritabanını ve iş hesabını oluşturmak için PowerShell kullanmanızı gerektirir. MSDB ve SQL Aracısı’nda olduğu gibi, Esnek İşler de iş tanımlarını, iş durumunu ve geçmişi depolamak için Azure SQL veritabanını kullanır. İş hesabı oluşturulduktan sonra, işleri hemen oluşturabilir ve izleyebilirsiniz.

1. **PowerShell ISE**’de …\\Öğrenme Modülleri\\Şema Yönetimi\\*Demo-SchemaManagement.ps1* öğesini açın.
1. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* betiği, katalog sunucusunda **jobaccount** adlı bir *S2* veritabanı oluşturmak için *Deploy-SchemaManagement.ps1* betiğini çağırır. Ardından jobaccount veritabanını bir parametre olarak iş hesabı oluşturma çağrısına aktararak iş hesabını oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

Her kiracı veritabanı, bir mekanda düzenlenen etkinliklerin türünü tanımlamak için bir mekan türü kümesi içerir. Bu alıştırmada, şu iki ek mekan türünü eklemek için tüm kiracı veritabanlarına yönelik bir güncelleştirmeyi dağıtacaksınız: *Motosiklet Yarışı* ve *Yüzme Kulübü*. Bu mekan türleri, kiracı etkinlikleri uygulamasında gördüğünüz arka plan görüntüsüne karşılık gelir.

Mekan Türü açılır menüsüne tıklayın ve yalnızca 10 mekan türü seçeneğinin kullanılabildiğini ve özellikle “Motosiklet Yarışı” ve “Yüzme Kulübü”nün listede yer almadığını doğrulayın.

Şimdi tüm kiracı veritabanlarında *VenueTypes* tablosunu güncelleştirmek ve yeni mekan türlerini eklemek için bir iş oluşturalım.

Yeni bir iş oluşturmak için iş hesabı oluşturulduğunda jobaccount veritabanında oluşturulmuş iş grubu sistemi saklı yordamlarını kullanırız.

1. SSMS’yi açın ve catalog-\<user\>.database.windows.net katalog sunucusuna bağlanın
1. tenants1-\<user\>.database.windows.net kiracı sunucusuna da bağlanın
1. *tenants1* sunucusunda *contosoconcerthall* veritabanına göz atın ve *Motosiklet Yarışı* ve *Yüzme Kulübü*’nün sonuç listesinde **yer almadığını** onaylamak için *VenueTypes* tablosunu sorgulayın.
1. …\\Öğrenme Modülleri\\Şema Yönetimi\\DeployReferenceData.sql öğesini açın
1. Betikteki 3 konumda da WTP uygulamasını dağıtırken kullandığınız kullanıcı adını kullanarak \<user\> öğesini değiştirin
1. jobaccount veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için **F5**’e basın

* **sp\_add\_target\_group**, hedef grubu adı DemoServerGroup’u oluşturur, artık hedef üyeleri eklememiz gerekiyor.
* **sp\_add\_target\_group\_member**, sunucu (bu, kiracı veritabanlarını içeren customer1-&lt;WtpUser&gt; sunucusudur) içindeki tüm veritabanlarının iş yürütülürken işe dahil edilmesi gerektiğini varsayan *server* hedef üyesini ekler. İkinci olarak *database* hedef üyesi türü, özellikle de “altın” veritabanı olan ve catalog-&lt;WtpUser&gt; sunucusunda bulunan baseTenantDB ve son olarak sonraki öğreticide kullanılan adhocanalytics veritabanını içeren başka bir *database* hedef grup üye türü eklenir.
* **sp\_add\_job**, “Başvuru Verileri Dağıtımı” adında bir iş oluşturur
* **sp\_add\_jobstep**, VenueTypes başvuru tablosunu güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. **Yaşam döngüsü** sütunundaki durum değerini gözden geçirin. İş, tüm kiracı veritabanlarında ve başvuru tablosunu içeren iki ek veritabanında başarıyla tamamlanmıştır.

1. SSMS’de, *tenants1* sunucusundaki *contosoconcerthall* veritabanına göz atın ve *Motosiklet Yarışı* ve *Yüzme Kulübü*’nün sonuç listesinde **yer aldığını** onaylamak için *VenueTypes* tablosunu sorgulayın.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Önceki alıştırmaya benzer şekilde, bu alıştırmada başvuru tablosu birincil anahtarında dizini yeniden oluşturmak için bir iş oluşturulur. Bu, tabloya büyük bir veri yüklendikten sonra bir yöneticinin gerçekleştirebileceği tipik bir veritabanı yönetim işlemidir.

Aynı iş “sistem” saklı yordamlarını kullanarak bir iş oluşturun.

1. SSMS’yi açın ve catalog-&lt;WtpUser&gt;.database.windows.net sunucusuna bağlanın
1. …\\Öğrenme Modülleri\\Şema Yönetimi\\OnlineReindex.sql dosyasını açın
1. Sağ tıklayıp Bağlantı’yı seçin ve henüz bağlı değilseniz catalog-&lt;WtpUser&gt;.database.windows.net sunucusuna bağlanın
1. jobaccount veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için F5’e basın

* sp\_add\_job, “Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885” adlı yeni bir iş oluşturur
* sp\_add\_jobstep, dizini güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur




## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Birden fazla kiracı genelinde sorgulama yapmak için bir iş hesabı oluşturma
> * Tüm kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

[Geçici analizler öğreticisi](sql-database-saas-tutorial-adhoc-analytics.md)


## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Ölçeği artırılmış bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md)
* [Ölçeği artırılmış bulut veritabanları oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)
