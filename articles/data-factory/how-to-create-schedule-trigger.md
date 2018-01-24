---
title: "Azure Data Factory'de zamanlama Tetikleyicileri oluşturma | Microsoft Docs"
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
ms.date: 01/23/2018
ms.author: shlo
ms.openlocfilehash: 51e2dddbe66ca372d89fc8efeb24bdab9fe6a442
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Bir işlem hattı bir zamanlamaya göre çalışan bir Tetikleyici oluşturma
Bu makalede zamanlama tetikleyici ve oluşturma, başlatma ve zamanlama tetikleyici izlemek için gereken adımlar hakkında bilgi sağlar. Tetikleyiciler diğer türleri için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md).

Bir zamanlama tetikleyici oluştururken, bir zamanlama belirtin (başlangıç tarihi, yineleme, bitiş tarihi vb.) tetikleyici ve bir ardışık düzen ilişkilendirmek için. İşlem hatları ve tetikleyiciler çoka çok ilişkisine sahiptir. Birden çok tetikleyici tek bir işlem hattını başlatabilir. Tek bir tetikleyici birden fazla işlem hattını başlatabilir.

> [!NOTE]
> Bu makale, Azure Data Factory şu anda önizlemede olan sürüm 2 için geçerlidir. Azure Data Factory genel olarak kullanılabilir (GA) olan sürüm 1, kullanıyorsanız, bkz: [sürüm 1 Azure Data Factory ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Aşağıdaki bölümlerde farklı şekillerde zamanlama tetikleyici oluşturmak için adımlar sağlar. 

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)
Oluşturabileceğiniz bir **zamanlama tetikleyici** (saatlik, günlük, vb.) düzenli olarak çalıştırmak için bir işlem hattı zamanlamak için. 

> [!NOTE]
> Bir ardışık düzen ve zamanlama tetikleyici, tetikleyici ardışık çalıştığını ve ardışık düzen izleme ile ilişkilendirme oluşturma izlenecek tam yol için bkz: [hızlı başlangıç: Data Factory kullanıcı arabirimini kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-portal.md).

1. **Düzen** sekmesine geçin. 

    ![Düzen sekmesine geçin](./media/how-to-create-schedule-trigger/switch-edit-tab.png)
1. Menüde **Tetikleyici**'ye tıklayın ve sonra da **Yeni/Düzenle**'ye tıklayın. 

    ![Yeni tetikleyici menüsü](./media/how-to-create-schedule-trigger/new-trigger-menu.png)
2. **Tetikleyici Ekle** sayfasında **Tetikleyici seç...** öğesine ve sonra da **Yeni**'ye tıklayın. 

    ![Tetikleyici ekle - yeni tetikleyici](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)
3. İçinde **Yeni Tetikleyici** sayfasında, aşağıdaki adımları uygulayın: 

    1. Onaylayın **zamanlama** için seçilen **türü**. 
    2. Tetikleyici için başlangıç datetime belirtin **başlangıç tarihi (UTC)**. Varsayılan olarak, geçerli datetime olarak ayarlanır. 
    3. Belirtin **yineleme** tetikleyici için. Değerlerden biri (her dakika, saatlik, günlük, haftalık ve aylık) aşağı açılan listeden seçin. Çarpan metin kutusuna girin. Örneğin, 15 dakikada bir kez çalıştır tetikleyicinin isterseniz, seçtiğiniz **her dakika**ve girin **15** metin kutusuna. 
    4. İçin **son** seçin tetikleyici için bir son tarih saat belirtmek istemiyorsanız, alan **Hayır son**. Bir son tarih belirtmek için işaretleyin **tarihini**, son tarih saat belirtin ve tıklatın **Uygula**. Her işlem hattı çalıştırmasının bir maliyeti vardır. Test yapıyorsanız, ardışık düzen tetiklenir olmak isteyebilirsiniz yalnızca birkaç kez. Öte yandan, yayımlama saatiyle bitiş saati arasında işlem hattının çalıştırılmasına yetecek kadar zaman olduğundan emin olun. Tetikleyici siz çözümü kullanıcı arabiriminde kaydettiğinizde değil ancak Data Factory'de yayımladığınızda devreye girer.

        ![Tetikleyici ayarları](./media/how-to-create-schedule-trigger/trigger-settings.png)
