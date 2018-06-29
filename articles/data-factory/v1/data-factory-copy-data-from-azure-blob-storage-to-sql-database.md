---
title: SQL veritabanı - Azure Blob depolama alanından verileri kopyalamak | Microsoft Docs
description: Bu öğretici, kopya etkinliği Azure Data Factory ardışık düzeninde verileri Blob depolama alanından SQL veritabanına kopyalamak için nasıl kullanılacağını gösterir.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4538e5b49b161f22ba6d5979234786a58cae5783
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37047735"
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Öğretici: verileri Blob depolama alanından Data Factory kullanarak SQL veritabanına kopyalamak
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
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [kopyalama etkinliği Öğreticisi](../quickstart-create-data-factory-dot-net.md). 

Bu öğreticide, verileri Blob depolama alanından SQL veritabanına kopyalamak için bir komut zinciriyle bir veri fabrikası oluşturun.

Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Bu etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.  

> [!NOTE]
> Data Factory hizmetinin ayrıntılı bir genel bakış için bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md) makalesi.
>
>

## <a name="prerequisites-for-the-tutorial"></a>Öğreticisi için Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

* **Azure aboneliği**.  Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Bkz: [ücretsiz deneme](http://azure.microsoft.com/pricing/free-trial/) Ayrıntılar için makale.
* **Azure depolama hesabı**. Blob depolama alanı olarak kullandığınız bir **kaynak** verileri depolamak Bu öğreticide. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
* **Azure SQL Veritabanı**. Bir Azure SQL veritabanı olarak kullandığınız bir **hedef** verileri depolamak Bu öğreticide. Öğretici, bkz: kullanabileceğiniz bir Azure SQL veritabanı yoksa [oluşturmak ve bir Azure SQL veritabanını yapılandırmak nasıl](../../sql-database/sql-database-get-started.md) oluşturmak için.
* **SQL Server 2012/2014 veya Visual Studio 2013**. SQL Server Management Studio veya Visual Studio bir örnek veritabanı oluşturma ve veritabanındaki sonuç verileri görüntülemek için kullanın.  

## <a name="collect-blob-storage-account-name-and-key"></a>BLOB Depolama hesabı adı ve anahtar Topla
Hesap adı ve bu öğreticinin yapmak için Azure depolama hesabınızın hesap anahtarı gerekir. Aşağı Not **hesap adı** ve **hesap anahtarı** Azure depolama hesabınız için.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Tıklatın **tüm hizmetleri** ve sol menüden üzerinde **depolama hesapları**.

    ![Gözat - depolama hesapları](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. İçinde **depolama hesapları** dikey penceresinde, select **Azure depolama hesabı** Bu öğreticide kullanmak istediğiniz.
4. Seçin **erişim anahtarları** altında bağlantı **ayarları**.
5. Tıklatın **kopyalama** (görüntü) düğmesini yanındaki **depolama hesabı adı** metin kutusuna ve kaydetme/bunu herhangi bir yerde Yapıştır (örneğin: bir metin dosyasındaki).
6. Kopyalama veya aşağı not için önceki adımı yineleyin **key1**.

    ![Depolama erişim anahtarı](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Tıklayarak tüm dikey pencereleri kapatmak **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, veritabanı, kullanıcı adları Topla
Azure SQL server, veritabanı ve bu öğreticinin yapmak için kullanıcı adları gerekir. Adlarını Not **server**, **veritabanı**, ve **kullanıcı** Azure SQL veritabanı.

1. İçinde **Azure portal**, tıklatın **tüm hizmetleri** seçin ve sol **SQL veritabanları**.
2. İçinde **SQL veritabanları dikey**seçin **veritabanı** Bu öğreticide kullanmak istediğiniz. Aşağı Not **veritabanı adı**.  
3. İçinde **SQL veritabanı** dikey penceresinde tıklatın **özellikleri** altında **ayarları**.
4. Değerlerini Not **sunucu adı** ve **Sunucu Yöneticisi oturum açma**.
5. Tıklayarak tüm dikey pencereleri kapatmak **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure hizmetlerinin SQL server erişmesine izin ver
Emin **Azure hizmetlerine erişime izin ver** açık ayarı **ON** Azure SQL sunucunuzun Data Factory hizmeti Azure SQL sunucunuza erişebilmesi için. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

1. Tıklatın **tüm hizmetleri** hub'ı tıklatın ve sol **SQL sunucuları**.
2. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
3. **Güvenlik Duvarı ayarları** dikey penceresinde, **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.
4. Tıklayarak tüm dikey pencereleri kapatmak **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>BLOB Storage ve SQL veritabanı hazırlayın
Şimdi, Azure blob depolama ve Azure SQL veritabanını öğretici için aşağıdaki adımları gerçekleştirerek hazırlayın:  

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayıp kaydedileceği **emp.txt** için **C:\ADFGetStarted** sabit diskinizde klasör.

    ```
    John, Doe
    Jane, Doe
    ```
2. [Azure Storage Gezgini](http://storageexplorer.com/) gibi araçları **adftutorial** kapsayıcısı oluşturmak ve **emp.txt** dosyasını kapsayıcıya yüklemek için kullanın.

    ![Azure Storage Gezgini. Verileri Blob depolama alanından SQL veritabanına kopyalamak.](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Azure SQL Database’inizde **emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın.  

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

    **SQL Server 2012/2014 bilgisayarınızda yüklü olup olmadığını:** makalesindeki yönergeleri izleyin [Azure SQL SQL Server Management Studio'yu kullanarak veritabanı yönetme](../../sql-database/sql-database-manage-azure-ssms.md) Azure SQL sunucunuza bağlanmak ve SQL betiğini çalıştırın. 

    İstemcinizin Azure SQL sunucusuna erişim izni yoksa, makinenizden (IP adresi) erişim izni vermek için Azure SQL sunucunuzun güvenlik duvarını yapılandırmanız gerekir. Azure SQL sunucunuzun güvenlik duvarını yapılandırmaya yönelik adımlar için [bu makaleye](../../sql-database/sql-database-configure-firewall-settings.md) bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Önkoşullar tamamladınız. Aşağıdaki yöntemlerden birini kullanarak bir veri fabrikası oluşturabilirsiniz. Öğreticiyi gerçekleştirme seçeneklerini aşağı açılan listesinde en üst veya aşağıdaki bağlantılardan birini tıklatın.     

* [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
* [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Bu öğreticideki veri işlem hattı, bir kaynak veri deposundaki verileri hedef veri deposuna kopyalar. Çıkış verileri üretmek için giriş verilerini dönüştürmez. Azure Data Factory kullanarak verileri dönüştürme hakkında bir öğretici için bkz. [Öğretici: Hadoop kümesi kullanarak verileri dönüştürmek için ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md).
> 
> Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Ayrıntılı bilgi için bkz. [Data Factory’de zamanlama ve yürütme](data-factory-scheduling-and-execution.md). 
