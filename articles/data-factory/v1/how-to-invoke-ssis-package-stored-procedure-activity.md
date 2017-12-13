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
ms.openlocfilehash: ff0d1e5d644926864641ceb3e3c3a102167c1fd2
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Saklı yordam etkinliği Azure Data Factory kullanarak bir SSIS paketi çağırma
Bu makalede, bir saklı yordam etkinliğini kullanarak bir Azure Data Factory işlem hattı SSIS paketinden çağrılacak açıklar. 

> [!NOTE]
> Bu makale, genel olarak kullanılabilir olduğu veri fabrikası 1 sürümü için geçerlidir. Genel önizlemede olan Data Factory hizmeti 2 sürümünü kullanıyorsanız bkz [sürüm 2 saklı yordam etkinliği kullanarak çağırma SSIS paketleri](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-sql-database"></a>Azure SQL Database 
Bu makaledeki Kılavuzu SSIS katalog barındıran Azure SQL veritabanını kullanır. Bir Azure SQL yönetilen örneği (özel olarak incelenmektedir) de kullanabilirsiniz.

### <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Adım adım yönergeleri izleyerek yoksa, bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma [Öğreticisi: dağıtmak SSIS paketleri](../tutorial-deploy-ssis-packages-azure.md). Veri Fabrikası Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için 2 sürümünün oluşturmanız gerekir. 

### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki yönergeler en son Azure PowerShell modülleri yüklemek [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
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
    $df 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Data Factory sürüm 2 şu anda Doğu ABD, Doğu ABD2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Azure SQL veritabanınızı barındıran bağlamak için bağlı hizmet, veri fabrikası SSIS kataloğa oluşturun. Veri Fabrikası SSISDB veritabanına bağlanmak için bu bağlı hizmetin bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordam yürütür. 

1. Adlı bir JSON dosyası oluşturun **AzureSqlDatabaseLinkedService.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip: (zaten yoksa, ADFv2TutorialBulkCopy klasörü oluşturun.)

    > [!IMPORTANT]
    > &lt;servername&gt;, &lt;databasename&gt;, &lt;username&gt;@&lt;servername&gt; ve &lt;password&gt; sözcüklerini Azure SQL Veritabanınızın değerleriyle değiştirin.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<AZURE SQL SERVER NAME>.database.windows.net,1433;Database=SSISDB;User ID=<USERNAME>;Password=<PASSWORD>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. İçinde **Azure PowerShell**, geçiş **C:\ADF\RunSSISPackage** klasör.
3. Çalıştırma **New-AzureRmDataFactoryLinkedService** bağlı hizmetini oluşturmak için cmdlet'i: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzureRmDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

## <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
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

## <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
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
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'SsisAdfTest', @project_name=N'MyProject', @package_name=N'Package.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
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

2. Ardışık düzen oluşturmak için: **RunSSISPackagePipeline**, çalışma **kümesi AzureRmDataFactoryPipeline** cmdlet'i.

    ```powershell
    $DFPipeLine = New-AzureRmDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma
Kullanım **Invoke-AzureRmDataFactoryPipeline** ardışık düzen cmdlet'ini. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = New-AzureRmDataFactoryPipeline $df -File ".\MyPipeline.json"
```

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

2. Çalıştırma **Get-AzureRmDataFactorySlice** çıkış veri kümesi hattının çıktı tablosu olan **, tüm dilimleri hakkında bilgi almak için.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Burada belirttiğiniz StartDateTime ile JSON işlem hattında belirtilen başlangıç zamanıyla aynı olmasına özen gösterin. 
3. Belirli bir dilimle ilgili etkinlik çalıştırmalarının ayrıntılarını almak için **Get-AzureRmDataFactoryRun** komutunu çalıştırın.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Dilimi **Hazır** durumunda veya **Başarısız** durumunda görene kadar bu cmdlet’i çalışır halde tutun. Dilim Hazır durumunda olduğunda, çıktı verileri için blob depolamanızın **adfgetstarted** klasöründe **partitioneddata** klasörünü denetleyin.  İsteğe bağlı HDInsight kümesinin oluşturulması genellikle biraz zaman alır.

    Yürütülen paket doğrulamak için Azure SQL Server'da SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Sonraki adımlar
Saklı yordam etkinliği hakkında daha fazla ayrıntı için bkz: [saklı yordam etkinliği](data-factory-stored-proc-activity.md) makalesi.

