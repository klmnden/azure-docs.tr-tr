---
title: "Azure Data Factory - saklı yordam etkinliği kullanarak SSIS paketi çağırma | Microsoft Docs"
description: "Bu makalede, bir SQL Server Integration Services (SSIS) paketi saklı yordam etkinliği kullanarak bir Azure Data Factory işlem hattı çağrılacak açıklar."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: 749deb6549e0ac90da4b44424026c897108a4bb7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Saklı yordam etkinliği Azure Data Factory kullanarak bir SSIS paketi çağırma
Bu makalede, bir saklı yordam etkinliğini kullanarak bir Azure Data Factory işlem hattı SSIS paketinden çağrılacak açıklar. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [saklı yordam etkinliği sürüm 1 kullanılarak çağırma SSIS paketleri](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-sql-database"></a>Azure SQL Database 
Bu makaledeki Kılavuzu SSIS katalog barındıran Azure SQL veritabanını kullanır. Bir Azure SQL yönetilen örneği (özel olarak incelenmektedir) de kullanabilirsiniz.

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Adım adım yönergeleri izleyerek yoksa, bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma [Öğreticisi: dağıtmak SSIS paketleri](tutorial-deploy-ssis-packages-azure.md).

## <a name="data-factory-ui-azure-portal"></a>Veri Fabrikası kullanıcı Arabirimi (Azure portalı)
Bu bölümde bir SSIS paketi çağıran bir saklı yordam etkinliği ile bir Data Factory işlem hattı oluşturmak için veri fabrikası UI kullanın.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure Portalı'nı kullanarak data factory oluşturmak ilk adımdır. 

1. [Azure portalına](https://portal.azure.com) gidin. 
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** want **ADFTutorialDataFactory** için **adı**. 
      
     ![Yeni veri fabrikası sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için şu hatayı görürseniz, veri fabrikası (örneğin, yournameADFTutorialDataFactory) adını değiştirin. Bkz: [Data Factory - adlandırma kurallarını](naming-rules.md) Data Factory yapıtlarının adlandırma kuralları için makale.
  
     ![Ad kullanılamıyor - hata](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Data Factory ile desteklenen konumlar aşağı açılan listesinde gösterilir. Verileri depolar (Azure Storage, Azure SQL Database, vb.) ve veri fabrikası tarafından kullanılan hesaplar (Hdınsight, vb.) başka bir konumda olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra gördüğünüz **Data Factory** görüntüde gösterildiği gibi sayfa.
   
    ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Tıklatın **Yazar & İzleyici** ayrı bir sekmede Azure Data Factory kullanıcı arabirimi (UI) uygulamayı başlatmak için döşeme. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma
Bu adımda, bir işlem hattı oluşturmak için veri fabrikası kullanıcı arabirimini kullanın. Saklı yordam etkinliği ardışık düzene ekleyip SSIS paketi sp_executesql saklı yordamı kullanarak çalıştırmak için yapılandırabilirsiniz. 

1. Get başlangıç sayfasını tıklatın **oluşturma ardışık düzen**: 

    ![Başlangıç sayfasına Al](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. İçinde **etkinlikleri** araç kutusu, genişletin **SQL veritabanı**ve sürükle ve bırak **saklı yordam** ardışık düzen desginer yüzeye etkinlik. 

    ![Sürükle ve bırak saklı yordam etkinliği](./media/how-to-invoke-ssis-package-stored-procedure-activity/drag-drop-sproc-activity.png)
3. Saklı yordam etkinliği Özellikleri penceresinde geçmek **SQL hesabı** sekmesine ve tıklayın **+ yeni**. SSIS katalog (SSIDB veritabanı) barındıran Azure SQL veritabanına bağlantı oluşturun. 
   
    ![Yeni bağlı hizmet düğmesi](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-linked-service-button.png)
4. İçinde **yeni bağlı hizmet** penceresinde, aşağıdaki adımları uygulayın: 

    1. Seçin **Azure SQL veritabanı** için **türü**.
    2. SSISDB veritabanını barındıran Azure SQL sunucunuzu seçin **sunucu adı** alan.
    3. Seçin **SSISDB** için **veritabanı adı**.
    4. İçin **kullanıcı adı**, veritabanına erişimi olan kullanıcı adını girin.
    5. İçin **parola**, kullanıcı parolasını girin. 
    6. Veritabanı bağlantısı tıklayarak test **Bağlantıyı Sına** düğmesi.
    7. Bağlantılı hizmet tıklayarak Kaydet **kaydetmek** düğmesi. 

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-settings.png)
5. Özellikler penceresinde geçiş **saklı yordam** gelen sekmesinde **SQL hesabı** sekmesini tıklatın ve aşağıdaki adımları uygulayın: 

    1. İçin **saklı yordam adı** alan, Enter `sp_executesql` . 
    2. Tıklatın **+ yeni** içinde **saklı yordam parametreleri** bölümü. 
    3. İçin **adı** parametresinin girin **stmt**. 
    4. İçin **türü** parametresinin girin **dize** . 
    5. İçin **değeri** parametresi, aşağıdaki SQL sorgusunu girin.

        SQL sorgusu için doğru değerleri belirtin **klasör_adı**, **project_name**, ve **paket_adı** parametreleri. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END
        ```

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/stored-procedure-settings.png)
6. Ardışık Düzen yapılandırmasını doğrulamak için tıklatın **doğrulama** araç çubuğunda. Kapatmak için **ardışık düzen doğrulama raporu**, tıklatın  **>>** .

    ![Ardışık Düzen doğrula](./media/how-to-invoke-ssis-package-stored-procedure-activity/validate-pipeline.png)
7. Tıklayarak Data Factory yayımlama kanalı **tümünü Yayımla** düğmesi. 

    ![Yayımlama](./media/how-to-invoke-ssis-package-stored-procedure-activity/publish-all-button.png)    

### <a name="run-and-monitor-the-pipeline"></a>Çalıştırın ve işlem hattını izleme
Bu bölümde bir ardışık düzen çalıştırma tetikler ve ardından izleyebilirsiniz. 

1. Çalıştıran bir ardışık düzen tetiklemek için tıklatın **tetikleyici** araç ve tıklatın **şimdi tetikleyebilir**. 

    ![Şimdi tetikleyebilir](./media/how-to-invoke-ssis-package-stored-procedure-activity/trigger-now.png)
2. Geçiş **İzleyici** sol sekmesinde. Çalıştırma ardışık düzen ve durumunu (örneğin, çalıştırma başlangıç saati) diğer bilgilerle birlikte bakın. Görünümü yenilemek için tıklatın **yenileme**.

    ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)
3. Tıklatın **görünüm etkinlik çalışması** bağlamak **Eylemler** sütun. Ardışık Düzen yalnızca bir etkinlik (saklı yordam etkinliği) sahip farklı çalıştır yalnızca bir etkinlik bakın.

    ![Etkinlik çalışması](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-runs.png) aşağıdaki çalıştırabilirsiniz 4 **sorgu** SSISDB karşı yürütülen paket doğrulamak için Azure SQL server veritabanı. 

    ```sql
    select * from catalog.executions
    ```

    ![Paket yürütmeleri doğrulayın](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

Ardışık Düzen (houly, günlük, vs.) zamanlamaya göre çalışır, zamanlanmış bir tetikleyici için işlem hattınızı oluşturabilirsiniz. Bir örnek için bkz: [data factory - Data Factory UI oluşturma](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde bir SSIS paketi çağıran bir saklı yordam etkinliği ile bir Data Factory işlem hattı oluşturmak için Azure PowerShell kullanın. 

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin. 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure SSIS IR sahip aynı veri fabrikası kullanın veya ayrı veri fabrikası oluşturun. Aşağıdaki yordam, bir data factory oluşturmak için adımları sağlar. Bu veri fabrikasında bir saklı yordam etkinliği ile işlem hattı oluşturun. Saklı yordam etkinliği bir saklı yordam SSIS paketi çalıştırmak için SSISDB veritabanı yürütür. 

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın
2. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın. 
3. Veri fabrikasının adı için bir değişken tanımlayın. 

    > [!IMPORTANT]
    >  Veri fabrikasının adını genel olarak benzersiz olacak şekilde güncelleştirin. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
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
* Data Factory sürüm 2 şu anda Doğu ABD, Doğu ABD2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Azure SQL veritabanınızı barındıran bağlamak için bağlı hizmet, veri fabrikası SSIS kataloğa oluşturun. Veri Fabrikası SSISDB veritabanına bağlanmak için bu bağlı hizmetin bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordam yürütür. 

1. Adlı bir JSON dosyası oluşturun **AzureSqlDatabaseLinkedService.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip: 

    > [!IMPORTANT]
    > Değiştir &lt;servername&gt;, &lt;kullanıcıadı&gt;, ve &lt;parola&gt; dosyayı kaydetmeden önce Azure SQL veritabanınıza değerlere sahip.

    ```json
    {
        "name": "AzureSqlDbLinkedService",
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

3. **AzureSqlDatabaseLinkedService** bağlı hizmetini oluşturmak için **Set-AzureRmDataFactoryV2LinkedService** cmdlet’ini çalıştırın. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
Bu adımda, bir saklı yordam etkinliği ile işlem hattı oluşturun. SSIS paketi çalıştırmak için sp_executesql saklı yordam etkinliği çağırır. 

1. Adlı bir JSON dosyası oluşturun **RunSSISPackagePipeline.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip:

    > [!IMPORTANT]
    > Değiştir &lt;klasör adı&gt;, &lt;proje adı&gt;, &lt;paket adı&gt; klasörü, proje ve dosyayı kaydetmeden önce SSIS katalog paketinde adlarıyla. 

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
                        "referenceName": "AzureSqlDbLinkedService",
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

2. Ardışık düzen oluşturmak için: **RunSSISPackagePipeline**, çalışma **kümesi AzureRmDataFactoryV2Pipeline** cmdlet'i.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
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
Kullanım **Invoke-AzureRmDataFactoryV2Pipeline** ardışık düzen cmdlet'ini. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

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

### <a name="create-a-trigger"></a>Bir Tetikleyici oluşturma
Önceki adımda, ardışık düzen isteğe bağlı çağrılır. Ardışık Düzen (saatlik, günlük, vs.) zamanlamaya göre çalıştırmak için bir zamanlama tetikleyici de oluşturabilirsiniz.

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
3. Çalıştırma **kümesi AzureRmDataFactoryV2Trigger** tetikleyici oluşturmak için cmdlet'i. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Varsayılan olarak, tetikleyici durdurulmuş durumda. Tetikleyici çalıştırırsınız **başlangıç AzureRmDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Çalıştırarak, tetikleyici başlatıldığını onaylayın **Get-AzureRmDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. Sonraki saat sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saati UTC saat 15: 25'e ise, 4 PM UTC komutunu çalıştırın. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    Yürütülen paket doğrulamak için Azure SQL Server'da SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Sonraki adımlar
Azure portalını kullanarak ardışık düzeni da izleyebilirsiniz. Adım adım yönergeler için bkz: [işlem hattını izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).
