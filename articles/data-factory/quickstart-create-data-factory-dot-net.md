---
title: .NET kullanarak Azure veri fabrikası oluşturma | Microsoft Docs
description: Azure Blob depolamadaki bir konumdan başka bir konuma veri kopyalamak için bir Azure veri fabrikası oluşturun.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: jingwang
ms.openlocfilehash: 835e0153039ecc3bb93fb7aa9b5007c205ec503e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793893"
---
# <a name="quickstart-create-a-data-factory-and-pipeline-using-net-sdk"></a>Hızlı Başlangıç: .NET SDK’sını kullanarak veri fabrikası ve işlem hattı oluşturma

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](quickstart-create-data-factory-dot-net.md)

Bu hızlı başlangıç, .NET SDK’sı kullanarak bir Azure veri fabrikası oluşturma işlemini açıklar. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure blob depolama alanındaki bir klasörden başka bir klasöre **kopyalar**. Hakkında bir öğretici için **dönüştürme** Azure Data Factory kullanarak verileri görmek [Öğreticisi: Spark kullanarak verileri dönüştürme](transform-data-using-spark.md). 

> [!NOTE]
> Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="visual-studio"></a>Visual Studio

Bu makaledeki kılavuzda Visual Studio 2017 kullanılır. Visual Studio 2013 veya 2015 de kullanabilirsiniz.

### <a name="azure-net-sdk"></a>Azure .NET SDK’sı

