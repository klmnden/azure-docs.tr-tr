---
title: Çok kiracılı bir uygulamada Azure SQL Veritabanı şemasını yönetme | Microsoft Docs
description: Azure SQL Veritabanı’nı kullanan çok kiracılı bir uygulamada birden fazla kiracı için Şemayı yönetme
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 2e4af3e3e1ef1d9da7c66b929885e3ec749b462f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646281"
---
# <a name="manage-schema-in-a-saas-application-using-the-database-per-tenant-pattern-with-azure-sql-database"></a>Kiracı başına veritabanı desen ile Azure SQL veritabanı kullanarak bir SaaS uygulaması şemada yönetme

Veritabanı uygulaması geliştikçe değişiklikleri kaçınılmaz olarak veritabanı şeması ya da başvuru verileri yapılması gerekir.  Ayrıca, veritabanı bakım görevlerini düzenli aralıklarla gereklidir. Kiracı deseni başına veritabanı kullanan bir uygulama yönetimi, bir kiracı veritabanı Donanma bu değişiklikleri veya bakım görevlerini geçerli gerektirir.

Bu öğretici iki senaryo - tüm kiracılar için başvuru veri güncelleştirmelerini dağıtma inceler ve başvuru verileri içeren tablo üzerinde bir dizini yeniden oluşturma. [Esnek iş](sql-database-elastic-jobs-overview.md) özelliği, tüm Kiracı veritabanları ve yeni Kiracı veritabanları oluşturmak için kullanılan şablon veritabanı bu eylemleri yürütmek için kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]

> * Bir İş Aracısı oluşturun
> * Tüm Kiracı veritabanları çalıştırılması için T-SQL işleri neden
> * Tüm Kiracı veritabanları başvuru verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Başına Wingtip biletleri SaaS veritabanı Kiracı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanı keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> Bu öğretici bir sınırlı (esnek veritabanı iş) önizlemede SQL veritabanı hizmetinin özelliklerini kullanır. Bu öğretici yapmak isterseniz, abonelik Kimliğinizi sağlamak SaaSFeedback@microsoft.com esnek işleri Önizleme konuyla =. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ile ilgili sorular veya destek.

## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS şema yönetimi desenleri giriş

Kiracı deseni başına veritabanı Kiracı verileri etkili bir şekilde yalıtır ancak yönetmek ve korumak için veritabanlarının sayısını artırır. [Esnek iş](sql-database-elastic-jobs-overview.md) yönetim ve SQL veritabanları yönetimini kolaylaştırır. İşlerini güvenli ve güvenilir bir şekilde, görevleri (T-SQL komut dosyaları) grubu karşı çalıştırmak etkinleştirin. İşlerini, şema ve ortak başvuru veri değişikliklerini, uygulamadaki tüm Kiracı veritabanları arasında dağıtabilirsiniz. Esnek iş de kullanılabilir korumak için bir *şablonu* her zaman sağlayarak, yeni kiracılar oluşturmak için kullanılan veritabanı, en son şema ve başvuru verileri içermiyor.

![ekran](media/saas-tenancy-schema-management/schema-management-dpt.png)


## <a name="elastic-jobs-limited-preview"></a>Esnek İşler sınırlı önizlemesi

Azure SQL veritabanı'nın tümleşik bir özellik sunulmuştur esnek iş yeni bir sürümü var. Esnek İşler’in bu yeni sürümü, şu anda sınırlı önizlemeyle sunulmaktadır. Bu sınırlı Önizleme şu anda bir iş aracısı ve T-SQL işleri oluşturmak ve yönetmek için oluşturmak için PowerShell kullanarak destekler.

