---
title: "Azure Data Factory ile verileri Blob Depolama’ya kopyalama | Microsoft Docs"
description: "Aynı Azure Blob Depolama alanındaki bir klasörden başka bir klasöre veri kopyalamak için bir Azure veri fabrikası oluşturun."
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
ms.date: 11/16/2017
ms.author: jingwang
ms.openlocfilehash: 254dcb6642afc19f434df837c9073d2dd7314313
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="create-an-azure-data-factory-using-powershell"></a>PowerShell kullanarak Azure veri fabrikası oluşturma 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](quickstart-create-data-factory-powershell.md)

Bu hızlı başlangıç, PowerShell kullanarak bir Azure veri fabrikası oluşturma işlemini açıklar. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure blob depolama alanındaki bir klasörden başka bir klasöre **kopyalar**. Azure Data Factory kullanarak verileri **dönüştürme** hakkında bir öğretici için bkz. [Öğretici: Spark kullanarak verileri dönüştürme](transform-data-using-spark.md). 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.
>
> Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md).

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

### <a name="azure-roles"></a>Azure rolleri
Data Factory örnekleri oluşturmak için, Azure'da oturum açarken kullandığınız kullanıcı hesabı **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır. Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalında sağ üst köşedeki **kullanıcı adınıza** tıklayın ve **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa doğru aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için [Rol ekleme](../billing/billing-add-change-azure-subscription-administrator.md) makalesine bakın.

### <a name="azure-storage-account"></a>Azure Depolama Hesabı
Bu hızlı başlangıçta, genel amaçlı Azure Depolama Hesabı'nı (özel olarak Blob Depolama) hem **kaynak** hem de **hedef** veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). 

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
Bu bölümde, Azure blob depolamanızda **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz. Ardından, kapsayıcıda **giriş** adlı bir klasör oluşturur ve giriş klasörüne örnek bir dosya yüklersiniz. 

1. **Depolama hesabı** sayfasında **Genel Bakış**’a geçin ve sonra **Bloblar**’a tıklayın. 

    ![Bloblar seçeneğini belirleyin](media/quickstart-create-data-factory-powershell/select-blobs.png)
2. **Blob hizmeti** sayfasında, araç çubuğundaki **+ Kapsayıcı**’ya tıklayın. 

    ![Kapsayıcı ekle düğmesi](media/quickstart-create-data-factory-powershell/add-container-button.png)    
3. **Yeni kapsayıcı** iletişim kutusuna ad olarak **adftutorial** girin ve **Tamam**’a tıklayın. 

    ![Kapsayıcı adı girme](media/quickstart-create-data-factory-powershell/new-container-dialog.png)
4. Kapsayıcılar listesinde **adftutorial** öğesine tıklayın. 

    ![Kapsayıcı seçme](media/quickstart-create-data-factory-powershell/seelct-adftutorial-container.png)
1. **Kapsayıcı** sayfasında araç çubuğundaki **Karşıya Yükle** öğesine tıklayın.  

    ![Karşıya yükle düğmesi](media/quickstart-create-data-factory-powershell/upload-toolbar-button.png)
6. **Blobu karşıya yükleme** sayfasında **Gelişmiş**’e tıklayın.

    ![Gelişmiş bağlantısına tıklayın](media/quickstart-create-data-factory-powershell/upload-blob-advanced.png)
7. **Not Defteri**’ni başlatın ve şu içeriğe sahip **emp.txt** adlı bir dosya oluşturun: **c:\ADFv2QuickStartPSH** klasörüne kaydedin: Henüz yoksa **ADFv2QuickStartPSH** klasörünü oluşturun.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. Azure portalında **Blobu karşıya yükleme** sayfasından **Dosyalar** alanı için **emp.txt** dosyasına göz atıp seçin. 
9. **Klasöre yükle** değeri olarak **input** girin. 

    ![Blobu karşıya yükleme ayarları](media/quickstart-create-data-factory-powershell/upload-blob-settings.png)    
