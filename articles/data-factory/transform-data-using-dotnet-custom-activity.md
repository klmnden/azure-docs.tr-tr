---
title: "Bir Azure Data Factory işlem hattında özel etkinlikler kullanma"
description: "Özel etkinlikler oluşturmak ve bunları bir Azure Data Factory ardışık düzeninde öğrenin."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shengc
ms.openlocfilehash: 4b9714bc456ad28d9dd46742ca16f52e68c61399
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
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
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
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
            Console.WriteLine(linkedServices[0].properties.typeProperties.connectionString.value);
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

## <a name="difference-between-custom-activity-in-azure-data-factory-version-2-and-custom-dotnet-activity-in-azure-data-factory-version-1"></a>Azure Data Factory sürüm 2 özel etkinlik ve Azure Data Factory sürüm 1 (özel) DotNet etkinlik arasındaki fark

  Azure Data Factory sürüm 1, (özel) DotNet Etkinlik kodu bir .net oluşturarak uygulamanız yürütme yönteminin IDotNetActivity arabirimi uygulayan bir sınıf olan sınıf kitaplığı proje. Bağlı hizmetler, veri kümeleri ve genişletilmiş özellikler (özel) DotNet etkinlik JSON yükündeki yürütme yöntemi güçlü yazılan nesnelerin geçirilir. Ayrıntılar için başvurmak [sürüm 1 (özel) DotNet](v1/data-factory-use-custom-activities.md). Bu uygulama nedeniyle özel kodunuzu .net yazılması gereken Framework 4.5.2 ve Azure Batch havuzu Windows tabanlı düğümlerinde yürütülür. 

  Azure veri fabrikası V2 özel etkinlik, bir .net arabirimini uygulayan gerekmez. Şimdi doğrudan komutlar, komut dosyaları, çalıştırabilir ve kendi özel çalıştırmak kod derlendiğini olarak çalıştırılabilir. Bunu folderPath özelliği ile birlikte komut özelliğini belirterek elde edin. Özel Etkinlik çalıştırılabilir ve folderpath bağımlılıkları yükler ve sizin için komutu çalıştırır. 

  Bağlı hizmetler, veri kümeleri (referenceObjects içinde tanımlanan) ve genişletilmiş özel etkinlik JSON yükü tanımlanan özellikler, yürütülebilir dosya tarafından JSON dosyaları olarak erişilebilir. Yukarıdaki SampleApp.exe kod örneğinde gösterildiği gibi JSON seri hale getirici kullanarak gerekli özellikleri erişebilir. 

  Azure veri fabrikası V2 özel etkinlik sunulan değişiklikler ile özel kod mantığınızı tercih ettiğiniz dili yazmak ve bunları Windows ve Linux işletim sistemi Azure Batch tarafından desteklenen yürütmek boş. 

  Aşağıdaki tabloda veri fabrikası V2 özel etkinliği ve Data Factory sürüm 1 (özel) arasındaki farklar açıklanmaktadır DotNet etkinlik: 


|Farkları      |sürüm 2 özel etkinlik      | Sürüm 1 (özel) DotNet etkinliği      |
| ---- | ---- | ---- |
|Nasıl özel mantık tanımlanır      |(Mevcut veya kendi yürütülebilir uygulama) herhangi bir yürütülebilir dosya çalıştırarak      |Bir .net DLL uygulayarak      |
|Özel mantık yürütme ortamı      |Windows veya Linux      |Windows (.Net Framework 4.5.2)      |
|Betikleri yürütülüyor      |Betikleri Yürütülüyor doğrudan (örneğin "cmd /c echo hello world" Windows VM üzerinde) desteği      |DLL .net uygulamasında gerektirir      |
|Veri kümesi gerekiyor      |İsteğe bağlı      |Etkinlikler zincir ve bilgi aktarmak için gerekli      |
|Özel mantık etkinliğinden geçiş bilgileri      |ReferenceObjects (LinkedServices ve veri kümeleri) ve ExtendedProperties (Özel Özellikler) aracılığıyla      |ExtendedProperties (özel özellikleri), giriş ve çıkış veri kümeleri      |
|Özel mantık bilgilerini alma      |Parse activity.json, linkedServices.json, and datasets.json stored in the same folder of the executable      |.NET SDK'sı (.Net çerçevesi 4.5.2)      |
|Günlüğe kaydetme      |STDOUT doğrudan Yazar      |Günlükçü .net DLL uygulama      |


  Sürüm 1 (özel) DotNet etkinlik yazılan varolan .net kodu varsa, kodunuz için bunları sürümüyle aşağıdaki üst düzey yönergeleri ile 2 özel etkinlik çalışması değiştirmeniz gerekir:  

   - Bir .net projesini değiştirebilir Sınıf Kitaplığı'na bir konsol uygulaması. 
   - Main yöntemi ile uygulamanızı başlatın, yürütme yönteminin IDotNetActivity arabiriminin artık gerekli değildir. 
   - Okuma ve JSON seri hale getirici yerine ile bağlı hizmetler, veri kümeleri ve etkinlik güçlü yazılan nesnelerin ayrıştırma ve ana özel kod mantığınızı gerekli özelliklerinin değerlerini geçirin. Örnek olarak SampleApp.exe kod önceki bakın. 
   - Günlükçü nesne artık desteklenmeyen, yürütülebilir çıktıları konsola yazdırma olabilir ve stdout.txt için kaydedilir. 
   - Microsoft.Azure.Management.DataFactories NuGet paketi artık gerekli değildir. 
   - Kodunuzu derleyin, Azure depolama alanına çalıştırılabilir ve bağımlılıkları yükleme ve yolun folderPath özelliğinde tanımlayın. 

Uçtan uca DLL ve ardışık örnek veri fabrikası sürümünde 1 belge nasıl açıklanan tam bir örnek için [bir Azure Data Factory ardışık düzeninde özel etkinlikleri kullanmak](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) Data Factory sürüm 2 özel etkinlik stili yazılabilir. Başvurduğu bir [Data Factory sürüm 2 özel etkinlik örnek](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFv2CustomActivitySample). 

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
* [MapReduce activity](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
