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
ms.date: 01/19/2018
ms.author: jingwang
ms.openlocfilehash: 99e3365a846f35262489fdccd753b4ce2e50fa49
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Saklı yordam etkinliği Azure Data Factory kullanarak bir SSIS paketi çağırma
Bu makalede, bir saklı yordam etkinliğini kullanarak bir Azure Data Factory işlem hattı SSIS paketinden çağrılacak açıklar. 

> [!NOTE]
> Bu makale, genel olarak kullanılabilir olduğu veri fabrikası 1 sürümü için geçerlidir. Genel önizlemede olan Data Factory hizmeti 2 sürümünü kullanıyorsanız bkz [sürüm 2 saklı yordam etkinliği kullanarak çağırma SSIS paketleri](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-sql-database"></a>Azure SQL Database 
Bu makaledeki Kılavuzu SSIS katalog barındıran Azure SQL veritabanını kullanır. Bir Azure SQL yönetilen örneği (özel olarak incelenmektedir) de kullanabilirsiniz.

### <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Adım adım yönergeleri izleyerek yoksa, bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma [Öğreticisi: dağıtmak SSIS paketleri](../tutorial-create-azure-ssis-runtime-portal.md). Veri Fabrikası Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için 2 sürümünün oluşturmanız gerekir. 

## <a name="azure-portal"></a>Azure portalına
Bu bölümde bir SSIS paketi çağıran bir saklı yordam etkinliği ile bir Data Factory işlem hattı oluşturmak için Azure Portalı'nı kullanın.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure Portalı'nı kullanarak data factory oluşturmak ilk adımdır. 

1. [Azure portalına](https://portal.azure.com) gidin. 
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) makalesine bakın.

    `Data factory name ADFTutorialDataFactory is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../../azure-resource-manager/resource-group-overview.md).  
4. Seçin **V1** için **sürüm**.
5. Data factory için **konum** seçin. Açılan listede yalnızca Data Factory tarafından desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
    ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Tıklatın **yazar ve dağıtma** döşeme Data Factory Düzenleyici başlatın.

    ![Data Factory Düzenleyicisi](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-editor.png)

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Azure SQL veritabanınızı barındıran bağlamak için bağlı hizmet, veri fabrikası SSIS kataloğa oluşturun. Veri Fabrikası SSISDB veritabanına bağlanmak için bu bağlı hizmetin bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordam yürütür. 

1. Data Factory Düzenleyici'yi tıklatın **yeni veri deposu** menüsüne ve ardından üzerinde **Azure SQL veritabanı**. 

    ![Yeni veri deposu -> Azure SQL veritabanı](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-sql-database-linked-service-menu.png)
2. Sağ bölmede, aşağıdaki adımları uygulayın:

    1. Değiştir `<servername>` Azure SQL sunucunuzun adı ile. 
    2. Değiştir `<databasename>` ile **SSISDB** (SSIS Katalog veritabanı adı). 
    3. Değiştir `<username@servername>` Azure SQL sunucusuna erişimi olan kullanıcı adı. 
    4. Değiştir `<password>` kullanıcı için parola ile. 
    5. Tıklayarak bağlı hizmeti dağıtmak **dağıtma** araç çubuğunda. 

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-definition.png)

### <a name="create-a-dummy-dataset-for-output"></a>Çıktı için sahte bir veri kümesi oluşturma
Bu çıktı veri kümesi zamanlamayı ardışık sürücüleri kukla bir veri kümesidir. Sıklık saat ve aralığı 1 olarak ayarlandığını görebilirsiniz. Bu nedenle, bir saat içinde ardışık düzen başlangıç ve bitiş saatlerini sonra ardışık düzen çalışır. 

1. Veri Fabrikası Düzenleyicisi'nin sol bölmesinde, **... Daha fazla** -> **yeni veri kümesi** -> **Azure SQL**.

    ![Daha fazla yeni veri kümesi ->](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-dataset-menu.png)
2. Aşağıdaki JSON parçacığı sağ bölmedeki JSON Düzenleyicisi'ni kopyalayın. 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. Araç çubuğunda **Dağıt**’a tıklayın. Bu eylem Azure Data Factory hizmetine veri kümesi dağıtır. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
Bu adımda, bir saklı yordam etkinliği ile işlem hattı oluşturun. SSIS paketi çalıştırmak için sp_executesql saklı yordam etkinliği çağırır. 

1. Sol bölmede **... Daha fazla** ve **Yeni işlem hattı** öğelerine tıklayın.
2. Aşağıdaki JSON parçacığı JSON düzenleyicisine kopyalayın: 

    > [!IMPORTANT]
    > Değiştir &lt;klasör adı&gt;, &lt;proje adı&gt;, &lt;paket adı&gt; klasörü, proje ve dosyayı kaydetmeden önce SSIS katalog paketinde adlarıyla.

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2018-01-19T00:00:00Z",
            "end": "2018-01-19T05:00:00Z",
            "isPaused": false
        }
    }    
    ```
3. Araç çubuğunda **Dağıt**’a tıklayın. Bu eylem Azure Data Factory hizmetine ardışık dağıtır. 

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme
Çıktı veri kümesi zamanlamada saatlik olarak tanımlanır. Ardışık Düzen bitiş zamanı başlangıç zamanından itibaren beş saattir. Bu nedenle, beş ardışık düzen çalıştırır bakın. 

1. Data factory giriş sayfasını görebilmesi için düzenleyici pencerelerini kapatın. Tıklatın **İzleyici & Yönet** döşeme. 

    ![Diyagram kutucuğu](./media/how-to-invoke-ssis-package-stored-procedure-activity/monitor-manage-tile.png)
2. Güncelleştirme **başlangıç zamanı** ve **bitiş saati** için **18/01/2018 08: 30'da** ve **20/01/2018 08: 30'da**, tıklatıp **Uygula**. Görmeniz gerekir **etkinlik windows** çalıştırmak ardışık düzen ile ilişkilendirilmiş. 

    ![Etkinlik windows](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-windows.png)

İşlem hatlarını izleme hakkında daha fazla bilgi için bkz: [İzleyicisi ve izleme ve yönetim uygulaması kullanarak Azure Data Factory işlem hatlarını yönetme](data-factory-monitor-manage-app.md).

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde bir SSIS paketi çağıran bir saklı yordam etkinliği ile bir Data Factory işlem hattı oluşturmak için Azure PowerShell kullanın.

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Aşağıdaki yordam, bir data factory oluşturmak için adımları sağlar. Bu veri fabrikasında bir saklı yordam etkinliği ile işlem hattı oluşturun. Saklı yordam etkinliği bir saklı yordam SSIS paketi çalıştırmak için SSISDB veritabanı yürütür.

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
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

5. Data factory oluşturmak için aşağıdaki komutu çalıştırın **New-AzureRmDataFactory** $ResGrp değişkeni konumu ve ResourceGroupName özelliğinden kullanarak cmdlet: 
    
    ```powershell       
    $df = New-AzureRmDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Azure SQL veritabanınızı barındıran bağlamak için bağlı hizmet, veri fabrikası SSIS kataloğa oluşturun. Veri Fabrikası SSISDB veritabanına bağlanmak için bu bağlı hizmetin bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordam yürütür. 

1. Adlı bir JSON dosyası oluşturun **AzureSqlDatabaseLinkedService.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip: 

    > [!IMPORTANT]
    > Değiştir &lt;servername&gt;, &lt;kullanıcıadı&gt;@&lt;servername&gt; ve &lt;parola&gt; önce Azure SQL veritabanınızın değerlerle dosyayı kaydetme.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. İçinde **Azure PowerShell**, geçiş **C:\ADF\RunSSISPackage** klasör.
3. Çalıştırma **New-AzureRmDataFactoryLinkedService** bağlı hizmetini oluşturmak için cmdlet'i: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzureRmDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Bu çıktı veri kümesi zamanlamayı ardışık sürücüleri kukla bir veri kümesidir. Sıklık saat ve aralığı 1 olarak ayarlandığını görebilirsiniz. Bu nedenle, bir saat içinde ardışık düzen başlangıç ve bitiş saatlerini sonra ardışık düzen çalışır. 

1. Aşağıdaki içerik ile bir OuputDataset.json dosyası oluşturun: 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. Çalıştırma **yeni AzureRmDataFactoryDataset** bir veri kümesi oluşturmak için cmdlet'i. 

    ```powershell
    New-AzureRmDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
Bu adımda, bir saklı yordam etkinliği ile işlem hattı oluşturun. SSIS paketi çalıştırmak için sp_executesql saklı yordam etkinliği çağırır. 

1. Adlı bir JSON dosyası oluşturun **MyPipeline.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip:

    > [!IMPORTANT]
    > Değiştir &lt;klasör adı&gt;, &lt;proje adı&gt;, &lt;paket adı&gt; klasörü, proje ve dosyayı kaydetmeden önce SSIS katalog paketinde adlarıyla.

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. Ardışık düzen oluşturmak için: **RunSSISPackagePipeline**, çalışma **yeni AzureRmDataFactoryPipeline** cmdlet'i.

    ```powershell
    $DFPipeLine = New-AzureRmDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

2. Çalıştırma **Get-AzureRmDataFactorySlice** çıkış veri kümesi hattının çıktı tablosu olan **, tüm dilimleri hakkında bilgi almak için.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Burada belirttiğiniz StartDateTime ile JSON işlem hattında belirtilen başlangıç zamanıyla aynı olmasına özen gösterin. 
3. Belirli bir dilimle ilgili etkinlik çalıştırmalarının ayrıntılarını almak için **Get-AzureRmDataFactoryRun** komutunu çalıştırın.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Dilimi **Hazır** durumunda veya **Başarısız** durumunda görene kadar bu cmdlet’i çalışır halde tutun. 

    Yürütülen paket doğrulamak için Azure SQL Server'da SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Sonraki adımlar
Saklı yordam etkinliği hakkında daha fazla ayrıntı için bkz: [saklı yordam etkinliği](data-factory-stored-proc-activity.md) makalesi.

