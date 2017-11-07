---
title: "Azure Data Factory kullanarak şirket içi verileri buluta kopyalama | Microsoft Docs"
description: "Azure Data Factory’de şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposundan Azure bulutuna veri kopyalama hakkında bilgi edinin."
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
ms.date: 10/06/2017
ms.author: jingwang
ms.openlocfilehash: 95d1dce536f8f8f0fc8d93f201520fd84f0f7629
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="copy-data-between-on-premises-and-cloud"></a>Şirket içi ile bulut arasında veri kopyalama

[!INCLUDE [data-factory-what-is-include-md](../../includes/data-factory-what-is-include.md)]

#### <a name="this-tutorial"></a>Bu öğretici

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Bu öğreticide, Azure PowerShell kullanarak verileri şirket içi SQL Server veritabanından bir Azure Blob depolama alanına kopyalayan Data Factory işlem hattı oluşturacaksınız. Şirket içi veri depoları ile bulut veri depolarının tümleştirilmesine olanak tanıyan Azure Data Factory şirket içinde barındırılan tümleştirme çalışma zamanı oluşturup kullanacaksınız.  Veri fabrikası oluşturmaya yönelik diğer araçlar/SDK’lar hakkında bilgi edinmek için bkz. [Hızlı Başlangıçlar](quickstart-create-data-factory-dot-net.md).

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde şirket içi SQL Server bağlı hizmeti oluşturup şifreleme.
> * Azure Depolama bağlı hizmeti oluşturma.
> * SQL Server ve Azure Depolama veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* **SQL Server**. Bu öğreticide şirket içi SQL Server veritabanını bir **kaynak** veri deposu olarak kullanırsınız.
* **Azure Depolama hesabı**. Bu öğreticide Azure blob depolamayı bir **hedef/havuz** veri deposu olarak kullanırsınız. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
* **Azure PowerShell**. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) bölümündeki yönergeleri izleyin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **PowerShell**’i başlatın. Bu öğreticide sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız komutları yeniden çalıştırmanız gerekir.

    Aşağıdaki komutu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin:
    ```powershell
    Login-AzureRmAccount
    ```
    Bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmSubscription
    ```
    Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"
    ```
2. Bir veri fabrikası oluşturmak için **Set-AzureRmDataFactoryV2** cmdlet’ini çalıştırın. Komutu yürütmeden önce yer tutucuları kendi değerlerinizle değiştirin.

    ```powershell
    $resourceGroupName = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $df = Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
    ```

    Aşağıdaki noktalara dikkat edin:

    * Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

        ```
        Data factory name "<data factory name>" is not available.
        ```

    * Data Factory örnekleri oluşturmak için Azure aboneliğinde Katkıda Bulunan veya Yönetici rolünüz olmalıdır.
    * Şu anda, Data Factory yalnızca Doğu ABD ve Doğu ABD 2 bölgesinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

## <a name="create-a-self-hosted-ir"></a>Şirket içinde barındırılan IR oluşturma

Bu bölümde Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturabilir ve bir şirket içi düğümle (makine) ilişkilendirebilirsiniz.

