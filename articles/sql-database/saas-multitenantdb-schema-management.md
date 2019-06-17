---
title: Çok kiracılı bir uygulamada Azure SQL Veritabanı şemasını yönetme | Microsoft Docs
description: Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 07e8fce5fd8db5d2070b8e382a0eba2ae7187b0d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242781"
---
# <a name="manage-schema-in-a-saas-application-that-uses-sharded-multi-tenant-sql-databases"></a>Parçalı çok kiracılı SQL veritabanlarını kullanan bir SaaS uygulamasında Şemayı yönetme

Bu öğreticide, bir hizmet (SaaS) uygulaması olarak bir yazılım veritabanlarında filosundan koruma karşılaşılan inceler. Çözümler, şema değişiklikleri filosundan veritabanları arasında fanning için gösterilir.

Herhangi bir uygulama gibi Wingtip bilet SaaS uygulaması zamanla gelişecek ve veritabanında değişiklikler yapılmasını gerektirecektir. Değişiklik etkisi şema ya da başvuru verileri veya veritabanı bakım görevlerini uygulamak. Kiracı deseni başına bir veritabanı'nı kullanan bir SaaS uygulaması ile değişiklikler oldukça büyük olabilecek filosundan Kiracı veritabanları arasında Eşgüdümlü gerekir. Ayrıca, bu değişiklikleri sağlama işlemi oluşturuldukları sırada yeni veritabanları içerdiği emin olmak için veritabanına eklemeniz gerekir.

#### <a name="two-scenarios"></a>İki senaryo

Bu öğretici, aşağıdaki iki senaryoda açıklar:
- Tüm kiracılar için başvuru verisi güncelleştirmelerini dağıtın.
- Başvuru verilerini içeren tabloda bir dizini yeniden oluşturun.

[Esnek işler](elastic-jobs-overview.md) özelliği, Azure SQL veritabanı, Kiracı veritabanlarında bu işlemleri yürütmek için kullanılır. İşler 'şablon' Kiracı veritabanı üzerinde de çalışır. Wingtip bilet örnek uygulamada, yeni bir kiracı veritabanı sağlamak için bu şablonu veritabanı kopyalanır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * İş Aracısı oluşturun.
> * T-SQL sorgusu birden çok Kiracı veritabanlarında yürütün.
> * Tüm Kiracı veritabanlarında başvuru verileri güncelleştirin.
> * Tüm Kiracı veritabanlarında bir tabloda bir dizin oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Wingtip bilet çok kiracılı veritabanı uygulaması zaten dağıtılmış olması gereken:
    - Wingtip bilet SaaS çok kiracılı veritabanı uygulama tanıtır ilk öğreticide yönergeler için bkz:<br />[Azure SQL veritabanı kullanan parçalı bir çok kiracılı uygulamasını dağıtma ve keşfetme](saas-multitenantdb-get-started-deploy.md).
        - Dağıtım işlemi beş dakikadan kısa bir süre için çalışır.
    - Olmalıdır *parçalı çok kiracılı* Wingtip yüklü sürümü. Sürümleri için *tek başına* ve *Kiracı başına veritabanı* Bu öğretici desteklemez.

