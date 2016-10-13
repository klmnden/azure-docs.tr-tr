<properties 
    pageTitle="Öğretici: Azure PowerShell kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma | Microsoft Azure" 
    description="Bu öğreticide, Azure PowerShell kullanarak Kopyalama Etkinliği ile bir Azure Data Factory işlem hattı oluşturursunuz." 
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


# Öğretici: Azure PowerShell kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma
> [AZURE.SELECTOR]
- [Genel bakış ve ön koşullar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
- [Azure portalı](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Bu öğreticide, Azure PowerShell cmdlet’leri kullanarak bir Azure data factory oluşturur ve izlersiniz. Bu öğreticide oluşturduğunuz data factory’de işlem hattı verileri Azure blob’tan Azure SQL veritabanına kopyalamak için Kopyalama Etkinliği’ni kullanır.

Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.   

> [AZURE.IMPORTANT] 
> [Öğreticiye Genel Bakış](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) makalesini inceleyin ve bu öğreticiyi uygulamadan önce **ön koşul** adımlarını tamamlayın.
>   
> Bu makalede, tüm Data Factory cmdlet'lerini kapsamaz. Data Factory cmdlet’leri hakkında kapsamlı bilgi için bkz. [Data Factory Cmdlet Başvurusu](https://msdn.microsoft.com/library/dn820234.aspx).
  

##Önkoşullar
Öğreticiye Genel Bakış konusunda listelenen önkoşullar dışında **Azure PowerShell** yüklemeniz gerekir. Bilgisayarınıza Azure PowerShell’i yüklemek için [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md) makalesindeki yönergeleri izleyin.

##Bu öğreticide
Aşağıdaki tabloda, öğreticinin ve açıklamalarının bir parçası olarak gerçekleştireceğiniz adımlar listelenmektedir. 

Adım | Açıklama
-----| -----------
[Azure Data Factory oluşturma](#create-data-factory) | Bu adımda, **ADFTutorialDataFactoryPSH** adlı bir Azure data factory oluşturursunuz. 
[Bağlı hizmetler oluşturma](#create-linked-services) | Bu adımda, iki bağlı hizmet oluşturursunuz: **StorageLinkedService** ve **AzureSqlLinkedService**. StorageLinkedService Azure depolamayı, AzureSqlLinkedService de Azure SQL veritabanını ADFTutorialDataFactoryPSH konumuna bağlar.
[Girdi ve çıktı veri kümeleri oluşturma](#create-datasets) | Bu adımda, iki veri kümesi tanımlarsınız (**EmpTableFromBlob** ve **EmpSQLTable**). Bu veri kümeleri sonraki adımda oluşturduğunuz ADFTutorialPipeline içindeki **Kopya Etkinliği** için girdi ve çıktı tabloları olarak kullanılır.
[İşlem hattını oluşturma ve çalıştırma](#create-pipeline) | Bu adımda, **ADFTutorialDataFactoryPSH** adlı data factory’nin **ADFTutorialPipeline** adlı işlem hattını oluşturursunuz. İşlem hattında, verileri Azure blob’tan çıktı Azure veritabanı tablosuna kopyalayan **Kopyalama Etkinliği** vardır.
[Veri kümelerini ve işlem hattını izleme](#monitor-pipeline) | Bu adımda, burada Azure PowerShell kullanan veri kümelerini ve işlem hattını izlersiniz.

## Veri fabrikası oluşturma
Bu adımda **ADFTutorialDataFactoryPSH** adlı bir Azure data factory oluşturmak için Azure PowerShell’i kullanırsınız.

1. **PowerShell**’i başlatın. Bu öğreticide sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız komutları yeniden çalıştırmanız gerekir.
    1. Aşağıdaki komutu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin:
    
            Login-AzureRmAccount   
    2. Bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

            Get-AzureRmSubscription 
    3. Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **&lt;NameOfAzureSubscription**&gt; değerini Azure aboneliğinizin adıyla değiştirin:

            Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

3. Aşağıdaki komutu kullanarak **ADFTutorialResourceGroup** adlı bir Azure kaynak grubu oluşturun:
   
        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Bu öğreticideki adımlardan bazıları **ADFTutorialResourceGroup** adlı kaynak grubunu kullandığınızı varsayar. Farklı bir kaynak grubu kullanıyorsanız, bu öğreticide ADFTutorialResourceGroup yerine onu kullanmanız gerekir. 
4. **ADFTutorialDataFactoryPSH** adlı bir data factory oluşturmak için **New-AzureRmDataFactory** cmdlet’ini çalıştırın:  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"

    
Aşağıdaki noktalara dikkat edin:
 
- Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hatayı alırsanız adı değiştirin (örneğin adınızADFTutorialDataFactory). Bu öğreticide adımları uygularken ADFTutorialFactoryPSH yerine bu adı kullanın. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
    
        Data factory name “ADFTutorialDataFactoryPSH” is not available
- Data Factory örnekleri oluşturmak için, Azure aboneliğinde katılımcı/yönetici rolünüz olmalıdır
- Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.
- Şu hatayı alırsanız: "**Abonelik, Microsoft.DataFactory ad alanını kullanacak şekilde kaydedilmemiş**", aşağıdakilerden birini yapın ve yeniden yayımlamayı deneyin: 

    - Azure PowerShell’de Data Factory sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Data Factory sağlayıcısının kayıtlı olduğunu onaylamak için aşağıdaki komutu çalıştırın: 
    
            Get-AzureRmResourceProvider
    - Azure aboneliğini kullanarak [Azure portalında](https://portal.azure.com) oturum açın ve Data Factory dikey penceresine gidin (ya da) Azure portalında bir data factory oluşturun. Bu eylem sağlayıcıyı sizin için otomatik olarak kaydeder.

## Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Veri deposu, Data Factory işlem hattı için girdi verilerini içeren veya çıktı verilerini depolayan bir Azure Depolama, Azure SQL Veritabanı veya şirket içi SQL Server veritabanı olabilir. İşlem hizmeti, girdi verilerini işleyen, çıktı verilerini de oluşturan bir hizmettir. 

Bu adımda, iki bağlı hizmet oluşturursunuz: **StorageLinkedService** ve **AzureSqlLinkedService**. StorageLinkedService bağlı hizmeti Azure Storage Hesabını, AzureSqlLinkedService de Azure SQL veritabanını data factory: **ADFTutorialDataFactoryPSH** konumuna bağlar. Daha sonra bu öğreticide, verileri StorageLinkedService’teki bir blob kapsayıcısından AzureSqlLinkedService’teki bir SQL tablosuna kopyalayan bir işlem hattı oluşturursunuz.

### Azure depolama hesabı için bağlı hizmet oluşturma
1.  **C:\ADFGetStartedPSH** konumunda aşağıdaki içeriğe sahip **StorageLinkedService.json** adlı bir JSON dosyası oluşturun: Henüz yoksa ADFGetStartedPSH klasörünü oluşturun.

            {
                "name": "StorageLinkedService",
                "properties": {
                    "type": "AzureStorage",
                    "typeProperties": {
                        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                    }
                }
            }

    **accountname** ve **accountkey** sözcüklerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin.
2.  **Azure PowerShell**’de **ADFGetStartedPSH** klasörüne geçin. 
3.  Bağlı hizmet oluşturmak için **New-AzureRmDataFactoryLinkedService** cmdlet’ini kullanabilirsiniz. Bu öğreticide kullandığınız bu cmdlet ve diğer Data Factory cmdlet’lerini **ResourceGroupName** ve **DataFactoryName** parametreleri için değerleri geçirmeniz gerekir. Alternatif olarak, DataFactory nesnesini almak ve cmdlet’i her çalıştırdığınızda ResourceGroupName ve DataFactoryName yazmadan nesneyi geçirmek için **Get-AzureRmDataFactory** kullanabilirsiniz. **Get-AzureRmDataFactory** cmdlet’inin çıktısını **$df** değişkenine atamak için aşağıdaki komutu çalıştırın: 

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.  Şimdi de, **StorageLinkedService** bağlı hizmetini oluşturmak için **New-AzureRmDataFactoryLinkedService** cmdlet’ini kullanın. 

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    **Get-AzureRmDataFactory** cmdlet’ini çalıştırmadıysanız ve çıktıyı **$df** değişkenine atamadıysanız, ResourceGroupName ve DataFactoryName parametreleri için değerleri aşağıdaki gibi belirtmeniz gerekir.   
        
        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

    Öğreticinin ortasında Azure PowerShell’i kapatırsanız, öğreticiyi tamamlamak için Azure PowerShell’i sonraki başlatışınızda Get-AzureRmDataFactory cmdlet’ini çalıştırmanız gerekir.

### Azure SQL Database için bağlı hizmet oluşturma
1.  Aşağıdaki içerikle AzureSqlLinkedService.json adlı bir JSON dosyası oluşturun:

            {
                "name": "AzureSqlLinkedService",
                "properties": {
                    "type": "AzureSqlDatabase",
                    "typeProperties": {
                        "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                    }
                }
            }

    Burada, **servername**, **databasename**, **username@servername** ve **password** sözcüklerini Azure SQL sunucunuzun, veritabanınızın, kullanıcı hesabınızın adlarıyla ve parolasıyla değiştirin.

2.  Bağlı hizmet oluşturmak için şu komutu çalıştırın: 
    
        New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json

    **Azure hizmetlerine erişime izin ver** ayarının Azure SQL sunucusunda AÇIK olduğunu onaylayın. Doğrulayıp etkinleştirmek için aşağıdaki adımları uygulayın:

    1. Soldaki **GÖZAT** hub’ına ve **SQL sunucuları**’na tıklayın.
    2. Sunucunuzu seçip SQL SERVER dikey penceresinde **AYARLAR**’a tıklayın.
    3. **AYARLAR** dikey penceresinde **Güvenlik Duvarı**’na tıklayın.
    4. **Güvenlik Duvarı ayarları** dikey penceresinde, **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.
    5. Soldaki **ETKİN** hub’ına tıklayarak açtığınız **Data Factory** dikey penceresine geçin.
    

## Veri kümeleri oluşturma

Önceki adımda, Azure Storage hesabını ve Azure SQL veritabanını data factory’ye bağlamak için **StorageLinkedService** ve **AzureSqlLinkedService** bağlı hizmetlerini oluşturdunuz; burada söz edilen data factory adı: **ADFTutorialDataFactoryPSH**. Bu adımda, bir sonraki adımda oluşturacağınız işlem hattının Kopyalama Etkinliği için girdi ve çıktı verilerini temsil eden veri kümeleri oluşturursunuz. 

Tablo dikdörtgen bir veri kümesidir ve bu sırada desteklenen tek veri kümesi türüdür. Bu öğreticideki girdi tablosu, Azure depolamadaki StorageLinkedService noktalarının işaret ettiği bir blob kapsayıcısını ifade eder. Çıktı tablosu, AzureSqlLinkedService hizmetinin işaret ettiği Azure SQL veritabanındaki bir SQL tablosunu ifade eder.  

### Azure Blob Storage ve Azure SQL Database’i öğretici için hazırlama
Öğreticide [Blob Depolamadan SQL Veritabanına veri kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) makalesinden ilerlediyseniz bu adımı atlayın. 

Bu öğreticide kullanılacak Azure blob depolama ve Azure SQL veritabanını hazırlamak için aşağıdaki adımları uygulayın. 
 
* **StorageLinkedService** tarafından belirtilen Azure blob depolamada **adftutorial** adlı bir blob kapsayıcı oluşturun. 
* **emp.txt** adıyla bir metin dosyasını oluşturup, bir blob olarak **adftutorial** kapsayıcısına yükleyin. 
* **AzureSqlLinkedService** tarafından belirtilen Azure SQL Database’de **emp** adlı bir tablo oluşturun.


1. Not Defteri’ni başlatın, aşağıdaki metni yapıştırın ve **emp.txt** olarak sabit diskinizdeki **C:\ADFGetStartedPSH** klasörüne kaydedin. 

        John, Doe
        Jane, Doe
                
2. [Azure Storage Gezgini](https://azurestorageexplorer.codeplex.com/) gibi araçları **adftutorial** kapsayıcısı oluşturmak ve **emp.txt** dosyasını kapsayıcıya yüklemek için kullanın.

    ![Azure Storage Gezgini](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Azure SQL Database’inizde **emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın.  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    Bilgisayarınızda SQL Server 2014 yüklüyse: Azure SQL sunucunuza bağlanmak ve SQL betiğini çalıştırmak için [2. Adım: SQL Server Management Studio kullanarak, Yönetilen Azure SQL Veritabanı’nın SQL Veritabanı’na Bağlanma](../sql-database/sql-database-manage-azure-ssms.md) makalesindeki yönergeleri uygulayın.

    İstemcinizin Azure SQL sunucusuna erişim izni yoksa, makinenizden (IP adresi) erişim izni vermek için Azure SQL sunucunuzun güvenlik duvarını yapılandırmanız gerekir. Azure SQL sunucunuzun güvenlik duvarını yapılandırmaya yönelik adımlar için [bu makaleye](../sql-database/sql-database-configure-firewall-settings.md) bakın.
        
### Girdi veri kümesi oluşturma 
Tablo dikdörtgen bir veri kümesidir ve bir şeması vardır. Bu adımda, **StorageLinkedService** bağlı hizmetiyle temsil edilen Azure Depolama’da bir blob kapsayıcısını belirten **EmpBlobTable** adlı bir tablo oluşturursunuz. Bu blob kapsayıcısında (**adftutorial**) şu dosyaya ait girdi verileri vardır: **emp.txt**. 

1.  **C:\ADFGetStartedPSH** klasöründe aşağıdaki içeriğe sahip **EmpBlobTable.json** adlı bir JSON dosyası oluşturun:

            {
              "name": "EmpTableFromBlob",
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
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                  "fileName": "emp.txt",
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
    
    Aşağıdaki noktalara dikkat edin: 
    
    - veri kümesi **türü** **AzureBlob** olarak ayarlanır.
    - **linkedServiceName** **StorageLinkedService** olarak ayarlanır. 
    - **folderPath** **adftutorial** kapsayıcısı olarak ayarlanır. 
    - **fileName** **emp.txt** olarak ayarlanır. Blob adını belirtmezseniz, kapsayıcıdaki tüm blob'lara ait veriler girdi verisi olarak kabul edilir.  
    - biçim **türü** **TextFormat** olarak ayarlanır
    - Metin dosyasında virgül karakteriyle (**columnDelimiter**) ayrılmış, **FirstName** ve **LastName** adlı iki alan vardır 
    - **Availability** **hourly** olarak ayarlanmıştır (**sıklık** **saat** olarak, **aralık** ise **1** olarak ayarlanmıştır). Bu nedenle Data Factory, blob kapsayıcısının (**adftutorial**) kök klasöründe girdi verilerini saatte bir kere arar.

    **Girdi** **tablosu** için bir **fileName** belirtmediyseniz, girdi klasörüne ait (**folderPath**) tüm dosyalar/blob’lar girdi olarak kabul edilir. JSON’da fileName belirtmediyseniz, yalnızca belirtilen dosya/blob girdi olarak kabul edilir. 
 
    **Çıktı tablosu** için bir **fileName** belirtmezseniz, **folderPath**’de oluşturulan dosyaları şu biçimde adlandırılır: Data.<Guid\>gt;.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **folderPath** ve **fileName** öğelerini dinamik olarak **SliceStart** zamanı temelinde ayarlamak için **partitionedBy** özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2016-10-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2016/10/20, fileName de 08.csv olarak ayarlanır. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],

    JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](http://go.microsoft.com/fwlink/?LinkId=516971).

2.  Data Factory veri kümesi oluşturmak için şu komutu çalıştırın:

        New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json

### Çıktı veri kümesi oluşturma
Bu adımda **EmpSQLTable** adlı bir çıktı veri kümesi oluşturursunuz. Bu veri kümesi, **AzureSqlLinkedService** ile temsil edilen Azure SQL veritabanında bir SQL tablosunu (**emp**) işaret eder. İşlem hattı verileri girdi blob’undan **emp** tablosuna kopyalar. 

1.  **C:\ADFGetStartedPSH** klasöründe aşağıdaki içeriğe sahip **EmpSQLTable.json** adlı bir JSON dosyası oluşturun:
        
            {
              "name": "EmpSQLTable",
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
    
    * veri kümesi **türü** **AzureSqlTable** olarak ayarlanır.
    * **linkedServiceName** **AzureSqlLinkedService** olarak ayarlanır.
    * **tablename** **emp** olarak ayarlanır.
    * Veritabanındaki emp tablosunda üç sütun vardır: **ID**, **FirstName** ve **LastName**. ID bir kimlik sütunu olduğundan, burada yalnızca **FirstName** ve **LastName** değerlerini belirtmeniz gerekir.
    * **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmeti Azure SQL veritabanındaki **emp** tablosunda her saat bir çıktı veri dilimi oluşturur.

2.  Data Factory veri kümesi oluşturmak için şu komutu çalıştırın: 
    
        New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json


## İşlem hattı oluşturma
Su adımda, girdi olarak **EmpTableFromBlob**, çıktı olarak da **EmpSQLTable** kullanan bir **Kopyalama Etkinliği**’ne sahip işlem hattı oluşturacaksınız.

1.  **C:\ADFGetStartedPSH** klasöründe aşağıdaki içeriğe sahip **ADFTutorialPipeline.json** adlı bir JSON dosyası oluşturun: 
    
             {
              "name": "ADFTutorialPipeline",
              "properties": {
                "description": "Copy data from a blob to Azure SQL table",
                "activities": [
                  {
                    "name": "CopyFromBlobToSQL",
                    "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                    "type": "Copy",
                    "inputs": [
                      {
                        "name": "EmpTableFromBlob"
                      }
                    ],
                    "outputs": [
                      {
                        "name": "EmpSQLTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "SqlSink"
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
                "start": "2016-08-09T00:00:00Z",
                "end": "2016-08-10T00:00:00Z",
                "isPaused": false
              }
            }

    Aşağıdaki noktalara dikkat edin:

    - Etkinlikler bölümünde, **türü** **Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
    - Etkinlik girdisi **EmpTableFromBlob** olarak, etkinlik çıktısı da **EmpSQLTable** olarak ayarlanmıştır.
    - **dönüştürme** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.

    **start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin. Başlangıç ve bitiş tarih saatleri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2016-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız. 
    
    **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9/9/9999** olarak ayarlayın.
    
    Örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.
    
    JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](http://go.microsoft.com/fwlink/?LinkId=516971).
2.  Data Factory tablosu oluşturmak için şu komutu çalıştırın: 
        
        New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json

**Tebrikler!** Başarılı bir şekilde Azure data factory, bağlı hizmetler, tablolar ve işlem hattı oluşturdunuz, işlem hattını zamanladınız.

## İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure PowerShell kullanırsınız.

1.  **Get-AzureRmDataFactory** komutunu çalıştırın ve çıktıyı $df değişkenine atayın.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
 
2.  İşlem hattının çıktı tablosu olan **EmpSQLTable** tablosunun tüm dilimleri hakkında bilgi almak için **Get-AzureRmDataFactorySlice** komutunu çalıştırın.  

        Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2016-08-09T00:00:00

    **StartDateTime** parametresinin year, month ve date bölümünü geçerli yıl, ay ve tarihle değiştirin. Bu ayar JSON işlem hattının **Başlat** değeriyle eşleşmelidir. 

    24 dilim görmeniz gerekir, geçerli günün saat 12 AM’den başlayıp ertesi günün 12 AM’inde biten her biri bir saati belirten dilimlerdir. 
    
    **Örnek çıktı:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 8/9/2016 12:00:00 AM
        End               : 8/9/2016 1:00:00 AM
        RetryCount        : 0
        Status            : Waiting
        LatencyStatus     :
        LongRetryCount    : 0

3.  **Belirli** bir dilimle ilgili etkinlik çalıştırmalarının ayrıntılarını almak için **Get-AzureRmDataFactoryRun** komutunu çalıştırın. **StartDateTime** parametresinin değerini, çıktıya ait dilimin **Başlat** zamanıyla eşleşecek şekilde değiştirin. **StartDateTime** değeri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. 

        Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2016-08-09T00:00:00

    Aşağıda örnek çıktıya benzer bir çıktı görmeniz gerekir:

        Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : ADFTutorialDataFactoryPSH
        TableName           : EmpSQLTable
        ProcessingStartTime : 8/9/2016 11:03:28 PM
        ProcessingEndTime   : 8/9/2016 11:04:36 PM
        PercentComplete     : 100
        DataSliceStart      : 8/9/2016 10:00:00 PM
        DataSliceEnd        : 8/9/2016 11:00:00 PM
        Status              : Succeeded
        Timestamp           : 8/9/2016 11:03:28 PM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : CopyFromBlobToSQL
        PipelineName        : ADFTutorialPipeline
        Type                : Copy

Data Factory cmdlet’leri hakkında kapsamlı bilgi için bkz. [Data Factory Cmdlet Başvurusu][cmdlet-reference]. 

## Özet
Bu öğreticide Azure blob’undan Azure SQL veritabanına veri kopyalamak üzere Azure data factory oluşturdunuz. Data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturmak için PowerShell’i kullandınız. Bu öğreticide gerçekleştirilen üst düzey adımları şunlardır:  

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



[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: ../powershell-install-configure.md
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md
 


<!--HONumber=Sep16_HO5-->


