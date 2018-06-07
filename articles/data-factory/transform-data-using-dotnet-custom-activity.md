---
title: Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
description: Özel etkinlikler oluşturmak ve bunları bir Azure Data Factory ardışık düzeninde öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 345ea6f91593e14ff19616f5512916ee77f38486
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619958"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-use-custom-activities.md)
> * [Sürüm 2 - Önizleme](transform-data-using-dotnet-custom-activity.md)

İki tür kullanabileceğiniz bir Azure Data Factory ardışık düzeninde etkinlik yok.

- [Veri taşıma etkinlikleri](copy-activity-overview.md) arasında veri taşımak için [desteklenen kaynak ve havuz veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
- [Veri dönüştürme etkinlikleri](transform-data.md) verileri dönüştürmek için kullanarak işlem hizmetlerini Azure Hdınsight, Azure Batch ve Azure Machine Learning gibi. 

Taşımak için veri/verileri Data Factory desteklemiyor veya veri fabrikası tarafından desteklenmeyen bir şekilde veri dönüştürme/işlemi için oluşturabileceğiniz depolamak bir **özel etkinlik** kendi veri taşıma veya dönüştürme mantığı ve kullanımı ardışık düzeninde etkinlik. Özel Etkinlik özelleştirilmiş kodu mantığınızı üzerinde çalıştığı bir **Azure Batch** sanal makinelerin havuzu.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory sürüm 1 (özel) DotNet etkinliğinde](v1/data-factory-use-custom-activities.md).
 

Azure Batch hizmetine yeniyseniz makaleleri aşağıdaki bakın:

* [Azure Batch Temelleri](../batch/batch-technical-overview.md) Azure Batch hizmetinin genel bakış.
* [New-AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) bir Azure Batch hesabı oluşturmak için cmdlet'i (veya) [Azure portal](../batch/batch-account-create-portal.md) Azure portalını kullanarak Azure Batch hesabı oluşturmak için. Bkz: [Azure Batch hesabını yönetmek için PowerShell kullanarak](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) makale cmdlet kullanma hakkında ayrıntılı yönergeler için.
* [Yeni-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) bir Azure Batch havuzu oluşturmak için cmdlet'i.

## <a name="azure-batch-linked-service"></a>Azure Batch bağlı hizmeti 
Aşağıdaki JSON örnek bir Azure Batch bağlantılı hizmeti tanımlar. Ayrıntılar için bkz [işlem Azure Data Factory ile desteklenen ortamlar](compute-linked-services.md)

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "batchaccount",
            "accessKey": {
                "type": "SecureString",
                "value": "access key"
            },
            "batchUri": "https://batchaccount.region.batch.azure.com",
            "poolName": "poolname",
            "linkedServiceName": {
                "referenceName": "StorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
}
```

 Azure Batch bağlantılı hizmeti hakkında daha fazla bilgi için bkz: [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. 

## <a name="custom-activity"></a>Özel etkinlik

Aşağıdaki JSON parçacığı basit bir özel etkinliği ile işlem hattı tanımlar. Etkinlik tanımı bağlı Azure Batch hizmeti bir başvuru içeriyor. 

```json
{
    "name": "MyCustomActivityPipeline",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "helloworld.exe",
          "folderPath": "customactv2/helloworld",
          "resourceLinkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }]
    }
  }