- SQL Server Management Studio (SSMS) en son sürümü yüklü olmalıdır. [İndirme ve yükleme SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell yüklü olması gerekir. Ayrıntılar için bkz [Azure PowerShell'i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Bu öğreticide Azure SQL veritabanı hizmetinin sınırlı Önizleme özellikleri ([elastik veritabanı işleri](sql-database-elastic-database-client-library.md)). Bu öğreticiyi uygulamak istiyorsanız, abonelik Kimliğinizi sağlamanız *SaaSFeedback\@microsoft.com* konuyla esnek işler önizlemesi yazarak. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun *SaaSFeedback\@microsoft.com* ilgili sorular veya destek için.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetimi düzenlerine giriş

Bu örnekte kullanılan parçalı çok kiracılı veritabanı modeli, kiracılar bir veya daha fazla içerecek şekilde bir kiracı veritabanı sağlar. Bu örnek etkinleştirme bir çok kiracılı ve bir kiracı veritabanları bir karışımını kullanmak için olası keşfediyor bir *karma* kiracı yönetim modeli. Bu veritabanları için değişiklik Yönetimi karmaşık olabilir. [Esnek işler](elastic-jobs-overview.md) çok sayıda veritabanı yönetimini kolaylaştırır. İşleri Kiracı veritabanlarından oluşan bir grupta karşı görevler olarak güvenli ve güvenilir bir şekilde Transact-SQL betikleri çalıştırmanıza olanak sağlar. Görevleri, kullanıcı etkileşimi veya girişi bağımsızdır. Bu yöntem, değişiklikleri bir uygulamadaki tüm kiracılar genelinde Şeması veya ortak başvuru verilerini dağıtmak için kullanılabilir. Esnek işler, veritabanının altın şablonunun bir kopyasını korumak için de kullanılabilir. Şablon, her zaman en son şema yeni Kiracı oluşturmak için kullanılır ve başvuru verileri kullanılıyor.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Artık Azure SQL veritabanı'nın tümleşik bir özelliği olan esnek işler, yeni bir sürümü var. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Sınırlı Önizleme şu anda bir iş aracısı ve T-SQL işleri oluşturmak ve yönetmek için oluşturmak için PowerShell kullanılmasını destekler.
> [!NOTE]
> Bu öğreticide SQL veritabanı hizmetinin sınırlı Önizleme (elastik veritabanı işleri) özellikleri kullanılır. Bu öğreticiyi uygulamak istiyorsanız, abonelik Kimliğinizi sağlamanız SaaSFeedback@microsoft.com konuyla esnek işler önizlemesi yazarak. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra indirin ve en son ön sürüm işleri cmdlet'lerini yükleyin. Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ilgili sorular veya destek için.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip bilet SaaS çok kiracılı veritabanı uygulama kaynak kodu ve betikleri Al

Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) github deposu. Bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>İş Aracısı veritabanı ve yeni iş Aracısı oluşturma

Bu öğretici, İş Aracısı ve İş Aracısı veritabanı oluşturmak için PowerShell kullanmanızı gerektirir. SQL aracısı tarafından kullanılan MSDB veritabanına iş tanımlarını, iş durumu ve geçmişi depolamak için bir Azure SQL veritabanı İş Aracısı kullanır. İş Aracısı oluşturulduktan sonra oluşturma ve işleri hemen izleyin.

1. İçinde **PowerShell ISE**açın *... \\Öğrenme modülleri\\Şema Yönetimi\\Demo-SchemaManagement.ps1*.
2. Betiği çalıştırmak için **F5**'e basın.

*Demo-SchemaManagement.ps1* komut çağrıları *Deploy-SchemaManagement.ps1* adlı bir veritabanı oluşturmak için betik _jobagent_ katalog sunucusunda. Betik daha sonra geçen İş Aracısı oluşturur _jobagent_ veritabanı bir parametre olarak.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

#### <a name="prepare"></a>Hazırlama

Her bir kiracının veritabanı mekan türleri kümesi içerir **VenueTypes** tablo. Her mekan türü bir hata mekanda olay türünü tanımlar. Bu mekan türleri, Kiracı etkinlikleri uygulamasında gördüğünüz arka plan görüntüleri karşılık gelir.  Bu alıştırmada, tüm veritabanları için iki ek mekan türünü eklemek için bir güncelleştirme dağıtın: *Motosiklet yarışı* ve *yüzme kulübü*.

İlk olarak, her bir kiracı veritabanında bulunan mekan türlerini gözden geçirin. Kiracı veritabanlarını SQL Server Management Studio (SSMS) birine bağlanın ve VenueTypes tabloyu inceleyin.  Ayrıca, Azure portalındaki sorgu Düzenleyicisi'nde bu tabloda sorgulayabilirsiniz veritabanı sayfasından erişilebilir.

1. SSMS'yi açın ve Kiracı sunucuya: *tenants1-dpt -&lt;kullanıcı&gt;. database.windows.net*
1. Onaylamak için *motosiklet yarışı* ve *yüzme kulübü* **değil** Gözat şu anda dahil, *contosoconcerthall* veritabanına *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusu ve sorgu *VenueTypes* tablo.



#### <a name="steps"></a>Adımlar

Güncelleştirilecek bir proje oluşturmak şimdi **VenueTypes** iki yeni mekan türlerini ekleyerek, her Kiracı veritabanı tablosunda.

Yeni bir proje oluşturmak için oluşturulan işleri sistem saklı yordamlarına kümesini kullanın. _jobagent_ veritabanı. İş Aracısı oluşturulduğunda, saklı yordamlar oluşturuldu.

1. SSMS'de, Kiracı sunucuya: tenants1-mt -&lt;kullanıcı&gt;. database.windows.net

2. Gözat *tenants1* veritabanı.

3. Sorgu *VenueTypes* onaylamak için tablo *motosiklet yarışı* ve *yüzme kulübü* henüz sonuç listesinde değil.

4. Olan katalog sunucusuna bağlanma *Kataloğu-mt -&lt;kullanıcı&gt;. database.windows.net*.

5. Bağlanma _jobagent_ katalog sunucusunda veritabanı.

6. SSMS'de, dosyayı açma *... \\Öğrenme modülleri\\Şema Yönetimi\\deployreferencedata.SQL öğesini*.

7. Deyimi değiştirin: ayarlayın @User = &lt;kullanıcı&gt; ve Wingtip bilet SaaS çok kiracılı veritabanı uygulamasını dağıtırken kullandığınız kullanıcı değerini değiştirin.

8. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemleyin

Aşağıdaki öğeleri inceleyin *deployreferencedata.SQL öğesini* betiği:

- **SP\_ekleme\_hedef\_grubu** hedef grubu adını oluşturur *Demoservergroup'u*, ve hedef üyeleri gruba ekler.

- **SP\_ekleme\_hedef\_grubu\_üye** aşağıdakileri ekler:
    - A *sunucu* hedef üye türü.
        - Bu *tenants1-mt -&lt;kullanıcı&gt;*  Kiracı veritabanlarını içeren bir sunucu.
        - Sunucu dahil olmak üzere iş yürütür sırada mevcut Kiracı veritabanlarını içerir.
    - A *veritabanı* hedef şablon veritabanı için üye türü (*basetenantdb*) üzerinde bulunan *Kataloğu-mt -&lt;kullanıcı&gt;*  sunucusu
    - A *veritabanı* hedef üye türü içerecek şekilde *adhocreporting* sonraki bir eğitimde kullanılan veritabanı.

- **SP\_ekleme\_iş** adında bir iş oluşturur *başvuru verileri dağıtımı*.

- **SP\_ekleme\_jobstep** VenueTypes başvuru tablosunu güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur.

- Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değerini gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** işin ne zaman sona belirlemek için sütun. İş kiracılar veritabanını güncelleştirir ve başvuru tablosunu içeren iki ek veritabanında güncelleştirir.

SSMS'de, Kiracı veritabanına gözatın *tenants1-mt -&lt;kullanıcı&gt;*  sunucusu. Sorgu *VenueTypes* onaylamak için tablo *motosiklet yarışı* ve *yüzme kulübü* artık tablosuna eklenir. Mekan türleri toplam sayısı ikiye artırılması.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırmada, tüm Kiracı veritabanlarında başvuru tablosu birincil anahtarında dizini yeniden oluşturmak için bir iş oluşturur. Bir dizini yeniden yönetici çalışabilecek bir tipik veritabanı yönetim işlemi durumda çok miktarda veri yüklemesi, performansı artırmak için yükleme sonrasında.

1. SSMS'de bağlanma _jobagent_ veritabanını *Kataloğu-mt -&lt;kullanıcı&gt;. database.windows.net* sunucusu.

2. SSMS'de açın *... \\Öğrenme modülleri\\Şema Yönetimi\\OnlineReindex.sql*.

3. Betiği çalıştırmak için **F5**'e basın.

#### <a name="observe"></a>Gözlemleyin

Aşağıdaki öğeleri inceleyin *OnlineReindex.sql* betiği:

* **SP\_ekleme\_iş** adında yeni bir iş oluşturur *çevrimiçi yeniden PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_ekleme\_jobstep** dizini güncelleştirecek T-SQL komut metnini içeren iş adımını oluşturur.

* Betikteki kalan görünümler, işin yürütülüşünü izler. Durum değerini gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** işi başarıyla tamamlandı tüm hedef grup üyeleri üzerinde ne zaman belirlemek için sütun.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Ölçeği artırılmış bulut veritabanlarını yönetme](elastic-jobs-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Birden çok veritabanı T-SQL işleri çalıştırmak için bir İş Aracısı oluşturma
> * Başvuru tüm Kiracı veritabanlarında verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, deneyin [öğretici Ad hoc raporlama](saas-multitenantdb-adhoc-reporting.md) dağıtılmış sorgular kiracıda veritabanları çalıştıran keşfetmek için.

