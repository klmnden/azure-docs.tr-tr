---
title: Saklı yordam etkinliği ile - Azure SSIS paketi çalıştırmak | Microsoft Docs
description: Bu makalede, saklı yordam etkinliği kullanarak bir Azure Data Factory işlem hattı, bir SQL Server Integration Services (SSIS) paketi çalıştırmak açıklar.
services: data-factory
documentationcenter: ''
author: swinarko
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: sawinark
ms.openlocfilehash: b71a954da746ba04aeaa0797c13bf2c81838179d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59256303"
---
# <a name="run-an-ssis-package-with-the-stored-procedure-activity-in-azure-data-factory"></a>Azure Data factory'de saklı yordam etkinliği ile bir SSIS paketi çalıştırma
Bu makalede bir saklı yordam etkinliği kullanarak SSIS paketi bir Azure Data Factory işlem hattında çalıştırmayı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-sql-database"></a>Azure SQL Database 
Bu makaledeki Kılavuzu, SSIS Kataloğu barındıran Azure SQL veritabanı kullanır. Azure SQL veritabanı yönetilen örneği de kullanabilirsiniz.

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Bir Azure-SSIS tümleştirme çalışma zamanı içinde adım adım yönergeleri izleyerek bir tane yoksa, oluşturma [Öğreticisi: SSIS paketlerini dağıtma](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="data-factory-ui-azure-portal"></a>Data Factory kullanıcı Arabirimi (Azure portalı)
Bu bölümde, bir SSIS paketi çağıran bir saklı yordam etkinliği bir Data Factory işlem hattı oluşturmak için Data Factory kullanıcı arabirimini kullanın.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
İlk adım, Azure portalını kullanarak veri fabrikası oluşturmaktır. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. [Azure portalına](https://portal.azure.com) gidin. 
3. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Ad yok - hata](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2**'yi seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca Data Factory tarafından desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**. 

     ![veri fabrikası dağıtılıyor kutucuğu](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
     ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma
Bu adımda, bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini kullanın. Bir saklı yordam etkinliği işlem hattının ekleyip sp_executesql saklı yordamı kullanarak SSIS paketi çalıştırmak için yapılandırabilirsiniz. 

1. Başlarken sayfasında, tıklayın **işlem hattı Oluştur**: 

    ![Başlarken sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. İçinde **etkinlikleri** araç, genişletin **genel**, sürükleyip **saklı yordam** etkinliğini işlem hattı Tasarımcı yüzeyine bırakın. 

    ![Sürükle ve bırak saklı yordam etkinliği](./media/how-to-invoke-ssis-package-stored-procedure-activity/drag-drop-sproc-activity.png)
3. Saklı yordam etkinliğinin Özellikler penceresinde geçiş **SQL hesabı** sekmesine ve tıklayın **+ yeni**. SSIS kataloğuna (SSIDB veritabanı) barındıran Azure SQL veritabanına bir bağlantı oluşturursunuz. 
   
    ![New Linked Service (Yeni bağlı hizmet) düğmesi](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-linked-service-button.png)
4. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde aşağıdaki adımları izleyin: 

    1. Seçin **Azure SQL veritabanı** için **türü**.
    2. Seçin **varsayılan** Azure Integration Runtime'ı barındıran Azure SQL veritabanına bağlanmak için `SSISDB` veritabanı.
    3. SSISDB veritabanını barındıran Azure SQL veritabanını seçin **sunucu adı** alan.
    4. Seçin **SSISDB** için **veritabanı adı**.
    5. İçin **kullanıcı adı**, veritabanına erişimi olan kullanıcının adını girin.
    6. İçin **parola**, kullanıcının parolasını girin. 
    7. Veritabanı bağlantısını tıklatarak test **Bağlantıyı Sına** düğmesi.
    8. Bağlı hizmet Kaydet'e tıklayarak **Kaydet** düğmesi. 

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-settings.png)
5. Özellikler penceresinde geçiş **saklı yordam** sekmesinden **SQL hesabı** sekmesini tıklatın ve aşağıdaki adımları uygulayın: 

    1. **Düzenle**’yi seçin. 
    2. İçin **saklı yordam adı** alan Enter `sp_executesql`. 
    3. Tıklayın **+ yeni** içinde **saklı yordam parametreleri** bölümü. 
    4. İçin **adı** parametresinin girin **stmt**. 
    5. İçin **türü** parametresinin girin **dize**. 
    6. İçin **değer** parametresi, aşağıdaki SQL sorgusunu girin:

        SQL sorgusunda doğru değerlerini belirtin **klasör_adı**, **project_name**, ve **package_name** parametreleri. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END
        ```

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/stored-procedure-settings.png)
6. İşlem hattı yapılandırmasını doğrulamak için tıklayın **doğrulama** araç. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

    ![İşlem hattını doğrulama](./media/how-to-invoke-ssis-package-stored-procedure-activity/validate-pipeline.png)
7. Tıklayarak işlem hattını Data Factory'de yayımlamak **tümünü Yayımla** düğmesi. 

    ![Yayımlama](./media/how-to-invoke-ssis-package-stored-procedure-activity/publish-all-button.png)    

### <a name="run-and-monitor-the-pipeline"></a>Çalıştırın ve işlem hattını izleme
Bu bölümde, bir işlem hattı çalıştırması tetiklemek ve daha sonra izleyin. 

1. Bir işlem hattı çalıştırması tetiklemek için tıklatın **tetikleyici** araç ve tıklatın **şimdi Tetikle**. 

    ![Şimdi tetikle](media/how-to-invoke-ssis-package-stored-procedure-activity/trigger-now.png)

2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 
3. Soldaki **İzleyici** sekmesine geçin. İşlem hattı çalıştırma ve yanı sıra diğer bilgiler (örneğin, çalıştırma başlangıç saati) durumunu görürsünüz. Görünümü yenilemek için **Yenile**’ye tıklayın.

    ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

3. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Yalnızca bir etkinlik olduğundan işlem hattı yalnızca bir etkinlik (saklı yordam etkinliği) çalıştırma görürsünüz.

    ![Etkinlik çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-runs.png)

4. Aşağıdakini çalıştırabilirsiniz. **sorgu** SSISDB karşı yürütülen paket doğrulamak için Azure SQL server veritabanı. 

    ```sql
    select * from catalog.executions
    ```

    ![Paket yürütme doğrulayın](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)


> [!NOTE]
> Böylece işlem hattını bir zamanlamaya göre (saatlik, günlük, vb.) çalıştırır, zamanlanmış bir tetikleyici için işlem hattınızı oluşturabilirsiniz. Bir örnek için bkz. [veri fabrikası - Data Factory kullanıcı Arabirimi oluşturma](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölümde, bir SSIS paketi çağıran bir saklı yordam etkinliği bir Data Factory işlem hattı oluşturmak için Azure PowerShell kullanırsınız. 

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-az-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin. 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure-SSIS IR sahip aynı data factory kullanabilir veya ayrı bir veri fabrikası oluşturma. Aşağıdaki yordam bir veri fabrikası oluşturmak için adımları sağlar. Bu veri fabrikasında bir saklı yordam etkinliği ile işlem hattı oluşturma. Saklı yordam etkinliği kullanarak SSIS paketi çalıştırmak için SSISDB veritabanı saklı yordamı yürütür. 

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın
2. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    $ResGrp = New-AzResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın. 
3. Veri fabrikasının adı için bir değişken tanımlayın. 

    > [!IMPORTANT]
    >  Veri fabrikasının adını genel olarak benzersiz olacak şekilde güncelleştirin. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Veri Fabrikası oluşturmak için aşağıdaki komutu çalıştırın. **kümesi AzDataFactoryV2** cmdlet'i $ResGrp değişkenindeki Location ve ResourceGroupName özelliğini kullanarak: 
    
    ```powershell       
    $DataFactory = Set-AzDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Barındıran Azure SQL veritabanınıza bağlamak için bağlı hizmet, SSIS Kataloğu'na veri fabrikanızı oluşturun. Data Factory SSISDB veritabanına bağlanmak için bu bağlı hizmeti bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordamı yürütür. 

1. Adlı bir JSON dosyası oluşturun **C:\adftutorials\ınccopymultitabletutorial** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip: 

    > [!IMPORTANT]
    > Değiştirin &lt;servername&gt;, &lt;kullanıcıadı&gt;, ve &lt;parola&gt; dosyayı kaydetmeden önce Azure SQL veritabanınızın değerleriyle.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            }
        }
    }
    ```

2. İçinde **Azure PowerShell**, geçiş **C:\ADF\RunSSISPackage** klasör.

3. Çalıştırma **kümesi AzDataFactoryV2LinkedService** bağlı hizmetini oluşturmak için cmdlet: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
Bu adımda, bir saklı yordam etkinliği ile işlem hattı oluşturun. Etkinlik, SSIS paketi çalıştırmak için sp_executesql depolanan yordamını çağırır. 

1. Adlı bir JSON dosyası oluşturun **RunSSISPackagePipeline.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip:

    > [!IMPORTANT]
    > Değiştirin &lt;klasör adı&gt;, &lt;proje adı&gt;, &lt;paket adı&gt; klasörü, proje ve dosyayı kaydetmeden önce SSIS Kataloğu paketinde adlarına sahip. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [
                {
                    "name": "My SProc Activity",
                    "description":"Runs an SSIS package",
                    "type": "SqlServerStoredProcedure",
                    "linkedServiceName": {
                        "referenceName": "AzureSqlDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "typeProperties": {
                        "storedProcedureName": "sp_executesql",
                        "storedProcedureParameters": {
                            "stmt": {
                                "value": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER NAME>', @project_name=N'<PROJECT NAME>', @package_name=N'<PACKAGE NAME>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                            }
                        }
                    }
                }
            ]
        }
    }
    ```

