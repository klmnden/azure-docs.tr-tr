---
title: "Veri Fabrikası işlem hattı oluşturmak için Azure Portal'ı kullanma | Microsoft Docs"
description: "Bu öğreticide işlem hattıyla veri fabrikası oluşturmak için Azure Portal'ı kullanmaya yönelik adım adım yönergeler sağlanır. İşlem hattı, verileri Azure blob depolama alanından Azure SQL veritabanına kopyalamak için kopyalama etkinliğini kullanır. "
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 7486e7c6816538fc120fd0b0a8bea0b006fb21f0
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob’dan Azure SQL Veritabanına veri kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak bir veri fabrikası oluşturursunuz. Bu veri fabrikasındaki işlem hattı, verileri Azure Blob Depolama’dan Azure SQL Veritabanı’na kopyalar. Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).
>
> - Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma
> * İşlem hattını el ile tetikleme
> * İşlem hattını bir zamanlamaya göre tetikleme
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar
* **Azure Aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* **Azure Depolama hesabı**. Blob depolama alanını **kaynak** veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
* **Azure SQL Veritabanı**. Veritabanını **havuz** veri deposu olarak kullanabilirsiniz. Azure SQL Veritabanınız yoksa, oluşturma adımları için [Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) makalesine bakın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Aşağıdaki adımları izleyerek Azure Blob ve Azure SQL Veritabanınızı öğreticiye hazırlayın:

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve diskinizde **emp.txt** dosyası olarak kaydedin.

    ```
    John,Doe
    Jane,Doe
    ```

2. Azure blob depolama alanınızda **adftutorial** adlı bir kapsayıcı oluşturun. Bu kapsayıcıda **input** adlı bir klasör oluşturun. Sonra, **emp.txt** dosyasını **input** klasörüne yükleyin. Bu görevleri yerine getirmek için Azure Portal'ı veya [Azure Depolama Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

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

