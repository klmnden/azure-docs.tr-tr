---
title: "Azure Data Factory'de Tetikleyicileri oluşturma | Microsoft Docs"
description: "Bir işlem hattı bir zamanlamaya göre çalışan bir Azure Data factory'de bir tetikleyici oluşturmayı öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: eed286c01604ab0e9ac2113d56cbce268503668d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Bir işlem hattı bir zamanlamaya göre çalışan bir Tetikleyici oluşturma
Bu makalede oluşturmak, başlatma ve tetikleyici izlemek için adımları sağlar. Kavramsal Tetikleyiciler hakkında bilgi için [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Bu bölümde Azure PowerShell oluşturma, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir. Bu örnek çalışma görmek istiyorsanız, ilk Git [hızlı başlangıç: Azure PowerShell kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md). Ardından, aşağıdaki kodu oluşturan ve her 15 dakikada bir zamanlama tetikleyici başlatır ana yöntemine ekleyin. Tetikleyici sahip işlem hattı ilişkilendirilir (**Adfv2QuickStartPipeline**), hızlı bir parçası olarak oluşturun.

1. C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip MyTrigger.json adlı bir JSON dosyası oluşturun: 

    > [!IMPORTANT]
    > Ayarlama **startTime** geçerli UTC saati için ve **endTime** geçerli UTC geçmiş bir saat için JSON dosyayı kaydetmeden önce zaman.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```
    
    JSON parçacığında: 
    - **Türü** tetikleyicisi kümesine **ScheduleTrigger**. 
    - **Sıklığı** ayarlanır **Minute** ve **aralığı** ayarlanır **15**. Bu nedenle, tetikleyici ardışık düzen 15 dakikada bir başlangıç ve bitiş zamanları arasında çalışır. 
    - **EndTime** bir saat sonra **startTime**, tetikleyici ardışık 15 dakika, 30 dakika ve 45 dakika startTime sonra çalışır. Geçerli UTC saati ve endTime startTime geçmiş bir saat için startTime güncelleştirmek unutmayın.  
    - Tetikleyici ile ilişkili **Adfv2QuickStartPipeline** ardışık düzen. Birden çok ardışık düzen tetikleyici ile ilişkilendirmek için daha fazla Ekle **pipelineReference** bölümler. 
    - Hızlı Başlangıç hattında iki alan **parametreleri**. Bu nedenle, bu parametrelerin değerlerini tetikleyiciyle geçirin. 
2. Kullanarak bir Tetikleyici oluşturma **kümesi AzureRmDataFactoryV2Trigger** cmdlet'i.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Tetikleyici durumunu onaylayın **durduruldu** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet'i. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
4. Tetikleyici başlamayı **başlangıç AzureRmDataFactoryV2Trigger** cmdlet: 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
5. Tetikleyici durumunu onaylayın **başlatıldı** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet'i.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
6.  PowerShell kullanarak tetikleyici çalıştırır alma **Get-AzureRmDataFactoryV2TriggerRun** cmdlet'i. Tetikleyici çalışmaları hakkında bilgi almak için aşağıdaki komutu düzenli aralıklarla çalıştırın: güncelleştirme **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** tetikleyici tanımında değerleriyle eşleşecek şekilde değerleri . 

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```

    Tetikleyici çalıştırır/ardışık çalıştığında Azure portalında izlemek için bkz: [işlem hattını izleme çalıştırır](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-net-sdk"></a>.NET SDK kullanma
Bu bölümde .NET SDK oluşturma, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir. Bu kodu çalışma görmek istiyorsanız, ilk Git [.NET SDK'yı kullanarak data factory oluşturmak için Hızlı Başlangıç](quickstart-create-data-factory-dot-net.md). Ardından, aşağıdaki kodu oluşturan ve her 15 dakikada bir zamanlama tetikleyici başlatır ana yöntemine ekleyin. Tetikleyici sahip işlem hattı ilişkilendirilir (**Adfv2QuickStartPipeline**), hızlı bir parçası olarak oluşturun. 

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method. 
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Çalıştıran bir tetikleyici izlemek için son önce aşağıdaki kodu ekleyin `Console.WriteLine` deyimi: 

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Tetikleyici çalıştırır/ardışık çalıştığında Azure portalında izlemek için bkz: [işlem hattını izleme çalıştırır](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-python-sdk"></a>Python SDK'sını kullanma
Bu bölümde Python SDK oluşturma, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir. Bu kodu çalışma görmek istiyorsanız, ilk Git [Python SDK'yı kullanarak data factory oluşturmak için Hızlı Başlangıç](quickstart-create-data-factory-python.md). Ardından, aşağıdaki kod bloğu python komut dosyasında "Çalıştır işlem hattını izleme" kod bloğunu ekleyin. Bu kod belirtilen başlangıç ve bitiş zamanları arasında 15 dakikada bir çalışır bir zamanlama tetikleyici oluşturur. Geçerli UTC saati ve geçerli UTC saati geçmiş bir saat end_time start_time güncelleştirin. 

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC') 
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Tetikleyici çalıştırır/ardışık çalıştığında Azure portalında izlemek için bkz: [işlem hattını izleme çalıştırır](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-resource-manager-template"></a>Kullanım Resource Manager şablonu
Bir tetikleyici oluşturmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Adım adım yönergeler için bkz: [Resource Manager şablonu kullanarak bir Azure data factory oluşturmak](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Tetikleyici başlangıç saati bir ardışık düzene geçirin
Sürüm 1'de, Azure Data Factory veri okunurken veya bölümlenmiş SliceStart/SliceEnd/WindowStart/WindowEnd sistem değişkenleri kullanılarak yazılırken desteklenir. Sürüm 2'de, ardışık düzen parametre ve tetikleyici başlangıç saati ve zamanlanan saat parametresinin değeri kullanarak bu davranışı elde edebilirsiniz. Aşağıdaki örnekte, tetikleyici zamanlanan saat ardışık düzen parametresi scheduledRunTime için bir değer olarak geçirilir. 

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Daha fazla bilgi için bkz: [veri okumak veya yazmak nasıl bölümlenmiş](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).