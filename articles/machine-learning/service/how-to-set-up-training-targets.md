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
ms.date: 09/24/2018
ms.openlocfilehash: 84cba0cb156e1d847c92596a9f2f6017a429b9d2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49113827"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Seçme ve modelinizi eğitmek için işlem hedefi kullanma

Azure Machine Learning hizmeti ile birçok farklı ortamlarda modelinizi eğitebilirsiniz. Olarak adlandırılan, bu ortamların __hedefleri işlem__, yerel veya Bulut üzerinde olabilir. Bu belgede, desteklenen işlem hedefleri ve bunların nasıl kullanıldığı hakkında bilgi edineceksiniz.

Bir işlem hedefine dağıtıldığında, eğitim betiği veya Konaklara modelinizi bir web hizmeti olarak çalışan bir kaynaktır. Bunlar oluşturulabilir ve Azure Machine Learning SDK'sı veya CLI kullanılarak yönetilebilir. Başka bir işlem tarafından (örneğin, Azure portal veya Azure CLI) oluşturulan işlem hedefleri varsa, Azure Machine Learning hizmeti çalışma alanınıza ekleyerek kullanabilirsiniz.

Makinenizde yerel çalışmalar başlayın ve ardından GPU veya Azure Batch AI ile uzak veri bilimi sanal makineler gibi diğer ortamlara ve ölçeklendirin. 

## <a name="supported-compute-targets"></a>Desteklenen işlem hedefleri

Azure Machine Learning hizmeti, aşağıdaki işlem hedeflerini destekler:

