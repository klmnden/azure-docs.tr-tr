---
title: Azure Data Factory işlem hattında dallanma | Microsoft Docs
description: Dallanma ve zincirleme etkinlikleriyle Azure Data Factory'de veri akışını denetleme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: shlo
ms.openlocfilehash: 9a03094683a973db16aa949f0610bc7f9914be45
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457029"
---
# <a name="branching-and-chaining-activities-in-a-data-factory-pipeline"></a>Data Factory işlem hattında dallanma ve zincirleme etkinlikleri

Bu öğreticide, bazı denetim akışı özelliklerini gösteren bir Data Factory işlem hattı oluşturacaksınız. Bu işlem hattı, Azure Blob Depolama içindeki kapsayıcıdan aynı depolama hesabındaki başka bir kapsayıcıya basit bir kopyalama işlemi yapar. Kopyalama etkinliği başarılı olursa, başarılı kopyalama işleminin ayrıntılarını (örneğin, yazılan veri miktarı) bir başarı e-postası ile göndermek istersiniz. Kopyalama etkinliği başarısız olursa, kopyalama hatasının ayrıntılarını (örneğin, hata iletisi) bir hata e-postası ile göndermek istersiniz. Öğretici boyunca parametreleri nasıl geçireceğinizi göreceksiniz.

Senaryoya üst düzey genel bakış: ![Genel Bakış](media/tutorial-control-flow/overview.png)

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure Depolama bağlı hizmeti oluşturma.
> * Azure blob veri kümesi oluşturma
> * Kopyalama etkinliği ve bir web etkinliği içeren işlem hattı oluşturma
> * Etkinliklerin çıktılarını sonraki etkinliklere gönderme
> * Parametre geçirme ve sistem değişkenlerini kullanma
> * Bir işlem hattı çalıştırması başlatma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme

