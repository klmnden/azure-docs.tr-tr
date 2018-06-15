---
title: Azure batch kapsayıcı iş yükleri | Microsoft Docs
description: Azure Batch kapsayıcı görüntülerden uygulamaları çalıştırmayı öğrenin.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 06/04/2018
ms.author: danlep
ms.openlocfilehash: 4ee8425bb5c3830b029b766aad464df0ffb15f41
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801124"
---
# <a name="run-container-applications-on-azure-batch"></a>Azure Batch kapsayıcı uygulamaları çalıştırma

Azure toplu işlem, çalıştırmak ve toplu işleri Azure ile ilgili bilgi işlem çok sayıda ölçeklendirme sağlar. Batch görevleriniz doğrudan sanal makinelerde (düğümler) Batch havuzunda çalıştırabilirsiniz ancak Docker uyumlu kapsayıcılarında düğümlerinde çalışan görevler için Batch havuzu oluşturmak ayarlayabilirsiniz. Bu makalede çalışan kapsayıcı görevleri destekleyen ve ardından havuzu kapsayıcı görevleri çalıştırmak işlem düğümleri havuzu oluşturulacağını gösterir. 

Kapsayıcı kavramları ve Batch havuzu ve iş nasıl oluşturulacağı hakkında bilginiz olması gerekir. Kod örnekleri Batch .NET ve Python SDK'ları kullanın. Kapsayıcı etkin Batch havuzları oluşturmak ve kapsayıcı görevleri çalıştırmak için diğer toplu SDK'lar ve Araçlar, Azure portal dahil olmak üzere de kullanabilirsiniz.

## <a name="why-use-containers"></a>Kapsayıcıları neden kullanılır?

Kapsayıcıları kullanma bir ortam ve uygulamaları çalıştırmak için bağımlılıkları yönetmek zorunda kalmadan toplu görevleri çalıştırmak için kolay bir yol sağlar. Kapsayıcıları uygulamaları birkaç farklı ortamlarda çalıştırabilirsiniz basit, taşınabilir, sünece birimleri olarak dağıtın. Örneğin, yapı ve test kapsayıcısı yerel olarak ardından Azure veya başka bir kayıt defteri için kapsayıcı görüntüyü karşıya yükleme. Kapsayıcı dağıtım modeli, çalışma zamanı ortamı, uygulamanızın her zaman doğru yüklendiğinden ve uygulamayı barındıran her yerde yapılandırılmış sağlar. Batch'teki görevleri kapsayıcı tabanlı uygulama paketleri ve kaynak dosyaları ve çıkış dosyalarının yönetimi gibi kapsayıcı olmayan görevleri özelliklerini de avantajından yararlanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* **SDK sürümleri**: Batch SDK'ları destek kapsayıcı görüntüleri aşağıdaki sürümlerinden itibaren:
    * Batch REST API'si sürüm 2017 09 01.6.0
    * Batch .NET SDK sürüm 8.0.0
    * Batch Python SDK'sı sürüm 4.0
    * Batch Java SDK'sı sürüm 3.0
    * Toplu Node.js SDK'sı sürüm 3.0

* **Hesapları**: Azure aboneliğinizde bir toplu işlem hesabı ve isteğe bağlı olarak bir Azure Storage hesabı oluşturmanız gerekir.

* **Desteklenen bir VM görüntüsü**: sanal makine yapılandırması ile oluşturulan havuzlarında yalnızca kapsayıcıları desteklenir aşağıdaki bölümde ayrıntılı görüntülerden "sanal makine görüntülerini desteklenmiyor." Özel görüntü sağlarsanız, şu bölümdeki konuları ve gereksinimleri bkz [sanal makinelerin bir havuzu oluşturmak için yönetilen özel görüntü kullanmak](batch-custom-images.md). 

### <a name="limitations"></a>Sınırlamalar

* Toplu iş, yalnızca Linux havuzlarını çalıştıran kapsayıcıları için RDMA desteği sağlar.

## <a name="supported-virtual-machine-images"></a>Desteklenen sanal makine görüntüleri

