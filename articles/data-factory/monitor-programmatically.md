---
title: Program aracılığıyla bir Azure data factory izleme | Microsoft Docs
description: Farklı yazılım geliştirme setleri (SDK) kullanarak bir veri fabrikasında ardışık düzeni izlemek öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 87e69349245c5f67e23022e3a45ed798400e6a2c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Program aracılığıyla bir Azure data factory izleme
Bu makalede, farklı yazılım geliştirme setleri (SDK) kullanarak bir veri fabrikasında ardışık düzeni izlemek açıklar. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [İzleyici ve ardışık düzenlerinde Data Factory version1 yönetmek](v1/data-factory-monitor-manage-pipelines.md).

## <a name="data-range"></a>Veri aralığı

Veri Fabrikası yalnızca ardışık düzen veri 45 gün çalıştırmak depolar. Sorguladığınızda program aracılığıyla Data Factory işlem hattı çalışır hakkında-veriler için örneğin, PowerShell komutuyla `Get-AzureRmDataFactoryV2PipelineRun` -isteğe bağlı için en fazla tarih vardır `LastUpdatedAfter` ve `LastUpdatedBefore` parametreleri. Ancak, sorgu için veri ve geçen yıl, örneğin, sorgu bir hata döndürmez, ancak yalnızca döndürür, son 45 gün çalışma veri kanalı.

Kendi tanılama günlük kaydıyla 45 günden fazla bir süre için veri çalıştırmak ardışık düzen kalıcı hale getirmek istiyorsanız, ayarlama [Azure İzleyici](monitor-using-azure-monitor.md).

## <a name="net"></a>.NET
Oluşturma ve .NET SDK kullanarak bir işlem hattı izleme izlenecek tam yol için bkz: [bir veri fabrikası oluşturun ve .NET kullanarak kanal](quickstart-create-data-factory-dot-net.md).

1. Sürekli olarak veri kopyalama sonlanana kadar çalıştırmak ardışık düzen durumunu denetlemek için aşağıdaki kodu ekleyin.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Aşağıdaki kodu ayrıntıları, örneğin, çalıştırmak, alır kopyalama etkinliği için okunur/yazılır veri boyutu ekleyin.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

.NET SDK'sı kapsamlı belgeler için bkz: [veri fabrikası .NET SDK'sı başvurusu](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet).

## <a name="python"></a>Python
Oluşturma ve Python SDK'sını kullanarak bir işlem hattı izleme izlenecek tam yol için bkz: [bir veri fabrikası oluşturun ve Python kullanarak kanal](quickstart-create-data-factory-python.md).

Ardışık Düzen Çalıştır izlemek için aşağıdaki kodu ekleyin:

```python
#Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Python SDK'sı kapsamlı belgeler için bkz: [veri fabrikası Python SDK'sı başvurusu](/python/api/overview/azure/datafactory?view=azure-python).

## <a name="rest-api"></a>REST API
Oluşturma ve REST API kullanarak bir işlem hattı izleme izlenecek tam yol için bkz: [bir veri fabrikası oluşturun ve REST API kullanarak kanal](quickstart-create-data-factory-rest-api.md).
 
1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) almak için aşağıdaki betiği çalıştırın.

    ```PowerShell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

REST API üzerindeki tüm belgeler için bkz: [veri fabrikası REST API Başvurusu](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
Oluşturma ve PowerShell kullanarak bir işlem hattı izleme izlenecek tam yol için bkz: [bir veri fabrikası oluşturun ve PowerShell kullanarak kanal](quickstart-create-data-factory-powershell.md).

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

PowerShell cmdlet'leri hakkında kapsamlı belgeler için bkz: [veri fabrikası PowerShell cmdlet başvurusunun](/powershell/module/azurerm.datafactoryv2/?view=azurermps-4.4.1).

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [İzleyici ardışık düzenleri Azure İzleyicisi'ni kullanarak](monitor-using-azure-monitor.md) makale Data Factory işlem hatlarını izlemek için Azure İzleyicisi'ni kullanma hakkında bilgi edinin. 