Bu öğreticide .NET SDK kullanılır. Azure Data Factory ile etkileşim kurmak için başka mekanizmalar kullanabilirsiniz; içindekiler tablosunda "Hızlı Başlangıçlar" bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* **Azure Depolama hesabı**. Blob depolama alanını **kaynak** veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) makalesine bakın.
* **Azure SQL Veritabanı**. Veritabanını **havuz** veri deposu olarak kullanabilirsiniz. Azure SQL Veritabanınız yoksa, oluşturma adımları için [Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) makalesine bakın.
* **Visual Studio** 2013, 2015 veya 2017. Bu makaledeki kılavuzda Visual Studio 2017 kullanılır.
* **[Azure .NET SDK](https://azure.microsoft.com/downloads/)** indirip yükleyin.
* **Azure Active Directory’de** [bu adımları](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) izleyerek bir uygulama oluşturun. Sonraki adımlarda kullandığınız şu değerleri not edin: **uygulama kimliği**, **kimlik doğrulama anahtarı** ve **kiracı kimliği**. Aynı makalede bulunan yönergeleri izleyerek uygulamayı "**Katkıda bulunan**" rolüne atayın.

### <a name="create-blob-table"></a>Blob tablosu oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayıp diskinizde **input.txt** dosyası olarak kaydedin.

    ```
    John|Doe
    Jane|Doe
    ```

2. [Azure Depolama Gezgini](https://storageexplorer.com/) gibi araçları **adfv2branch** kapsayıcısı oluşturmak ve **input.txt** dosyasını kapsayıcıya yüklemek için kullanın.

## <a name="create-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio 2015/2017'yi kullanarak bir C# .NET konsol uygulaması oluşturun.

1. **Visual Studio**’yu başlatın.
2. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın. .NET sürüm 4.5.2 veya üzeri gereklidir.
3. Sağ taraftaki proje türleri listesinden **Visual C#** -> **Konsol Uygulaması (.NET Framework)** öğesini seçin.
4. Ad olarak **ADFv2BranchTutorial** girin.
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

2. Bu statik değişkenleri **Program sınıfına** ekleyin. Yer tutucuları kendi değerlerinizle değiştirin. Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

    ```csharp
        // Set variables
        static string tenantID = "<tenant ID>";
        static string applicationId = "<application ID>";
        static string authenticationKey = "<Authentication key for your application>";
        static string subscriptionId = "<Azure subscription ID>";
        static string resourceGroup = "<Azure resource group name>";

        static string region = "East US";
        static string dataFactoryName = "<Data factory name>";

        // Specify the source Azure Blob information
        static string storageAccount = "<Azure Storage account name>";
        static string storageKey = "<Azure Storage account key>";
        // confirm that you have the input.txt file placed in th input folder of the adfv2branch container. 
        static string inputBlobPath = "adfv2branch/input";
        static string inputBlobName = "input.txt";
        static string outputBlobPath = "adfv2branch/output";
        static string emailReceiver = "<specify email address of the receiver>";

        static string storageLinkedServiceName = "AzureStorageLinkedService";
        static string blobSourceDatasetName = "SourceStorageDataset";
        static string blobSinkDatasetName = "SinkStorageDataset";
        static string pipelineName = "Adfv2TutorialBranchCopy";

        static string copyBlobActivity = "CopyBlobtoBlob";
        static string sendFailEmailActivity = "SendFailEmailActivity";
        static string sendSuccessEmailActivity = "SendSuccessEmailActivity";
    
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

Program.cs dosyanızda "CreateOrUpdateDataFactory" işlevini oluşturun:

```csharp
static Factory CreateOrUpdateDataFactory(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating data factory " + dataFactoryName + "...");
    Factory resource = new Factory
    {
        Location = region
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(resource, client.SerializationSettings));

    Factory response;
    {
        response = client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, resource);
    }

    while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
    {
        System.Threading.Thread.Sleep(1000);
    }
    return response;
}
```



Aşağıdaki kodu **veri fabrikası** oluşturan **Main** yöntemine ekleyin. 

```csharp
Factory df = CreateOrUpdateDataFactory(client);
```

## <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

Program.cs dosyanızda "StorageLinkedServiceDefinition" işlevini oluşturun:

```csharp
static LinkedServiceResource StorageLinkedServiceDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");
    AzureStorageLinkedService storageLinkedService = new AzureStorageLinkedService
    {
        ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
    LinkedServiceResource linkedService = new LinkedServiceResource(storageLinkedService, name:storageLinkedServiceName);
    return linkedService;
}
```

Aşağıdaki kodu bir **Azure Depolama bağlı hizmeti** oluşturan **Main** yöntemine ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için [Azure Blob bağlı hizmeti özellikleri](connector-azure-blob-storage.md#linked-service-properties) makalesine bakın.

```csharp
client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, StorageLinkedServiceDefinition(client));
```

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Bu bölümde biri kaynak, diğeri havuz için olmak üzere iki veri kümesi oluşturacaksınız. 

### <a name="create-a-dataset-for-source-azure-blob"></a>Kaynak Azure Blob için veri kümesi oluşturma

Aşağıdaki kodu bir **Azure blob veri kümesi** oluşturan **Main** yöntemine ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için [Azure Blob veri kümesi özellikleri](connector-azure-blob-storage.md#dataset-properties) makalesine bakın.

Azure Blob’da kaynak verilerini temsil eden bir veri kümesi tanımlayın. Bu Blob veri kümesi, önceki adımda oluşturduğunuz Azure Depolama bağlı hizmetini ifade eder:

- Blob kopyalama kaynağı konumu: **FolderPath** ve **FileName**;
- FolderPath için parametre kullanımına dikkat edin. “sourceBlobContainer” parametrenin adıdır ve ifade, işlem hattı çalıştırmasında geçirilen değerlerle değiştirilir. Parametreleri tanımlamaya yönelik söz dizimi `@pipeline().parameters.<parameterName>`

Program.cs dosyanızda "SourceBlobDatasetDefinition" işlevi oluşturma

```csharp
static DatasetResource SourceBlobDatasetDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating dataset " + blobSourceDatasetName + "...");
    AzureBlobDataset blobDataset = new AzureBlobDataset
    { 
        FolderPath = new Expression { Value = "@pipeline().parameters.sourceBlobContainer" },
        FileName = inputBlobName,
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        }
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
    DatasetResource dataset = new DatasetResource(blobDataset, name:blobSourceDatasetName);
    return dataset;
}
```

### <a name="create-a-dataset-for-sink-azure-blob"></a>Havuz Azure Blob için veri kümesi oluşturma

Program.cs dosyanızda "SourceBlobDatasetDefinition" işlevi oluşturma

```csharp
static DatasetResource SinkBlobDatasetDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating dataset " + blobSinkDatasetName + "...");
    AzureBlobDataset blobDataset = new AzureBlobDataset
    {
        FolderPath = new Expression { Value = "@pipeline().parameters.sinkBlobContainer" },
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        }
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
    DatasetResource dataset = new DatasetResource(blobDataset, name: blobSinkDatasetName);
    return dataset;
}
```

Aşağıdaki kodu hem Azure blob depolama kaynağı hem de havuz veri kümeleri oluşturan **Main** yöntemine ekleyin. 

```csharp
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSourceDatasetName, SourceBlobDatasetDefinition(client));

