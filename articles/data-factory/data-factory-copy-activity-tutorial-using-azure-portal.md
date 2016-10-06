<properties 
    pageTitle="Öğretici: Azure portalı kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma | Microsoft Azure" 
    description="Bu öğreticide, Azure Portal'daki Data Factory Düzenleyiciyi kullanarak Kopyalama Etkinliği ile bir Azure Data Factory işlem hattı oluşturursunuz." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>


# Öğretici: Azure portalı kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma
> [AZURE.SELECTOR]
- [Genel bakış ve ön koşullar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)


Bu öğretici, Azure portalını kullanarak bir Azure data factory oluşturmayı ve izlemeyi gösterir. Veri fabrikasındaki işlem hattı, Azure Blob Depolama’dan Azure SQL veritabanı’na veri kopyalamak için bir Kopyalama Etkinliği kullanır.

Bu eğitimin bir parçası olarak gerçekleştireceğiniz adımlar şunlardır:

Adım | Açıklama
-----| -----------
[Azure Data Factory oluşturma](#create-data-factory) | Bu adımda, **ADFTutorialDataFactory** adlı bir Azure data factory oluşturursunuz.  
[Bağlı hizmetler oluşturma](#create-linked-services) | Bu adımda iki bağlı hizmet oluşturursunuz: **AzureStorageLinkedService** ve **AzureSqlLinkedService**. <br/><br/>AzureStorageLinkedService Azure depolamayı, AzureSqlLinkedService de Azure SQL veritabanını ADFTutorialDataFactory konumuna bağlar. İşlem hattıyla ilgili girdi verileri Azure blob depolamadaki bir blob kapsayıcısında yer alırken çıktı verileri de Azure SQL veritabanındaki bir tabloda depolanır. Bu nedenle, bu iki veri deposunu bağlı hizmet olarak data factory’ye ekliyorsunuz.      
[Girdi ve çıktı veri kümeleri oluşturma](#create-datasets) | Önceki adımda girdi/çıktı verilerini kapsayan veri depolarına başvuran bağlı hizmetleri oluşturdunuz. Bu adımda veri depolarına depolanan girdi/çıktı verilerini temsil eden **InputDataset** ve **OutputDataset** adlı iki veri kümesi tanımlayacaksınız. <br/><br/>InputDataset için, kaynak verilere sahip bir blob’un bulunduğu blob kapsayıcısını ve OutputDataset için çıktı verilerini depolayan SQL tablosunu belirtin. Yapı, kullanılabilirlik ve ilke gibi diğer özellikleri de belirtirsiniz. 
[İşlem hattı oluşturma](#create-pipeline) | Bu adımda, ADFTutorialDataFactory’de **ADFTutorialPipeline** adlı işlem hattını oluşturursunuz. <br/><br/>İşlem hattına, girdi verilerini Azure blob’dan çıktı Azure SQL tablosuna kopyalayan bir **Kopyalama Etkinliği** ekleyin. Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Bu etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın. 
[İşlem hattını izleme](#monitor-pipeline) | Bu adımda, girdi ve çıktı tablolarının dilimlerini Azure Portal’ı kullanarak izlersiniz.

## Ön koşullar 
Bu öğreticiyi uygulamadan önce [Öğreticiye Genel Bakış](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) makalesinde listelenen önkoşulları tamamlayın.

## Veri fabrikası oluşturma
Bu adımda **ADFTutorialDataFactory** adlı bir Azure data factory oluşturmak için Azure Portal’ı kullanırsınız.

1.  [Azure portalında](https://portal.azure.com/) oturum açtıktan sonra **Yeni**’ye tıklayın, **Intelligence + Analytics**’i seçin ve **Data Factory**’ye tıklayın. 

    ![Yeni->DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)  

6. **Yeni data factory** dikey penceresinde:
    1. **Ad** için **ADFTutorialDataFactory** girin. 
    
        ![Yeni veri fabrikası dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)

        Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızADFTutorialDataFactory) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
    
            Data factory name “ADFTutorialDataFactory” is not available  
     
        ![Data Factory adı yok](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
    2. Azure **aboneliğinizi** seçin.
    3. Kaynak Grubu için aşağıdaki adımlardan birini uygulayın:
        1. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
        2. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
    
            Bu öğreticideki adımlardan bazıları kaynak grubu için şu adı kullandığınızı varsayar: **ADFTutorialResourceGroup**. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../resource-group-overview.md).  
    4. Data factory için **konum** seçin. Açılır listede yalnızca Data Factory hizmeti tarafından desteklenen bölgeler gösterilmektedir.
    5. **Başlangıç Panosuna Sabitle**'yi seçin.     
    6. **Oluştur**’a tıklayın.

        > [AZURE.IMPORTANT] Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory Katılımcısı](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) rolünün üyesi olmanız gerekir.
        >  
        >  Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.              
9.  Durum/bildirim iletilerini görmek için araç çubuğundaki zil simgesine tıklayın. 

    ![Bildirim iletileri](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
10. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** dikey penceresini görürsünüz.

    ![Data factory giriş sayfası](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Kopyalama Etkinliği tarafından desteklenen tüm kaynaklar ve havuzlar için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md##supported-data-stores-and-formats). Data Factory tarafından desteklenen işlem hizmetlerinin listesi için bkz. [bağlantılı işlem hizmetleri](data-factory-compute-linked-services.md). Bu öğreticide herhangi bir işlem hizmeti kullanmayın. 

Bu adımda iki bağlı hizmet oluşturursunuz: **AzureStorageLinkedService** ve **AzureSqlLinkedService**. AzureStorageLinkedService bağlı hizmeti Azure Storage Hesabını, AzureSqlLinkedService de Azure SQL veritabanını **ADFTutorialDataFactory** konumuna bağlar. Daha sonra bu öğreticide, verileri AzureStorageLinkedService’teki bir blob kapsayıcısından AzureSqlLinkedService’teki bir SQL tablosuna kopyalayan bir işlem hattı oluşturacaksınız.

### Azure depolama hesabı için bağlı hizmet oluşturma
1.  **Data Factory** dikey penceresinde **Geliştir ve dağıt** kutucuğuna tıklayarak data factory için **Düzenleyici**’yi başlatın.

    ![Geliştir ve Dağıt Kutucuğu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
5. **Düzenleyici**’de, araç çubuğundaki **Yeni veri deposu** düğmesine tıklayın ve açılan menüden **Azure depolama**’yı seçin. Sağ bölmede Azure depolama bağlı hizmeti oluşturmak için JSON şablonunu görmeniz gerekir. 

    ![Düzenleyici Yeni veri deposu düğmesi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
6. Burada, `<accountname>` ve `<accountkey>` sözcüklerini Azure depolama hesabınıza ait hesap adı ve hesap anahtarı değerleriyle değiştirin. 

    ![Düzenleyici Blob Storage JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png) 
6. Araç çubuğunda **Dağıt**’a tıklayın. Dağıtılan **AzureStorageLinkedService** öğesini şu anda ağaçta görmeniz gerekir. 

    ![Düzenleyici Blob Storage Dağıtma](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [AZURE.NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob’dan/Azure Blob’a veri taşıma](data-factory-azure-blob-connector.md#azure-storage-linked-service).

### Azure SQL Database için bağlı hizmet oluşturma
1. **Data Factory Düzenleyici**’de, araç çubuğundaki **Yeni veri deposu** düğmesine tıklayın ve açılan menüden **Azure SQL Veritabanı**’nı seçin. Sağ bölmede Azure SQL bağlı hizmeti oluşturmak için JSON şablonunu görmeniz gerekir.
2. `<servername>`, `<databasename>`, `<username>@<servername>` ve `<password>` öğesini Azure SQL sunucusu, veritabanı, kullanıcı hesabı ve parolası ile değiştirin. 
3. **AzureSqlLinkedService**’i oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın.
4. Ağaç görünümünde **AzureSqlLinkedService** öğesini gördüğünüzü onaylayın. 

> [AZURE.NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [SQL Veritabanı’ndan/SQL Veritabanı’na veri taşıma](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).

## Veri kümeleri oluşturma
Önceki adımda, Azure Storage hesabını ve Azure SQL veritabanını data factory’ye bağlamak için **AzureStorageLinkedService** ve **AzureSqlLinkedService** bağlı hizmetlerini oluşturdunuz; burada söz edilen data factory adı: **ADFTutorialDataFactory**. Bu adımda, sırasıyla AzureStorageLinkedService ve AzureSqlLinkedService tarafından başvurulan veri depolarında depolanan girdi/çıktı verilerini temsil eden **InputDataset** ve **OutputDataset** adlı iki data factory tablosunu tanımlayın. InputDataset için, kaynak verilere sahip bir blob’un bulunduğu blob kapsayıcısını ve OutputDataset için çıktı verilerini depolayan SQL tablosunu belirtin. 

### Girdi veri kümesi oluşturma 
Bu adımda, Azure Storage hizmetinde **AzureStorageLinkedService** bağlı hizmetiyle temsil edilen bir blob kapsayıcısını işaret eden **InputDataset** adlı bir veri kümesi oluşturacaksınız.

1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla**, **Yeni veri kümesi** ve **Azure Blob depolama** öğelerine tıklayın. 

    ![Yeni veri kümesi menüsü](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin: 

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
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
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
        
     Aşağıdaki noktalara dikkat edin: 
    
    - veri kümesi **türü** **AzureBlob** olarak ayarlanır.
    - **linkedServiceName** **AzureStorageLinkedService** olarak ayarlanır. Bu bağlı hizmeti 2. adımda oluşturmuştunuz.
    - **folderPath** **adftutorial** kapsayıcısı olarak ayarlanır. Ayrıca **fileName** özelliğini kullanarak klasörün içinde bir blob’un adını belirtebilirsiniz. Blob adını belirtmediğinizden, kapsayıcıdaki tüm blob'lara ait veriler girdi verisi olarak kabul edilir.  
    - biçim **türü** **TextFormat** olarak ayarlanır
    - Metin dosyasında virgül karakteriyle (**columnDelimiter**) ayrılmış, **FirstName** ve **LastName** adlı iki alan vardır 
    - **Availability** **hourly** olarak ayarlanmıştır (**sıklık** **saat** olarak, **aralık** ise **1** olarak ayarlanmıştır). Bu nedenle, Data Factory belirttiğiniz blob kapsayıcısının (**adftutorial**) kök klasöründe girdi verilerini saatte bir kere arar. 
    
    **Girdi** veri kümesi için bir **fileName** belirtmezseniz, girdi klasörüne (**folderPath**) ait tüm dosyalar/blob’lar girdi olarak kabul edilir. JSON’da fileName belirtmediyseniz, yalnızca belirtilen dosya/blob girdi olarak kabul edilir.
 
    **Çıktı tablosu** için bir **fileName** belirtmezseniz, **folderPath**’de oluşturulan dosyaları şu biçimde adlandırılır: Data.&lt;Guid\&gt;.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **folderPath** ve **fileName** öğelerini dinamik olarak **SliceStart** zamanı temelinde ayarlamak için **partitionedBy** özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2016-09-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2016/09/20, fileName de 08.csv olarak ayarlanır. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],
2. **InputDataset** veri kümesini oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. **InputDataset** öğesini ağaç görünümünde gördüğünüzü onaylayın.

> [AZURE.NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob’dan/Azure Blob’a veri taşıma](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).

### Çıktı veri kümesi oluşturma
Adımın bu bölümünde **OutputDataset** adlı bir çıktı veri kümesi oluşturursunuz. Bu veri kümesi, **AzureSqlLinkedService** ile temsil edilen Azure SQL veritabanında bir SQL tablosunu işaret eder. 

1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla**, **Yeni veri kümesi** ve **Azure SQL** öğelerine tıklayın. 
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin:

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
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Aşağıdaki noktalara dikkat edin: 
    
    - veri kümesi **türü** **AzureSQLTable** olarak ayarlanır.
    - **linkedServiceName** **AzureSqlLinkedService** olarak ayarlanır (bu bağlı hizmeti 2. adımda oluşturmuştunuz).
    - **tablename** **emp** olarak ayarlanır.
    - Veritabanındaki emp tablosunda üç sütun vardır: **ID**, **FirstName** ve **LastName**. ID bir kimlik sütunu olduğundan, burada yalnızca **FirstName** ve **LastName** değerlerini belirtmeniz gerekir.
    - **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmeti Azure SQL veritabanındaki **emp** tablosunda her saat bir çıktı veri dilimi oluşturur.

3. **OutputDataset** veri kümesini oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. **OutputDataset** öğesini ağaç görünümünde gördüğünüzü onaylayın. 

> [AZURE.NOTE]
> JSON özellikleri hakkında ayrıntılar için bkz. [SQL Veritabanı’ndan/SQL Veritabanı’na veri taşıma](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).

## İşlem hattı oluşturma
Bu adımda, girdi olarak **InputDataset** ve çıktı olarak **OutputDataset** kullanan **Kopyalama Etkinliği**’ne sahip bir işlem hattı oluşturursunuz.

1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla** ve **Yeni işlem hattı** öğelerine tıklayın. Alternatif olarak, ağaç görünümünde **İşlem hatları**’na sağ tıklayın ve**Yeni işlem hattı**’na tıklayın.
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin: 
        
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
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
          }
        } 

    Aşağıdaki noktalara dikkat edin:

    - Etkinlikler bölümünde, **türü** **Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
    - Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır.
    - **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.

    **start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin. Tarih saatin yalnızca tarih bölümünü belirtip saat bölümünü atlayabilirsiniz. Örneğin, "2016-02-03", "2016-02-03T00:00:00Z" ile eşdeğerdir
    
    Başlangıç ve bitiş tarih saatleri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2016-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız. 
    
    **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın.
    
    Önceki örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.
    
4. **ADFTutorialPipeline** tablosunu oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. İşlem hattını ağaç görünümünde gördüğünüzü onaylayın. 
5. Şimdi, **Düzenleyici** dikey penceresini **X** işaretine tıklayarak kapatın. **X** simgesine yeniden tıklayarak **ADFTutorialDataFactory** için **Data Factory** giriş sayfasını görüntüleyin.

**Tebrikler!** Başarılı bir şekilde Azure data factory, bağlı hizmetler, tablolar ve işlem hattı oluşturdunuz, işlem hattını zamanladınız.   
 
### Data factory’yi Diyagram Görünümünde görüntüleme 
1. **Data Factory** dikey penceresinde **Diyagram**’a tıklayın.

    ![Data Factory Dikey Penceresi - Diyagram Kutucuğu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Aşağıdaki görüntüye benzer bir diyagram görmeniz gerekir: 

    ![Diyagram görünümü](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)

    İşlem hatlarını ve tabloları yakınlaştırabilir, uzaklaştırabilir, %100 yakınlaştırabilir, sığacak kadar yakınlaştırabilirsiniz ve çizgileri gösterebilirsiniz (seçilen öğelerin yukarı akış ve aşağı akış öğelerini vurgular).  Özelliklerini görmek için bir nesneye (girdi/çıktı tablosu veya işlem hattı) çift tıklayabilirsiniz. 
3. Diyagram Görünümü’nde **ADFTutorialPipeline**’a sağ tıklayın ve **Ardışık düzeni aç**’a tıklayın. 

    ![İşlem Hattını Açma](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. İşlem hattında etkinlikleri, etkinliklerle ilgili girdi ve çıktı veri kümeleriyle birlikte görebilirsiniz. Bu öğreticide, işlem hattında girdi verisi olarak InputDataset, çıktı verisi olarak da OutputDataset bulunan tek bir etkinliğiniz (Kopyalama Etkinliği) vardır.   

    ![Açık işlem hattı görünümü](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Diyagram görünümüne dönmek için sol üst köşede yer alan içerik haritasındaki **Data factory**’ye tıklayın. Diyagram görünümü tüm işlem hatlarını görüntüler. Bu örnekte, yalnızca bir işlem hattı oluşturdunuz.   
 

## İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure Portal kullanacaksınız. 

### Diyagram Görünümünü kullanarak işlem hattını izleme

1. Veri fabrikasına ait Data Factory giriş sayfasını görmek üzere **Diyagram** görünümünü kapatmak için **X** simgesine tıklayın. Web tarayıcısını kapattıysanız aşağıdaki adımları uygulayın: 
    2. [Azure portalına](https://portal.azure.com/) gidin. 
    2. **Başlangıç Panosu** üzerindeki **ADFTutorialDataFactory** öğesine çift tıklayın (veya) sol menüdeki **Veri fabrikaları** öğesine tıklayıp ADFTutorialDataFactory araması yapın. 
3. Bu dikey pencerede tabloların adlarını ve sayısını, oluşturduğunuz işlem hattını görmeniz gerekir.

    ![adların olduğu giriş sayfası](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
4. Şimdi, **Veri kümeleri** kutucuğuna tıklayın.
5. **Veri kümeleri** dikey penceresinde **InputDataset**’e tıklayın. Bu veri kümesi, **ADFTutorialPipeline** için girdi veri kümesidir.

    ![InputDataset seçiliyken veri kümeleri](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Tüm veri dilimlerini görmek için **… (üç nokta)** seçeneğine tıklayın.

    ![Tüm girdi veri dilimleri](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  

    **emp.txt** dosyası her zaman **adftutorial\input** blob kapsayıcısında yer aldığından geçerli zamana kadar olan tüm veri dilimleri **Hazır**’dır. Alttaki **En son başarısız olan dilimler** bölümünde hiç dilim gösterilmediğini onaylayın.

    Hem **En son güncelleştirilen dilimler**, hem de **En son başarısız olan dilimler** listesi **SON GÜNCELLEŞTİRME ZAMANI**’na göre listelenir. 
    
    Dilimlere filtre uygulamak için araç çubuğunda **Filtre**’ye tıklayın.  
    
    ![Girdi dilimlerini filtreleme](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. **Veri kümeleri** dikey penceresini görene kadar dikey pencereleri kapatın. **OutputDataset** öğesine tıklayın. Bu veri kümesi, **ADFTutorialPipeline** için çıktı veri kümesidir.

    ![veri kümeleri dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
6. **OutputDataset** dikey penceresini aşağıda resimde olduğu gibi görmeniz gerekir:

    ![tablo dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
7. Geçerli zamana kadar olan veri dilimlerinin zaten oluşturulduğunu ve **Hazır** olduklarını unutmayın. Alttaki **Sorun dilimleri** bölümünde hiç dilim gösterilmiyor.
8. Tüm dilimleri görmek için **… (Üç nokta)** seçeneğine tıklayın.

    ![veri dilimleri dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
9. Listeden herhangi bir veri dilimine tıklayın; **Veri dilimi** dikey penceresini görmeniz gerekir.

    ![veri dilimi dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
  
    Dilim **Hazır** durumunda değilse, Hazır olmayan ve geçerli dilimin yürütülmesini engelleyen yukarı akış dilimlerini **Hazır olmayan yukarı akış dilimleri** listesinde görebilirsiniz.
11. **VERİ DİLİMİ** dikey penceresinde, alttaki listede tüm etkinlik çalıştırmalarını görmelisiniz. **Etkinlik çalışma ayrıntıları** dikey penceresini görmek için bir **etkinlik çalışması**’na tıklayın. 

    ![Etkinlik Çalışma Ayrıntıları](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. **ADFTutorialDataFactory** giriş dikey penceresine dönene kadar tüm dikey pencereleri kapatmak için **X** işaretine tıklayın.
14. (isteğe bağlı) **ADFTutorialDataFactory** için girdi sayfasındaki **İşlem hatları**’na, **İşlem hatları** dikey penceresinde **ADFTutorialPipeline**’a tıklayın, girdi tablolarında (**Tüketilen**) çıktı tablolarında (**Üretilen**) ayrıntılarına gidin.
15. **SQL Server Management Studio**’yu başlatın, Azure SQL Veritabanı’na bağlanın ve veritabanındaki **emp** tablosuna satırların eklenmiş olduğunu doğrulayın.

    ![sql sorgu sonuçları](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### İzleme ve Yönetme Uygulamasını kullanarak işlem hattını izleme
İşlem hatlarınızı izlemek için İzleme ve Yönetme uygulamasını da kullanabilirsiniz. Bu uygulamanın kullanımına ilişkin ayrıntılı bilgi için bkz. [İzleme ve Yönetme Uygulamasını kullanarak Azure Data Factory işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md).

1. Data factory’nin giriş sayfasındaki **İzleme ve Yönetme** kutucuğuna tıklayın.

    ![İzleme ve Yönetme kutucuğu](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. **İzleme ve Yönetme uygulaması**’nı görmeniz gerekir. **Başlangıç saati** ve **Bitiş saati**’ni işlem hattınızın başlangıç (2016-07-12) ve bitiş saatlerini (2016-07-13) içerecek şekilde değiştirin ve **Uygula**’ya tıklayın. 

    ![İzleme ve Yönetme Uygulaması](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Ayrıntılarını görmek için **Etkinlik Pencereleri** listesinden bir etkinlik penceresi seçin. 
    ![Etkinlik penceresi ayrıntıları](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## Özet 
Bu öğreticide Azure blob’undan Azure SQL veritabanına veri kopyalamak üzere Azure data factory oluşturdunuz. Data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturmak için Azure Portal’ı kullandınız. Bu öğreticide gerçekleştirilen üst düzey adımları şunlardır:  

1.  Oluşturulan Azure **data factory**.
2.  Oluşturulan **bağlı hizmetler**:
    1. Girdi verilerini tutan Azure Storage hesabınıza bağlamak için **Azure Storage** bağlı hizmeti.    
    2. Çıktı verilerini tutan Azure SQL veritabanınıza bağlamak için **Azure SQL** bağlı hizmeti. 
3.  İşlem hatları için girdi verilerini ve çıktı verilerini açıklayan oluşturulan **veri kümeleri**.
4.  Kaynak olarak **BlobSource**’u, havuz olarak da **SqlSink**’i kapsayan **Kopyalama Etkinliği**’ne sahip oluşturulan **işlem hattı**.  


## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) | Bu makalede, öğreticide kullandığınız Kopyalama Etkinliği hakkında ayrıntılı bilgi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makale, Azure Data Factory’deki işlem hatlarını ve veri kümelerini anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 





<!--HONumber=Sep16_HO4-->