|Hedef işlem| GPU hızlandırma | Otomatik hiper parametre ayarı | Otomatik model seçimi | İşlem hatlarında kullanılabilir|
|----|:----:|:----:|:----:|:----:|
|[Yerel bilgisayar](#local)| Belki de | &nbsp; | ✓ | &nbsp; |
|[Veri bilimi sanal makinesi (DSVM)](#dsvm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Batch AI](#batch)| ✓ | ✓ | ✓ | ✓ | ✓ |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

__[Azure Container Instances'a (ACI)](#aci)__  modelleri eğitmek için de kullanılabilir. Bu, Hesaplı ve kolayca oluşturmak ve bunlarla çalışmak sunucusuz bulut teklifi olur. ACI veya model seçimi otomatik GPU hızlandırma, otomatik hyper parametresi ayarlama, desteklemiyor. Ayrıca, bir işlem hattında kullanılamaz.

İşlem hedefler arasında önemli farkları vardır:
* __GPU hızlandırma__: GPU'ları Azure Batch AI ve veri bilimi sanal makinesi ile kullanılabilir. Donanım, sürücüler ve yüklenen çerçeveleri bağlı olarak, yerel bilgisayarınızda bir GPU erişimi olabilir.
* __Hiper parametre ayarı otomatik__: Azure Machine Learning, Hiper parametre otomatik iyileştirme modeliniz için en iyi hiperparametreleri bulmanıza yardımcı olur.
* __Model seçimi otomatik__: Azure Machine Learning hizmeti akıllı bir şekilde önerilir seçimi algoritması ve hiper parametre bir modeli oluştururken. Otomatik model seçimi farklı birleşimleri el ile çalışılırken daha hızlı, yüksek kaliteli modeline yakınsama yardımcı olur. Daha fazla bilgi için [Öğreticisi: otomatik olarak bir Azure Machine Learning otomatik sınıflandırma modeli eğitme](tutorial-auto-train-models.md) belge.
* __İşlem hatları__: Azure Machine Learning hizmeti, eğitim ve dağıtım işlem hattı içine gibi farklı görevler birleştirmenize olanak sağlar. İşlem hatları paralel veya sıralı olması ve güvenilir Otomasyon için bir mekanizma sağlar. Daha fazla bilgi için [machine learning işlem hatlarını Azure Machine Learning hizmeti ile derleme](concept-ml-pipelines.md) belge.

İşlem hedeflerini oluşturmak için Azure Machine Learning SDK'sı, Azure CLI veya Azure Portalı'nı kullanabilirsiniz. Var olan işlem hedefleri (iliştiriliyor) ekleyerek de kullanabilirsiniz çalışma alanınıza bunları.

> [!IMPORTANT]
> Var olan bir Azure kapsayıcı örneğini çalışma alanınıza eklenemiyor. Bunun yerine, yeni bir örneğini oluşturmanız gerekir.
>
> Bir çalışma alanı içinde bir Azure HDInsight kümesi oluşturulamıyor. Bunun yerine, mevcut bir kümeye eklemeniz gerekir.

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

Bir işlem hedefinden başka bir hesaba geçiyorum oluşturmanız gerekir bir [çalıştırma yapılandırmasını](concept-azure-machine-learning-architecture.md#run-configuration). Çalıştırma yapılandırma işlem hedef betiği çalıştırmak nasıl tanımlar.

## <a name="training-scripts"></a>Eğitim betikleriniz

Bir eğitim çalıştırması başlattığınızda, eğitim betikleriniz içeren tüm dizin gönderilir. Bir anlık görüntüsü oluşturulur ve işlem hedefine gönderildi. Daha fazla bilgi için [anlık görüntüleri](concept-azure-machine-learning-architecture.md#snapshot).

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

Jupyter eğitim kullanıcı tarafından yönetilen bir ortamda gösteren not defteri için bkz: [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).
  
### <a name="system-managed-environment"></a>Sistem tarafından yönetilen ortamı

Bağımlılıkları yönetmek için conda ortamları sistem yönetilen kullanır. Conda adlı bir dosya oluşturur `conda_dependencies.yml` bu bağımlılıkların bir listesini içerir. Ardından sistemin içinde komut dosyalarınızı çalıştırın ve yeni bir conda ortamı oluşturmak isteyebilirsiniz. Ortamları sistem yönetilen olabilir yeniden kullanılan daha sonra sürece `conda_dependencies.yml` dosya değişmeden kalır. 

Yeni bir ortam kurmak ilk kurulumu gerekli bağımlılıkları boyutuna bağlı olarak tamamlanması birkaç dakika sürebilir. Aşağıdaki kod parçacığı, sistem tarafından yönetilen bir ortam oluşturma, üzerinde scikit bağlıdır gösterir-öğrenin:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Jupyter eğitim sistem tarafından yönetilen bir ortamda gösteren not defteri için bkz: [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).

## <a id="dsvm"></a>Veri bilimi sanal makinesi

Yerel makinenize işlem veya GPU kaynakları modeli eğitmek için gerekli olmayabilir. Bu durumda, ölçeğini artırabilir veya ölçek genişletme ek ekleyerek Eğitim işlemini işlem hedeflerini bir veri bilimi sanal makineleri (DSVM) gibi.

> [!WARNING]
> Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler. Bir sanal makine oluştururken veya mevcut bir seçerek, Ubuntu kullanan bir seçtiğinizde gerekir.

Eğitim hedef olarak bir veri bilimi sanal makinesi (DSVM) yapılandırmak için SDK'sı aşağıdaki adımları kullanın:

1. Oluşturun veya bir sanal makine ekleme
    
    * Yeni DSVM oluşturma için önce aynı ada sahip bir DSVM sahip değilse, yeni bir VM oluşturmak görmek için kontrol edin:
    
        ```python
        from azureml.core.compute import DsvmCompute
        from azureml.core.compute_target import ComputeTargetException

        compute_target_name = 'mydsvm'

        try:
            dsvm_compute = DsvmCompute(workspace = ws, name = compute_target_name)
            print('found existing:', dsvm_compute.name)
        except ComputeTargetException:
            print('creating new.')
            dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
            dsvm_compute = DsvmCompute.create(ws, name = compute_target_name, provisioning_configuration = dsvm_config)
            dsvm_compute.wait_for_completion(show_output = True)
        ```
    * Mevcut bir sanal makine işlem hedefi olarak eklemek için sanal makine için tam etki alanı adı, oturum açma adı ve parola sağlamalısınız.  Bu örnekte değiştirin ```<fqdn>``` VM veya genel IP adresi genel tam etki alanı adı. Değiştirin ```<username>``` ve ```<password>``` SSH kullanıcı adı ve sanal makine için parola ile:

        ```python
        from azureml.core.compute import RemoteCompute

        dsvm_compute = RemoteCompute.attach(ws,
                                        name="attach-dsvm",
                                        username='<username>',
                                        address="<fqdn>",
                                        ssh_port=22,
                                        password="<password>")

        dsvm_compute.wait_for_completion(show_output=True)
    
   It takes around 5 minutes to create the DSVM instance.

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

    # Use CPU base image from DockerHub
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

1. İşiniz bittiğinde, işlem kaynaklarını silmek için aşağıdaki kodu kullanın:

    ```python
    dsvm_compute.delete()
    ```

Jupyter eğitim üzerinde veri bilimi sanal makinesi gösteren not defteri için bkz: [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb).

## <a id="batch"></a>Azure Batch AI

Modelinizi eğitmek için uzun sürerse, eğitim bulut bilgi işlem kaynaklarının bir küme dağıtmak için Azure Batch AI'ı kullanabilirsiniz. Batch AI, GPU kaynak etkinleştirmek için de yapılandırılabilir.

Aşağıdaki örnekte mevcut bir Batch AI kümesi adına göre arar. Biri bulunamazsa oluşturulur:

```python
from azureml.core.compute import BatchAiCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
batchai_cluster_name = os.environ.get("BATCHAI_CLUSTER_NAME", ws.name + "gpu")
cluster_min_nodes = os.environ.get("BATCHAI_CLUSTER_MIN_NODES", 1)
cluster_max_nodes = os.environ.get("BATCHAI_CLUSTER_MAX_NODES", 3)
vm_size = os.environ.get("BATCHAI_CLUSTER_SKU", "STANDARD_NC6")
autoscale_enabled = os.environ.get("BATCHAI_CLUSTER_AUTOSCALE_ENABLED", True)


if batchai_cluster_name in ws.compute_targets():
    compute_target = ws.compute_targets()[batchai_cluster_name]
    if compute_target and type(compute_target) is BatchAiCompute:
        print('found compute target. just use it. ' + batchai_cluster_name)
else:
    print('creating a new compute target...')
    provisioning_config = BatchAiCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                vm_priority = 'lowpriority', # optional
                                                                autoscale_enabled = autoscale_enabled,
                                                                cluster_min_nodes = cluster_min_nodes, 
                                                                cluster_max_nodes = cluster_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

İşlem hedefi olarak varolan bir Batch AI kümesi eklemek için Azure kaynak kimliğini sağlamanız gerekir Azure portalında kaynak Kimliğini almak için aşağıdaki adımları kullanın:
1. Arama `Batch AI` altında service **tüm hizmetleri**
1. Kümenizi ait olduğu çalışma alanının adına tıklayın
1. Kümeyi seçin
1. Tıklayarak **özellikleri**
1. Kopyalama **kimliği**

Aşağıdaki örnek, bir küme çalışma alanınıza eklemek için SDK'sını kullanır. Bu örnekte değiştirin `<name>` işlem için herhangi bir ada sahip. Adı küme adıyla eşleşmesi gerekmez. Değiştirin `<resource-id>` Azure kaynak kimliği ayrıntılı yukarıda:

```python
from azureml.core.compute import BatchAiCompute
BatchAiCompute.attach(workspace=ws,
                      name=<name>,
                      resource_id=<resource-id>)
```

Ayrıca, aşağıdaki Azure CLI komutları kullanarak Batch AI kümesi ve iş durumunu kontrol edebilirsiniz:

- Küme durumunu kontrol edin. Kaç düğümleri kullanarak çalıştırıyor gördüğünüz `az batchai cluster list`.
- İş durumunu kontrol edin. Kaç tane işin kullanılarak çalıştırdığınız gördüğünüz `az batchai job list`.

Batch AI kümesi oluşturmak için yaklaşık 5 dakika sürer.

Jupyter gösteren bir Batch AI kümesi Eğitim not defteri için bkz: [ https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb).

## <a name='aci'></a>Azure Container örneği (ACI)

Azure Container Instances hızlı başlangıç süreleri ve tüm sanal makineleri yönetmek kullanıcı gerektirmeyen yalıtılmış kapsayıcılardır. Azure Machine Learning hizmeti westus, eastus, westeurope, northeurope, westus2 ve southeastasia bölgelerinde kullanılabilir olan Linux kapsayıcıları kullanır. Daha fazla bilgi için [bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas#region-availability). 

Aşağıdaki örnek bir ACI işlem hedefi oluşturmak ve bir modeli eğitmek için kullanmak için SDK'sını kullanmayı gösterir: 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use ACI to run script.
run_config.target = "containerinstance"

# ACI container group is only supported in certain regions, which can be different than the region the Workspace is in.
run_config.container_instance.region = 'eastus'

# set the ACI CPU and Memory 
run_config.container_instance.cpu_cores = 1
run_config.container_instance.memory_gb = 2

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
#run_config.environment.docker.base_image = 'microsoft/mmlspark:plus-0.9.9'

# use conda_dependencies.yml to create a conda environment in the Docker image
run_config.environment.python.user_managed_dependencies = False

# auto-prepare the Docker image when used for the first time (if it is not already prepared)
run_config.auto_prepare_environment = True

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Bir ACI işlem hedefi oluşturmak için birkaç saniyeden birkaç dakika sürebilir.

Jupyter Azure Container Instance üzerinde eğitim gösteren not defteri için bkz: [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb).

## <a id="hdinsight"></a>Bir HDInsight kümesi ekleme 

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
    
Eğitim çalıştırması gönderildiği kodunu işlem hedef bağımsız olarak aynıdır:

* Oluşturma bir `ScriptRunConfig` işlem hedefi çalıştırma Yapılandırması'nı kullanarak nesne.
* Çalıştırma gönderin.
* Çalıştırmak için bekleyin.

Aşağıdaki örnek, bu belgede daha önce oluşturulan sistem tarafından yönetilen yerel işlem hedefi için yapılandırmayı kullanır:

```pyghon
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```

Jupyter HDInsight üzerinde Spark ile eğitim gösteren not defteri için bkz: [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Görüntüleyebilir ve Azure portalını kullanarak hesaplamayı ayarlayabilirsiniz

Hangi hedef Azure portalından, bir çalışma alanıyla ilişkili işlem görüntüleyebilirsiniz. Listeye almak için aşağıdaki adımları kullanın:

1. Ziyaret [Azure portalında](https://portal.azure.com) ve çalışma alanınıza gidin.
2. Tıklayarak __işlem__ altında bağlantı __uygulamaları__ bölümü.

    ![Görünüm işlem sekmesi](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

İşlem hedeflerin listesini görüntülemek için yukarıdaki adımları izleyin ve sonra da işlem hedefi oluşturmak için aşağıdaki adımları kullanın:

1. Tıklayın __+__ işlem hedefi eklemek oturum açın.

    ![İşlem ekle ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. İşlem hedefi için bir ad girin.
1. İçin eklemek için işlem türünü seçin __eğitim__. 
1. Seçin __Yeni Oluştur__ ve gerekli formu doldurun. 
1. __Oluştur__’u seçin
1. Durumunu görüntüleyebilirsiniz. işlem hedef listeden seçerek oluşturma işlemi.

    ![İşlem listesini görüntüle](./media/how-to-set-up-training-targets/View_list.png) ardından, işlem ayrıntılarını görürsünüz.
    ![Ayrıntıları görüntüle](./media/how-to-set-up-training-targets/vm_view.PNG)
1. Artık bu hedeflere yukarıdaki ayrıntılı olarak karşı çalıştırma gönderebilirsiniz.

### <a name="reuse-existing-compute-in-your-workspace"></a>Yeniden çalışma alanınızdaki mevcut işlem

İşlem hedeflerin listesini görüntülemek için yukarıdaki adımları izleyin ve ardından hedef işlem yeniden kullanmak için aşağıdaki adımları kullanın:

1. Tıklayın **+** işlem hedefi eklemek oturum açın.
2. İşlem hedefi için bir ad girin.
3. Eğitim için eklemek için işlem türünü seçin. Batch AI ve sanal makineler şu anda portalında eğitim için desteklenir.
4. 'Var olanı Kullan' seçin.
    - Batch AI kümeleri eklerken, açılan listeden işlem hedefini seçin, Batch AI Çalışma ve Batch AI kümesi seçin ve ardından **Oluştur**.
    - Bir sanal makineyi eklerken, IP adresi, kullanıcı adı/parola bileşimi, Private/Public anahtarları ve bağlantı noktasını girin ve Oluştur'a tıklayın.

    > [!NOTE]
    > Parolalara göre daha güvenli oldukları gibi Microsoft SSH anahtarları kullanmanızı önerir. SSH anahtarları şifreleme imzalarını kullanır ancak parola deneme yanılma saldırılarına karşı savunmasız. Azure sanal makineler ile kullanmak için SSH anahtarları oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:
    >
    > * [Oluşturma ve Linux veya Macos'ta SSH anahtarlarını kullanma]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Oluşturma ve Windows üzerinde SSH anahtarlarını kullanma]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

5. İşlem hedef hesaplar listesinden seçerek sağlama durumu durumunu görüntüleyebilirsiniz.
6. Artık bu hedeflere karşı çalıştırma gönderebilirsiniz.

## <a name="examples"></a>Örnekler
Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:
* [01.Getting-Started/02.Train-on-Local/02.Train-on-Local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.Getting-Started/04.Train-on-Remote-VM/04.Train-on-Remote-VM.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm)
* [01.Getting-Started/03.Train-on-aci/03.Train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci)
* [01.Getting-Started/05.Train-in-Spark/05.Train-in-Spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark)
* [Tutorials/01.Train-models.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb)

Bu not defterlerini alın: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning SDK başvurusu](http://aka.ms/aml-sdk)
* [Öğretici: Modeli eğitme](tutorial-train-models-with-aml.md)
* [Modelleri dağıtılacağı yeri](how-to-deploy-and-where.md)
* [Machine learning işlem hatlarını Azure Machine Learning hizmeti ile derleme](concept-ml-pipelines.md)
