---
title: "PowerShell kullanarak Azure veri fabrikası oluşturma | Microsoft Docs"
description: "Aynı Azure Blob Depolama alanındaki bir konumdan başka bir konuma veri kopyalamak için bir Azure veri fabrikası oluşturun."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: 63e4c654409651f6655da1bed6ab2f544cf024dd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-azure-data-factory-and-pipeline-using-powershell"></a>PowerShell kullanarak Azure veri fabrikası ve işlem hattı oluşturma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](quickstart-create-data-factory-powershell.md)

Bu hızlı başlangıç, PowerShell kullanarak bir Azure veri fabrikası oluşturma işlemini açıklar. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure blob depolama alanındaki bir konumdan başka bir konuma kopyalar. Azure Data Factory kullanarak verileri dönüştürme hakkında bir öğretici için bkz. [Öğretici: Spark kullanarak verileri dönüştürme](transform-data-using-spark.md). 

Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md).

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

### <a name="azure-storage-account"></a>Azure Depolama Hesabı
Bu hızlı başlangıçta, genel amaçlı Azure Depolama Hesabı'nı (özel olarak Blob Depolama) hem **kaynak** hem de **havuz/hedef** veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). 

#### <a name="get-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu hızlı başlangıçta, Azure depolama hesabınızın adını ve anahtarını kullanırsınız. Aşağıdaki yordam, depolama hesabınızın adını ve anahtarını alma adımlarını sağlar. 

1. Web tarayıcısını açın ve [Azure Portal](https://portal.azure.com)'a gidin. Azure kullanıcı adınız ve parolanızla oturum açın. 
2. Sol taraftaki menüde **Diğer hizmetler >** öğesine tıklayın, **Depolama** anahtar sözcüğüyle filtreleyin ve **Depolama hesapları**'nı seçin.

    ![Depolama hesabını arama](media/quickstart-create-data-factory-powershell/search-storage-account.png)
3. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse) ve ardından **depolama hesabınızı** seçin. 
4. **Depolama hesabı** sayfasında, menüden **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adını ve anahtarını alma](media/quickstart-create-data-factory-powershell/storage-account-name-key.png)
5. **Depolama hesabı adı** ve **key1** alanlarının değerlerini panoya kopyalayın. Bunları not defterine veya başka bir düzenleyici yapıştırın ve kaydedin.  

#### <a name="create-input-folder-and-files"></a>Giriş klasörünü ve dosyaları oluşturma
Bu bölümde, Azure blob depolamanızda adftutorial adlı bir blob kapsayıcısı oluşturursunuz. Ardından, kapsayıcıda giriş adlı bir klasör oluşturur ve giriş klasörüne örnek bir dosya yüklersiniz. 

