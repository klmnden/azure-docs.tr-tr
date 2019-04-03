---
title: Çalışma alanı oluşturma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti çalışma alanınızı oluşturmak için Azure portalı, SDK, bir şablon veya CLI kullanın. Bu çalışma alanı, Azure Machine Learning hizmeti kullanırken oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerde sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sgilley
author: sdgilley
ms.date: 03/21/2019
ms.openlocfilehash: f417aef1fd1cc48a37399ff7a157a0e658bbbb02
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879293"
---
# <a name="create-an-azure-machine-learning-service-workspace"></a>Bir Azure Machine Learning hizmeti çalışma alanı oluşturma

Azure Machine Learning hizmeti kullanmak için gereken bir [ **Azure Machine Learning hizmeti çalışma alanında**](concept-azure-machine-learning-architecture.md#workspace).  Bu çalışma alanı hizmeti için en üst düzey bir kaynaktır ve oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerde sağlar. 

Bu makalede, bu yöntemlerden birini kullanarak bir çalışma alanı oluşturmayı öğrenin: 
* [Azure portalında](#portal) arabirimi
* [Azure makine için Python SDK'sı öğrenme](#sdk)
* Bir Azure Resource Manager şablonu
* [Azure Machine Learning CLI](#cli)

Burada içindeki adımları kullanarak oluşturduğunuz çalışma alanı, diğer öğreticileri ve nasıl yapılır makaleleri bir önkoşul olarak kullanılabilir. 

Bir çalışma alanı oluşturduğunuzda aşağıdaki Azure kaynakları otomatik olarak (Bölgesel kullanılabilir iseler) eklendi:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Storage](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

>[!Note]
>Diğer Azure hizmetlerinde olduğu gibi bazı limitler ve kotalar Machine Learning ile ilişkilendirilir. [Kotalar ve daha fazla isteği hakkında bilgi edinin.](how-to-manage-quotas.md)


## <a name="prerequisites"></a>Önkoşullar
Bir çalışma alanı oluşturmak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="portal"></a> Azure portalı

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Nasıl oluşturulduğu ne olursa olsun, çalışma alanınızda görüntüleyebilirsiniz [Azure portalında](https://portal.azure.com/).  Bkz: [bir çalışma alanını görüntülemek](how-to-manage-workspace.md#view) Ayrıntılar için.

## <a name="sdk"></a> Python SDK'sı

Python SDK'sını kullanarak çalışma alanınızı oluşturun. Öncelikle, SDK'yı yüklemeniz gerekir.

> [!IMPORTANT]
> Bir Azure veri bilimi sanal makinesi veya Azure Databricks kullanırsanız, SDK'ın yüklenmesini atlayın.
> * 27 Eylül 2018'den sonra oluşturulan Azure veri bilimi sanal makineleri, önceden yüklenmiş Python SDK ile birlikte gelir. Yüklemeyi atlayabilir ve başlayın [SDK'sı ile çalışma alanı oluşturma](#sdk-create).
> * Azure Databricks ortamda [Databricks yükleme adımlarını](how-to-configure-environment.md#azure-databricks) yerine.

>[!NOTE]
> Yerel bilgisayarınızdan SDK'yı yükleyip için bu yönergeleri kullanın. Jupyter uzak bir sanal makinede kullanmak için bir Uzak Masaüstü veya terminal oturumu ayarlayın.

SDK'yı yüklemeden yalıtılmış bir Python ortamı oluşturmanızı öneririz. Bu makalede kullansa [Miniconda](https://docs.conda.io/en/latest/miniconda.html), ayrıca tam kullanabilirsiniz [Anaconda](https://www.anaconda.com/) yüklü veya [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

Bu makaledeki yönergeleri hızlı ve öğretici not defterlerini çalıştırmak için gereken tüm paketleri yükler.  Diğer örnek not defterleri, ek bileşen yüklenmesini gerektirebilir.  Bu bileşenler hakkında daha fazla bilgi için bkz. [Python için Azure Machine Learning SDK'sını yükleme](https://docs.microsoft.com/python/api/overview/azure/ml/install).

### <a name="install-miniconda"></a>Miniconda'yı yükleme

[Miniconda yükleyip](https://docs.conda.io/en/latest/miniconda.html). Python 3.7 sürümün yükleneceğini seçin. Python 2.x sürümünü seçmeyin.  

### <a name="create-an-isolated-python-environment"></a>Yalıtılmış Python ortamı oluşturma

1. Bir komut satırı penceresi açın ve ardından adlı yeni bir conda ortam oluşturmak *myenv* ve Python 3.6.5 yükleyin. Python 3.5.2 ile iş veya üzeri Azure Machine Learning SDK'sı olacaktır, ancak otomatik makine bileşenleri öğrenimi Python-3.7 tam işlevsel değildir.  Bileşenleri ve paketleri indirilen ortamı oluşturmak için birkaç dakika sürer.

    ```shell
    conda create -n myenv python=3.6.5
    ```

1. Ortamı etkinleştirin.

    ```shell
    conda activate myenv
    ```

1. Ortama özgü ıpython çekirdekler etkinleştir:

    ```shell
    conda install notebook ipykernel
    ```

    Ardından çekirdek oluşturun:

    ```shell
    ipython kernel install --user
    ```

### <a name="install-the-sdk"></a>SDK yükle

1. Etkinleştirilen conda ortamında, Machine Learning SDK'sı temel bileşenleri ile Jupyter not defteri özellikleri yükleyin. Yükleme, makinenizin yapılandırmasına göre tamamlanması birkaç dakika sürer.

    ```shell
    pip install --upgrade azureml-sdk[notebooks]
    ```

1. Azure Machine Learning öğreticileri için bu ortamı kullanmak için bu paketleri yükleyin.

    ```shell
    conda install -y cython matplotlib pandas
    ```

1. Azure Machine Learning öğreticileri için bu ortamı kullanmak için otomatik makine öğrenme bileşenleri yükleyin.

    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

> [!IMPORTANT]
> Bazı komut satırı araçlarını, tırnak işaretleri gibi eklemeniz gerekebilir:
> *  'azureml-sdk[notebooks]'
> * 'azureml-sdk [automl]'
>

### <a name='sdk-create'></a> SDK'sı ile çalışma alanı oluşturma

Python SDK'sını kullanarak bir Jupyter not defterinde çalışma alanınızı oluşturun.

1. Oluşturma ve/veya öğreticileri ve hızlı başlangıç için kullanmak istediğiniz dizine cd.

1. Jupyter not defteri başlatmak için şu komutu girin:

    ```shell
    jupyter notebook
    ```

1. Tarayıcı penceresinde varsayılan `Python 3` çekirdeğini kullanarak yeni bir notebook oluşturun. 

1. SDK sürümü görüntülemek için girin ve ardından bir not defteri hücreye aşağıdaki Python kodunu yürütün:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. Bulma için bir değer `<azure-subscription-id>` parametresinde [Azure portalında abonelikleri listesi](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sahip veya katkıda bulunan rolüne sahip olduğunuz herhangi bir aboneliği kullanabilirsiniz. Roller hakkında daha fazla bilgi için bkz. [yönetmek için bir Azure Machine Learning çalışma alanına erişim](how-to-assign-roles.md) makalesi.

   ```python
   from azureml.core import Workspace
   ws = Workspace.create(name='myworkspace',
                         subscription_id='<azure-subscription-id>', 
                         resource_group='myresourcegroup',
                         create_resource_group=True,
                         location='eastus2' 
                        )
   ```

   Siz kodu yürütürken, Azure hesabınızda oturum açmanız istenebilir. Oturum açtıktan sonra kimlik doğrulama belirteci yerel önbelleğe alınır.

1. Çalışma alanını görüntülemek için aşağıdaki kodu ilişkili depolama, kapsayıcı kayıt defteri ve anahtar kasası gibi ayrıntılarını girin:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


### <a name="write-a-configuration-file"></a>Yapılandırma dosyası yazma

Bir yapılandırma dosyası geçerli dizin için çalışma alanınızı ayrıntılarını kaydedin. Bu dosya adında *aml_config/config.json*.  

Bu çalışma alanı yapılandırma dosyası, aynı çalışma alanına daha sonra yüklemek kolaylaştırır. Diğer dizüstü bilgisayarlar ve aynı dizine veya kod kullanarak bir alt komut yük `ws=Workspace.from_config()` . 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

Bu `write_config()` API çağrısı, yapılandırma dosyasını geçerli dizinde oluşturur. *Config.json* dosyası şunları içerir:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

> [!TIP]
> Python betiklerini veya Jupyter Notebook diğer dizinlerde yer alan çalışma alanınızla kullanmak üzere, bu dosya bu dizine kopyalayın. Adlı bir alt dizinde aynı dizinde dosya olabilir *aml_config*, veya bir üst dizin.

## <a name="resource-manager-template"></a>Resource manager şablonu

Bir çalışma alanı ile bir şablon oluşturmak için bkz [bir şablonu kullanarak bir Azure Machine Learning hizmeti çalışma alanı oluşturma](how-to-create-workspace-template.md)

## <a name="cli"></a>CLI

CLI ile bir çalışma alanı oluşturmak için bkz [CLI uzantısını Azure Machine Learning hizmeti için kullanacağınız](reference-azure-machine-learning-cli.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl oluşturulduğu ne olursa olsun, çalışma alanınızda görüntüleyebilirsiniz [Azure portalında](https://portal.azure.com/).  Bkz: [bir çalışma alanını görüntülemek](how-to-manage-workspace.md#view) Ayrıntılar için.

* Bu hızlı başlangıçlar ve öğreticilerle çalışma alanınızı deneyin.

    * Hızlı Başlangıç: [Jupyter not defteri bulutta çalışan](quickstart-run-cloud-notebook.md).
    * Hızlı Başlangıç: [Kendi sunucunuza Jupyter not defteri çalıştırmak](quickstart-run-local-notebook.md).
    * İki bölümlü öğretici: [Train](tutorial-train-models-with-aml.md) ve [dağıtma](tutorial-deploy-models-with-aml.md) bir görüntü sınıflandırma modu.
    * İki bölümlü öğretici: [Veri hazırlama](tutorial-data-prep.md) ve [otomatik machine Learning'i](tutorial-auto-train-models.md) bir regresyon modeli derler.

* Kullanma hakkında daha fazla bilgi edinin [geliştirme ortamını yapılandırma](how-to-configure-environment.md).

* Daha fazla bilgi edinin [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk).
