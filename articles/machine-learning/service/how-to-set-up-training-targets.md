---
title: Azure Machine Learning hizmeti ile modeli eğitimi için işlem hedefleri ayarlama | Microsoft Docs
description: Seçin ve makine öğrenimi Modellerinizi eğitmek için kullanılan (hedef işlem) eğitim ortamları yapılandırma hakkında bilgi edinin. Azure Machine Learning hizmeti kolayca geçiş eğitim ortamları sağlar. Yerel olarak eğitim başlatın ve ölçeğini genişletmek gerekiyorsa, bir bulut tabanlı bir işlem hedefine geçiş yapın.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 07ea61ffe3ffc17cd255b826e3506ffe2b1ce9cd
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53017731"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Seçme ve modelinizi eğitmek için işlem hedefi kullanma

Azure Machine Learning hizmeti ile farklı işlem kaynakları, modelinize eğitebilirsiniz. Bu işlem kaynakları, adlı __hedefleri işlem__, yerel veya Bulut üzerinde olabilir. Bu belgede, desteklenen işlem hedefleri ve bunların nasıl kullanıldığı hakkında bilgi edineceksiniz.

Bir işlem hedefine Burada eğitim betiğinizi çalıştırmak veya modelinizi bir web hizmeti olarak dağıtıldığında barındırıldığı bir kaynaktır. Oluşturun ve Azure Machine Learning SDK'sı, Azure portal veya Azure CLI kullanarak bir işlem hedefine yönetin. Başka bir hizmete (örneğin, bir HDInsight kümesi) oluşturulan işlem hedefleri varsa, Azure Machine Learning hizmeti çalışma alanınıza ekleyerek kullanabilirsiniz.

Azure Machine Learning destekleyen işlem hedefleri üç kategoriden vardır:

* __Yerel__: yerel makinenize veya geliştirme/deneme ortamı olarak kullanabileceğiniz bulut tabanlı bir VM. 

* __İşlem yönetilen__: Azure Machine Learning işlem olduğundan Azure Machine Learning hizmeti tarafından yönetilen bir işlem sunar. Tek veya çok node işlem eğitim, test ve batch çıkarım kolayca oluşturmanıza olanak tanır.

* __İşlem bağlı__: Ayrıca kendi Azure bulut bilgi işlem getirin ve Azure Machine Learning ile ekleyin. Daha fazla aşağıda desteklenen işlem türleri ve bunların nasıl kullanıldığı hakkında okuyun.


## <a name="supported-compute-targets"></a>Desteklenen işlem hedefleri

Azure Machine Learning hizmeti çeşitli işlem hedef arasında değişen desteğe sahiptir. Az miktarda veriniz üzerinde dev/deneme ile tipik model geliştirme yaşam döngüsü başlatır. Bu aşamada, yerel bir ortamı kullanmanızı öneririz. Örneğin, yerel bilgisayarınıza veya bulut tabanlı bir VM. Büyük veri kümeleri üzerinde eğitim ölçeğini veya dağıtılmış eğitimi yapmak gibi bir Farklı Çalıştır gönderdiğiniz her zaman bu daralttığında tek veya çok node küme oluşturmak için Azure Machine Learning işlem kullanmanızı öneririz. Çeşitli senaryolarda olarak değişiklik gösterebilir destek aşağıda ayrıntılarıyla olsa da, kendi işlem kaynağı ekleyebilirsiniz:

