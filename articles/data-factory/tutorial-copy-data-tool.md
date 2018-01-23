---
title: "Azure Veri Kopyalama aracını kullanarak veri kopyalama | Microsoft Docs"
description: "Azure Blob Depolama’daki verileri Azure SQL Veritabanı’na kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: a8c7035ebf462bb168147e9d8eb60742333ce8b8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-copy-data-tool"></a>Veri Kopyalama aracını kullanarak Azure Blob’dan Azure SQL Veritabanına veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](tutorial-copy-data-tool.md)

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak bir Azure blob depolamadaki verileri Azure SQL veritabanına kopyalarsınız. 

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).
>
> - Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak işlem hattı oluşturma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* **Azure Depolama hesabı**. Blob depolama alanını **kaynak** veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
* **Azure SQL Veritabanı**. Veritabanını **havuz** veri deposu olarak kullanabilirsiniz. Azure SQL Veritabanınız yoksa, oluşturma adımları için [Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) makalesine bakın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Aşağıdaki adımları izleyerek Azure Blob ve Azure SQL Veritabanınızı öğreticiye hazırlayın:

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayıp diskinizde **inputEmp.txt** dosyası olarak kaydedin.

    ```
    John|Doe
    Jane|Doe
    ```

2. [Azure Depolama Gezgini](http://storageexplorer.com/) gibi araçları **adfv2tutorial** kapsayıcısı oluşturmak ve **inputEmp.txt** dosyasını kapsayıcıya yüklemek için kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. Azure SQL Veritabanınızda **dbo.emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın.

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

2. Azure hizmetlerinin SQL sunucusuna erişmesine izin ver. Data Factory hizmetinin Azure SQL sunucunuza veri yazabilmesi için Azure SWL sunucunuza ait **Azure hizmetlerine erişime izin ver** ayarının etkinleştirildiğinden emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    1. Soldaki **Diğer hizmetler** hub’ına ve sonra **SQL sunucuları**’na tıklayın.
    2. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
    3. **Güvenlik Duvarı ayarları** sayfasında **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Yeni veri fabrikası sayfası](./media/tutorial-copy-data-tool/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda/bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-tool/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-copy-data-tool/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğuna tıklayın. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. Veri Kopyalama aracını başlatmak için başlangıç sayfasında **Veri Kopyalama**’ya tıklayın. 

   ![Veri Kopyalama aracının kutucuğu](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Veri Kopyalama aracının **Özellikler** sayfasında, **Görev adı** olarak **CopyFromBlobToSqlPipeline** adını belirtin ve **İleri**’ye tıklayın. Data Factory kullanıcı arabirimi, görev adı için belirttiğiniz ada sahip bir işlem hattı oluşturur. 

    ![Veri Kopyalama aracı - Özellikler sayfası](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **Azure Blob Depolama**’yı seçip **İleri**’ye tıklayın. Kaynak veriler bir Azure blob depolama alanındadır. 

    ![Kaynak veri deposu sayfası](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. **Azure Blob depolama hesabı belirtin** sayfasında aşağıdaki adımları uygulayın: 
    1. **Bağlantı adı** için **AzureStorageLinkedService** adını girin.
    2. Listeden **depolama hesabınızın adını** seçin.
    3. **İleri**’ye tıklayın. 

        ![Blob depolama hesabını belirtin](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

        Bağlı hizmetler, bir veri deposunu veya işlemi veri fabrikasına bağlar. Bu durumda, Azure Depolama hesabınızı veri deposuna bağlamak için bir Azure Depolama bağlı hizmeti oluşturursunuz. Bağlı hizmet, Data Factory hizmetlerinin çalışma zamanında blob depolama alanına bağlanmak için kullandığı bağlantı bilgilerini içerir. Veri kümesi tarafından kaynak verileri içeren kapsayıcı, klasör ve dosya (isteğe bağlı) belirtilir. 
5. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:
    
    1. **adfv2tutorial/input** klasörüne gidin.
    2. **inputEmp.txt** dosyasını seçin
    3. **Seçin**’e tıklayın. Alternatif olarak, **inputEmp.txt** dosyasına çift tıklayabilirsiniz. 
    4. **İleri**’ye tıklayın. 

    ![Girdi dosyasını veya klasörünü seçin](./media/tutorial-copy-data-tool/choose-input-file-folder.png)
6. **Dosya biçimi ayarları** sayfasında aracın sütun ve satır sınırlayıcılarını otomatik olarak belirlediğine dikkat edin ve **İleri**’ye tıklayın. Ayrıca, bu sayfada verilerin önizlemesini yapabilir ve giriş verilerinin şemasını görüntüleyebilirsiniz. 

    ![Dosya biçimi ayarları sayfası](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. **Hedef veri deposu** sayfasında Azure **SQL Veritabanı**’nı ve **İleri**’yi seçin. 

    ![Hedef veri deposu sayfası](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. **Azure SQL veritabanını belirtin** sayfasında aşağıdaki adımları uygulayın: 

    1. **Bağlantı adı** için **AzureSqlDatabaseLinkedService** adını belirtin. 
    2. **Sunucu adı** için Azure SQL Server’ınızı seçin.
    3. **Veritabanı adı** için Azure SQL veritabanınızı seçin.
    4. **Kullanıcı adı** için kullanıcının adını seçin.
    5. **Parola** için kullanıcının parolasını belirtin.
    6. **İleri**’ye tıklayın. 

        ![Azure SQL veritabanı belirtme](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

        Bağlı hizmeti bir veri kümesi ile ilişkilendirilmelidir. Bağlı hizmet, Data Factory hizmetinin çalışma zamanında Azure SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtilir.
9.  **Tablo eşleme** sayfasında **[dbo].[emp]** seçeneğini belirleyip **İleri**’ye tıklayın. 

    ![Tablo eşleme sayfası](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. **Şema eşleme** sayfasında, girdi dosyasındaki birinci ve ikinci sütunun **emp** tablosundaki **FirstName** ve **LastName** sütunuyla eşlendiğine dikkat edin. 

    ![Şema eşleme sayfası](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. **Ayarlar** sayfasında **İleri**’ye tıklayın. 

    ![Ayarlar sayfası](./media/tutorial-copy-data-tool/settings-page.png)
12. **Özet** sayfasında ayarları gözden geçirin ve **İleri**’ye tıklayın.

    ![Özet sayfası](./media/tutorial-copy-data-tool/summary-page.png)
13. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’ye tıklayın.

    ![Dağıtım sayfası](./media/tutorial-copy-data-tool/deployment-page.png)
14. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütununda etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını görürsünüz. Listeyi yenilemek için **Yenile**’ye tıklayın. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Göster** bağlantısına tıklayın. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısına (gözlük simgesi) tıklayın. İşlem hattı çalıştırmaları görünümüne dönmek için üstteki **İşlem hatları** bağlantısına tıklayın. Görünümü yenilemek için **Yenile**’ye tıklayın. 

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Düzenleyici moduna geçmek için soldaki **Düzenle** sekmesine tıklayın. Düzenleyiciyi kullanarak araç tarafından oluşturulan bağlı hizmetleri, veri kümelerini ve işlem hatlarını güncelleştirebilirsiniz. Düzenleyicide açılan varlıkla ilişkili JSON kodunu görüntülemek için **Kod**’a tıklayın. Bu varlıkları Data Factory kullanıcı arabiriminde düzenlemeyle ilgili ayrıntılar için [bu öğreticinin Azure portalı sürümüne](tutorial-copy-data-portal.md) bakın.

    ![Düzenleyici sekmesi](./media/tutorial-copy-data-tool/edit-tab.png)
17. Verilerin Azure SQL veritabanınızdaki **emp** tablosuna eklendiğini doğrulayın. 

    ![SQL çıktısını doğrulama](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, bir Azure Blob depolama alanındaki verileri Azure SQL veritabanına kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak işlem hattı oluşturma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Buluttan şirket içine veri kopyalama](tutorial-hybrid-copy-data-tool.md)