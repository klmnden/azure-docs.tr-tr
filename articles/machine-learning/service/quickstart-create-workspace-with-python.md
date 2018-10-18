---
title: "Hızlı başlangıç: Machine Learning hizmeti çalışma alanı oluşturmak için Python SDK'sını kullanma - Azure Machine Learning"
description: Azure Machine Learning hizmetini kullanmaya başlayın.  Python SDK'sını yükleyin ve çalışma alanı oluşturmak için kullanın. Bu çalışma alanı Azure Machine Learning hizmetiyle bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için temel bileşenlerden biridir.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: ee24c1797d0f52d2529ed583a0cfe90cc9e27035
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067777"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Hızlı başlangıç: Machine Learning hizmetini kullanmaya başlamak için Python'ı kullanma

Bu hızlı başlangıçta Python için Azure Machine Learning SDK'sını kullanarak bir Machine Learning hizmeti [çalışma alanı](concept-azure-machine-learning-architecture.md) oluşturup kullanacaksınız. Bu çalışma alanı Azure Machine Learning hizmetiyle bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için temel bileşenlerden biridir.

Bu öğreticide Python SDK'sını yükleyip şu işlemleri gerçekleştireceksiniz:
* Azure aboneliğinizde çalışma alanı oluşturma
* Bu çalışma alanı için daha sonra diğer notebook'larda ve betiklerde kullanmak üzere bir yapılandırma dosyası oluşturma
* Çalışma alanına değer kaydeden bir kod yazma
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme

Bu hızlı başlangıçta oluşturduğunuz çalışma alanı ve yapılandırma dosyası, diğer Azure Machine Learning öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir. Tüm Azure hizmetlerinde olduğu gibi Azure Machine Learning hizmetinde de sınırlar ve kotalar vardır. [Kotalar ve artış talebinde bulunma hakkında bilgi edinin.](how-to-manage-quotas.md)

Size kolaylık sağlamak için, şu Azure kaynakları bölgesel olarak sağlandığında otomatik olarak çalışma alanınıza eklenir: [kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/), [depolama](https://azure.microsoft.com/services/storage/), [uygulama içgörüleri](https://azure.microsoft.com/services/application-insights/), ve [anahtar kasası](https://azure.microsoft.com/services/key-vault/).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


##  <a name="install-the-sdk"></a>SDK yükle

27 Eylül 2018 tarihinden sonra oluşturulmuş olan bir Veri Bilimi Sanal Makinesi (DSVM) kullanıyorsanız **bu bölümü atlayın**. Bu DSVM'lerde Python SDK önceden yüklenmiştir.

SDK'yı yüklemeden önce yalıtılmış bir Python ortamı oluşturmanızı öneririz. Bu hızlı başlangıçta [Miniconda](https://conda.io/docs/user-guide/install/index.html) kullanılmıştır ancak yüklü tam [Anaconda](https://www.anaconda.com/) sürümünü veya [Python virtualenv](https://virtualenv.pypa.io/en/stable/) ortamını da kullanabilirsiniz.

### <a name="install-miniconda"></a>Miniconda'yı yükleme


Miniconda'yı [indirin](https://conda.io/miniconda.html) ve yükleyin. Python 3.7 veya üzeri sürümünü seçin. Python 2.x sürümünü seçmeyin.

### <a name="create-an-isolated-python-environment"></a>Yalıtılmış Python ortamı oluşturma 

Komut satırı penceresi açın ve Python 3.6 ile `myenv` adlı yeni bir conda ortamı oluşturun.

```sh
conda create -n myenv -y Python=3.6
```

Ortamı etkinleştirin.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>SDK yükle

Etkinleştirilen conda ortamına SDK'yı yükleyin. Bu kod `myenv` conda ortamına Azure Machine Learning SDK'sı temel bileşenlerine ek olarak bir Jupyter Notebook sunucusu yükler.  Yüklemenin tamamlanması **yaklaşık 4 dakika** sürer.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bu komutu yazarak Jupyter Notebook sunucusunu başlatın.
```sh
jupyter notebook
```

Tarayıcı penceresinde varsayılan `Python 3` çekirdeğini kullanarak yeni bir notebook oluşturun. 

Aşağıdaki Python kodunu bir notebook hücresine yazıp yürüterek SDK sürümünü görüntüleyin.

```python
import azureml.core
print(azureml.core.VERSION)
```

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

Depolama alanı, kapsayıcı kayıt defteri ve anahtar kasası dahil olmak üzere çalışma alanının ayrıntılarını görmek için şunu yazın:

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Yapılandırma dosyası yazma

Çalışma alanınızın ayrıntılarını geçerli dizindeki bir yapılandırma dosyasına kaydedin. Bu dosyaya 'aml_config\config.json' adı verilir.  

Bu çalışma alanı yapılandırma dosyası, bu çalışma alanını daha sonra aynı dizinde veya alt dizinde diğer notebook'larda ve betiklerde yüklemeyi kolaylaştırır. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

`write_config()` API çağrısı geçerli dizinde yapılandırma dosyası oluşturur. `config.json` dosyası şunları içerir:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>Çalışma alanını kullanma

Deneme çalıştırmalarını izlemek için SDK'nın temel API'lerini kullanan bir kod yazın.

```python
from azureml.core import Experiment

# create a new experiemnt
exp = Experiment(workspace=ws, name='myexp')

# start a run
run = exp.start_logging()

# log a number
run.log('my magic number', 42)

# log a list (Fibonacci numbers)
run.log_list('my list', [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]) 

# finish the run
run.complete()
```

## <a name="view-logged-results"></a>Günlüğe kaydedilen sonuçları görüntüleme
Çalıştırma tamamlandığında deneme çalıştırmasının sonucunu Azure portalda görüntüleyebilirsiniz. Son çalıştırmanın sonuçlarını içeren bir URL yazdırmak için aşağıdaki kodu kullanın.

```python
print(run.get_portal_url())
```

Bağlantıyı kullanarak Azure portala kaydedilen değerleri tarayıcınızda görüntüleyebilirsiniz.

![portala kaydedilen değerler](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 
>[!IMPORTANT]
>Oluşturduğunuz kaynaklar, diğer Azure Machine Learning öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir.

Burada oluşturduğunuz kaynakları kullanmayacaksanız ücret ödememek için bu hızlı başlangıçta oluşturduğunuz kaynakları silin.

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Sonraki adımlar

Artık modelleri denemeye ve dağıtmaya başlamak için gerekli kaynakları oluşturdunuz. Ayrıca not defterinden kod çalıştırdınız ve buluttaki çalışma alanınızda bu koddan gelen çalıştırma geçmişini incelediniz.

Ortamınızı Azure Machine Learning öğreticilerinde kullanabilmek için birkaç pakete daha ihtiyacınız var:

1. Tarayıcınızda notebook'unuzu kapatın.
1. Komut satırı penceresinde `Ctrl`+`C` komutunu kullanarak notebook sunucusunu durdurun.
1. Ek paketleri yükleyin.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Bu paketleri yükledikten sonra model eğitme ve dağıtma öğreticilerini izleyin.  

> [!div class="nextstepaction"]
> [Öğretici: Görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md)

[GitHub'daki daha gelişmiş örnekleri](https://aka.ms/aml-notebooks) de keşfedebilirsiniz.
