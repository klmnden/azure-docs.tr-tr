---
title: Çalıştırma denemeleri ve sanal ağ içinde çıkarımı
titleSuffix: Azure Machine Learning service
description: Makine öğrenimi denemeleri ve bir Azure sanal ağ çıkarım güvenli hale getirme çalıştırın. İşlem hedeflerine yönelik model eğitiminin oluşturmayı öğrenin ve Azure sanal ağ içindeki çıkarımı yapma. Ayrıca güvenli sanal ağlar için gereksinimleri kapsar, gelen ve giden bağlantı noktaları gibi gerektirir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 01/08/2019
ms.openlocfilehash: fb67821d883317901617bda101ae91a9a92018c2
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246407"
---
# <a name="securely-run-experiments-and-inferencing-inside-an-azure-virtual-network"></a>Denemeler ve bir Azure sanal ağ içindeki çıkarım güvenli bir şekilde çalıştırın

Bu makalede, denemeleri ve sanal ağ içindeki çıkarım nasıl çalıştırılacağını öğrenin. Bir sanal ağ, Azure kaynaklarınızın genel internet'ten yalıtarak bir güvenlik sınırı görevi görür. Şirket içi ağınızı bir Azure sanal ağı da katılabilirsiniz. Güvenli bir şekilde, modelleri eğitme ve çıkarım için dağıtılan Modellerinizi erişim sağlar.

Diğer Azure Hizmetleri şeyler işlem kaynakları için Azure Machine Learning hizmeti kullanır. İşlem kaynakları (hedef işlem), modelleri eğitme ve kullanılır. Bu işlem, hedef sanal ağ içinde oluşturulabilir. Örneğin, bir veri bilimi sanal makinesi bir model eğitip ve ardından modeli için Azure Kubernetes hizmeti dağıtmak için kullanabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [sanal ağlarına genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) belge.

## <a name="storage-account-for-your-workspace"></a>Çalışma alanınız için depolama hesabı

Bir Azure Machine Learning hizmeti çalışma alanı oluşturduğunuzda, bir Azure depolama hesabı gerektirir. Bu depolama hesabı için güvenlik duvarı kurallarında etkinleştirmeyin. Azure Machine Learning hizmeti, depolama hesabında sınırsız erişim gerektirir.

Bu ayarları değiştiren emin değilseniz ya da olmayabilir, __varsayılan ağ erişim kuralını değiştirmek__ bölümünü [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security) belge ve kullanın adımları _erişime izin ver_ gelen __tüm ağlar__.

## <a name="use-machine-learning-compute"></a>Machine Learning işlem kullanma

Machine Learning işlem bir sanal ağ kullanmak için ağ gereksinimlerini anlamak için aşağıdaki bilgileri kullanın:

- Sanal ağ, Azure Machine Learning hizmeti çalışma alanı olarak aynı abonelik ve aynı bölgede olması gerekir.

- Küme için hedeflenen olması gerekir Machine Learning işlem kümesi için VM sayısı'nı barındırmak için yeterli sayıda atanmamış IP adresi alt ağ belirtilmiş. Alt ağ yeterli sayıda atanmamış IP adresleri yoksa, küme kısmen ayrılır.

- Sanal ağ trafiği kısıtlayarak güvenli planlıyorsanız, bazı bağlantı noktaları Machine Learning işlem hizmeti için açık bırakın gerekir. Daha fazla bilgi için [gerekli bağlantı noktaları](#mlcports) bölümü.

- Güvenlik ilkeleri ve sanal ağın abonelik veya kaynak grubunun kilitlerini sanal ağı yönetmek için izinleri kısıtlamak olup olmadığını denetleyin.

- Bir sanal ağda birden fazla Machine Learning işlem kümeleri yerleştirmek için kullanacaksanız, bir veya daha fazla kaynak için kota artışı isteğinde gerekebilir.

    Machine Learning işlem otomatik olarak sanal ağ içeren kaynak grubunu, ek ağ kaynakları ayırır. Machine Learning işlem her küme için Azure Machine Learning hizmeti, aşağıdaki kaynakları ayırır: 

    - Bir ağ güvenlik grubu (NSG)

    - Bir genel IP adresi

    - Bir yük dengeleyici

    Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-subscription-service-limits) sınırlıdır. 

### <a id="mlcports"></a> Gerekli bağlantı noktaları

