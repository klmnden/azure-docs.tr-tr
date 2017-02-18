---
title: "Öğretici: REST API kullanarak Kopyalama Etkinlikli bir işlem hattı oluşturma | Microsoft Belgeleri"
description: "Bu öğreticide, REST API kullanarak Kopyalama Etkinlikli bir Azure Data Factory işlem hattı oluşturursunuz."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/17/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 4b29fd1c188c76a7c65c4dcff02dc9efdf3ebaee
ms.openlocfilehash: c5049cbe98dbb04deae4a2b9dc098938aa65495a


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-rest-api"></a>Öğretici: REST API kullanarak Kopyalama Etkinlikli işlem hattı oluşturma
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

Bu öğretici, REST API kullanarak bir Azure veri fabrikası oluşturmayı ve izlemeyi gösterir. Veri fabrikasındaki işlem hattı, Azure Blob Depolama’dan Azure SQL veritabanı’na veri kopyalamak için bir Kopyalama Etkinliği kullanır.

> [!NOTE]
> Bu makale, Data Factory REST API’sinin tamamını kapsamaz. Data Factory cmdlet’leri hakkında kapsamlı belgeler için bkz. [Data Factory REST API Başvurusu](/rest/api/datafactory/).
> 
> Bu öğreticideki veri işlem hattı, bir kaynak veri deposundaki verileri hedef veri deposuna kopyalar. Çıkış verileri üretmek için giriş verilerini dönüştürmez. Azure Data Factory kullanarak verileri dönüştürme hakkında bir öğretici için bkz. [Öğretici: Hadoop kümesi kullanarak verileri dönüştürmek için işlem hattı oluşturma](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Ön koşullar
* [Öğreticiye Genel Bakış](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) bölümünü inceleyin ve **ön koşul** adımlarını tamamlayın.
* [Curl](https://curl.haxx.se/dlwiz/) aracını makinenize yükleyin. Bir veri fabrikası oluşturmak için Curl aracını REST komutlarıyla kullanırsınız. 
* Aşağıdakileri yapmak için [bu makaledeki](../azure-resource-manager/resource-group-create-service-principal-portal.md) yönergeleri izleyin: 
  1. Azure Active Directory’de **ADFCopyTutorialApp** adlı bir Web uygulaması oluşturun.
  2. **İstemci kimliği** ve **gizli anahtarı** alın. 
  3. **İstemci kimliğini** alın. 
  4. **ADFCopyTutorialApp** uygulamasını **Data Factory Katılımcısı** rolüne atayın.  
* [Azure PowerShell](/powershell/azureps-cmdlets-docs)'i yükleyin.  
* **PowerShell**’i başlatın ve aşağıdaki komutu çalıştırın. Bu öğreticide sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız komutları yeniden çalıştırmanız gerekir.
  
  1. Aşağıdaki komutu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin.
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. Bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın.

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **&lt;NameOfAzureSubscription**&gt; değerini Azure aboneliğinizin adıyla değiştirin. 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. PowerShell’de aşağıdaki komutu çalıştırarak **ADFTutorialResourceGroup** adlı bir Azure kaynak grubu oluşturun.  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      Kaynak grubu zaten varsa bunun güncelleştirileceğini mi (Y) yoksa (N) olarak tutulacağını mı belirtirsiniz. 
     
      Bu öğreticideki adımlardan bazıları ADFTutorialResourceGroup adlı kaynak grubunu kullandığınızı varsayar. Farklı bir kaynak grubu kullanıyorsanız, bu öğreticide kullanılan ADFTutorialResourceGroup yerine kaynak grubunuzun adını kullanmanız gerekir.

## <a name="create-json-definitions"></a>JSON tanımları oluşturma
Curl.exe’nin bulunduğu klasörde aşağıdaki JSON dosyalarını oluşturun. 

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Adın genel olarak benzersiz olması gerektiğinden, ADFCopyTutorialDF’yi benzersiz bir ada dönüştürmek için önüne/sonuna bir şeyler eklemek isteyebilirsiniz. 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> **accountname** ve **accountkey** sözcüklerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](../storage/storage-create-storage-account.md#manage-your-storage-access-keys).
> 
> 

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.json
> [!IMPORTANT]
> **servername**, **databasename**, **username** ve **password** sözcüklerini Azure SQL sunucunuzun adı, SQL veritabanınızın adı, kullanıcı hesabınız ve hesap parolanızla değiştirin.  
> 
> 

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
  "name": "AzureBlobInput",
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
```

JSON tanımı, işlem hattındaki bir etkinliğin girdi verilerini temsil eden **AzureBlobInput** adlı veri kümesini tanımlar. Buna ek olarak, girdi verilerinin **adftutorial** adlı blob kapsayıcısındaki **emp.txt** dosyasında bulunduğunu belirtir. 

 Aşağıdaki noktalara dikkat edin: 

* veri kümesi **türü** **AzureBlob** olarak ayarlanır.
* **linkedServiceName** **AzureStorageLinkedService** olarak ayarlanır. 
* **folderPath** değeri **adftutorial** kapsayıcısı olarak, **fileName** ise **emp.txt** olarak ayarlanmıştır.  
* biçim **türü** **TextFormat** olarak ayarlanır
* Metin dosyasında virgül karakteriyle (**columnDelimiter**) ayrılmış, **FirstName** ve **LastName** adlı iki alan vardır    
* **Availability** **hourly** olarak ayarlanmıştır (sıklık saat olarak, aralıksa 1 olarak ayarlanmıştır). Bu nedenle, Data Factory saatte bir kere belirtilen blob kapsayıcısının (**adftutorial**) kök klasöründe girdi verilerini arar. 

Girdi veri kümesi için bir **fileName** belirtmezseniz, girdi klasörüne (**folderPath**) ait tüm dosyalar/blob’lar girdi olarak kabul edilir. JSON’da bir fileName belirtirseniz yalnızca belirtilen dosya/blob girdi olarak kabul edilir.

**Çıktı tablosu** için bir **fileName** belirtmezseniz **folderPath**’de oluşturulan dosyalar şu biçimde adlandırılır: Data.&lt;Guid&gt;.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

**folderPath** ve **fileName** öğelerini dinamik olarak **SliceStart** zamanı temelinde ayarlamak için **partitionedBy** özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2014-10-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2014/10/20, fileName de 08.csv olarak ayarlanır. 

```JSON
  "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy": 
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
],
```

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
  "name": "AzureSqlOutput",
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
```

JSON tanımı, işlem hattındaki bir etkinliğin çıktı verilerini temsil eden **AzureSqlOutput** adlı veri kümesini tanımlar. Buna ek olarak, sonuçların AzureSqlLinkedService ile temsil edilen veritabanındaki **emp** tablosunda depolandığını belirtir. **Availability** bölümü, çıktı veri kümesinin saatlik (sıklık: saat aralık: 1) tabanda oluşturulduğunu belirtir.

Aşağıdaki noktalara dikkat edin: 

* veri kümesi **türü** **AzureSQLTable** olarak ayarlanır.
* **linkedServiceName** **AzureSqlLinkedService** olarak ayarlanır.
* **tablename** **emp** olarak ayarlanır.
* Veritabanındaki emp tablosunda üç sütun vardır: **ID**, **FirstName** ve **LastName**. ID bir kimlik sütunu olduğundan, burada yalnızca **FirstName** ve **LastName** değerlerini belirtmeniz gerekir.
* **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmeti Azure SQL veritabanındaki **emp** tablosunda her saat bir çıktı veri dilimi oluşturur.

### <a name="pipelinejson"></a>pipeline.json

```JSON
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
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
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
    "start": "2016-08-12T00:00:00Z",
    "end": "2016-08-13T00:00:00Z"
  }
}
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde, **türü** **CopyActivity** olarak ayarlanmış yalnızca bir etkinlik vardır.
* Etkinlik girdisi **AzureBlobInput** olarak, etkinlik çıktısıysa **AzureSqlOutput** olarak ayarlanmıştır.
* **dönüştürme** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.

**start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin. Tarih saatin yalnızca tarih bölümünü belirtip saat bölümünü atlayabilirsiniz. Örneğin, "2015-02-03", "2015-02-03T00:00:00Z" ile eşdeğerdir

Başlangıç ve bitiş tarih saatleri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2014-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız. 

**end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın.

Örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.

> [!NOTE]
> Önceki örnekte kullanılan JSON özellikleri hakkında ayrıntılı bilgi için bkz. [İşlem Hattı Anatomisi](data-factory-create-pipelines.md).
> 
> 

## <a name="set-global-variables"></a>Genel değişkenleri ayarlama
Azure PowerShell’de değerleri kendi değerlerinizle değiştirdikten sonra aşağıdaki komutları çalıştırın:

> [!IMPORTANT]
> İstemci kimliği, istemci parolası, kiracı kimliği ve abonelik kimliğini edinme konusunda yönergeler için [Önkoşullar](#prerequisites) bölümüne bakın.   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a>AAD ile kimlik doğrulama
Azure Active Directory (AAD) ile kimlik doğrulamak için aşağıdaki komutu çalıştırın. 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, **ADFCopyTutorialDF** adlı bir Azure Data Factory oluşturacaksınız. Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, bir kaynaktan hedef veri depolama alanına veri kopyalamak için bir Kopyalama Etkinliği. Girdi verilerini ürün çıktı verilerine dönüştürmek için Hive betiği çalıştırmak üzere bir HDInsight Hive etkinliği. Veri fabrikasını oluşturmak için aşağıdaki komutu çalıştırın: 

1. Komutu **cmd** adlı değişkene atayın. 
   
    Burada belirttiğiniz veri fabrikasının (ADFCopyTutorialDF) adının **datafactory.json**’da belirtilen adla eşleştiğini doğrulayın. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF?api-version=2015-10-01};
    ```
2. **Invoke-Command** komutunu kullanarak komutu çalıştırın.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Sonuçlara bakın. Veri fabrikası başarıyla oluşturulduysa, **results** bölümünde veri fabrikasının JSON’unu görürsünüz; aksi takdirde bir hata iletisi görürsünüz.  
   
    ```
    Write-Host $results
    ```

Aşağıdaki noktalara dikkat edin:

* Azure Data Factory adı küresel olarak benzersiz olmalıdır. Sonuçlarda **Veri fabrikası adı “ADFCopyTutorialDF” kullanılamıyor** hatasını görürseniz aşağıdaki adımları uygulayın:  
  
  1. **datafactory.json** dosyasında adı değiştirin (örneğin, adınızADFCopyTutorialDF).
  2. **$cmd** değişkenine bir değerin atandığı ilk komutta, ADFCopyTutorialDF’yi yeni adla değiştirip komutu çalıştırın. 
  3. Veri fabrikasını oluşturmak ve işlemin sonuçlarını yazdırmak üzere REST API’yi çağırmak için sonraki iki komutu çalıştırın. 
     
     Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
* Data Factory örnekleri oluşturmak için, Azure aboneliğinde katılımcı/yönetici rolünüz olmalıdır
* Veri fabrikasının adı gelecekte bir DNS adı olarak kaydedilmiş ve herkese görünür hale gelmiş olabilir.
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

İşlem hattı oluşturmadan önce, öncelikle birkaç Data Factory varlığı oluşturmanız gerekir. İlk olarak kaynak ve hedef veri depolarını kendi veri deponuza bağlamak için bağlı hizmetler oluşturun. Daha sonra, bağlı veri depolarındaki verileri temsil eden girdi ve çıktı veri kümeleri tanımlayın. Son olarak bu veri kümelerini kullanan bir etkinlik ile işlem hattını oluşturun.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Veri deposu, Data Factory işlem hattı için girdi verilerini içeren veya çıktı verilerini depolayan bir Azure Depolama, Azure SQL Veritabanı veya şirket içi SQL Server veritabanı olabilir. İşlem hizmeti, girdi verilerini işleyen, çıktı verilerini de oluşturan bir hizmettir. 

Bu adımda iki bağlı hizmet oluşturursunuz: **AzureStorageLinkedService** ve **AzureSqlLinkedService**. AzureStorageLinkedService bağlı hizmeti bir Azure Depolama Hesabını, AzureSqlLinkedService ise bir Azure SQL veritabanını **ADFCopyTutorialDF** veri fabrikasına bağlar. Bu öğreticinin ilerleyen bölümlerinde, AzureStorageLinkedService’teki bir blob kapsayıcısından AzureSqlLinkedService’teki bir SQL tablosuna veri kopyalayan bir işlem hattı oluşturacaksınız.

### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure Depolama hesabınızı veri fabrikanıza bağlarsınız. Bu öğreticide girdi verilerini depolamak için Azure Depolama hesabını kullanırsınız. 

1. Komutu **cmd** adlı değişkene atayın. 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. **Invoke-Command** komutunu kullanarak komutu çalıştırın.
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Sonuçlara bakın. Bağlı hizmet başarıyla oluşturulduysa, **results** bölümünde bağlı hizmetin JSON’unu görürsünüz; aksi takdirde bir hata iletisi görürsünüz.

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a>Azure SQL bağlı hizmeti oluşturma
Bu adımda, Azure SQL veritabanınızı veri fabrikanıza bağlarsınız. Bu öğreticide çıktı verilerini depolamak için aynı Azure SQL veritabanını kullanırsınız.

1. Komutu **cmd** adlı değişkene atayın. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. **Invoke-Command** komutunu kullanarak komutu çalıştırın.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Sonuçlara bakın. Bağlı hizmet başarıyla oluşturulduysa, **results** bölümünde bağlı hizmetin JSON’unu görürsünüz; aksi takdirde bir hata iletisi görürsünüz.
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Önceki adımda, bir Azure Depolama hesabını ve Azure SQL veritabanını **ADFCopyTutorialDF** veri fabrikasına bağlamak için **AzureStorageLinkedService** ve **AzureSqlLinkedService** bağlı hizmetlerini oluşturdunuz. Bu adımda, bir sonraki adımda oluşturacağınız işlem hattının Kopyalama Etkinliği için girdi ve çıktı verilerini temsil eden veri kümeleri oluşturursunuz. 

Bu öğreticideki girdi veri kümesi, AzureStorageLinkedService hizmetinin işaret ettiği Azure Depolama’daki bir blob kapsayıcısını ifade eder. Çıktı veri kümesi, AzureSqlLinkedService hizmetinin işaret ettiği Azure SQL veritabanındaki bir SQL tablosunu ifade eder.  

### <a name="prepare-azure-blob-storage-and-azure-sql-database-for-the-tutorial"></a>Azure Blob Storage ve Azure SQL Database’i öğretici için hazırlama
Bu öğreticide kullanılacak Azure blob depolama ve Azure SQL veritabanını hazırlamak için aşağıdaki adımları uygulayın. 

* **AzureStorageLinkedService** tarafından işaret edilen Azure blob depolamada **adftutorial** adlı bir blob kapsayıcı oluşturun. 
* **emp.txt** adıyla bir metin dosyasını oluşturup, bir blob olarak **adftutorial** kapsayıcısına yükleyin. 
* **AzureSqlLinkedService** tarafından belirtilen Azure SQL Database’de **emp** adlı bir tablo oluşturun.

1. Not Defteri’ni başlatın, aşağıdaki metni yapıştırın ve **emp.txt** olarak sabit diskinizdeki **C:\ADFGetStartedPSH** klasörüne kaydedin. 

    ```   
    John, Doe
    Jane, Doe
    ```
2. [Azure Storage Gezgini](https://azurestorageexplorer.codeplex.com/) gibi araçları **adftutorial** kapsayıcısı oluşturmak ve **emp.txt** dosyasını kapsayıcıya yüklemek için kullanın.
   
    ![Azure Storage Gezgini](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
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

    Bilgisayarınızda SQL Server 2014 yüklüyse: Azure SQL sunucunuza bağlanmak ve SQL betiğini çalıştırmak için [2. Adım: SQL Server Management Studio kullanarak, Yönetilen Azure SQL Veritabanı'nın SQL Veritabanı'na Bağlanma][sql-management-studio] makalesindeki yönergeleri uygulayın.

    İstemcinizin Azure SQL sunucusuna erişim izni yoksa, makinenizden (IP adresi) erişim izni vermek için Azure SQL sunucunuzun güvenlik duvarını yapılandırmanız gerekir. Azure SQL sunucunuzun güvenlik duvarını yapılandırmaya yönelik adımlar için [bu makaleye](../sql-database/sql-database-configure-firewall-settings.md) bakın.

### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
Bu adımda, **AzureStorageLinkedService** bağlı hizmetiyle temsil edilen Azure Depolama’daki bir blob kapsayıcısını işaret eden **AzureBlobInput** adlı bir veri kümesi oluşturacaksınız. Bu blob kapsayıcısında (**adftutorial**) şu dosyaya ait girdi verileri vardır: **emp.txt**. 

1. Komutu **cmd** adlı değişkene atayın. 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. **Invoke-Command** komutunu kullanarak komutu çalıştırın.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Sonuçlara bakın. Veri kümesi başarıyla oluşturulduysa, **results** bölümünde veri kümesinin JSON’unu görürsünüz; aksi takdirde bir hata iletisi görürsünüz.
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma
Bu adımda **AzureSqlOutput** adlı bir çıktı tablosu oluşturursunuz. Bu veri kümesi, **AzureSqlLinkedService** ile temsil edilen Azure SQL veritabanında bir SQL tablosunu (**emp**) işaret eder. İşlem hattı verileri girdi blob’undan **emp** tablosuna kopyalar. 

1. Komutu **cmd** adlı değişkene atayın.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. **Invoke-Command** komutunu kullanarak komutu çalıştırın.
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Sonuçlara bakın. Veri kümesi başarıyla oluşturulduysa, **results** bölümünde veri kümesinin JSON’unu görürsünüz; aksi takdirde bir hata iletisi görürsünüz.
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a>İşlem hattı oluşturma
Bu adımda, girdi olarak **AzureBlobInput**, çıktı olaraksa **AzureSqlOutput** kullanan bir **Kopyalama Etkinliği**’ne sahip bir işlem hattı oluşturursunuz.

1. Komutu **cmd** adlı değişkene atayın.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. **Invoke-Command** komutunu kullanarak komutu çalıştırın.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Sonuçlara bakın. Veri kümesi başarıyla oluşturulduysa, **results** bölümünde veri kümesinin JSON’unu görürsünüz; aksi takdirde bir hata iletisi görürsünüz.  

    ```PowerShell   
    Write-Host $results
    ```

**Tebrikler!** Azure Blob Depolama’dan Azure SQL veritabanına veri kopyalayan bir işlem hattına sahip bir Azure veri fabrikasını başarıyla oluşturdunuz.

## <a name="monitor-pipeline"></a>İşlem hattını izleme
Bu adımda, Data Factory REST API’sini kullanarak işlem hattı tarafından üretilmekte olan dilimleri izlersiniz.

```PowerShell
$ds ="AzureSqlOutput"
```

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

**Ready** durumunda veya **Failed** durumunda bir dilim görene kadar bu komutları çalıştırın. Dilim Ready durumundayken Azure SQL veritabanınızdaki **emp** tablosunda çıktı verileri olup olmadığını denetleyin. 

Her dilim için kaynak dosyasından Azure SQL veritabanındaki emp tablosuna iki satır veri kopyalanır. Bu nedenle, tüm dilimler başarıyla işlendiğinde (Ready durumunda) emp tablosunda 24 yeni kayıt görürsünüz. 

## <a name="summary"></a>Özet
Bu öğreticide bir Azure blob’undan Azure SQL veritabanına veri kopyalamak için REST API kullanarak bir Azure veri fabrikası oluşturdunuz. Bu öğreticide gerçekleştirilen üst düzey adımları şunlardır:  

1. Azure **data factory** oluşturuldu.
2. Oluşturulan **bağlı hizmetler**:
   1. Girdi verilerinizi barındıran Azure Depolama hesabınızı bağlamak için bir Azure Depolama bağlı hizmeti.     
   2. Çıktı verilerinizi barındıran Azure SQL veritabanınızı bağlamak için bir Azure SQL bağlı hizmeti. 
3. İşlem hatları için girdi ve çıktı verilerini açıklayan **veri kümeleri** oluşturuldu.
4. Kaynak olarak BlobSource’u, havuz olarak SqlSink’i kapsayan bir Kopyalama Etkinliği’ne sahip bir **işlem hattı** oluşturuldu. 

## <a name="see-also"></a>Ayrıca Bkz.
| Konu | Açıklama |
|:--- |:--- |
| [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) |Bu makalede, öğreticide kullandığınız Kopyalama Etkinliği hakkında ayrıntılı bilgi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) |Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) |Bu makale, Azure Data Factory’de işlem hatlarının ve etkinliklerini anlamanıza ve senaryonuz ya da işletmeniz için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) |Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur. |
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) |Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. |

[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: /powershell/azureps-cmdlets-docs
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md



<!--HONumber=Feb17_HO1-->