2. İşlem hattını oluşturmak için: **RunSSISPackagePipeline**çalıştırın **kümesi AzDataFactoryV2Pipeline** cmdlet'i.

    ```powershell
    $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma
Kullanım **Invoke-AzDataFactoryV2Pipeline** cmdlet'ini işlem hattını çalıştırın. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

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

### <a name="create-a-trigger"></a>Tetikleyici oluşturma
Önceki adımda, işlem hattı talep üzerine çağrılır. İşlem hattını bir zamanlamaya göre (saatlik, günlük, vb.) çalıştırmak için bir zamanlama tetikleyicisi de oluşturabilirsiniz.

1. Adlı bir JSON dosyası oluşturun **MyTrigger.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip: 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. İçinde **Azure PowerShell**, geçiş **C:\ADF\RunSSISPackage** klasör.
3. Çalıştırma **kümesi AzDataFactoryV2Trigger** cmdlet'i, bir tetikleyici oluşturur. 

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Varsayılan olarak, tetikleyici durdurulmuş durumdadır. Tetikleyiciyi çalıştırmadan **başlangıç AzDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Çalıştırarak tetikleyicinin başlatıldığını onaylayın **Get-AzDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. Sonraki saat sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saati 3: 25'te ise, 4'te komutu çalıştırın. 
    
    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    Yürütülen paket doğrulamak için Azure SQL sunucunuza SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Sonraki adımlar
Azure portalını kullanarak işlem hattını da izleyebilirsiniz. Adım adım yönergeler için bkz: [işlem hattını izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).
