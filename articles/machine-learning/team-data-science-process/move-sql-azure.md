---
title: Azure Machine Learning için bir Azure SQL veritabanına veri taşıma | Microsoft Docs
description: SQL tablosu ve SQL tablosuna yük verileri oluşturma
services: machine-learning
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: 50f8b862-4d32-44b2-a1e2-4fbc8024acaa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2018
ms.author: deguhath
ms.openlocfilehash: ce349aedc6b733d34ab61eb2e23b378727e01800
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Azure Machine Learning için Azure SQL Veritabanına veri taşıma
Bu konu, verileri düz dosyalardan (CSV veya TSV biçimleri) veya bir Azure SQL veritabanı için bir şirket içi SQL Server'da depolanan veri taşıma seçeneklerini özetler. Verileri buluta taşımak için bu görevleri takım veri bilimi işleminin bir parçasıdır.

Bir şirket içi SQL Server'a Machine Learning için verileri taşıma seçeneklerini özetlenmektedir bir konuya bakın [veri taşıma SQL Server için bir Azure sanal makinede](move-sql-server-virtual-machine.md).

Aşağıdaki **menü** burada veri depolanabilir ve takım veri bilimi işlem (TDSP) sırasında işlenen hedef ortamlara veri alma nasıl açıklayan konulara bağlantılar.

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

Bir Azure SQL veritabanına veri taşıma seçenekleri aşağıdaki tabloda özetlenmiştir.

| <b>KAYNAK</b> | <b>Hedef: Azure SQL veritabanı</b> |
| --- | --- |
| <b>Düz dosya (CSV ya da biçimlendirilmiş TSV)</b> |[Toplu ekleme SQL sorgusu](#bulk-insert-sql-query) |
| <b>Şirket içi SQL Server</b> |1.[düz dosyasına dışarı aktarma](#export-flat-file)<br> 2. [SQL veritabanı Geçiş Sihirbazı](#insert-tables-bcp)<br> 3. [Yukarı geri veritabanı ve geri yükleme](#db-migration)<br> 4. [Azure Data Factory](#adf) |

## <a name="prereqs"></a>Önkoşullar
Burada gösterilen yordamları sahip olması gerekir:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verileri depolamak için bir Azure depolama hesabı kullanın. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz: [depolama erişim tuşlarınızı yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Erişim bir **Azure SQL veritabanı**. Bir Azure SQL veritabanı, ayarlarsanız, [Microsoft Azure SQL veritabanı ile çalışmaya başlama](../../sql-database/sql-database-get-started.md) bir Azure SQL veritabanına yeni bir örneğini sağlayacak bilgiler verilmektedir.
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

**Veri**: geçiş işlemleri kullanarak gösterilmiştir [NYC ücreti dataset](http://chriswhong.com/open-data/foil_nyc_taxi/). NYC ücreti dataset seyahat veri ve fairs hakkında bilgi içerir ve Azure blob depolama alanında kullanılabilir: [NYC ücreti verileri](http://www.andresmh.com/nyctaxitrips/). Bir örnek ve bu dosyaların açıklaması sağlanan [NYC ücreti dönüşleri veri kümesi tanımı](sql-walkthrough.md#dataset).

Kendi veri kümesine burada açıklanan yordamları uyum veya NYC ücreti dataset kullanarak açıklanan adımları izleyin. NYC ücreti veri kümesi şirket içi SQL Server veritabanınıza karşıya yüklemek için özetlenen yordamı izleyin [toplu içeri aktarma verileri SQL Server veritabanına](sql-walkthrough.md#dbload). SQL Server üzerinde bir Azure sanal makine için bu yönergeleri bağlıdır, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="file-to-azure-sql-database"></a> Bir Azure SQL veritabanına bir düz dosya kaynaktan veri taşıma
(CSV veya TSV biçimlendirilmiş) düz dosyalardaki veriler toplu Ekle bir SQL sorgusunu kullanarak bir Azure SQL veritabanına taşınabilir.

### <a name="bulk-insert-sql-query"></a> Toplu ekleme SQL sorgusu
Adımları toplu Ekle SQL sorgusunu kullanarak yordamı için verileri bir düz dosya kaynaktan SQL Server için bir Azure VM taşıma için bölümlerde ele benzerdir. Ayrıntılar için bkz [toplu Ekle SQL sorgusu](move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a> Şirket içi SQL Server'dan Azure SQL veritabanına veri taşıma
Veri kaynağını bir şirket içi SQL sunucusunda depolanan, bir Azure SQL veritabanına veri taşıma için çeşitli seçenekler vardır:

1. [Düz dosya dışarı aktarma](#export-flat-file)
2. [SQL veritabanı Geçiş Sihirbazı](#insert-tables-bcp)
3. [Yukarı geri veritabanı ve geri yükleme](#db-migration)
4. [Azure Data Factory](#adf)

İlk üç adımları Bu bölümlerdeki çok benzer [veri taşıma SQL Server için bir Azure sanal makinesi üzerinde](move-sql-server-virtual-machine.md) aynı yordamları kapsar. Bu konudaki uygun bölümleri bağlantılar aşağıdaki yönergelerde yer verilmiştir.

### <a name="export-flat-file"></a>Düz dosya dışarı aktarma
Bu düz bir dosyaya vermek için adımlar, bu ele benzer [verme düz dosya](move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>SQL veritabanı Geçiş Sihirbazı
SQL veritabanı Geçiş Sihirbazı'nı kullanmaya yönelik adımlar ele kişilere benzer [SQL veritabanı Geçiş Sihirbazı'nı](move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Yukarı geri veritabanı ve geri yükleme
Veritabanı kullanmaya yönelik adımlar yedeklemek ve geri yükleme olanlar ele benzer [geri veritabanı yedeklemek ve geri yükleme](move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Azure veri fabrikası
Azure veri fabrikası (ADF) ile Azure SQL veritabanına veri taşıma yordamı konusunda sağlanan [veri taşıma bir şirket içi SQL Server'dan Azure Data Factory ile SQL Azure](move-sql-azure-adf.md). Bu konuda, ADF kullanarak verileri bir şirket içi SQL Server veritabanından bir Azure SQL veritabanına Azure Blob Storage taşıma kullanmayı gösterir.

Hem şirket içi erişen bir karma senaryoda sürekli geçirilmesi ve kaynakların bulut verilere ihtiyaç duyduğunda ve veri işlem yapılan işlem ya da değiştirilmiş veya iş mantığı kendisine geçirilen zaman eklenmiş olması gerekiyor ADF kullanmayı düşünün. ADF zamanlama ve işleri düzenli aralıklarla veri hareketini yönetmek basit JSON komut dosyaları kullanarak izlenmesini sağlar. ADF karmaşık işlemleri için destek gibi diğer özellikleri de vardır.
