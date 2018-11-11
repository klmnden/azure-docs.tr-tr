---
title: Azure Machine Learning için Azure SQL veritabanı'na veri taşıma | Microsoft Docs
description: SQL tablo oluşturup SQL tablosuna veri yükleme
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 50f8b862-4d32-44b2-a1e2-4fbc8024acaa
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2018
ms.author: deguhath
ms.openlocfilehash: 5fa86387861ca87f9280a4c900d9dd83db5f1a39
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344544"
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Azure Machine Learning için Azure SQL Veritabanına veri taşıma

Bu makalede, verileri düz dosyaları (CSV veya TSV biçimleri) veya bir Azure SQL veritabanı için bir şirket içi SQL Server'da depolanan verileri taşımak için seçenekler özetlenmektedir. Verileri buluta taşımak için bu görevleri Team Data Science Process bir parçasıdır.

Machine Learning için bir şirket içi SQL Server'a veri taşıma için seçenekler özetlenmektedir bir konuya bakın [veri taşıma için SQL Server Azure sanal makinesinde](move-sql-server-virtual-machine.md).

Aşağıdaki tabloda, bir Azure SQL veritabanı'na veri taşımak için seçenekler özetlenmektedir.

| <b>KAYNAK</b> | <b>Hedef: Azure SQL veritabanı</b> |
| --- | --- |
| <b>Düz dosya (CSV veya biçimlendirilmiş TSV)</b> |[Toplu ekleme SQL sorgusu](#bulk-insert-sql-query) |
| <b>Şirket içi SQL Server</b> |1.[dışarı aktarmak için düz dosya](#export-flat-file)<br> 2. [SQL veritabanı Geçiş Sihirbazı](#insert-tables-bcp)<br> 3. [Yedekleme geri veritabanı ve geri yükleme](#db-migration)<br> 4. [Azure Data Factory](#adf) |

## <a name="prereqs"></a>Önkoşullar
Burada özetlenen yordamlar sahip olmanızı gerektirir:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verilerin depolanması için bir Azure depolama hesabını kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz: [depolama erişim anahtarlarınızı yönetme](../../storage/common/storage-account-manage.md#access-keys).
* Erişim bir **Azure SQL veritabanı**. Bir Azure SQL veritabanı, ayarlamanız gerekirse [Microsoft Azure SQL veritabanı ile çalışmaya başlama](../../sql-database/sql-database-get-started.md) Azure SQL veritabanı yeni bir örneğini sağlama hakkında bilgi sağlar.
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

**Veri**: kullanarak geçiş işlemleri gösterilmiştir [NYC taksi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/). NYC taksi dataset seyahat verilerini ve fairs ilgili bilgiler içerir ve Azure blob depolama alanında kullanılabilir: [NYC taksi verileri](http://www.andresmh.com/nyctaxitrips/). Bir örnek ve açıklama bu dosyaların sağlanan [NYC taksi Gelişlerin veri kümesi tanımı](sql-walkthrough.md#dataset).

Kendi veri kümesine burada açıklanan yordamlar uyarlayabilir veya NYC taksi veri kümesini kullanarak açıklanan adımları izleyin. Şirket içi SQL Server veritabanınıza NYC taksi veri kümesini yüklemek için bölümünde açıklanan yordamı izleyin [toplu içeri aktarma verileri SQL Server veritabanına](sql-walkthrough.md#dbload). SQL Server üzerinde bir Azure sanal makine için bu yönergeleri yöneliktir, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="file-to-azure-sql-database"></a> Bir Azure SQL veritabanı için bir düz dosya kaynaktan veri taşıma
Düz dosyaları (CSV veya TSV biçimli) verileri toplu ekleme bir SQL sorgusunu kullanarak bir Azure SQL veritabanına taşınabilir.

### <a name="bulk-insert-sql-query"></a> Toplu ekleme SQL sorgusu
Adımları için yordamı toplu ekleme SQL sorgusunu kullanarak verileri düz dosya kaynağından bir Azure sanal makinesinde SQL Server'a taşımak için aşağıdaki bölümlerde ele benzerdir. Ayrıntılar için bkz [toplu ekleme SQL sorgusu](move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a> Verileri şirket içi SQL Server'dan Azure SQL veritabanına taşıma
Kaynak veriler bir şirket içi SQL Server'da depolanır, verileri bir Azure SQL veritabanına taşımak için çeşitli olasılık vardır:

1. [Düz dosyaya](#export-flat-file)
2. [SQL veritabanı Geçiş Sihirbazı](#insert-tables-bcp)
3. [Yedekleme geri veritabanı ve geri yükleme](#db-migration)
4. [Azure Data Factory](#adf)

İlk üç için bu bölümlerdeki çok benzer adımlarla [veri taşıma için SQL Server Azure sanal makinesinde](move-sql-server-virtual-machine.md) aynı yordamları içerir. Bu konudaki uygun bölümleri bağlantılar aşağıdaki yönergelerde yer verilmiştir.

### <a name="export-flat-file"></a>Düz dosyaya
Bu düz bir dosyaya dışa aktarmak için bu ele benzer adımlarla [dışarı aktarmak için düz dosya](move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>SQL veritabanı Geçiş Sihirbazı
SQL veritabanı Geçiş Sihirbazı'nı kullanma adımları ele kişilere benzer [SQL veritabanı Geçiş Sihirbazı'nı](move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Yedekleme geri veritabanı ve geri yükleme
Veritabanı kullanma adımları'ı yedekleme ve geri yükleme olanlar ele benzer [arka veritabanı yedeklemek ve geri yükleme](move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Azure veri fabrikası
Azure Data Factory (ADF) ile bir Azure SQL veritabanına veri taşıma yordamı konu başlığı altında sağlanan [veri taşıma bir şirket içi SQL Server'dan Azure Data Factory ile SQL Azure için](move-sql-azure-adf.md). Bu konu, ADF kullanarak, verileri bir şirket içi SQL Server veritabanından Azure Blob Depolama bir Azure SQL veritabanına taşıma gösterir.

ADF veri hem şirket içi erişen bir karma senaryoda sürekli olarak geçirilmesi ve bulut kaynaklarına ihtiyacı olduğunda ve veriler işlem temelli veya değiştirilmesi veya kendisine geçirilen eklenmesi iş mantığı kullanmayı düşünün. Zamanlama ve düzenli aralıklarla veri taşıma işlemlerini yönetmek basit JSON betiklerini kullanarak işleri izlemek için ADF sağlar. ADF karmaşık işlemleri desteği gibi diğer özellikleri de vardır.