4. İçinde **Yeni Tetikleyici** penceresinde, onay **etkinleştirildi** seçeneğini ve tıklayın **sonraki**. Daha sonra tetikleyici devre dışı bırakmak için bu onay kutusunu kullanın. 

    ![Tetikleyici ayarları - İleri düğmesi](./media/how-to-create-schedule-trigger/trigger-settings-next.png)
5. **Yeni Tetikleyici** sayfasında uyarı iletisini gözden geçirin ve **Son**'a tıklayın.

    ![Tetikleyici ayarları - Son düğmesi](./media/how-to-create-schedule-trigger/new-trigger-finish.png)
6. Değişiklikleri Data Factory'de yayımlamak için **Yayımla**'ya tıklayın. Veri Fabrikası değişiklikleri yayımlama kadar tetikleyici ardışık düzen çalıştırır tetikleme başlatılmaz. 

    ![Yayımla düğmesi](./media/how-to-create-schedule-trigger/publish-2.png)
8. Soldaki **İzleyici** sekmesine geçin. Listeyi yenilemek için **Yenile**’ye tıklayın. Ardışık Düzen zamanlanmış tetik tarafından tetiklenen çalışır bakın. **Tetikleyen** sütunundaki değerlere dikkat edin. Kullanırsanız **şimdi tetikleyici** seçeneği, listede el ile tetikleyici bakın. 

    ![Tetiklenen çalıştırmaları izleme](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)