1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma. Aynı ada sahip başka bir tümleştirme çalışma zamanı varsa benzersiz bir ad kullanın.

   ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Oluşturulan tümleştirme çalışma zamanının durumunu almak için aşağıdaki komutu çalıştırın.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Şirket içinde barındırılan tümleştirme çalışma zamanını bulutta Data Factory hizmetine kaydetmek üzere kimlik doğrulama anahtarlarını almak için aşağıdaki komutu çalıştırın. Şirket içinde barındırılan tümleştirme çalışma zamanını kaydetmek için anahtarlardan birini kopyalayın.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   {
       "AuthKey1":  "IR@8437c862-d6a9-4fb3-87dd-7d4865a9e845@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@8437c862-d6a9-4fb3-85dd-7d4865a9e845@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

4. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel Windows makinesine [indirin](https://www.microsoft.com/download/details.aspx?id=39717) ve önceki adımda elde edilen Kimlik Doğrulama Anahtarını kullanarak şirket içinde barındırılan tümleştirme çalışma zamanını el ile kaydedin.

   ![Tümleştirme çalışma zamanını kaydetme](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)

   Şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla kaydedildiğinde aşağıdaki iletiyi görürsünüz:

   ![Başarıyla kaydedildi](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

   Düğüm bulut hizmetine bağlandığında şu sayfayı görürsünüz:

   ![Düğüm bağlı](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Azure Depolama bağlı hizmeti oluşturma (hedef/havuz)

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **AzureStorageLinkedService.json** adlı bir JSON dosyası oluşturun. Henüz yoksa ADFv2Tutorial klasörünü oluşturun.  &lt;accountname&gt; ve &lt;accountkey&gt; sözcüklerini Azure Depolama hesabınızın adı ve anahtarıyla değiştirin.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

2. **Azure PowerShell**’de **ADFv2Tutorial** klasörüne geçin.

   **AzureStorageLinkedService** bağlı hizmetini oluşturmak için **Set-AzureRmDataFactoryV2LinkedService** cmdlet’ini çalıştırın. Bu öğreticide kullanılan cmdlet’ler, **ResourceGroupName** ve **DataFactoryName** parametrelerinin değerlerini alır. Alternatif olarak, bir cmdlet’i her çalıştırdığınızda ResourceGroupName ve DataFactoryName yazmadan Set-AzureRmDataFactoryV2 cmdlet’i tarafından döndürülen **DataFactory** nesnesini geçirebilirsiniz.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Örnek çıktı aşağıdaki gibidir:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>SQL Server bağlı hizmeti oluşturma ve şifreleme (kaynak)

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **SqlServerLinkedService.json** adlı bir JSON dosyası oluşturun: Dosyayı kaydetmeden önce **&lt;servername>**, **&lt;databasename>**, **&lt;username>**, **&lt;servername>** ve **&lt;password>** değerlerini SQL Server değerleriyle değiştirin. **&lt;integration** **runtime** **name>** değerlerini tümleştirme çalışma zamanınızın adıyla değiştirin.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde JSON yükünden alınan gizli verileri şifrelemek için, **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** komutunu çalıştırıp yukarıdaki JSON yükünü geçirebiliriz. Bu şifreleme, kimlik bilgilerinin Veri Koruma Uygulama Programlama Arabirimi (DPAPI) kullanılarak şifrelenmesini ve şirket içinde barındırılan tümleştirme çalışma zamanı düğümüne yerel olarak kaydedilmesini sağlar. Çıktı yükü, şifrelenmiş kimlik bilgilerini içeren başka bir JSON dosyasına (bu örnekte 'encryptedLinkedService.json') yönlendirilebilir.

    Komutu çalıştırmadan önce, **&lt;integration runtime name&gt;** değerini tümleştirme çalışma zamanınızın adıyla değiştirin.

   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName <integration runtime name> -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. **SqlServerLinkedService** öğesini oluşturmak için önceki adımda yer alan JSON dosyasını kullanarak aşağıdaki komutu çalıştırın:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Veri kümeleri oluşturma

### <a name="prepare-an-on-premises-sql-server-for-the-tutorial"></a>Öğretici için şirket içi SQL Server hazırlama

Bu adımda, kopyalama işlemi için girdi ve çıktı verilerini temsil eden girdi ve çıktı veri kümeleri oluşturursunuz (Şirket içi SQL Server veritabanı = > Azure blob depolama). Veri kümeleri oluşturmadan önce aşağıdaki adımları uygulayın (listeden sonra ayrıntılı adımlar verilir):

- Veri fabrikasına bağlı hizmet olarak eklediğiniz SQL Server Veritabanında **emp** adlı bir tablo oluşturun ve tabloya birkaç örnek giriş ekleyin.
- Veri fabrikasına bağlı hizmet olarak eklediğiniz Azure blob depolama hesabında **adftutorial** adlı bir blob kapsayıcı oluşturun.


1. Şirket içi SQL Server bağlı hizmeti için belirttiğiniz veritabanında (**SqlServerLinkedService**), aşağıdaki SQL betiğini kullanarak veritabanında **emp** tablosunu oluşturun.

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Tabloya bazı örnekler ekleyin:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```



### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **SqlServerDataset.json** adlı bir JSON dosyası oluşturun:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. **SqlServerDataset** veri kümesini oluşturmak için **Set-AzureRmDataFactoryV2Dataset** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Havuz Azure Blob Depolama için veri kümesi oluşturma

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **AzureBlobDataset.json** adlı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > Bu örnek kod Azure Blob Depolamada **adftutorial** adlı bir blob kapsayıcıya sahip olduğunuzu varsayar.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. **AzureBlobDataset** veri kümesini oluşturmak için **Set-AzureRmDataFactoryV2Dataset** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>İşlem hattı oluşturma

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>"SqlServerToBlobPipeline" işlem hattını oluşturma

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **SqlServerToBlobPipeline.json** adlı bir JSON dosyası oluşturun:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. **SQLServerToBlobPipeline** işlem hattını oluşturmak için **Set-AzureRmDataFactoryV2Pipeline** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını başlatma ve izleme

1. "SQLServerToBlobPipeline" işlem hattı için bir işlem hattı çalıştırması başlatın ve gelecekte izlemek üzere işlem hattı çalıştırma kimliğini yakalayın.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. "**SQLServerToBlobPipeline**" işlem hattının çalıştırma durumunu sürekli olarak denetlemek için aşağıdaki betiği çalıştırın ve kesin sonucun çıktısını alın.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Örnek çalıştırmanın çıktısı aşağıdaki gibidir:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. "**SQLServerToBlobPipeline**" işlem hattının çalıştırma kimliğini alabilir ve ayrıntılı etkinlik çalıştırma sonucunu aşağıdaki gibi denetleyebilirsiniz.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Örnek çalıştırmanın çıktısı aşağıdaki gibidir:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 4,
      "throughput": 0.01,
      "errors": []
    }
    ```
4. Havuz Azure Blob depolama hesabınıza bağlanın ve verilerin Azure SQL Veritabanından düzgün şekilde kopyalandığını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma
> * Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde şirket içi SQL Server bağlı hizmeti oluşturup şifreleme
> * Azure Depolama bağlı hizmeti oluşturma.
> * SQL Server ve Azure Depolama veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Azure Data Factory tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) makalesine bakın.

Kaynaktan hedefe verileri toplu olarak kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy.md)