2. Azure hizmetlerinin SQL sunucusuna erişmesine izin ver. Data Factory hizmetinin Azure SQL sunucunuza veri yazabilmesi için Azure SWL sunucunuza ait **Azure hizmetlerine erişime izin ver** ayarının **AÇIK** olduğundan emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    1. Soldaki **Diğer hizmetler** hub’ına ve sonra **SQL sunucuları**’na tıklayın.
    2. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
    3. **Güvenlik Duvarı ayarları** sayfasında **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturur ve Azure Data Factory kullanıcı arabirimini başlatarak veri fabrikasında bir işlem hattı oluşturursunuz. 

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Yeni veri fabrikası sayfası](./media/tutorial-copy-data-portal/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
        Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.      
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-copy-data-portal/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, veri fabrikasında Kopyalama etkinliği ile bir işlem hattı oluşturursunuz. Kopyalama etkinliği, verileri Azure Blob Depolama Alanı'ndan Azure SQL Veritabanı'na kopyalar. [Hızlı başlangıç eğitiminde](quickstart-create-data-factory-portal.md), şu adımları izleyerek bir işlem hattı oluşturdunuz:

1. Bağlı hizmeti oluşturma. 
2. Giriş ve çıkış veri kümelerini oluşturma.
3. Ardından, işlem hattını oluşturma.

Bu öğreticide, işlem hattı oluşturmakla başlar ve işlem hattını yapılandırmak için gerektiğinde bağlı hizmetleri ve veri kümelerini oluşturursunuz. 

1. Başlarken sayfasında **İşlem Hattı Oluştur** kutucuğuna tıklayın. 

   ![İşlem hattı oluştur kutucuğu](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
3. İşlem hattının **Özellikler** penceresinde, işlem hattının **adını** **CopyPipeline** olarak ayarlayın.

    ![İşlem hattı adı](./media/tutorial-copy-data-portal/pipeline-name.png)
4. **Etkinlikler** araç kutusunda **Veri Akışı** kategorisini genişletin ve **Kopyalama** etkinliğini araç kutusundan sürükleyip işlem hattı tasarımcı yüzeyine bırakın. 

    ![Kopyalama etkinliğini sürükleyip bırakma](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
5. **Özellikler** penceresinin **Genel** sekmesinde, etkinliğin adı olarak **CopyFromBlobToSql** değerini belirtin.

    ![Etkinlik adı](./media/tutorial-copy-data-portal/activity-name.png)
6. **Kaynak** sekmesine geçin. Kaynak veri kümesi oluşturmak için **+ Yeni** öğesine tıklayın. 

    ![Yeni kaynak veri kümesi menüsü](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
7. **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama Alanı**’nı seçip **Son**’a tıklayın. Kaynak veriler bir Azure blob depolama alanında olduğu için, kaynak veri kümesi olarak Azure Blob Depolama Alanı'nı seçersiniz. 

    ![Azure Blob Depolama Alanı’nı seçin](./media/tutorial-copy-data-portal/select-azure-storage.png)
8. Uygulamada **AzureBlob1** başlıklı yeni bir **sekme** açıldığını görürsünüz.

    ![Azure Blob1 sekmesi ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
9. En alttaki **Özellikler** penceresinin **Genel** sekmesinde, **ad** olarak **SourceBlobDataset** değerini belirtin.

    ![Veri kümesi adı](./media/tutorial-copy-data-portal/dataset-name.png)
10. Özellikler penceresinde **Bağlantı** sekmesine geçin.   

    ![Bağlantı sekmesi](./media/tutorial-copy-data-portal/source-dataset-connection-tab.png)
11. **Bağlı hizmet** metin kutusunun yanındaki **+ Yeni** öğesine tıklayın. Bağlı hizmetler, bir veri deposunu veya işlemi veri fabrikasına bağlar. Bu durumda, Azure Depolama hesabınızı veri deposuna bağlamak için bir Azure Depolama bağlı hizmeti oluşturursunuz. Bağlı hizmet, Data Factory hizmetlerinin çalışma zamanında blob depolama alanına bağlanmak için kullandığı bağlantı bilgilerini içerir. Veri kümesi tarafından kaynak verileri içeren kapsayıcı, klasör ve dosya (isteğe bağlı) belirtilir. 

    ![Yeni bağlı hizmet düğmesi](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
12. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** alanında **AzureStorageLinkedService** değerini belirtin. 
    2. **Depolama hesabı adı** alanı için Azure depolama hesabınızı seçin.
    3. Azure Depolama hesabıyla bağlantıyı test etmek için **Bağlantıyı sına**'ya tıklayın.  
    4. Bağlı hizmeti kaydetmek için **Kaydet**’e tıklayın.

        ![Yeni Azure Depolama Bağlı Hizmeti](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
13. **Dosya yolu** alanı için **Gözat**'a tıklayın.  

    ![Dosya için Gözat düğmesi](./media/tutorial-copy-data-portal/file-browse-button.png)
14. **adftutorial/input** klasörüne gidin, **emp.txt** dosyasını seçin ve **Son**'a tıklayın. Alternatif olarak, emp.txt dosyasına çift tıklayabilirsiniz. 

    ![Giriş dosyasını seçme](./media/tutorial-copy-data-portal/select-input-file.png)
15. **Dosya biçimi** olarak **Metin biçimi**'nin ve **sütun sınırlayıcısı** olarak da **Virgül(`,`)** değerinin ayarlandığını onaylayın. Kaynak dosyada farklı satır ve sütun sınırlayıcıları kullanılıyorsa, **Dosya biçimi** alanı için **Metin Biçimini Algıla**'ya tıklayabilirsiniz. Verileri Kopyala aracı sizin için dosya biçimini ve sınırlayıcıları otomatik olarak algılar. Yine de bu değerleri geçersiz kılabilirsiniz. Bu sayfada **Verileri önizle**'ye tıklayarak verilerin önizlemesini görebilirsiniz.

    ![Metin biçimini algılama](./media/tutorial-copy-data-portal/detect-text-format.png)
17. Özellikler penceresinde **Şema** sekmesine geçin ve **Şemayı İçeri Aktar**'a tıklayın. Uygulamanın kaynak dosyada iki sütun algıladığına dikkat edin. Şemayı buraya aktarmanızın nedeni, kaynak veri deposundaki sütunları havuz veri deposundaki sütunlarla eşleyebilmektir. Sütunları eşlemeniz gerekmiyorsa, bu adımı atlayabilirsiniz. Bu öğreticide, şemayı içeri aktarın.

    ![Kaynak şemayı algılama](./media/tutorial-copy-data-portal/detect-source-schema.png)  
19. Şimdi, **işlem hattının bulunduğu** sekmeye geçin veya sol taraftaki **ağaç görünümünde** işlem hattına tıklayın.  

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal/pipeline-tab.png)
20. Özellikler penceresindeki Kaynak Veri Kümesi alanında **SourceBlobDataset** öğesinin seçildiğini onaylayın. Bu sayfada **Verileri önizle**'ye tıklayarak verilerin önizlemesini görebilirsiniz. 
    
    ![Kaynak veri kümesi](./media/tutorial-copy-data-portal/source-dataset-selected.png)
21. **Havuz** sekmesine geçin ve havuz veri kümesini oluşturmak için **Yeni**’ye tıklayın. 

    ![Yeni havuz veri kümesi menüsü](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
22. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçin ve **Son**’a tıklayın. Bu öğreticide verileri Azure SQL veritabanına kopyalıyorsunuz. 

    ![Azure SQL Veritabanı’nı seçin](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
23. Özellikler penceresinin **Genel** sekmesinde, adı **OutputSqlDataset** olarak ayarlayın. 
    
    ![Çıkış veri kümesi adı](./media/tutorial-copy-data-portal/output-dataset-name.png)
24. **Bağlantı** sekmesine geçin ve **Bağlı hizmet** için **Yeni**’ye tıklayın. Bağlı hizmetle bir veri kümesi ilişkilendirilmelidir. Bağlı hizmet, Data Factory hizmetinin çalışma zamanında Azure SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtir. 
    
    ![Yeni bağlı hizmet düğmesi](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
25. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** alanına **AzureSqlDatabaseLinkedService** adını girin. 
    2. **Sunucu adı** alanı için Azure SQL sunucunuzu seçin.
    4. **Veritabanı adı** alanı için Azure SQL veritabanınızı seçin. 
    5. **Kullanıcı adı** alanına kullanıcının adını girin. 
    6. **Parola** alanına kullanıcının parolasını girin. 
    7. Bağlantıyı test etmek için **Bağlantıyı sına**’ya tıklayın.
    8. Bağlı hizmeti kaydetmek için **Kaydet**’e tıklayın. 
    
        ![Yeni Azure SQL Veritabanı bağlı hizmeti](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

26. **Tablo** için **[dbo].[emp]** seçeneğini belirtin. 

    ![emp tablosunu seçme](./media/tutorial-copy-data-portal/select-emp-table.png)
27. **Şema** sekmesine geçin ve Şemayı İçeri Aktar'a tıklayın. 

    ![Hedef şemayı içeri aktarma](./media/tutorial-copy-data-portal/import-destination-schema.png)
28. **ID** sütununu seçin ve **Sil**'e tıklayın. ID sütunu SQL veritabanındaki bir kimlik sütunudur, dolayısıyla kopyalama etkinliğinin bu sütuna veri eklemesi gerekmez.

    ![ID sütununu silme](./media/tutorial-copy-data-portal/delete-id-column.png)
30. **İşlem hattının** bulunduğu sekmeye geçin ve **Havuz Veri Kümesi** olarak **OutputSqlDataset** seçildiğini onaylayın.

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
31. En alttaki özellikler penceresinde **Eşleme** sekmesine geçin ve **Şemaları İçeri Aktar**'a tıklayın. Kaynak dosyadaki birinci ve ikinci sütunların SQL veritabanındaki **FirstName** ve **LastName** alanlarına eşlendiğine dikkat edin.

    ![Şemaları eşleme](./media/tutorial-copy-data-portal/map-schemas.png)
33. **Doğrula** düğmesine tıklayarak işlem hattını doğrulayın. Doğrulama penceresini kapatmak için **sağ ok** düğmesine tıklayın.

    ![İşlem hattı doğrulama çıkışı](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
34. Sağ köşedeki **Kod** düğmesine tıklayın. İşlem hattıyla ilişkilendirilmiş JSON kodunu görürsünüz. 

    ![Kod düğmesi](./media/tutorial-copy-data-portal/code-button.png)
35. Şu kod parçacığına benzer JSON kodunu görürsünüz:  

    ```json
    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToSql",
                    "type": "Copy",
                    "dependsOn": [],
                    "policy": {
                        "timeout": "7.00:00:00",
                        "retry": 0,
                        "retryIntervalInSeconds": 20
                    },
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "SqlSink",
                            "writeBatchSize": 10000
                        },
                        "enableStaging": false,
                        "parallelCopies": 0,
                        "cloudDataMovementUnits": 0,
                        "translator": {
                            "type": "TabularTranslator",
                            "columnMappings": "Prop_0: FirstName, Prop_1: LastName"
                        }
                    },
                    "inputs": [
                        {
                            "referenceName": "SourceBlobDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "OutputSqlDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ]
                }
            ]
        }
    }
    ```

## <a name="test-run-the-pipeline"></a>İşlem hattında test çalıştırması yapma
Yapıları (bağlı hizmetler, veri kümeleri ve işlem hattı) Data Factory'de veya kendi VSTS GIT deponuzda yayımlamadan önce işlem hattında test çalıştırması yapabilirsiniz. 

1. İşlem hattında test çalıştırması yapmak için araç çubuğunda **Test Çalıştırması**'na tıklayın. En alttaki pencerenin **Çıkış** sekmesinde işlem hattı çalıştırmasının durumunu görürsünüz. 

    ![Test Çalıştırması düğmesi](./media/tutorial-copy-data-portal/test-run-output.png)
2. Kaynak dosyadan verilerin hedef SQL veritabanına eklendiğini doğrulayın. 

    ![SQL çıkışını doğrulama](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. Sol bölmede **Yayımla**'ya tıklayın. Bu eylem, oluşturduğunuz varlıkları (bağlı hizmetler, veri kümeleri ve işlem hatları) Azure Data Factory'de yayımlar.

    ![Yayımla düğmesi](./media/tutorial-copy-data-portal/publish-button.png)
4. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sol kenar çubuğunda **Bildirimleri Göster** sekmesine tıklayın. **X** simgesine tıklayarak bildirim penceresini kapatın.

    ![Bildirimleri göster](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Kod deposunu yapılandırma
Veri fabrikası yapılarınızla ilişkilendirilmiş kodu Visual Studio Team System (VSTS) kod deposuna yayımlayabilirsiniz. Bu adımda kod deposunu oluşturursunuz. 

VSTS kod deposuyla çalışmak istemiyorsanız, bu adımı atlayabilir ve önceki adımda yaptığınız gibi Data Factory'ye yayımlamaya devam edebilirsiniz. 

1. Sol köşedeki **Data Factory**'ye veya yanındaki aşağı oka tıklayın ve sonra da **Kod Deposunu Yapılandır**'a tıklayın. 

    ![Kod düğmesi](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. **Depo Ayarları** sayfasında aşağıdaki adımları uygulayın: 
    1. **Depo Türü** alanında **Visual Studio Team Services Git** değerini seçin.
    2. **Visual Studio Team Services Hesabı** alanı için VSTS hesabınızı seçin.
    3. **Proje Adı** alanı için VSTS hesabınızdaki bir projeyi seçin.
    4. Veri fabrikanızla ilişkilendirilecek **Git deposunun adı** olarak **Tutorial2** girin. 
    5. **Mevcut Data Factory kaynaklarını depoya içeri aktar** seçeneğinin seçili olduğunu onaylayın. 
    6. Ayarları kaydetmek için **Kaydet**’e tıklayın. 

        ![Depo ayarları](./media/tutorial-copy-data-portal/repository-settings.png)
3. Depo olarak **VSTS GIT**'in seçildiğini onaylayın.

    ![VSTS GIT seçildi](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Web tarayıcısındaki ayrı bir sekmede **Tutorial2** deposuna gidin; iki dal görürsünüz: **master** ve **adf_publish**.

    ![Master ve adf_publish dalları](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Data Factory varlıkları için **JSON dosyalarının** **master** dalında yer aldığını doğrulayın.

    ![Master dalındaki dosyalar](./media/tutorial-copy-data-portal/master-branch-files.png)
6. **JSON dosyalarının** henüz **adf_publish** dalında yer almadığını doğrulayın. 

    ![Adf_publish dalındaki dosyalar](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. **İşlem hattı** için **açıklama** ekleyin ve araç çubuğunda **Kaydet** düğmesine tıklayın. 

    ![İşlem hattı için açıklama ekleme](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Şimdi, **Tutorial2** deposunda sizin kullanıcı adınızla bir **dal** görüyor olmalısınız. Yaptığınız değişiklik master dalında değil sizin kendi dalınızda yer alır. Yalnızca master dalından varlıkları yayımlayabilirsiniz.

    ![Dalınız](./media/tutorial-copy-data-portal/your-branch.png)
9. **Eşitle** düğmesinin üzerine gelin (henüz tıklamayın), **Değişiklikleri Gönder** seçeneğini belirtin ve **Eşitle** düğmesine tıklayarak değişikliklerinizi **master** dalıyla eşitleyin. 

    ![Değişikliklerinizi gönderme ve eşitleme](./media/tutorial-copy-data-portal/commit-and-sync.png)
9. Değişiklikleri eşitle penceresinde aşağıdaki eylemleri gerçekleştirin: 

    1. Güncelleştirilmiş işlem hatları listesinde **CopyPipeline** öğesinin gösterildiğini onaylayın.
    2. **Eşitleme sonrasında değişiklikleri yayımla**'nın seçili olduğunu onaylayın. Bu seçeneğin işaretini kaldırırsanız, yalnızca dalınızdaki değişikliklerinizi master dalıyla eşitlersiniz ama bunları Data Factory hizmetinde yayımlamazsınız. Bunları daha sonra, **Yayımla** düğmesini kullanarak yayımlayabilirsiniz. Bu seçeneği işaretlerseniz, değişiklikler önce master dalıyla eşitlenir ve ardından Data Factory hizmetinde yayımlanır.
    3. **Eşitle**'ye tıklayın. 

    ![Değişiklikleri eşitle penceresi](./media/tutorial-copy-data-portal/sync-your-changes.png)
10. Artık, **Tutorial2** deposunun **adf_publish** dalında dosyaları görebilirsiniz. Ayrıca bu dalda Data Factory çözümünüze yönelik Azure Resource Manager şablonunu da bulabilirsiniz.  

    ![Adf_publish dalındaki dosyalar](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme
Bu adımda, önceki adımda yayımladığınız işlem hattını el ile tetiklersiniz. 

1. Araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın. 

    ![Şimdi tetikle menüsü](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Soldaki **İzleyici** sekmesine geçin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve işlem hattını yeniden çalıştırmak için Eylemler sütunundaki bağlantıları kullanabilirsiniz.

    ![İşlem hattı çalıştırmasını izleme](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Göster** bağlantısına tıklayın. Bu örnekte, tek bir etkinlik olduğundan listede tek giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısına (gözlük simgesi) tıklayın. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için en üstteki **İşlem Hatları**'na tıklayabilirsiniz. Görünümü yenilemek için **Yenile**’ye tıklayın.

    ![Etkinlik çalıştırmalarını görüntüleme](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Azure SQL veritabanında **emp** tablosuna iki satır daha eklendiğini doğrulayın. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>İşlem hattını bir zamanlamaya göre tetikleme
Bu zamanlamada, işlem hattı için bir zamanlayıcı tetikleyicisi oluşturursunuz. Tetikleyici, işlem hattını belirli bir zamanlamaya göre (saatlik, günlük, vb.) çalıştırır. Bu örnekte, tetikleyiciyi belirtilen bitiş tarihi saatine kadar dakikada bir çalıştırılacak şekilde ayarlarsınız. 

1. Soldaki **Düzenle** sekmesine geçin. 

    ![Düzenle sekmesi](./media/tutorial-copy-data-portal/edit-tab.png)
2. **Tetikleyici**'ye tıklayın ve **Yeni/Düzenle**'yi seçin. İşlem hattı etkin durumda değilse, bu duruma geçirin. 

    ![Tetikleyici yeni/düzenle menüsü](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. **Tetikleyici Ekle** penceresinde **Tetikleyici seç...** öğesine ve sonra da **+ Yeni**'ye tıklayın. 

    ![Tetikleyici Ekle - yeni tetikleyici](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. **Yeni Tetikleyici** penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** olarak **RunEveryMinute** değerini ayarlayın.
    2. **Bitiş** için **Tarihinde** öğesini seçin. 
    3. **Bitiş** için açılan listeye tıklayın.
    4. **Geçerli gün**'ü seçin. Varsayılan olarak, bitiş günü olarak bir sonraki gün ayarlanır. 
    5. **dakika** bölümünü, geçerli tarih saati birkaç dakika geçecek şekilde güncelleştirin. Tetikleyicinin etkinleştirilmesi için, önce sizin değişiklikleri yayımlamanız gerekir. Bu nedenle, bunu birkaç dakika sonraya ayarlarsanız ve bu arada değişiklikleri yayımlamazsanız, tetikleyici çalıştırması görmezsiniz.  
    6. **Uygula**'ya tıklayın. 

        ![Tetikleyici özelliklerini ayarlama](./media/tutorial-copy-data-portal/set-trigger-properties.png)
    7. **Etkinleştirildi** seçeneğini işaretleyin. Bunu devre dışı bırakabilir ve daha sonra bu onay kutusunu kullanarak etkinleştirebilirsiniz.
    8. **İleri**’ye tıklayın.

        ![Tetikleyici etkinleştirildi - ileri](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Her işlem hattı çalıştırmasının bir maliyeti vardır. Bu nedenle, bitiş tarihini düzgün bir şekilde ayarlayın. 
6. **Tetikleyici Çalıştırma Parametreleri** sayfasındaki uyarıyı gözden geçirin ve **Son**'a tıklayın. Bu örnekteki işlem hattı hiçbir parametre almaz. 

    ![İşlem hattı parametreleri](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
7. Değişiklikleri depoya yayımlamak için **Yayımla**'ya tıklayın. Yayımlama başarılı olana kadar tetikleyici gerçekten etkinleştirilmez. 

    ![Tetikleyiciyi yayımlama](./media/tutorial-copy-data-portal/publish-trigger.png) 
8. Tetiklenen işlem hattı çalıştırmalarını görmek için sol taraftaki **İzleyici** sekmesine geçin. 

    ![Tetiklenen işlem hattı çalıştırmaları](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
9. İşlem hattı çalıştırmaları görünümünden tetikleyici çalıştırmaları görünümüne geçmek için, İşlem Hattı Çalıştırmaları'na tıklayın ve Tetikleyici Çalıştırmaları'nı seçin.
    
    ![Tetikleyici çalıştırmaları menüsü](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
10. Listede tetikleyici çalıştırmalarını görürsünüz. 

    ![Tetikleyici çalıştırmaları listesi](./media/tutorial-copy-data-portal/trigger-runs-list.png)
11. Belirtilen bitiş saatine kadar **emp** tablosuna dakikada iki satır (her işlem hattı çalıştırması için) eklendiğini doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma
> * İşlem hattını el ile tetikleme
> * İşlem hattını bir zamanlamaya göre tetikleme
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.


Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Buluttan şirket içine veri kopyalama](tutorial-hybrid-copy-data-tool.md)
