---
title: "Öğretici: Visual Studio kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma | Microsoft Belgeleri"
description: "Bu öğreticide, Visual Studio kullanarak Kopyalama Etkinliği ile bir Azure Data Factory işlem hattı oluşturursunuz."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 01a6f060e6ae800b0de930c7c46ed60f73b530ac
ms.openlocfilehash: 58aae152e49a4e90822f98c9cf5ee7aad067ffa8


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Öğretici: Visual Studio kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma
> [!div class="op_single_selector"]
> * [Genel bakış ve önkoşullar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Bu öğretici, Visual Studio kullanarak bir Azure data factory oluşturmayı ve izlemeyi gösterir. Veri fabrikasındaki işlem hattı, Azure Blob Depolama’dan Azure SQL veritabanı’na veri kopyalamak için bir Kopyalama Etkinliği kullanır.

Bu eğitimin bir parçası olarak gerçekleştireceğiniz adımlar şunlardır:

1. İki bağlı hizmet oluşturun: **AzureStorageLinkedService1** ve **AzureSqlinkedService1**. 
   
    AzureStorageLinkedService1 bağlı hizmeti Azure depolamayı, AzureSqlLinkedService1 de Azure SQL veritabanını data factory: **ADFTutorialDataFactoryVS** konumuna bağlar. İşlem hattıyla ilgili girdi verileri Azure blob depolamadaki bir blob kapsayıcısında yer alırken çıktı verileri de Azure SQL veritabanındaki bir tabloda depolanır. Bu nedenle, bu iki veri deposunu bağlı hizmet olarak data factory’ye ekliyorsunuz.
2. Depolanan girdi/çıktı verilerini temsil eden iki veri kümesi oluşturun: **InputDataset** ve **OutputDataset**. 
   
    InputDataset için, kaynak verilerin bulunduğu blob’u içeren blob kapsayıcısını belirtin. OutputDataset için çıktı verilerini depolayan SQL tablosunu belirtin. Yapı, kullanılabilirlik ve ilke gibi diğer özellikleri de belirtirsiniz.
3. ADFTutorialDataFactoryVS’de **ADFTutorialPipeline** adlı işlem hattını oluşturun. 
   
    İşlem hattında, girdi verilerini Azure blob’tan çıktı Azure SQL tablosuna kopyalayan **Kopyalama Etkinliği** vardır. Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın. 
4. **VSTutorialFactory** adlı bir veri fabrikası oluşturun. Veri fabrikasını ve tüm Data Factory varlıklarını (bağlı hizmetler, tablolar ve işlem hattı) dağıtın.    

## <a name="prerequisites"></a>Ön koşullar
1. [Öğreticiye Genel Bakış](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) makalesinin tamamını okuyun ve **ön koşul** adımlarını tamamlayın. 
2. Azure Data Factory’ye Data Factory varlıkları yayımlayabilmek için bir **Azure aboneliğinin yöneticisi** olmanız gerekir.  
3. Bilgisayarınızda şunların yüklü olması gerekir: 
   * Visual Studio 2013 veya Visual Studio 2015
   * Visual Studio 2013 veya Visual Studio 2015 için Azure SDK’sını indirin. [Azure İndirme Sayfası](https://azure.microsoft.com/downloads/)’na gidin ve **.NET** bölümündeki **VS 2013** veya **VS 2015**’e tıklayın.
   * Visual Studio için en son Azure Data Factory eklentisini indirin: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) veya [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Aşağıdaki adımları uygulayarak eklentiyi güncelleştirebilirsiniz: Menüde **Araçlar** -> **Uzantılar ve Güncelleştirmeler** -> **Çevrimiçi** -> **Visual Studio Galerisi** -> **Visual Studio için Microsoft Azure Data Factory Araçları** -> **Güncelleştir**’e tıklayın.

## <a name="create-visual-studio-project"></a>Visual Studio projesi oluşturma
1. **Visual Studio 2013**’ü başlatın. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın. **Yeni Proje** iletişim kutusu görmeniz gerekir.  
2. **Yeni Proje** iletişim kutusunda **DataFactory** şablonunu seçip **Boş Data Factory Projesi**’ne tıklayın. DataFactory şablonunu görmüyorsanız, Visual Studio’yu kapatın, Visual Studio 2013 için Azure SDK'sını yükleyin ve Visual Studio’yu yeniden açın.  
   
    ![Yeni proje iletişim kutusu](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Proje için bir **ad**, **konum** ve **çözüm** için ad girip **Tamam**’a tıklayın.
   
    ![Çözüm Gezgini](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Kopyalama Etkinliği tarafından desteklenen tüm kaynaklar ve havuzlar için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data Factory tarafından desteklenen işlem hizmetlerinin listesi için bkz. [bağlantılı işlem hizmetleri](data-factory-compute-linked-services.md). Bu öğreticide herhangi bir işlem hizmeti kullanmayın. 

Bu adımda, iki bağlı hizmet oluşturursunuz: **AzureStorageLinkedService1** ve **AzureSqlLinkedService1**. AzureStorageLinkedService1 bağlı hizmeti Azure Storage Hesabını, AzureSqlLinkedService de Azure SQL veritabanını data factory: **ADFTutorialDataFactory** konumuna bağlar. 

### <a name="create-the-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
1. Çözüm gezgininde **Bağlı Hizmetler**’e sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.      
2. **Yeni Öğe Ekle** iletişim kutusunda **Azure Storage Bağlı Hizmeti**’ni listeden seçip **Ekle**’ye tıklayın. 
   
    ![Yeni Bağlı Hizmet](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. `<accountname>` ve `<accountkey>`* sözcüklerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin. 
   
    ![Azure Storage Bağlı Hizmeti](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. **AzureStorageLinkedService1.json** dosyasını kaydedin.

> JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob’dan/Azure Blob’a veri taşıma](data-factory-azure-blob-connector.md#azure-storage-linked-service).
> 
> 

### <a name="create-the-azure-sql-linked-service"></a>Azure SQL bağlı hizmeti oluşturma
1. **Çözüm Gezgini**’nde bir kez daha **Bağlı Hizmetler** düğümüne sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın. 
2. Bu sefer, **Azure SQL Bağlı Hizmeti**’ni seçin ve **Ekle**’ye tıklayın. 
3. **AzureSqlLinkedService1.json dosyasında** `<servername>`, `<databasename>`, `<username@servername>` ve `<password>` öğesini Azure SQL sunucusu, veritabanı, kullanıcı hesabı ve parolası ile değiştirin.    
4. **AzureSqlLinkedService1.json** dosyasını kaydedin. 

> [!NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [SQL Veritabanı’ndan/SQL Veritabanı’na veri taşıma](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Önceki adımda, Azure Storage hesabını ve Azure SQL veritabanını data factory’ye bağlamak için **AzureStorageLinkedService1** ve **AzureSqlLinkedService1** bağlı hizmetlerini oluşturdunuz; burada söz edilen data factory adı: **ADFTutorialDataFactory**. Bu adımda, sırasıyla AzureStorageLinkedService1 ve AzureSqlLinkedService1 tarafından başvurulan veri depolarında depolanan girdi/çıktı verilerini temsil eden **InputDataset** ve **OutputDataset** adlı iki data factory tablosunu tanımlayın. InputDataset için, kaynak verileriyle birlikte bir blob içeren blob kapsayıcısını belirtin. OutputDataset için çıktı verilerini depolayan SQL tablosunu belirtin.

### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
Bu adımda, Azure Storage hizmetinde **AzureStorageLinkedService1** bağlı hizmetiyle temsil edilen bir blob kapsayıcısını işaret eden **InputDataset** adlı bir veri kümesi oluşturacaksınız. Tablo dikdörtgen bir veri kümesidir ve şu anda yalnızca bu veri kümesi türü desteklenmektedir. 

1. **Çözüm Gezgini**’nde **Tablolar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.
2. **Yeni Öğe Ekle** iletişim kutusunda, **Azure Blob**’u seçin ve **Ekle**’ye tıklayın.   
3. JSON metnini aşağıdaki metinle değiştirin ve **AzureBlobLocation1.json** dosyasını kaydedin. 

  ```json   
  {
    "name": "InputDataset",
    "properties": {
      "structure": [
        {
          "name": "FirstName",
          "type": "String"
        },
        {
          "name": "LastName",
          "type": "String"
        }
      ],
      "type": "AzureBlob",
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
        "folderPath": "adftutorial/",
        "format": {
          "type": "TextFormat",
          "columnDelimiter": ","
        }
      },
      "external": true,
      "availability": {
        "frequency": "Hour",
        "interval": 1
      }
    }
  }
  ``` 
    Aşağıdaki noktalara dikkat edin: 
   
   * veri kümesi **türü** **AzureBlob** olarak ayarlanır.
   * **linkedServiceName** **AzureStorageLinkedService** olarak ayarlanır. Bu bağlı hizmeti 2. adımda oluşturmuştunuz.
   * **folderPath** **adftutorial** kapsayıcısı olarak ayarlanır. Ayrıca **fileName** özelliğini kullanarak klasörün içinde bir blob’un adını belirtebilirsiniz. Blob adını belirtmediğinizden, kapsayıcıdaki tüm blob'lara ait veriler girdi verisi olarak kabul edilir.  
   * biçim **türü** **TextFormat** olarak ayarlanır
   * Metin dosyasında virgül karakteriyle (**columnDelimiter**) ayrılmış, **FirstName** ve **LastName** adlı iki alan vardır    
   * **Availability** **hourly** olarak ayarlanmıştır (**sıklık** **saat** olarak, **aralık** ise **1** olarak ayarlanmıştır). Bu nedenle, Data Factory belirttiğiniz blob kapsayıcısının (**adftutorial**) kök klasöründe girdi verilerini saatte bir kere arar. 
   
   **Girdi** veri kümesi için bir **fileName** belirtmezseniz, girdi klasörüne (**folderPath**) ait tüm dosyalar/blob’lar girdi olarak kabul edilir. JSON’da fileName belirtmediyseniz, yalnızca belirtilen dosya/blob girdi olarak kabul edilir.
   
   **Çıktı tablosu** için bir **fileName** belirtmezseniz, **folderPath**’de oluşturulan dosyaları şu biçimde adlandırılır: Data.&lt;Guid\&gt;.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).
   
   **folderPath** ve **fileName** öğelerini dinamik olarak **SliceStart** zamanı temelinde ayarlamak için **partitionedBy** özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2016-09-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2016/09/20, fileName de 08.csv olarak ayarlanır. 
  
    ```json   
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ```
            
> [!NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob’dan/Azure Blob’a veri taşıma](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
> 
> 

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma
Bu adımda **OutputDataset** adlı bir çıktı veri kümesi oluşturursunuz. Bu veri kümesi, **AzureSqlLinkedService1** ile temsil edilen Azure SQL veritabanında bir SQL tablosunu işaret eder. 

1. **Çözüm Gezgini**’nde bir kez daha **Tablolar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.
2. **Yeni Öğe Ekle** iletişim kutusunda, **Azure SQL**’i seçin ve **Ekle**’ye tıklayın. 
3. JSON metnini aşağıdaki JSON ile değiştirin ve **AzureSqlTableLocation1.json** dosyasını kaydedin.

    ```json
    {
     "name": "OutputDataset",
     "properties": {
       "structure": [
         {
           "name": "FirstName",
           "type": "String"
         },
         {
           "name": "LastName",
           "type": "String"
         }
       ],
       "type": "AzureSqlTable",
       "linkedServiceName": "AzureSqlLinkedService1",
       "typeProperties": {
         "tableName": "emp"
       },
       "availability": {
         "frequency": "Hour",
         "interval": 1
       }
     }
    }
    ```
   
    Aşağıdaki noktalara dikkat edin: 
   
   * veri kümesi **türü** **AzureSQLTable** olarak ayarlanır.
   * **linkedServiceName** **AzureSqlLinkedService** olarak ayarlanır (bu bağlı hizmeti 2. adımda oluşturmuştunuz).
   * **tablename** **emp** olarak ayarlanır.
   * Veritabanındaki emp tablosunda üç sütun vardır: **ID**, **FirstName** ve **LastName**. ID bir kimlik sütunu olduğundan, burada yalnızca **FirstName** ve **LastName** değerlerini belirtmeniz gerekir.
   * **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmeti Azure SQL veritabanındaki **emp** tablosunda her saat bir çıktı veri dilimi oluşturur.

> [!NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [SQL Veritabanı’ndan/SQL Veritabanı’na veri taşıma](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## <a name="create-pipeline"></a>İşlem hattı oluşturma
Şu ana kadar girdi/çıktı bağlı hizmetleri ve tablolarını oluşturdunuz. Şimdi de, verileri Azure blob’tan Azure SQL veritabanına kopyalamak için **Kopyalama Etkinliği**’ne sahip bir işlem hattı oluşturursunuz. 

1. **Çözüm Gezgini**’nde **İşlem Hatları**’na sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.  
2. **Yeni Öğe Ekle** iletişim kutusunda **Veri İşlem Hattı Kopyala**’yı seçip **Ekle**’ye tıklayın. 
3. JSON’u aşağıdaki JSON ile değiştirin ve **CopyActivity1.json** dosyasını kaydedin.

    ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob to Azure SQL table",
       "activities": [
         {
           "name": "CopyFromBlobToSQL",
           "type": "Copy",
           "inputs": [
             {
               "name": "InputDataset"
             }
           ],
           "outputs": [
             {
               "name": "OutputDataset"
             }
           ],
           "typeProperties": {
             "source": {
               "type": "BlobSource"
             },
             "sink": {
               "type": "SqlSink",
               "writeBatchSize": 10000,
               "writeBatchTimeout": "60:00:00"
             }
           },
           "Policy": {
             "concurrency": 1,
             "executionPriorityOrder": "NewestFirst",
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2015-07-12T00:00:00Z",
       "end": "2015-07-13T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
   Aşağıdaki noktalara dikkat edin:
   
   * Etkinlikler bölümünde, **türü** **Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
   * Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır.
   * **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.
   
   **start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin. Tarih saatin yalnızca tarih bölümünü belirtip saat bölümünü atlayabilirsiniz. Örneğin, "2016-02-03", "2016-02-03T00:00:00Z" ile eşdeğerdir
   
   Başlangıç ve bitiş tarih saatleri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2016-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız. 
   
   **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın.
   
   Önceki örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.

## <a name="publishdeploy-data-factory-entities"></a>Data Factory varlıklarını yayımlama/dağıtma
Bu adımda daha önce oluşturduğunuz Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) yayımlayacaksınız. Ayrıca bu varlıkları tutmak için oluşturulacak yeni veri fabrikasının adını belirteceksiniz.  

1. Çözüm Gezgini’nde projeye sağ tıklayın ve ardından **Yayımla**’ya tıklayın. 
2. **Microsoft hesabınızda oturum açın** iletişim kutusunu görmezseniz, Azure aboneliğindeki kimlik bilgilerini hesap için girin ve **oturum aç**’a tıklayın.
3. Aşağıdaki iletişim kutusunu göreceksiniz:
   
   ![Yayımla iletişim kutusu](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Data factory yapılandırma sayfasında aşağıdaki adımları uygulayın: 
   
   1. **Yeni Data Factory Oluştur** seçeneğini seçin.
   2. **Ad** için **VSTutorialFactory** girin.  
      
      > [!IMPORTANT]
      > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Yayımlandığında data factory adıyla ilgili bir hata alırsanız data factory adını değiştirip (örneğin, yournameVSTutorialFactory) yayımlamayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.        
      > 
      > 
   3. **Abonelik** alanı için Azure aboneliğinizi seçin.
      
      > [!IMPORTANT]
      > Herhangi bir abonelik görmüyorsanız aboneliğin yöneticisi veya ortak yöneticisi olan bir hesapla oturum açtığınızdan emin olun.  
      > 
      > 
   4. oluşturulacak data factory için **kaynak grubu** seçin. 
   5. Data factory için **bölge** seçin. Açılır listede yalnızca Data Factory hizmeti tarafından desteklenen bölgeler gösterilmektedir.
   6. **Öğeleri Yayımla** sayfasına geçmek için **İleri**’ye tıklayın.
      
       ![Veri fabrikası yapılandırma sayfası](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. **Öğeleri Yayımla** sayfasında tüm Data Factory varlıklarının işaretli olmasını sağlayın ve **Özet** sayfasına geçmek için **İleri**’ye tıklayın.
   
   ![Öğe yayımlama sayfası](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Özeti gözden geçirin, dağıtım işlemini başlatmak ve **Dağıtım Durumu**’nu görüntülemek için **İleri**’ye tıklayın.
   
   ![Özet yayımlama sayfası](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. **Dağıtım Durumu** sayfasında dağıtım sürecinin durumunu görmelisiniz. Dağıtımını gerçekleştirdikten sonra Son'a tıklayın.
 
   ![Dağıtım durumu sayfası](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Aşağıdaki noktalara dikkat edin: 

* Şu hatayı alırsanız: "**Abonelik, Microsoft.DataFactory ad alanını kullanacak şekilde kaydedilmemiş**", aşağıdakilerden birini yapın ve yeniden yayımlamayı deneyin: 
  
  * Azure PowerShell’de Data Factory sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Data Factory sağlayıcısının kayıtlı olduğunu onaylamak için aşağıdaki komutu çalıştırabilirsiniz. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Azure aboneliğini kullanarak [Azure portalında](https://portal.azure.com) oturum açın ve Data Factory dikey penceresine gidin (ya da) Azure portalında bir data factory oluşturun. Bu eylem sağlayıcıyı sizin için otomatik olarak kaydeder.
* Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.

> [!IMPORTANT]
> Data Factory örnekleri oluşturmak için Azure aboneliğinde yönetici/ortak yönetici olmanız gerekir
> 
> 

## <a name="summary"></a>Özet
Bu öğreticide Azure blob’undan Azure SQL veritabanına veri kopyalamak üzere Azure data factory oluşturdunuz. Data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturmak için Visual Studio’yu kullandınız. Bu öğreticide gerçekleştirilen üst düzey adımları şunlardır:  

1. Oluşturulan Azure **data factory**.
2. Oluşturulan **bağlı hizmetler**:
   1. Girdi verilerini tutan Azure Storage hesabınıza bağlamak için **Azure Storage** bağlı hizmeti.     
   2. Çıktı verilerini tutan Azure SQL veritabanınıza bağlamak için **Azure SQL** bağlı hizmeti. 
3. İşlem hatları için girdi ve çıktı verilerini açıklayan **veri kümeleri** oluşturuldu.
4. Kaynak olarak **BlobSource**’u, havuz olarak da **SqlSink**’i kapsayan **Kopyalama Etkinliği**’ne sahip oluşturulan **işlem hattı**. 

## <a name="use-server-explorer-to-view-data-factories"></a>Data factory’leri görüntülemek için Sunucu Gezgini’ni kullanın
1. **Visual Studio**’nun menüsünde **Görünüm**’e ve **Sunucu Gezgini**’ne tıklayın.
2. Sunucu Gezgini penceresinde, **Azure**’ü ve **Data Factory**’yi genişletin. **Visual Studio'da oturum açın**’ı görürseniz Azure aboneliğiyle ilişkili **hesabı** girin ve **Devam**’a tıklayın. **parola** girip **Oturum aç**’a tıklayın. Visual Studio, aboneliğinizdeki tüm Azure data factory’leri hakkında bilgi almaya çalışır. Bu işlemin durumunu **Data Factory Görev Listesi** penceresinde görürsünüz.

    ![Sunucu Gezgini](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. İstediğiniz data factory’ye sağ tıklayıp, mevcut bir data factory’ye dayandırılan Visual Studio projesi oluşturmak için Data Factory’yi Yeni Projeye Aktar’ı seçin.

    ![Data factory’yi VS projesine aktarma](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Visual Studio için Data Factory araçlarını güncelleştirme
Visual Studio için Azure Data Factory araçlarını güncelleştirmek üzere aşağıdaki adımları uygulayın:

1. Menüde **Araçlar**’a tıklayın ve **Uzantılar ve Güncelleştirmeler**’i seçin. 
2. Sol bölmede **Güncelleştirmeler**’i, sonra da **Visual Studio Galerisi**’ni seçin.
3. **Visual Studio için Azure Data Factory araçları**’nı seçin ve **Güncelleştir**’e tıklayın. Bu girişi görmüyorsanız araçların en son sürümü zaten yüklüdür. 

Bu öğreticide oluşturduğunuz işlem hattını ve veri kümelerini izlemek üzere Azure Portal’ın ilişkin yönergeler için bkz. [Veri kümelerini ve işlem hatlarını izleme](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline).

## <a name="see-also"></a>Ayrıca Bkz.
| Konu | Açıklama |
|:--- |:--- |
| [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) |Bu makalede, öğreticide kullandığınız Kopyalama Etkinliği hakkında ayrıntılı bilgi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) |Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) |Bu makale, Azure Data Factory’deki işlem hatlarını ve veri kümelerini anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) |Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur. |
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) |Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. |




<!--HONumber=Dec16_HO3-->


