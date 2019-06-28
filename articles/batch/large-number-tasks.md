---
title: Çok sayıda görevler - Azure Batch gönderme | Microsoft Docs
description: Verimli bir şekilde çok sayıda görevleri tek bir Azure Batch işi gönderme
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 08/24/2018
ms.author: lahugh
ms.custom: ''
ms.openlocfilehash: 0aff792d7e005fb17ebec0715ca3ac7237fd7a71
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341000"
---
# <a name="submit-a-large-number-of-tasks-to-a-batch-job"></a>Çok sayıda Batch işi görevlere gönderin

Büyük ölçekli Azure Batch iş yüklerinizi çalıştırdığınızda, on binlerce, yüzlerce, binlerce veya daha fazla görevleri tek bir iş göndermek isteyebilirsiniz. 

Bu makalede, rehberlik ve görevler için tek bir Batch işi, önemli ölçüde artması ile çok sayıda göndermek için bazı kod örnekleri sağlar. Görevleri gönderdikten sonra toplu iş kuyruğu işlenmek üzere iş için belirttiğiniz havuzunda girin.

## <a name="use-task-collections"></a>Görev koleksiyonları kullanın

Batch API'lerine görevleri bir iş olarak verimli bir şekilde eklemek için yöntemler sağlar bir *koleksiyon*, bir kerede yanı sıra. Çok sayıda görev eklerken bir koleksiyonu olarak görev eklemek için uygun yöntemleri veya aşırı kullanmanız gerekir. Genellikle, işiniz için bir giriş dosyaları veya parametreler kümesi yineleme gibi görevler tanımlayarak bir görev koleksiyonu oluşturun.

Tek bir çağrıda ekleyebileceğiniz görev koleksiyonu en büyük boyutunu kullandığınız Batch API'ye bağlıdır:

* Aşağıdaki Batch API'leri koleksiyona sınırlama **100 görevleri**. Sınır, çok sayıda kaynak dosyaları veya ortam değişkenlerini görevler varsa, daha küçük görevler - örneğin boyutuna bağlı olarak olabilir.

    * [REST API](/rest/api/batchservice/task/addcollection)
    * [Python API’si](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python)
    * [Node.js API’si](/javascript/api/azure-batch/task?view=azure-node-latest)

  Bu API'leri kullanarak, koleksiyon sınırını karşılamak için ve ayrıca görev başarısız olursa, hataları ve yeniden deneme işlemek için görevlerin sayısını ayırmak için mantıksal vermeniz gerekir. Bir görev koleksiyonu eklemek için çok büyük ise, istek bir hata oluşturur ve daha az görevleriyle yeniden denenmesi gerekiyor.