```

Bu örnekte, helloworld.exe resourceLinkedService kullanılan Azure depolama hesabının customactv2/helloworld klasöründe depolanan bir özel bir uygulamadır. Özel Etkinlik Azure Batch yürütülmek üzere özel bu uygulama gönderir. Hedef Azure Batch havuzu düğümlerin işletim sistemi yürütülebilir herhangi bir tercih edilen uygulama komutu değiştirebilirsiniz. 

Aşağıdaki tabloda, adları ve açıklamaları bu etkinliğe özgü özellikleri açıklanmaktadır. 

| Özellik              | Açıklama                              | Gerekli |
| :-------------------- | :--------------------------------------- | :------- |
| ad                  | İşlem hattında etkinlik adı     | Evet      |
| açıklama           | Etkinlik yaptığı açıklayan metin.  | Hayır       |
| type                  | Özel Etkinlik için etkinlik türüdür **özel**. | Evet      |
| linkedServiceName     | Azure toplu işlem için bağlı hizmeti. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.  | Evet      |
| komutu               | Yürütülecek özel uygulama komutu. Uygulama zaten Azure Batch havuzu düğüm üzerinde kullanılabilir haldeyse, folderPath ve resourceLinkedService atlanabilir. Örneğin, olması için komutu belirtebilirsiniz `cmd /c dir`, yerel olarak desteklendiği Windows Batch havuzu düğümün. | Evet      |
| resourceLinkedService | Özel uygulama depolandığı depolama hesabı Azure Storage bağlı hizmeti | Hayır       |
| folderPath            | Özel uygulama ve onun bağımlılıklarını klasör yolu | Hayır       |
| referenceObjects      | Varolan bağlı hizmetleri ve veri kümeleri dizisi. Özel kod kaynakları Data Factory başvurabilir şekilde başvurulan bağlı hizmetler ve veri kümelerini JSON biçiminde özel uygulama geçirilecek | Hayır       |
| extendedProperties    | Özel kod ek özellikler başvurabilir, böylece özel uygulama JSON biçiminde geçirilecek kullanıcı tanımlı Özellikler | Hayır       |

## <a name="executing-commands"></a>Komutları çalıştırma

Özel Etkinlik kullanarak bir komut doğrudan yürütebilir. Aşağıdaki örnek hedef Azure Batch havuzu düğümlerinde "echo hello world" komutu çalıştırır ve çıktısını Stdout'a yazdırır. 

  ```json
  {
    "name": "MyCustomActivity",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "cmd /c echo hello world"
        }
      }]
    }
  } 
  ```

## <a name="passing-objects-and-properties"></a>Nesneleri ve özellikleri geçirme

Bu örnek, veri fabrikası nesneleri ve özellikleri kullanıcı tanımlı özel uygulamanızı geçirmek için referenceObjects ve extendedProperties nasıl kullanabileceğinizi gösterir. 


```json
{
  "name": "MyCustomActivityPipeline",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "SampleApp.exe",
        "folderPath": "customactv2/SampleApp",
        "resourceLinkedService": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        },
        "referenceObjects": {
          "linkedServices": [{
            "referenceName": "AzureBatchLinkedService",
            "type": "LinkedServiceReference"
          }]
        },
        "extendedProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "aSampleSecureString"
            },
            "PropertyBagPropertyName1": "PropertyBagValue1",
            "propertyBagPropertyName2": "PropertyBagValue2",
            "dateTime1": "2015-04-12T12:13:14Z"              
        }
      }
    }]
  }
}
```

Etkinlik çalıştırıldığında referenceObjects ve extendedProperties SampleApp.exe aynı yürütme klasöre dağıtılan aşağıdaki dosyaları depolanır: 

- activity.json

  ExtendedProperties ve özel etkinlik özelliklerini depolar. 

- linkedServices.json

  Depoları bağlı hizmetler dizisi referenceObjects özelliği içinde tanımlı. 

- DataSets.JSON

  Depoları veri kümeleri dizisi referenceObjects özelliği içinde tanımlı. 

Örnek kod SampleApp.exe JSON dosyalarından gerekli bilgileri nasıl erişebileceğiniz gösterilmektedir: 

```csharp
using Newtonsoft.Json;
using System;
using System.IO;

