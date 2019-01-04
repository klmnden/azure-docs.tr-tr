---
title: İşlem modeli eğitim hedefleri
titleSuffix: Azure Machine Learning service
description: (Hedef işlem) eğitim ortamları için makine öğrenme modeli eğitimi yapılandırın. Eğitim ortamlar arasında kolayca geçiş yapabilirsiniz. Eğitim yerel olarak başlatın. Ölçeği genişletmek gerekiyorsa, bir bulut tabanlı bir işlem hedefine geçiş yapın.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 9037f6d7602f186bc30e55acbc050280bca134ee
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794473"
---
# <a name="set-up-compute-targets-for-model-training"></a>İşlem hedeflerine yönelik model eğitiminin ayarlama

Azure Machine Learning hizmeti ile farklı işlem kaynakları, modelinize eğitebilirsiniz. Bu işlem kaynakları, adlı __hedefleri işlem__, yerel veya Bulut üzerinde olabilir. Bu makalede, desteklenen işlem hedefleri ve bunları nasıl kullanacağınızı öğrenin.

İşlem hedefi, bir web hizmeti olarak dağıtıldığında eğitim betiği veya ev sahipliği modelinizi çalıştırdığınız bir kaynaktır. Oluşturabilir ve Azure Machine Learning SDK'sı, Azure portalında veya Azure CLI kullanarak bir işlem hedefine yönetebilirsiniz. Başka bir hizmete (örneğin, bir Azure HDInsight kümesi) oluşturulan işlem hedefleri varsa, Azure Machine Learning hizmeti çalışma alanınıza ekleyerek bu hedefler kullanabilirsiniz.

Azure Machine Learning destekleyen işlem hedefleri üç kategoriden vardır:

* __Yerel__: Yerel makinenizde veya bir bulut tabanlı sanal bir geliştirme ve deneme ortamı olarak kullanabileceğiniz makine (VM). 
* __Yönetilen işlem__: Azure Machine Learning işlem Azure Machine Learning hizmeti tarafından yönetilen bir işlem teklifi olur. Teklif, eğitim, test ve batch çıkarım tek veya çok düğümlü bir işlem kolayca oluşturmanıza olanak sağlar.
* __İşlem bağlı__: Ayrıca, kendi Azure bulut bilgi işlem getirin ve Azure Machine Learning ile ekleyin. Hakkında daha fazla bilgi işlem türleri ve bunları aşağıdaki bölümlerde kullanma desteklenmiyor.


## <a name="supported-compute-targets"></a>Desteklenen işlem hedefleri

Azure Machine Learning hizmeti çeşitli işlem hedef arasında değişen desteğe sahiptir. Geliştirme ve küçük bir veri miktarına deneme tipik model geliştirme yaşam döngüsü başlatır. Bu aşamada, yerel bilgisayarınıza veya bulut tabanlı bir VM gibi yerel bir ortamı kullanmanızı öneririz. Bir çalıştırma, daha büyük veri kümeleri üzerinde eğitim ölçeğini dağıtılmış Eğitimi, kullanımı bir Azure Machine Learning işlem ortamını tek bir oluşturmak için veya her zaman çok düğümlü küme bu ihtiyaçlara göre otomatik olarak gönderin. Çeşitli senaryolarda olarak değişebilir desteği aşağıdaki tabloda açıklanan olsa da, kendi işlem kaynağı ekleyebilirsiniz:

|Hedef işlem| GPU hızlandırma | Otomatik<br/> Hiper parametre ayarı | Otomatik</br> makine öğrenimi | Kolay bir işlem hattı|
|----|:----:|:----:|:----:|:----:|
|[Yerel bilgisayar](#local)| Belki de | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning işlem](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Uzak VM](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics'i](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ _Yalnızca bir işlem hattı, Azure Databricks ve Azure Data Lake Analytics kullanılabilir._<br/>
> İşlem hatları hakkında daha fazla bilgi için bkz. [Azure Machine Learning işlem hatlarında](concept-ml-pipelines.md).
>
> Azure Machine Learning işlem ortamını bir çalışma alanı içinde oluşturulması gerekir. Bir çalışma alanına mevcut örneklerdeki eklenemiyor.
>
> Diğer işlem hedefleri dışında Azure Machine Learning oluşturulur ve ardından çalışma alanınıza bağlı.
>
> Bir model eğitip, hedefler üzerinde Docker kapsayıcı görüntüleri kullanan bazı işlem. GPU temel görüntü yalnızca Microsoft Azure hizmetleri üzerinde kullanılmalıdır. Model yönetimi için hizmetler şunlardır:
> * Azure Machine Learning işlem
> * Azure Kubernetes Service
> * Windows veri bilimi sanal makinesi (DSVM)

## <a name="workflow"></a>İş akışı

Geliştirme ve Azure Machine Learning ile bir model dağıtma iş akışı adımları izler:

1. Makine öğrenimi eğitim betikleriniz python'da geliştirin.
1. Oluşturma ve işlem hedef yapılandırabilir veya var olan bir işlem hedef ekleyin.
1. Eğitim betikleriniz işlem hedefine gönderin.
1. En iyi modeli bulmak için sonuçları inceleyin.
1. Model kayıt defterinde modeli kaydedin.
1. Model dağıtma.

> [!NOTE]
> Eğitim betiğinizi belirli bir işlem hedefine bağlı değildir. Yerel bilgisayarınızda başlangıçta eğitim ve eğitim betiği yeniden yazmak zorunda kalmadan işlem hedefleri geçiş.
> 
> Oluşturarak işlem hedefi çalışma alanınız ile ilişkilendirdiğinizde yönetilen bir işlem veya mevcut bir işlem ekleyerek, işlem için bir ad sağlayın. Ad, iki ile 16 arasında olmalıdır. karakter uzunluğunda.

Bir işlem hedefine geçiş yapmak için gereken bir [çalıştırma yapılandırmasını](concept-azure-machine-learning-architecture.md#run-configuration). Çalıştırma yapılandırma işlem hedef betiği çalıştırmak nasıl tanımlar.

## <a name="training-scripts"></a>Eğitim betikleriniz

Bir eğitim çalıştırması başlattığınızda, eğitim betikleriniz içerir ve işlem hedefe gönderen directory anlık görüntüsünü oluşturur. Daha fazla bilgi için [anlık görüntüleri](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Yerel bilgisayar

Yerel olarak eğittiğinizde, eğitim işlemi göndermek için SDK'yi kullanın. Kullanıcı tarafından yönetilen ya da sistem tarafından yönetilen bir ortamı kullanarak eğitebilirsiniz.

### <a name="user-managed-environment"></a>Kullanıcı tarafından yönetilen ortamı

Kullanıcı tarafından yönetilen bir ortamda, tüm gerekli paketleri betik çalıştırdığı Python ortamında kullanılabilir olduğundan emin olun. Aşağıdaki kod parçacığı, bir kullanıcı tarafından yönetilen ortam için eğitim yapılandırma örneğidir:

```python
from azureml.core.runconfig import RunConfiguration

# Edit a run configuration property on the fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# Choose a specific Python environment by pointing to a Python path. For example:
# run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Sistem tarafından yönetilen ortamı

Bağımlılıkları yönetmek için conda ortamları sistem yönetilen kullanır. Conda adlı bir dosya oluşturur **conda_dependencies.yml** bu bağımlılıkların bir listesini içerir. Sistemin var. komut dosyalarınızı çalıştırın ve yeni bir conda ortamı oluşturmak isteyebilirsiniz. Conda_dependencies.yml Dosya değiştirilmediği sürece ortamları sistem yönetilen daha sonra yeniden kullanılabilir. 

Yeni bir ortam kurmak ilk kurulumu, gerekli bağımlılıkları boyutuna göre tamamlanması birkaç dakika sürebilir. Aşağıdaki kod parçacığını üzerinde scikit bağlıdır sistem tarafından yönetilen bir ortam oluşturmak nasıl gösterir-öğrenin:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify the conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Azure Machine Learning işlem

Azure Machine Learning işlem kolayca tek veya çok düğümlü bir işlem oluşturmak kullanıcıya izin veren bir yönetilen işlem altyapısıdır. İşlem, çalışma alanı bölge içinde çalışma alanınızda diğer kullanıcılarla paylaşılabilir bir kaynak oluşturulur. Bir iş gönderilir ve bir Azure sanal ağında koyabilirsiniz işlem otomatik olarak ölçeklendirilebilir. İşlem, bir kapsayıcı ortamında yürütür ve model bağımlılıklarınızı Docker kapsayıcısında paketler.

Azure Machine Learning işlem eğitim işlemin CPU veya GPU işlem düğümü bulutta bir küme dağıtmak için kullanabilirsiniz. GPU'ları içeren VM boyutları hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Azure Machine Learning işlemi, ayrılan çekirdek sayısı gibi varsayılan sınırlara sahiptir. Daha fazla bilgi için [Azure kaynaklarını yönetin ve istek kotalarını](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

Bir farklı çalıştır zamanladığınızda isteğe bağlı veya bir kalıcı kaynak olarak bir Azure Machine Learning işlem ortamı oluşturabilirsiniz.

### <a name="run-based-creation"></a>Çalıştırma tabanlı oluşturma

Çalışma zamanında işlem hedefi bir Azure Machine Learning işlem ortamı oluşturabilirsiniz. İşlem çalıştırma için otomatik olarak oluşturulur ve büyük sayıda **max_nodes** çalıştırma, yapılandırmada belirttiğiniz. İşlem, çalıştırma tamamlandıktan sonra otomatik olarak silinir.

> [!IMPORTANT]
> Bir Azure Machine Learning işlem ortamını çalıştırma tabanlı oluşturulması şu anda Önizleme aşamasındadır. Otomatik hiper parametre ayarı kullanın ya da machine learning otomatik çalıştırma tabanlı olarak oluşturulmasını kullanmayın. Bir farklı çalıştır göndermeden önce hiper parametre ayarı veya otomatik makine öğrenimi kullanmak için Azure Machine Learning işlem ortamını oluşturun.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

# First, list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# Create a new runconfig object
run_config = RunConfiguration()

# Signal that you want to use AmlCompute to execute the script
run_config.target = "amlcompute"

# AmlCompute is created in the same region as your workspace
# Set the VM size for AmlCompute from the list of supported_vmsizes
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Kalıcı işlem: Temel

Kalıcı bir Azure Machine Learning işlem ortamını iş arasında yeniden kullanılabilir. İşlem, çalışma alanındaki diğer kullanıcılarla paylaşılabilir ve işleri arasında tutulur.

Kalıcı bir Azure Machine Learning işlem ortamını kaynak oluşturmak için belirttiğiniz **vm_size** ve **max_nodes** özellikleri. Azure Machine Learning, daha sonra diğer özellikler için akıllı Varsayılanları kullanır. Kullanılmayan ve oluşturduğunda sıfır düğümleri aşağı işlem daralttığında Vm'leri gerektiğinde işlerinizi çalıştırmak için ayrılmış. 

* **vm_size**: Azure Machine Learning işlem tarafından oluşturulan düğümler VM ailesi.
* **max_nodes**: Otomatik ölçeklendirme, Azure Machine Learning işlem iş çalıştırıldığında en fazla düğüm sayısı.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Kalıcı işlem: Gelişmiş

Bir Azure Machine Learning işlem ortamını oluştururken, bazı gelişmiş özellikler yapılandırabilirsiniz. Özellikleri aboneliğinizde sabit boyutlu ya da mevcut bir Azure sanal ağı içinde kalıcı bir küme oluşturmanıza imkan tanır.

İle birlikte **vm_size** ve **max_nodes** özellikleri, aşağıdaki özellikleri de kullanabilirsiniz:

* **min_nodes**: İşlem ortamı için bir Azure Machine Learning'de bir işi çalıştırdığınızda aşağı ölçeklemenizi için düğüm sayısı alt sınırı. En az sıfır (0) düğümleri varsayılandır.
* **vm_priority**: Bir Azure Machine Learning işlem ortamını kaynak oluştururken kullanmak üzere VM türü. Arasında seçim **adanmış** (varsayılan) ve **lowpriority**. Düşük öncelikli VM'ler, Azure'da aşırı kapasitesini kullanın. Bu VM'ler ucuz, ancak bu sanal makineler kullanıldığında, çalıştırmalarını pre-empted.
* **idle_seconds_before_scaledown**: Bir çalıştırma tamamlandıktan sonra beklenecek boşta kalma süresi ve otomatik ölçeklendirme sayısı kadar önce miktarı **min_nodes**. Varsayılan boşta kalma süresi, 120 saniyedir.
* **vnet_resourcegroup_name**: Kaynak grubunu __mevcut__ sanal ağ. Azure Machine Learning işlem ortamını, bu sanal ağ içinde oluşturulur.
* **vnet_name**: Sanal ağ adı. Sanal ağ, Azure Machine Learning çalışma alanı ile aynı bölgede olması gerekir.
* **subnet_name**: Sanal ağ içindeki alt ağ adı. Azure Machine Learning işlem ortamını kaynaklarını bu alt ağı aralığından IP adresleri atanır.

> [!TIP]
> Kalıcı bir Azure Machine Learning işlem ortamını kaynağı oluşturduğunuzda, sayısı gibi özellikleri güncelleştirebilirsiniz **min_nodes** veya **max_nodes**. Özellik Güncelleştirme için çağrı `update()` özelliği için işlevi.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist 
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

Azure Machine Learning, ayrıca kendi işlem kaynağı getiren ve çalışma alanınıza eklenmesini destekler. Azure Machine Learning hizmetini erişilebilir olduğu sürece bir kaynak türü bir rasgele uzak VM ' dir. Kaynak, bir Azure VM, kuruluş veya şirket içi uzak bir sunucuda olabilir. Özellikle, verilen IP adresini ve kimlik bilgilerini (kullanıcı adı ve parola veya SSH anahtarı), tüm erişilebilir VM'ler uzaktan çalıştırmalar için kullanabilirsiniz.
Bir Docker kapsayıcısı, bir Python ortamı veya sistem tarafından oluşturulan conda ortamda kullanabilirsiniz. Bir Docker kapsayıcısı kullanarak yürüttüğünüzde, VM'de çalışan Docker altyapısının olması gerekir. Uzak VM işlevselliği, özellikle yerel makinenize daha esnek bir bulut tabanlı geliştirme ve deneme ortamı istediğinizde yararlıdır.

> [!TIP]
> DSVM, bu senaryo için tercih ettiğiniz Azure VM olarak kullanın. Bu bir vm'dir önceden yapılandırılmış bir veri bilimi ve yapay ZEKA geliştirme ortamında Azure VM araç ve çerçeve tam yaşam döngüsü makine öğrenimi geliştirme için seçkin bir seçenek sunar. Azure Machine Learning ile DSVM'sini kullanma hakkında daha fazla bilgi için bkz. [geliştirme ortamını yapılandırma](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler. Bir VM oluşturmak veya mevcut bir VM'yi seçin, Ubuntu kullanan bir VM seçmeniz gerekir.

Eğitim hedef olarak bir DSVM yapılandırmak için SDK'sı aşağıdaki adımları kullanın:

1. Mevcut bir sanal makine işlem hedefi olarak eklemek için sanal makine için tam etki alanı adı (FQDN), kullanıcı adı ve parola sağlamalısınız. Bu örnekte değiştirin \<fqdn > Genel VM'nin genel IP adresi veya FQDN ile. Değiştirin \<username > ve \<parola > SSH kullanıcı adı ve parolayla VM için.

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create the compute config
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")

    # If you use SSH instead of a password, use this code:
    #                                                  ssh_port=22,
    #                                                  username='<username>',
    #                                                  password=None,
    #                                                  private_key_file="path-to-file",
    #                                                  private_key_passphrase="passphrase")

    # Attach the compute target
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. DSVM işlem hedefi için bir yapılandırma oluşturun. Docker ve conda oluşturmak ve eğitim ortamı DSVM'nin yapılandırmak için kullanılır.

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load into memory the cpu-dsvm.runconfig file created in the previous attach operation
    run_config = RunConfiguration(framework = "python")

    # Set the compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use the CPU base image
    # To use GPU in DSVM, you must also use the GPU base Docker image "azureml.core.runconfig.DEFAULT_GPU_IMAGE"
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask the system to provision a new conda environment based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when they're used for the first time
    run_config.prepare_environment = True

    # Specify the CondaDependencies object
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks, Azure bulutta Apache Spark tabanlı bir ortam olan. Bir Azure Machine Learning işlem hattı modelleriyle eğittiğinizde ortam işlem hedefi kullanılabilir.

> [!IMPORTANT]
> Bir Azure Databricks işlem hedefi bir Machine Learning işlem hattında yalnızca kullanılabilir.
>
> Modelinizi eğitmek için kullanmadan önce bir Azure Databricks çalışma alanı oluşturmanız gerekir. Bu kaynak oluşturmak için bkz [Azure Databricks'te Spark işini çalıştırma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Azure Databricks işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

* __İşlem adı__: Bu işlem kaynağına atanacak ad.
* __Databricks çalışma alanı adı__: Azure Databricks çalışma alanı adı.
* __Erişim belirteci__: Azure Databricks için kimliğini doğrulamak için kullanılan erişim belirteci. Bir erişim belirteci oluşturmak için bkz: [kimlik doğrulaması](https://docs.azuredatabricks.net/api/latest/authentication.html).

Aşağıdaki kod, Azure Databricks işlem hedefi olarak eklemek gösterilmektedir:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create the attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics'i

Azure Data Lake Analytics, Azure bulutunda bir büyük veri analiz platformudur. Bir Azure Machine Learning işlem hattı modelleriyle eğittiğinizde platform işlem hedefi kullanılabilir.

> [!IMPORTANT]
> Bir Azure Data Lake Analytics işlem hedefi bir Machine Learning işlem hattında yalnızca kullanılabilir.
>
> Modelinizi eğitmek için kullanmadan önce bir Azure Data Lake Analytics hesabı oluşturmanız gerekir. Bu kaynak oluşturmak için bkz [Azure Data Lake Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Data Lake Analytics işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

* __İşlem adı__: Bu işlem kaynağına atanacak ad.
* __Kaynak grubu__: Data Lake Analytics hesabını içeren kaynak grubu.
* __Hesap adı__: Data Lake Analytics hesap adı.

Aşağıdaki kod, Data Lake Analytics işlem hedefi olarak eklemek gösterilmektedir:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    
    # Create the attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach the ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning işlem hatları yalnızca Data Lake Analytics hesabı varsayılan veri deposunda depolanan verileri ile çalışma. Varsayılan olmayan deposunda ihtiyacınız olan verileri ise kullanabileceğiniz bir [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) modeli eğitme önce veri kopyalama işlemi.

## <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight, büyük veri analizi için popüler bir platformdur. Apache Spark, modelinizi eğitmek için kullanılan platform sağlar.

> [!IMPORTANT]
> Modelinizi eğitmek için kullanmadan önce HDInsight kümesi oluşturmanız gerekir. HDInsight kümesinde bir Spark oluşturmak için bkz: [HDInsight Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Kümeyi oluşturduğunuzda, bir SSH kullanıcı adı ve parola belirtmeniz gerekir. Bir işlem hedefi olarak HDInsight'ı kullanmaya gerek duyduğunuzda, bu değerleri not alın.
>
> Küme oluşturulduktan sonra FQDN sahip \<clustername >. azurehdinsight.net burada \<clustername >, küme için sağlanan addır. Küme işlem hedefi kullanmak için FQDN adresini (veya kümesinin genel IP adresi) gerekir.

HDInsight işlem hedefi yapılandırmak için HDInsight kümesi için FQDN, kullanıcı adı ve parola sağlamalısınız. Aşağıdaki örnek, bir küme çalışma alanınıza eklemek için SDK'sını kullanır. Bu örnekte değiştirin \<fqdn > ortak FQDN kümenin veya genel IP adresi ile. Değiştirin \<username > ve \<parola > SSH kullanıcı adı ve parola küme için.

> [!NOTE]
> Kümeniz için FQDN bulmak için Azure portalına gidin ve HDInsight kümenizi seçin. Altında __genel bakış__, FQDN gördüğünüz __URL__ girişi. FQDN almak için https Kaldır:\// girişin başından önek.

![Azure portalında bir HDInsight kümesi için FQDN'yi alma](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attach an HDInsight cluster as a compute target
    attach_config = HDInsightCompute.attach_configuration(address = "fqdn-or-ipaddress",
                                                          ssh_port = 22,
                                                          username = "username",
                                                          password = None, #if using ssh key
                                                          private_key_file = "path-to-key-file",
                                                          private_key_phrase = "key-phrase")
    compute = ComputeTarget.attach(ws, "myhdi", attach_config)
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure the HDInsight run
# Load the runconfig object from the myhdi.runconfig file generated in the previous attach operation
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# Ask the system to prepare the conda environment automatically when it's used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-runs"></a>Eğitim çalıştırmalarının gönderin

Bir eğitim çalıştırma göndermek için iki yolu vardır:

* Bir eğitim çalıştırmak gönderme bir `ScriptRunConfig` nesne.
* Bir eğitim çalıştırmak gönderme bir `Pipeline` nesne.

> [!IMPORTANT]
> Azure Databricks ve Azure Datalake Analytics hedefleri yalnızca bir işlem hattında kullanılabilen işlem.
>
> Yerel işlem hedefi, bir işlem hattında kullanılamaz.

### <a name="scriptrunconfig-object"></a>ScriptRunConfig nesnesi

Bir eğitim göndermek için kod desenini çalıştırmasını `ScriptRunConfig` nesnedir aynı işlem hedefleri tüm türleri için:

1. Oluşturma bir `ScriptRunConfig` işlem hedefi için bir çalıştırma yapılandırma kullanarak nesne.
1. Çalıştırma gönderin.
1. Çalıştırmak için bekleyin.

Aşağıdaki örnek, sistem tarafından yönetilen yerel işlem hedefi daha önce oluşturduğunuz yapılandırmayı kullanır:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="pipeline-object"></a>İşlem hattı nesnesi

Bir eğitim göndermek için kod desenini çalıştırma ile bir `Pipeline` nesnedir aynı işlem hedefleri tüm türleri için:

1. Bir adım eklemek `Pipeline` işlem kaynak nesnesi.
1. Bir işlem hattını kullanarak gönderin.
1. Çalıştırmak için bekleyin.

Aşağıdaki örnek, daha önce oluşturduğunuz Azure Databricks işlem hedefini kullanır:

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

# List of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Machine learning işlem hatlarını hakkında daha fazla bilgi için bkz. [işlem hatları ve Azure Machine Learning](concept-ml-pipelines.md).

Örneğin bir işlem hattı kullanarak bir model eğitip göstermektedir Jupyter not defterleri bkz [ https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline ](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="access-computes-in-the-azure-portal"></a>Azure portalında erişim hesaplar

Azure portalında bir çalışma alanınız ile ilişkili olan işlem hedefleri erişebilirsiniz. 

### <a name="view-compute-targets"></a>İşlem hedefleri görüntüle

Çalışma alanınız için işlem hedefleri görmek için aşağıdaki adımları kullanın:

1. Gidin [Azure portalında](https://portal.azure.com) ve, çalışma alanını açın.
1. Altında __uygulamaları__seçin __işlem__.

    ![İşlem hedefleri görüntüle](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

İşlem hedeflerin listesini görüntülemek için önceki adımları izleyin. Ardından işlem hedefi oluşturmak için aşağıdaki adımları kullanın:

1. İşlem hedefi eklemek için artı işaretini (+) seçin.

    ![İşlem hedefi ekleyin](./media/how-to-set-up-training-targets/add-compute-target.png)

1. İşlem hedefi için bir ad girin.
1. Seçin **Machine Learning işlem** kullanılmak üzere işlem türü olarak __eğitim__.

    > [!IMPORTANT]
    > Eğitim için yönetilen işlem kaynağı olarak yalnızca bir Azure Machine Learning işlem ortamını oluşturabilirsiniz.

1. Formu doldurun. Gerekli özellikleri için değerleri sağlayın, özellikle **VM ailesi**ve **en fazla düğüme** hesaplamayı dönmesi için kullanılacak. 
1. __Oluştur__’u seçin.
1. İşlem hedef listeden seçerek oluşturma işleminin durumunu görüntüleyin:

    ![Oluşturma işlemi durumunu görüntülemek için bir işlem hedef seçin](./media/how-to-set-up-training-targets/View_list.png)

1. Ardından işlem hedef ayrıntılarına bakın:

    ![Bilgisayar hedef ayrıntıları görüntüleyin](./media/how-to-set-up-training-targets/compute-target-details.png)

Şimdi daha önce anlatıldığı gibi bilgisayarı hedeflere karşı çalıştırma gönderebilirsiniz.


### <a name="reuse-existing-compute-targets"></a>Var olan işlem hedefleri yeniden kullanma

İşlem hedeflerin listesini görüntülemek için daha önce açıklanan adımları izleyin. Ardından bir işlem hedefine yeniden kullanmak için aşağıdaki adımları kullanın:

1. İşlem hedefi eklemek için artı işaretini (+) seçin.
1. İşlem hedefi için bir ad girin.
1. İçin eklemek için işlem türünü seçin __eğitim__:

    > [!IMPORTANT]
    > Tüm işlem, türleri, Azure portalından eklenebilir.
    > Eğitim için şu anda eklenebilecek işlem türleri şunlardır:
    >
    > * Uzak VM
    > * Azure Databricks
    > * Azure Data Lake Analytics
    > * Azure HDInsight

1. Formu doldurun ve gerekli özellikleri için değerler sağlayın.

    > [!NOTE]
    > Microsoft, SSH anahtarları, parolalara göre daha güvenlidir kullanmanızı önerir. Parola deneme yanılma saldırılarına karşı savunmasızdır. SSH anahtarlarını, şifreleme imzalarını kullanır. Azure sanal makineler ile kullanmak için SSH anahtarları oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:
    >
    > * [Oluşturma ve Linux veya Macos'ta SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Oluşturma ve Windows üzerinde SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Seçin __ekleme__.
1. İşlem hedef listeden seçerek iliştirme işlemi durumunu görüntüleyin.

Gönderebilmek için artık bu karşı çalıştırma işlem hedeflerini daha önce açıklandığı gibi.

## <a name="notebook-examples"></a>Not Defteri örnekleri

Not Defterleri şu konumlarda örnekler için bkz:

* [Yardım-How-to-kullanın-azureml/eğitimi](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning SDK başvurusu](https://aka.ms/aml-sdk)
* [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md)
* [Modelleri dağıtılacağı yeri](how-to-deploy-and-where.md)
* [Machine learning işlem hatlarını Azure Machine Learning hizmeti ile derleme](concept-ml-pipelines.md)