* Aşağıdaki API'leri, çok daha büyük görev koleksiyonlar - RAM kullanılabilirliğine gönderen istemci tarafından yalnızca sınırlı destekler. Bu API'ler ayrıca görevlerin başarısız olursa görev koleksiyonu "öbeklere" alt düzey API'ler ve yeniden deneme için bölme şeffaf bir şekilde işleyin.

    * [.NET API’si](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync?view=azure-dotnet)
    * [Java API’si](/java/api/com.microsoft.azure.batch.protocol.tasks.addcollectionasync?view=azure-java-stable)
    * [Azure Batch CLI uzantısı](batch-cli-templates.md) Batch CLI şablonlarını ile
    * [Python SDK'sı uzantısı](https://pypi.org/project/azure-batch-extensions/)

## <a name="increase-throughput-of-task-submission"></a>Görev gönderimi verimini artırma

Bu, büyük bir görev koleksiyonu için bir iş - örneğin eklemek için biraz zaman alabilir, 20.000 eklemek için 1 dakika yukarı .NET API aracılığıyla görevler. Batch API'sini ve iş yükünüz bağlı olarak, bir veya daha fazlasını değiştirerek görev aktarım hızını artırabilirsiniz:

* **Görev boyutu** -geniş kapsamlı görevlerin ekleyerek daha küçük depolara eklemeye kıyasla daha uzun sürer. Bir koleksiyondaki her görev boyutunu azaltmak için görev komut satırı basitleştirin, ortam değişkenleri sayısını azaltın veya gereksinimler için görev yürütme daha verimli bir şekilde işlemek. Örneğin, çok sayıda kaynak dosyalarını kullanmak yerine, görev bağımlılıklarını kullanarak yüklemeniz bir [başlangıç görevi](batch-api-basics.md#start-task) kullanın ve havuz üzerinde bir [uygulama paketi](batch-application-packages.md) veya [Docker kapsayıcısı](batch-docker-container-workloads.md).

* **Paralel işlemleri sayısı** - Batch API'si, Batch istemcisi eşzamanlı işlemlerin sayısını artırarak artışı işleme bağlı olarak. Bu ayarı kullanarak yapılandırma [BatchClientParallelOptions.MaxDegreeOfParallelism](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) .NET API özelliğinde veya `threads` gibi yöntemlerin parametre [TaskOperations.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python)Batch Python SDK'sını uzantısı. (Bu özellik yerel Batch Python SDK'sını içinde kullanılabilir değil.) Varsayılan olarak, bu özelliği 1 olarak ayarlanmış, ancak işlemlerinin verimini artırmak için daha yüksek ayarlayın. Alıcı ağ bant genişliği olarak artan iş hacmi ve bazı CPU performansı takas edebilir. Görev verimliliğini artırır, en fazla 100 kat `MaxDegreeOfParallelism` veya `threads`. Uygulamada, 100'den düşük eşzamanlı işlemlerin sayısını ayarlamanız gerekir. 
 
  Batch şablonlarını ile Azure Batch CLI uzantısı kullanılabilir çekirdek sayısı temel alınarak otomatik olarak eşzamanlı işlemlerin sayısını artırır, ancak bu özellik CLI'daki yapılandırılabilir değildir. 

* **HTTP bağlantı sınırları** -HTTP eş zamanlı bağlantı sayısı çok sayıda görev eklerken Batch istemci performansını kısıtlayabilirsiniz. HTTP bağlantı sayısı, belirli API'leri ile sınırlıdır. .NET API ile Örneğin, geliştirirken [ServicePointManager.DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit) özelliği varsayılan olarak 2'ye ayarlanır. Paralel işlemleri sayısına yakın veya bu değerden sayı değerini artırmanız önerilir.

## <a name="example-batch-net"></a>Örnek: Batch .NET

Aşağıdaki C# kod parçacıkları, çok sayıda Batch .NET API'si kullanılarak görevleri eklerken yapılandırmak için ayarları görüntüleyin.

Görev verimliliği artırmak için değeri artırmak [Maxanalyticsunits](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) özelliği [BatchClient](/dotnet/api/microsoft.azure.batch.batchclient?view=azure-dotnet). Örneğin:

```csharp
BatchClientParallelOptions parallelOptions = new BatchClientParallelOptions()
  {
    MaxDegreeOfParallelism = 15
  };
...
```
Uygun bir aşırı yüklemesini kullanarak işine bir görev koleksiyonu eklemek [AddTaskAsync](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync?view=azure-dotnet) veya [AddTask](/dotnet/api/microsoft.azure.batch.cloudjob.addtask?view=azure-dotnet
) yöntemi. Örneğin:

```csharp
// Add a list of tasks as a collection
List<CloudTask> tasksToAdd = new List<CloudTask>(); // Populate with your tasks
...
await batchClient.JobOperations.AddTaskAsync(jobId, tasksToAdd, parallelOptions);
```


## <a name="example-batch-cli-extension"></a>Örnek: Batch CLI uzantısı

Azure Batch CLI uzantıları kullanarak [Batch CLI şablonlarını](batch-cli-templates.md), içeren bir proje şablonu JSON dosyası oluşturun bir [görev fabrikasını](https://github.com/Azure/azure-batch-cli-extensions/blob/master/doc/taskFactories.md). Görev fabrikasını ilgili görevler için tek bir görev tanımı bir işten koleksiyonunu yapılandırır.  

Örnek Proje şablonu için bir tek boyutlu parametreli Süpürme işlemiyle çok sayıda görevler - bu durumda, 250.000 verilmiştir. Basit görev komut satırında bir `echo` komutu.

```json
{
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "myjob",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "mypool"
            },
            "taskFactory": {
                "type": "parametricSweep",
                "parameterSets": [
                    {
                        "start": 1,
                        "end": 250000,
                        "step": 1
                    }
                ],
                "repeatTask": {
                    "commandLine": "/bin/bash -c 'echo Hello world from task {0}'",
                    "constraints": {
                        "retentionTime":"PT1H"
                    }
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```
Şablonu ile bir işi çalıştırmak için bkz: [kullanımı Azure Batch CLI şablonlarını ve dosya aktarımı](batch-cli-templates.md).

## <a name="example-batch-python-sdk-extension"></a>Örnek: Toplu işlem Python SDK'sı uzantısı

Azure Batch Python SDK'sını uzantısını kullanmak için ilk Python SDK'sını ve uzantıyı yükleyin:

```
pip install azure-batch
pip install azure-batch-extensions
```

Ayarlanmış bir `BatchExtensionsClient` SDK uzantısı kullanan:

```python

client = batch.BatchExtensionsClient(
    base_url=BATCH_ACCOUNT_URL, resource_group=RESOURCE_GROUP_NAME, batch_account=BATCH_ACCOUNT_NAME)
...
```

Bir projeye eklemek için görevler koleksiyonu oluşturun. Örneğin:


```python
tasks = list()
# Populate the list with your tasks
...
```

Görev koleksiyon kullanarak ekleme [task.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python). Ayarlama `threads` eşzamanlı işlemlerin sayısını artırmak için parametre:

```python
try:
    client.task.add_collection(job_id, threads=100)
except Exception as e:
    raise e
```

Batch Python SDK'sını uzantısı bir JSON belirtimi için bir görev fabrikasını kullanarak proje görev parametreler eklemeyi de destekler. Örneğin, önceki Batch CLI şablon örnekte gösterilene benzer bir parametreli Süpürme iş parametrelerini yapılandırın:

```python
parameter_sweep = {
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "myjob",
            "poolInfo": {
                "poolId": "mypool"
            },
            "taskFactory": {
                "type": "parametricSweep",
                "parameterSets": [
                    {
                        "start": 1,
                        "end": 250000,
                        "step": 1
                    }
                ],
                "repeatTask": {
                    "commandLine": "/bin/bash -c 'echo Hello world from task {0}'",
                    "constraints": {
                        "retentionTime": "PT1H"
                    }
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
...
job_json = client.job.expand_template(parameter_sweep)
job_parameter = client.job.jobparameter_from_json(job_json)
```

İş parametresi projeye ekleyin. Ayarlama `threads` eşzamanlı işlemlerin sayısını artırmak için parametre:

```python
try:
    client.job.add(job_parameter, threads=50)
except Exception as e:
    raise e
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Batch CLI uzantısı ile kullanma hakkında daha fazla bilgi [Batch CLI şablonlarını](batch-cli-templates.md).
* Daha fazla bilgi edinin [Batch Python SDK'sını uzantısı](https://pypi.org/project/azure-batch-extensions/).
