---
title: SQL veritabanı - Azure Blob depolamadan veri kopyalama | Microsoft Docs
description: Bu öğreticide verileri Blob depolama alanından SQL veritabanına kopyalamak için bir Azure Data Factory işlem hattında kopyalama etkinliği kullanma işlemini gösterir.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 557228bafc00c3028a1fda520da8fe4ec8c7a6f2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60487341"
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Öğretici: Data Factory kullanarak SQL veritabanına veri kopyalama Blob depolamadan
> [!div class="op_single_selector"]
> * [Genel bakış ve önkoşullar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz. [kopyalama etkinliği öğreticisi](../quickstart-create-data-factory-dot-net.md). 

Bu öğreticide, verileri Blob depolama alanından SQL veritabanına kopyalamak için bir işlem hattıyla veri fabrikası oluşturun.

Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Bu etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.  

> [!NOTE]
> Data Factory hizmetine ayrıntılı bir genel bakış için bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md) makalesi.
>
>

## <a name="prerequisites-for-the-tutorial"></a>Öğreticisi için Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

* **Azure aboneliği**.  Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Bkz: [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/) makale Ayrıntılar için.
* **Azure depolama hesabı**. Blob depolama alanı olarak kullanabilmeniz bir **kaynak** Bu öğreticide verileri depolayın. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın.
* **Azure SQL Veritabanı**. Bir Azure SQL veritabanı olarak kullandığınız bir **hedef** Bu öğreticide verileri depolayın. Bakın öğreticide kullandığınız bir Azure SQL veritabanınız yoksa [oluşturma ve bir Azure SQL veritabanı yapılandırma](../../sql-database/sql-database-get-started.md) oluşturmak için.
* **SQL Server 2012/2014 veya Visual Studio 2013**. SQL Server Management Studio veya Visual Studio örnek bir veritabanı oluşturun ve sonuç verilerini veritabanında görüntülemek için kullanın.  

## <a name="collect-blob-storage-account-name-and-key"></a>BLOB Depolama hesabı adı ve anahtarı Topla
Hesap adını ve bu öğreticiyi uygulamak için Azure depolama hesabınızın hesap anahtarı gerekir. Not **hesap adı** ve **hesap anahtarı** Azure depolama hesabınız için.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Tıklayın **tüm hizmetleri** seçin ve soldaki menüden **depolama hesapları**.

    ![Göz at - depolama hesapları](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. İçinde **depolama hesapları** dikey penceresinde **Azure depolama hesabı** , bu öğreticide kullanmak istiyorsunuz.
4. Seçin **erişim anahtarları** altında bağlantı **ayarları**.
5. Tıklayın **kopyalama** (görüntü) düğmesinin yanındaki **depolama hesabı adı** metin kutusu ve kaydetme/bunu yere yapıştırın (örneğin: bir metin dosyasında).
6. Kopyalayın veya not için önceki adımı yineleyin **key1**.

    ![Depolama erişim anahtarı](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Tıklayarak tüm dikey pencereleri kapatın **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, veritabanı, kullanıcı adları Topla
Azure SQL server, veritabanı ve bu öğreticiyi uygulamak için kullanıcı adları gerekir. Not edin adlarını **sunucu**, **veritabanı**, ve **kullanıcı** Azure SQL veritabanı.

1. İçinde **Azure portalında**, tıklayın **tüm hizmetleri** seçeneğini ve üzerinde **SQL veritabanları**.
2. İçinde **SQL veritabanları dikey**seçin **veritabanı** , bu öğreticide kullanmak istiyorsunuz. Not **veritabanı adı**.  
3. İçinde **SQL veritabanı** dikey penceresinde tıklayın **özellikleri** altında **ayarları**.
4. Not edin değerlerini **sunucu adı** ve **Sunucu Yöneticisi oturum açma**.
5. Tıklayarak tüm dikey pencereleri kapatın **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure hizmetlerinin SQL sunucusuna erişmesine izin ver
Emin **Azure hizmetlerine erişime izin ver** ayarı **ON** Azure SQL sunucunuzun böylece Data Factory hizmetinin Azure SQL sunucunuza erişebilir. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

1. Tıklayın **tüm hizmetleri** tıklayın ve sol hub'da **SQL sunucuları**.
2. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
3. **Güvenlik Duvarı ayarları** dikey penceresinde, **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.
4. Tıklayarak tüm dikey pencereleri kapatın **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>BLOB Depolama ve SQL veritabanınızı hazırlayın
Şimdi, Azure blob depolama ve Azure SQL veritabanı öğreticisi için aşağıdaki adımları uygulayarak hazırlayın:  

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve kaydedileceği **emp.txt** için **C:\ADFGetStarted** sabit sürücünüzdeki bir klasöre.

    ```
    John, Doe
    Jane, Doe
    ```
2. [Azure Storage Gezgini](https://storageexplorer.com/) gibi araçları **adftutorial** kapsayıcısı oluşturmak ve **emp.txt** dosyasını kapsayıcıya yüklemek için kullanın.

3. Azure SQL Veritabanı’nınizde **emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **SQL Server 2012/2014, bilgisayarınızda yüklü olup olmadığını:** makalesindeki yönergeleri uygulayın [yönetme Azure SQL Server Management Studio kullanarak SQL veritabanı](../../sql-database/sql-database-manage-azure-ssms.md) Azure SQL sunucunuza bağlanmak ve SQL betiğini çalıştırın. 

    İstemcinizin Azure SQL sunucusuna erişim izni yoksa, makinenizden (IP adresi) erişim izni vermek için Azure SQL sunucunuzun güvenlik duvarını yapılandırmanız gerekir. Azure SQL sunucunuzun güvenlik duvarını yapılandırmaya yönelik adımlar için [bu makaleye](../../sql-database/sql-database-configure-firewall-settings.md) bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Önkoşulları tamamladınız. Aşağıdaki yöntemlerden birini kullanarak veri fabrikası oluşturabilirsiniz. Öğreticiyi gerçekleştirme seçeneklerini aşağı açılan listesinde üst veya aşağıdaki bağlantılardan birine tıklayın.     

* [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
* [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Bu öğreticideki veri işlem hattı, bir kaynak veri deposundaki verileri hedef veri deposuna kopyalar. Çıkış verileri üretmek için giriş verilerini dönüştürmez. Azure Data Factory kullanarak verileri dönüştürme hakkında bir öğretici için bkz. [Öğreticisi: Hadoop kümesi kullanarak verileri dönüştürmek için ilk işlem hattınızı](data-factory-build-your-first-pipeline.md).
> 
> Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Ayrıntılı bilgi için bkz. [Data Factory’de zamanlama ve yürütme](data-factory-scheduling-and-execution.md). 