1. Makinenizde [Azure Depolama gezgini](https://azure.microsoft.com/features/storage-explorer/) yoksa yükleyin. 
2. Makinenizde **Microsoft Azure Depolama Gezgini**'ni başlatın.   
3. **Azure Depolamaya Bağlan** penceresinde, **Depolama hesap adı ve anahtarı kullan**'ı seçin ve **İleri**'ye tıklayın. **Azure Depolamaya Bağlan** penceresini görmüyorsanız, ağaç görünümünde **Depolama Hesapları**'na sağ tıklayın ve **Azure depolamaya bağlan**'a tıklayın. 

    ![Azure depolamaya bağlanma](media/quickstart-create-data-factory-powershell/storage-explorer-connect-azure-storage.png)
4. **Ad ve Anahtar kullanarak ekle** penceresinde, önce adımda kaydettiğiniz **Hesap adı** ve **Hesap anahtarı** değerlerini yapıştırın. Ardından **İleri**'ye tıklayın. 
5. **Bağlantı Özeti** penceresinde **Bağlan**'a tıklayın.
6. Ağaç görünümünde **(Yerel ve Bağlı)** -> **Depolama Hesapları**'nın altında depolama hesabınızı gördüğünüzü doğrulayın. 
7. **Blob Kapsayıcıları**'nı genişletin ve **adftutorial** blob kapsayıcısının olmadığını doğrulayın. Zaten varsa, kapsayıcıyı oluşturma adımlarını atlayın. 
8. **Blob Kapsayıcıları**'na sağ tıklayın ve **Blob Kapsayıcısı Oluştur**'u seçin.

    ![Blob kapsayıcısı oluşturma](media/quickstart-create-data-factory-powershell/stroage-explorer-create-blob-container-menu.png)
9. Ad olarak **adftutorial** girin ve **ENTER** tuşuna basın. 
10. Ağaç görünümünde **adftutorial** kapsayıcısının seçildiğini doğrulayın. 
11. Araç çubuğunda **Yeni Klasör**'e tıklayın. 

    ![Klasör oluştur düğmesi](media/quickstart-create-data-factory-powershell/stroage-explorer-new-folder-button.png)
12. **Yeni Sanal Dizin Oluştur** penceresinde **Ad** olarak **giriş** girin ve **Tamam**'a tıklayın. 

    ![Dizin oluştur iletişim kutusu](media/quickstart-create-data-factory-powershell/storage-explorer-create-new-directory-dialog.png)
13. **Not Defteri**'ni başlatın ve aşağıdakileri içeren **emp.txt** adlı bir dosya oluşturun: 
    
    ```
    John, Doe
    Jane, Doe
    ```    
    Bu dosyayı **c:\ADFv2QuickStartPSH** klasörüne kaydedin: **ADFv2QuickStartPSH** klasörü yoksa, oluşturun. 
14. Araç çubuğunda **Karşıya Yükle** düğmesine tıklayın ve **Dosyaları Karşıya Yükle**'yi seçin. 

    ![Karşıya yükle düğmesi](media/quickstart-create-data-factory-powershell/storage-explorer-upload-button.png)
15. **Dosyaları karşıya yükle** penceresinde, **Dosyalar** için `...` öğesini seçin. 
16. **Karşıya yüklenecek dosyaları seçin** penceresinde, **emp.txt** dosyasını içeren klasöre gidin ve dosyayı seçin. 

    ![Dosyaları karşıya yükle iletişim kutusu](media/quickstart-create-data-factory-powershell/storage-explorer-upload-files-dialog.png)
17. **Dosyaları karşıya yükle** penceresinde **Karşıya Yükle**'ye tıklayın. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
En son Azure PowerShell makinenizde yüklü değilse, yükleyin. 

1. Web tarayıcısında [Azure SDK İndirmeleri ve SDK'lar](https://azure.microsoft.com/downloads/) sayfasına gidin. 
2. **Komut satırı araçları** -> **PowerShell** bölümünde **Windows yüklemesi**'ne tıklayın. 
3. Azure PowerShell'i yüklemek için **MSI** dosyasını çalıştırın. 

Ayrıntılı yönergeler için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Azure PowerShell'de oturum açma
Makinenizde **PowerShell**'i başlatın. Bu hızlı başlangıcın sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.

1. Aşağıdaki komutu çalıştırın ve Azure Portal'da oturum açmak için kullandığınız Azure kullanıcı adını ve parolasını girin:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Birden çok Azure aboneliğiniz varsa, bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Daha sonra PowerShell komutlarında kullanabileceğiniz veri fabrikası adı için bir değişken tanımlayın. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Veri fabrikasının konumu için bir değişken tanımlayın: 

    ```powershell
    $location = "East US"
    ```
4. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$resourceGroupName` değişkenine farklı bir değer atayın ve yeniden deneyin. Kaynak grubunu diğeriyle paylaşmak isterseniz, sonraki adımdan devam edin. 
5. Veri fabrikası oluşturmak için aşağıdaki **Set-AzureRmDataFactoryV2** cmdlet’ini çalıştırın: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Data Factory örnekleri oluşturmak için Azure aboneliğinde **katkıda bulunan** veya **yönetici** rolünüz olmalıdır.
* Data Factory sürüm 2 şu anda Doğu ABD, Doğu ABD2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

## <a name="create-a-linked-service"></a>Bağlı hizmet oluşturma

Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturun. Bu hızlı başlangıçta yalnızca bir Azure Depolama bağlı hizmetini örnekte "AzureStorageLinkedService" olarak adlandırılmış bir kaynak ve havuz deposu olarak kullanılmak üzere oluşturmanız gerekir.

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
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>",
                    "type": "SecureString"
                }
            }
        }
    }
    ```

2. **Azure PowerShell**’de **ADFv2QuickStartPSH** klasörüne geçin.

3. **AzureStorageLinkedService** bağlı hizmetini oluşturmak için **Set-AzureRmDataFactoryV2LinkedService** cmdlet’ini çalıştırın. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Bir kaynaktan havuza kopyalanacak verileri temsil eden bir veri kümesi tanımlayın. Bu örnekte, bu Blob veri kümesi önceki adımda oluşturduğunuz Azure Depolama bağlı hizmetini ifade eder. Veri kümesi, değeri veri kümesini kullanan bir etkinlikte tanımlanmış bir parametre alır. Parametre, verilerin bulunduğu/depolandığı yeri işaret eden **folderPath** öğesini oluşturmak için kullanılır.

1. **C:\ADFv2QuickStartPSH** klasöründe aşağıdaki içeriğe sahip **BlobDataset.json** adlı bir JSON dosyası oluşturun:

    ```json
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
    ```

2. **BlobDataset** veri kümesini oluşturmak için **Set-AzureRmDataFactoryV2Dataset** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
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
  
Bu örnekte, işlem hattı bir etkinlik içerir ve giriş blob yolu ve çıkış blob yolu şeklinde iki parametre alır. Bu parametrelerin değerleri, işlem hattı tetiklendiğinde/çalıştırıldığında ayarlanır. Kopyalama etkinliği, önceki adımda girdi ve çıktı olarak oluşturulmuş blob veri kümesini kullanır. Veri kümesi bir girdi veri kümesi olarak kullanıldığında girdi yolu belirtilir. Ayrıca, veri kümesi bir çıktı veri kümesi olarak kullanıldığında çıktı yolu belirtilir. 

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
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
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

    Farklı kapsayıcılar ve klasörler kullanıyorsanız, **inputPath** ve **outputPath** değerlerini verilerin kopyalanacağı kaynak ve havuz blob yolunuzla değiştirin.

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```

2. **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet’ini çalıştırarak bir işlem hattı çalıştırması oluşturun ve parametre değerlerini geçirin. Ayrıca, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini yakalar.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
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

2. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) almak için aşağıdaki betiği çalıştırın.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
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
    "usedCloudDataMovementUnits": 2
    "billedDuration": 14
    ```

## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı adftutorial blob kapsayıcısında çıktı klasörünü otomatik olarak oluşturur. Ardından, giriş klasöründeki emp.txt dosyasını çıktı klasörüne kopyalar. inputBlobPath içindeki blobların outputBlobPath yoluna kopyalandığından emin olmak için [Azure Depolama gezginini](https://azure.microsoft.com/features/storage-explorer/) kullanın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki şekilde temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silebilirsiniz. Diğer kaynakları korumak istiyorsanız yalnızca bu öğreticide oluşturduğunuz veri kaynağını silin.

Kaynak grubunun tamamını silmek için şu komutu çalıştırın: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Yalnızca veri fabrikası tablosunu silmek için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun. 