> [!NOTE]
> Bu öğretici bir sınırlı (esnek veritabanı iş) önizlemede SQL veritabanı hizmetinin özelliklerini kullanır. Bu öğretici yapmak isterseniz, abonelik Kimliğinizi sağlamak SaaSFeedback@microsoft.com esnek işleri Önizleme konuyla =. Aboneliğinizin etkinleştirildiğini belirten onayı aldıktan sonra, [en son ön sürüm işleri cmdlet’lerini indirip yükleyin](https://github.com/jaredmoo/azure-powershell/releases). Bu önizleme sınırlıdır, bu nedenle başvurun SaaSFeedback@microsoft.com ile ilgili sorular veya destek.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip biletleri SaaS veritabanı Kiracı uygulama betikler başına Al

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.

## <a name="create-a-job-agent-database-and-new-job-agent"></a>Veritabanı ve İş Aracısı'nın yeni bir İş Aracısı oluşturma

Bu öğretici, bir iş aracısı ve yedekleme işi Aracısı veritabanını oluşturmak için PowerShell kullanın gerektirir. İş Aracısı veritabanına iş tanımları, iş durumunu ve geçmişini tutar. İş Aracısı ve veritabanı oluşturulduktan sonra oluşturmak ve işleri hemen izleyin.

1. **PowerShell ıse'de**açın... \\Modülleri öğrenme\\Şema Yönetimi\\*Demo SchemaManagement.ps1*.
1. Betiği çalıştırmak için **F5**'e basın.

*Demo SchemaManagement.ps1* komut çağrıları *dağıtma SchemaManagement.ps1* adlı bir SQL veritabanı oluşturmak için komut dosyası *osagent* katalog sunucusunda. Ardından, veritabanını parametre olarak kullanan iş aracısı oluşturur.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tüm kiracılara yeni başvuru verilerini dağıtmak için bir iş oluşturma

Wingtip biletleri uygulamada, her bir kiracı veritabanı desteklenen salonundan türleri içerir. Her salonundan barındırılabilir ve bu uygulamada kullanılan arka plan görüntüsü belirleyen olayların türünü tanımlayan bir belirli salonundan türü değil. Uygulamanın yeni tür olay desteklemek için bu başvuru verileri eklenen güncelleştirilmiş ve yeni salonundan türler olmalıdır.  Bu alıştırmada, şu iki ek mekan türünü eklemek için tüm kiracı veritabanlarına yönelik bir güncelleştirmeyi dağıtacaksınız: *Motosiklet Yarışı* ve *Yüzme Kulübü*.

İlk olarak, her bir kiracı veritabanı içinde bulunan salonundan türlerini gözden geçirin. SQL Server Management Studio (SSMS) Kiracı veritabanlarından birini bağlanın ve VenueTypes tablosunu inceleyin.  Azure portalında, sorgu Düzenleyicisi'ni bu tabloda ayrıca Sorgulayabileceğiniz veritabanı sayfasından erişilebilir. 

1. SSMS açın ve Kiracı sunucuya bağlanın: *tenants1-dpt -&lt;kullanıcı&gt;. database.windows.net*
1. Onaylamak için *Motosikletinizin yarış* ve *yüzme kulübü* **değil** için şu anda dahil, Gözat _contosoconcerthall_ üzerinde veritabanı *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusunda ve sorgu *VenueTypes* tablo.

Şimdi güncelleştirmek için bir iş oluşturalım *VenueTypes* yeni salonundan türleri eklemek için tüm Kiracı veritabanları tablosunda.

İşlerini sistem saklı yordamları oluşturulmuş bir dizi kullandığınız yeni bir proje oluşturmak için _jobagent_ veritabanı İş Aracısı oluşturulduğu zaman.

1. SSMS, katalog sunucuya: *katalog-dpt -&lt;kullanıcı&gt;. database.windows.net* sunucu 
1. SSMS, Dosya Aç... \\Modülleri öğrenme\\Şema Yönetimi\\DeployReferenceData.sql
1. Deyim değiştirin: AYARLAMAK @wtpUser = &lt;kullanıcı&gt; ve başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtıldığında kullanılan kullanıcı değerini değiştirin
1. Bağlı olduğunuzdan emin olun _jobagent_ veritabanı ve tuşuna **F5** komut dosyasını çalıştırmak için

Aşağıdaki öğeleri inceleyin *DeployReferenceData.sql* komut dosyası:
* **SP\_ekleme\_hedef\_grup** hedef grup adı DemoServerGroup oluşturur.
* **SP\_ekleme\_hedef\_grup\_üye** hedef veritabanlarına kümesini tanımlamak için kullanılır.  İlk _tenants1-dpt -&lt;kullanıcı&gt;_  sunucu eklenir.  Sunucu hedefi olarak eklemek veritabanlarını bu sunucu işe dahil edilecek iş yürütme zaman neden olur. Ardından _basetenantdb_ veritabanı ve *adhocreporting* (daha sonra öğreticide kullanılan) veritabanı hedefleri olarak eklenir.
* **SP\_ekleme\_iş** adlı bir işi oluşturur _başvuru veri dağıtımı_.
* **SP\_ekleme\_iş** VenueTypes başvuru tablosu güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur.
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** tüm hedef veritabanlarına işi bittiğinde belirlemek için sütun.

Komut dosyası tamamlandıktan sonra başvuru verileri güncelleştirilmiş doğrulayabilirsiniz.  SSMS, Gözat *contosoconcerthall* üzerinde veritabanı *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusunda ve sorgu *VenueTypes* tablo.  Denetleyin *Motosikletinizin yarış* ve *yüzme kulübü* **olan** şimdi sunar.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Başvuru tablosu dizinini yönetmek için bir iş oluşturma

Bu alıştırmada, başvuru tablosunda birincil anahtar dizini yeniden oluşturmak için bir işi kullanır.  Bu, büyük miktarlarda verinin yüklendikten sonra yapılabilir bir tipik veritabanı bakım işlemdir.

Aynı iş “sistem” saklı yordamlarını kullanarak bir iş oluşturun.

1. SSMS açın ve bağlanmak _katalog-dpt -&lt;kullanıcı&gt;. database.windows.net_ sunucu
1. Dosyayı açmak _... \\Modülleri öğrenme\\Şema Yönetimi\\OnlineReindex.sql_
1. Sağ tıklayın, bağlantıyı seçin ve bağlanmak _katalog-dpt -&lt;kullanıcı&gt;. database.windows.net_ henüz bağlı sunucu
1. Bağlı olduğunuzdan emin olun _jobagent_ veritabanı ve tuşuna **F5** komut dosyasını çalıştırmak için

Aşağıdaki öğeleri inceleyin _OnlineReindex.sql_ komut dosyası:
* **SP\_ekleme\_iş** adlı yeni bir iş oluşturur "çevrimiçi arat PK\_\_VenueTyp\_\_265E44FD7FD4C885"
* **SP\_ekleme\_iş** dizini güncelleştirmek için T-SQL komut metni içeren işi adımı oluşturur
* Kalan görünümlerde betik iş yürütme izleyin. Durum değeri gözden geçirmek için bu sorguları kullanmak **yaşam döngüsü** işi başarıyla tamamlandı tüm hedef grup üyeleri zaman belirlemek için sütun.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Birden fazla veritabanı T-SQL işlerini çalıştırmak için bir İş Aracısı oluşturun
> * Tüm Kiracı veritabanları başvuru verileri güncelleştirme
> * Tüm kiracı veritabanlarında bir tabloda dizin oluşturma

Ardından, deneyin [geçici raporlama öğretici](saas-tenancy-cross-tenant-reporting.md) dağıtılmış sorgular arasında Kiracı veritabanları çalıştıran keşfetmek için.


## <a name="additional-resources"></a>Ek kaynaklar

* [Derleme başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtımı sırasında ek öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Ölçeği artırılmış bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md)
* [Ölçeği artırılmış bulut veritabanları oluşturma ve yönetme](sql-database-elastic-jobs-create-and-manage.md)