---
title: Çalıştırma denemeleri ve sanal ağ içindeki çıkarım
titleSuffix: Azure Machine Learning service
description: Makine öğrenimi denemeleri ve bir Azure sanal ağ içinde çıkarımı güvenli hale getirme çalıştırın. İşlem hedeflerine yönelik model eğitiminin oluşturmayı öğrenin ve sanal ağ içindeki çıkarımı yapma. Güvenli sanal ağlar için gereksinimleri hakkında bilgi edinmek için aşağıdaki gibi gelen ve giden bağlantı noktalarını gerektirir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 01/08/2019
ms.openlocfilehash: c1006aa21b3009bb7508c7a24ab501d39737261c
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978233"
---
# <a name="securely-run-experiments-and-inference-inside-an-azure-virtual-network"></a>Denemeler ve bir Azure sanal ağ içindeki çıkarım güvenli bir şekilde çalıştırın

Bu makalede, denemeleri ve sanal ağ içindeki çıkarımı nasıl çalıştırılacağını öğrenin. Bir sanal ağ, Azure kaynaklarınızın genel internet'ten yalıtarak bir güvenlik sınırı görevi görür. Şirket içi ağınızı bir Azure sanal ağı da katılabilirsiniz. Güvenli bir şekilde Modellerinizi eğitmek ve çıkarım için dağıtılan Modellerinizi erişim sağlar. Çıkarım veya Puanlama modeli, dağıtılan model için tahmin, üretim veri çubuğunda en yaygın olarak kullanıldığı aşamasıdır.

