---
title: Azure Data Factory - saklı yordam etkinliği kullanarak SSIS paketi çağırma | Microsoft Docs
description: Bu makalede, bir SQL Server Integration Services (SSIS) paketi saklı yordam etkinliği kullanarak bir Azure Data Factory işlem hattından çağırma açıklar.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: jingwang
ms.openlocfilehash: d61874a57801a6c02af885cab6a97ed38da1deb1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66156714"
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Saklı yordam etkinliği kullanarak Azure Data Factory'de bir SSIS paketi çağırma
Bu makalede bir saklı yordam etkinliği kullanarak bir SSIS paketi bir Azure Data Factory işlem hattından çağırma açıklar. 

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [saklı yordam etkinliği kullanarak SSIS çağırma paketlerini](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-sql-database"></a>Azure SQL Veritabanı 
Bu makaledeki Kılavuzu, SSIS Kataloğu barındıran Azure SQL veritabanı kullanır. Azure SQL veritabanı yönetilen örneği de kullanabilirsiniz.

### <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Bir Azure-SSIS tümleştirme çalışma zamanı içinde adım adım yönergeleri izleyerek bir tane yoksa, oluşturma [Öğreticisi: SSIS paketlerini dağıtma](../tutorial-create-azure-ssis-runtime-portal.md). Bir Azure-SSIS tümleştirme çalışma zamanı oluşturma, Data Factory sürüm 1'i kullanamazsınız. 

## <a name="azure-portal"></a>Azure portal
Bu bölümde bir SSIS paketi çağıran bir saklı yordam etkinliği bir Data Factory işlem hattı oluşturmak için Azure portalını kullanın.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
İlk adım, Azure portalını kullanarak veri fabrikası oluşturmaktır. 

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
8. Panoda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**. 

     ![veri fabrikası dağıtılıyor kutucuğu](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
     ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Tıklayın **yazar ve dağıtma** Data Factory Düzenleyicisi'ni başlatmak için.

    ![Data Factory Düzenleyicisi](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-editor.png)

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Barındıran Azure SQL veritabanınıza bağlamak için bağlı hizmet, SSIS Kataloğu'na veri fabrikanızı oluşturun. Data Factory SSISDB veritabanına bağlanmak için bu bağlı hizmeti bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordamı yürütür. 

1. Data Factory Düzenleyicisi'nde tıklayın **yeni veri deposu** menüsüne ve ardından şirket **Azure SQL veritabanı**. 

    ![Yeni veri deposu -> Azure SQL veritabanı](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-sql-database-linked-service-menu.png)
2. Sağ bölmede, aşağıdaki adımları uygulayın:

    1. Değiştirin `<servername>` Azure SQL sunucunuzun adı ile. 
    2. Değiştirin `<databasename>` ile **SSISDB** (SSIS Kataloğu veritabanını adı). 
    3. Değiştirin `<username@servername>` Azure SQL Server'a erişim olan kullanıcı adı. 
    4. Değiştirin `<password>` kullanıcı parolası ile. 
    5. Tıklayarak bağlı hizmeti dağıtmak **Dağıt** araç çubuğunda. 

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-definition.png)

### <a name="create-a-dummy-dataset-for-output"></a>Çıkış için işlevsiz bir veri kümesi oluşturma
Bu çıktı veri kümesi işlem hattının zamanlamasını sürücüleri işlevsiz bir veri kümesi var. Sıklık saat olarak ayarlanır ve aralık 1 olarak ayarlanmış olduğuna dikkat edin. Bu nedenle, bir saat içinde bir işlem hattı başlangıç ve bitiş zamanlarını sonra işlem hattını çalışır. 

1. Data Factory Düzenleyicisi'nin sol bölmesinde, tıklatın **... Daha fazla** -> **yeni veri kümesi** -> **Azure SQL**.

    ![Daha fazla yeni veri kümesi ->](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-dataset-menu.png)
2. Aşağıdaki JSON kod parçacığında, JSON Düzenleyicisi'nde sağ bölmeye kopyalayın. 
    
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
3. Araç çubuğunda **Dağıt**’a tıklayın. Bu eylem, Azure Data Factory hizmetine veri kümesi dağıtır. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
Bu adımda, bir saklı yordam etkinliği ile işlem hattı oluşturun. Etkinlik, SSIS paketi çalıştırmak için sp_executesql depolanan yordamını çağırır. 

1. Sol bölmede **... Daha fazla** ve **Yeni işlem hattı** öğelerine tıklayın.
2. Aşağıdaki JSON kod parçacığında, JSON düzenleyicisine kopyalayın: 

    > [!IMPORTANT]
    > Değiştirin &lt;klasör adı&gt;, &lt;proje adı&gt;, &lt;paket adı&gt; klasörü, proje ve dosyayı kaydetmeden önce SSIS Kataloğu paketinde adlarına sahip.

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
3. Araç çubuğunda **Dağıt**’a tıklayın. Bu eylem, işlem hattı için Azure Data Factory hizmetinin dağıtır. 

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme
Zamanlama çıktı veri kümesi üzerinde saatlik olarak tanımlanır. İşlem hattı bitiş zamanı başlangıç zamanından itibaren beş saattir. Bu nedenle, beş işlem hattı çalıştırması görürsünüz. 

1. Veri fabrikasının giriş sayfasını görürsünüz, düzenleyici pencerelerini kapatın. Tıklayın **izleme ve yönetme** Döşe. 

    ![Diyagram kutucuğu](./media/how-to-invoke-ssis-package-stored-procedure-activity/monitor-manage-tile.png)
2. Güncelleştirme **başlangıç zamanı** ve **bitiş saati** için **18/01/2018 08:30 AM** ve **20/01/2018 08:30 AM**, tıklatıp **Uygula**. Görmelisiniz **etkinlik pencereleri** işlem hattı çalıştırması ile ilişkili. 

    ![Etkinlik pencereleri](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-windows.png)

İşlem hatlarını izleme hakkında daha fazla bilgi için bkz. [İzleyicisi ve izleme ve yönetim uygulaması kullanarak Azure Data Factory işlem hatlarını yönetmek](data-factory-monitor-manage-app.md).

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde bir SSIS paketi çağıran bir saklı yordam etkinliği bir Data Factory işlem hattı oluşturmak için Azure PowerShell kullanırsınız.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-az-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Aşağıdaki yordam bir veri fabrikası oluşturmak için adımları sağlar. Bu veri fabrikasında bir saklı yordam etkinliği ile işlem hattı oluşturma. Saklı yordam etkinliği kullanarak SSIS paketi çalıştırmak için SSISDB veritabanı saklı yordamı yürütür.

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
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

5. Veri Fabrikası oluşturmak için aşağıdaki komutu çalıştırın. **yeni AzDataFactory** cmdlet'i $ResGrp değişkenindeki Location ve ResourceGroupName özelliğini kullanarak: 
    
    ```powershell       
    $df = New-AzDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
Barındıran Azure SQL veritabanınıza bağlamak için bağlı hizmet, SSIS Kataloğu'na veri fabrikanızı oluşturun. Data Factory SSISDB veritabanına bağlanmak için bu bağlı hizmeti bilgileri kullanır ve bir SSIS paketi çalıştırmak için bir saklı yordamı yürütür. 

1. Adlı bir JSON dosyası oluşturun **C:\adftutorials\ınccopymultitabletutorial** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip: 

    > [!IMPORTANT]
    > Değiştirin &lt;servername&gt;, &lt;kullanıcıadı&gt;@&lt;servername&gt; ve &lt;parola&gt; önce Azure SQL veritabanınızın değerleriyle Dosya kaydediliyor.

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
3. Çalıştırma **yeni AzDataFactoryLinkedService** bağlı hizmetini oluşturmak için cmdlet: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Bu çıktı veri kümesi işlem hattının zamanlamasını sürücüleri işlevsiz bir veri kümesi var. Sıklık saat olarak ayarlanır ve aralık 1 olarak ayarlanmış olduğuna dikkat edin. Bu nedenle, bir saat içinde bir işlem hattı başlangıç ve bitiş zamanlarını sonra işlem hattını çalışır. 

1. Aşağıdaki içerikle OutputDataset.json dosyası oluşturun: 
    
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
2. Çalıştırma **yeni AzDataFactoryDataset** bir veri kümesini oluşturmak için cmdlet'i. 

    ```powershell
    New-AzDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Saklı yordam etkinliği ile işlem hattı oluşturma 
Bu adımda, bir saklı yordam etkinliği ile işlem hattı oluşturun. Etkinlik, SSIS paketi çalıştırmak için sp_executesql depolanan yordamını çağırır. 

1. Adlı bir JSON dosyası oluşturun **MyPipeline.json** içinde **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriğe sahip:

    > [!IMPORTANT]
    > Değiştirin &lt;klasör adı&gt;, &lt;proje adı&gt;, &lt;paket adı&gt; klasörü, proje ve dosyayı kaydetmeden önce SSIS Kataloğu paketinde adlarına sahip.

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

2. İşlem hattını oluşturmak için: **RunSSISPackagePipeline**çalıştırın **yeni AzDataFactoryPipeline** cmdlet'i.

    ```powershell
    $DFPipeLine = New-AzDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. Çalıştırma **Get-AzDataFactorySlice** çıkış veri kümesi işlem hattının çıktı tablosu olan **, tüm dilimleri hakkında bilgi almak için.

    ```powershell
    Get-AzDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Burada belirttiğiniz StartDateTime ile JSON işlem hattında belirtilen başlangıç zamanıyla aynı olmasına özen gösterin. 
1. Çalıştırma **Get-AzDataFactoryRun** ayrıntılarını almak için belirli bir dilimle ilgili etkinlik çalışır.

    ```powershell
    Get-AzDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Dilimi **Hazır** durumunda veya **Başarısız** durumunda görene kadar bu cmdlet’i çalışır halde tutun. 

    Yürütülen paket doğrulamak için Azure SQL sunucunuza SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Sonraki adımlar
Saklı yordam etkinliği hakkında daha fazla ayrıntı için bkz: [saklı yordam etkinliği](data-factory-stored-proc-activity.md) makalesi.