10. Klasörün **input**, dosyanın ise **emp.txt** olduğunu onaylayıp **Karşıya Yükle**’ye tıklayın.
11. Listede **emp.txt** dosyasını ve karşıya yükleme durumunu görmeniz gerekir. 
12. Köşedeki **X** simgesine tıklayarak **Blobu karşıya yükleme** sayfasını kapatın. 

    ![Blobu karşıya yükleme sayfasını kapatma](media/quickstart-create-data-factory-powershell/close-upload-blob.png)
1. **Kapsayıcı** sayfasını açık tutun. Bu hızlı başlangıcın sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
En son Azure PowerShell makinenizde yüklü değilse, yükleyin. 

1. Web tarayıcısında [Azure SDK İndirmeleri ve SDK'lar](https://azure.microsoft.com/downloads/) sayfasına gidin. 
2. **Komut satırı araçları** -> **PowerShell** bölümünde **Windows yüklemesi**'ne tıklayın. 
3. Azure PowerShell'i yüklemek için **MSI** dosyasını çalıştırın. 

Ayrıntılı yönergeler için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Azure PowerShell'de oturum açma

1. Makinenizde **PowerShell**'i başlatın. Bu hızlı başlangıcın sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.

    ![PowerShell’i başlatın](media/quickstart-create-data-factory-powershell/search-powershell.png)
1. Aşağıdaki komutu çalıştırın ve Azure Portal'da oturum açmak için kullandığınız aynı Azure kullanıcı adını ve parolasını girin:
       
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
1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`.
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Veri fabrikasının adı için bir değişken tanımlayın. 

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
    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$resourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın. 
5. Veri fabrikası oluşturmak için aşağıdaki **Set-AzureRmDataFactoryV2** cmdlet’ini çalıştırın: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure'da oturum açarken kullandığınız kullanıcı hesabı **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Data Factory sürüm 2 şu anda Doğu ABD, Doğu ABD2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

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
Bu adımda, bir kaynaktan havuza kopyalanacak verileri temsil eden bir veri kümesi tanımlarsınız. Veri kümesi **AzureBlob** türündedir. Önceki adımda oluşturduğunuz **Azure Depolama bağlı hizmetini** ifade eder. **folderPath** özelliğini oluşturmak için bir parametre alır. Bir giriş veri kümesi için işlem hattındaki kopyalama etkinliği bu parametrenin değeri olarak giriş yolunu geçirir. Benzer şekilde, bir çıkış veri kümesi için kopyalama etkinliği bu parametrenin değeri olarak çıkış yolunu geçirir. 

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

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```
2. **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet’ini çalıştırarak bir işlem hattı çalıştırması oluşturun ve parametre değerlerini geçirin. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

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
İşlem hattı adftutorial blob kapsayıcısında çıktı klasörünü otomatik olarak oluşturur. Ardından, giriş klasöründeki emp.txt dosyasını çıktı klasörüne kopyalar. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı sayfasında **Yenile**’ye tıklayın. 
    
    ![Yenile](media/quickstart-create-data-factory-powershell/output-refresh.png)
2. Klasör listesinde **output** öğesine tıklayın. 
2. **emp.txt** dosyasının output klasörüne kopyalandığını onaylayın. 

    ![Yenile](media/quickstart-create-data-factory-powershell/output-file.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki şekilde temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silebilirsiniz. Diğer kaynakları korumak istiyorsanız yalnızca bu öğreticide oluşturduğunuz veri kaynağını silin.

Bir kaynak grubunun silinmesi, içindeki veri fabrikaları dahil olmak üzere tüm kaynakları siler. Kaynak grubunun tamamını silmek için şu komutu çalıştırın: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Tüm kaynak grubu yerine yalnızca veri fabrikasını silmek istiyorsanız aşağıdaki aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun. 