İşlem kaynakları için diğer Azure hizmetleriyle Azure Machine Learning hizmeti kullanır. İşlem kaynakları (hedef işlem), modelleri eğitme ve kullanılır. Bu işlem, hedef sanal ağ içinde oluşturulabilir. Örneğin, Microsoft Veri bilimi sanal makinesi bir modeli eğitmek ve sonra model Azure Kubernetes Service (AKS) dağıtmak için kullanabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [Azure sanal ağına genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

## <a name="prerequisites"></a>Önkoşullar

Bu belge, Azure sanal ağları ve IP genel ağ ile ilgili bilgi sahibi olduğunuz varsayılır. Bu belge, ayrıca bir sanal ağ ve işlem kaynaklarınızı ile kullanılacak alt ağı oluşturmuş olduğunuzu varsayar. Azure sanal ağları ile ilgili bilgi sahibi değilseniz, hizmeti hakkında bilgi edinmek için bu makaleleri okuyun:

* [IP adresleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
* [Güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview)
* [Hızlı Başlangıç: Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)
* [Ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="storage-account-for-your-workspace"></a>Çalışma alanınız için depolama hesabı

> [!IMPORTANT]
> Azure Machine Learning hizmeti çalışma alanında sanal ağ arkasında yalnızca deneme yaparken bağlı depolama hesabını koyabilirsiniz. Çıkarım depolama hesabında sınırsız erişim gerektirir. Bu ayarlar değiştirdiyseniz emin değilseniz, ya da olmayabilir, __varsayılan ağ erişim kuralını değiştirmek__ içinde [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security). Çıkarımı sırasında tüm ağlardan erişime izin ver veya Puanlama modeli için bu adımları kullanın.

Azure Machine Learning deneme özelliklerinin bir sanal ağ ile Azure depolama kullanmak için aşağıdaki adımları izleyin:

1. Bir deneme işlem ex oluşturun. Bir sanal ağ machine Learning işlem veya bir deneme işlem çalışma alanına ekleyin. HDInsight kümesi veya sanal makine. Daha fazla bilgi için [kullanın, Machine Learning işlem](#use-machine-learning-compute) ve [bir sanal makine ya da HDInsight kümesi kullanma](#use-a-virtual-machine-or-hdinsight-cluster) bu belgedeki bölümleri
2. Çalışma alanına bağlı depolama gidin. ![Azure portalında Azure Machine Learning hizmeti çalışma alanına bağlı Azure depolama gösteren bir görüntüsü](./media/how-to-enable-virtual-network/workspace-storage.png)
3. Azure depolama sayfasında __güvenlik duvarları ve sanal ağlar__. ![Azure depolama sayfasının bölümünde görüntüsünü Azure portal gösteren güvenlik duvarları ve sanal ağlar](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)
4. Üzerinde __güvenlik duvarları ve sanal ağlar__ sayfasında, aşağıdaki girdileri seçin:
    - __Seçili ağlar__'ı seçin.
    - Altında __sanal ağlar__seçin __var olan sanal ağı Ekle__ deneme işlem bulunduğu sanal ağı eklemek için. (1. adıma bakın.)
    - Seçin __güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver__.
![Resmi Azure portal gösteren güvenlik duvarları ve sanal ağlar sayfasında Azure Storage'nın altında](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png) 

5. Deneme, deneme kodunuzu çalıştırırken, blob depolama kullanma çalıştırma yapılandırma değiştirin:
    ```python
    run_config.source_directory_data_store = "workspaceblobstore"
    ```
    
## <a name="key-vault-for-your-workspace"></a>Çalışma alanınız için anahtar kasası
Çalışma alanı ile ilişkilendirilmiş Key Vault örneği, çeşitli türlerdeki kimlik bilgilerini depolamak için Azure Machine Learning hizmeti tarafından kullanılır:
* İlişkili depolama hesabı bağlantı dizesi
* Azure kapsayıcı deposuna örneklerine parolaları
* Bağlantı için veri depolarını dizeleri. 

Azure Machine Learning denemesi kullanmak için bir sanal ağ arkasında Key Vault ile özellikleri, aşağıdaki adımları izleyin:
1. Çalışma alanı ile ilişkilendirilmiş Key vault'a gidin. ![Azure portalında Azure Machine Learning hizmeti çalışma alanı ile ilişkili olan bir Key Vault gösteren bir görüntüsü](./media/how-to-enable-virtual-network/workspace-key-vault.png)
2. Anahtar kasası sayfasında __güvenlik duvarları ve sanal ağlar__ bölümü. ![Bölüm anahtar kasası sayfasında görüntü, Azure portal gösteren güvenlik duvarları ve sanal ağlar](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)
3. Üzerinde __güvenlik duvarları ve sanal ağlar__ sayfasında, aşağıdaki girdileri seçin:
    - __Seçili ağlar__'ı seçin.
    - Altında __sanal ağlar__seçin __var olan sanal ağları Ekle__ deneme işlem bulunduğu sanal ağı eklemek için.
    - Seçin __güvenilen Microsoft hizmetlerinin bu güvenlik duvarını geçmesine izin ver__.
![Görüntüyü Azure portalında gösteren güvenlik duvarları ve sanal ağlar sayfasında anahtar Kasası'nın altında](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png) 


## <a name="use-machine-learning-compute"></a>Machine Learning işlem kullanma

Azure Machine Learning işlem bir sanal ağ kullanmak için ağ gereksinimleri hakkında aşağıdaki bilgileri kullanın:

- Sanal ağ, Azure Machine Learning hizmeti çalışma alanı olarak aynı abonelik ve aynı bölgede olması gerekir.

- Küme için hedeflenen olması gerekir Machine Learning işlem kümesi için VM sayısı'nı barındırmak için yeterli sayıda atanmamış IP adresi alt ağ belirtilmiş. Alt ağ yeterli sayıda atanmamış IP adresleri yoksa, küme kısmen ayrılır.

- Bazı bağlantı noktaları, sanal ağ trafiği kısıtlayarak güvenli planlıyorsanız, Machine Learning işlem hizmeti için açık bırakın. Daha fazla bilgi için [gerekli bağlantı noktaları](#mlcports).

- Güvenlik ilkeleri ve sanal ağın abonelik veya kaynak grubunun kilitlerini sanal ağı yönetmek için izinleri kısıtlamak olup olmadığını denetleyin.

- Bir sanal ağda birden fazla Machine Learning işlem kümeleri koymak için kullanacaksanız, bir veya daha fazla kaynaklarınız için bir kota artırım talebinde gerekebilir.

    Machine Learning işlem, kaynak grubundaki sanal ağı içeren ek ağ kaynakları otomatik olarak ayırır. Machine Learning işlem her küme için Azure Machine Learning hizmeti, aşağıdaki kaynakları ayırır:

    - Bir ağ güvenlik grubu (NSG)

    - Bir genel IP adresi

    - Bir yük dengeleyici

  Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-subscription-service-limits) sınırlıdır.

### <a id="mlcports"></a> Gerekli bağlantı noktaları

Machine Learning işlem şu anda Azure Batch hizmeti belirtilen sanal ağdaki VM'ler sağlamak için kullanır. Alt ağın Batch hizmetinden gelen iletişimine izin vermelidir. Bu iletişim zamanlamak için kullanılan Machine Learning işlem düğümlerini ve diğer kaynaklar ile Azure depolama ile iletişim kurmak için çalışır. Batch, Vm'lere bağlı ağ arabirimleri (NIC) düzeyinde Nsg'ler ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

- TCP trafiği 29876 ve 29877 numaralı gelen bağlantı noktalarında gelen bir __hizmet etiketi__ , __BatchNodeManagement__.

    ![Azure portalında BatchNodeManagement hizmet etiketini kullanarak bir gelen kuralı gösteren görüntüsü](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)
 
- (isteğe bağlı) Uzaktan erişime izin vermek için 22 numaralı bağlantı noktasını gelen TCP trafiğine. Bu bağlantı noktası, yalnızca genel IP SSH kullanarak bağlanmak istiyorsanız gereklidir.
 
- Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

- İnternete giden herhangi bir bağlantı noktasında giden trafik. 

Değiştirme ya da toplu yapılandırılan Nsg'ler gelen/giden kuralları ekleme dikkatli olun. Bir NSG blokları iletişim, işlem düğümlerine sonra Machine Learning işlem Hizmetleri ayarlar işlem düğümlerinin durumunu için kullanılamaz.

Batch, kendi Nsg'ler yapılandırdığından alt ağ düzeyinde Nsg belirtmeniz gerekmez. Ancak, belirtilen alt ağ ilişkili Nsg'ler ve/veya bir güvenlik duvarı varsa gelen ve giden güvenlik kuralları daha önce belirtildiği gibi yapılandırın. 

Aşağıdaki ekran görüntüsünde, NSG kuralı yapılandırmasını Azure portalında nasıl göründüğünü gösterir:

![Machine Learning işlemi için ekran görüntüsü gelen NSG kuralları](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Machine Learning işlemi için ekran görüntüsü giden NSG kuralları](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a> Sanal ağdan giden bağlantı sınırlama

Giden varsayılan kuralları ve giden sanal ağınıza erişimini sınırlamak istediğiniz istemiyorsanız, aşağıdaki adımları izleyin:

- NSG kurallarını kullanarak giden internet bağlantısı Reddet 

- Azure depolamaya giden trafiği sınırlamak (kullanarak __hizmet etiketi__ , __Storage.Region_Name__ örn. Storage.EastUS), Azure Container Registry (kullanarak __hizmet etiketi__ , __AzureContainerRegistry.Region_Name__ örn. AzureContainerRegistry.EastUS) ve Azure Machine Learning hizmeti (kullanarak __hizmet etiketi__ , __AzureMachineLearning__)

Aşağıdaki ekran görüntüsünde, NSG kuralı yapılandırmasını Azure portalında nasıl göründüğünü gösterir:

![Machine Learning işlemi için ekran görüntüsü giden NSG kuralları](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)

### <a name="user-defined-routes-for-forced-tunneling"></a>Kullanıcı tanımlı yollar, zorlamalı tünel

Azure Machine Learning işlem ile zorlamalı tünel kullanıyorsanız, eklemeniz gerekir [kullanıcı tanımlı yollar (UDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) alt ağa bilgi işlem kaynağını içerir.

* Kullanıcı tanımlı bir yol kaynaklarınızı bulunduğu bölgede Azure Batch hizmeti tarafından kullanılan her bir IP adresi için. Bu Udr'ler görev zamanlama için işlem düğümleriyle iletişim kurmak için batch hizmetini etkinleştirin. Batch hizmetinin IP adreslerinin bir listesini almak için Azure desteğine başvurun.

* Azure Depolama'ya giden trafik (özellikle biçimindeki URL'ler `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, ve `<account>.blob.core.windows.net`) şirket içi ağ aletiniz tarafından engellenmediğinden gerekir.

Kullanıcı tanımlı yollar eklediğinizde, her bir ilgili Batch IP adresi ön eki için rota tanımlayın ve ayarlayın __sonraki atlama türü__ için __Internet__. Aşağıdaki görüntüde, Azure portalında bu UDR örneği gösterilmektedir:

![Örnek bir adres ön eki için kullanıcı tanımlı yol](./media/how-to-enable-virtual-network/user-defined-route.png)

Daha fazla bilgi için [sanal ağ içinde bir Azure Batch havuzu oluşturma](/azure/batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling) makalesi.

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Machine Learning işlem bir sanal ağ oluşturma

Azure portalını kullanarak bir Machine Learning işlem kümesi oluşturmak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınızı seçin.

1. İçinde __uygulama__ bölümünden __işlem__. Ardından __işlem Ekle__. 

    ![Azure Machine Learning hizmetinde bir işlem ekleme](./media/how-to-enable-virtual-network/add-compute.png)

1. Bir sanal ağ kullanmak için bu işlem kaynak yapılandırmak için bu seçenekleri kullanın:

    - __Ağ Yapılandırması__: __Gelişmiş__'i seçin.

    - __Kaynak grubu__: Sanal ağ içeren kaynak grubunu seçin.

    - __Sanal ağ__: Alt ağ içeren sanal ağı seçin.

    - __Alt ağ__: Kullanılacak alt ağı seçin.

   ![Makine öğrenimi işlemi için sanal ağ ayarları gösteren ekran görüntüsü](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Ayrıca, Azure Machine Learning SDK'sını kullanarak bir Machine Learning işlem kümesi oluşturabilirsiniz. Yeni bir Machine Learning işlem kümesinde aşağıdaki kodu oluşturur `default` adlı bir sanal ağın alt ağında `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")
    
    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name = vnet_resourcegroup_name,
                                                           vnet_name = vnet_name,
                                                           subnet_name = subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    
    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Oluşturma işlemini tamamladığında küme kullanarak modelinizi eğitebilirsiniz. Daha fazla bilgi için [seçin ve eğitim için bir işlem hedefine kullanma](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Bir sanal makine ya da HDInsight kümesi kullanma

Çalışma alanınız ile bir sanal ağdaki bir sanal makine ya da Azure HDInsight kümesi kullanmak için aşağıdaki adımları izleyin:

> [!IMPORTANT]
> Azure Machine Learning hizmeti yalnızca Ubuntu çalıştıran sanal makineleri destekler.

1. Azure portalı veya Azure CLI kullanarak bir VM veya HDInsight kümesi oluşturma ve bir Azure sanal ağına yerleştirin. Daha fazla bilgi için aşağıdaki belgelere bakın:
    * [Oluşturma ve Linux VM'ler için Azure sanal ağları yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Bir Azure sanal ağı kullanarak HDInsight'ı genişletin](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. VM veya küme SSH bağlantı noktası ile iletişim kurmak Azure Machine Learning hizmeti izin vermek için NSG için bir kaynak girişi yapılandırmanız gerekir. Genellikle SSH bağlantı noktası olan bağlantı noktası 22. Bu kaynak gelen trafiğe izin vermek için aşağıdaki bilgileri kullanın:

    * __Kaynak__: Seçin __hizmet etiketi__.

    * __Kaynak hizmet etiketi__: Seçin __AzureMachineLearning__.

    * __Kaynak bağlantı noktası aralıkları__: Seçin __*__.

    * __Hedef__: Seçin __herhangi__.

    * __Hedef bağlantı noktası aralıkları__: Seçin __22__.

    * __Protokol__: Seçin __herhangi__.

    * __Eylem__: Seçin __izin__.

   ![Bir VM veya HDInsight kümesindeki bir sanal ağ içinde deneme yapmak için gelen kuralları ekran görüntüsü](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Varsayılan giden kuralları, NSG'nin tutun. Daha fazla bilgi için bkz varsayılan güvenlik kuralları [güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).

    Giden varsayılan kuralları ve sanal ağınızın giden erişimi sınırlamak istiyorsanız bkz istemiyorsanız [sanal ağdan giden bağlantı sınırlama](#limiting-outbound-from-vnet)
    
1. VM veya HDInsight kümesi, Azure Machine Learning hizmeti çalışma alanınıza ekleyin. Daha fazla bilgi için [işlem hedeflerine yönelik model eğitiminin ayarlama](how-to-set-up-training-targets.md).

## <a name="use-azure-kubernetes-service"></a>Azure Kubernetes hizmeti kullanın

> [!IMPORTANT]
> Önkoşul denetimi ve kümenizin adımlara devam etmeden önce IP adresleme planlayın. Daha fazla bilgi için [Gelişmiş Yapılandırma Azure Kubernetes hizmetinde ağ](https://docs.microsoft.com/azure/aks/configure-advanced-networking).
> 
>
> Varsayılan giden kuralları, NSG'nin tutun. Daha fazla bilgi için bkz varsayılan güvenlik kuralları [güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).
>
> Azure Kubernetes hizmeti ve Azure sanal ağı, aynı bölgede olması gerekir.

Azure Kubernetes hizmeti bir sanal ağ içinde çalışma alanınıza eklemek için Azure portalında aşağıdaki adımları izleyin:

1. Gelen kuralı Azure Machine Learning hizmeti kullanmak için etkin olan sanal ağ denetimleri yapma emin NSG grubu __hizmet etiketi__ , __AzureMachineLearning__

    ![Azure Machine Learning hizmetinde bir işlem ekleme](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)     
 
1. İçinde [Azure portalında](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınızı seçin.

1. İçinde __uygulama__ bölümünden __işlem__. Ardından __işlem Ekle__. 

    ![Azure Machine Learning hizmetinde bir işlem ekleme](./media/how-to-enable-virtual-network/add-compute.png)

1. Bir sanal ağ kullanmak için bu işlem kaynak yapılandırmak için bu seçenekleri kullanın:

    - __Ağ Yapılandırması__: __Gelişmiş__'i seçin.

    - __Kaynak grubu__: Sanal ağ içeren kaynak grubunu seçin.

    - __Sanal ağ__: Alt ağ içeren sanal ağı seçin.

    - __Alt ağ__: Alt ağ seçin.

    - __Kubernetes hizmeti adres aralığı__: Kubernetes hizmeti adres aralığı seçin. Bu adres aralığı CIDR notasyonu IP aralığı küme için kullanılabilir IP adreslerini tanımlamak için kullanılır. Hiçbir alt ağ IP aralığı ile çakışmamalıdır. Örneğin: 10.0.0.0/16.

    - __Kubernetes DNS hizmeti IP adresi__: Kubernetes DNS hizmeti IP adresi seçin. Kubernetes DNS hizmetine bu IP adresi atanır. Kubernetes hizmeti adres aralığı içinde olmalıdır. Örneğin: 10.0.0.10.

    - __Docker köprü adresi__: Docker köprü adresi seçin. Docker köprüsüne bu IP adresi atanır. Hiçbir alt ağ IP aralığı veya Kubernetes hizmeti adres aralığı olmamalıdır. Örneğin: 172.17.0.1/16.

   ![Azure Machine Learning hizmeti: Makine öğrenimi işlem sanal ağ ayarları](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Sanal ağ olan denetimler gelen kuralı, gelen sanal ağ dışında çağrılabilir böylece Puanlama uç noktası için etkin yap emin NSG grubu

    ![Azure Machine Learning hizmetinde bir işlem ekleme](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)

    > [!TIP]
    > Bir AKS kümesi bir sanal ağda zaten varsa, çalışma alanına ekleyebilirsiniz. Daha fazla bilgi için [AKS'ye dağıtma](how-to-deploy-to-aks.md).

Ayrıca **Azure Machine Learning SDK'sı** bir sanal ağda Azure Kubernetes hizmeti eklemek için. Aşağıdaki kod yeni bir Azure Kubernetes hizmeti örneğinde oluşturur `default` adlı bir sanal ağın alt ağında `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace = ws,
                                  name = "myaks",
                                  provisioning_configuration = config)
```

Oluşturma işlemi tamamlandığında, çıkarım/puanına göre bir AKS kümesi bir sanal ağ arkasında kullanabilirsiniz. Daha fazla bilgi için [AKS'ye dağıtma](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim ortamlarını ayarlama](how-to-set-up-training-targets.md)
* [Modelleri dağıtılacağı yeri](how-to-deploy-and-where.md)
* [Modelleri SSL ile güvenli bir şekilde dağıtın](how-to-secure-web-service.md)

