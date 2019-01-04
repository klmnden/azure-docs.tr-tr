---
title: Eğitim ve VS Code modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning için Visual Studio Code ve eğitim ve dağıtma makine öğrenimi ve derin öğrenme modelleri Visual Studio Code kullanarak Azure Machine Learning hizmetindeki başlatma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: e7df9086fa5ffc6273a6cb063bdee3cfdfa73e34
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013324"
---
# <a name="use-visual-studio-code-to-train-and-deploy-machine-learning-models"></a>Visual Studio Code eğitmek ve makine öğrenimi modelleri dağıtmak için kullanın

Bu makalede, nasıl kullanılacağını öğreneceksiniz **Azure Machine Learning için Visual Studio Code** eğitmek ve makine öğrenimi ve derin öğrenme modelleri Visual Studio code'da (VS Code), Azure Machine Learning hizmeti ile dağıtmak için uzantı.

Azure Machine Learning, yerel ve uzak bilgisayar hedefine denemeleri çalıştırmak için destek sağlar. Her bir deneme için birden çok çalıştırma çalıştırmalarınızı farklı teknikleri, hiperparametreleri ve daha fazlasını deneyin genellikle gerektiği şekilde takip edebilirsiniz. Azure Machine Learning, özel ölçümler izlemek ve çalıştırma, veri bilimi yeniden üretilebilirliğini ve denetlenebilirlik etkinleştirme denemek için kullanabilirsiniz.

Ve bu modeli, test ve üretim gereksinimlerine göre dağıtabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

+ Sahip [VS Code için Azure Machine Learning](how-to-vscode-tools.md) uzantısı ayarlayın.

+ Sahip [yüklü Python için Azure Machine Learning SDK](how-to-vscode-tools.md) VS Code ile.

## <a name="create-and-manage-compute-targets"></a>Oluşturma ve yönetme işlem hedefleri

VS Code için Azure Machine Learning ile veri hazırlama, modelleri eğitme ve hem yerel hem de uzak işlem hedeflerde dağıtırsınız.

Bu uzantı, Azure Machine Learning için birkaç farklı uzak işlem hedeflerini destekler. Bkz: [desteklenen işlem hedeflerin tam listesi](how-to-set-up-training-targets.md) Azure Machine Learning için.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>VS code'da Azure Machine Learning işlem hedeflerini oluşturma

**İşlem hedefi oluşturmak için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

2. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. Animasyonlu görüntüde, abonelik adı 'Ücretsiz deneme sürümü' ve 'TeamWorkspace' çalışma alanıdır. 

3. Çalışma alanı düğümünde sağ **işlem** düğüm ve **oluşturma işlem**.

4. İşlem hedef listeden seçin. 

5. Komut paletini içinde bir sanal makine boyutu seçin.

6. Komut paletini işlem hedef alan için bir ad girin. 

7. Herhangi bir Gelişmiş özellikleri yeni bir sekmede açılır JSON yapılandırma dosyası belirtin. En yüksek düğüm sayısı gibi özellikleri belirtebilirsiniz...

8. İşiniz bittiğinde işlem hedef yapılandırma, tıklayın **Gönder** ekranının sağ alt köşesindeki.