|Hedef işlem| GPU hızlandırma | Otomatik hiper parametre ayarı | Otomatik makine öğrenimi | Kolay bir işlem hattı|
|----|:----:|:----:|:----:|:----:|
|[Yerel bilgisayar](#local)| Belki de | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning işlem](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Uzak VM](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics'i](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ Azure Databricks ve Azure Data Lake Analytics __yalnızca__ bir işlem hattında kullanılabilir. İşlem hatları hakkında daha fazla bilgi için bkz. [Azure Machine Learning işlem hatlarında](concept-ml-pipelines.md) belge.

> [!IMPORTANT]
> Azure Machine Learning işlem alanından bir çalışma alanı içinde oluşturulmuş olması gerekir. Bir çalışma alanına mevcut örneklerdeki eklenemiyor.
>
> Diğer işlem hedefleri dışında Azure Machine Learning oluşturulur ve ardından çalışma alanınıza bağlı.

> [!NOTE]
> Bazı hedefler üzerinde Docker kapsayıcı görüntülerini bir modeli eğitimindeki kullanan işlem. GPU temel görüntü yalnızca Microsoft Azure hizmetleri üzerinde kullanılmalıdır. Model yönetimi için bu hizmetler şunlardır:
>
> * Azure Machine Learning işlem
> * Azure Kubernetes Service
> * Veri bilimi sanal makinesi.

## <a name="workflow"></a>İş akışı

Geliştirme ve Azure Machine Learning ile bir model dağıtma iş akışı adımları izler:

1. Machine learning Python betiklerini eğitim geliştirin.
1. Oluşturma, yapılandırma veya var olan bir işlem hedef ekleyin.
1. Eğitim betikleriniz işlem hedefine gönderin.
1. En iyi modeli bulmak için sonuçları inceleyin.
1. Model kayıt defterinde modeli kaydedin.
1. Model dağıtma.

> [!IMPORTANT]
> Eğitim betiğinizi belirli bir işlem hedefine bağlı değildir. Yerel bilgisayarınızda başlangıçta eğitim ve eğitim betiği yeniden yazmak zorunda kalmadan işlem hedefleri geçiş.

> [!TIP]
> Bir işlem hedefine çalışma ile ilişkilendirmek her yönetilen oluşturarak işlem veya mevcut işlem iliştirme, işlem için bir ad sağlamanız gerekir. Bu, 2 ila 16 karakter uzunluğunda olmalıdır.

Bir işlem hedefinden başka bir hesaba geçiyorum oluşturmanız gerekir bir [çalıştırma yapılandırmasını](concept-azure-machine-learning-architecture.md#run-configuration). Çalıştırma yapılandırma işlem hedef betiği çalıştırmak nasıl tanımlar.

## <a name="training-scripts"></a>Eğitim betikleriniz

Bir eğitim çalıştırması başlattığınızda, eğitim komut dosyalarınızı içeren dizine anlık görüntüsünü oluşturulur ve işlem hedefine gönderildi. Daha fazla bilgi için [anlık görüntüleri](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Yerel bilgisayar

Yerel eğitim, eğitim işlemi göndermek için SDK'yi kullanın. Kullanıcı tarafından yönetilen ya da sistem tarafından yönetilen bir ortamı kullanarak eğitebilirsiniz.

### <a name="user-managed-environment"></a>Kullanıcı tarafından yönetilen ortamı

Kullanıcı tarafından yönetilen bir ortamda, gerekli tüm paketleri betiği çalıştırmak için seçtiğiniz Python ortamında kullanılabilir olduğundan emin olmak sizin sorumluluğunuzdadır. Aşağıdaki kod parçacığı eğitim kullanıcı tarafından yönetilen bir ortam için yapılandırma örneği verilmiştir:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Sistem tarafından yönetilen ortamı

Bağımlılıkları yönetmek için conda ortamları sistem yönetilen kullanır. Conda adlı bir dosya oluşturur `conda_dependencies.yml` bu bağımlılıkların bir listesini içerir. Ardından sistemin içinde komut dosyalarınızı çalıştırın ve yeni bir conda ortamı oluşturmak isteyebilirsiniz. Ortamları sistem yönetilen olabilir yeniden kullanılan daha sonra sürece `conda_dependencies.yml` dosya değişmeden kalır. 

Yeni bir ortam kurmak ilk kurulumu gerekli bağımlılıkları boyutuna bağlı olarak tamamlanması birkaç dakika sürebilir. Aşağıdaki kod parçacığı, sistem tarafından yönetilen bir ortam oluşturma, üzerinde scikit bağlıdır gösterir-öğrenin:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Azure Machine Learning işlem

Azure Machine Learning işlem tek - için çok - node işlem kolayca oluşturmasına olanak tanır, bir yönetilen bilgi işlem altyapısıdır. Oluşturulduğu __, çalışma alanı bölgesi içinde__ ve çalışma alanınızdaki diğer kullanıcılarla paylaşılabilir bir kaynaktır. Bir iş gönderilir ve bir Azure sanal ağında koyabilirsiniz zaman bunu otomatik olarak ölçeklendirilebilir. İçinde yürüten bir __kapsayıcılı ortam__, bir Docker kapsayıcısında modelinizin bağımlılıkları paketleme.

Azure Machine Learning işlem eğitim işlemin CPU veya GPU işlem düğümü bulutta bir küme dağıtmak için kullanabilirsiniz. GPU'ları içeren VM boyutları hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) belgeleri.

> [!NOTE]
> Azure Machine Learning işlemi ayrılan çekirdek sayısı gibi şeyleri varsayılan sınırlara sahiptir. Daha fazla bilgi için [Azure kaynaklarını yönetin ve istek kotalarını](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas) belge.

Azure Machine Learning işlem isteğe bağlı bir çalıştırma zamanladığınızda veya kalıcı bir kaynak olarak oluşturabilirsiniz.

### <a name="run-based-creation"></a>Çalıştırma tabanlı oluşturma

Çalışma zamanında işlem hedefi olarak Azure Machine Learning işlem oluşturabilirsiniz. Bu durumda, işlem çalıştırmanız, çalıştırma, yapılandırmada belirttiğiniz max_nodes kadar ölçekler için otomatik olarak oluşturulur ve sonra __otomatik olarak silinmesini__ çalıştırma tamamlandıktan sonra.

> [!IMPORTANT]
> Azure Machine Learning işlem çalışma tabanlı oluşturulması şu anda Önizleme aşamasındadır. Hiper parametre ayarı veya Machine Learning otomatik kullanıyorsanız çalışma tabanlı olarak oluşturulmasını kullanmayın. Hiper parametre ayarı veya Machine Learning otomatik kullanmanız gerekiyorsa, Azure Machine Learning işlem çalıştırma göndermeden önce oluşturun.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

#Let us first list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use AmlCompute to execute script.
run_config.target = "amlcompute"

# AmlCompute will be created in the same region as workspace. Set vm size for AmlCompute from the list returned above
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Kalıcı işlem (Temel)

Bir kalıcı Azure Machine Learning işlem birden fazla iş arasında yeniden kullanılabilir. Çalışma alanındaki diğer kullanıcılarla paylaşılabilir ve işleri arasında tutulur.

Azure Machine Learning işlem kalıcı kaynak oluşturmak için belirttiğiniz `vm_size` ve `max_nodes` parametreleri. Azure Machine Learning, ardından parametreleri geri kalanı için akıllı Varsayılanları kullanır.  Örneğin, işlem, otomatik ölçeklendirme kullanılmadığı sıfır düğümleri aşağı ve adanmış sanal makineler oluşturmak için gerektiği şekilde işlerinizi çalıştırmak için ayarlanır. 

* **vm_size**: Azure Machine Learning işlem tarafından oluşturulan düğümler VM ailesi.
* **max_nodes**: Azure Machine Learning işlem bir işi çalışırken otomatik ölçeklendirme için en fazla düğümü.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Kalıcı işlem (Gelişmiş)

Ayrıca, Azure Machine Learning işlem oluşturulurken çeşitli gelişmiş özellikler yapılandırabilirsiniz.  Bu özellikler, aboneliğinize sabit boyutlu ya da mevcut bir Azure sanal ağ içindeki kalıcı bir küme oluşturmanıza imkan tanır.

Ek olarak `vm_size` ve `max_nodes`, aşağıdaki özellikleri kullanabilirsiniz:

* **min_nodes**: Azure Machine Learning işlemi bir iş çalıştırmak while aşağı ölçeklemenizi için en az düğümler (varsayılan 0 düğümleri).
* **vm_priority**: Azure Machine Learning işlem oluşturulurken 'ayrılmış' (varsayılan) ve 'lowpriority' VM'ler arasında seçin. Düşük öncelikli VM'ler Azure'nın aşırı kapasitesini kullanın ve bu nedenle ucuz ancak erine olan çalıştırma risk.
* **idle_seconds_before_scaledown**: otomatik ölçeklendirmeyi min_nodes için önce çalıştırma tamamlandıktan sonra beklenecek boşta kalma süresi (varsayılan 120 saniye).
* **vnet_resourcegroup_name**: kaynak grubu __mevcut__ sanal ağ. Azure Machine Learning işlem, bu sanal ağ içinde oluşturulur.
* **vnet_name**: sanal ağ adı. Sanal ağ, Azure Machine Learning çalışma alanı ile aynı bölgede olması gerekir.
* **subnet_name**: sanal ağ içindeki alt ağın adı. Azure Machine Learning işlem kaynakları, bu alt ağı aralığından IP adresleri atanır.

> [!TIP]
> Azure Machine Learning işlem kalıcı bir kaynak oluştururken de min_nodes veya max_nodes gibi özellikleri güncelleştirmek olanağına da sahip olursunuz. Yalnızca çağrı `update()` işlevi de.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           vm_priority='lowpriority',
                                                           min_nodes=2,
                                                           max_nodes=4,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```


## <a id="vm"></a>Uzak VM

Azure Machine Learning, ayrıca kendi işlem kaynağı getiren ve çalışma alanınıza eklenmesini destekler. Azure Machine Learning hizmetini erişilebilir olduğu sürece bu tür bir kaynak türü bir rasgele uzak vm'dir. Bir Azure VM veya kuruluş veya şirket içi uzak bir sunucuda olabilir. Özellikle, verilen IP adresi ve kimlik (kullanıcı adı/parola veya SSH anahtarı), tüm erişilebilir VM'ler uzaktan çalıştırmalar için kullanabilirsiniz.
Bir Docker kapsayıcısı, zaten var olan bir Python ortamını veya sistem tarafından oluşturulan conda ortamda kullanabilirsiniz. Docker kapsayıcısı kullanarak yürütme VM'de çalışan Docker altyapısına sahip olmasını gerektirir. Yerel makinenize daha esnek, bulut tabanlı geliştirme/deneme ortamı istediğinizde, bu işlev özellikle yararlıdır.

> [!TIP]
> Veri bilimi sanal makinesi, bu senaryo için tercih ettiğiniz Azure VM olarak kullanarak önerilir. Bu önceden yapılandırılmış bir veri bilimi ve yapay ZEKA geliştirme ortamında Azure seçkin seçeneğiyle araç ve çerçeve ML geliştirme tam yaşam döngüsü için olur. Azure Machine Learning ile veri bilimi sanal makinesi kullanma hakkında daha fazla bilgi için bkz. [geliştirme ortamını yapılandırma](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm) belge.

> [!WARNING]
> Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler. Bir sanal makine oluştururken veya mevcut bir seçerek, Ubuntu kullanan bir seçtiğinizde gerekir.

Eğitim hedef olarak bir veri bilimi sanal makinesi (DSVM) yapılandırmak için SDK'sı aşağıdaki adımları kullanın:

1. Mevcut bir sanal makine işlem hedefi olarak eklemek için sanal makine için tam etki alanı adı, oturum açma adı ve parola sağlamalısınız.  Bu örnekte değiştirin ```<fqdn>``` VM veya genel IP adresi genel tam etki alanı adı. Değiştirin ```<username>``` ve ```<password>``` SSH kullanıcı adı ve sanal makine için parola ile:

    ```python
    from azureml.core.compute import RemoteCompute

    dsvm_compute = RemoteCompute.attach(ws,
                                    name="attach-dsvm",
                                    username='<username>',
                                    address="<fqdn>",
                                    ssh_port=22,
                                    password="<password>")

    dsvm_compute.wait_for_completion(show_output=True)

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks, Azure bulutta Apache Spark tabanlı bir ortam olan. Bir Azure Machine Learning işlem hattı ile bir işlem hedefine eğitimindeki modeller gibi kullanılabilir.

> [!IMPORTANT]
> Bir Azure Databricks işlem hedefi bir Machine Learning işlem hattında yalnızca kullanılabilir.
>
> Modelinizi eğitmek için kullanmadan önce bir Azure Databricks çalışma alanı oluşturmanız gerekir. Bu kaynak oluşturmak için bkz [Azure Databricks'te Spark işini çalıştırma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) belge.

Azure Databricks işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

* __İşlem adı__: Bu işlem kaynağına atamak istediğiniz ad.
* __Kaynak Kimliği__: Azure Databricks çalışma alanı kaynak kimliği. Bu değer için biçim örneği aşağıda gösterilmiştir:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.Databricks/workspaces/<databricks-workspace-name>
    ```

    > [!TIP]
    > Kaynak Kimliğini almak için aşağıdaki Azure CLI komutunu kullanın. Değiştirin `<databricks-ws>` , Databricks çalışma alanı adı:
    > ```azurecli-interactive
    > az resource list --name <databricks-ws> --query [].id
    > ```

* __Erişim belirteci__: Azure Databricks için kimliğini doğrulamak için kullanılan erişim belirteci. Bir erişim belirteci oluşturmak için bkz: [kimlik doğrulaması](https://docs.azuredatabricks.net/api/latest/authentication.html) belge.

Aşağıdaki kod, Azure Databricks işlem hedefi olarak eklemek gösterilmektedir:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_resource_id = os.environ.get("AML_DATABRICKS_RESOURCE_ID", "<databricks_resource_id>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_resource_id {}'.format(databricks_resource_id))
    print('databricks_access_token {}'.format(databricks_access_token))
    databricks_compute = DatabricksCompute.attach(
             workspace=ws,
             name=databricks_compute_name,
             resource_id=databricks_resource_id,
             access_token=databricks_access_token
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics'i

Azure Data Lake Analytics, Azure bulutta büyük veri analiz platformudur. Bir Azure Machine Learning işlem hattı ile bir işlem hedefine eğitimindeki modeller gibi kullanılabilir.

> [!IMPORTANT]
> Bir Azure Data Lake Analytics işlem hedefi bir Machine Learning işlem hattında yalnızca kullanılabilir.
>
> Modelinizi eğitmek için kullanmadan önce bir Azure Data Lake Analytics hesabı oluşturmanız gerekir. Bu kaynak oluşturmak için bkz [Azure Data Lake Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) belge.

Data Lake Analytics işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

* __İşlem adı__: Bu işlem kaynağına atamak istediğiniz ad.
* __Kaynak Kimliği__: Data Lake Analytics hesabı kaynak kimliği. Bu değer için biçim örneği aşağıda gösterilmiştir:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.DataLakeAnalytics/accounts/<datalakeanalytics-name>
    ```

    > [!TIP]
    > Kaynak Kimliğini almak için aşağıdaki Azure CLI komutunu kullanın. Değiştirin `<datalakeanalytics>` Data Lake Analytics hesap adınızı adı:
    > ```azurecli-interactive
    > az resource list --name <datalakeanalytics> --query [].id
    > ```

Aşağıdaki kod, Data Lake Analytics işlem hedefi olarak eklemek gösterilmektedir:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_id = os.environ.get("AML_ADLA_RESOURCE_ID", "<adla_resource_id>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_id))
    adla_compute = AdlaCompute.attach(
             workspace=ws,
             name=adla_compute_name,
             resource_id=adla_resource_id
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning işlem hatlarını, yalnızca varsayılan Data Lake Analytics hesabı veri deposunda depolanan verilerle çalışabilirsiniz. Veri gerekirse çalışma varsayılandan farklı bir depoda, kullanabilirsiniz bir [ `DataTransferStep` ](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) eğitim önce verileri kopyalamak için.

## <a id="hdinsight"></a>Azure HDInsight 

HDInsight, büyük veri analizi için popüler bir platformdur. Apache Spark, modelinizi eğitmek için kullanılan sağlar.

> [!IMPORTANT]
> Modelinizi eğitmek için kullanmadan önce HDInsight kümesi de oluşturmanız gerekir. HDInsight kümesinde bir Spark oluşturmak için bkz: [HDInsight Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) belge.
>
> Kümeyi oluştururken bir SSH kullanıcı adı ve parola belirtmeniz gerekir. HDInsight işlem hedefi kullanılırken gerek duyduğunuzda, bu değerleri unutmayın.
>
> Küme oluşturulduktan sonra bir tam etki alanı adı (FQDN), erişiminizde `<clustername>.azurehdinsight.net`burada `<clustername>` küme için belirtilen adı. Bir işlem hedefi olarak kullanmak için bu adresi (veya kümesinin genel IP adresi) gerekir

HDInsight işlem hedefi yapılandırmak için HDInsight kümesi için tam etki alanı adı, küme oturum açma adı ve parola sağlamalısınız. Aşağıdaki örnek, bir küme çalışma alanınıza eklemek için SDK'sını kullanır. Bu örnekte değiştirin `<fqdn>` küme veya genel IP adresi genel tam etki alanı adı ile. Değiştirin `<username>` ve `<password>` ile SSH kullanıcı adı ve parola küme için:

> [!NOTE]
> Kümeniz için FQDN bulmak için Azure portalını ziyaret edin ve HDInsight kümenizi seçin. Gelen __genel bakış__ bölümünde FQDN: parçası __URL__ girişi. Yalnızca kaldırmak `https://` değerin başından itibaren.
>
> ![Vurgulanan URL girişi ile HDInsight kümesi genel bakış görüntüsü](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Çalıştırma eğitim gönderin

Bir eğitim çalıştırma göndermek için iki yolu vardır:

* Gönderme bir `ScriptRunConfig` nesne.
* Gönderme bir `Pipeline` nesne.

> [!IMPORTANT]
> Azure Databricks ve Azure Datalake Analytics hedefleri yalnızca bir işlem hattında kullanılabilen işlem.
> Yerel işlem hedefi, bir işlem hattında kullanılamaz.

### <a name="submit-using-scriptrunconfig"></a>Kullanarak Gönder `ScriptRunConfig`

Bir eğitim göndermek için kod desenini kullanarak çalıştıran `ScriptRunConfig` işlem hedef bağımsız olarak aynıdır:

* Oluşturma bir `ScriptRunConfig` işlem hedefi çalıştırma Yapılandırması'nı kullanarak nesne.
* Çalıştırma gönderin.
* Çalıştırmak için bekleyin.

Aşağıdaki örnek, bu belgede daha önce oluşturulan sistem tarafından yönetilen yerel işlem hedefi için yapılandırmayı kullanır:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="submit-using-a-pipeline"></a>Bir işlem hattı kullanarak Gönder

Kod bir eğitim çalışan bir işlem hattı kullanarak gönderme işlem hedefi aynı için Desen:

* İşlem kaynağı için işlem hattı için bir adım ekleyin.
* İşlem hattını kullanarak farklı çalıştır gönderin.
* Çalıştırmak için bekleyin.

Aşağıdaki örnek, bu belgede daha önce oluşturduğunuz Azure Databricks işlem hedefini kullanır:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Machine learning işlem hatlarını hakkında daha fazla bilgi için bkz. [işlem hatları ve Azure Machine Learning](concept-ml-pipelines.md) belge.

Örneğin, bir işlem hattı, eğitim gösteren Jupyter Notebook bkz [ https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline ](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Görüntüleyebilir ve Azure portalını kullanarak hesaplamayı ayarlayabilirsiniz

Hangi hedef Azure portalından, bir çalışma alanıyla ilişkili işlem görüntüleyebilirsiniz. Listeye almak için aşağıdaki adımları kullanın:

1. Ziyaret [Azure portalında](https://portal.azure.com) ve çalışma alanınıza gidin.
2. Tıklayarak __işlem__ altında bağlantı __uygulamaları__ bölümü.

    ![Görünüm işlem sekmesi](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

İşlem hedeflerin listesini görüntülemek için yukarıdaki adımları izleyin ve sonra da işlem hedefi oluşturmak için aşağıdaki adımları kullanın:

1. Tıklayın __+__ işlem hedefi eklemek oturum açın.

    ![İşlem ekle ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. İşlem hedefi için bir ad girin
1. Seçin **Machine Learning işlem** kullanılmak üzere işlem türü olarak __eğitim__

    > [!IMPORTANT]
    > Eğitim için yönetilen bir işlem olarak Azure Machine Learning işlem yalnızca oluşturabilirsiniz

1. Özellikle VM ailesi ve en fazla düğüme işlem olunan kullanılmak üzere gerekli formu doldurun. 
1. __Oluştur__’u seçin
1. İşlem hedef listeden seçerek oluşturma işleminin durumunu görüntüleyebilirsiniz.

    ![İşlem listesini görüntüle](./media/how-to-set-up-training-targets/View_list.png)

1. Ardından, işlem hedef ayrıntılarını görürsünüz.

    ![Ayrıntıları görüntüle](./media/how-to-set-up-training-targets/compute-target-details.png)

1. Artık bu hedeflere yukarıdaki ayrıntılı olarak karşı çalıştırma gönderebilirsiniz.


### <a name="reuse-existing-compute-in-your-workspace"></a>Yeniden çalışma alanınızdaki mevcut işlem

İşlem hedeflerin listesini görüntülemek için yukarıdaki adımları izleyin ve ardından hedef işlem yeniden kullanmak için aşağıdaki adımları kullanın:

1. Tıklayın **+** işlem hedefi eklemek oturum açın
2. İşlem hedefi için bir ad girin
3. İçin eklemek için işlem türünü seçin __eğitim__

    > [!IMPORTANT]
    > Tüm türleri, portalı kullanarak eklenebilecek işlem.
    > Şu anda eğitim eklenebilecek türleri şunlardır:
    > 
    > * Uzak VM
    > * Databricks
    > * Data Lake Analytics
    > * HDInsight

1. Gerekli formu doldurun

    > [!NOTE]
    > Parolalara göre daha güvenli oldukları gibi Microsoft SSH anahtarları kullanmanızı önerir. SSH anahtarları şifreleme imzalarını kullanır ancak parola deneme yanılma saldırılarına karşı savunmasız. Azure sanal makineler ile kullanmak için SSH anahtarları oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:
    >
    > * [Oluşturma ve Linux veya Macos'ta SSH anahtarlarını kullanma]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Oluşturma ve Windows üzerinde SSH anahtarlarını kullanma]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Select ekleme
1. İşlem hedef listeden seçerek iliştirme işlemi durumunu görüntüleyebilirsiniz.
1. Artık bu hedeflere yukarıdaki ayrıntılı olarak karşı çalıştırma gönderebilirsiniz.

## <a name="examples"></a>Örnekler
Not Defterleri şu konumlarda bakın:
* [Yardım-How-to-kullanın-azureml/eğitimi](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)

* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning SDK başvurusu](https://aka.ms/aml-sdk)
* [Öğretici: Modeli eğitme](tutorial-train-models-with-aml.md)
* [Modelleri dağıtılacağı yeri](how-to-deploy-and-where.md)
* [Machine learning işlem hatlarını Azure Machine Learning hizmeti ile derleme](concept-ml-pipelines.md)
