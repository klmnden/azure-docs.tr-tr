---
title: "Azure batch kapsayıcı iş yükleri | Microsoft Docs"
description: "Azure Batch kapsayıcı görüntülerden uygulamaları çalıştırmayı öğrenin."
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 12/01/2017
ms.author: danlep
ms.openlocfilehash: 2fa5f9335a4d00f489f11c0db23322ab971a224f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="run-container-applications-on-azure-batch"></a>Azure Batch kapsayıcı uygulamaları çalıştırma

Azure toplu işlem, çalıştırmak ve toplu işleri Azure ile ilgili bilgi işlem çok fazla sayıda ölçeklendirme sağlar. Şimdiye kadar toplu işlem görevleri doğrudan sanal makinelerde (VM'ler) Batch havuzunda çalıştırdıktan ancak Docker kapsayıcılarında görevleri çalıştırmak için Batch havuzu oluşturmak artık ayarlayabilirsiniz.

Kapsayıcıları kullanma, uygulama paketler ve bağımlılıklar yönetmek zorunda kalmadan toplu görevleri çalıştırmak için kolay bir yol sağlar. Kapsayıcıları, çeşitli ortamlar çalıştırabilirsiniz basit, taşınabilir, sünece birimleri olarak uygulamaları dağıtın. Örneğin, yapı ve test kapsayıcısı yerel olarak ardından Azure veya başka bir kayıt defteri için kapsayıcı görüntüyü karşıya yükleme. Kapsayıcı dağıtım modeli, çalışma zamanı ortamı, uygulamanızın her zaman doğru yüklendiğinden ve yapılandırıldığından, uygulama barındırdığınız bağımsız olarak sağlar. Bu öğretici, Batch .NET SDK'sı çalışan kapsayıcı görevleri destek işlem düğümleri havuzu oluşturmak için nasıl kullanılacağı ve havuzunda kapsayıcı görevleri çalıştırmak nasıl gösterir.

Bu makale, Docker kapsayıcısı kavramları ve Batch havuzu ve .NET SDK kullanarak iş oluşturma varsayar. Kod parçacıkları benzer bir istemci uygulamasında kullanılması amaçlanmıştır [DotNetTutorial örnek](batch-dotnet-get-started.md), ve toplu işlemde kapsayıcı uygulamaları desteklemek için ihtiyacınız kod örnekleri.


## <a name="prerequisites"></a>Önkoşullar

* SDK sürümleri: Batch SDK'ları destek kapsayıcı görüntüleri aşağıdaki sürümler:
    * Batch REST API'si sürüm 2017 09 01.6.0
    * Batch .NET SDK sürüm 8.0.0
    * Batch Python SDK'sı sürüm 4.0
    * Batch Java SDK'sı sürüm 3.0
    * Toplu Node.js SDK'sı sürüm 3.0

* Hesaplar: Azure hesabınızdaki, toplu işlem hesabı ve isteğe bağlı olarak genel amaçlı depolama hesabı oluşturmanız gerekir.

* Desteklenen bir VM görüntüsü. Kapsayıcılar yalnızca desteklenen aşağıdaki bölümde ayrıntılı görüntülerden sanal makine yapılandırması ile oluşturulan havuzlarında "sanal makine görüntülerini desteklenmiyor."

* Özel görüntü sağlarsanız, uygulamanızın kapsayıcı tabanlı iş yüklerini çalıştırmak için Azure Active Directory (Azure AD) ile kimlik doğrulaması kullanmanız gerekir. Bir Azure Market görüntüsü kullanıyorsanız, Azure AD kimlik doğrulama gerekmez; paylaşılan anahtar kimlik doğrulaması çalışmaz. Azure AD için Azure Batch desteği, [Batch hizmeti çözümlerinin kimliğini Active Directory ile doğrulama](batch-aad-auth.md) makalesinde belirtilmiştir.


## <a name="supported-virtual-machine-images"></a>Desteklenen sanal makine görüntüleri

VM havuzu oluşturmak için görüntüyü Linux işlem düğümlerini veya Windows sağlamanız gerekir.

### <a name="windows-images"></a>Windows görüntüleri

Windows kapsayıcı iş yükleri için Batch şu anda Docker Windows üzerinde çalışan vm'lerden oluşturduğunuz özel resimler destekler veya Azure Marketi'nden kapsayıcıları görüntüsü ile Windows Server 2016 Datacenter kullanabilirsiniz. Bu görüntü ile uyumlu `batch.node.windows amd64` düğüm Aracısı SKU kimliği Desteklenen kapsayıcı türü şu an için Docker sınırlıdır.

### <a name="linux-images"></a>Linux görüntüleri

Linux kapsayıcı iş yükleri için toplu şu anda, Docker aşağıdaki Linux dağıtımları üzerinde çalışan sanal makineler oluşturmak yalnızca özel resimler destekler: Ubuntu 16.04 LTS veya CentOS 7.3. Kendi özel Linux görüntü sağlamak isterseniz deki yönergelere bakın [sanal makinelerin bir havuzu oluşturmak için yönetilen özel görüntü kullanmak](batch-custom-images.md).

Kullanabileceğiniz [Docker Community Edition (CE)](https://www.docker.com/community-edition) veya [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Azure NC veya NV VM boyutlarını GPU performansını yararlanmak isterseniz, görüntüde NVIDIA sürücüleri yüklemeniz gerekir. Ayrıca, yüklemek ve NVIDIA GPU için Docker altyapısı yardımcı programı'nı çalıştırmak gereken [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

Azure RDMA ağ erişmek için aşağıdaki boyutları Vm'leri kullanın: A8, A9, H16r, H16mr veya NC24r. Gerekli RDMA sürücüleri Azure Marketi'nden CentOS 7.3 HPC ve Ubuntu 16.04 LTS görüntülerinde yüklenir. MPI iş yüklerini çalıştırmak için ek yapılandırma gerekebilir. Bkz: [kullanım RDMA özellikli GPU etkinleştirilmiş veya örneklerinde Batch havuzunda](batch-pool-compute-intensive-sizes.md).


## <a name="limitations"></a>Sınırlamalar

* Toplu iş, yalnızca Linux havuzlarını çalıştıran kapsayıcıları için RDMA desteği sağlar.


## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kimlik doğrulama

Batch havuzu oluşturmak için özel bir VM görüntüsü kullanıyorsanız, istemci uygulamanızı Azure AD ile tümleşik kimlik doğrulaması kullanarak kimlik doğrulaması gerekir (paylaşılan anahtar kimlik doğrulaması çalışmaz). Uygulamayı çalıştırmadan önce bunun için bir kimlik oluşturmak ve kendi diğer uygulamalara izinler belirtmek için Azure AD'de kaydetmek emin olun.

Ayrıca, özel bir VM görüntüsü kullandığınızda, IAM erişim denetimi VM görüntüsü erişmek için uygulamaya vermeniz gerekir. Azure Portalı'nda açmak **tüm kaynakları**, kapsayıcı görüntüsünü seçin ve **erişim denetimi (IAM)** görüntü dikey ve tıklatın bölümünü **Ekle**. İçinde **izinleri eklemek** dikey penceresinde belirtin bir **rol**, **atamak için erişim**seçin **Azure AD kullanıcı, Grup veya uygulama**, sonra **Seçin** uygulamanın adını girin.

Toplu istemcisini kullanarak oluşturduğunuzda, uygulamanızda Azure AD kimlik doğrulama belirtecini geçirin [BatchClient.Open](/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_)açıklandığı gibi [Active Directory ile kimlik doğrulaması toplu hizmet çözümlerine](batch-aad-auth.md).


## <a name="reference-a-vm-image-for-pool-creation"></a>Havuzu oluşturmak için bir VM görüntüsü başvurusu

Uygulama kodunuzda havuzun işlem düğümleri oluşturulurken kullanılacak VM görüntüsü için bir başvuru sağlar. Oluşturarak bunu bir [Imagereference](/dotnet/api/microsoft.azure.batch.imagereference) nesnesi. Aşağıdaki yollardan biriyle kullanılacak görüntünün belirtebilirsiniz:

* Özel görüntü kullanıyorsanız, bir Azure Resource Manager kaynak tanımlayıcısı için sanal makine görüntüsü sağlar. Görüntü tanımlayıcısı, aşağıdaki örnekte gösterildiği gibi bir yol biçime sahiptir:

  ```csharp
  // Provide a reference to a custom image using an image ID
  ImageReference imageReference = new ImageReference("/subscriptions/<subscription-ID>/resourceGroups/<resource-group>/providers/Microsoft.Compute/images/<imageName>");
  ```

    Azure portalından bu görüntü Kimliğini almak için açık **tüm kaynakları**, özel görüntüyü seçin ve **genel bakış** bölüm yolu görüntü kanadında kopyalayın **kaynak kimliği**.

* Kullanıyorsanız bir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute?page=1&subcategories=windows-based) görüntü, bir grup görüntüsünü açıklayan parametrelerinin sağlayın: Teklif türü, yayımcı, SKU ve sürümü içinde listelenen görüntünün [sanal makine görüntülerini listesi](batch-linux-nodes.md#list-of-virtual-machine-images):

  ```csharp
  // Provide a reference to an Azure Marketplace image for
  // "Windows Server 2016 Datacenter with Containers"
  ImageReference imageReference = new ImageReference(
    publisher: "MicrosoftWindowsServer",
    offer: "WindowsServer",
    sku: "2016-Datacenter-with-Containers",
    version: "latest");
  ```


## <a name="container-configuration-for-batch-pool"></a>Batch havuzu için kapsayıcı yapılandırma

Batch havuzundaki işlem düğümleri, görevler bir işi batch'in koleksiyonudur. Havuz oluşturduğunuzda, işlem düğümleri için VM yapılandırmasını sağlar. [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) nesnesini içeren bir başvuru [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) nesnesi. Kapsayıcı iş yükleri havuzunda etkinleştirmek üzere belirtin `ContainerConfiguration` ayarlar. VM de resim başvurusu ve görüntünün düğüm Aracısı SKU kimliği, belirlediğiniz aşağıdaki örneklerde gösterildiği gibi yapılandırmadır.

Havuzu oluşturmak için birkaç seçeneğiniz vardır. İle veya prefetched kapsayıcı görüntüler olmadan bir havuz oluşturabilirsiniz. 

Çekme (veya önceden getirme) işlemi, kapsayıcı görüntüleri Docker hub'a veya başka bir kapsayıcı kayıt defteri Internet üzerinde önceden yükleme sağlar. Kapsayıcı görüntüleri prefetching avantajı çalışan görevler ilk kez başlattığınızda, bunlar indirmek kapsayıcı görüntü için beklenecek gerekmemesidir. Havuz oluşturulduğunda kapsayıcısı yapılandırmasını kapsayıcı görüntüleri Vm'lere çeker. Havuzu üzerinde çalışır görevleri sonra kapsayıcı görüntülerin listesi başvurusu yapabilir ve kapsayıcı seçenekleri çalıştırın.



### <a name="pool-without-prefetched-container-images"></a>Havuz prefetched kapsayıcı görüntüler olmadan

Prefetched kapsayıcı görüntüler olmadan havuzunu yapılandırmak için tanımlamak `ContainerConfiguration` ve `VirtualMachineConfiguration` nesneleri aşağıdaki örnekte gösterildiği gibi. Bu ve aşağıdaki örnekleri Docker yüklü altyapısı ile özel bir Ubuntu 16.04 LTS görüntü kullandığınız varsayılır.

```csharp
// Specify container configuration
ContainerConfiguration containerConfig = new ContainerConfiguration();

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

// Commit pool creation
pool.Commit();
```


### <a name="prefetch-images-for-container-configuration"></a>Görüntüleri kapsayıcısı yapılandırmasını için hazırlık

Kapsayıcı görüntülerin listesi kapsayıcı görüntüleri havuzunda hazırlık ekleyin (`containerImageNames`) için `ContainerConfiguration`ve resim listesi bir ad verin. Aşağıdaki örnek özel bir Ubuntu 16.04 LTS görüntü kullandığınızı varsayar, TensorFlow görüntüden hazırlık [Docker hub'a](https://hub.docker.com), ve bir başlangıç görevi TensorFlow başlatın.

```csharp
// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Define container settings
TaskContainerSettings startTaskContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu");
StartTask startTaskContainer = new StartTask(
    CommandLine: "<docker-image-command-line>",
    TaskContainerSettings: startTaskContainerSettings);

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);

// Commit pool creation
pool.Commit();
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Görüntüleri özel kapsayıcı kayıt defterinden Hazırlık

Kapsayıcı görüntüleri hazırlık özel kapsayıcı kayıt sunucusuna kimlik doğrulaması. Aşağıdaki örnekte, `ContainerConfiguration` ve `VirtualMachineConfiguration` nesneleri özel TensorFlow görüntü özel Azure kapsayıcı kayıt defterinden hazırlık ve özel bir Ubuntu 16.04 LTS görüntü kullanın.

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

// Commit pool creation
pool.Commit();
```


## <a name="container-settings-for-the-task"></a>Görev için kapsayıcı ayarları

İşlem düğümlerinde çalıştırılacak görevleri ayarladığınızda, kapsayıcı özgü ayarları seçenekleri, kullanılacak görüntüleri ve kayıt defteri çalışan görev gibi belirtmeniz gerekir.

Kapsayıcı özgü ayarları yapılandırmak için görev sınıfların ContainerSettings özelliğini kullanın. Bu ayarları tarafından tanımlanan [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) sınıfı.

Kapsayıcı görüntülerinde görevleri çalıştırırsanız [bulut görev](/dotnet/api/microsoft.azure.batch.cloudtask) ve [iş yöneticisi görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) kapsayıcı ayarları gerektirir. Ancak, [başlangıç görevi](/dotnet/api/microsoft.azure.batch.starttask), [iş hazırlama görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), ve [iş bırakma görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) kapsayıcı ayarları gerektirmez (diğer bir deyişle, bir kapsayıcı bağlamı içinde ya da doğrudan çalıştırabilirler düğümde).

Kapsayıcı ayarları, tüm dizinler yinelemeli olarak aşağıdaki yapılandırırken `AZ_BATCH_NODE_ROOT_DIR` (kök düğümde Azure Batch dizinlerin) kapsayıcı, tüm görev ortam değişkenleri, kapsayıcı ve görevin komut satırı eşlendi içine eşlenmiş kapsayıcıda yürütülür.

Aşağıdaki kod örneğinde [hazırlık kapsayıcısı yapılandırmasını görüntülerde](#prefetch-images-for-container-configuration) bir başlangıç görevi kapsayıcı yapılandırmayı belirtmek nasıl oluşturulacağını gösterir. Aşağıdaki kod örneği, nasıl bir bulut görev kapsayıcı yapılandırmayı belirtmek gösterir:

```csharp
// Simple task command

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

* Toplu ayrıntılı bir bakış için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md).

* Yükleme ve Docker CE Linux'ta kullanma hakkında daha fazla bilgi için bkz: [Docker](https://docs.docker.com/engine/installation/) belgeleri.

* Özel resimler kullanma hakkında daha fazla bilgi için bkz: [sanal makinelerin bir havuzu oluşturmak için yönetilen özel görüntü kullanmak ](batch-custom-images.md).
