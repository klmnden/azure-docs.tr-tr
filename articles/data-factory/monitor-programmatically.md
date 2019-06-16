---
title: Program aracılığıyla bir Azure data factory izleyin | Microsoft Docs
description: Farklı yazılım geliştirme setleri (SDK'lar) kullanarak bir veri fabrikasında bir işlem hattını izleme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: 035e12da67d28e8e3fb46ac295717dd6b579922c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66167050"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Program aracılığıyla bir Azure data factory izleyin
Bu makalede, farklı yazılım geliştirme setleri (SDK'lar) kullanarak bir veri fabrikasında bir işlem hattı izlemek açıklar. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="data-range"></a>Veri aralığı

Veri Fabrikası işlem hattı 45 gün boyunca veri çalıştırması yalnızca depolar. Sorguladığınızda programlı olarak Data Factory işlem hattı çalıştırmaları - ilgili veriler için örnek PowerShell komutuyla `Get-AzDataFactoryV2PipelineRun` -tarih yok için isteğe bağlı `LastUpdatedAfter` ve `LastUpdatedBefore` parametreleri. Ancak, verileri sorgulamak için önceki yıl, örneğin, sorgu bir hata döndürmez, ancak yalnızca döndürür, son 45 güne ait çalıştırma veri işlem hattı.

İşlem hattı 45 gün boyunca veri çalıştırması kalıcı hale getirmek isterseniz, kendi tanılama günlüğüne kaydetme ile ayarlama [Azure İzleyici](monitor-using-azure-monitor.md).

## <a name="net"></a>.NET
Oluşturma ve .NET SDK'sını kullanarak bir işlem hattı izlemek üzere izlenecek tam yol için bkz: [veri fabrikası oluşturma ve .NET kullanarak işlem hattı](quickstart-create-data-factory-dot-net.md).

1. İşlem hattı verileri kopyalama işlemi tamamlanıncaya kadar çalıştırma durumunu sürekli olarak denetlemek için aşağıdaki kodu ekleyin.

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

2. Çalıştırma ayrıntıları, örneğin, bu alır kopyalama etkinliğine okunan/yazılan verilerin boyutu aşağıdaki kodu ekleyin.

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

.NET SDK'sı ile ilgili kapsamlı belgeler için bkz. [Data Factory .NET SDK başvuru](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet).

## <a name="python"></a>Python
Oluşturma ve Python SDK'sını kullanarak bir işlem hattı izlemek üzere izlenecek tam yol için bkz: [veri fabrikası oluşturma ve Python kullanarak işlem hattı](quickstart-create-data-factory-python.md).

İşlem hattı çalıştırmasını izlemek için aşağıdaki kodu ekleyin:

```python
#Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Python SDK'sı ile ilgili kapsamlı belgeler için bkz. [Data Factory Python SDK başvurusu](/python/api/overview/azure/datafactory?view=azure-python).

## <a name="rest-api"></a>REST API
Oluşturma ve REST API kullanarak bir işlem hattı izlemek üzere izlenecek tam yol için bkz: [veri fabrikası oluşturma ve REST API kullanarak işlem hattı](quickstart-create-data-factory-rest-api.md).
 
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

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

REST API ile ilgili kapsamlı belgeler için bkz. [Data Factory REST API Başvurusu](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
Oluşturma ve PowerShell kullanarak bir işlem hattı izlemek üzere izlenecek tam yol için bkz: [veri fabrikası oluşturma ve PowerShell kullanarak işlem hattı](quickstart-create-data-factory-powershell.md).

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

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
    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

PowerShell cmdlet'leri hakkında kapsamlı belgeler için bkz. [Data Factory PowerShell cmdlet başvurusu](/powershell/module/az.datafactory).

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [işlem hatlarını izleme Azure İzleyicisi'ni kullanarak](monitor-using-azure-monitor.md) makale, Data Factory işlem hatlarını izlemek için Azure İzleyicisi'ni kullanma hakkında bilgi edinmek için. 