[Azure .NET SDK’sını](https://azure.microsoft.com/downloads/) indirip makinenize yükleyin.

## <a name="create-an-application-in-azure-active-directory"></a>Azure Active Directory’de uygulama oluşturma

Aşağıdaki görevleri gerçekleştirmek için [bu makaledeki](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) bölümlerde yer alan yönergeleri izleyin: 

1. **Azure Active Directory uygulaması oluşturma**. Azure Active Directory'de, bu öğreticide oluşturduğunuz .NET uygulamasını temsil eden bir uygulama oluşturma. Oturum açma URL'si için, makalede gösterildiği gibi bir işlevsiz URL sağlayabilirsiniz (`https://contoso.org/exampleapp`).
2. **Uygulama kimliği** ve **kimlik doğrulaması anahtarı** değerlerini alın ve bu öğreticinin ilerleyen kısmında kullanmak üzere bu değerleri not edin. 
3. **Kiracı kimliği** değerini alın ve bu öğreticinin ilerleyen kısmında kullanmak üzere bu değeri not edin.
4. Uygulamanın abonelikte veri fabrikaları oluşturabilmesi için uygulamayı **Katkıda Bulunan** rolüne atayın.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio 2013/2015/2017'yi kullanarak bir C# .NET konsol uygulaması oluşturun.

1. **Visual Studio**’yu başlatın.
2. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın.
3. Sağ taraftaki proje türleri listesinden **Visual C#** -> **Konsol Uygulaması (.NET Framework)** öğesini seçin. .NET sürüm 4.5.2 veya üzeri gereklidir.
4. Ad için **ADFv2QuickStart** girin.
5. Projeyi oluşturmak için **Tamam**'a tıklayın.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

1. **Araçlar** -> **NuGet Paket Yöneticisi** -> **Paket Yöneticisi Konsolu**’na tıklayın.
2. İçinde **Paket Yöneticisi Konsolu**, paketleri yüklemek için aşağıdaki komutları çalıştırın. Başvurmak [Microsoft.Azure.Management.DataFactory nuget paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactory/) ayrıntılarla.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactory
    Install-Package Microsoft.Azure.Management.ResourceManager
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-data-factory-client"></a>Veri fabrikası istemcisi oluşturma

1. **Program.cs** dosyasını açın ve ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Aşağıdaki kodu, değişkenleri ayarlayan **Main** yöntemine ekleyin. Yer tutucuları kendi değerlerinizle değiştirin. Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

    ```csharp
    // Set variables
    string tenantID = "<your tenant ID>";
    string applicationId = "<your application ID>";
    string authenticationKey = "<your authentication key for the application>";
    string subscriptionId = "<your subscription ID where the data factory resides>";
    string resourceGroup = "<your resource group where the data factory resides>";
    string region = "East US 2";
    string dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>";
    string storageAccount = "<your storage account name to copy data>";
    string storageKey = "<your storage account key>";
    // specify the container and input folder from which all files need to be copied to the output folder. 
    string inputBlobPath = "<the path to existing blob(s) to copy data from, e.g. containername/foldername>";
    //specify the contains and output folder where the files are copied
    string outputBlobPath = "<the blob path to copy data to, e.g. containername/foldername>";

    string storageLinkedServiceName = "AzureStorageLinkedService";  // name of the Azure Storage linked service
    string blobDatasetName = "BlobDataset";             // name of the blob dataset
    string pipelineName = "Adfv2QuickStartPipeline";    // name of the pipeline
    ```

3. Aşağıdaki kodu **DataFactoryManagementClient** sınıfının bir örneğini oluşturan **Main** yöntemine ekleyin. Veri fabrikası, bağlı hizmet, veri kümeleri ve işlem hattı oluşturmak için bu nesneyi kullanırsınız. Bu nesneyi ayrıca işlem hattı ayrıntılarını izlemek için kullanabilirsiniz.

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
    ```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Aşağıdaki kodu **veri fabrikası** oluşturan **Main** yöntemine ekleyin. 

```csharp
// Create a data factory
Console.WriteLine("Creating data factory " + dataFactoryName + "...");
Factory dataFactory = new Factory
{
    Location = region,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
Console.WriteLine(SafeJsonConvert.SerializeObject(dataFactory, client.SerializationSettings));

while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
{
    System.Threading.Thread.Sleep(1000);
}
```

## <a name="create-a-linked-service"></a>Bağlı hizmet oluşturma

Aşağıdaki kodu bir **Azure Depolama bağlı hizmeti** oluşturan **Main** yöntemine ekleyin.

Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturursunuz. Bu Hızlı Başlangıçta yalnızca bir Azure Depolama bağlı hizmetini örnekte "AzureStorageLinkedService" olarak adlandırılmış bir kopyalama kaynağı ve havuz deposu için oluşturmanız gerekir.

```csharp
// Create an Azure Storage linked service
Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");

LinkedServiceResource storageLinkedService = new LinkedServiceResource(
    new AzureStorageLinkedService
    {
        ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
    }
);
client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, storageLinkedService);
Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
```

## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Aşağıdaki kodu bir **Azure blob veri kümesi** oluşturan **Main** yöntemine ekleyin.

Bir kaynaktan havuza kopyalanacak verileri temsil eden bir veri kümesi tanımlayın. Bu örnekte, bu Blob veri kümesi önceki adımda oluşturduğunuz Azure Depolama bağlı hizmetine başvurur. Veri kümesi, değeri veri kümesini kullanan bir etkinlikte tanımlanmış bir parametre alır. Parametre, verilerin bulunduğu/depolandığı yeri işaret eden "folderPath" öğesini oluşturmak için kullanılır.

```csharp
// Create an Azure Blob dataset
Console.WriteLine("Creating dataset " + blobDatasetName + "...");
DatasetResource blobDataset = new DatasetResource(
    new AzureBlobDataset
    {
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        },
        FolderPath = new Expression { Value = "@{dataset().path}" },
        Parameters = new Dictionary<string, ParameterSpecification>
        {
            { "path", new ParameterSpecification { Type = ParameterType.String } }

        }
    }
);
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobDatasetName, blobDataset);
Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
```

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Aşağıdaki kodu **bir kopyalama etkinliği ile işlem hattı** oluşturan **Main** yöntemine ekleyin.

Bu örnekte, işlem hattı bir etkinlik içerir ve giriş blob yolu ve çıkış blob yolu şeklinde iki parametre alır. Bu parametrelerin değerleri, işlem hattı tetiklendiğinde/çalıştırıldığında ayarlanır. Kopyalama etkinliği, önceki adımda girdi ve çıktı olarak oluşturulmuş blob veri kümesini ifade eder. Veri kümesi bir girdi veri kümesi olarak kullanıldığında girdi yolu belirtilir. Ayrıca, veri kümesi bir çıktı veri kümesi olarak kullanıldığında çıktı yolu belirtilir. 

```csharp
// Create a pipeline with a copy activity
Console.WriteLine("Creating pipeline " + pipelineName + "...");
PipelineResource pipeline = new PipelineResource
{
    Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "inputPath", new ParameterSpecification { Type = ParameterType.String } },
        { "outputPath", new ParameterSpecification { Type = ParameterType.String } }
    },
    Activities = new List<Activity>
    {
        new CopyActivity
        {
            Name = "CopyFromBlobToBlob",
            Inputs = new List<DatasetReference>
            {
                new DatasetReference()
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.inputPath" }
                    }
                }
            },
            Outputs = new List<DatasetReference>
            {
                new DatasetReference
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.outputPath" }
                    }
                }
            },
            Source = new BlobSource { },
            Sink = new BlobSink { }
        }
    }
};
client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, pipeline);
Console.WriteLine(SafeJsonConvert.SerializeObject(pipeline, client.SerializationSettings));
```

## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma

Aşağıdaki kodu **bir işlem hattı çalıştırması tetikleyen** **Main** yöntemine ekleyin.

Bu kod ayrıca işlem hattında belirtilen **inputPath** ve **outputPath** parametrelerinin değerlerini kaynak ve havuz blob yollarının gerçek değerleriyle ayarlar.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> parameters = new Dictionary<string, object>
{
    { "inputPath", inputBlobPath },
    { "outputPath", outputBlobPath }
};
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters: parameters).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. Veri kopyalama işlemi tamamlanıncaya kadar durumu sürekli olarak denetlemek için aşağıdaki kodu **Main** yöntemine ekleyin.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Aşağıdaki kodu, okunan/yazılan veri boyutu gibi kopyalama etkinliği çalıştırma ayrıntılarını alan **Main** yöntemine ekleyin.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı derleyip başlatın, ardından işlem hattı yürütmesini doğrulayın.

Konsol; veri fabrikası, bağlı hizmet, veri kümeleri, işlem hattı ve işlem hattı çalıştırmasının ilerleme durumunu yazdırır. Daha sonra işlem hattı çalıştırma durumunu denetler. Okunan/yazılan veri boyutunu içeren kopyalama etkinliği ayrıntılarını görene kadar bekleyin. Ardından, [Azure Depolama gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi araçlar kullanarak, blobların değişkenlerde belirttiğiniz şekilde "inputBlobPath" yolundan "outputBlobPath" yoluna kopyalandığını doğrulayın.

### <a name="sample-output"></a>Örnek çıktı

```json
Creating data factory SPv2Factory0907...
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>",
        "type": "SecureString"
      }
    }
  }
}
Creating dataset BlobDataset...
{
  "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@{dataset().path}",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "AzureStorageLinkedService",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "path": {
        "type": "String"
      }
    }
  }
}
Creating pipeline Adfv2QuickStartPipeline...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "inputs": [
          {
            "referenceName": "BlobDataset",
            "parameters": {
              "path": "@pipeline().parameters.inputPath"
            },
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "BlobDataset",
            "parameters": {
              "path": "@pipeline().parameters.outputPath"
            },
            "type": "DatasetReference"
          }
        ],
        "name": "CopyFromBlobToBlob"
      }
    ],
    "parameters": {
      "inputPath": {
        "type": "String"
      },
      "outputPath": {
        "type": "String"
      }
    }
  }
}
Creating pipeline run...
Pipeline run ID: 308d222d-3858-48b1-9e66-acd921feaa09
Checking pipeline run status...
Status: InProgress
Status: InProgress
Checking copy activity run details...
{
    "dataRead": 331452208,
    "dataWritten": 331452208,
    "copyDuration": 23,
    "throughput": 14073.209,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedDataIntegrationUnits": 2,
    "billedDuration": 23
}

Press any key to exit...
```

## <a name="verify-the-output"></a>Çıktıyı doğrulama

İşlem hattı adftutorial blob kapsayıcısında çıktı klasörünü otomatik olarak oluşturur. Ardından, giriş klasöründeki emp.txt dosyasını çıktı klasörüne kopyalar. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı sayfasında **Yenile**’ye tıklayın. 
    
    ![Yenile](media/quickstart-create-data-factory-dot-net/output-refresh.png)
2. Klasör listesinde **output** öğesine tıklayın. 
2. **emp.txt** dosyasının output klasörüne kopyalandığını onaylayın. 

    ![Yenile](media/quickstart-create-data-factory-dot-net/output-file.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri fabrikasını programlı olarak silmek için aşağıdaki kod satırlarını programa ekleyin: 

```csharp
            Console.WriteLine("Deleting the data factory");
            client.Factories.Delete(resourceGroup, dataFactoryName);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun. 