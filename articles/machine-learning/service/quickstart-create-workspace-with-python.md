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
ms.openlocfilehash: fd86e3a65b542bad0f3114a32362041c34fb74d8
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53791243"
---
# <a name="quickstart-use-the-python-sdk-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için Python SDK'sını kullanma

Bu hızlı başlangıçta, oluşturun ve ardından Azure Machine Learning hizmeti için Python için Azure Machine Learning SDK'sını kullanın [çalışma](concept-azure-machine-learning-architecture.md). Bu çalışma alanı Machine Learning ile bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için kullanabileceğiniz temel bileşenlerden biridir. Kendi Python ortamını ve Jupyter Notebook sunucusu yapılandırarak başlatın. Yükleme ile çalıştırmak için bkz: [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için Azure portal'ı kullanmanızı](quickstart-get-started.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Bu öğreticide Python SDK'sını yükleyin ve bu görevleri tamamlayın:

* Azure aboneliğinizde çalışma alanı oluşturma.
* Bu çalışma alanı için daha sonra diğer notebook'larda ve betiklerde kullanmak üzere bir yapılandırma dosyası oluşturma.
* Çalışma alanına değer kaydeden bir kod yazma.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Bu hızlı başlangıçta bir çalışma alanı ve bir yapılandırma dosyası oluşturacaksınız. Bu kaynakları diğer Machine Learning öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak kullanabilirsiniz. Diğer Azure hizmetlerinde olduğu gibi vardır limitler ve kotalar Machine Learning ile ilişkilendirilmiş. [Kotalar ve daha fazla isteği hakkında bilgi edinin](how-to-manage-quotas.md).

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

SDK'yı yüklemeden yalıtılmış bir Python ortamı oluşturmanızı öneririz. Bu hızlı başlangıçta [Miniconda](https://conda.io/docs/user-guide/install/index.html) kullanılmıştır ancak yüklü tam [Anaconda](https://www.anaconda.com/) sürümünü veya [Python virtualenv](https://virtualenv.pypa.io/en/stable/) ortamını da kullanabilirsiniz.

### <a name="install-miniconda"></a>Miniconda'yı yükleme


Miniconda'yı [indirin](https://conda.io/miniconda.html) ve yükleyin. Python 3.7 veya üzeri sürümünü seçin. Python 2.x sürümünü seçmeyin.

### <a name="create-an-isolated-python-environment"></a>Yalıtılmış Python ortamı oluşturma 

Komut satırı penceresi açın. Adlı yeni bir conda ortamı oluşturup **myenv** Python 3.6 ile.

```shell
conda create -n myenv -y Python=3.6
```

Ortam etkinleştirin:

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

SDK'ın diğer bileşenleri yüklemek için ek anahtar sözcükleri de kullanabilirsiniz.

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

Jupyter not defteri başlatmak için şu komutu girin:

```shell
jupyter notebook
```

Tarayıcı penceresinde varsayılan kullanarak yeni bir not defteri oluşturma **Python 3** çekirdek. 

SDK sürümünü görüntülemek için aşağıdaki Python kodunu bir notebook hücresine yazın ve yürütün.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

Yeni bir Azure kaynak grubu ve yeni bir çalışma alanı oluşturun.

Bulma için bir değer `<azure-subscription-id>` parametresinde [Azure portalında abonelikleri listesi](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sahip veya katkıda bulunan rolüne sahip olduğunuz herhangi bir aboneliği kullanabilirsiniz.

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

Çalışma alanını görmek için aşağıdaki kodu ilişkili depolama, kapsayıcı kayıt defteri ve anahtar kasası gibi ayrıntılarını girin:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


## <a name="write-a-configuration-file"></a>Yapılandırma dosyası yazma

Ayrıntılarını çalışma alanınızı bir yapılandırma dosyası geçerli dizinde kaydedin. Dosyanın nasıl adlandırıldığı **aml_config\config.json**.  

Çalışma alanı yapılandırma dosyası, aynı bu çalışma alanı daha sonra yüklemek kolaylaştırır. Çalışma alanı diğer dizüstü bilgisayarlar ve aynı dizine veya bir alt komut ile yükleyebilirsiniz. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]


`write_config()` API çağrısı geçerli dizinde yapılandırma dosyası oluşturur. Aşağıdaki betik config.json dosyası içerir:

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

> [!IMPORTANT]
> Oluşturduğunuz kaynakları, diğer Machine Learning öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak kullanılabilir.

Bu hızlı başlangıçta oluşturulan kaynakları kullanmayı planlamıyorsanız, herhangi bir ücret ödememeniz silin.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, denemeler ve model kaynakları oluşturdunuz. Kod içinde bir not defteri çalıştırıldı ve bulutta çalışma alanınızdaki kodunu çalıştırma geçmişini incelediniz.

Machine Learning öğreticileri ile kodu kullanmak için ortamınızda birkaç daha fazla paketleri gerekir.

1. Tarayıcınızda notebook'unuzu kapatın.
1. Jupyter Notebook sunucusu durdurmak için Ctrl + C komut satırı penceresinde girin.
1. Ek paketleri yükleyin:

    ```shell
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```


Paketler yüklendikten sonra eğitmek ve model dağıtma için öğreticileri ile devam edin. 

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)

[GitHub'daki daha gelişmiş örnekleri](https://aka.ms/aml-notebooks) de keşfedebilirsiniz.
