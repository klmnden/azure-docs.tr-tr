---
title: Azure Veri Kopyalama aracını kullanarak veri kopyalama | Microsoft Docs
description: Azure Blob depolamadaki verileri bir SQL veritabanına kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 09/11/2018
ms.author: jingwang
ms.openlocfilehash: 553e87cacf407da2333da3105647719679feaabf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60624906"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Veri Kopyalama aracını kullanarak Azure Blob depolama alanında SQL veritabanına veri kopyalama
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](tutorial-copy-data-tool.md)

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak bir Azure Blob depolamadaki verileri bir SQL veritabanına kopyalarsınız.

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: BLOB Depolama alanı olarak kullanabilmeniz _kaynak_ veri deposu. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) bölümündeki yönergelere bakın.
* **Azure SQL veritabanı**: SQL veritabanı olarak kullanmanıza _havuz_ veri deposu. SQL veritabanınız yoksa [SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) konusundaki yönergelere bakın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Bu adımları uygulayarak Blob depolama alanınızı ve SQL veritabanınızı öğretici için hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. **Not Defteri**'ni başlatın. Aşağıdaki metni kopyalayıp diskinizde **inputEmp.txt** adlı bir dosyaya kaydedin:

    ```
    John|Doe
    Jane|Doe
    ```

1. **adfv2tutorial** adlı bir kapsayıcı oluşturun ve inputEmp.txt dosyasını kapsayıcıya yükleyin. Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. Aşağıdaki SQL betiğini kullanarak SQL veritabanınızda **dbo.emp** adlı bir tablo oluşturun:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Azure hizmetlerinin SQL Server’a erişmesine izin ver. SQL Database’i çalıştıran sunucunuz için **Azure hizmetlerine erişime izin ver** ayarının etkin olduğunu doğrulayın. Bu ayar, Data Factory’nin veritabanı örneğinize veri yazmasına imkan tanır. Bu ayarı doğrulamak ve etkinleştirmek için Azure SQL server’ınızda **Güvenlik** > **Güvenlik duvarları ve sanal ağlar** bölümüne gidin > **Azure hizmetlerine erişime izin ver** seçeneğini **AÇIK** olarak ayarlayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden **+ Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin:
    
    ![Yeni veri fabrikası oluşturma](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
1. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.
    
    ![Yeni veri fabrikası](./media/tutorial-copy-data-tool/new-azure-data-factory.png)

    Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:
    
    ![Yeni veri fabrikası hata iletisi](./media/tutorial-copy-data-tool/name-not-available-error.png)

    Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
1. Yeni veri fabrikasının oluşturulacağı Azure **aboneliğini** seçin.
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
    
    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.
    
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).

1. **Sürüm** bölümünde **V2**'yi seçin.
1. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikanız tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
1. **Panoya sabitle**’yi seçin.
1. **Oluştur**’u seçin.
1. Panodaki **Veri Fabrikası Dağıtılıyor** kutucuğu işlem durumunu gösterir.

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-tool/deploying-data-factory.png)
1. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.
    
    ![Data factory giriş sayfası](./media/tutorial-copy-data-tool/data-factory-home-page.png)
1. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğunu seçin.

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala** kutucuğunu seçin.

    ![Veri Kopyalama aracının kutucuğu](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
1. **Özellikler** sayfasındaki **Görev adı** bölümüne **CopyFromBlobToSqlPipeline** adını girin. Sonra **İleri**’yi seçin. Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur.

    ![Özellikler sayfası](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
1. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur**'a tıklayın

    ![Yeni kaynak olarak bağlantılı hizmet](./media/tutorial-copy-data-tool/new-source-linked-service.png)

    b. Galeriden **Azure Blob Depolama**'yı ve ardından **İleri**'yi seçin.

    ![Blob kaynağını seçin](./media/tutorial-copy-data-tool/select-blob-source.png)

    c. **Yeni Bağlı Hizmet** sayfasında **Depolama hesabı adı** listesinden depolama hesabınızı ve ardından **Son**'u seçin.

    ![Azure depolama hesabını yapılandırın](./media/tutorial-copy-data-tool/configure-azure-storage.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve **İleri**'ye tıklayın.

    ![Kaynak olarak bağlantılı hizmeti seçin](./media/tutorial-copy-data-tool/select-source-linked-service.png)

1. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:
    
    a. **Göz at**'a tıklayarak **adfv2tutorial/input** klasörüne gidin, **inputEmp.txt** dosyasını seçin ve **Seç**'e tıklayın.

    ![Girdi dosyası veya klasörü seçin](./media/tutorial-copy-data-tool/specify-source-path.png)

    b. Sonraki adıma geçmek için **İleri**'ye tıklayın.

1. **Dosya biçimi ayarları** sayfasında aracın sütun ve satır sınırlayıcılarını otomatik olarak belirlediğine dikkat edin. **İleri**’yi seçin. Ayrıca, bu sayfada verilerin önizlemesini yapabilir ve giriş verilerinin şemasını görüntüleyebilirsiniz.

    ![Dosya biçimi ayarları](./media/tutorial-copy-data-tool/file-format-settings-page.png)
1. **Hedef veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur**'a tıklayın

    ![Yeni havuza bağlı hizmet](./media/tutorial-copy-data-tool/new-sink-linked-service.png)

    b. Seçin **Azure SQL veritabanı** galeri ve ardından **sonraki**.

    ![Azure SQL veritabanını seçme](./media/tutorial-copy-data-tool/select-azure-sql-db.png)

    c. **Yeni Bağlı Hizmet** sayfasında açılır listeden sunucunuzun ve veritabanınızın adını seçip kullanıcı adı ve parolayı belirttikten sonra **Son**'u seçin.

    ![Azure SQL veritabanını yapılandırma](./media/tutorial-copy-data-tool/config-azure-sql-db.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı havuz hizmetini seçin ve **İleri**'ye tıklayın.

    ![Havuza bağlı hizmeti seçin](./media/tutorial-copy-data-tool/select-sink-linked-service.png)

1. **Tablo eşleme** sayfasında **[dbo].[emp]** tablosunu seçip **İleri**’yi seçin.

    ![Tablo eşleme](./media/tutorial-copy-data-tool/table-mapping.png)
1. **Şema eşleme** sayfasında, girdi dosyasındaki birinci ve ikinci sütunun **emp** tablosundaki **FirstName** ve **LastName** sütunuyla eşlendiğine dikkat edin. **İleri**’yi seçin.

    ![Şema eşleme sayfası](./media/tutorial-copy-data-tool/schema-mapping.png)
1. **Ayarlar** sayfasında **İleri**’yi seçin.
1. **Özet** sayfasında ayarları gözden geçirin ve **İleri**’yi seçin.

    ![Özet sayfası](./media/tutorial-copy-data-tool/summary-page.png)
1. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.

    ![Dağıtım sayfası](./media/tutorial-copy-data-tool/deployment-page.png)
1. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir. Listeyi yenilemek için **Yenile**’yi seçin.

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-copy-data-tool/pipeline-monitoring.png)
1. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstteki **İşlem hatları** bağlantısını seçin. Görünümü yenilemek için **Yenile**’yi seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-tool/activity-monitoring.png)

    ![Etkinlik ayrıntılarını kopyalama](./media/tutorial-copy-data-tool/copy-execution-details.png)

1. Verilerin SQL veritabanınızdaki **emp** tablosuna eklendiğini doğrulayın.

    ![SQL çıkışını doğrulama](./media/tutorial-copy-data-tool/verify-sql-output.png)

1. Düzenleyici moduna geçmek için soldaki **Yazar** sekmesini seçin. Düzenleyici kullanılarak araç üzerinden oluşturulan bağlı hizmetleri, veri kümelerini ve işlem hatlarını güncelleştirebilirsiniz. Bu varlıkları Data Factory kullanıcı arabiriminde düzenlemeyle ilgili ayrıntılar için [bu öğreticinin Azure portalı sürümüne](tutorial-copy-data-portal.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, bir Blob depolama alanındaki verileri bir SQL veritabanına kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Verileri şirket içinden buluta kopyalamayı öğrenmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Şirket içinden buluta veri kopyalama](tutorial-hybrid-copy-data-tool.md)
