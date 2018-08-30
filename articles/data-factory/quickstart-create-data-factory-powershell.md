---
title: Azure Data Factory ile verileri Blob Depolama’ya kopyalama | Microsoft Docs
description: Azure Blob depolamadaki bir konumdan başka bir konuma veri kopyalamak için bir Azure veri fabrikası oluşturun.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: 04fd3be1bde6f640e5966b445ff2aeb8f3c27458
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43109664"
---
# <a name="create-an-azure-data-factory-using-powershell"></a>PowerShell kullanarak Azure veri fabrikası oluşturma 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](quickstart-create-data-factory-powershell.md)

Bu hızlı başlangıç, PowerShell kullanarak bir Azure veri fabrikası oluşturma işlemini açıklar. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure blob depolama alanındaki bir klasörden başka bir klasöre **kopyalar**. Azure Data Factory kullanarak verileri **dönüştürme** hakkında bir öğretici için bkz. [Öğretici: Spark kullanarak verileri dönüştürme](transform-data-using-spark.md). 

> [!NOTE]
> Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin.

#### <a name="log-in-to-powershell"></a>PowerShell’de oturum açın

1. Makinenizde **PowerShell**'i başlatın. Bu hızlı başlangıcın sonuna kadar PowerShell’i açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.
2. Aşağıdaki komutu çalıştırın ve Azure Portal'da oturum açmak için kullandığınız aynı Azure kullanıcı adını ve parolasını girin:
       
    ```powershell
    Connect-AzureRmAccount
    ```        
3. Bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmSubscription
    ```
4. Hesabınızla ilişkili birden çok aboneliğiniz varsa, birlikte çalışmak istediğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFQuickStartRG";
    ```

    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın
2. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'East US'
    ``` 
    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın. 
3. Veri fabrikasının adı için bir değişken tanımlayın. 

    > [!IMPORTANT]
    >  Veri fabrikasının adını genel olarak benzersiz olacak şekilde güncelleştirin. Örneğin, ADFTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFQuickStartFactory";
    ```

5. Veri fabrikası oluşturmak için, $ResGrp değişkenindeki Location ve ResourceGroupName özelliğini kullanarak şu **Set-AzureRmDataFactoryV2** cmdlet’ini çalıştırın: 
    
    ```powershell       
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

## <a name="create-a-linked-service"></a>Bağlı hizmet oluşturma

Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturun. Bu hızlı başlangıçta hem kaynak hem de havuz deposu olarak kullanılan bir Azure Depolama bağlı hizmeti oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin bunlara bağlanmak için çalışma zamanında kullandığı bağlantı bilgilerini içerir.

1. **C:\ADFv2QuickStartPSH** klasöründe aşağıdaki içeriğe sahip **AzureStorageLinkedService.json** adlı bir JSON dosyası oluşturun: (Henüz yoksa ADFv2QuickStartPSH adlı bir klasör oluşturun.). 

    > [!IMPORTANT]
    > Dosyayı kaydetmeden önce &lt;accountName&gt; ve &lt;accountKey&gt; değerlerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>;EndpointSuffix=core.windows.net",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
    Not Defteri’ni kullanıyorsanız, **Farklı kaydet** iletişim kutusunda **Farklı kaydetme türü** için **Tüm dosyalar**’ı seçin. Aksi takdirde, dosyaya `.txt` uzantısını ekleyebilir. Örneğin, `AzureStorageLinkedService.json.txt`. Dosyayı Not Defteri’nde açmadan önce Dosya Gezgini’nde oluşturduysanız, **Bilinen dosya türleri için uzantıları gizle** seçeneği varsayılan olarak ayarlandığı için `.txt` uzantısını görmeyebilirsiniz. Sonraki adıma geçmeden önce `.txt` uzantısını kaldırın.
2. **PowerShell**’de **ADFv2QuickStartPSH** klasörüne geçin.

    ```powershell
    Set-Location 'C:\ADFv2QuickStartPSH'
    ```
3. **AzureStorageLinkedService** bağlı hizmetini oluşturmak için **Set-AzureRmDataFactoryV2LinkedService** cmdlet’ini çalıştırın. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Veri kümesi oluşturma
Bu adımda, bir kaynaktan havuza kopyalanacak verileri temsil eden bir veri kümesi tanımlarsınız. Veri kümesi **AzureBlob** türündedir. Önceki adımda oluşturduğunuz **Azure Depolama bağlı hizmetini** ifade eder. **folderPath** özelliğini oluşturmak için bir parametre alır. Bir giriş veri kümesi için işlem hattındaki kopyalama etkinliği bu parametrenin değeri olarak giriş yolunu geçirir. Benzer şekilde, bir çıkış veri kümesi için kopyalama etkinliği bu parametrenin değeri olarak çıkış yolunu geçirir. 

1. **C:\ADFv2QuickStartPSH** klasöründe aşağıdaki içeriğe sahip **BlobDataset.json** adlı bir JSON dosyası oluşturun:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "@{dataset().path}"
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
    ```

