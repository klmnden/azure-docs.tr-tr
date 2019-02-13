---
title: SSIS paketi yürütme etkinliği - Azure SSIS paketi çalıştırmak | Microsoft Docs
description: Bu makalede SSIS paketi yürütme etkinliğini kullanarak Azure Data Factory işlem hattı, bir SQL Server Integration Services (SSIS) paketi çalıştırmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/12/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 2a948a75ce3f6c21d7e92e3e1ccb1ef98dbe2ea0
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114393"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>SSIS paketi yürütme etkinliği Azure Data Factory ile SSIS paketi çalıştırın
Bu makalede, Azure Data Factory (ADF) işlem hattı, SSIS paketi yürütme etkinliği kullanarak SSIS paketi çalıştırılacak açıklar. 

## <a name="prerequisites"></a>Önkoşullar

Adım adım yönergeleri izleyerek zaten bir tane yoksa bir Azure-SSIS tümleştirme çalışma zamanı (IR) oluşturma [Öğreticisi: SSIS paketlerini Azure'a dağıtma](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Azure portalında bir paket çalıştırın
Bu bölümde, ADF oluşturmak için uygulama, SSIS paketi çalışan SSIS paketi yürütme etkinliği ile işlem hattı / ADF kullanıcı arabirimi (UI) kullanın.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Bir SSIS paketi yürütme etkinliği ile işlem hattı oluşturma
Bu adımda, bir işlem hattı oluşturmak için ADF UI/uygulaması kullanın. SSIS paketi yürütme etkinlik işlem hattının ekleyip, SSIS paketi çalıştırmak için yapılandırabilirsiniz. 

1. Azure portal'da, ADF genel bakış/giriş sayfasında tıklayarak **yazar ve İzleyici** ADF UI/uygulaması ayrı bir sekmede başlatmak için. 

   ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   Üzerinde **başlayalım** sayfasında **işlem hattı Oluştur**: 

   ![Başlarken sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. İçinde **etkinlikleri** araç genişletin **genel**, ardından Sürükle & bırak bir **SSIS paketi yürütme** etkinliğini işlem hattı Tasarımcı yüzeyine bırakın. 

   ![SSIS paketi yürütme etkinliği Tasarımcı yüzeyine sürükleyin.](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Üzerinde **genel** SSIS paketi yürütme etkinliği için sekmesinde, bir ad ve açıklama etkinliği sağlar. İsteğe bağlı zaman aşımını ayarlayın ve değerleri yeniden deneyin.

   ![Genel sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Üzerinde **ayarları** SSIS paketi yürütme etkinliği için sekmesinde, paketin dağıtıldığı SSISDB veritabanı ile ilişkili olan Azure-SSIS IR seçin. Paketiniz çalıştırmak için 32 bit çalışma zamanı gerekiyorsa işaretleyin **32 Bit çalışma zamanı** onay kutusu. İçin **günlük düzeyi**, günlük, paket yürütme için önceden tanımlanmış bir kapsam seçin. Denetleme **özelleştirilmiş** özelleştirilmiş günlük adınızı yerine girmek istiyorsanız onay kutusunu. Azure-SSIS IR çalışırken ve **el ile yapılan girişler** onay kutusu işaretli, göz atabilir ve mevcut klasörleri/projelerini/paketlerini/ortamlarınızı SSISDB seçin. Tıklayın **Yenile** gezinme ve seçim için kullanılabilir olduklarından SSISDB yeni eklenen klasörler/projelerini/paketlerini/ortamlarınızda getirilecek düğmesi. 

   ![Ayarlar sekmesinde - Otomatik özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Azure-SSIS IR değil çalışırken veya **el ile yapılan girişler** onay kutusu seçiliyse, aşağıdaki biçimlerde SSISDB, paket ve ortam yolları girebilirsiniz: `<folder name>/<project name>/<package name>.dtsx` ve `<folder name>/<environment name>`.

   ![Ayarlar sekmesinde - el ile özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

5. Üzerinde **SSIS parametreleri** Azure-SSIS IR çalışırken SSIS paketi yürütme etkinliği için sekmesinde ve **el ile yapılan girişler** onay kutusuna **ayarları** sekmedir işaretli Seçilen proje/paketinizi SSISDB var olan SSIS parametrelerinde, bunlara değer atayamazsınız görüntülenir. Aksi takdirde, bunları tek tek el ile bunlara değer atayamazsınız: Lütfen mevcut ve başarılı olması paket yürütme için doğru girildiğinden emin olun için girebilirsiniz. Dinamik içerik ifadeleri, İşlevler, ADF sistem değişkenleri ve ADF işlem hattı parametre/değişkenleri değerleri için de ekleyebilirsiniz.

   ![SSIS parametreleri sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. Üzerinde **bağlantı yöneticileri** Azure-SSIS IR çalışırken SSIS paketi yürütme etkinliği için sekmesinde ve **el ile yapılan girişler** onay kutusuna **ayarları** sekmedir işaretli Seçilen proje/paketinizi SSISDB mevcut bağlantı yöneticileri, bunlara değer atayamazsınız görüntülenir. Aksi takdirde, bunları tek tek el ile bunlara değer atayamazsınız: Lütfen mevcut ve başarılı olması paket yürütme için doğru girildiğinden emin olun için girebilirsiniz. Dinamik içerik ifadeleri, İşlevler, ADF sistem değişkenleri ve ADF işlem hattı parametre/değişkenleri değerleri için de ekleyebilirsiniz.

   ![Bağlantı yöneticileri sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. Üzerinde **özelliğini geçersiz kılar** sekmesini SSIS paketi yürütme etkinliği için mevcut özelliklerin yolları seçili paketinizdeki el ile bunlara değer atayamazsınız: Lütfen mevcut ve olduğundan emin olmak için SSISDB tek tek girebilirsiniz başarılı, örneğin, paket yürütme için doğru girdiğinizi, kullanıcı değişkeninin değerini geçersiz kılmak için yol şu biçimde girin: `\Package.Variables[User::YourVariableName].Value`. Dinamik içerik ifadeleri, İşlevler, ADF sistem değişkenleri ve ADF işlem hattı parametre/değişkenleri değerleri için de ekleyebilirsiniz.

   ![Özelliğini geçersiz kılar sekmesinde özelliklerini ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

8. İşlem hattı yapılandırmasını doğrulamak için tıklayın **doğrulama** araç. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

9. Yayımlama kanalı için ADF tıklayarak **tümünü Yayımla** düğmesi. 

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma
Bu adımda, bir işlem hattı çalıştırması tetikleyin. 

1. Bir işlem hattı çalıştırması tetiklemek için tıklatın **tetikleyici** araç ve tıklatın **şimdi Tetikle**. 

   ![Şimdi tetikle](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. İşlem hattı çalıştırma ve yanı sıra diğer bilgiler (örneğin, çalıştırma başlangıç saati) durumunu görürsünüz. Görünümü yenilemek için **Yenile**’ye tıklayın.

   ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Yalnızca bir etkinlik olduğundan işlem hattı yalnızca bir etkinlik (SSIS paketi yürütme etkinliği) çalıştırma görürsünüz.

   ![Etkinlik çalıştırmaları](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Aşağıdakini çalıştırabilirsiniz. **sorgu** SSISDB karşı yürütülen paket doğrulamak için Azure SQL server veritabanı. 

   ```sql
   select * from catalog.executions
   ```

   ![Paket yürütme doğrulayın](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. İşlem hattı etkinlik çalıştırması çıktısından SSISDB yürütme Kimliğini alın ve daha kapsamlı yürütme günlükleri ve hata iletilerinde SSMS denetlemek için kimliği kullanın.

   ![Yürütme kimliği Al](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Zamanlama bir tetikleyici ile işlem hattı

Böylece işlem hattını bir zamanlamaya göre (saatlik, günlük, vb.) çalıştırır, zamanlanmış bir tetikleyici için işlem hattınızı oluşturabilirsiniz. Bir örnek için bkz. [veri fabrikası - Data Factory kullanıcı Arabirimi oluşturma](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>Bir paket PowerShell ile Çalıştır
Bu bölümde, SSIS paketi çalışan SSIS paketi yürütme etkinliği ile bir ADF işlem hattı oluşturmak için Azure PowerShell kullanırsınız. 

Adım adım yönergeleri izleyerek en güncel Azure PowerShell modüllerini yükleme [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/azurerm/install-azurerm-ps).

### <a name="create-an-adf-with-azure-ssis-ir"></a>ADF ile Azure-SSIS IR oluşturma
Azure-SSIS IR'nin sağlanması zaten mevcut ADF kullanabilir veya yeni bir ADF ile adım adım talimatları Azure-SSIS IR oluşturma [Öğreticisi: Azure PowerShell aracılığıyla SSIS paketlerini dağıtma](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Bir SSIS paketi yürütme etkinliği ile işlem hattı oluşturma 
Bu adımda, bir SSIS paketi yürütme etkinliği ile işlem hattı oluşturma. Etkinlik, SSIS paketi çalıştırır. 

1. Adlı bir JSON dosyası oluşturun **RunSSISPackagePipeline.json** içinde **C:\ADF\RunSSISPackage** aşağıdaki örneğe benzer içeriğe sahip klasörde:

   > [!IMPORTANT]
   > Dosyayı kaydetmeden önce nesne adları ve açıklamaları yolları, özellik ve parametre değerlerini, parolalar ve diğer değişken değerleri değiştirin. 

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
                   "environmentPath": "FolderName/EnvironmentName",
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
                       "\\Package.MaxConcurrentExecutables": {
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

2. Azure PowerShell'de geçin `C:\ADF\RunSSISPackage` klasör.

3. İşlem hattını oluşturmak için **RunSSISPackagePipeline**çalıştırın **Set-AzureRmDataFactoryV2Pipeline** cmdlet'i.

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

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma
Kullanım **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet'ini işlem hattını çalıştırın. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme

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

Azure portalını kullanarak işlem hattını da izleyebilirsiniz. Adım adım yönergeler için bkz: [işlem hattını izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>Zamanlama bir tetikleyici ile işlem hattı
Önceki adımda, işlem hattı talep üzerine çalıştı. İşlem hattını bir zamanlamaya göre (saatlik, günlük, vb.) çalıştırmak için bir zamanlama tetikleyicisi de oluşturabilirsiniz.

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
           }]
       }
   }    
   ```
2. İçinde **Azure PowerShell**, geçiş **C:\ADF\RunSSISPackage** klasör.
3. Çalıştırma **Set-AzureRmDataFactoryV2Trigger** cmdlet'i, bir tetikleyici oluşturur. 

   ```powershell
   Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. Varsayılan olarak, tetikleyici durdurulmuş durumdadır. Tetikleyiciyi çalıştırmadan **Start-AzureRmDataFactoryV2Trigger** cmdlet'i. 

   ```powershell
   Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Çalıştırarak tetikleyicinin başlatıldığını onaylayın **Get-AzureRmDataFactoryV2Trigger** cmdlet'i. 

   ```powershell
   Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Sonraki saat sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saati 3: 25'te ise, 4'te komutu çalıştırın. 
    
   ```powershell
   Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Yürütülen paket doğrulamak için Azure SQL sunucunuza SSISDB veritabanında şu sorguyu çalıştırabilirsiniz. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Sonraki adımlar
İçin şu blog yayınına bakın:
-   [Modernleştirin ve ETL/ELT iş akışlarınızı SSIS etkinliklerle ADF işlem hatlarını genişletin](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
