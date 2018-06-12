---
title: SSIS paketi yürütme SSIS paketi etkinlikle - Azure çalıştırmak | Microsoft Docs
description: Bu makalede, SSIS paketi yürütme etkinliğini kullanarak bir Azure Data Factory ardışık düzeninde bir SQL Server Integration Services (SSIS) paketi çalıştırmak açıklar.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: douglasl
ms.openlocfilehash: ce041813d52e645c336869ef04c9522962c80cf5
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297164"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>SSIS paketi yürütme etkinliği Azure Data factory'de bir SSIS paketi çalıştırın
Bu makalede, bir SSIS paketi yürütme etkinliğini kullanarak bir Azure Data Factory ardışık düzeninde bir SSIS paketi çalıştırmak açıklar. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. SSIS paketi yürütme etkinliği sürüm genel olarak kullanılabilir (GA) olan Data Factory hizmeti 1 kullanılabilir değil. Data Factory hizmetinin 1 sürümü ile bir SSIS paketi çalıştırmak alternatif bir yöntemi için bkz: [saklı yordam etkinliği sürüm 1 kullanılarak çalıştırmak SSIS paketleri](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-sql-database"></a>Azure SQL Database 
Bu makaledeki Kılavuzu SSIS katalog barındıran Azure SQL veritabanını kullanır. Bir Azure SQL yönetilen örneği (Önizleme) de kullanabilirsiniz.

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Adım adım yönergeleri izleyerek yoksa, bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma [Öğreticisi: dağıtmak SSIS paketleri](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="data-factory-ui-azure-portal"></a>Veri Fabrikası kullanıcı Arabirimi (Azure portalı)
Bu bölümde, veri fabrikası UI SSIS paketi çalıştırır bir SSIS paketi yürütme etkinlikle Data Factory işlem hattı oluşturmak için kullanın.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure Portalı'nı kullanarak data factory oluşturmak ilk adımdır. 

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
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca Data Factory tarafından desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
    ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın. 

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>SSIS paketi yürütme etkinliği ile işlem hattı oluşturma
Bu adımda, bir işlem hattı oluşturmak için veri fabrikası kullanıcı arabirimini kullanın. SSIS paketi yürütme aktivite ardışık düzene ekleyip SSIS paketi çalıştırmak için yapılandırabilirsiniz. 

1. Get başlangıç sayfasını tıklatın **oluşturma ardışık düzen**: 

    ![Başlarken sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. İçinde **etkinlikleri** araç kutusu, genişletin **genel**ve sürükle ve bırak **SSIS paketi yürütme** ardışık düzen Tasarımcı yüzeyine etkinlik. 

   ![SSIS etkinlik Tasarımcı yüzeyine sürükleyin](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Üzerinde **genel** sekmesi özelliklerini SSIS paketi yürütme etkinliği için bir ad ve açıklama etkinliği sağlar. İsteğe bağlı zaman aşımını ayarlamanız ve değerleri yeniden deneyin.

    ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Üzerinde **ayarları** Azure SSIS tümleştirmesi çalışma zamanı ilişkili SSIS paketi yürütme etkinliği için select özelliklerinin sekmesinde `SSISDB` paketin dağıtıldığı veritabanı. Paket yolu sağlayın `SSISDB` biçimindeki veritabanı `<folder name>/<project name>/<package name>.dtsx`. İsteğe bağlı olarak 32-bit yürütme ve önceden tanımlanmış veya özel günlüğe kaydetme düzeyi belirtin ve bir ortam yol biçimde sağlayın `<folder name>/<environment name>`.

    ![Ayarlar sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

5. Ardışık Düzen yapılandırmasını doğrulamak için tıklatın **doğrulama** araç çubuğunda. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

6. Tıklayarak Data Factory yayımlama kanalı **tümünü Yayımla** düğmesi. 

### <a name="optionally-parameterize-the-activity"></a>İsteğe bağlı olarak, etkinlik Parametreleştirme

İsteğe bağlı olarak, değerler ifadeler veya veri fabrikası sistem değişkenleri, proje veya paket parametrelerinizi JSON biçiminde üzerinde başvurabilirsiniz işlevleri atamak **Gelişmiş** sekmesi. Örneğin, Data Factory işlem hattı parametreleri SSIS projenize veya aşağıdaki ekran görüntüsünde gösterildiği gibi paket parametreleri atayabilirsiniz:

![SSIS paketi yürütme etkinlik parametreleri ekleme](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters.png)

### <a name="run-and-monitor-the-pipeline"></a>Çalıştırın ve işlem hattını izleme
Bu bölümde bir ardışık düzen çalıştırma tetikler ve ardından izleyebilirsiniz. 

1. Çalıştıran bir ardışık düzen tetiklemek için tıklatın **tetikleyici** araç ve tıklatın **şimdi tetikleyebilir**. 

    ![Şimdi tetikle](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 

3. Soldaki **İzleyici** sekmesine geçin. Çalıştırma ardışık düzen ve durumunu (örneğin, çalıştırma başlangıç saati) diğer bilgilerle birlikte bakın. Görünümü yenilemek için **Yenile**’ye tıklayın.

    ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

4. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Ardışık Düzen yalnızca bir etkinlik (SSIS paketi yürütme etkinliği) sahip farklı çalıştır yalnızca bir etkinlik bakın.

    ![Etkinlik çalıştırmaları](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

5. Aşağıdaki çalıştırabilirsiniz **sorgu** SSISDB karşı yürütülen paket doğrulamak için Azure SQL server veritabanı. 

    ```sql
    select * from catalog.executions
    ```

    ![Paket yürütmeleri doğrulayın](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

6. Ardışık Düzen etkinlik Çalıştır çıktısından SSISDB yürütme kimliği alın ve daha kapsamlı yürütme günlüklerini ve hata iletileri SSMS denetlemek için kimliği kullanın.

    ![Yürütme kimliği alma](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

> [!NOTE]
> Ardışık Düzen (saatlik, günlük, vs.) zamanlamaya göre çalışır, zamanlanmış bir tetikleyici için işlem hattınızı oluşturabilirsiniz. Bir örnek için bkz: [data factory - Data Factory UI oluşturma](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde, bir SSIS paketi çalıştıran SSIS sahip bir etkinliğini Data Factory işlem hattı oluşturmak için Azure PowerShell kullanın. 

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) konusundaki yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleyin. 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure SSIS IR sahip aynı veri fabrikası kullanın veya ayrı veri fabrikası oluşturun. Aşağıdaki yordam, bir data factory oluşturmak için adımları sağlar. Bu veri fabrikasında SSIS etkinliği ile işlem hattı oluşturun. SSIS etkinlik, SSIS paketi çalıştırır. 

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
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName `
                                            -Location $ResGrp.Location `
                                            -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Şu anda, veri fabrikası sürüm 2, veri fabrikaları yalnızca Doğu ABD, Doğu US2, Batı Avrupa ve Güneydoğu Asya bölgeleri oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

### <a name="create-a-pipeline-with-an-ssis-activity"></a>SSIS etkinliği ile işlem hattı oluşturma 
Bu adımda, SSIS etkinliği ile işlem hattı oluşturun. Etkinlik, SSIS paketi çalıştırır. 

1. Adlı bir JSON dosyası oluşturun **RunSSISPackagePipeline.json** içinde **C:\ADF\RunSSISPackage** klasör içeriği aşağıdaki örneğe benzer:

    > [!IMPORTANT]
    > Dosyayı kaydetmeden önce nesne adları, açıklamalar ve yolları, özellik ve parametre değerlerini, parolalar ve diğer değişken değerlerini değiştirin. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [{
                "name": "mySSISActivity",
                "description": "My SSIS package/activity description",
                "type": "ExecuteSSISPackage",
                "typeProperties": {
                    "connectVia": {
                        "referenceName": "myAzureSSISIR",
                        "type": "IntegrationRuntimeReference"
                    },
                    "runtime": "x64",
                    "loggingLevel": "Basic",
                    "packageLocation": {
                        "packagePath": "FolderName/ProjectName/PackageName.dtsx"            
                    },
                    "environmentPath":   "FolderName/EnvironmentName",
                    "projectParameters": {
                        "project_param_1": {
                            "value": "123"
                        }
                    },
                    "packageParameters": {
                        "package_param_1": {
                            "value": "345"
                        }
                    },
                    "projectConnectionManagers": {
                        "MyAdonetCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "abc"
                                }
                            }
                        }
                    },
                    "packageConnectionManagers": {
                        "MyOledbCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "def"
                                }
                            }
                        }
                    },
                    "propertyOverrides": {
                        "\\PackageName.dtsx\\MaxConcurrentExecutables ": {
                            "value": 8,
                            "isSensitive": false
                        }
                    }
                },
                "policy": {
                    "timeout": "0.01:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30
                }
            }]
        }
    }
    ```

2.  Azure PowerShell'de geçmek `C:\ADF\RunSSISPackage` klasör.

3. Ardışık düzen oluşturmak için **RunSSISPackagePipeline**, çalışma **kümesi AzureRmDataFactoryV2Pipeline** cmdlet'i.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                   -ResourceGroupName $ResGrp.ResourceGroupName `
                                                   -Name "RunSSISPackagePipeline"
                                                   -DefinitionFile ".\RunSSISPackagePipeline.json"
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
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

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

Azure portalını kullanarak ardışık düzeni da izleyebilirsiniz. Adım adım yönergeler için bkz: [işlem hattını izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="create-a-trigger"></a>Bir Tetikleyici oluşturma
Önceki adımda, ardışık düzen isteğe bağlı verdi. Ardışık Düzen (saatlik, günlük, vs.) zamanlamaya göre çalıştırmak için bir zamanlama tetikleyici de oluşturabilirsiniz.

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
3. Çalıştırma **kümesi AzureRmDataFactoryV2Trigger** cmdlet'ini tetikleyici oluşturur. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                    -DataFactoryName $DataFactory.DataFactoryName `
                                    -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Varsayılan olarak, tetikleyici durdurulmuş durumda. Tetikleyici çalıştırırsınız **başlangıç AzureRmDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                      -DataFactoryName $DataFactory.DataFactoryName `
                                      -Name "MyTrigger" 
    ```
5. Çalıştırarak, tetikleyici başlatıldığını onaylayın **Get-AzureRmDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                    -DataFactoryName $DataFactoryName `
                                    -Name "MyTrigger"     
    ```    
6. Sonraki saat sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saati UTC saat 15: 25'e ise, 4 PM UTC komutunu çalıştırın. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                       -DataFactoryName $DataFactoryName `
                                       -TriggerName "MyTrigger" `
                                       -TriggerRunStartedAfter "2017-12-06" `
                                       -TriggerRunStartedBefore "2017-12-09"
    ```

    Yürütülen paket doğrulamak için Azure SQL Server'da SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki blog gönderisine bakın:
-   [Modernize ve ETL/ELT akışlarınızı ADF ardışık düzende SSIS etkinliklerle genişletme](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