Bir Azure Machine Learning işlem (AMLCompute) oluşturmaya yönelik bir örnek aşağıda verilmiştir: [![VS Code'da AML işlem oluşturma](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>'Çalıştırma Yapılandırması' dosyası

VS Code uzantısı otomatik olarak yerel işlem hedefi oluşturmak ve çalıştırmak için yapılandırmaları, **yerel** ve **docker** yerel bilgisayarınızda ortamları. Çalıştırma yapılandırma dosyaları ilişkili işlem hedefi altında bulunamadı. 

Varsayılan yerel çalıştırma yapılandırma dosyasından bir kod parçacığı budur. Varsayılan olarak, `userManagedDependencies: True` yerel deneme çalıştırmalarınızın VS Code Python uzantısı tarafından belirlenen varsayılan Python ortamınızı daha sonra kullanır ve tüm kitaplıklar/bağımlılıklarınızı kendiniz yüklemeniz gerekir.

```yaml
# user_managed_dependencies = True indicates that the environment will be user managed. False indicates that AzureML will manage the user environment.
    userManagedDependencies: True
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

## <a name="train-and-tune-models"></a>Modelleri eğitme ve ayarlama

Azure Machine Learning (Önizleme) VS Code için hızlı bir şekilde kodunuz üzerinde yineleme, adım adım ve hata ayıklama ve kaynak kod denetimi çözümünüzü tercih ettiğiniz kullanmak için kullanın. 

**Denemenizi Azure Machine Learning ile yerel olarak çalıştırmak için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. 

1. Çalışma alanı düğümünde genişletin **işlem** düğümünü seçip sağ tıklayın **çalıştırma yapılandırma** kullanmak istediğiniz işlem. 

1. Seçin **denemeyi çalıştırma**.

1. Dosya Gezgini'nden çalıştırılacak betik seçin. 

1. Tıklayın **görünümü, denemeyi çalıştırma** çalıştırmalarınızı izleyin ve eğitilen Modellerinizi görmek için tümleşik Azure Machine Learning portalı görmek için.

Bir deney yerel olarak çalıştırmaya yönelik bir örnek aşağıda verilmiştir: [![Bir deney yerel olarak çalıştırma](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>VS code'da denemeleri için Uzak hesaplar kullanın

Uzak işlem hedefi eğitimindeki kullanmak için bir çalıştırma yapılandırma dosyası oluşturmanız gerekir. Azure Machine Learning denemenizi çalıştırmak nerede değil yalnızca bu dosya bildirir ancak ortamı hazırlama nasıl de.

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
- tensorflow

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

**Azure Machine Learning ile denemenizi bir uzaktan çalıştırmak için hedef işlem:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. 

1. Düzenleyici penceresinde python betiğinizi üzerinde sağ tıklayıp **AML: Azure deneme Çalıştır**. 

1. Komut paletini işlem hedefini seçin. 

1. Komut paletini içinde alanında çalışma yapılandırması adını girin. 

1. Deneme çalışma zamanı bağımlılıklarını belirtin ve ardından conda_dependencies.yml dosyasını düzenleyerek **Gönder** ekranının sağ alt köşesindeki. 

1. Tıklayın **görünümü, denemeyi çalıştırma** çalıştırmalarınızı izleyin ve eğitilen Modellerinizi görmek için tümleşik Azure Machine Learning portalı görmek için.

Uzak işlem hedefte bir denemeyi çalıştırmak için bir örnek aşağıda verilmiştir: [![Uzaktan bir hedef bir denemeyi çalıştırma](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Model dağıtıp yönetmek
Azure Machine Learning dağıtma ve yönetme, makine öğrenimi modellerini Bulut ve uçta sağlar. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Modelinizin Azure Machine Learning için VS koddan kaydedin.

Modelinizi eğitimli, çalışma alanınızda kaydedebilirsiniz.
Kayıtlı modelleri izlenir ve dağıtılabilir.

**Modelinizi kaydetmek için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin.

1. Çalışma alanı düğümü altında sağ **modelleri** ve **modelini kaydettirmek**.

1. Komut paletini içinde bir model ad alanını girin. 

1. Listeden yüklemek isteyip istemediğinizi seçin. bir **model dosyası** (tek modelleri için) bir **model klasörünü** (için modeller Tensorflow gibi birden çok dosya ile). 

1. Klasör veya dosyayı seçin.

1. İşiniz bittiğinde, model özellikleri yapılandırmak, tıklayın **Gönder** ekranının sağ alt köşesindeki. 

AML modelinize kaydetmek için bir örnek aşağıda verilmiştir: [![AML modeline kaydediliyor](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-vs-code"></a>VS Code hizmetinizden dağıtma

VS Code kullanarak web hizmetinize dağıtabilirsiniz:
+ Azure Container Instance (ACI): sınama
+ Azure Kubernetes Service'i (AKS): üretim için 

Anında oluşturulduğundan önceden test etmek için bir ACI kapsayıcı oluşturmanız gerekmez. Ancak, AKS küme önceden yapılandırılmış olması gerekir. 

Daha fazla bilgi edinin [Azure Machine Learning ile dağıtım](how-to-deploy-and-where.md) genel.

**Bir web hizmetini dağıtmak için:**

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanınızı genişletin.

1. Çalışma alanı düğümünde genişletin **modelleri** düğümü.

1. Dağıtmak ve istediğiniz modeline sağ tıklayıp **kayıtlı modelden hizmeti Dağıt** bağlam menüsünden komutu.

1. Komut paletini, listeden dağıtılacağı işlem hedefi seçin. 

1. Komut paletini içinde alanına bu hizmet için bir ad girin.  

1. Komut paletini içinde klavyenizde ve komut dosyasını seçmek için Enter tuşuna basın.

1. Komut paletini klavyenizde ve conda bağımlılık dosyasını seçmek için Enter tuşuna basın.

1. İşiniz bittiğinde, hizmet özellikleri yapılandırmak, tıklayın **Gönder** dağıtmak için ekranının sağ alt köşesindeki. Hizmet Özellikleri dosyasında bu yerel bir Docker dosyası veya kullanmak istediğiniz schema.json dosyası belirtebilirsiniz.

Web hizmeti artık dağıtılır.

Bir web hizmeti dağıtmak için bir örnek aşağıda verilmiştir: [![Bir web Hizmeti'ni dağıtma](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

VS Code dışında Machine Learning ile eğitim talimatları için okuma [Öğreticisi: Azure Machine Learning ile modelleri eğitme](tutorial-train-models-with-aml.md).

Çalıştıran ve yerel kodda hata ayıklama düzenleme talimatları için bkz. [Python Hello World Öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial)