9. **İşlem Hattı Çalıştırmaları**'nın yanındaki aşağı oka tıklayarak **Tetikleyici Çalıştırmaları** görünümüne geçin. 

    ![Tetikleme çalıştırmalarını izleme](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde Azure PowerShell oluşturmak, başlatma ve zamanlama tetikleyici izlemek için nasıl kullanılacağını gösterir. Bu örnek çalışma görmek için önce geçtikleri [hızlı başlangıç: Azure PowerShell kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md). Ardından, aşağıdaki kodu oluşturan ve her 15 dakikada bir zamanlama tetikleyici başlatır ana yöntemine ekleyin. Tetikleyici adlı işlem hattını ile ilişkilendirilen **Adfv2QuickStartPipeline** , hızlı bir parçası olarak oluşturun.

1. Adlı bir JSON dosyası oluşturun **MyTrigger.json** C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip:

    > [!IMPORTANT]
    > JSON dosyasının kaydetmeden önce değerini ayarlamak **startTime** öğesi geçerli UTC saati için. Değerini **endTime** öğesi geçerli UTC saati geçmiş bir saat.

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
    - **Türü** tetikleyici öğesinin "ScheduleTrigger" olarak ayarlanmış
    - **Sıklığı** öğesi, "Dakika" olarak ayarlanır ve **aralığı** öğesi, 15'e ayarlanır. Bu nedenle, tetikleyici ardışık düzen 15 dakikada bir başlangıç ve bitiş zamanları arasında çalışır.
    - **EndTime** öğesidir bir saat değeri sonra **startTime** öğesi. Bu nedenle, tetikleyici ardışık düzen 15 dakika, 30 dakika ve 45 dakika başlama saatinden sonra çalışır. Başlangıç zamanı geçerli UTC zamanı ve bitiş saati başlangıç saatinden geçmiş bir saat için güncelleştirmeyi unutmayın. 
    - Tetikleyici ile ilişkili **Adfv2QuickStartPipeline** ardışık düzen. Birden çok ardışık düzen tetikleyici ile ilişkilendirmek için daha fazla Ekle **pipelineReference** bölümler.
    - Hızlı Başlangıç hattında iki alan **parametreleri** değerleri: **InputPath** ve **outputPath**. Bu nedenle, bu parametreler için değerler tetikleyiciyle geçirin.

2. Kullanarak bir Tetikleyici oluşturma **kümesi AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

3. Tetikleyici durumunu onaylayın **durduruldu** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Tetikleyici başlamayı **başlangıç AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Tetikleyici durumunu onaylayın **başlatıldı** kullanarak **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6.  Tetikleyici kullanarak Azure PowerShell'de çalışan get **Get-AzureRmDataFactoryV2TriggerRun** cmdlet'i. Tetikleyici çalışmaları hakkında bilgi almak için düzenli aralıklarla aşağıdaki komutu yürütün. Güncelleştirme **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** tetikleyici tanımınızı değerler eşleşecek şekilde değerler:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Tetikleyici izlemek için çalışır ve ardışık düzen çalışır Azure Portalı'nda, bkz: [işlem hattını izleme çalıştıran](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="net-sdk"></a>.NET SDK
Bu bölümde .NET SDK'sı oluşturmak, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir. Bu örnek çalışma görmek için önce geçtikleri [hızlı başlangıç: .NET SDK kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md). Ardından, aşağıdaki kodu oluşturan ve her 15 dakikada bir zamanlama tetikleyici başlatır ana yöntemine ekleyin. Tetikleyici adlı işlem hattını ile ilişkilendirilen **Adfv2QuickStartPipeline** , hızlı bir parçası olarak oluşturun.

Oluşturun ve her 15 dakikada bir zamanlama tetikleyici başlatmak için ana yöntemine aşağıdaki kodu ekleyin:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Çalıştıran bir tetikleyici izlemek için son önce aşağıdaki kodu ekleyin `Console.WriteLine` örnek deyiminde:

```csharp
            // Check that the trigger runs every 15 minutes
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

Tetikleyici izlemek için çalışır ve ardışık düzen çalışır Azure Portalı'nda, bkz: [işlem hattını izleme çalıştıran](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="python-sdk"></a>Python SDK'sı
Bu bölümde Python SDK'sı oluşturmak, başlatma ve tetikleyici izlemek için nasıl kullanılacağını gösterir. Bu örnek çalışma görmek için önce geçtikleri [hızlı başlangıç: Python SDK'sını kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-python.md). Ardından, aşağıdaki kod bloğu Python komut dosyasında "Çalıştır işlem hattını izleme" kod bloğunu ekleyin. Bu kod belirtilen başlangıç ve bitiş zamanları arasında 15 dakikada bir çalışır bir zamanlama tetikleyici oluşturur. Güncelleştirme **start_time** geçerli UTC saati için değişken ve **end_time** geçerli UTC saati geçmiş bir saat için değişken.

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

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Tetikleyici izlemek için çalışır ve ardışık düzen çalışır Azure Portalı'nda, bkz: [işlem hattını izleme çalıştıran](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Bir tetikleyici oluşturmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Adım adım yönergeler için bkz: [Resource Manager şablonu kullanarak bir Azure data factory oluşturma](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Tetikleyici başlangıç saati bir ardışık düzene geçirin
Azure Data Factory sürüm 1 okuma veya sistem değişkenleri kullanılarak bölümlenmiş veri yazmayı destekler: **SliceStart**, **SliceEnd**, **WindowStart**ve **WindowEnd**. Azure Data Factory sürüm 2, ardışık düzen parametresini kullanarak bu davranışı elde edebilirsiniz. Tetikleyici için zamanlanan saat ve başlangıç saati, ardışık düzen parametresinin değeri olarak ayarlanır. Aşağıdaki örnekte, ardışık düzene bir değer olarak tetikleyici için zamanlanan süreden geçirilen **scheduledRunTime** parametre:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    

' Ndaki yönergeleri daha fazla bilgi için bkz: [veri okumak veya yazmak nasıl bölümlenmiş](how-to-read-write-partitioned-data.md).

## <a name="json-schema"></a>JSON şeması
Aşağıdaki JSON tanımını zamanlama ve yineleme zamanlaması tetikleyici oluşturulacağını gösterir:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  **Parametreleri** özelliği bir özelliğidir zorunlu **ardışık düzen** öğesi. Herhangi bir parametre hattınızı almazsa için boş bir JSON tanımı içermelidir **parametreleri** özelliği.


### <a name="schema-overview"></a>Şema genel bakış
Aşağıdaki tabloda, yineleme ile ilgili ve tetikleyici zamanlama ana şema öğeleri üst düzey bir genel bakış sağlar:

| JSON özelliği | Açıklama |
|:--- |:--- |
| **startTime** | Bir tarih-saat değeri. Basit zamanlamaları, değeri için **startTime** özelliği, ilk oluşumuna uygulanır. Karmaşık zamanlamalar için daha önce belirtilen tetikleyici başlatır **startTime** değeri. |
| **endTime** | Bitiş tarihi ve saati tetikleyici için. Tetikleyici belirtilen bitiş tarih ve saatten sonra yürütmez. Özelliğinin değeri geçmişte olamaz. Bu özellik isteğe bağlıdır. |
| **timeZone** | Saat dilimi. Şu anda yalnızca UTC saat dilimi desteklenir. |
| **recurrence** | Tetikleyici yineleme kurallarını belirtir yinelenme nesnesi. Yinelenme nesnesi destekler **sıklığı**, **interva**l, **endTime**, **sayısı**, ve **zamanlama**öğeleri. Yinelenme nesnesi tanımlandığında, **sıklığı** öğesi gereklidir. Yineleme nesnesinin diğer öğeler isteğe bağlıdır. |
| **Sıklık** | Tetikleyici yineleneceği sıklığı birimidir. "Dakika" desteklenen değerler arasında "saati", "gün" "hafta" ve "ay." |
| **interval** | İçin bir aralığı gösterir pozitif bir tamsayı **sıklığı** tetikleyici ne sıklıkta çalıştırılacağını belirler değeri. Örneğin, varsa **aralığı** 3 ve **sıklığı** "hafta" 3 haftada tetikleyici yinelenen. |
| **schedule** | Yinelenme zamanlaması için tetikleyici. Belirtilen bir tetikleyici **sıklığı** değeri yinelenme zamanlamaya göre kendi yinelenme değiştirir. **Zamanlama** özelliği, dakika, saat, hafta, ay gün ve hafta numarasını göre yinelenme ilgili değişiklikleri içerir.


### <a name="schema-defaults-limits-and-examples"></a>Şema Varsayılanları, sınırlar ve örnekler

| JSON özelliği | Tür | Gerekli | Varsayılan değer | Geçerli değerler | Örnek |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Dize | Evet | None | ISO-8601 Tarih-Saatleri | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Nesne | Evet | None | Yinelenme nesnesi | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Sayı | Hayır | 1 | 1 1. 000'ı için | `"interval":10` |
| **endTime** | Dize | Evet | Hiçbiri | Gelecekteki bir zamanı temsil eden bir tarih-saat değeri. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Nesne | Hayır | Hiçbiri | Zamanlama nesnesi | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>startTime özelliği
Aşağıdaki tabloda gösterilmektedir nasıl **startTime** özelliği çalıştırmak bir tetikleyici denetler:

| startTime değeri | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme |
|:--- |:--- |:--- |
| Başlangıç zamanı geçmişte | İlk gelecekteki yürütme zamanı başlangıç zamanından sonra hesaplar ve o anda çalıştırır.<br/><br/>Sonraki yürütmelerde son yürütme saati hesaplamaya göre çalıştırır.<br/><br/>Bu tablonun altındaki örneğe bakın. | Tetikleyici başlatır _Hayır sooner daha_ belirtilen başlangıç saati. Birinci başlangıç saati hesaplanan zamanlamayı temel alır.<br/><br/>Sonraki yürütmelerde yineleme zamanlamaya göre çalışır. |
| Başlangıç zamanı gelecekte veya güncel | Belirtilen başlangıç zamanda bir kez çalışır.<br/><br/>Sonraki yürütmelerde son yürütme saati hesaplamaya göre çalıştırır. | Tetikleyici başlatır _Hayır er_ belirtilen dönemden başlangıç zamanı. Birinci başlangıç saati hesaplanan zamanlamayı temel alır.<br/><br/>Sonraki yürütmelerde yineleme zamanlamaya göre çalışır. |

Başlangıç zamanı geçmişteki bir yineleme, ancak hiçbir zamanlama olduğunda ne olacağını örneği görelim. Geçerli saati olduğunu varsayın `2017-04-08 13:00`, başlangıç saati `2017-04-07 14:00`, ve yinelenme her iki gün. ( **Yineleme** değeri ayarlayarak tanımlanmış **sıklığı** "gün" özelliğine ve **aralığı** özelliğini 2.) Dikkat **startTime** değeri geçmişte ve geçerli saatinden önce.

Bu şartlar altında ilk çalıştırma zamanı `2017-04-09 at 14:00` olacaktır. Zamanlayıcı altyapısı çalıştırma yinelenmelerini başlangıç zamanından itibaren hesaplar. Geçmişteki örnekler dikkate alınmaz. Altyapı gelecekte gerçekleşen bir sonraki örneği kullanır. Bu senaryoda, başlangıç zamanıdır `2017-04-07 at 2:00pm`, o zamandan olan iki gün sonraki örneği olacak şekilde `2017-04-09 at 2:00pm`.

İlk yürütme süresi aynı bile ise **startTime** değer `2017-04-05 14:00` veya `2017-04-01 14:00`. İlk yürütme işleminden sonra sonraki yürütmelerde zamanlama kullanılarak hesaplanır. Sonraki yürütmelerde altındadır bu nedenle, `2017-04-11 at 2:00pm`, ardından `2017-04-13 at 2:00pm`, ardından `2017-04-15 at 2:00pm`ve benzeri.

Son olarak, saat veya dakika ilk yürütme saat veya dakika tetikleyici için zamanlamayı oluşturulmamış olduğunda, varsayılan olarak kullanılır.

### <a name="schedule-property"></a>Zamanlama özelliği
Bir yandan, bir zamanlama kullanımını tetikleyici yürütmeleri sayısını sınırlayabilirsiniz. Örneğin, bir tetikleyici aylık sıklık yalnızca 31 günde çalışacak şekilde zamanlanır, tetikleyici yalnızca 31 gün içeren bu aylarda çalıştırılır.

Diğer yandan zamanlama, tetikleyici çalıştırma sayısını da genişletebilir. Örneğin, ayın günleri 1 ve 2 çalışmak üzere zamanlanmış aylık bir sıklık tetikleyici ayda bir kez yerine ayın 1 ve 2 gün üzerinde çalışır.

Birden çok **zamanlama** öğeleri belirtilirse, Değerlendirme sırasını büyükten en küçük zamanlama ayarı. Değerlendirme ile hafta numarasını ve ardından ay gün, hafta içi günü, saat, başlatır ve son olarak, dakika.

Aşağıdaki tabloda açıklanmaktadır **zamanlama** ayrıntılı öğeleri:


| JSON element | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |
| **dakika** | Tetikleyicinin çalıştığı dakika değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul>
| **saatleri** | Tetikleyicinin çalıştığı saat değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul> |
| **weekDays** | Tetikleyici çalıştığı haftanın günleri. Değer yalnızca bir haftalık sıklığı ile belirtilebilir. | <ul><li>Pazartesi, Salı, Çarşamba, Perşembe, Cuma, Cumartesi Pazar</li><li>(En büyük dizi boyutu 7'dir) gün değerleri dizisi</li><li>Gün değerleri büyük küçük harfe duyarlı değildir</li></ul> |
| **monthlyOccurrences** | Tetikleyici çalıştığı ayın günleri. Değer yalnızca bir aylık sıklığı ile belirtilebilir. | <ul><li>Dizi **monthlyOccurence** nesneleri: `{ "day": day,  "occurrence": occurence }`.</li><li>**Gün** tetikleyici çalıştığı haftanın gününü bir özniteliktir. Örneğin, bir **monthlyOccurrences** özelliği ile bir **gün** değerini `{Sunday}` ayın her Pazar anlamına gelir. **Gün** özniteliği gereklidir.</li><li>**Oluşumu** özniteliktir belirtilen oluşumunu **gün** ay. Örneğin, bir **monthlyOccurrences** özelliğiyle **gün** ve **oluşumu** değerlerini `{Sunday, -1}` ayın son Pazar anlamına gelir. **Oluşumu** özniteliği isteğe bağlıdır.</li></ul> |
| **monthDays** | Tetikleyici çalıştığı ayın günü. Değer yalnızca bir aylık sıklığı ile belirtilebilir. | <ul><li><= -1 ve >= -31 şartlarını karşılayan herhangi bir değer</li><li>>= 1 ve <= 31 şartlarını karşılayan herhangi bir değer</li><li>Değerleri dizisi</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Tetikleyici yineleme zamanlamaları örnekleri
Bu bölümde, yineleme zamanlamaları örnekleri sağlar ve odaklanır **zamanlama** nesne ve öğeleri.

Örnekleri **aralığı** değeri 1 ve o **sıklığı** değerdir zamanlama tanımı göre doğru. Örneğin, olamaz bir **sıklığı** "gün" değerini ve "monthDays" değişiklik de **zamanlama** nesnesi. Bunlar gibi kısıtlamaları, önceki bölümde tabloda açıklanan.

| Örnek | Açıklama |
|:--- |:--- |
| `{"hours":[5]}` | 5: 00'da her gün çalıştırın. |
| `{"minutes":[15], "hours":[5]}` | 5: 15'te her gün çalıştırın. |
| `{"minutes":[15], "hours":[5,17]}` | 5: 15'da ve 17:15:00 saatleri her gün olarak çalışır. |
| `{"minutes":[15,45], "hours":[5,17]}` | 5: 15'te, 5: 45'te, çalıştırmak 17:15:00 saatleri ve 5:45 PM her gün. |
| `{"minutes":[0,15,30,45]}` | Her 15 dakikada çalıştırın. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Her saat çalıştırın. Bu tetikleyici saatte bir çalışır. Dakika denetlediği **startTime** bir değeri belirtildiğinde değeri. Bir değer belirtilmezse, dakika oluşturulma tarihine göre denetlenir. Başlangıç saati veya oluşturma zamanı (hangisi) saat 12: 25'e ise, tetikleyici 00:25 ', 01:25, örneğin, çalışan 02:25,... ve 23:25.<br/><br/>Bu zamanlamayı tetikleyici ile zorunda eşdeğer olan bir **sıklığı** "saat" değerini bir **aralığı** değeri 1 ve Hayır **zamanlama**.  Bu zamanlama ile farklı kullanılabilir **sıklığı** ve **aralığı** başka Tetikleyicileri oluşturmak için değerleri. Örneğin, **sıklığı** değerdir "ay", her gün yerine yalnızca ayda bir zamanlama çalıştırır zaman **sıklığı** değerdir "day." |
| `{"minutes":[0]}` | Her saat, saat üzerinde çalıştırın. Bu tetikleyici her saat 12: 00'da, 1: 00'da, 2: 00'da, başlangıç saati çalışır ve benzeri.<br/><br/>Bu zamanlamayı tetikleyici ile eşdeğerdir bir **sıklığı** "saat" değerini ve **startTime** değeri sıfır dakika veya no **zamanlama** ancak **sıklığı**  değeri günün"." Varsa **sıklığı** değerdir "hafta" veya "ay" zamanlama bir gün, hafta veya ayda bir gün yürütür yalnızca sırasıyla. |
| `{"minutes":[15]}` | Başından 15 dakika sonra her saat çalıştırın. 00: 15'de, 1: 15'da, 2: 15'da, başlangıç saati aşan 15 dakika, bu tetikleyici her saat çalışır ve vb. ve 11: 15'te adresindeki bitiş. |
| `{"hours":[17], "weekDays":["saturday"]}` | Haftada 5:00 PM Cumartesi günleri çalıştırmanıza. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Haftada 5:00 PM Pazartesi, Çarşamba ve Cuma çalıştırmanıza. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | 17:15:00 saatleri ve 5:45 PM Pazartesi, Çarşamba ve Cuma her hafta çalıştırın. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Her 15 dakikada günlerinde çalıştırın. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Hafta içi günlerde 9:00 ve 4:45 arasında her 15 dakikada çalıştırın. |
| `{"weekDays":["tuesday", "thursday"]}` | Salı ve Perşembe günleri saat belirtilen başlangıç saatinde çalıştırın. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | 6: 00'da, her ayın 28 gününde Çalıştır (varsayarak bir **sıklığı** "ay" değeri). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | 6: 00'da ayın son gününde Çalıştır. Bir tetikleyici bir ayın son günü çalıştırmak için -1 28, 29, 30 ve 31 gün yerine kullanın. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | 6: 00'da, her ayın ilk ve son gününde Çalıştır. |
| `{monthDays":[1,14]}` | Belirtilen başlangıç saatinde her ayın ilk ve 14 gün çalıştırın. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | 5: 00'da her ayın ilk Cuma çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Belirtilen başlangıç saatinde her ayın ilk Cuma çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Belirtilen başlangıç saatinde her ay ay sonundan üçüncü Cuma çalıştırın. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | İlk ve son Cuma her ayın 5: 15'da çalışır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | İlk ve son Cuma her ayın belirtilen başlama saatinde Çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Her ayın belirtilen başlangıç saatinde beşinci Cuma çalıştırın. Bir ay içinde hiçbir beşinci Cuma olduğunda yalnızca beşinci Cuma günleri çalışmak üzere zamanlanmış olduğundan işlem hattı çalıştırmaz. Tetikleyici çalıştırmak için son 5 için yerine -1 kullanmayı düşünün Cuma günü, gerçekleşen **oluşumu** değeri. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Ayın son Cuma günü 15 dakikada bir çalışır. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | 5: 15'te, 5: 45'te, çalıştırmak 17:15:00 saatleri ve 5:45 PM her ayın üçüncü Çarşamba günü. |


## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz: [kanal yürütme ve Tetikleyicileri](concepts-pipeline-execution-triggers.md#triggers).
