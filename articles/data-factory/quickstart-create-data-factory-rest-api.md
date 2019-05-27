---
title: REST API kullanarak Azure veri fabrikası oluşturma | Microsoft Docs
description: Azure Blob depolamadaki bir konumdan başka bir konuma veri kopyalamak için bir Azure veri fabrikası oluşturun.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: jingwang
ms.openlocfilehash: d5255e8cd8c662295a714931a3e292b20ab10079
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66156946"
---
# <a name="quickstart-create-an-azure-data-factory-and-pipeline-by-using-the-rest-api"></a>Hızlı Başlangıç: REST API kullanarak Azure veri fabrikası ve işlem hattı oluşturma

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](quickstart-create-data-factory-rest-api.md)

Azure Data Factory, bulutta veri hareketi ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir. Azure Data Factory’yi kullanarak, farklı veri depolarından veri alabilen, Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri işleyebilen/dönüştürebilen ve çıktı verilerini iş zekası (BI) uygulamaları tarafından kullanılabilmesi için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilen veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturup zamanlayabilirsiniz.

Bu hızlı başlangıç, REST API kullanarak bir Azure veri fabrikası oluşturma işlemini açıklar. Bu veri fabrikasındaki işlem hattı, verileri Azure blob depolama alanındaki bir konumdan başka bir konuma kopyalar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* **Azure aboneliği**. Bir aboneliğiniz yoksa, bir [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz.
* **Azure Depolama hesabı**. Blob depolama alanını **kaynak** ve **havuz** veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) makalesine bakın.
* Blob Depolama içinde bir **blob kapsayıcısı** oluşturun, kapsayıcıda bir giriş **klasörü** oluşturun ve bazı dosyaları klasöre yükleyin. [Azure Depolama gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi araçları kullanarak Azure Blob depolama hesabına bağlanabilir, bir blob kapsayıcısı oluşturabilir, giriş dosyasını karşıya yükleyebilir ve çıktı dosyasını doğrulayabilirsiniz.
* **Azure PowerShell**'i yükleyin. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-Az-ps) bölümündeki yönergeleri izleyin. Bu hızlı başlangıçta RES API çağrılarını çağırmak için PowerShell kullanılır.
* **Azure Active Directory’de** [bu yönergeyi](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) izleyerek bir uygulama oluşturun. Sonraki adımlarda kullandığınız şu değerleri not edin: **uygulama kimliği**, **kimlik doğrulama anahtarı** ve **kiracı kimliği**. Uygulamayı "**Katkıda Bulunan**" rolüne atayın.

## <a name="set-global-variables"></a>Genel değişkenleri ayarlama

1. **PowerShell**’i başlatın. Bu hızlı başlangıcın sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız komutları yeniden çalıştırmanız gerekir.

    Aşağıdaki komutu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin:
    
    ```powershell
    Connect-AzAccount
    ```
    Bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzSubscription
    ```
    Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzSubscription -SubscriptionId "<SubscriptionId>"
    ```
2. Yer tutucuları kendi değerlerinizle değiştirdikten sonra, sonraki adımlarda kullanılacak genel değişkenleri ayarlamak için aşağıdaki komutları çalıştırın.

    ```powershell
    $tenantID = "<your tenant ID>"
    $appId = "<your application ID>"
    $authKey = "<your authentication key for the application>"
    $subsId = "<your subscription ID to create the factory>"
    $resourceGroup = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $apiVersion = "2018-06-01"
    ```

## <a name="authenticate-with-azure-ad"></a>Azure AD ile kimlik doğrulaması

Azure Active Directory (AAD) ile kimlik doğrulaması yapmak için aşağıdaki komutları çalıştırın:

```powershell
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]"https://login.microsoftonline.com/${tenantId}"
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($appId, $authKey)
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Veri fabrikası oluşturmak için aşağıdaki komutları çalıştırın:

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}?api-version=${apiVersion}"
$body = @"
{
    "name": "$dataFactoryName",
    "location": "East US",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    Data factory name "ADFv2QuickStartDataFactory" is not available.
    ```
* Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

Örnek yanıt aşağıdaki gibidir:

```json
{
    "name": "<dataFactoryName>",
    "tags": {
    },
    "properties":  {
        "provisioningState":  "Succeeded",
        "loggingStorageAccountKey":  "**********",
        "createTime":  "2017-09-14T06:22:59.9106216Z",
        "version":  "2018-06-01"
    },
    "identity":  {
        "type":  "SystemAssigned",
        "principalId":  "<service principal ID>",
        "tenantId":  "<tenant ID>"
    },
    "id":  "dataFactoryName",
    "type":  "Microsoft.DataFactory/factories",
    "location":  "East US"
}
```

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturursunuz. Bu hızlı başlangıçta yalnızca bir Azure Depolama bağlı hizmetini örnekte "AzureStorageLinkedService" olarak adlandırılmış bir kopyalama kaynağı ve havuz deposu olarak oluşturmanız gerekir.

**AzureStorageLinkedService** adlı bağlı hizmeti oluşturmak için aşağıdaki komutları çalıştırın:

Komutları yürütmeden önce &lt;accountName&gt; ve &lt;accountKey&gt; değerlerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin.

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/linkedservices/AzureStorageLinkedService?api-version=${apiVersion}"
$body = @"
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>",
                "type": "SecureString"
            }
        }
    }
}
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Örnek çıktı aşağıdaki gibidir:

