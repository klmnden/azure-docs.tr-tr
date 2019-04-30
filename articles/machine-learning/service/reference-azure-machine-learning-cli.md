---
title: Machine learning CLI uzantısı
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning CLI uzantı hakkında Azure CLI için öğrenin. Azure CLI'yı Azure buluttaki kaynaklar ile çalışmanıza olanak sağlayan bir platformlar arası komut satırı yardımcı programıdır. Machine Learning uzantısı, Azure Machine Learning hizmeti ile çalışmanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 2504ca9cb785529a9eab321c2521db46390632b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60752998"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için CLI uzantısını kullanma

Azure Machine Learning CLI uzantısıdır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), Azure platformu için platformlar arası komut satırı arabirimi. Bu uzantı, komut satırından Azure Machine Learning hizmeti ile çalışmaya yönelik komutları sağlar. Makine öğrenimi iş akışlarını otomatikleştiren betikler oluşturmak sağlar. Örneğin, aşağıdaki eylemleri gerçekleştirin betikleri oluşturabilirsiniz:

+ Makine öğrenimi modelleri oluşturma çalıştırın

+ Makine öğrenimi modelleri müşteri kullanım için kaydolun

+ Paketlemeyi, dağıtmayı ve makine öğrenimi Modellerinizi ömrünü izleme

CLI, Azure Machine Learning SDK'sı yerine değil. Yüksek oranda parametreli görevler gibi işlemek için optimize edilmiştir tamamlayıcı bir araçtır:

* İşlem kaynakları oluşturma

* Parametreli deneme gönderme

* Model kaydı

* Görüntü oluşturma

* Hizmet dağıtımı

## <a name="prerequisites"></a>Önkoşullar


* CLI kullanmak için Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Uzantıyı yükleme

Machine Learning CLI uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-1.0.10-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Sorulduğunda, `y` uzantıyı yüklemek için.

Uzantı yüklü olduğunu doğrulamak için ML özgü alt komutları listesini görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Yapmanız gerekenler uzantıyı güncelleştirmek için __kaldırmak__ ve ardından __yüklemek__ bu. Bu, en son sürümünü yükler.

## <a name="remove-the-extension"></a>Uzantıyı kaldırın

CLI uzantısını kaldırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Kaynak yönetimi

Aşağıdaki komutları, Azure Machine Learning tarafından kullanılan kaynakları yönetmek için CLI'yı kullanmayı göstermektedir.


+ Bir Azure Machine Learning hizmeti çalışma alanında oluşturun:

    ```azurecli-interactive
    az ml workspace create -n myworkspace -g myresourcegroup
    ```

+ Varsayılan çalışma alanı ayarlayın:

    ```azurecli-interactive
    az configure --defaults aml_workspace=myworkspace group=myresourcegroup
    ```
    
+ Bir AKS kümesi ekleme

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a name="experiments"></a>Denemeler

Aşağıdaki komutları denemeleri ile çalışmak için CLI kullanma işlemini gösterir:

* Bir deney göndermeden önce (yapılandırmayı Çalıştır) bir proje ekleyin:

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Çalıştırma denemenizi başlatın. Bu komutu kullanırken çalıştırma yapılandırmasını içeren runconfig dosyasının adını belirtin. İşlem hedefi çalıştırma yapılandırma modeli için eğitim ortamı oluşturmak için kullanır. Bu örnekte, çalıştırma yapılandırma öğesinden yüklenen `./aml_config/myrunconfig.runconfig` dosya.

    ```azurecli-interactive
    az ml run submit -c myrunconfig train.py
    ```

    Runconfig dosya hakkında daha fazla bilgi için bkz. [RunConfig](#runconfig) bölümü.

* Gönderilen denemeleri listesini görüntüleyin:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Model kaydı, görüntü oluşturma ve dağıtım

Aşağıdaki komutlar, eğitilen bir modeli kaydedin ve ardından bunu bir üretim hizmeti dağıtacağız göstermektedir:

+ Bir modeli Azure Machine Learning ile kaydedin:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Makine öğrenimi modeli ve bağımlılıklar içeren bir görüntü oluşturun: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Bir resim bir işlem hedefine dağıtın:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```

## <a id="runconfig"></a> Runconfig dosyası

Bir çalıştırma yapılandırma modelinizi eğitmek için kullanılan eğitim ortamı yapılandırmak için kullanılır. Bu yapılandırma, bellek içi SDK'sını kullanarak oluşturulabilir veya runconfig dosyasından yüklenebilir.

Eğitim ortamı için yapılandırma açıklayan bir metin belgesi runconfig dosyasıdır. Örneğin, eğitim betiğini ve modeli eğitmek için gereken conda bağımlılıklarını içeren dosyanın adını listeler.

Azure Machine Learning CLI iki varsayılan oluşturur `.runconfig` dosyalardaki `docker.runconfig` ve `local.runconfig` kullanarak bir proje bağladığınızda `az ml project attach` komutu. 

Kullanarak bir çalıştırma yapılandırma oluşturan kodu varsa [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) kullanabileceğiniz sınıfı `save()` yöntemi için kalıcı hale getirmek için bir `.runconfig` dosya.

Aşağıdaki içeriği örneğidir bir `.runconfig` dosyası:

```text
# The script to run.
script: train.py
# The arguments to the script file.
arguments: []
# The name of the compute target to use for this run.
target: local
# Framework to execute inside. Allowed values are "Python" ,  "PySpark", "CNTK",  "TensorFlow", and "PyTorch".
framework: PySpark
# Communicator for the given framework. Allowed values are "None" ,  "ParameterServer", "OpenMpi", and "IntelMpi".
communicator: None
# Automatically prepare the run environment as part of the run itself.
autoPrepareEnvironment: true
# Maximum allowed duration for the run.
maxRunDurationSeconds:
# Number of nodes to use for running job.
nodeCount: 1
# Environment details.
environment:
# Environment variables set for the run.
  environmentVariables:
    EXAMPLE_ENV_VAR: EXAMPLE_VALUE
# Python details
  python:
# user_managed_dependencies=True indicates that the environmentwill be user managed. False indicates that AzureML willmanage the user environment.
    userManagedDependencies: false
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
  docker:
# Set True to perform this run inside a Docker container.
    enabled: true
# Base image used for Docker-based runs.
    baseImage: mcr.microsoft.com/azureml/base:0.2.4
# Set False if necessary to work around shared volume bugs.
    sharedVolumes: true
# Run with NVidia Docker extension to support GPUs.
    gpuSupport: false
# Extra arguments to the Docker run command.
    arguments: []
# Image registry that contains the base image.
    baseImageRegistry:
# DNS name or IP address of azure container registry(ACR)
      address:
# The username for ACR
      username:
# The password for ACR
      password:
# Spark details
  spark:
# List of spark repositories.
    repositories:
    - https://mmlspark.azureedge.net/maven
    packages:
    - group: com.microsoft.ml.spark
      artifact: mmlspark_2.11
      version: '0.12'
    precachePackages: true
# Databricks details
  databricks:
# List of maven libraries.
    mavenLibraries: []
# List of PyPi libraries
    pypiLibraries: []
# List of RCran libraries
    rcranLibraries: []
# List of JAR libraries
    jarLibraries: []
# List of Egg libraries
    eggLibraries: []
# History details.
history:
# Enable history tracking -- this allows status, logs, metrics, and outputs
# to be collected for a run.
  outputCollection: true
# whether to take snapshots for history.
  snapshotProject: true
# Spark configuration details.
spark:
  configuration:
    spark.app.name: Azure ML Experiment
    spark.yarn.maxAppAttempts: 1
# HDI details.
hdi:
# Yarn deploy mode. Options are cluster and client.
  yarnDeployMode: cluster
# Tensorflow details.
tensorflow:
# The number of worker tasks.
  workerCount: 1
# The number of parameter server tasks.
  parameterServerCount: 1
# Mpi details.
mpi:
# When using MPI, number of processes per node.
  processCountPerNode: 1
# data reference configuration details
dataReferences: {}
# Project share datastore reference.
sourceDirectoryDataStore:
# AmlCompute details.
amlcompute:
# VM size of the Cluster to be created.Allowed values are Azure vm sizes. The list of vm sizes is available in 'https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs
  vmSize:
# VM priority of the Cluster to be created.Allowed values are "dedicated" , "lowpriority".
  vmPriority:
# A bool that indicates if the cluster has to be retained after job completion.
  retainCluster: false
# Name of the cluster to be created. If not specified, runId will be used as cluster name.
  name:
# Maximum number of nodes in the AmlCompute cluster to be created. Minimum number of nodes will always be set to 0.
  clusterMaxNodeCount: 1
```