Desteklenen Windows aşağıdakilerden birini kullanın veya Linux görüntülerinden VM havuzu oluşturmak için işlem düğümleri kapsayıcı iş yükleri için. Batch ile uyumlu Market görüntüleri hakkında daha fazla bilgi için bkz: [sanal makine görüntülerini listesi](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Windows görüntüleri

Windows kapsayıcı iş yükleri için Batch şu anda destekler **Windows Server 2016 Datacenter kapsayıcılarla** Azure Marketi görüntüde. Yalnızca Docker kapsayıcısı görüntüleri Windows üzerinde desteklenir.

Docker Windows üzerinde çalışan sanal makineleri özel görüntülerinizi de oluşturabilirsiniz.

### <a name="linux-images"></a>Linux görüntüleri

Linux kapsayıcı iş yükleri için Batch şu anda Microsoft Azure toplu işlemde Azure Marketi tarafından yayımlanan aşağıdaki Linux görüntüleri destekler:

* **Azure Batch kapsayıcı havuzları centOS**

* **Azure Batch kapsayıcı havuzları için centOS (ile RDMA sürücüleri)**

* **Azure Batch kapsayıcı havuzları ubuntu Server**

* **Azure Batch kapsayıcı havuzları ubuntu Server (ile RDMA sürücüleri)**

Bu görüntüler, yalnızca Azure Batch havuzlarında kullanım için desteklenir. Bunlar özellik:

* Önceden yüklenmiş [Moby](https://github.com/moby/moby) kapsayıcı çalışma zamanı 

* Dağıtım Azure N-serisi vm'lerde kolaylaştırmak için önceden yüklenmiş NVIDIA GPU sürücüleri

* Görüntüler ile veya önceden yüklenmiş RDMA sürücüleri olmadan; RDMA özelliğine sahip VM boyutlarını dağıtıldığında Azure RDMA ağ erişmek havuz düğümleri bu sürücüleri izin ver  

Docker Batch ile uyumlu Linux dağıtımları biri üzerinde çalışan sanal makineler özel görüntülerinizi de oluşturabilirsiniz. Kendi özel Linux görüntü sağlamak isterseniz deki yönergelere bakın [sanal makinelerin bir havuzu oluşturmak için yönetilen özel görüntü kullanmak](batch-custom-images.md).

Özel görüntü üzerinde Docker desteğini yükleyin [Docker Community Edition (CE)](https://www.docker.com/community-edition) veya [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Özel Linux görüntü kullanmak için ek hususlar:

* Özel görüntü kullanırken Azure N-serisi boyutları GPU performansını avantajlarından yararlanmak için NVIDIA sürücülerini önceden yükleyin. Ayrıca, NVIDIA GPU için Docker altyapısı yardımcı programı'nı yüklemeniz gerekir [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* Azure RDMA ağ erişmek için RDMA özellikli VM boyutu kullanın. Gerekli RDMA sürücüleri CentOS HPC ve Batch tarafından desteklenen Ubuntu görüntüleri yüklenir. MPI iş yüklerini çalıştırmak için ek yapılandırma gerekebilir. Bkz: [kullanım RDMA özellikli GPU etkinleştirilmiş veya örneklerinde Batch havuzunda](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>Batch havuzu için kapsayıcı yapılandırma

Kapsayıcı iş yüklerini çalıştırmak için bir Batch havuzu etkinleştirmek için belirtmelisiniz [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) havuzun ayarlarında [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) nesnesi. (Bu makalede, Batch .NET API'si başvurusu bağlantılar sağlar. Karşılık gelen ayarları olan [Batch Python](/python/api/azure.batch) API.)

Aşağıdaki örneklerde gösterildiği gibi ile veya prefetched kapsayıcı görüntüler olmadan kapsayıcı özellikli bir havuz oluşturabilirsiniz. Çekme (veya önceden getirme) işlemi, kapsayıcı görüntülerden Docker hub'a veya Internet üzerindeki başka bir kapsayıcı kayıt defteri önceden yükleme sağlar. En iyi performans için kullanmak bir [Azure kapsayıcı kayıt defteri](../container-registry/container-registry-intro.md) toplu işlem hesabı ile aynı bölgede.

Kapsayıcı görüntüleri prefetching avantajı çalışan görevler ilk kez başlattığınızda, bunlar indirmek kapsayıcı görüntü için beklenecek gerekmemesidir. Havuz oluşturulduğunda kapsayıcısı yapılandırmasını kapsayıcı görüntüleri Vm'lere çeker. Havuzu üzerinde çalışır görevleri sonra kapsayıcı görüntülerin listesi başvurusu yapabilir ve kapsayıcı seçenekleri çalıştırın.


### <a name="pool-without-prefetched-container-images"></a>Havuz prefetched kapsayıcı görüntüler olmadan

Kapsayıcı özellikli bir havuz prefetched kapsayıcı görüntüler olmadan yapılandırmak için tanımlamak `ContainerConfiguration` ve `VirtualMachineConfiguration` nesneleri aşağıdaki Python örnekte gösterildiği gibi. Bu örnek için Azure Batch kapsayıcı havuzları Market görüntüsünden Ubuntu Server kullanır.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Görüntüleri kapsayıcısı yapılandırmasını için hazırlık

Kapsayıcı görüntülerin listesi kapsayıcı görüntüleri havuzunda hazırlık ekleyin (`container_image_names`, python'da) için `ContainerConfiguration`. 

Aşağıdaki temel Python örnek bir standart Ubuntu kapsayıcı görüntüsünden hazırlık gösterilmektedir [Docker hub'a](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


Aşağıdaki örnek C# örnek TensorFlow görüntüden hazırlık istediğinizi varsayar [Docker hub'a](https://hub.docker.com). Bu örnek, VM konak havuzu düğümler üzerinde çalışan bir başlangıç görevi içerir. Bir başlangıç görevi konak, örneğin, kapsayıcılardan erişilebilir bir dosya sunucusu bağlamak için çalıştırabilirsiniz.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Görüntüleri özel kapsayıcı kayıt defterinden Hazırlık

Kapsayıcı görüntüleri hazırlık özel kapsayıcı kayıt sunucusuna kimlik doğrulaması. Aşağıdaki örnekte, `ContainerConfiguration` ve `VirtualMachineConfiguration` nesneleri hazırlık özel TensorFlow görüntü özel Azure kapsayıcı kayıt defterinden. Yansıma başvurusu önceki örnekte olduğu gibi aynıdır.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## <a name="container-settings-for-the-task"></a>Görev için kapsayıcı ayarları

İşlem düğümlerinde kapsayıcı görevleri çalıştırmak için kapsayıcı özgü ayarları seçenekleri, kullanılacak görüntüleri ve kayıt defteri çalışan görev gibi belirtmeniz gerekir.

Kullanım `ContainerSettings` kapsayıcı özgü ayarları yapılandırmak için görev sınıfların özelliği. Bu ayarları tarafından tanımlanan [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) sınıfı.

Kapsayıcı görüntülerinde görevleri çalıştırırsanız [bulut görev](/dotnet/api/microsoft.azure.batch.cloudtask) ve [iş yöneticisi görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) kapsayıcı ayarları gerektirir. Ancak, [başlangıç görevi](/dotnet/api/microsoft.azure.batch.starttask), [iş hazırlama görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), ve [iş bırakma görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) kapsayıcı ayarları gerektirmez (diğer bir deyişle, bir kapsayıcı bağlamı içinde ya da doğrudan çalıştırabilirler düğümde).

Kapsayıcı ayarları, tüm dizinler yinelemeli olarak aşağıdaki yapılandırırken `AZ_BATCH_NODE_ROOT_DIR` (kök düğümde Azure Batch dizinlerin) kapsayıcı, tüm görev ortam değişkenleri, kapsayıcı ve görevin komut satırı eşlendi içine eşlenmiş kapsayıcıda yürütülür.

Aşağıdaki Python parçacığını Docker hub'dan çekilen bir Ubuntu kapsayıcısındaki çalıştıran temel bir komut satırı gösterir. Kapsayıcı çalıştırma seçenekleri için ek bağımsız değişkenler `docker create` görev çalışır komutu. Burada, `--rm` seçenek, görev tamamlandıktan sonra kapsayıcı kaldırır.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='echo hello',
    container_settings=task_container_settings
)

```

Aşağıdaki C# örnek bir bulut görevin temel kapsayıcı ayarlarını gösterir:

```csharp
// Simple container task command

string cmdLine = "<my-command-line>";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Sonraki adımlar

* Ayrıca bkz. [toplu Shipyard](https://github.com/Azure/batch-shipyard) kapsayıcı iş yüklerinin Azure batch kolay dağıtım için Araç Seti [Shipyard tarif](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Yükleme ve Docker CE Linux'ta kullanma hakkında daha fazla bilgi için bkz: [Docker](https://docs.docker.com/engine/installation/) belgeleri.

* Özel resimler kullanma hakkında daha fazla bilgi için bkz: [sanal makinelerin bir havuzu oluşturmak için yönetilen özel görüntü kullanmak ](batch-custom-images.md).

* Daha fazla bilgi edinmek [Moby proje](https://mobyproject.org/), kapsayıcı tabanlı sistemler oluşturmak için bir çerçeve.