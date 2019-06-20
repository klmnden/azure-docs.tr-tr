---
title: Özel Docker görüntüsü kullanarak bir model dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti Modellerinizi dağıtırken özel Docker görüntüsü kullanmayı öğrenin. Eğitilen bir modelin dağıtım yaparken, ana görüntü, web sunucusu ve hizmeti çalıştırmak için gereken diğer bileşenleri için bir Docker görüntüsü oluşturulur. Azure Machine Learning hizmeti için varsayılan bir görüntü sağlarken kendi görüntünüzü de kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/05/2019
ms.openlocfilehash: bd0e8099be5422d561541aeb8911c9a1610befcb
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272772"
---
# <a name="deploy-a-model-using-a-custom-docker-image"></a>Özel Docker görüntüsü kullanarak model dağıtma

Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtırken özel Docker görüntüsü kullanmayı öğrenin.

Eğitilen bir modelin bir web hizmeti veya IOT Edge cihazına dağıtırken bir Docker görüntüsü oluşturulur. Bu görüntü, model, conda ortam ve modelini kullanmak için gerekli varlıkları içerir. Ayrıca, bir web hizmeti ve Azure IOT Hub ile çalışmak için gereken bileşenleri olarak dağıtıldığında gelen istekleri işlemek için bir web sunucusu içerir.

Azure Machine Learning hizmeti varsayılan bir Docker görüntüsü sağlar. böylece oluşturma hakkında endişelenmeniz gerekmez. Olarak oluşturduğunuz özel bir görüntü kullanabilirsiniz bir _temel görüntü_. Bir dağıtım için bir görüntü oluşturulduğunda, temel görüntü başlangıç noktası olarak kullanılır. Bu, temel işletim sistemi ve bileşenleri sağlar. Dağıtım işlemi modeliniz, conda ortam ve diğer varlıklar gibi ek bileşenler dağıtmadan önce resim ardından ekler.

Genellikle, bileşen sürümleri denetlemek veya dağıtım sırasında zamandan istediğinizde bir özel görüntü oluşturun. Örneğin, Python, Conda veya başka bir bileşen belirli bir sürüme standart hale getirmek isteyebilirsiniz. Burada yükleme işlemi uzun süren modelinizi tarafından gerekli yazılımları yüklemek isteyebilirsiniz. Temel görüntü oluşturulurken yazılım yükleme, her dağıtım için yüklemek gerekmediği anlamına gelir.

> [!IMPORTANT]
> Model dağıtımı, web sunucusu gibi temel bileşenleri veya IOT Edge bileşenleri geçersiz kılamaz. Bu bileşenler, test ve Microsoft tarafından desteklenen bilinen bir çalışma ortamı sağlar.

> [!WARNING]
> Microsoft tarafından bir özel görüntü neden olduğu sorunları gidermeye yardımcı olmak mümkün olmayabilir. Sorunlarla karşılaşırsanız, varsayılan görüntü veya görüntüye belirli sorun olup olmadığını görmek için Microsoft sağlar görüntülerden birini kullanmak için istenebilir.

Bu belge, iki bölüme ayrılır:

* Özel bir görüntü oluşturun: Özel görüntü oluşturma ve Machine Learning CLI ve Azure CLI kullanarak bir Azure Container Registry kimlik doğrulaması yapılandırma bilgilerini yöneticileri ve DevOps sağlar.
* Özel görüntü kullanma: Python SDK'sı veya ML CLI eğitilen bir modelin dağıtırken özel görüntüleri kullanma hakkında bilgi veri Bilimcilerine ve DevOps/MLOps sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning hizmeti çalışma. Daha fazla bilgi için [çalışma alanı oluşturma](setup-create-workspace.md) makalesi.
* Azure Machine SDK Learning. Daha fazla bilgi için Python SDK'sı bölümüne bakın. [çalışma alanı oluşturma](setup-create-workspace.md#sdk) makalesi.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [CLI uzantısını Azure Machine Learning için](reference-azure-machine-learning-cli.md).
* Bir [Azure Container Registry](/azure/container-registry) veya internet üzerinden erişilebilir diğer Docker kayıt defteri.
* Bu belgedeki adımlarda oluşturma ve kullanma ile ilgili bilgi sahibi olduğunuz varsayılır bir __çıkarımı yapılandırma__ nesne modeli dağıtımının bir parçası olarak. Daha fazla bilgi için "dağıtmaya hazırlanma" bölümüne bakın. [dağıtılacağı yeri ve nasıl](how-to-deploy-and-where.md#prepare-to-deploy).

## <a name="create-a-custom-image"></a>Özel görüntü oluşturma

Bu bölümdeki bilgiler, Docker görüntülerini depolamak için bir Azure Container Registry kullandığınızı varsayar. Azure Machine Learning hizmeti için özel görüntüleri oluşturmak için planlarken aşağıdaki denetim listesini kullanın:

* Azure Machine Learning hizmeti çalışma alanında veya tek başına bir Azure Container Registry için oluşturulmuş Azure Container Registry kullanacak mısınız?

    Görüntüleri kullanarak, depolanan __çalışma alanı için kapsayıcı kayıt defteri__, kayıt defterinde kimlik doğrulaması gerekmez. Kimlik doğrulaması, çalışma alanı tarafından işlenir.

    > [!TIP]
    > Çalışma alanınız için kapsayıcı kayıt defteri denenmesi veya çalışma alanını kullanarak model dağıtma ilk kez oluşturulur. Yeni bir çalışma alanı oluşturduğunuz ancak değil eğitilmiş ve bir model oluşturdunuz, hiçbir Azure Container Registry için çalışma alanı bulunur.

    Çalışma alanınız için Azure Container Registry adı alma hakkında daha fazla bilgi için bkz: [Get kapsayıcı kayıt defteri adı](#getname) bu makalenin.

    Görüntüleri kullanarak, depolanan bir __tek başına kapsayıcı kayıt defteri__, en azından okuma erişimi bir hizmet sorumlusu yapılandırmanız gerekecektir. Ardından kayıt defterinden görüntüleri kullanan herkes için hizmet sorumlusu kimliği (kullanıcı adı) ve parola'ni sağlayın. Kapsayıcı kayıt defteri genel olarak erişilebilir yaptığınız varsa istisnadır.

    Özel bir Azure Container Registry oluşturma hakkında daha fazla bilgi için bkz. [özel kapsayıcı kayıt defteri oluşturma](/azure/container-registry/container-registery-get-started-azure-cli).

    Azure Container Registry ile hizmet sorumlularını kullanma hakkında daha fazla bilgi için bkz: [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](/azure/container-registry/container-registry-auth-service-principal).

* Azure Container Registry ve görüntü bilgileri: Görüntü adı kullanmak için gereken herkes için sağlar. Örneğin, adlı bir görüntü `myimage`adlı bir kayıt defterinde saklanan `myregistry`, başvurulan `myregistry.azurecr.io/myimage` görüntü için model dağıtımı kullanırken

* Resim gereksinimleri: Azure Machine Learning hizmeti yalnızca aşağıdaki yazılım sağlayan Docker görüntüleri destekler:

    * Ubuntu 16.04 veya büyük.
    * Conda 4.5. # veya büyük.
    * Python 3.5. # veya 3.6. #.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Kapsayıcı kayıt defteri bilgilerini alma

Bu bölümde, Azure Machine Learning hizmeti çalışma alanınız için Azure Container Registry adı alma konusunda bilgi edinin.

> [!TIP]
> Çalışma alanınız için kapsayıcı kayıt defteri denenmesi veya çalışma alanını kullanarak model dağıtma ilk kez oluşturulur. Yeni bir çalışma alanı oluşturduğunuz ancak değil eğitilmiş ve bir model oluşturdunuz, hiçbir Azure Container Registry için çalışma alanı bulunur.

Zaten eğitim almış veya dağıtılan modeller Azure Machine Learning hizmetini kullanarak, çalışma alanınız için bir kapsayıcı kayıt defteri oluşturuldu. Bu kapsayıcı kayıt defteri adını bulmak için aşağıdaki adımları kullanın:

1. Yeni kabuğu veya komut istemi açın ve Azure aboneliğinize kimliğini doğrulamak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az login
    ```

    Abonelik kimliğini doğrulamak için yönergeleri izleyin.

2. Çalışma alanı için kapsayıcı kayıt defteri listelemek için aşağıdaki komutu kullanın. Değiştirin `<myworkspace>` , Azure Machine Learning hizmet çalışma alanı adına sahip. Değiştirin `<resourcegroup>` çalışma alanınızı içeren Azure kaynak grubu ile:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    Döndürülen bilgileri aşağıdaki metne benzer:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    `<registry_name>` Çalışma alanınız için Azure Container Registry adı bir değerdir.

### <a name="build-a-custom-image"></a>Özel bir görüntü oluşturun

Bu bölümü gözden geçirme, Azure Container Registry'de özel bir Docker görüntüsü oluşturma adımları.

1. Adlı yeni bir metin dosyası oluşturun `Dockerfile`ve aşağıdaki metin içeriği kullanın:

    ```text
    FROM ubuntu:16.04

    ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
    ENV PATH /opt/miniconda/bin:$PATH

    RUN apt-get update --fix-missing && \
        apt-get install -y wget bzip2 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

    RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-4.5.12-Linux-x86_64.sh -O ~/miniconda.sh && \
        /bin/bash ~/miniconda.sh -b -p /opt/miniconda && \
        rm ~/miniconda.sh && \
        /opt/miniconda/bin/conda clean -tipsy

    RUN conda install -y python=3.6 && \
        conda clean -aqy && \
        rm -rf /opt/miniconda/pkgs && \
        find / -type d -name __pycache__ -prune -exec rm -rf {} \;
    ```

2. Bir kabuk veya komut istemi, Azure Container Registry'ye kimliğini doğrulamak için aşağıdakileri kullanın. Değiştirin `<registry_name>` görüntüyü depolamak istediğiniz kapsayıcı kayıt defteri adı:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Dockerfile karşıya yükleme ve bunları oluşturmak için aşağıdaki komutu kullanın. Değiştirin `<registry_name>` görüntüyü depolamak istediğiniz kapsayıcı kayıt defteri adı:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    Derleme işlemi sırasında komut satırına yedeklemek için bilgi akışla aktarılır. Derleme başarılı olursa aşağıdaki metne benzer bir ileti alırsınız:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Bir Azure Container Registry ile görüntü derleme hakkında daha fazla bilgi için bkz. [oluşturup Azure Container kayıt defteri görevleri kullanarak bir kapsayıcı görüntüsünü çalıştırın](/docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli.md)

Bir Azure Container Registry'ye mevcut görüntü karşıya daha fazla bilgi için bkz: [özel bir Docker kapsayıcı kayıt defterine ilk görüntünüzü itme](/azure/container-registry/container-registry-get-started-docker-cli.md).

## <a name="use-a-custom-image"></a>Özel görüntü kullanma

Özel görüntü kullanmak için aşağıdaki bilgiler gereklidir:

* __Görüntü adı__. Örneğin, `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` Microsoft tarafından sağlanan temel bir Docker görüntüsü yoludur.
* Görüntü ise bir __özel depoya__, aşağıdaki bilgiler gereklidir:

    * Kayıt defteri __adresi__. Örneğin, `myregistry.azureecr.io`.
    * Bir hizmet sorumlusu __kullanıcıadı__ ve __parola__ kayıt defterine okuma erişimine sahip.

    Bu bilgiler yoksa yöneticinin içeren görüntünüzü Azure Container Registry için konuşun.

### <a name="publicly-available-images"></a>Genel kullanıma açık görüntüleri

Microsoft, bu bölümdeki adımları kullanılabilir bir genel olarak erişilebilir deposundaki birkaç docker görüntüleri sağlar:

| Image | Açıklama |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Azure Machine Learning hizmeti için temel görüntü |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0` | ONNX çalışma zamanı içerir. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7` | ONNX çalışma zamanı ve CUDA bileşenleri içerir. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03` | ONNX çalışma zamanı ve TensorRT içerir. |

> [!TIP]
> Bu görüntüler genel kullanıma açık olduğundan, bunları kullanırken bir adresi, kullanıcı adı veya parola sağlamanız gerekmez.

> [!IMPORTANT]
> CUDA veya TensorRT kullanan Microsoft görüntüleri yalnızca Microsoft Azure hizmetleri üzerinde kullanılmalıdır.

> [!TIP]
>__Modelinizin Azure Machine Learning işlem eğitildi__kullanarak __1.0.22 sürümü veya üzeri__ Azure Machine Learning SDK'sının eğitim sırasında bir görüntü oluşturulur. Bu görüntü adını bulmak için kullanmak `run.properties["AzureML.DerivedImageName"]`. Aşağıdaki örnek, bu görüntünün nasıl kullanılacağını gösterir:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Görüntü Azure Machine Learning SDK ile birlikte kullanın.

Özel görüntü kullanmak için ayarlanmış `base_image` özelliği [çıkarımı yapılandırma nesnesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) görüntü adresine:

```python
# use an image from a registry named 'myregistry'
inference_config.base_image = "myregistry.azurecr.io/myimage:v1"
```

Bu biçim, genel olarak erişilebilir olan, çalışma alanı ve kapsayıcı kayıt defterleri için Azure Container Registry'de depolanan her iki görüntüleri için çalışır. Örneğin, aşağıdaki kod, Microsoft tarafından sağlanan varsayılan bir görüntü kullanır:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Bir görüntüden kullanmak için bir __özel kapsayıcı kayıt defteri__ olmayan çalışma alanınızda, deponun ve bir kullanıcı adı ve parola adresini belirtmeniz gerekir:

```python
# Use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

### <a name="use-an-image-with-the-machine-learning-cli"></a>Machine Learning CLI ile bir görüntü kullanma

> [!IMPORTANT]
> Şu anda Machine Learning CLI görüntülerini Azure Container Registry'den çalışma alanı veya genel olarak erişilebilir depoları için kullanabilirsiniz. Tek başına özel defterlerinden görüntüleri kullanamazsınız.

Machine Learning CLI kullanarak bir model dağıtımına özel görüntüyü başvuran bir çıkarımı yapılandırma dosyası belirtin. Aşağıdaki JSON belgesini görüntüyü public kapsayıcı kayıt defterindeki nasıl başvurulacağını gösterir:

```json
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "infenv.yml",
   "extraDockerfileSteps": null,
   "sourceDirectory": null,
   "enableGpu": false,
   "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda",
   "baseImageRegistry": "mcr.microsoft.com"
}
```

Bu dosya ile kullanılır `az ml model deploy` komutu. `--ic` Parametresi çıkarımı yapılandırma dosyasını belirtmek için kullanılır.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

"Model kaydı, profil oluşturma ve dağıtımını" bölümünü ML CLI'yı kullanarak bir model dağıtımına ilişkin daha fazla bilgi için bkz. [CLI uzantısını Azure Machine Learning hizmeti için](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [dağıtılacağı yeri ve nasıl](how-to-deploy-and-where.md).
* Bilgi nasıl [eğitme ve Azure işlem hatları kullanarak makine öğrenimi modelleri dağıtma](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops).