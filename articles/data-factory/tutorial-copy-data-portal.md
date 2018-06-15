---
title: Veri Fabrikası işlem hattı oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Bu öğreticide işlem hattıyla veri fabrikası oluşturmak için Azure portalını kullanmaya yönelik adım adım yönergeler sağlanır. İşlem hattı, verileri Azure Blob depolama alanından SQL veritabanına kopyalamak için kopyalama etkinliğini kullanır.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 34c78a114c1d106c400a94941aa113153383e206
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30173347"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob depolama alanında SQL veritabanına veri kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak bir veri fabrikası oluşturursunuz. Bu veri fabrikasındaki işlem hattı, verileri Azure Blob Depolama alanından SQL veritabanına kopyalar. Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

> [!NOTE]
> - İlk kez Data Factory kullanıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).
>
> - Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory’nin genel kullanıma açık 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma.
> * İşlem hattını el ile tetikleme.
> * İşlem hattını bir zamanlamaya göre tetikleme.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* **Azure depolama hesabı**. Blob depolama alanını *kaynak* veri deposu olarak kullanabilirsiniz. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* **Azure SQL Veritabanı**. Veritabanını *havuz* veri deposu olarak kullanabilirsiniz. SQL veritabanınız yoksa, oluşturma adımları için bkz. [SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Aşağıdaki adımları uygulayarak öğretici için Blob depolama alanınızı ve SQL veritabanınızı hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve diskinizde **emp.txt** dosyası olarak kaydedin:

    ```
    John,Doe
    Jane,Doe
    ```

2. Blob depolama alanınızda **adftutorial** adlı bir kapsayıcı oluşturun. Bu kapsayıcıda **input** adlı bir klasör oluşturun. Sonra, **emp.txt** dosyasını **input** klasörüne yükleyin. Bu görevleri yerine getirmek için Azure Portal'ı veya [Azure Depolama Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. SQL veritabanınızda **dbo.emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın:

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

2. Azure hizmetlerinin SQL Server’a erişmesine izin verin. Data Factory’nin SQL Server’ınıza veri yazabilmesi için SQL Server’ınız için **Azure hizmetlerine erişime izin ver** ayarının **AÇIK** olduğundan emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    a. Solda **Diğer hizmetler** > **SQL sunucuları** seçeneğini belirleyin.

    b. Sunucunuzu seçin ve **AYARLAR** bölümünden **Güvenlik Duvarı**’nı seçin.

    c. **Güvenlik Duvarı ayarları** sayfasında **Azure hizmetlerine erişime izin ver** için **AÇIK** seçeneğini belirleyin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. Soldaki menüden **Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin. 
  
   ![Yeni veri fabrikası oluşturma](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Ad alanı için aşağıdaki hata iletisini görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).
  
   ![Hata iletisi](./media/tutorial-copy-data-portal/name-not-available-error.png)
4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin. 
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md). 
6. **Sürüm** bölümünde **V2 (Önizleme)** seçeneğini belirleyin.
7. **Konum** bölümünden veri fabrikası için bir konum seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.
8. **Panoya sabitle**’yi seçin. 
9. **Oluştur**’u seçin. 
10. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz: 

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-portal/deploying-data-factory.png)
11. Oluşturma işlemi bittikten sonra, resimde gösterildiği gibi **Veri Fabrikası** sayfası görüntülenir.
   
    ![Data factory giriş sayfası](./media/tutorial-copy-data-portal/data-factory-home-page.png)
12. Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede başlatmak için **Geliştir ve İzle**’yi seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, veri fabrikasında kopyalama etkinliği ile bir işlem hattı oluşturacaksınız. Kopyalama etkinliği, verileri Blob depolama alanından SQL Veritabanı'na kopyalar. [Hızlı başlangıç eğitiminde](quickstart-create-data-factory-portal.md), şu adımları izleyerek bir işlem hattı oluşturdunuz:

1. Bağlı hizmeti oluşturma. 
2. Giriş ve çıkış veri kümelerini oluşturma.
3. İşlem hattı oluşturma.

