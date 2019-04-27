---
title: Sanal makine üzerinde çalıştırma Linux işlem düğümlerini - Azure Batch | Microsoft Docs
description: Azure batch'te Linux sanal makinelerinin havuzlarında paralel işlem iş yüklerinizi işlemek öğrenin.
services: batch
documentationcenter: python
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: na
ms.date: 06/01/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e228e73283685988247c8d419ba0a97b8c7b2974
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776161"
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Batch havuzlarında Linux işlem düğümleri sağlama

Azure Batch hem Linux hem de Windows sanal makinelerde paralel işlem iş yüklerini çalıştırmak için kullanabilirsiniz. Bu makalede her ikisini de kullanarak Batch hizmetinde Linux işlem düğümü havuzlarını oluşturma ayrıntıları [Batch Python] [ py_batch_package] ve [Batch .NET] [ api_net]istemci kitaplıkları.

> [!NOTE]
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez. Uygulama paketlerini kullanarak uygulamalarınızı Batch düğümlerine dağıtma hakkında daha fazla bilgi için bkz. [Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtımı](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Sanal makine yapılandırması
Batch hizmetinde işlem düğümü havuzu oluştururken, işletim sistemi ve düğüm boyutu seçmek için iki seçeneğiniz vardır: Cloud Services yapılandırması ve sanal makine yapılandırması.

**Cloud Services Yapılandırması** *yalnızca* Windows işlem düğümleri sağlar. Kullanılabilir bilgi işlem düğümü boyutları listelenir [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md), ve kullanılabilir işletim sistemleri olarak listelenen [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](../cloud-services/cloud-services-guestos-update-matrix.md). Azure Cloud Services düğümleri içeren bir havuz oluşturduğunuzda düğüm boyutunu ve işletim sistemi ailesi daha önce bahsedilen makalelerinde açıklanan belirtin. Bulut Hizmetleri, bilgi işlem düğümü havuzlarını Windows için en yaygın olarak kullanılır.

**Sanal Makine Yapılandırması** hem Linux hem de Windows görüntüleri için işlem düğümleri sağlar. Kullanılabilir bilgi işlem düğümü boyutları listelenir [azure'da sanal makine boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) ve [azure'da sanal makine boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). Sanal Makine Yapılandırması düğümleri içeren bir havuz oluşturduğunuzda, düğümler, sanal makine görüntü başvurusunu ve Batch düğüm Aracısı SKU düğümlere yüklenecek boyutunu belirtmeniz gerekir.

### <a name="virtual-machine-image-reference"></a>Sanal makine görüntü başvurusunu
Batch hizmeti kullandığı [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) işlem düğümlerine sanal makine yapılandırmasını sağlamak için. Bir görüntüden belirtebilirsiniz [Azure Marketi][vm_marketplace], veya hazırladığınız özel bir görüntü sağlayın. Özel görüntüleri hakkında daha fazla ayrıntı için bkz. [özel bir görüntü ile havuz oluşturma](batch-custom-images.md).

Bir sanal makine görüntü başvurusunu yapılandırdığınızda, sanal makine görüntüsünün özelliklerini belirtin. Bir sanal makine görüntü başvurusunu oluşturduğunuzda aşağıdaki özellikler gereklidir:

| **Görüntü başvurusu özellikleri** | **Örnek** |
| --- | --- |
| Yayımcı |Canonical |
| Sunduğu |UbuntuServer |
| SKU |14.04.4-LTS |
| Version |en son |

> [!TIP]
> Bu özellikler ve Market görüntüleri listeleme hakkında daha fazla bilgi [seçin CLI veya PowerShell ile azure'da Linux sanal makine görüntülerine erişin ve](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tüm Market görüntüleri şu anda Batch ile uyumlu olduğunu unutmayın. Daha fazla bilgi için [düğüm Aracısı SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Düğüm Aracısı SKU
Batch düğüm Aracısı, havuzdaki her düğümde çalışan ve Batch hizmeti düğüm arasındaki komut ve denetim arabirimi sağlayan bir programdır. Düğüm Aracısı SKU'ları bilinen farklı işletim sistemleri için farklı uygulamaları vardır. Esas olarak, bir sanal makine yapılandırması oluşturduğunuzda, ilk sanal makine görüntü başvurusunu belirtin ve ardından, resmi yüklemek için düğüm Aracısı belirtin. Genellikle, her düğüm Aracısı SKU birden çok sanal makine görüntüleri ile uyumludur. Düğüm Aracısı SKU'ları bazı örnekleri aşağıda verilmiştir:

* Batch.node.ubuntu 14.04
* Batch.node.centos 7
* Batch.node.Windows amd64

> [!IMPORTANT]
> Market'te kullanıma sunulan tüm sanal makine görüntüleri, şu anda kullanılabilir Batch düğüm aracıları ile uyumludur. Kullanılabilir düğüm Aracısı SKU'ları ve uyumlu olduğu sanal makine görüntülerini listelemek için Batch SDK'ları kullanın. Bkz: [listesi, sanal makine görüntüleri](#list-of-virtual-machine-images) zamanında geçerli görüntülerin listesini almak nasıl örnekleri ve daha fazla bilgi için bu makalenin sonraki bölümlerinde.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Linux havuzu oluşturun: Batch Python
Aşağıdaki kod parçacığını nasıl kullanılacağına ilişkin bir örnektir [Python için Microsoft Azure Batch istemci kitaplığını] [ py_batch_package] işlem düğümleri havuzu Ubuntu Server oluşturmak için. Batch Python modülünde için başvuru belgeleri bulunabilir [azure.batch paket] [ py_batch_docs] okunur belgeleri.

Bu kod parçacığı oluşturur bir [Imagereference] [ py_imagereference] açıkça ve her özelliklerini (yayımcı, teklif, SKU, sürüm) belirtir. Üretim kodunda ancak kullanmanızı öneririz [list_node_agent_skus] [ py_list_skus] yöntemini belirlemek ve kullanılabilir görüntü ve düğüm Aracısı SKU birleşimlerini zamanında seçin.

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

Daha önce belirtildiği gibi oluşturmak yerine öneririz [Imagereference] [ py_imagereference] açıkça kullandığınız [list_node_agent_skus] [ py_list_skus] dinamik şu anda desteklenen düğüm Aracısı/Market görüntü birleşimleri arasından seçim yapmak için yöntemi. Aşağıdaki Python kod parçacığında bu yöntemin nasıl kullanılacağı gösterilmektedir.

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

## <a name="create-a-linux-pool-batch-net"></a>Linux havuzu oluşturun: Batch .NET
Aşağıdaki kod parçacığını nasıl kullanılacağına ilişkin bir örnektir [Batch .NET] [ nuget_batch_net] işlem düğümleri havuzu Ubuntu Server oluşturmak için istemci kitaplığı. Bulabilirsiniz [Batch .NET başvuru belgeleri] [ api_net] docs.microsoft.com'da.

Aşağıdaki kod parçacığı kullandığı [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] listesinden şu anda seçmek için Market görüntüsü ve düğüm Aracısı SKU bileşimleri desteklenir. Desteklenen kombinasyonlar listesini zaman zaman değişebilir olduğundan bu arzu bir tekniktir. En yaygın olarak desteklenen kombinasyonlar eklenir.

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

Önceki kod parçacığı kullansa [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] dinamik olarak listelemek ve seçin için yöntemi (önerilen) görüntü ve düğüm Aracısı SKU birleşimlerini desteklenmez, ayrıca yapılandırabilirsiniz bir [Imagereference] [ net_imagereference] açıkça:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Sanal makine görüntülerinin listesi
Aşağıdaki tabloda, bu makalenin son güncelleştirildiği zaman kullanılabilir Batch düğüm aracı ile uyumlu olan Market sanal makine görüntülerini listeler. Görüntüleri ve aracılarını eklenebilir veya herhangi bir zamanda kaldırıldı çünkü bu liste eksiksiz olmadığına dikkat edin önemlidir. Batch uygulamalarınızı ve hizmetlerinizi her zaman kullanmanızı öneririz [list_node_agent_skus] [ py_list_skus] (Python) veya [ListNodeAgentSkus] [ net_list_skus] () Şu anda kullanılabilen SKU'ları belirlemek ve batch .NET).

> [!WARNING]
> Aşağıdaki listede, herhangi bir zamanda değişebilir. Her zaman **listesi düğüm Aracısı SKU** Batch API'leri, Batch işleriniz çalıştırdığınızda uyumlu sanal makine ve düğüm Aracısı SKU'ları listelemek için kullanılabilen yöntemler.
>
>

| **Yayımcı** | **Teklif** | **Görüntü SKU'su** | **Sürüm** | **Düğüm Aracısı SKU kimliği** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| toplu iş | işleme centos73 | işleme | en son | Batch.node.centos 7 |
| toplu iş | rendering-windows2016 | işleme | en son | Batch.node.Windows amd64 |
| Canonical | UbuntuServer | 16.04-LTS | en son | Batch.node.ubuntu 16.04 |
| Canonical | UbuntuServer | 14.04.5-LTS | en son | Batch.node.ubuntu 14.04 |
| credativ | Debian | 9 | en son | batch.node.debian 9 |
| credativ | Debian | 8 | en son | batch.node.debian 8 |
| Microsoft reklamları | linux-data-science-vm | linuxdsvm | en son | Batch.node.centos 7 |
| Microsoft reklamları | standard-data-science-vm | standard-data-science-vm | en son | Batch.node.Windows amd64 |
| Microsoft azure batch | centos kapsayıcı | 7-4 | en son | Batch.node.centos 7 |
| Microsoft azure batch | centos kapsayıcı rdma | 7-4 | en son | Batch.node.centos 7 |
| Microsoft azure batch | ubuntu server kapsayıcı | 16-04-lts | en son | Batch.node.ubuntu 16.04 |
| Microsoft azure batch | ubuntu-server-container-rdma | 16-04-lts | en son | Batch.node.ubuntu 16.04 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter smalldisk | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter-with-Containers | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter-smalldisk | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter-smalldisk | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008-R2-SP1 | en son | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008-R2-SP1-smalldisk | en son | Batch.node.Windows amd64 |
| OpenLogic | CentOS | 7.4 | en son | Batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.4 | en son | Batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.3 | en son | Batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.1 | en son | Batch.node.centos 7 |
| Oracle | Oracle-Linux | 7.4 | en son | Batch.node.centos 7 |
| SUSE | SLES-HPC | 12-SP2 | en son | Batch.node.opensuse 42.1 |

## <a name="connect-to-linux-nodes-using-ssh"></a>SSH kullanarak Linux düğümlere bağlanma
Geliştirme sırasında veya sorun giderme sırasında havuzunuzdaki düğümler için oturum açmak gerekli bulabilirsiniz. Windows işlem düğümleri, Linux düğümlerine bağlanmak için Uzak Masaüstü Protokolü (RDP) kullanamazsınız. Bunun yerine, Batch hizmeti için Uzak bağlantı SSH erişimini her düğümde sağlar.

Aşağıdaki Python kod parçacığı, uzak bağlantı için bir havuzdaki her düğümde bir kullanıcı oluşturur. Ardından, her düğüm için güvenli Kabuk (SSH) bağlantı bilgilerini yazdırır.

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

Önceki kodun dört Linux düğümleri içeren bir havuz için örnek çıktı aşağıdaki gibidir:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Bir parola yerine bir kullanıcı bir düğümde oluştururken bir SSH ortak anahtarı belirtebilirsiniz. Python SDK'sını içinde **ssh_public_key** parametresi [ComputeNodeUser][py_computenodeuser]. .NET ile kullanma [ComputeNodeUser][net_computenodeuser].[ SshPublicKey] [ net_ssh_key] özelliği.

## <a name="pricing"></a>Fiyatlandırma
Azure Batch, Azure Cloud Services ve Azure sanal makineler teknolojisi üzerine kurulmuştur. Batch hizmeti, Batch hesaplarınızın kullandığı yalnızca işlem kaynakları için ücretlendirilirsiniz anlamına gelir ücretsiz olarak sunulur. Seçeneğini belirlediğinizde **Cloud Services Yapılandırması**, temel alınarak ücretlendirilir [bulut Hizmetleri fiyatlandırması] [ cloud_services_pricing] yapısı. Seçeneğini belirlediğinizde **sanal makine yapılandırması**, temel alınarak ücretlendirilir [sanal makineleri fiyatlandırması] [ vm_pricing] yapısı. 

Uygulama kullanarak Batch düğümlerine dağıtıyorsanız [uygulama paketleri](batch-application-packages.md), uygulama paketlerinizi kullanmamasından Azure Storage kaynakları için ücretlendirilirsiniz. Genel olarak, Azure depolama maliyetlerini en az düzeydedir. 

## <a name="next-steps"></a>Sonraki adımlar

[Python kod örnekleri] [ github_samples_py] içinde [azure-batch-samples] [ github_samples] GitHub deposunu nasıl gerçekleştirileceğini göstereceğiz betikleri içerir havuz, iş ve görev oluşturma gibi ortak toplu işlemleri. [Benioku] [ github_py_readme] , Python eşlik örnekleri olan gerekli paketleri yükleme hakkında ayrıntılar.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
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
[nuget_batch_net]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: https://azure-sdk-for-python.readthedocs.io/batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: https://docs.microsoft.com/python/api/azure.batch.models.computenodeuser
[py_imagereference]: https://docs.microsoft.com/python/api/azure.mgmt.batch.models.imagereference
[py_list_skus]: https://docs.microsoft.com/python/api/azure-batch/azure.batch.operations.AccountOperations?view=azure-python
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
