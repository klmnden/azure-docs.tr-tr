---
title: "Hızlı Başlangıç: Python'da kullanmaya başlayın"
titleSuffix: Azure Machine Learning service
description: Python için Azure Machine Learning hizmetinde kullanmaya başlayın. Deneme, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulut ıaas'yi bloğunda bir çalışma alanı oluşturmak için Python SDK'sını kullanın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: da84d6361d80db8aea797827ed3d7bc612e2eda3
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999059"
---
# <a name="quickstart-use-the-python-sdk-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için Python SDK'sını kullanma

Bu makalede, Python için Azure Machine Learning SDK'sı oluşturun ve ardından bir Azure Machine Learning hizmeti için kullandığınız [çalışma](concept-azure-machine-learning-architecture.md). Çalışma alanı, denemeler, eğitmek ve Machine Learning ile makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır. 

Kendi Python ortamını ve Jupyter Notebook sunucusu yapılandırarak başlamadan. Bu yükleme ile çalıştırmak için bkz [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için Azure portal'ı kullanmanızı](quickstart-get-started.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Bu makalede şunları yapacaksınız:
* Python SDK'yı yükleyin.
* Azure aboneliğinizde çalışma alanı oluşturma.
* Bu çalışma alanı için daha sonra diğer notebook'larda ve betiklerde kullanmak üzere bir yapılandırma dosyası oluşturma.
* Çalışma alanına değer kaydeden bir kod yazma.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Bir çalışma alanı ve diğer Machine Learning öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak kullanılacak bir yapılandırma dosyası oluşturun. Diğer Azure hizmetlerinde olduğu gibi bazı limitler ve kotalar Machine Learning ile ilişkilendirilir. [Kotalar ve artış talebinde bulunma hakkında bilgi edinin.](how-to-manage-quotas.md)

Aşağıdaki Azure kaynakları, bölgesel kullanıma sunulduğunda çalışma alanınıza otomatik olarak eklenir:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Depolama](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Anahtar Kasası.](https://azure.microsoft.com/services/key-vault/)

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

## <a name="install-the-sdk"></a>SDK yükle

> [!IMPORTANT]
> 27 Eylül 2018'den sonra oluşturulmuş bir veri bilimi sanal makinesi kullanıyorsanız, bu bölümü atlayın.
> Veri bilimi sanal makineleri önceden Python SDK ile gelen bu tarihten sonra oluşturulan.

Bu makaledeki kod gerektiren Azure Machine Learning SDK sürümü 1.0.2 veya üzeri.

SDK'yı yüklemeden yalıtılmış bir Python ortamı oluşturmanızı öneririz. Bu makalede kullansa [Miniconda](https://conda.io/docs/user-guide/install/index.html), ayrıca tam kullanabilirsiniz [Anaconda](https://www.anaconda.com/) yüklü veya [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Miniconda'yı yükleme

[Miniconda yükleyip](https://conda.io/miniconda.html). Python 3.7 veya sonraki bir sürümü seçin. Python seçmeyin 2.x.

### <a name="create-an-isolated-python-environment"></a>Yalıtılmış Python ortamı oluşturma 

1. Bir komut satırı penceresi açın ve ardından adlı yeni bir conda ortam oluşturmak *myenv* Python 3.6 ile.

    ```shell
    conda create -n myenv -y Python=3.6
    ```

1. Ortamı etkinleştirin.

    ```shell
    conda activate myenv
    ```

### <a name="install-the-sdk"></a>SDK yükle

Etkinleştirilen conda ortamına SDK'yı yükleyin. Bu kod, Machine Learning SDK'sının temel bileşenlerini yükler. Ayrıca conda ortamında Jupyter Notebook sunucusu yükler. Yükleme, makinenizin yapılandırmasına göre tamamlanması birkaç dakika sürer.

```sh
# Install Jupyter
conda install nb_conda

# Install the base SDK and Jupyter Notebook
pip install azureml-sdk[notebooks]
```

SDK'ın diğer bileşenleri yüklemek için ek anahtar sözcükleri kullanabilirsiniz:

```sh
# Install the base SDK and auto ml components
pip install azureml-sdk[automl]

# Install the base SDK and the model explainability component
pip install azureml-sdk[explain]

# Install the base SDK and experimental components
pip install azureml-sdk[contrib]
```

Azure Databricks ortamında, bunun yerine aşağıdaki yükleme komutunu kullanın:

```
# Install the base SDK and automl components in the Azure Databricks environment.
# For more information, see https://github.com/Azure/MachineLearningNotebooks/tree/master/databricks.
pip install azureml-sdk[databricks]
```


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

1. Jupyter not defteri başlatmak için şu komutu girin:

    ```shell
    jupyter notebook
    ```

1. Tarayıcı penceresinde varsayılan `Python 3` çekirdeğini kullanarak yeni bir notebook oluşturun. 

1. SDK sürümü görüntülemek için girin ve ardından bir not defteri hücreye aşağıdaki Python kodunu yürütün:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. Bulma için bir değer `<azure-subscription-id>` parametresinde [Azure portalında abonelikleri listesi](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sahip veya katkıda bulunan rolüne sahip olduğunuz herhangi bir aboneliği kullanabilirsiniz.

   ```python
   from azureml.core import Workspace
   ws = Workspace.create(name='myworkspace',
                         subscription_id='<azure-subscription-id>', 
                         resource_group='myresourcegroup',
                         create_resource_group=True,
                         location='eastus2' # Or other supported Azure region   
                        )
   ```

   Siz kodu yürütürken, Azure hesabınızda oturum açmanız istenebilir. Oturum açtıktan sonra kimlik doğrulama belirteci yerel önbelleğe alınır.

1. Çalışma alanını görüntülemek için aşağıdaki kodu ilişkili depolama, kapsayıcı kayıt defteri ve anahtar kasası gibi ayrıntılarını girin:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


## <a name="write-a-configuration-file"></a>Yapılandırma dosyası yazma

Bir yapılandırma dosyası geçerli dizin için çalışma alanınızı ayrıntılarını kaydedin. Bu dosya adında *aml_config\config.json*.  

Bu çalışma alanı yapılandırma dosyası, aynı çalışma alanına daha sonra yüklemek kolaylaştırır. Bunu başka notebook'lar ve betiklerle aynı dizine veya bir alt dizine yükleyebilirsiniz. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

`write_config()` API çağrısı geçerli dizinde yapılandırma dosyası oluşturur. *Config.json* dosyası aşağıdaki betiği içerir:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>Çalışma alanını kullanma

Deneme çalıştırmalarını izlemek için SDK'nın temel API'lerini kullanan bir kod yazın.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=useWs)]

## <a name="view-logged-results"></a>Günlüğe kaydedilen sonuçları görüntüleme
Çalıştırma tamamlandığında deneme çalıştırmasının sonucunu Azure portalda görüntüleyebilirsiniz. Son çalıştırmanın sonuçlarını ızgaranın URL yazdırmak için aşağıdaki kodu kullanın:

```python
print(run.get_portal_url())
```

Bağlantıyı kullanarak Azure portala kaydedilen değerleri tarayıcınızda görüntüleyebilirsiniz.

![Azure portalında oturum değerleri](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 
>[!IMPORTANT]
>Kullanabileceğiniz kaynakları oluşturduğunuz diğer Machine Learning öğreticileri için ön koşulları olarak burada ve nasıl yapılır makaleleri.

Bu makalede oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, tüm geçmeyecekseniz ücretlendirmeden kaçınmak için bunları silin.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, denemeler ve modelleri dağıtmak için ihtiyacınız olan kaynakları oluşturdunuz. Kod içinde bir not defteri çalıştırdığınız ve bulutta çalışma alanınızdaki kodunu çalıştırma geçmişini incelediniz.

Machine Learning öğreticileri ile kodu kullanmak için ortamınızda birkaç daha fazla paketleri gerekir.

1. Tarayıcınızda notebook'unuzu kapatın.
1. Jupyter Notebook sunucusu durdurmak için Ctrl + C komut satırı penceresinde seçin.
1. Ek paketleri yükleyin.

    ```shell
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Bu paketler yüklendikten sonra eğitmek ve model dağıtma için öğreticileri ile devam edin. 

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)

[GitHub'daki daha gelişmiş örnekleri](https://aka.ms/aml-notebooks) de keşfedebilirsiniz.