Bu öğreticide işlem hattını oluşturmaya başlayacaksınız. Daha sonra işlem hattını yapılandırmanız gerektiğinde bağlı hizmetleri ve veri kümelerini oluşturacaksınız. 

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin. 

   ![İşlem hattı oluşturma](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
2. İşlem hattının **Özellikler** penceresinde **Ad** bölümüne işlem hattının adı için **CopyPipeline** girin.

    ![İşlem hattı adı](./media/tutorial-copy-data-portal/pipeline-name.png)
3. **Etkinlikler** araç kutusunda **Veri Akışı** kategorisini genişletin ve **Kopyalama** etkinliğini araç kutusundan sürükleyip işlem hattı tasarımcı yüzeyine bırakın. 

    ![Kopyalama etkinliği](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
4. **Özellikler** penceresinin **Genel** sekmesinde, etkinliğin adı olarak **CopyFromBlobToSql** değerini girin.

    ![Etkinlik adı](./media/tutorial-copy-data-portal/activity-name.png)
5. **Kaynak** sekmesine gidin. Kaynak veri kümesi oluşturmak için **+ Yeni** seçeneğini belirleyin. 

    ![Kaynak sekmesi](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
6. **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama Alanı**’nı ve ardından **Son**’u seçin. Kaynak veriler bir Blob depolama alanında olduğundan kaynak veri kümesi olarak **Azure Blob Depolama Alanı**'nı seçmeniz gerekir. 

    ![Storage seçimi](./media/tutorial-copy-data-portal/select-azure-storage.png)
7. Uygulamada **AzureBlob1** başlıklı yeni bir sekme açıldığını görebilirsiniz.

    ![AzureBlob1 sekmesi ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
8. **Özellikler** penceresinin altındaki **Genel** sekmesinde **Ad** bölümüne **SourceBlobDataset** girin.

    ![Veri kümesi adı](./media/tutorial-copy-data-portal/dataset-name.png)
9. **Özellikler** penceresinin **Bağlantı** sekmesine gidin. **Bağlı hizmet** metin kutusunun yanındaki **+ Yeni** seçeneğini belirleyin. 

    Bağlı hizmetler, bir veri deposunu veya işlemi veri fabrikasına bağlar. Bu durumda, depolama hesabınızı veri deposuna bağlamak için bir Depolama bağlı hizmeti oluşturmanız gerekir. Bağlı hizmet, Data Factory’nin çalışma zamanında Blob depolama alanına bağlanmak için kullandığı bağlantı bilgilerini içerir. Veri kümesi tarafından kaynak verileri içeren kapsayıcı, klasör ve dosya (isteğe bağlı) belirtilir. 

    ![Yeni bağlı hizmet düğmesi](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
10. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** bölümüne **AzureStorageLinkedService** adını girin. 

    b. **Depolama hesabı adı** bölümünde depolama hesabınızı seçin.

    c. Depolama hesabı bağlantısını test etmek için**Bağlantıyı sına** seçeneğini belirleyin.

    d. Bağlı hizmeti kaydetmek için **Kaydet**’i seçin.

    ![Yeni bağlı hizmet](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
11. **Dosya yolu**’nun yanındaki **Gözat** seçeneğini belirleyin.

    ![Dosya yolu için gözat düğmesi](./media/tutorial-copy-data-portal/file-browse-button.png)
12. **adftutorial/input** klasörüne gidin, **emp.txt** dosyasını ve ardından **Son**'u seçin. Bunun yerine **emp.txt** dosyasına çift tıklayabilirsiniz. 

    ![Giriş dosyasını seçme](./media/tutorial-copy-data-portal/select-input-file.png)
13. **Dosya biçimi** olarak **Metin biçimi**'nin ve **Sütun sınırlayıcısı** olarak da **Virgül(`,`)** değerinin ayarlandığından emin olun. Kaynak dosyada farklı satır ve sütun sınırlayıcıları kullanılıyorsa, **Dosya biçimi** için **Metin Biçimini Algıla**'yı seçebilirsiniz. Verileri Kopyala aracı sizin için dosya biçimini ve sınırlayıcıları otomatik olarak algılar. Yine de bu değerleri geçersiz kılabilirsiniz. Bu sayfadaki verilerin önizlemesini görüntülemek için **Veri önizleme** ‘yi seçin.

    ![Metin biçimini algılama](./media/tutorial-copy-data-portal/detect-text-format.png)
14. **Özellikler** penceresinin **Şema** sekmesine gidin ve **Şemayı İçeri Aktar**’ı seçin. Uygulamanın kaynak dosyada iki sütun algıladığına dikkat edin. Kaynak veri deposundaki sütunları havuz veri deposu ile eşlemek için şemayı buraya aktarmanız gerekir. Sütunları eşlemeniz gerekmiyorsa, bu adımı atlayabilirsiniz. Bu öğreticide, şemayı içeri aktarın.

    ![Kaynak şemayı algılama](./media/tutorial-copy-data-portal/detect-source-schema.png)  
15. İşlem hattını içeren sekmeye gidin veya soldaki işlem hattını seçin.

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal/pipeline-tab.png)
16. **Özellikler** penceresindeki **Kaynak Veri Kümesi**’nde **SourceBlobDataset**’in seçili olduğundan emin olun. Bu sayfadaki verilerin önizlemesini görüntülemek için **Veri önizleme** ‘yi seçin. 
    
    ![Kaynak veri kümesi](./media/tutorial-copy-data-portal/source-dataset-selected.png)
17. **Havuz** sekmesine gidin ve havuz veri kümesi oluşturmak için **+Yeni** seçeneğini belirleyin. 

    ![Havuz veri kümesi](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
18. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçin ve ardından **Son**’u seçin. Bu öğreticide verileri bir SQL veritabanına kopyalayacaksınız. 

    ![SQL veritabanı seçimi](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
19. **Özellikler** penceresinin **Genel** sekmesinde **Ad** bölümüne **OutputSqlDataset** girin. 
    
    ![Çıkış veri kümesi adı](./media/tutorial-copy-data-portal/output-dataset-name.png)
20. **Bağlantı**sekmesine gidin ve **Bağlı hizmet**’in yanındaki **+ Yeni** seçeneğini belirleyin. Bağlı hizmetle bir veri kümesi ilişkilendirilmelidir. Bağlı hizmet, Data Factory’nin çalışma zamanında SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtir. 
    
    ![Bağlı hizmet](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
21. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** bölümüne **AzureSqlDatabaseLinkedService** girin.

    b. **Sunucu adı** bölümünde SQL Server örneğinizi seçin.

    c. **Veritabanı adı** bölümünde SQL veritabanınızı seçin.

    d. **Kullanıcı adı** bölümüne kullanıcının adını girin.

    e. **Parola** bölümüne kullanıcının parolasını girin.

    f. Bağlantıyı test etmek için **Bağlantıyı sına**’yı seçin.

    g. Bağlı hizmeti kaydetmek için **Kaydet**’i seçin. 
    
    ![Yeni bağlı hizmeti kaydedin](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

22. **Tablo**’da **[dbo].[emp]** seçeneğini belirleyin. 

    ![Tablo](./media/tutorial-copy-data-portal/select-emp-table.png)
23. **Şema** sekmesine gidin ve **Şemayı İçeri Aktar**’ı seçin. 

    ![Şemayı içeri aktar’ı seçin](./media/tutorial-copy-data-portal/import-destination-schema.png)
24. **ID** sütununu ve ardından **Sil**’i seçin. **ID** sütunu, SQL veritabanındaki bir kimlik sütunu olduğundan kopyalama etkinliğinin bu sütuna veri eklemesi gerekmez.

    ![ID sütununu silme](./media/tutorial-copy-data-portal/delete-id-column.png)
25. İşlem hattının bulunduğu sekmeye gidin ve **Havuz Veri Kümesi**’nde **OutputSqlDataset** seçeneğinin belirlendiğinden emin olun.

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
26. **Özellikler** penceresinin altındaki **Eşleme** sekmesine gidin ve **Şemaları İçeri Aktar**’ı seçin. Kaynak dosyadaki birinci ve ikinci sütunların SQL veritabanındaki **FirstName** ve **LastName** alanlarına eşlendiğini görebilirsiniz.

    ![Şemaları eşleme](./media/tutorial-copy-data-portal/map-schemas.png)
27. İşlem hattını doğrulamak için **Doğrula**'yı seçin. Doğrulama penceresini kapatmak için sağ üst köşedeki sağ oku seçin.

    ![İşlem hattı doğrulama çıkışı](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
28. Sağ üst köşede **Kod**’u seçin. İşlem hattıyla ilişkilendirilmiş JSON kodunu görürsünüz. 

    ![Kod düğmesi](./media/tutorial-copy-data-portal/code-button.png)
29. Aşağıdaki kod parçacığına benzer JSON kodunu görürsünüz: 

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
Yapıtları (bağlı hizmetler, veri kümeleri ve işlem hattı) Data Factory'de veya kendi Visual Studio Team Services Git deponuzda yayımlamadan önce işlem hattında test çalıştırması yapabilirsiniz. 

1. İşlem hattında test çalıştırması yapmak için araç çubuğunda **Test Çalıştırması**'nı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir. 

    ![İşlem hattını test etme](./media/tutorial-copy-data-portal/test-run-output.png)
2. Kaynak dosyadan verilerin hedef SQL veritabanına eklendiğini doğrulayın. 

    ![SQL çıkışını doğrulama](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. Sol bölmede **Tümünü Yayımla**’yı seçin. Bu eylem, oluşturduğunuz varlıkları (bağlı hizmetler, veri kümeleri ve işlem hatları) Data Factory'de yayımlar.

    ![Yayımlama](./media/tutorial-copy-data-portal/publish-button.png)
4. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sol kenar çubuğunda **Bildirimleri Göster** sekmesini seçin. Bildirimler penceresini kapatmak için **Kapat**’ı seçin.

    ![Bildirimleri göster](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Kod deposunu yapılandırma
Veri fabrikası yapıtlarınızla ilişkilendirilmiş kodu Visual Studio Team Services kod deposuna yayımlayabilirsiniz. Bu adımda kod deposunu oluşturursunuz.  VSTS tümleştirmesiyle görsel yazma hakkında daha fazla bilgi edinmek için bkz. [VSTS Git tümleştirmesiyle yazma](author-visually.md#author-with-vsts-git-integration).

Visual Studio Team Services kod deposuyla çalışmak istemiyorsanız bu adımı atlayabilirsiniz. Önceki adımda yaptığınız gibi Data Factory’ye yayımlamaya devam edebilirsiniz. 

1. Sol üst köşede **Data Factory**’yi seçin veya yanındaki aşağı oku kullanın ve **Kod Deposunu Yapılandır**’ı seçin. 

    ![Kod deposunu yapılandırma](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. **Depo Ayarları** sayfasında aşağıdaki adımları uygulayın:

    a. **Depo Türü** bölümünde **Visual Studio Team Services Git**’i seçin.

    b. **Visual Studio Team Services Hesabı** bölümünde Visual Studio Team Services hesabınızı seçin.

    c. **Proje Adı** bölümünde Visual Studio Team Services hesabınızdan bir proje seçin.

    d. **Git deposu adı** bölümünde veri fabrikanızla ilişkilendirilecek Git deposu için **Tutorial2** girin.

    e. **Mevcut Data Factory kaynaklarını depoya aktar** onay kutusunun seçili olduğundan emin olun.

    f. Ayarları kaydetmek için **Kaydet**’i seçin. 

    ![Depo ayarları](./media/tutorial-copy-data-portal/repository-settings.png)
3. Depo olarak **VSTS GIT**'in seçildiğini onaylayın.

    ![VSTS GIT’i seçin](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Web tarayıcısının ayrı bir sekmesinde **Tutorial2** deposuna gidin. İki dal görüntülenir: **adf_publish** ve **master**.

    ![Master ve adf_publish dalları](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Data Factory varlıkları için JSON dosyalarının **master** dalında yer aldığından emin olun.

    ![Master dalındaki dosyalar](./media/tutorial-copy-data-portal/master-branch-files.png)
6. JSON dosyalarının henüz **adf_publish** dalında yer almadığından emin olun. 

    ![Adf_publish dalındaki dosyalar](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. **Açıklama** bölümüne işlem hattı için bir açıklama ekleyin ve araç çubuğundaki **Kaydet** seçeneğini belirleyin. 

    ![İşlem hattı açıklaması](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Artık **Tutorial2** deposunda kullanıcı adınızla bir dal görüntülenir. Yaptığınız değişiklik master dalında değil sizin kendi dalınızda yer alır. Yalnızca master dalındaki varlıkları yayımlayabilirsiniz.

    ![Dalınız](./media/tutorial-copy-data-portal/your-branch.png)
9. Fare imleciyle **Eşitle** düğmesinin üzerine gelin (düğmeyi seçmeyin), değişikliklerinizi master dalıyla eşitlemek için **Değişiklikleri Gönder** onay kutusunu seçin ve **Eşitle** seçeneğini belirleyin. 

    ![Değişiklikleri gönderme ve eşitleme](./media/tutorial-copy-data-portal/commit-and-sync.png)
10. **Değişikliklerinizi eşitleyin** penceresinde aşağıdaki eylemleri uygulayın: 

    a. Güncelleştirilmiş **İşlem Hatları** listesinde **CopyPipeline** öğesinin görüntülendiğinden emin olun.

    b. **Eşitleme sonrasında değişiklikleri yayımla**'nın seçili olduğunu onaylayın. Bu onay kutusunu temizlediğinizde dalınızdaki değişiklikleriniz yalnızca master dalıyla eşitlenir. Data Factory’ye yayımlanmaz. Bunları daha sonra, **Yayımla** düğmesini kullanarak yayımlayabilirsiniz. Bu onay kutusunu seçtiğinizde değişiklikler öncelikle master dalı ile eşitlenir ve ardından Data Factory’ye yayımlanır.

    c. **Eşitle**’yi seçin. 

    ![Değişikliklerinizi eşitleyin](./media/tutorial-copy-data-portal/sync-your-changes.png)
11. Artık, **Tutorial2** deposunun **adf_publish** dalında dosyaları görebilirsiniz. Ayrıca bu dalda Data Factory çözümünüze yönelik Azure Resource Manager şablonunu da bulabilirsiniz. 

    ![Adf_publish dalındaki dosya listesi](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme
Bu adımda, önceki adımda yayımladığınız işlem hattını el ile tetiklersiniz. 

1. Araç çubuğunda **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin. **İşlem Hattı Çalıştırma** sayfasında **Son**’u seçin.  

    ![İşlem hatlarını tetikleme](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Soldaki **İzleyici** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve işlem hattını yeniden çalıştırmak için **Eylemler** sütunundaki bağlantıları kullanabilirsiniz.

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu örnekte, tek bir etkinlik olduğundan listede tek giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için en üstteki **İşlem Hatları**'nı seçin. Görünümü yenilemek için **Yenile**’yi seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. SQL veritabanında **emp** tablosuna iki satır daha eklendiğinden emin olun. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>İşlem hattını bir zamanlamaya göre tetikleme
Bu zamanlamada, işlem hattı için bir zamanlayıcı tetikleyicisi oluşturacaksınız. Tetikleyici, işlem hattını saatlik veya günlük gibi belirli bir zamanlamaya göre çalıştırır. Bu örnekte, tetikleyiciyi belirtilen bitiş tarihi saatine kadar dakikada bir çalıştırılacak şekilde ayarlarsınız. 

1. Soldaki **Düzenle** sekmesine gidin. 

    ![Düzenle sekmesi](./media/tutorial-copy-data-portal/edit-tab.png)
2. **Tetikleyici**'yi ve **Yeni/Düzenle**'yi seçin. İşlem hattı etkin değilse işlem hattına gidin. 

    ![Tetikleyici seçeneği](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. **Tetikleyici Ekle** penceresinde **Tetikleyici seç**’i ve sonra **+Yeni**’yi seçin. 

    ![Yeni düğmesi](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. **Yeni Tetikleyici** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** bölümüne **RunEveryMinute** girin.

    b. **Bitiş** bölümünde **Tarih** seçeneğini belirleyin.

    c. **Bitiş Tarihi** bölümde açılan listeden seçim yapın.

    d. **Geçerli gün** seçeneğini belirleyin. Varsayılan olarak, bitiş günü olarak bir sonraki gün ayarlanır.

    e. **dakika** bölümünü, geçerli tarih saati birkaç dakika geçecek şekilde güncelleştirin. Tetikleyicinin etkinleştirilmesi için, önce sizin değişiklikleri yayımlamanız gerekir. Birkaç dakika sonraya ayarlarsanız ve bu süre içinde değişiklikleri yayımlamazsanız, tetikleyici çalıştırması göremezsiniz.

    f. **Uygula**’yı seçin. 

    ![Tetikleyici özellikleri](./media/tutorial-copy-data-portal/set-trigger-properties.png)

    g. **Etkinleştirildi** seçeneğini belirleyin. Bunu devre dışı bırakabilir ve daha sonra bu onay kutusunu kullanarak etkinleştirebilirsiniz.

    h. **İleri**’yi seçin.

    ![Etkinleştirildi düğmesi](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Her bir işlem hattı çalıştırması ile bir maliyet ilişkilendirildiğinden bitiş tarihini uygun bir şekilde ayarlayın. 
5. **Tetikleyici Çalıştırma Parametreleri** sayfasında uyarıyı gözden geçirin ve **Son**'u seçin. Bu örnekteki işlem hattı hiçbir parametre almaz. 

    ![Tetikleyici çalıştırması parametreleri](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
6. Dalınızdaki değişiklikleri master dalıyla eşitlemek için **Eşitle**'yi seçin. Varsayılan olarak, **Eşitleme sonrasında değişiklikleri yayımla** seçili durumdadır. **Eşitle**'yi seçtiğinizde güncelleştirilmiş varlıklar master dalından Data Factory'ye de yayımlanır. Yayımlama başarılı olana kadar tetikleyici etkinleştirilmez.

    ![Değişikliklerinizi eşitleyin](./media/tutorial-copy-data-portal/sync-your-changes-with-trigger.png) 
7. Tetiklenen işlem hattı çalıştırmalarını görmek için sol taraftaki **İzleyici** sekmesine gidin. 

    ![Tetiklenen işlem hattı çalıştırmaları](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
8. **İşlem Hattı Çalıştırmaları** görünümünden **Tetikleyici Çalıştırmaları** görünümüne geçmek için, **İşlem Hattı Çalıştırmaları**'nı ve ardından **Tetikleyici Çalıştırmaları**'nı seçin.
    
    ![Tetikleyici çalıştırmaları](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
9. Listede tetikleyici çalıştırmalarını görürsünüz. 

    ![Tetikleyici çalıştırmaları listesi](./media/tutorial-copy-data-portal/trigger-runs-list.png)
10. Belirtilen bitiş saatine kadar **emp** tablosuna dakikada iki satır (her işlem hattı çalıştırması için) eklendiğini doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma.
> * İşlem hattını el ile tetikleme.
> * İşlem hattını bir zamanlamaya göre tetikleme.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.


Verileri şirket içinden buluta kopyalamayı öğrenmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Şirket içinden buluta veri kopyalama](tutorial-hybrid-copy-portal.md)