2. **BlobDataset** veri kümesini oluşturmak için **Set-AzureRmDataFactoryV2Dataset** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```
    DatasetName       : BlobDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
  
Bu hızlı başlangıçta etkinlik içeren ve giriş blob yolu ve çıkış blob yolu şeklinde iki parametre alan bir işlem hattı oluşturursunuz. Bu parametrelerin değerleri, işlem hattı tetiklendiğinde/çalıştırıldığında ayarlanır. Kopyalama etkinliği, önceki adımda girdi ve çıktı olarak oluşturulmuş blob veri kümesini kullanır. Veri kümesi bir girdi veri kümesi olarak kullanıldığında girdi yolu belirtilir. Ayrıca, veri kümesi bir çıktı veri kümesi olarak kullanıldığında çıktı yolu belirtilir. 

1. **C:\ADFv2QuickStartPSH** klasöründe aşağıdaki içeriğe sahip **Adfv2QuickStartPipeline.json** adlı bir JSON dosyası oluşturun:

    ```json
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
    ```

2. **Adfv2QuickStartPipeline** işlem hattını oluşturmak için **Set-AzureRmDataFactoryV2Pipeline** cmdlet’ini çalıştırın.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma

Bu adımda, **inputPath** ve **outputPath** işlem hattı parametrelerinin değerlerini kaynak ve havuz blob yollarının gerçek değerleriyle ayarlarsınız. Ardından, bu bağımsız değişkenleri kullanarak bir işlem hattı çalıştırması oluşturursunuz. 

1. **C:\ADFv2QuickStartPSH** klasöründe aşağıdaki içeriğe sahip **PipelineParameters.json** adlı bir JSON dosyası oluşturun:

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```
2. **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet’ini çalıştırarak bir işlem hattı çalıştırması oluşturun ve parametre değerlerini geçirin. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

    ```powershell
    while ($True) {
        $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

        if ($Run) {
            if ($run.Status -ne 'InProgress') {
                Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
                $Run
                break
            }
            Write-Output  "Pipeline is running...status: InProgress"
        }

        Start-Sleep -Seconds 10
    }   
    ```

    İşlem hattı çalıştırmasının örnek çıktısı aşağıdaki gibidir:

    ```
    Pipeline is running...status: InProgress
    Pipeline run finished. The status is:  Succeeded
    
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    RunId             : 0000000000-0000-0000-0000-0000000000000
    PipelineName      : Adfv2QuickStartPipeline
    LastUpdated       : 9/28/2017 8:28:38 PM
    Parameters        : {[inputPath, adftutorial/input], [outputPath, adftutorial/output]}
    RunStart          : 9/28/2017 8:28:14 PM
    RunEnd            : 9/28/2017 8:28:38 PM
    DurationInMs      : 24151
    Status            : Succeeded
    Message           :
    ```

    Şu hatayı görürseniz:
    ```
    Activity CopyFromBlobToBlob failed: Failed to detect region of linked service 'AzureStorage' : 'AzureStorageLinkedService' with error '[Region Resolver] Azure Storage failed to get address for DNS. Warning: System.Net.Sockets.SocketException (0x80004005): No such host is known
    ```
    aşağıdaki adımları uygulayın: 
    1. AzureStorageLinkedService.json dosyasında Azure Depolama Hesabınızın ad ve anahtarının doğru olduğunu onaylayın. 
    2. Bağlantı dizesi biçiminin doğru olduğundan emin olun. AccountName ve AccountKey gibi özellikler noktalı virgül (`;`) karakteriyle ayrılır. 
    3. Hesap adı ve hesap anahtarının çevresinde köşeli ayraç varsa bunları kaldırın. 
    4. Örnek bir bağlantı dizesi aşağıda gösterilmiştir: 

        ```json
        "connectionString": {
            "value": "DefaultEndpointsProtocol=https;AccountName=mystorageaccountname;AccountKey=mystorageacountkey;EndpointSuffix=core.windows.net",
            "type": "SecureString"
        }
        ```
    5. [Bağlı hizmet oluşturma](#create-a-linked-service) bölümündeki adımları izleyerek bağlı hizmeti yeniden oluşturun. 
    6. [İşlem hattı çalıştırması oluşturma](#create-a-pipeline-run) bölümündeki adımları izleyerek işlem hattını yeniden çalıştırın. 
    7. Yeni işlem hattı çalıştırmasını izlemek için geçerli izleme komutunu tekrar çalıştırın. 
1. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) almak için aşağıdaki betiği çalıştırın.

    ```powershell
    Write-Output "Activity run details:"
    $Result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineRunId $RunId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $Result

    Write-Output "Activity 'Output' section:"
    $Result.Output -join "`r`n"

    Write-Output "Activity 'Error' section:"
    $Result.Error -join "`r`n"
    ```
3. Etkinlik çalıştırma sonucunun aşağıdaki örnek çıktısına benzer çıktıyı gördüğünüzü onaylayın:

    ```json
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    ActivityName      : CopyFromBlobToBlob
    PipelineRunId     : 00000000000-0000-0000-0000-000000000000
    PipelineName      : Adfv2QuickStartPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, copyDuration, throughput...}
    LinkedServiceName :
    ActivityRunStart  : 9/28/2017 8:28:18 PM
    ActivityRunEnd    : 9/28/2017 8:28:36 PM
    DurationInMs      : 18095
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity 'Output' section:
    "dataRead": 38
    "dataWritten": 38
    "copyDuration": 7
    "throughput": 0.01
    "errors": []
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)"
    "usedDataIntegrationUnits": 2
    "billedDuration": 14
    ```

[!INCLUDE [data-factory-quickstart-verify-output-cleanup.md](../../includes/data-factory-quickstart-verify-output-cleanup.md)] 

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun. 