namespace SampleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            //From Extend Properties
            dynamic activity = JsonConvert.DeserializeObject(File.ReadAllText("activity.json"));
            Console.WriteLine(activity.typeProperties.extendedProperties.connectionString.value);

            // From LinkedServices
            dynamic linkedServices = JsonConvert.DeserializeObject(File.ReadAllText("linkedServices.json"));
            Console.WriteLine(linkedServices[0].properties.typeProperties.accountName);
        }
    }
}
```

## <a name="retrieve-execution-outputs"></a>Yürütme çıkışları alma

  Aşağıdaki PowerShell komutunu kullanarak bir ardışık düzen çalıştırma başlatabilirsiniz: 

  ```.powershell
  $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
  ```
  Ardışık Düzen çalışırken, aşağıdaki komutları kullanarak yürütme çıktısını denetleyebilirsiniz: 

  ```.powershell
  while ($True) {
      $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

      if(!$result) {
          Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
      }
      elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
          Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
      }
      else {
          Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
          $result
          break
      }
      ($result | Format-List | Out-String)
      Start-Sleep -Seconds 15
  }

  Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
  $result.Output -join "`r`n"

  Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
  $result.Error -join "`r`n"
  ```

  **Stdout** ve **stderr** özel uygulamanızın kaydedilir **adfjobs** Azure depolama bağlantılı Azure Batch bağlantılı oluştururken tanımladığınız hizmet kapsayıcısında Hizmet görevinin GUID. Aşağıdaki kod parçacığında gösterildiği gibi ayrıntılı yol etkinliği çalıştırmak çıktısını alabilirsiniz: 

  ```shell
  Pipeline ' MyCustomActivity' run finished. Result:

  ResourceGroupName : resourcegroupname
  DataFactoryName   : datafactoryname
  ActivityName      : MyCustomActivity
  PipelineRunId     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
  PipelineName      : MyCustomActivity
  Input             : {command}
  Output            : {exitcode, outputs, effectiveIntegrationRuntime}
  LinkedServiceName : 
  ActivityRunStart  : 10/5/2017 3:33:06 PM
  ActivityRunEnd    : 10/5/2017 3:33:28 PM
  DurationInMs      : 21203
  Status            : Succeeded
  Error             : {errorCode, message, failureType, target}

  Activity Output section:
  "exitcode": 0
  "outputs": [
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stdout.txt",
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stderr.txt"
  ]
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
  Activity Error section:
  "errorCode": ""
  "message": ""
  "failureType": ""
  "target": "MyCustomActivity"
  ```
Aşağı Akış etkinlikleri stdout.txt içeriği kullanmak istiyorsanız, ifadede stdout.txt dosyasının yolunu alabilirsiniz "@activity('MyCustomActivity').output.outputs [0]". 

  > [!IMPORTANT]
  > - Activity.json, linkedServices.json ve datasets.json toplu görev çalışma zamanı klasöründe depolanır. Bu örnekte, activity.json, linkedServices.json ve datasets.json depolanmış "https://adfv2storage.blob.core.windows.net/adfjobs/<GUID>/runtime/" yolu. Gerekirse, ayrı ayrı temizlenmesi gerekir. 
  > - Self-Hosted tümleştirme çalışma zamanı, anahtarlar veya parolalar gibi hassas bilgileri Self-Hosted tümleştirme kimlik bilgileri sağlamak için çalışma zamanı tarafından şifrelenmiş bağlı hizmetler kullanımlar için özel ağ ortamı kalır müşteri tanımlı. Bu şekilde, özel uygulama kodu tarafından başvurulduğunda bazı önemli alanlar eksik olabilir. Bağlantılı hizmet başvurusu gerekirse kullanmak yerine extendedProperties SecureString kullanın. 

## <a name="compare-v2-v1"></a> V2 özel etkinlik ve sürüm 1 (özel) karşılaştırmak DotNet etkinliği

  Azure Data Factory sürüm 1, (özel) DotNet etkinliği bir .net oluşturarak uygulamanız uygulayan bir sınıf olan sınıf kitaplığı proje `Execute` yöntemi `IDotNetActivity` arabirimi. Bağlı hizmetler, veri kümeleri ve genişletilmiş özellikler (özel) DotNet etkinlik JSON yükündeki yürütme yöntemi türü kesin belirlenmiş nesnelerin geçirilir. Sürüm 1 davranışı hakkında daha fazla ayrıntı için bkz: [sürüm 1 (özel) DotNet](v1/data-factory-use-custom-activities.md). Bu uygulama nedeniyle .net hedeflemek sürüm 1 DotNet etkinlik kodunuzu sahip Framework 4.5.2. Sürüm 1 DotNet etkinlik Azure Batch havuzu Windows tabanlı düğümlerinde çalıştırılacak da sahiptir. 

  Azure veri fabrikası V2 özel etkinliğinde .net arabirimini uygulayan gerekmez. Şimdi doğrudan komutlar, komut dosyaları ve yürütülebilir bir dosya derlenmiş kendi özel kod çalıştırabilirsiniz. Bu uygulama yapılandırmak için belirttiğiniz `Command` özelliği ile birlikte `folderPath` özelliği. Özel Etkinlik yürütülebilir ve onun bağımlılıklarını yükler `folderpath` ve sizin için komutu yürütür. 

  Bağlı hizmetler, veri kümeleri (referenceObjects içinde tanımlanan) ve genişletilmiş özel etkinlik olarak JSON dosyaları, yürütülebilir dosya tarafından erişilebilir bir veri fabrikası v2 JSON yükü tanımlanan özellikler. Yukarıdaki SampleApp.exe kod örneğinde gösterildiği gibi JSON seri hale getirici kullanarak gerekli özellikleri erişebilir. 

  Veri Fabrikası V2 özel etkinlik sunulan değişiklikler ile özel kod mantığınızı tercih ettiğiniz dili yazma ve Windows ve Linux işlemi Azure Batch tarafından desteklenen sistemleri üzerinde çalıştırın. 

  Veri Fabrikası V2 özel etkinliği ve Data Factory sürüm 1 arasındaki farklar (özel) aşağıdaki tabloda açıklanmaktadır DotNet etkinlik: 


|Farkları      |sürüm 2 özel etkinlik      | Sürüm 1 (özel) DotNet etkinliği      |
| ---- | ---- | ---- |
|Nasıl özel mantık tanımlanır      |Yürütülebilir bir dosya sağlayarak      |Bir .net DLL uygulayarak      |
|Özel mantık yürütme ortamı      |Windows veya Linux      |Windows (.Net Framework 4.5.2)      |
|Betikleri yürütülüyor      |Doğrudan komut dosyaları (örneğin "cmd /c echo hello world" Windows VM üzerinde) yürütme destekler      |DLL .net uygulamasında gerektirir      |
|Veri kümesi gerekiyor      |İsteğe bağlı      |Etkinlikler zincir ve bilgi aktarmak için gerekli      |
|Özel mantık etkinliğinden geçiş bilgileri      |ReferenceObjects (LinkedServices ve veri kümeleri) ve ExtendedProperties (Özel Özellikler) aracılığıyla      |ExtendedProperties (özel özellikleri), giriş ve çıkış veri kümeleri      |
|Özel mantık bilgilerini alma      |Activity.JSON, linkedServices.json ve yürütülebilir aynı klasörde depolanan datasets.json ayrıştırır      |.NET SDK'sı (.Net çerçevesi 4.5.2)      |
|Günlüğe kaydetme      |STDOUT doğrudan Yazar      |Günlükçü .net DLL uygulama      |


  Var olan .net kodu için bir sürüm 1 (özel) DotNet etkinlik yazılmış varsa, kodunuz için 2 özel etkinlik bir sürümüyle çalışacak şekilde değiştirmeniz gerekir. Bu üst düzey yönergeleri izleyerek kodunuzu güncelleştirin:  

   - Bir .net projesini değiştirebilir Sınıf Kitaplığı'na bir konsol uygulaması. 
   - Uygulamanızla Başlat `Main` yöntemi. `Execute` Yöntemi `IDotNetActivity` arabirimidir artık gerekli. 
   - Okuma ve JSON seri hale getirici ile türü kesin belirlenmiş nesnelerin olarak değil bağlı hizmetler, veri kümeleri ve etkinlik ayrıştırılamadı. Gerekli özellik değerlerini ana özel kod mantığınızı geçirin. Önceki SampleApp.exe kod örnek olarak bakın. 
   - Günlükçü nesne artık desteklenmiyor. Yürütülebilir dosyanın çıktısını konsola yazdırılabilir ve stdout.txt için kaydedilir. 
   - Microsoft.Azure.Management.DataFactories NuGet paketi artık gerekli değildir. 
   - Kodunuzu derleyin, Azure depolama alanına yürütülebilir ve bağımlılıklarını yükleme ve yolunda tanımlamak `folderPath` özelliği. 

Nasıl uçtan uca DLL ve ardışık örnek veri fabrikası sürümünde 1 makalede açıklanan tam bir örnek için [bir Azure Data Factory ardışık düzeninde özel etkinlikleri kullanmak](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) Data Factory v2 özel bir etkinlik yazılabilir, bakın[ Veri Fabrikası sürüm 2 özel etkinlik örneği](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFv2CustomActivitySample). 

## <a name="auto-scaling-of-azure-batch"></a>Azure batch otomatik olarak ölçeklendirme
Bir Azure Batch havuzuyla oluşturabilirsiniz **otomatik ölçeklendirme** özelliği. Örneğin, 0 özel VM'ler ve beklemedeki görevlerin sayısına dayalı bir otomatik ölçeklendirme formülü ile bir azure batch havuzu oluşturabilirsiniz. 

Formül örneği burada aşağıdaki davranışı elde eder: havuzu başlangıçta oluşturulduğunda 1 VM ile başlar. $PendingTasks ölçüm tanımlar görev sayısı çalışan + (kuyruğa alınmış) etkin durumu.  Formül Son 180 saniye içinde görevleri bekleyen ortalama sayısı bulur ve TargetDedicated uygun şekilde ayarlar. TargetDedicated hiçbir zaman 25 VM'ler gider sağlar. Bu nedenle, yeni görevler gönderildiği haliyle havuzu otomatik olarak büyür ve görevler tamamlanınca VM'ler boş bir birer birer hale ve bu sanal makineleri otomatik ölçeklendirmeyi küçültür. startingNumberOfVMs ve maxNumberofVMs gereksinimlerinize göre ayarlanabilir.

Otomatik ölçeklendirme formülü:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Bkz: [ölçek işlem düğümlerini Azure Batch havuzunda otomatik olarak](../batch/batch-automatic-scaling.md) Ayrıntılar için.

Varsayılan havuzu kullanıyorsanız [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), Batch hizmeti VM özel etkinlik çalıştırmadan önce hazırlamak için 15-30 dakika sürebilir.  Havuz farklı autoScaleEvaluationInterval kullanıyorsanız, Batch hizmeti autoScaleEvaluationInterval + 10 dakika sürebilir.


## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
