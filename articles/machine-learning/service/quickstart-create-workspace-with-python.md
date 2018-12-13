---
title: 'Hızlı Başlangıç: Python kullanmaya başlayın'
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
ms.openlocfilehash: 2ca97275848d87ccc03c7839265f867f9c3c3948
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53073372"
---
# <a name="quickstart-use-python-sdk-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak kullanım Python SDK'sı

Bu hızlı başlangıçta Python için Azure Machine Learning SDK'sını kullanarak bir Machine Learning hizmeti [çalışma alanı](concept-azure-machine-learning-architecture.md) oluşturup kullanacaksınız. Bu çalışma alanı Machine Learning ile bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için kullanabileceğiniz temel bileşenlerden biridir. Bu hızlı başlangıçta ilk olarak kendi Python ortamınızı ve Jupyter notebook sunucunuzu yapılandıracaksınız. Uygulamaları yükleme yapmadan çalıştırmak için bkz. [Hızlı başlangıç: Azure portalı kullanarak Azure Machine Learning'i kullanmaya başlama](quickstart-get-started.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Bu öğreticide Python SDK'sını yükleyip şu işlemleri gerçekleştireceksiniz:

* Azure aboneliğinizde çalışma alanı oluşturma.
* Bu çalışma alanı için daha sonra diğer notebook'larda ve betiklerde kullanmak üzere bir yapılandırma dosyası oluşturma.
* Çalışma alanına değer kaydeden bir kod yazma.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Bu hızlı başlangıçta bir çalışma alanı ve bir yapılandırma dosyası oluşturacaksınız. Bunları diğer Machine Learning öğreticileri ve nasıl yapılır makaleleri için ön gereksinim olarak kullanabilirsiniz. Tüm Azure hizmetlerinde olduğu gibi Machine Learning'de de sınırlar ve kotalar vardır. [Kotalar ve artış talebinde bulunma hakkında bilgi edinin.](how-to-manage-quotas.md)

Aşağıdaki Azure kaynakları, bölgesel kullanıma sunulduğunda çalışma alanınıza otomatik olarak eklenir:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Depolama](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Anahtar Kasası.](https://azure.microsoft.com/services/key-vault/)

>[!NOTE]
> Bu makalede kod gerektiren Azure Machine Learning SDK'sı sürüm 1.0.2 veya üzeri. 


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.


## <a name="install-the-sdk"></a>SDK yükle

*27 Eylül 2018 tarihinden sonra oluşturulan bir veri bilimi sanal makinesini kullanıyorsanız bu bölümü atlayın.* Bu veri bilimi sanal makinelerinde Python SDK önceden yüklenmiştir.

SDK'yı yüklemeden yalıtılmış bir Python ortamı oluşturmanızı öneririz. Bu hızlı başlangıçta [Miniconda](https://conda.io/docs/user-guide/install/index.html) kullanılmıştır ancak yüklü tam [Anaconda](https://www.anaconda.com/) sürümünü veya [Python virtualenv](https://virtualenv.pypa.io/en/stable/) ortamını da kullanabilirsiniz.

### <a name="install-miniconda"></a>Miniconda'yı yükleme


Miniconda'yı [indirin](https://conda.io/miniconda.html) ve yükleyin. Python 3.7 veya üzeri sürümünü seçin. Python 2.x sürümünü seçmeyin.

### <a name="create-an-isolated-python-environment"></a>Yalıtılmış Python ortamı oluşturma 

Komut satırı penceresi açın. Python 3.6 ile `myenv` adlı yeni bir conda ortamı oluşturun.

```shell
conda create -n myenv -y Python=3.6
```

Ortamı etkinleştirin.

  ```shell
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>SDK yükle

Etkinleştirilen conda ortamına SDK'yı yükleyin. Bu kod, Machine Learning SDK'sının temel bileşenlerini yükler. Ayrıca conda ortamında Jupyter Notebook sunucusu yükler. Yüklemenin tamamlanması makinenizin yapılandırmasına bağlı olarak birkaç dakika sürer.

```sh
# install Jupyter
conda install nb_conda

# install the base SDK and Jupyter Notebook
pip install azureml-sdk[notebooks]

```

Farklı "fazladan" anahtar sözcükler, SDK'sının ek bileşenleri yüklemek için de kullanabilirsiniz.

```sh
# install the base SDK and auto ml components
pip install azureml-sdk[automl]

# install the base SDK and model explainability component
pip install azureml-sdk[explain]

# install the base SDK and experimental components
pip install azureml-sdk[contrib]
```

Bu yükleme, bunun yerine bir Databricks ortamında kullanın.

```
# install the base SDK and automl components in Azure Databricks environment
# read more at: https://github.com/Azure/MachineLearningNotebooks/tree/master/databricks
pip install azureml-sdk[databricks]
```


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Jupyter Notebook'u başlatmak için bu komutu girin.
```shell
jupyter notebook
```

Tarayıcı penceresinde varsayılan `Python 3` çekirdeğini kullanarak yeni bir notebook oluşturun. 

SDK sürümünü görüntülemek için aşağıdaki Python kodunu bir notebook hücresine yazın ve yürütün.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

Yeni bir Azure kaynak grubu ve yeni bir çalışma alanı oluşturun.

[Azure portaldaki abonelikler listesinde](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) `<azure-subscription-id>` için bir değer bulun. Sahip veya katkıda bulunan rolüne sahip olduğunuz herhangi bir aboneliği kullanabilirsiniz.

```python
from azureml.core import Workspace
ws = Workspace.create(name='myworkspace',
                      subscription_id='<azure-subscription-id>',    
                      resource_group='myresourcegroup',
                      create_resource_group=True,
                      location='eastus2' # or other supported Azure region  
                     )
```

Yukarıdaki kodu yürüttüğünüzde Azure hesabınızda oturum açmanızı isteyen yeni bir tarayıcı penceresi açılabilir. Oturum açtıktan sonra kimlik doğrulama belirteci yerel önbelleğe alınır.

Depolama alanı, kapsayıcı kayıt defteri ve anahtar kasası dahil olmak üzere çalışma alanının ayrıntılarını görmek için aşağıdaki kodu girin.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


## <a name="write-a-configuration-file"></a>Yapılandırma dosyası yazma

Çalışma alanınızın ayrıntılarını geçerli dizindeki bir yapılandırma dosyasına kaydedin. Bu dosyaya 'aml_config\config.json' adı verilir.  

Bu çalışma alanı yapılandırma dosyası, bu çalışma alanını daha sonra yüklemeyi kolaylaştırır. Bunu başka notebook'lar ve betiklerle aynı dizine veya bir alt dizine yükleyebilirsiniz. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]


`write_config()` API çağrısı geçerli dizinde yapılandırma dosyası oluşturur. `config.json` dosyası aşağıdaki betiği içerir.

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
Çalıştırma tamamlandığında deneme çalıştırmasının sonucunu Azure portalda görüntüleyebilirsiniz. Son çalıştırmanın sonuçlarını içeren bir URL yazdırmak için aşağıdaki kodu kullanın.

```python
print(run.get_portal_url())
```

Bağlantıyı kullanarak Azure portala kaydedilen değerleri tarayıcınızda görüntüleyebilirsiniz.

![Portala kaydedilen değerler](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 
>[!IMPORTANT]
>Oluşturduğunuz kaynaklar, diğer Machine Learning öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir.

Burada oluşturduğunuz kaynakları daha sonra kullanmayı planlamıyorsanız silerek ücret tahsil edilmesini engelleyebilirsiniz.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]


## <a name="next-steps"></a>Sonraki adımlar

Deneme ve model dağıtımı için gerekli kaynakları oluşturdunuz. Ayrıca bir defterde bulunan kodları da çalıştırdınız. Buluttaki çalışma alanınızda bu koddan gelen çalıştırma geçmişini de incelediniz.

Ortamınızı Machine Learning öğreticilerinde kullanabilmek için birkaç pakete daha ihtiyacınız var.

1. Tarayıcınızda notebook'unuzu kapatın.
1. Komut satırı penceresinde `Ctrl`+`C` komutunu kullanarak notebook sunucusunu durdurun.
1. Ek paketleri yükleyin.

    ```shell
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```


Bu paketleri yükledikten sonra model eğitme ve dağıtma öğreticilerini izleyin. 

> [!div class="nextstepaction"]
> [Öğretici: Görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md)

[GitHub'daki daha gelişmiş örnekleri](https://aka.ms/aml-notebooks) de keşfedebilirsiniz.
