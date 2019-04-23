---
title: Visual Studio kodu ile kullanma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning için Visual Studio Code yükleme ve Azure Machine Learning'de basit bir deneme oluşturma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: bbeb0dbbd5e9c919eda4b298dc5bee31965e9bac
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995260"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning'i kullanmaya başlayın

Bu makalede, nasıl kullanılacağını öğreneceksiniz **Azure Machine Learning için Visual Studio Code** eğitmek ve makine öğrenimi ve derin öğrenme modelleri Visual Studio code'da (VS Code), Azure Machine Learning hizmeti ile dağıtmak için uzantı.

Azure Machine Learning hizmeti, yerel ve uzak bilgisayar hedefine denemeleri çalıştırmak için destek sağlar. Her bir deneme için birden çok çalıştırma çalıştırmalarınızı farklı teknikleri, hiperparametreleri ve daha fazlasını deneyin genellikle gerektiği şekilde takip edebilirsiniz. Azure Machine Learning, özel ölçümler izlemek ve çalıştırma, veri bilimi yeniden üretilebilirliğini ve denetlenebilirlik etkinleştirme denemek için kullanabilirsiniz.

Ve bu modeli, test ve üretim gereksinimlerine göre dağıtabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

+ Visual Studio Code yüklenmesi gerekir. VS Code masaüstünüzde çalışan bir hafif ama güçlü bir kaynak kod düzenleyicidir. Python ve daha fazlası için yerleşik destek ile birlikte gelir.  [VS Code yükleme öğrenin](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 veya daha fazla yükleme](https://www.anaconda.com/download/).


## <a name="install-the-azure-machine-learning-for-vs-code-extension"></a>Azure Machine Learning için VS Code uzantısı yükleme

Yüklediğinizde **Azure Machine Learning** uzantısı (internet erişimi varsa) iki daha fazla uzantı otomatik olarak yüklenir. Bunlar [Azure hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısı ve [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) uzantısı

Azure Machine Learning ile çalışmak için Python IDE'ye VS Code etkinleştirmek ihtiyacımız var. Çalışma [Visual Studio Code içindeki Python](https://code.visualstudio.com/docs/languages/python), Azure Machine Learning uzantısı ile otomatik olarak yüklenen Microsoft Python uzantı gerektirir. Uzantı, VS Code mükemmel bir IDE sağlar ve tüm işletim sistemlerinde Python yorumlayıcılarını çeşitli ile çalışır. Otomatik Tamamlama ve IntelliSense, linting, hata ayıklama ve birim testi birlikte conda ortamları ve sanal dahil olmak üzere, Python ortamları arasında kolayca geçiş olanağı sağlamak için VS Code'nın gücü tümünün yararlanır. Çalışan, düzenleme, bu kılavuzun denetleyin ve Python kodunda hata ayıklama, bkz: [Python Hello World Öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial)

**Azure Machine Learning uzantıyı yüklemek için:**

1. VS Code'u başlatın.

1. Bir tarayıcıda ziyaret edin: [Azure Machine Learning için Visual Studio Code (Önizleme)](https://aka.ms/vscodetoolsforai) uzantısı

1. Bu web sayfasında tıklatın **yükleme**. 

1. Uzantı sekmesini tıklatın **yükleme**.

1. Hoş Geldiniz sekme VS Code'da uzantısı açar ve (kırmızı kutu aşağıdaki resimde açıklandığı) Azure simgesi etkinlik çubuğunuza eklenir.

   ![Visual Studio Code etkinlik çubuğundaki Azure simgesi](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. İletişim kutusunda **oturum** ve Azure kimlik doğrulaması yapmak için ekrandaki istemi izleyin. 
   
   VS Code uzantısı için Azure Machine Learning ile birlikte yüklenen Azure hesabı uzantısını Azure hesabınızla kimlik doğrulaması yardımcı olur. Komutları listesini görmek [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) sayfası.

> [!Tip] 
> Kullanıma [Intellicode uzantısı VS Code (Önizleme) için](https://go.microsoft.com/fwlink/?linkid=2006060). Intellicode geçerli kod bağlama göre en uygun Otomatik Tamamlama çıkarımını yapma gibi IntelliSense Python için bir dizi yapay ZEKA destekli özelliği sağlar.

## <a name="azure-ml-sdk-installation"></a>Azure ML SDK'sını yükleme

1. Python 3.5 veya üzeri yüklü ve VS Code tarafından tanınan olduğundan emin olun. Şimdi yüklemek, VS Code'u yeniden başlatın ve bölümündeki yönergeleri kullanarak bir Python yorumlayıcısı seçin https://code.visualstudio.com/docs/python/python-tutorial.

1. Tümleşik terminal penceresinde, kullanılacak Python yorumlayıcısı belirtin ya da ziyaret **Enter** , varsayılan Python yorumlayıcısı kullanılacak.

   ![Yorumlayıcı seçin](./media/vscode-tools-for-ai/python.png)

1. Penceresinin sağ alt köşesinde Azure ML SDK'sını otomatik olarak yüklü olduğunu belirten bir bildirim görüntülenir.    Azure Machine Learning hizmeti ile çalışmak için Visual Studio Code önkoşullarına sahiptir yerel ve özel bir Python ortamı oluşturulur.

   ![Python için Azure Machine Learning SDK'sını yükleyin](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

VS Code kullanarak makine öğrenimi modelleri dağıtma, oluşturmanız gerekir ve eğitim başlamadan önce bir [Azure Machine Learning hizmeti çalışma alanında](concept-azure-machine-learning-architecture.md#workspace) bulutta verilerinizin modelleriniz ve kaynakları içerir. Oluşturun ve bu çalışma alanında ilk denemenizi oluşturma hakkında bilgi edinin.

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

   [![Yükleme](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Azure aboneliğinize sağ tıklayıp **çalışma alanı oluştur**. Bir liste görüntülenir. Animasyonlu görüntüde, abonelik adı 'Ücretsiz deneme sürümü' ve 'TeamWorkspace' çalışma alanıdır. 

1. Listeden mevcut bir kaynak grubunu seçin veya Sihirbazı içinde komut paletini kullanarak yeni bir tane oluşturun.

1. Yeni bir çalışma alanınız için benzersiz ve açık bir ad alanına yazın. Ekran görüntülerinde 'TeamWorkspace' çalışma alanı olarak adlandırılır.

1. ENTER tuşuna basın ve yeni bir çalışma alanı oluşturulur. Abonelik adı altındaki ağacın görüntülenir.

1. Deneme düğümüne sağ tıklayın ve seçin **deney oluşturma** bağlam menüsünden.  Denemeleri, Azure Machine Learning kullanarak gerçekleştirdiğiniz çalıştırmaların izler.

1. Alanda denemenizi bir ad girin. Ekran görüntülerinde denemeyi 'MNIST' olarak adlandırılır.
 
1. ENTER tuşuna basın ve yeni bir deneme oluşturulur. Çalışma alanı adı altındaki ağacın görüntülenir.

1. Bir çalışma alanında bir denemeyi sağ tıklayın ve 'Etkin deneyim olarak Ayarla' seçin. **'Active'** deneme denemeyi olduğundan şu anda kullanmakta olduğunuz ve bulutta bu deneme için VS code'da açık klasörünüze bağlanacak. Bu klasör, yerel Python betiklerini içermelidir.

   Artık her denemenizi denemenizi ile önemli ölçümlerinizin tamamını deneme geçmişinde depolanacak ve eğittiğiniz modeli otomatik olarak Azure Machine Learning için karşıya yüklenen ve, deneme ölçüm ve günlükleri ile depolanan çalışır.

   [![VS code'da bir klasör ekleme](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


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

7. Herhangi bir Gelişmiş özellikleri yeni bir sekmede açılır JSON yapılandırma dosyası belirtin. En yüksek düğüm sayısı gibi özellikleri belirtebilirsiniz.

8. İşiniz bittiğinde işlem hedef yapılandırma, tıklayın **Gönder** ekranının sağ alt köşesindeki.

Bir Azure Machine Learning işlem (AMLCompute) oluşturmaya yönelik bir örnek aşağıda verilmiştir: [![VS Code'da AML işlem oluşturma](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>'Çalıştırma Yapılandırması' dosyası

VS Code uzantısı otomatik olarak yerel işlem hedefi oluşturmak ve çalıştırmak için yapılandırmaları, **yerel** ve **docker** yerel bilgisayarınızda ortamları. Çalıştırma yapılandırma dosyaları ilişkili işlem hedef düğümü altında bulunabilir. 

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

Varsayılan olarak, yeni bir conda ortamı oluşturulur ve yükleme bağımlılıklarınızı yönetilir. Ancak, bağımlılıklar ve bunların sürümlerinde belirtmelisiniz `aml_config/conda_dependencies.yml` dosya. 

Varsayılan 'aml_config/conda_dependencies.yml' nden bir kod parçacığı budur. Örneğin, belirtebilirsiniz ' tensorflow 1.12.0' aşağıda görüldüğü gibi =. Bağımlılık sürümünü belirtmezseniz, en son sürümü kullanılır.  
Yapılandırma dosyasında ek bağımlılıklar ekleyebilirsiniz.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

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

### <a name="use-keyboard-shortcuts"></a>Klavye kısayollarını kullanma

VS Code çoğunu gibi Azure Machine Learning özellikleri VS code'da klavyeden erişilebilir. Bilmeniz gereken en önemli tuş bileşimi, Ctrl + Shift + komut paletini getiren P ' dir. Buradan, tüm klavye kısayolları en yaygın işlemler de dahil olmak üzere VS Code işlevselliğini erişebilirsiniz.

[![VS Code için Azure Machine Learning için klavye kısayolları](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

VS Code dışında Machine Learning ile eğitim talimatları için okuma [Öğreticisi: Azure Machine Learning ile modelleri eğitme](tutorial-train-models-with-aml.md).

Çalıştıran ve yerel kodda hata ayıklama düzenleme talimatları için bkz. [Python Hello World Öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial)