client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSinkDatasetName, SinkBlobDatasetDefinition(client));
```

## <a name="create-a-c-class-emailrequest"></a>Oluşturma bir C# sınıfı: EmailRequest

C# projenizde **EmailRequest** adlı bir sınıf oluşturun. Bu sınıf, bir e-posta gönderirken işlem hattının gövde isteğinde gönderdiği özellikleri tanımlar. Bu öğreticide işlem hattı, işlem hattından e-postaya dört özellik gönderir:

- **İleti**: E-postanın gövdesi. Kopyalama başarılı olursa bu özellik, çalıştırmanın ayrıntılarını (yazılan veri sayısı) içerir. Kopyalama başarısız olursa, bu özellik hatanın ayrıntılarını içerir.
- **Veri fabrikası adı**: veri fabrikasının adı
- **İşlem hattı adı**: İşlem hattının adı
- **Alıcı**: Geçirilen parametre. Bu özellik, e-postanın alıcısını belirtir.

```csharp
    class EmailRequest
    {
        [Newtonsoft.Json.JsonProperty(PropertyName = "message")]
        public string message;

        [Newtonsoft.Json.JsonProperty(PropertyName = "dataFactoryName")]
        public string dataFactoryName;

        [Newtonsoft.Json.JsonProperty(PropertyName = "pipelineName")]
        public string pipelineName;

        [Newtonsoft.Json.JsonProperty(PropertyName = "receiver")]
        public string receiver;

        public EmailRequest(string input, string df, string pipeline, string receiverName)
        {
            message = input;
            dataFactoryName = df;
            pipelineName = pipeline;
            receiver = receiverName;
        }
    }
```

## <a name="create-email-workflow-endpoints"></a>E-posta iş akışı uç noktaları oluşturma

E-posta göndermeyi tetiklemek için, [Logic Apps](../logic-apps/logic-apps-overview.md) kullanarak iş akışını tanımlayın. Mantıksal Uygulama iş akışı oluşturma ayrıntıları için bkz. [Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

### <a name="success-email-workflow"></a>Başarı e-postası iş akışı 

`CopySuccessEmail` adlı bir Mantıksal Uygulama iş akışı oluşturun. İş akışı tetikleyicisini `When an HTTP request is received` olarak tanımlayın ve bir `Office 365 Outlook – Send an email` eylemi ekleyin.

![Başarı e-postası iş akışı](media/tutorial-control-flow/success-email-workflow.png)

İstek tetikleyiciniz için `Request Body JSON Schema` alanını aşağıdaki JSON ile doldurun:

```json
{
    "properties": {
        "dataFactoryName": {
            "type": "string"
        },
        "message": {
            "type": "string"
        },
        "pipelineName": {
            "type": "string"
        },
        "receiver": {
            "type": "string"
        }
    },
    "type": "object"
}
```

Bu işlem, önceki bölümde oluşturduğunuz **EmailRequest** sınıfı ile uyumludur. 

İsteğiniz Mantıksal Uygulama Tasarımcısı'nda aşağıdaki gibi görünmelidir:

![Mantıksal Uygulama tasarımcısı - istek](media/tutorial-control-flow/logic-app-designer-request.png)

**E-posta Gönder** eylemi için, isteğin Gövde JSON şemasında geçirilen özellikleri kullanarak e-posta biçimini özelleştirin. Örnek aşağıda verilmiştir:

![Mantıksal Uygulama tasarımcısı - e-posta gönderme eylemi](media/tutorial-control-flow/send-email-action.png)

Başarı e-postası iş akışınız için HTTP Post isteği URL’sini not alın:

```
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