Machine Learning işlem şu anda Azure Batch hizmeti belirtilen sanal ağdaki VM'ler sağlamak için kullanır. Alt ağın Batch hizmetinden gelen iletişimine izin vermelidir. Bu iletişim zamanlamak için kullanılan Azure depolama ve diğer kaynaklar ile iletişim kurmak için ve Machine Learning işlem düğümleri üzerinde çalışır. Batch, Vm'lere bağlı ağ arabirimleri (NIC) düzeyinde Nsg'ler ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

- Batch hizmet rolü IP adreslerinden 29876 ve 29877 numaralı bağlantı noktalarına gelen TCP trafiği. 
 
- Uzaktan erişime izin vermek için 22 numaralı bağlantı noktasını gelen TCP trafiğine.
 
- Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

- İnternete giden herhangi bir bağlantı noktasında giden trafik.

Değiştirme ya da toplu yapılandırılan Nsg'ler gelen/giden kuralları ekleme dikkatli olun. Bir NSG blokları iletişim, işlem düğümlerine sonra Machine Learning işlem Hizmetleri ayarlar işlem düğümlerinin durumunu için kullanılamaz.

Batch kendi NSG'lerini yapılandırdığından alt ağ düzeyinde NSG belirtmenize gerek yoktur. Ancak, belirtilen alt ağ ilişkili Nsg'ler ve/veya bir güvenlik duvarı varsa gelen ve giden güvenlik kuralları daha önce belirtildiği gibi yapılandırın. Aşağıdaki ekran görüntüleri, kural yapılandırma Azure portalında nasıl göründüğünü gösterir:

![Machine Learning işlemi için ekran görüntüsü gelen NSG kuralları](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Machine Learning işlemi için ekran görüntüsü giden NSG kuralları](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Machine Learning işlem bir sanal ağ oluşturma

Bir Machine Learning işlem kümesi ile oluşturmak için **Azure portalında**, aşağıdaki adımları kullanın:

1. Gelen [Azure portalında](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınızı seçin.

1. Gelen __uygulama__ bölümünden __işlem__. Ardından __işlem Ekle__. 

    ![Azure Machine Learning hizmetinde bir işlem ekleme](./media/how-to-enable-virtual-network/add-compute.png)

1. Bir sanal ağ kullanmak için bu işlem kaynak yapılandırmak için bu seçenekleri kullanın:

    - __Ağ Yapılandırması__: __Gelişmiş__'i seçin.

    - __Kaynak grubu__: Sanal ağ içeren kaynak grubunu seçin.

    - __Sanal ağ__: Alt ağ içeren sanal ağı seçin.

    - __Alt ağ__: Kullanılacak alt ağı seçin.

    ![Makine öğrenimi işlemi için sanal ağ ayarları gösteren ekran görüntüsü](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Bir Machine Learning işlem kümesi kullanarak da oluşturabilirsiniz **Azure Machine Learning SDK'sı**. Yeni bir Machine Learning işlem kümesinde aşağıdaki kodu oluşturur `default` adlı bir sanal ağın alt ağında `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure Virtual Network name, subnet, and resource group
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

Oluşturma işlemi tamamlandıktan sonra küme kullanarak modelinizi eğitebilirsiniz. Daha fazla bilgi için [seçin ve eğitim için bir işlem hedefine kullanma](how-to-set-up-training-targets.md) belge.

## <a name="use-a-virtual-machine-or-hdinsight"></a>Bir sanal makine ya da HDInsight kullanma

Çalışma alanınız ile bir sanal ağdaki bir sanal makine ya da HDInsight kümesi kullanmak için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Azure Machine Learning hizmeti yalnızca Ubuntu çalıştıran sanal makineleri destekler.

1. Azure portalı, Azure CLI kullanarak bir VM veya HDInsight kümesi oluşturmak vb. ve bir Azure sanal ağında yerleştirin. Daha fazla bilgi için aşağıdaki belgelere bakın:
    * [Oluşturma ve Linux VM'ler için Azure sanal ağları yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Azure sanal ağlarını kullanarak HDInsight'ı genişletin](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. VM veya küme SSH bağlantı noktası ile iletişim kurmak Azure Machine Learning hizmetine izin vermek için NSG için bir kaynak girişi yapılandırmanız gerekir. Genellikle SSH bağlantı noktası olan bağlantı noktası 22. Bu kaynak gelen trafiğe izin vermek için aşağıdaki bilgileri kullanın:

    * __Kaynak__: __Hizmet Etiketi__’ni seçin

    * __Kaynak hizmet etiketi__: Seçin __AzureMachineLearning__

    * __Kaynak bağlantı noktası aralıkları__: Seçin __*__

    * __Hedef__: Seçin __herhangi__

    * __Hedef bağlantı noktası aralıkları__: Seçin __22__

    * __Protokol__: Seçin __herhangi__

    * __Eylem__: Seçin __izin ver__

   ![Bir sanal ağ içindeki sanal makine veya HDInsight deneme yapmak için gelen kuralları ekran görüntüsü](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Varsayılan giden kuralları, NSG'nin tutun. Daha fazla bilgi için bkz varsayılan güvenlik kuralları bölümünde [güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) belgeleri.
    
1. VM veya HDInsight kümesi, Azure Machine Learning hizmeti çalışma alanınıza ekleyin. Daha fazla bilgi için [işlem hedeflerine yönelik model eğitiminin ayarlama](how-to-set-up-training-targets.md) belge.

## <a name="use-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'i (AKS) kullanma

> [!IMPORTANT]
> Lütfen Önkoşul denetimi ve Aşağıda sözü edilen adımlarla devam etmeden önce kümeniz için IP adresleme planlayın. Başvurabilirsiniz [Azure Kubernetes Service (AKS) ağ Gelişmiş Yapılandırma](https://docs.microsoft.com/azure/aks/configure-advanced-networking)
> 
> Varsayılan giden kuralları, NSG'nin tutun. Daha fazla bilgi için bkz varsayılan güvenlik kuralları bölümünde [güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) belgeleri.
>
> Azure Kubernetes hizmeti ve Azure sanal ağ aynı bölgede olması gerekir.

Azure Kubernetes hizmeti bir sanal ağ içinde çalışma alanınıza eklemek için aşağıdaki adımları kullanın. __Azure portalında__:

1. Gelen [Azure portalında](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınızı seçin.

1. Gelen __uygulama__ bölümünden __işlem__. Ardından __işlem Ekle__. 

    ![Azure Machine Learning hizmetinde bir işlem ekleme](./media/how-to-enable-virtual-network/add-compute.png)

1. Bir sanal ağ kullanmak için bu işlem kaynak yapılandırmak için bu seçenekleri kullanın:

    - __Ağ Yapılandırması__: __Gelişmiş__'i seçin.

    - __Kaynak grubu__: Sanal ağ içeren kaynak grubunu seçin.

    - __Sanal ağ__: Alt ağ içeren sanal ağı seçin.

    - __Alt ağ__: Alt ağ seçin.

    - __Kubernetes hizmeti adres aralığı__: Kubernetes hizmeti adres aralığı seçin. Bu adres aralığı CIDR notasyonu IP aralığı küme için kullanılabilir IP adreslerini tanımlamak için kullanılır. Hiçbir alt ağ IP aralığı ile çakışmamalıdır. Örneğin: 10.0.0.0/16.

    - __Kubernetes DNS hizmeti IP adresi__: Kubernetes DNS hizmeti IP adresi seçin. Kubernetes DNS hizmetine bu IP adresi atanır. Kubernetes hizmeti adres aralığında olmalıdır. Örneğin: 10.0.0.10.

    - __Docker köprü adresi__: Docker köprü adresi seçin. Docker köprüsüne bu IP adresi atanır. Hiçbir alt ağ IP aralığı veya Kubernetes hizmeti adres aralığında olmamalıdır. Örneğin: 172.17.0.1/16

   ![Azure Machine Learning hizmeti: Machine Learning işlem sanal ağ ayarları](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

    > [!TIP]
    > Bir AKS kümesi bir sanal ağda zaten varsa, çalışma alanına ekleyebilirsiniz. Daha fazla bilgi için [AKS'ye dağıtma](how-to-deploy-to-aks.md).

Ayrıca **Azure Machine Learning SDK'sı** bir sanal ağda Azure Kubernetes hizmeti eklemek için. Aşağıdaki kod yeni bir Azure Kubernetes Service'teki oluşturur `default` adlı bir sanal ağın alt ağında `mynetwork`:

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

Oluşturma işlemi tamamlandıktan sonra bir sanal ağ arkasında AKS kümesinde çıkarım yapabilirsiniz. Daha fazla bilgi için [AKS'ye dağıtma](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim ortamlarını ayarlama](how-to-set-up-training-targets.md)
* [Modelleri dağıtılacağı yeri](how-to-deploy-and-where.md)
* [Dağıtılan modellerinde SSL ile güvenli hale getirme](how-to-secure-web-service.md)
