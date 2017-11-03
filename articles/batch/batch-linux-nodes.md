---
title: "Sanal makinedeki çalıştırma Linux işlem düğümlerini - Azure Batch | Microsoft Docs"
description: "Linux sanal makinelerin Azure Batch havuzları, paralel işlem iş yükünü işlemek öğrenin."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b2257917e2368478beb75957677de23d4157865
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Batch havuzlarında Linux işlem düğümlerini sağlama

Linux ve Windows sanal makinelerde paralel işlem iş yüklerini çalıştırmak için Azure Batch kullanabilirsiniz. Bu makalede her ikisini de kullanarak Batch hizmetinde Linux işlem düğümleri havuzları oluşturmak nasıl ayrıntıları [Batch Python] [ py_batch_package] ve [Batch .NET] [ api_net]istemci kitaplıkları.

> [!NOTE]
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez. Uygulama paketlerini kullanarak uygulamalarınızı Batch düğümlerine dağıtma hakkında daha fazla bilgi için bkz. [Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtımı](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Sanal Makine Yapılandırması
Toplu işlem düğümleri havuzu oluşturduğunuzda, işletim sistemi ve düğüm boyutu seçmek için iki seçeneğiniz vardır: Bulut Hizmetleri Yapılandırması ve sanal makine yapılandırma.

**Cloud Services Yapılandırması** *yalnızca* Windows işlem düğümleri sağlar. Kullanılabilir işlem düğümü boyutları içinde listelenen [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md), ve kullanılabilir işletim sistemleri olarak listelenen [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](../cloud-services/cloud-services-guestos-update-matrix.md). Azure Cloud Services düğümleri içeren bir havuz oluşturduğunuzda yukarıda açıklanan makalelerinde açıklanan düğüm boyutu ve işletim sistemi ailesi belirtin. Bulut Hizmetleri, Windows havuzları işlem düğümleri için en yaygın olarak kullanılır.

**Sanal Makine Yapılandırması** işlem düğümleri için hem Linux hem de Windows görüntüleri sağlar. Kullanılabilir işlem düğümü boyutları içinde listelenen [azure'da sanal makineler için Boyutlar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) ve [azure'da sanal makineler için Boyutlar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). Sanal Makine Yapılandırması düğümleri içeren bir havuz oluşturduğunuzda, düğümler, sanal makine görüntü başvurusunu ve toplu işlem düğüm Aracısı düğümlerine yüklenmesi için SKU boyutunu belirtmeniz gerekir.

### <a name="virtual-machine-image-reference"></a>Sanal makine görüntü başvurusunu
Batch hizmetini kullanan [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Linux işlem düğümlerini sağlamak için. Bir görüntüden belirtebilirsiniz [Azure Marketi][vm_marketplace], veya hazırladığınız özel bir görüntü sağlayın. Özel görüntüler hakkında daha fazla ayrıntı için bkz. [Batch ile büyük ölçekli paralel işlem çözümleri geliştirme](batch-api-basics.md#pool).

Bir sanal makine görüntü başvurusunu yapılandırdığınızda sanal makine görüntüsünün özelliklerini belirtin. Bir sanal makine görüntü başvurusunu oluşturduğunuzda, aşağıdaki özellikler gereklidir:

| **Resim başvurusu özellikleri** | **Örnek** |
| --- | --- |
| Yayımcı |Canonical |
| Sunduğu |UbuntuServer |
| SKU |14.04.4-LTS |
| Sürüm |en son |

> [!TIP]
> Bu özellikler ve Market görüntülerini listesi hakkında daha fazla bilgi edinebilirsiniz [gidin ve Azure CLI veya PowerShell ile select Linux sanal makine görüntüleri](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tüm Market görüntülerini Batch ile şu anda uyumlu olduğunu unutmayın. Daha fazla bilgi için bkz: [düğümü aracı SKU'sunu](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Düğüm Aracısı SKU
Toplu işlem düğüm Aracısı havuzdaki her düğüm üzerinde çalışır ve düğümü ile Batch hizmeti arasındaki komut ve denetim arabirimi sağlayan bir programdır. Farklı işletim sistemleri için SKU'ları bilinen düğüm Aracısı'nın farklı uygulamaları vardır. Esas olarak, bir sanal makine yapılandırması oluşturduğunuzda, önce sanal makine görüntü başvurusunu belirtin ve ardından görüntüde yüklenecek düğüm Aracısı belirtin. Genellikle, her düğüm Aracısı SKU birden çok sanal makine görüntüsü ile uyumludur. Düğüm Aracısı SKU'ları bazı örnekleri şunlardır:

* Batch.node.ubuntu 14.04
* Batch.node.centos 7
* Batch.node.Windows amd64

> [!IMPORTANT]
> Market kullanılabilir tüm sanal makine görüntüleri şu anda kullanılabilir toplu aracılarını ile uyumlu değildir. Kullanılabilir düğüm Aracısı SKU'ları ve ile uyumlu olduklarından sanal makine görüntülerini listelemek için Batch SDK'ları kullanın. Bkz: [listesi, sanal makine görüntülerini](#list-of-virtual-machine-images) daha sonra bu makalede daha fazla bilgi ve geçerli görüntüleri çalışma zamanında bir listesini almak nasıl örnekleri.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Bir Linux havuzu oluşturun: Batch Python
Aşağıdaki kod parçacığını nasıl kullanılacağına ilişkin bir örnek göstermektedir [Python için Microsoft Azure Batch istemci Kitaplığı] [ py_batch_package] işlem düğümlerinin Ubuntu Server havuzu oluşturmak için. Batch Python modülü için başvuru belgeleri bulunabilir [azure.batch paket] [ py_batch_docs] okunur belgeleri.

Bu kod parçacığında oluşturur bir [Imagereference] [ py_imagereference] açıkça ve her (yayımcı, teklif, SKU, sürüm) özelliklerini belirtir. Üretim kodunda ancak kullanmanızı öneririz [list_node_agent_skus] [ py_list_skus] yöntemi belirlemek için kullanılabilir görüntü ve düğüm Aracısı SKU birleşimlerini çalışma zamanında seçin.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Daha önce belirtildiği gibi oluşturmak yerine öneririz [Imagereference] [ py_imagereference] açıkça kullandığınız [list_node_agent_skus] [ py_list_skus] dinamik şu anda desteklenen düğüm Aracısı/Market görüntü birleşimleri arasından seçim yapmak için yöntem. Aşağıdaki Python parçacığını bu yönteminin nasıl kullanılacağını gösterir.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Bir Linux havuzu oluşturun: Batch .NET
Aşağıdaki kod parçacığını nasıl kullanılacağına ilişkin bir örnek göstermektedir [Batch .NET] [ nuget_batch_net] işlem düğümlerinin Ubuntu Server havuzu oluşturmak için istemci kitaplığı. Bulabileceğiniz [Batch .NET başvuru belgeleri] [ api_net] konusuna bakın.

Aşağıdaki kod parçacığını kullanır [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] listesinden şu anda seçmek için yöntemi desteklenen Market görüntü ve düğüm Aracısı SKU birleşimleri. Bu teknik arzu çünkü desteklenen birleşimlerin listesi zaman zaman değişebilir. En yaygın olarak desteklenen birleşimlerin eklenir.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

Önceki kod parçacığını kullansa [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] dinamik olarak listelemek ve seçmek için yöntemi (önerilen) görüntüsü ve düğüm Aracısı SKU birleşimler desteklenmez, de yapılandırabilirsiniz bir [Imagereference] [ net_imagereference] açıkça:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Sanal makine görüntülerini listesi
Aşağıdaki tabloda, bu makalenin en son güncelleştirildiği kullanılabilir toplu aracılarını ile uyumlu olan Market sanal makine görüntüleri listeler. Görüntüleri ve aracılarını veya herhangi bir anda kaldırılamaz bu listeyi kesin olmadığından olduğunu dikkate almak önemlidir. Toplu işlem uygulamaları ve Hizmetleri zaman kullanmanızı öneririz [list_node_agent_skus] [ py_list_skus] (Python) ve [ListNodeAgentSkus] [ net_list_skus] (Belirlemek ve şu anda kullanılabilir SKU'lar seçmek için batch .NET).

> [!WARNING]
> Aşağıdaki listede, herhangi bir zamanda değişebilir. Her zaman kullanmak **listesi düğüm Aracısı SKU** Batch işleriniz çalıştırdığınızda, uyumlu bir sanal makine ve düğüm Aracısı SKU'ları listelemek için Batch API'lerini kullanılabilir yöntemleri.
>
>

| **Yayımcı** | **Teklif** | **Görüntü SKU** | **Sürüm** | **Düğüm Aracısı SKU kimliği** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| Canonical | UbuntuServer | 14.04.5-LTS | en son | Batch.node.ubuntu 14.04 |
| Canonical | UbuntuServer | 16.04.0-LTS | en son | Batch.node.ubuntu 16.04 |
| Credativ | Debian | 8 | en son | Batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | en son | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | en son | Batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | en son | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | en son | Batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | en son | Batch.node.centos 7 |
| Oracle | Oracle Linux | 7.2 | en son | Batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | en son | Batch.node.opensuse 13.2 |
| SUSE | openSUSE artık | 42.1 | en son | Batch.node.opensuse 42.1 |
| SUSE | SLES | 12 SP1 | en son | Batch.node.opensuse 42.1 |
| SUSE | SLES HPC | 12 SP1 | en son | Batch.node.opensuse 42.1 |
| Microsoft reklam | Linux veri bilimi vm | linuxdsvm | en son | Batch.node.centos 7 |
| Microsoft reklam | Standart veri bilimi vm | Standart veri bilimi vm | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Kapsayıcılar ile 2016 Datacenter | en son | Batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes-using-ssh"></a>SSH kullanarak Linux düğümüne bağlanma
Geliştirme sırasında veya sorun giderme sırasında havuzunuzdaki düğümlerden oturum açmak gerekli bulabilirsiniz. Windows işlem düğümleri, Linux düğümlerine bağlanmak için Uzak Masaüstü Protokolü (RDP) kullanamazsınız. Bunun yerine, Batch hizmeti uzak bağlantı için her düğümde SSH erişimini etkinleştirir.

Aşağıdaki Python kod parçacığında, uzak bağlantı için bir havuzdaki her düğümde bir kullanıcı oluşturur. Ardından, her düğüm için güvenli Kabuk (SSH) bağlantı bilgilerini yazdırır.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Önceki kod dört Linux düğümleri içeren bir havuz için örnek çıktı aşağıda verilmiştir:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Bir düğümde bir kullanıcı oluşturduğunuzda, bir parola yerine bir SSH ortak anahtarı belirtebilirsiniz. Python SDK'ın kullanmak **ssh_public_key** parametresini [ComputeNodeUser][py_computenodeuser]. .NET ile kullanma [ComputeNodeUser][net_computenodeuser].[ SshPublicKey] [ net_ssh_key] özelliği.

## <a name="pricing"></a>Fiyatlandırma
Azure Batch Azure Cloud Services ve Azure sanal makineleri teknolojisi üzerine kurulmuştur. Batch hizmeti, Batch çözümlerinizi kullandığını yalnızca işlem kaynakları için ücretlendirilirsiniz anlamına gelir hiçbir ücret ödemeden sunulur. Seçtiğinizde **Cloud Services Yapılandırması**, göre ücretlendirilen [bulut Hizmetleri fiyatlandırma] [ cloud_services_pricing] yapısı. Seçtiğinizde **sanal makine yapılandırması**, göre ücretlendirilen [sanal makineler fiyatlandırma] [ vm_pricing] yapısı. 

Kullanarak, toplu işlem düğümleri için uygulama dağıtıyorsanız [uygulama paketleri](batch-application-packages.md), uygulama paketlerinizi kullandığını Azure Storage kaynakları için ücretlendirilirsiniz. Genel olarak, Azure Storage maliyetleri en az. 

## <a name="next-steps"></a>Sonraki adımlar
### <a name="batch-python-tutorial"></a>Batch Python öğreticisi
Python kullanarak Batch ile çalışma hakkında daha ayrıntılı bir öğretici için kullanıma [Azure Batch Python istemcisini kullanmaya başlama](batch-python-tutorial.md). Kendi yardımcı [kod örneği] [ github_samples_pyclient] yardımcı bir işlev içerir `get_vm_config_for_distro`, bir sanal makine yapılandırmasını almak için başka bir yöntemi gösterir.

### <a name="batch-python-code-samples"></a>Batch Python kod örnekleri
[Python kod örnekleri] [ github_samples_py] içinde [azure-batch-samples] [ github_samples] github'daki nasıl gerçekleştirileceğini Göster komut dosyalarını içerir havuz, iş ve görev oluşturma gibi ortak toplu işlemleri. [Benioku] [ github_py_readme] Python eşlik örnekleri gerekli paketleri yüklemek nasıl kullanılacağı hakkındaki ayrıntıları sahiptir.

### <a name="batch-forum"></a>Toplu İşlem forumu
[Azure toplu işlem Forumu] [ forum] MSDN'de toplu ele almaktadır ve hizmet hakkında sorular sormak için iyi bir yerdir. "Okuma yararlı sabitlenmiş" yazılarını ve Batch çözümlerinizi derleme sırasında çıktıkları anda sorularınızı gönderin.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