## <a name="fail-email-workflow"></a>Hata e-postası iş akışı 

**CopySuccessEmail**’i kopyalayın ve başka bir **CopyFailEmail** Logic Apps iş akışı oluşturun. İstek tetikleyicisinde `Request Body JSON schema` değeri aynıdır. Hata e-postasına uyarlamak için e-postanızın biçimini `Subject` olarak değiştirin. Örnek aşağıda verilmiştir:

![Mantıksal Uygulama tasarımcısı - hata e-postası iş akışı](media/tutorial-control-flow/fail-email-workflow.png)

Hata e-postası iş akışınız için HTTP Post isteği URL’sini not alın:

```
//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

Şu anda iki iş akışı URL'niz olmalıdır:

```
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000

//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Aşağıdaki kodu bir kopyalama etkinliği ve dependsOn özelliği ile işlem hattı oluşturan Main yöntemine ekleyin. Bu öğreticide işlem hattı, kaynak olarak Blob veri kümesini ve havuz olarak başka bir Blob veri kümesini alan kopyalama etkinliği adlı bir etkinlik içerir. Kopyalama etkinliği başarılı ve başarısız olduktan sonra farklı e-posta görevlerini çağırır.

Bu işlem hattında aşağıdaki özellikleri kullanırsınız:

- Parametreler
- Web Etkinliği
- Etkinlik bağımlılığı
- Bir etkinliğin çıktısını sonraki etkinliğin girdisi olarak kullanma

Şimdi aşağıdaki işlem hattını bölümler halinde analiz edelim:

```csharp

static PipelineResource PipelineDefinition(DataFactoryManagementClient client)
        {
            Console.WriteLine("Creating pipeline " + pipelineName + "...");
            PipelineResource resource = new PipelineResource
            {
                Parameters = new Dictionary<string, ParameterSpecification>
                {
                    { "sourceBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
                    { "sinkBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
                    { "receiver", new ParameterSpecification { Type = ParameterType.String } }

                },
                Activities = new List<Activity>
                {
                    new CopyActivity
                    {
                        Name = copyBlobActivity,
                        Inputs = new List<DatasetReference>
                        {
                            new DatasetReference
                            {
                                ReferenceName = blobSourceDatasetName
                            }
                        },
                        Outputs = new List<DatasetReference>
                        {
                            new DatasetReference
                            {
                                ReferenceName = blobSinkDatasetName
                            }
                        },
                        Source = new BlobSource { },
                        Sink = new BlobSink { }
                    },
                    new WebActivity
                    {
                        Name = sendSuccessEmailActivity,
                        Method = WebActivityMethod.POST,
                        Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/00000000000000000000000000000000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=0000000000000000000000000000000000000000000000",
                        Body = new EmailRequest("@{activity('CopyBlobtoBlob').output.dataWritten}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
                        DependsOn = new List<ActivityDependency>
                        {
                            new ActivityDependency
                            {
                                Activity = copyBlobActivity,
                                DependencyConditions = new List<String> { "Succeeded" }
                            }
                        }
                    },
                    new WebActivity
                    {
                        Name = sendFailEmailActivity,
                        Method =WebActivityMethod.POST,
                        Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/000000000000000000000000000000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=0000000000000000000000000000000000000000000",
                        Body = new EmailRequest("@{activity('CopyBlobtoBlob').error.message}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
                        DependsOn = new List<ActivityDependency>
                        {
                            new ActivityDependency
                            {
                                Activity = copyBlobActivity,
                                DependencyConditions = new List<String> { "Failed" }
                            }
                        }
                    }
                }
            };
            Console.WriteLine(SafeJsonConvert.SerializeObject(resource, client.SerializationSettings));
            return resource;
        }
```

Aşağıdaki kodu, işlem hattını oluşturan **Main** yöntemine ekleyin:

```
client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, PipelineDefinition(client));
```

### <a name="parameters"></a>Parametreler

İşlem hattının birinci bölümü parametreleri tanımlar. 

- sourceBlobContainer - işlem hattında kaynak blob veri kümesi tarafından tüketilen parametre.
- sinkBlobContainer - işlem hattında havuz blob veri kümesi tarafından tüketilen parametre
- receiver – bu parametre, işlem hattında e-posta adresi bu parametre ile belirtilen alıcıya başarı veya hata e-postaları gönderen iki Web etkinliği tarafından kullanılır.


```csharp
Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "sourceBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
        { "sinkBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
        { "receiver", new ParameterSpecification { Type = ParameterType.String } }
    },
```

### <a name="web-activity"></a>Web Etkinliği

Web Etkinliği herhangi bir REST uç noktasına çağrı yapmaya olanak tanır. Etkinlik hakkında daha fazla bilgi için bkz. [Web Etkinliği](control-flow-web-activity.md). Bu işlem hattı, Logic Apps e-posta iş akışını çağırmak için bir Web Etkinliği kullanır. İki web etkinliği oluşturursunuz: bir tanesi **CopySuccessEmail**, diğeri ise **CopyFailWorkFlow** iş akışını çağırır.

```csharp
        new WebActivity
        {
            Name = sendCopyEmailActivity,
            Method = WebActivityMethod.POST,
            Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/12345",
            Body = new EmailRequest("@{activity('CopyBlobtoBlob').output.dataWritten}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
            DependsOn = new List<ActivityDependency>
            {
                new ActivityDependency
                {
                    Activity = copyBlobActivity,
                    DependencyConditions = new List<String> { "Succeeded" }
                }
            }
        }
```

“Url” özelliğinde İstek URL uç noktalarını Logic Apps iş akışınızdan uygun şekilde yapıştırın. “Body” özelliğine “EmailRequest” sınıfının bir örneğini geçirin. E-posta isteği aşağıdaki özellikleri içerir:

- İleti – `@{activity('CopyBlobtoBlob').output.dataWritten` değerini geçirme. Önceki kopyalama etkinliğinin bir özelliğine erişir ve dataWritten değerini geçirir. Hata durumunda `@{activity('CopyBlobtoBlob').error.message` yerine hata çıktısını geçirir.
- Veri Fabrikası Adı – `@{pipeline().DataFactory}` değerini geçiren ve ilgili veri fabrikası adına erişmenize olanak tanıyan bir sistem değişkenidir. Sistem değişkenlerinin listesi için [Sistem Değişkenleri](control-flow-system-variables.md) makalesine bakın.
- İşlem Hattı Adı – `@{pipeline().Pipeline}` değerini geçirme. Bu da ilgili işlem hattı adına erişmenize olanak tanıyan bir sistem değişkenidir. 
- Alıcı – "\@pipeline().parameters.receiver") değerini geçirme. İşlem hattı parametrelerine erişim.
 
Bu kod, başarılı olan önceki kopyalama etkinliğine bağlı olarak yeni bir Etkinlik Bağımlılığı oluşturur.

## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma

Aşağıdaki kodu **bir işlem hattı çalıştırması tetikleyen** **Main** yöntemine ekleyin.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> arguments = new Dictionary<string, object>
{
    { "sourceBlobContainer", inputBlobPath },
    { "sinkBlobContainer", outputBlobPath },
    { "receiver", emailReceiver }
};
 
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, arguments).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="main-class"></a>Main sınıfı 

Son Main yönteminiz aşağıdaki gibi görünmelidir. Bir işlem hattı çalıştırması tetiklemek için programınızı derleyip çalıştırın!