```json
{
    "id":  "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>/linkedservices/AzureStorageLinkedService",
    "name":  "AzureStorageLinkedService",
    "properties":  {
        "type":  "AzureStorage",
        "typeProperties":  {
            "connectionString":  "@{value=**********; type=SecureString}"
        }
    },
    "etag":  "0000c552-0000-0000-0000-59b1459c0000"
}
```

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Bir kaynaktan havuza kopyalanacak verileri temsil eden bir veri kümesi tanımlayın. Bu örnekte, bu Blob veri kümesi önceki adımda oluşturduğunuz Azure Depolama bağlı hizmetini ifade eder. Veri kümesi, değeri veri kümesini kullanan bir etkinlikte tanımlanmış bir parametre alır. Parametre, verilerin bulunduğu/depolandığı yeri işaret eden "folderPath" öğesini oluşturmak için kullanılır.

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/datasets/BlobDataset?api-version=${apiVersion}"
$body = @"
{
    "name": "BlobDataset",
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
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Örnek çıktı aşağıdaki gibidir:

```json
{
    "id":  "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>/datasets/BlobDataset",
    "name":  "BlobDataset",
    "properties":  {
        "type":  "AzureBlob",
        "typeProperties":  {
            "folderPath":  "@{value=@{dataset().path}; type=Expression}"
        },
        "linkedServiceName":  {
            "referenceName":  "AzureStorageLinkedService",
            "type":  "LinkedServiceReference"
        },
        "parameters":  {
            "path":  "@{type=String}"
        }
    },
    "etag":  "0000c752-0000-0000-0000-59b1459d0000"
}
```

## <a name="create-pipeline"></a>İşlem hattı oluşturma

Bu örnekte, işlem hattı bir etkinlik içerir ve giriş blob yolu ve çıkış blob yolu şeklinde iki parametre alır. Bu parametrelerin değerleri, işlem hattı tetiklendiğinde/çalıştırıldığında ayarlanır. Kopyalama etkinliği, önceki adımda girdi ve çıktı olarak oluşturulmuş blob veri kümesini ifade eder. Veri kümesi bir girdi veri kümesi olarak kullanıldığında girdi yolu belirtilir. Ayrıca, veri kümesi bir çıktı veri kümesi olarak kullanıldığında çıktı yolu belirtilir.

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelines/Adfv2QuickStartPipeline?api-version=${apiVersion}"
$body = @"
{
    "name": "Adfv2QuickStartPipeline",
    "properties": {
        "activities": [
            {
                "name": "CopyFromBlobToBlob",
                "type": "Copy",
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
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                }
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
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Örnek çıktı aşağıdaki gibidir:

```json
{
    "id":  "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>/pipelines/Adfv2QuickStartPipeline",
    "name":  "Adfv2QuickStartPipeline",
    "properties":  {
        "activities":  [
            "@{name=CopyFromBlobToBlob; type=Copy; inputs=System.Object[]; outputs=System.Object[]; typeProperties=}"
        ],
        "parameters":  {
            "inputPath":  "@{type=String}",
            "outputPath":  "@{type=String}"
        }
    },
    "etag":  "0000c852-0000-0000-0000-59b1459e0000"
}
```

## <a name="create-pipeline-run"></a>İşlem hattı çalıştırması oluşturma

Bu adımda, işlem hattında belrtilen **inputPath** ve **outputPath** parametrelerinin değerlerini kaynak ve havuz blob yollarının gerçek değerleriyle ayarlayacak ve bir işlem hattı çalıştırması tetikleyeceksiniz. Yanıt gövdesinde döndürülen işlem hattı çalıştırma kimliği sonraki izleme API’sinde kullanılır.

Dosyayı kaydetmeden önce **inputPath** ve **outputPath** değerini, verilerin kopyalanacağı kaynak ve havuz blob yolunuzla değiştirin.


```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelines/Adfv2QuickStartPipeline/createRun?api-version=${apiVersion}"
$body = @"
{
    "inputPath": "<the path to existing blob(s) to copy data from, e.g. containername/path>",
    "outputPath": "<the blob path to copy data to, e.g. containername/path>"
}
"@
$response = Invoke-RestMethod -Method POST -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
$runId = $response.runId
```

Örnek çıktı aşağıdaki gibidir:

```json
{
    "runId":  "2f26be35-c112-43fa-9eaa-8ba93ea57881"
}
```

## <a name="monitor-pipeline"></a>İşlem hattını izleme

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    {
        "key":  "000000000-0000-0000-0000-00000000000",
        "timestamp":  "2017-09-07T13:12:39.5561795Z",
        "runId":  "000000000-0000-0000-0000-000000000000",
        "dataFactoryName":  "<dataFactoryName>",
        "pipelineName":  "Adfv2QuickStartPipeline",
        "parameters":  [
            "inputPath: <inputBlobPath>",
            "outputPath: <outputBlobPath>"
        ],
        "parametersCount":  2,
        "parameterNames":  [
            "inputPath",
            "outputPath"
        ],
        "parameterNamesCount":  2,
        "parameterValues":  [
            "<inputBlobPath>",
            "<outputBlobPath>"
        ],
        "parameterValuesCount":  2,
        "runStart":  "2017-09-07T13:12:00.3710792Z",
        "runEnd":  "2017-09-07T13:12:39.5561795Z",
        "durationInMs":  39185,
        "status":  "Succeeded",
        "message":  ""
    }
    ```

2. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) almak için aşağıdaki betiği çalıştırın.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    {
        "value":  [
            {
                "id":  "000000000-0000-0000-0000-00000000000",
                "timestamp":  "2017-09-07T13:12:38.4780542Z",
                "pipelineRunId":  "000000000-0000-00000-0000-0000000000000",
                "pipelineName":  "Adfv2QuickStartPipeline",
                "status":  "Succeeded",
                "failureType":  "",
                "linkedServiceName":  "",
                "activityName":  "CopyFromBlobToBlob",
                "activityType":  "Copy",
                "activityStart":  "2017-09-07T13:12:02.3299261Z",
                "activityEnd":  "2017-09-07T13:12:38.4780542Z",
                "duration":  36148,
                "input":  "@{source=; sink=}",
                "output":  "@{dataRead=331452208; dataWritten=331452208; copyDuration=22; throughput=14712.9; errors=System.Object[]; effectiveIntegrationRuntime=DefaultIntegrationRuntime (West US); usedDataIntegrationUnits=2; billedDuration=22}",
                "error":  "@{errorCode=; message=; failureType=; target=CopyFromBlobToBlob}"
            }
        ]
    }
    ```

## <a name="verify-the-output"></a>Çıktıyı doğrulama

Azure Depolama gezginini kullanarak, bir işlem hattı çalıştırması oluştururken belirttiğiniz şekilde blobların "inputBlobPath" yolundan "outputBlobPath" yoluna kopyalandığını doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki şekilde temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silebilirsiniz. Diğer kaynakları korumak istiyorsanız yalnızca bu öğreticide oluşturduğunuz veri kaynağını silin.

Kaynak grubunun tamamını silmek için şu komutu çalıştırın:
```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

Yalnızca veri fabrikası tablosunu silmek için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzDataFactoryV2 -Name "<NameOfYourDataFactory>" -ResourceGroupName "<NameOfResourceGroup>"
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun.
