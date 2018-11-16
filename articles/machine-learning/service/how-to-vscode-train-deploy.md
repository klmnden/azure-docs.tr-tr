---
title: Azure Machine Learning ile yapay ZEKA uzantısı için Visual Studio Code araçları kullanma
description: Yapay ZEKA ve eğitim başlatma ve makine öğrenimi ve derin öğrenme modelleri VS code'da Azure Machine Learning hizmeti ile dağıtmak için Visual Studio Code araçları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: 38a7a4e80542551057b230e9d0217d1cecbc5c42
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710316"
---
# <a name="vs-code-tools-for-ai-train-and-deploy-ml-models-from-vs-code"></a>AI için VS Code araçları: eğitme ve VS koddan ML modelleri dağıtma
Bu makalede, nasıl kullanılacağını öğreneceksiniz **yapay ZEKA için VS Code Araçları** eğitmek ve makine öğrenimi ve derin öğrenme modelleri VS code'da Azure Machine Learning hizmeti ile dağıtmak için uzantı.

Azure Machine Learning, yerel ve uzak bilgisayar hedefine denemeleri çalıştırmak için destek sağlar. Her bir deneme için birden çok çalıştırma çalıştırmalarınızı farklı teknikleri, hiperparametreleri ve daha fazlasını deneyin genellikle gerektiği şekilde takip edebilirsiniz. Azure Machine Learning, özel ölçümler izlemek ve çalıştırma, veri bilimi yeniden üretilebilirliğini ve denetlenebilirlik etkinleştirme denemek için kullanabilirsiniz.

Ve bu modeli, test ve üretim gereksinimlerine göre dağıtabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ [Yapay ZEKA için VS Code araçları sahip](how-to-vscode-tools.md) Azure Machine Learning için ayarlayın.

+ Sahip [yüklü Python için Azure Machine Learning SDK](how-to-vscode-tools.md) VS Code ile.

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.

## <a name="create-and-manage-compute-targets"></a>Oluşturma ve yönetme işlem hedefleri

Visual Studio Code araçları ile yapay ZEKA için verilerinizi hazırlama, modelleri eğitme ve hem yerel hem de uzak işlem hedeflerde dağıtırsınız.

Bu uzantı, Azure Machine Learning için birkaç farklı uzak işlem hedeflerini destekler. Bkz: [desteklenen işlem hedeflerin tam listesi](how-to-set-up-training-targets.md) Azure Machine Learning için.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>VS code'da Azure Machine Learning işlem hedeflerini oluşturma

**İşlem hedefi oluşturmak için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure: Machine Learning kenar çubuğu görünür.

2. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. Animasyonlu görüntüde, abonelik adı 'OpenMind Studio' ve 'MyWorkspace' çalışma alanıdır. 

3. Çalışma alanı düğümünde sağ **işlem** düğüm ve **oluşturma işlem**.

4. İşlem hedef listeden seçin. 

5. Alan bu işlem hedef için benzersiz bir ad girin ve sanal makine boyutunu belirtin.

6. Herhangi bir Gelişmiş özellikleri yeni bir sekmede açılır JSON yapılandırma dosyası belirtin. 

7. İşiniz bittiğinde işlem hedef yapılandırma, tıklayın **son** sağ alt.

İşte bir örnek için Azure Batch AI: [ ![VS code'da Azure Batch AI'ı oluşturma işlem](./media/vscode-tools-for-ai/createcompute.gif)](./media/vscode-tools-for-ai/createcompute.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>VS code'da denemeleri için Uzak hesaplar kullanın

Uzak işlem hedefi eğitimindeki kullanmak için bir çalıştırma yapılandırma dosyası oluşturmanız gerekir. Azure Machine Learning denemenizi çalıştırmak nerede değil yalnızca bu dosya bildirir ancak ortamı hazırlama nasıl de.

#### <a name="the-run-configuration-file"></a>'Çalıştırma Yapılandırması' dosyası

VS Code uzantısı için bir çalıştırma yapılandırması otomatik olarak oluşturur, **yerel** ve **docker** yerel bilgisayarınızda ortamları.

Yapılandırma dosyasını çalıştırın varsayılan bir kod parçacığı budur.

Tüm kitaplıkları/bağımlılıklarınızı yüklemek istiyorsanız, `userManagedDependencies: True` ve yerel deneme çalıştırmalarınızın VS Code Python uzantısı tarafından belirlenen varsayılan Python ortamınızı daha sonra kullanır.

```yaml
# user_managed_dependencies=True indicates that the environment will be user managed. False indicates that AzureML will manage the user environment.
    userManagedDependencies: False
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
    docker:
# Set True to perform this run inside a Docker container.
    enabled: false
```

#### <a name="the-conda-dependencies-file"></a>Conda bağımlılıkları dosyasının

Varsayılan olarak, yeni bir conda ortamı oluşturulur ve yükleme bağımlılıklarınızı yönetilir. Bağımlılıklarınızı de belirtmeniz gerekir ancak `aml_config/conda_dependencies.yml` dosya.

Varsayılan 'aml_config/conda_dependencies.yml' nden bir kod parçacığı budur.
Yapılandırma dosyasında ek bağımlılıklar ekleyebilirsiniz.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

## <a name="train-and-tune-models"></a>Modelleri eğitme ve ayarlama

Azure Machine Learning VS Code ile hızlı bir şekilde kodunuz üzerinde yineleme, adım adım ve hata ayıklama ve kaynak kod denetimi çözümünüz tercih ettiğiniz kullanmayı öğrenin. 

**Azure Machine Learning denemenizi çalıştırmak için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure: Machine Learning kenar çubuğu görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. Animasyonlu görüntüde, abonelik adı 'OpenMind Studio' ve 'MyWorkspace' çalışma alanıdır. 

1. Çalışma alanı düğümünde genişletin **işlem** düğüm ve sağ **çalıştırma yapılandırma** kullanmak istediğiniz işlem. 

1. Seçin **denemeyi çalıştırma**.

1. Tıklayın **görünümü, denemeyi çalıştırma** çalıştırmalarınızı izleyin ve eğitilen Modellerinizi görmek için tümleşik Azure Machine Learning portalı görmek için.

   [![Machine learning denemesi VS kodu çalıştırın.](./media/vscode-tools-for-ai/runexperiment.gif)](./media/vscode-tools-for-ai/runexperiment.gif#lightbox)

## <a name="deploy-and-manage-models"></a>Model dağıtıp yönetmek
Azure Machine Learning dağıtma ve yönetme, makine öğrenimi modellerini Bulut ve uçta sağlar. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Modelinizin Azure Machine Learning için VS koddan kaydedin.

Modelinizi eğitimli, çalışma alanınızda kaydedebilirsiniz.
Kayıtlı modelleri izlenir ve dağıtılabilir.

**Modelinizi kaydetmek için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure: Machine Learning kenar çubuğu görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin.

1. Çalışma alanı düğümü altında sağ **modelleri** ve **modelini kaydettirmek**.

1. Listeden yüklemek isteyip istemediğinizi seçin. bir **model dosyası** (tek modelleri için) bir **model klasörünü** (için modeller Tensorflow gibi birden çok dosya ile). 

1. Dosya Seçici iletişim kutusu, dosya veya klasörü seçmek için kullanın.

   [![İşlem](./media/vscode-tools-for-ai/registermodel.gif)](./media/vscode-tools-for-ai/registermodel.gif#lightbox)

> [!Warning]
> Şimdilik, lütfen etiketleri oluşturulan json dosyasından kaldırın.

### <a name="deploy-your-service-from-vs-code"></a>VS Code hizmetinizden dağıtma

VS Code kullanarak web hizmetinize dağıtabilirsiniz:
+ Azure Container Instance (ACI): sınama
+ Azure Kubernetes Service'i (AKS): üretim için 

Anında oluşturulduğundan önceden test etmek için bir ACI kapsayıcı oluşturmanız gerekmez. Ancak, AKS küme önceden yapılandırılmış olması gerekir. 

Daha fazla bilgi edinin [Azure Machine Learning ile dağıtım](how-to-deploy-and-where.md) genel.

**Bir web hizmetini dağıtmak için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure: Machine Learning kenar çubuğu görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanınızı genişletin.

1. Çalışma alanı düğümünde genişletin **modelleri** düğümü.

1. Dağıtmak ve istediğiniz modeline sağ tıklayıp **kayıtlı modelden hizmeti Dağıt** bağlam menüsünden komutu.

1. VS Code komut paletini, listeden dağıtılacağı işlem hedefi seçin. 

1. Bu hizmet için bir ad alanına girin. 

1. Sağ alt köşesindeki iletişim kutusuna tıklayın **Gözat** ve puanlama komut dosyanızı seçin. İletişim kutusunu kapatır.

1. Yerel bir Docker dosyası varsa, tıklayın **Gözat** ikinci iletişim kutusunda görüntülenen. 

   İletişim kutusunu iptal edin ve yerel bir Docker dosyası belirtmezseniz, bir 'Azure Machine Learning' bir varsayılan olarak kullanılır.

1. Görüntülenen üçüncü iletişim kutusunda **Gözat** yerel conda dosya yolunu seçin ve daha sonra json düzenleyicisinde dosya yolu girin.

1. Bir schema.json dosyanız varsa, istediğiniz'a tıklayın **Gözat** dördüncü iletişim kutusunda görünen ve dosyayı seçin.

Web hizmeti artık dağıtılır.

Azure Container Instance için bir örnek aşağıda verilmiştir: [ ![VS Code Azure Container örneği](./media/vscode-tools-for-ai/deploy.gif)](./media/vscode-tools-for-ai/deploy.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

VS Code dışında Machine Learning ile eğitim talimatları için okuma [öğretici: Azure Machine Learning ile modelleri eğitme](tutorial-train-models-with-aml.md).

Çalıştıran ve yerel kodda hata ayıklama düzenleme talimatları için bkz. [Python Hello World Öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial)