```csharp
// Authenticate and create a data factory management client
var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };

Factory df = CreateOrUpdateDataFactory(client);

client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, StorageLinkedServiceDefinition(client));
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSourceDatasetName, SourceBlobDatasetDefinition(client));
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSinkDatasetName, SinkBlobDatasetDefinition(client));

client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, PipelineDefinition(client));

Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> arguments = new Dictionary<string, object>
{
    { "sourceBlobContainer", inputBlobPath },
    { "sinkBlobContainer", outputBlobPath },
    { "receiver", emailReceiver }
};

CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, arguments).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. İşlem hattı çalıştırmasını veri kopyalama işlemi tamamlanıncaya kadar sürekli olarak izlemek için aşağıdaki kodu **Main** yöntemine ekleyin.

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
    {
        Console.WriteLine(activityRuns.First().Output);
        //SaveToJson(SafeJsonConvert.SerializeObject(activityRuns.First().Output, client.SerializationSettings), "ActivityRunResult.json", folderForJsons);
    }
    else
        Console.WriteLine(activityRuns.First().Error);

    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı derleyip başlatın, ardından işlem hattı yürütmesini doğrulayın.
Konsol; veri fabrikası, bağlı hizmet, veri kümeleri, işlem hattı ve işlem hattı çalıştırmasının ilerleme durumunu yazdırır. Daha sonra işlem hattı çalıştırma durumunu denetler. Okunan/yazılan veri boyutunu içeren kopyalama etkinliği ayrıntılarını görene kadar bekleyin. Ardından, Azure Depolama gezgini gibi araçlar kullanarak, blobların değişkenlerde belirttiğiniz şekilde "inputBlobPath" yolundan "outputBlobPath" yoluna kopyalandığını doğrulayın.

**Örnek çıktı:**

```json
Creating data factory DFTutorialTest...
{
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "type": "AzureStorage",
  "typeProperties": {
    "connectionString": {
      "type": "SecureString",
      "value": "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***"
    }
  }
}
Creating dataset SourceStorageDataset...
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "type": "Expression",
      "value": "@pipeline().parameters.sourceBlobContainer"
    },
    "fileName": "input.txt"
  },
  "linkedServiceName": {
    "type": "LinkedServiceReference",
    "referenceName": "AzureStorageLinkedService"
  }
}
Creating dataset SinkStorageDataset...
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "type": "Expression",
      "value": "@pipeline().parameters.sinkBlobContainer"
    }
  },
  "linkedServiceName": {
    "type": "LinkedServiceReference",
    "referenceName": "AzureStorageLinkedService"
  }
}
Creating pipeline Adfv2TutorialBranchCopy...
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
            "type": "DatasetReference",
            "referenceName": "SourceStorageDataset"
          }
        ],
        "outputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SinkStorageDataset"
          }
        ],
        "name": "CopyBlobtoBlob"
      },
      {
        "type": "WebActivity",
        "typeProperties": {
          "method": "POST",
          "url": "https://xxxx.eastus.logic.azure.com:443/workflows/... ",
          "body": {
            "message": "@{activity('CopyBlobtoBlob').output.dataWritten}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
          }
        },
        "name": "SendSuccessEmailActivity",
        "dependsOn": [
          {
            "activity": "CopyBlobtoBlob",
            "dependencyConditions": [
              "Succeeded"
            ]
          }
        ]
      },
      {
        "type": "WebActivity",
        "typeProperties": {
          "method": "POST",
          "url": "https://xxx.eastus.logic.azure.com:443/workflows/... ",
          "body": {
            "message": "@{activity('CopyBlobtoBlob').error.message}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
          }
        },
        "name": "SendFailEmailActivity",
        "dependsOn": [
          {
            "activity": "CopyBlobtoBlob",
            "dependencyConditions": [
              "Failed"
            ]
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      },
      "receiver": {
        "type": "String"
      }
    }
  }
}
Creating pipeline run...
Pipeline run ID: 00000000-0000-0000-0000-0000000000000
Checking pipeline run status...
Status: InProgress
Status: InProgress
Status: Succeeded
Checking copy activity run details...
{
  "dataRead": 20,
  "dataWritten": 20,
  "copyDuration": 4,
  "throughput": 0.01,
  "errors": [],
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
}
{}

Press any key to exit...
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure Depolama bağlı hizmeti oluşturma.
> * Azure blob veri kümesi oluşturma
> * Kopyalama etkinliği ve bir web etkinliği içeren işlem hattı oluşturma
> * Etkinliklerin çıktılarını sonraki etkinliklere gönderme
> * Parametre geçirme ve sistem değişkenlerini kullanma
> * Bir işlem hattı çalıştırması başlatma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme

Şimdi, Azure Data Factory hakkında daha fazla bilgi için Kavramlar bölümüne geçebilirsiniz.
> [!div class="nextstepaction"]
>[İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)