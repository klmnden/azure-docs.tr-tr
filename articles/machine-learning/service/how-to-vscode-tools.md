---
title: Machine learning için Visual Studio Code'u kullanma
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
ms.openlocfilehash: 70f9c34957b977aff9fc6211bf79415ed9abe255
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66016519"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning'i kullanmaya başlayın

Bu makalede, uzantı Azure Machine Learning için Visual Studio Code için eğitmek ve makine öğrenimi ve derin öğrenme modelleri dağıtmak için nasıl kullanılacağını öğreneceksiniz.

[Azure Machine Learning hizmeti](overview-what-is-azure-ml.md) hedefleri yerel olarak ve uzaktan çalışan denemeleri işlem için destek sağlar. Her bir deneme için birden çok çalıştırma çalıştırmalarınızı farklı teknikleri, hiperparametreleri ve daha fazlasını deneyin genellikle gerektiği şekilde takip edebilirsiniz. Azure Machine Learning, özel ölçümler izlemek ve çalıştırma, veri bilimi yeniden üretilebilirliğini ve denetlenebilirlik etkinleştirme denemek için kullanabilirsiniz.

Bu gibi durumlarda, bu modeller ayrıca test ve üretim ihtiyaçlarınız için dağıtabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree).

+ Visual Studio Code yüklenmesi gerekir. Visual Studio Code masaüstünüzde çalışan bir hafif ama güçlü bir kaynak kod düzenleyicidir. Python ve diğer programlama dillerine yönelik yerleşik destek ile birlikte gelir. Visual Studio Code henüz yüklemediyseniz [öğrenin](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 veya sonraki sürümler yükleme](https://www.anaconda.com/download/).


## <a name="install-the-extension-for-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning için uzantıyı yükleme

Azure Machine Learning uzantısını yüklediğinizde (internet erişimi varsa) iki daha fazla uzantı otomatik olarak yüklenir. Bunlar [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) ve [Microsoft Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Azure Machine Learning ile çalışmak için bir Python ile tümleşik geliştirme ortamına (IDE) Visual Studio Code açmanız gerekir. Kullanılacak Microsoft Python uzantısı ihtiyacınız [Visual Studio Code içindeki Python](https://code.visualstudio.com/docs/languages/python). Bu uzantı Azure Machine Learning uzantısı ile otomatik olarak yüklenir. Uzantı, Visual Studio Code mükemmel bir IDE sağlar ve tüm işletim sistemlerinde Python yorumlayıcılarını çeşitli ile çalışır. Otomatik Tamamlama, IntelliSense, linting, hata ayıklama ve birim testi sağlamak için Microsoft Python uzantı tüm Visual Studio Code'nın gücünü kullanır. Uzantı, sanal dahil olmak üzere, Python ortamları ve conda ortamları arasında kolayca geçiş yapmanızı sağlar. Çalıştıran ve Python kodunda hata ayıklama, düzenleme hakkında daha fazla bilgi için bkz [Python hello-world öğretici](https://code.visualstudio.com/docs/python/python-tutorial).

Azure Machine Learning uzantıyı yüklemek için:

1. Visual Studio Code'u açın.

1. Bir web tarayıcısında Git [Azure Machine Learning için Visual Studio Code uzantısı (Önizleme)](https://aka.ms/vscodetoolsforai).

1. Bu web sayfasında **yükleme**. 

1. Uzantı sekmesinde **yükleme**.

1. Visual Studio Code'da uzantısı için Hoş Geldiniz bir sekme açar ve (aşağıdaki ekran görüntüsünde kırmızı ana hatları) Azure simgesi etkinlik Çubuğu'na eklenir.

   ![Visual Studio Code etkinlik çubuğundaki Azure simgesi](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. İletişim kutusunda **oturum** ve Azure ile kimlik doğrulaması için istemleri izleyin. 
   
   Visual Studio Code uzantısı için Azure Machine Learning ile birlikte yüklenen Azure hesabı uzantısını Azure hesabınızla kimlik doğrulaması yardımcı olur. Sayfa için komutları listesi için bkz. [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Kullanıma [Intellicode uzantısı için Visual Studio Code (Önizleme)](https://go.microsoft.com/fwlink/?linkid=2006060). Intellicode geçerli kod bağlama göre en uygun Otomatik Tamamlama çıkarımını yapma gibi IntelliSense Python için bir dizi yapay ZEKA destekli özelliği sağlar.

## <a name="install-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK'sını yükleme

1. Python 3.5 veya sonraki sürümünün yüklü olduğundan ve Visual Studio Code tarafından tanınan emin olun. Şimdi yüklemek, Visual Studio Code'u yeniden başlatın ve [bir Python yorumlayıcısı seçin](https://code.visualstudio.com/docs/python/python-tutorial).

1. Tümleşik terminal penceresinde, kullanılacak Python yorumlayıcısı belirtin. Veya, varsayılan Python yorumlayıcısı kullanmak için Enter tuşuna basın.

   ![Yorumlayıcı seçin](./media/vscode-tools-for-ai/python.png)

1. Penceresinin sağ alt köşesinde bir bildirim, gösteren görünür [Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) otomatik olarak yükleniyor. Yeni oluşturulan Python ortamını yerel ve özel ve Azure Machine Learning hizmeti ile çalışmak için Visual Studio Code önkoşulları vardır.

   ![Azure Machine için Python SDK'sı Learning yükleme](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

Visual Studio code'da makine öğrenimi modelleri dağıtma, oluşturmanız gerekir ve eğitim başlamadan önce bir [Azure Machine Learning hizmeti çalışma alanında](concept-workspace.md) bulutta. Bu çalışma alanı modelleri ve kaynakları içerir. 

Bir çalışma alanı oluşturun ve İlk denemenizi eklemek için:

1. Visual Studio Code etkinlik çubuğundaki Azure simgesini seçin. Azure Machine Learning Kenar çubuğunda görünür.

   [![Çalışma alanı oluşturma](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Azure aboneliğinize sağ tıklayıp **çalışma alanı oluştur**. Bir liste görüntülenir. Örnek animasyonlu görüntüde abonelik addır **ücretsiz deneme**, ve çalışma alanı **TeamWorkspace**. 

1. Listeden bir kaynak grubu seçin ya da komut paletinden Sihirbazı'nı kullanarak yeni bir tane oluşturun.

1. Yeni bir çalışma alanınız için benzersiz ve açık bir ad alanına yazın. Çalışma alanını adlı örnek görüntüde **TeamWorkspace**.

1. Yeni çalışma alanı oluşturmak için Enter tuşuna basın. Abonelik adı altındaki ağacın görüntülenir.

1. Sağ **deneme** düğümünü seçin **deney oluşturma** bağlam menüsünden.  Denemeleri, Azure Machine Learning kullanarak gerçekleştirdiğiniz çalıştırmaların izler.

1. Alanda denemeniz için bir ad girin. Örnek ekran görüntülerinde, denemeyi adlı **MNIST**.
 
1. Yeni bir deneme oluşturmak için Enter tuşuna basın. Denemeyi ağacında, çalışma alanı adı altında görünür.

1. Bir çalışma alanında bir deney olarak ayarlamak için sağ **etkin** denemeler yapın. **Etkin** deneme olan geçerli denemenizi. Bu deneyde bulutta, Visual Studio code'da klasörü aç bağlanacağı. Bu klasör, yerel Python betiklerini içermelidir.

Artık, ana ölçümleri deneme geçmişi içinde depolanır. Benzer şekilde, eğittiğiniz modeli otomatik olarak Azure Machine Learning ile yüklenir ve deneme ölçümleriniz ve günlükleri depolanır. 

[![Visual Studio code'da bir klasör ekleme](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


## <a name="create-and-manage-compute-targets"></a>Oluşturma ve yönetme işlem hedefleri

Visual Studio Code için Azure Machine Learning ile veri hazırlama, modelleri eğitme ve hem yerel hem de uzak işlem hedeflerde dağıtırsınız.

Uzantı, Azure Machine Learning için birden fazla uzak bilgisayar hedefine destekler. Tam listesi daha fazla bilgi için bkz. desteklenen [hedefleri için Azure Machine Learning işlem](how-to-set-up-training-targets.md).

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Visual Studio code'da Azure Machine Learning işlem hedeflerini oluşturma

İşlem hedefi oluşturmak için:

1. Visual Studio Code etkinlik çubuğundaki Azure simgesini seçin. Azure Machine Learning Kenar çubuğunda görünür.

2. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. Aşağıdaki örnek görüntüde abonelik addır **ücretsiz deneme**, ve çalışma alanı **TeamWorkspace**. 

3. Çalışma alanı düğümünde sağ **işlem** düğüm ve **oluşturma işlem**.

4. İşlem hedef listeden seçin. 

5. Komut paleti üzerinde bir sanal makine boyutu seçin.

6. Komut paletini temel alanda işlem hedefi için bir ad girin. 

7. Yeni bir sekmede açılır JSON yapılandırma dosyası içinde herhangi bir Gelişmiş özelliğini belirtin. En yüksek düğüm sayısı gibi özellikleri belirtebilirsiniz.

8. İşlem hedefiniz penceresinin sağ alt köşesindeki yapılandırılıyor bitirdikten sonra Seç **Gönder**.

Bir Azure Machine Learning işlem (AMLCompute) oluşturmak nasıl bir örnek aşağıda verilmiştir:

[![Visual Studio Code'da AML işlem oluşturma](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Çalıştırma yapılandırma dosyası

Visual Studio Code uzantısı, yerel bilgisayarınızda yerel işlem hedefi ve çalıştırma yapılandırmaları, yerel ve docker ortamları için otomatik olarak oluşturur. Çalıştırma yapılandırma dosyaları ilişkili işlem hedef düğümü altında bulabilirsiniz. 

## <a name="train-and-tune-models"></a>Modelleri eğitme ve ayarlama

Azure Machine Learning (Önizleme) Visual Studio Code için hızlı bir şekilde kodunuz üzerinde yineleme, adım adım ve hata ayıklama ve kaynak kodu denetimi çözümünüzü kullanmak için kullanın. 

Azure Machine Learning kullanarak yerel olarak denemenizi çalıştırmak için:

1. Visual Studio Code etkinlik çubuğundaki Azure simgesini seçin. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. 

1. Çalışma alanı düğümünde genişletin **işlem** düğüm ve sağ **çalıştırma yapılandırma** kullanmak istediğiniz işlem. 

1. Seçin **denemeyi çalıştırma**.

1. Dosya Gezgini'nde, çalıştırmak istediğiniz komut dosyasını seçin. 

1. Seçin **görünümü, denemeyi çalıştırma** çalıştırmalarınızı izleyin ve eğitilen Modellerinizi görmek için tümleşik Azure Machine Learning portalı görmek için.

Bir deney yerel olarak çalıştırmak nasıl bir örnek aşağıda verilmiştir:

[![Bir deneme yerel olarak çalıştırma](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-visual-studio-code"></a>Visual Studio code'da denemeleri için Uzak hesaplar kullanın

Eğitim için uzak işlem hedefi kullanmak için bir çalıştırma yapılandırma dosyası oluşturmak gerekir. Azure Machine Learning denemenizi çalıştırmak nerede değil yalnızca bu dosya bildirir ancak ortamı hazırlama nasıl de.

#### <a name="the-conda-dependencies-file"></a>Conda bağımlılıkları dosyasının

Varsayılan olarak, yeni bir conda ortamı oluşturulur ve yükleme bağımlılıklarınızı yönetilir. Ancak, bağımlılıklar ve bunların sürümlerinde belirtmelisiniz *aml_config/conda_dependencies.yml* dosya. 

Aşağıdaki kod parçacığı varsayılan *aml_config/conda_dependencies.yml* belirtir `tensorflow=1.12.0`. Bağımlılık sürümünü belirtmezseniz, en son sürümü kullanılır. Yapılandırma dosyasında ek bağımlılıklar ekleyebilirsiniz.

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

Azure Machine Learning ile denemenizi bir uzaktan çalıştırmak için hedef işlem:

1. Visual Studio Code etkinlik çubuğundaki Azure simgesini seçin. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. 

1. Düzenleyici penceresinde Python betiğinizi sağ tıklatın ve seçin **AML: Azure deneme Çalıştır**. 

1. Komut paleti üzerinde işlem hedefini seçin. 

1. Komut paletini temel alanda çalışma yapılandırması adını girin. 

1. Düzen *conda_dependencies.yml* deneme çalışma zamanı bağımlılıklarını belirtmek için dosya. Penceresinin sağ alt köşesinde seçip **Gönder**. 

1. Seçin **görünümü, denemeyi çalıştırma** çalıştırmalarınızı izleyin ve eğitilen Modellerinizi görmek için tümleşik Azure Machine Learning portalı görmek için.

Uzak işlem hedefte bir deneme çalıştırma konusunda bir örnek aşağıda verilmiştir:

[![Bir uzak hedef bir deneme çalıştırma](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Model dağıtıp yönetmek
Azure Machine Learning dağıtma ve makine öğrenimi Modellerinizi Bulut ve uçta yönetin. 

### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Modelinizin Azure Machine Learning için Visual Studio Code'dan kaydedin.

Modelinizi eğittiğimize göre çalışma alanınızda kaydedebilirsiniz. İzleyebilir ve kaydedilen Modellerinizi dağıtın.

Modelinizi kaydetmek için:

1. Visual Studio Code etkinlik çubuğundaki Azure simgesini seçin. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin.

1. Çalışma alanı düğümü altında sağ **modelleri** ve **modelini kaydettirmek**.

1. Komut paletini temel alanda bir model adı girin. 

1. Listeden yüklemek isteyip istemediğinizi seçin bir **model dosyası** (için tek modelleri) veya bir **model klasörünü** (için modeller TensorFlow gibi birden çok dosya ile). 

1. Klasör veya dosyayı seçin.

1. Model özelliklerinizi penceresinin sağ alt köşesindeki yapılandırılıyor bitirdikten sonra Seç **Gönder**. 

Azure Machine Learning, modelinizin kaydetmek nasıl bir örnek aşağıda verilmiştir:

[![AML modeline kaydediliyor](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-visual-studio-code"></a>Visual Studio Code hizmetinizden dağıtma

Visual Studio Code'da web hizmetinize dağıtabilirsiniz:
+ Azure Container Instances'a (test ACI).
+ Azure Kubernetes Service (AKS) üretim için.

ACI kapsayıcı çalışma sırasında oluşturulur çünkü önceden test etmek için bir ACI kapsayıcı oluşturmanız gerekmez. Ancak, AKS kümeleri önceden yapılandırmanız gerekir. Daha fazla bilgi için [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md).

Bir web hizmetini dağıtmak için:

1. Visual Studio Code etkinlik çubuğundaki Azure simgesini seçin. Azure Machine Learning Kenar çubuğunda görünür.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanınızı genişletin.

1. Çalışma alanı düğümünde genişletin **modelleri** düğümü.

1. Dağıtmak ve istediğiniz modeline sağ tıklayıp **kayıtlı modelden hizmeti Dağıt** bağlam menüsünden.

1. Komut paletini üzerinde dağıtmak istediğiniz işlem hedefini seçin. 

1. Komut paletini temel alanda bu hizmet için bir ad girin.  

1. Komut paleti üzerinde klavyenizde göz atın ve komut dosyasını seçmek için Enter tuşunu seçin.

1. Komut paleti üzerinde klavyenizde göz atın ve conda bağımlılık dosyasını seçmek için Enter tuşunu seçin.

1. Hizmet özelliklerinizi penceresinin sağ alt köşesindeki yapılandırılıyor bitirdikten sonra Seç **Gönder** dağıtılacak. Hizmet Özellikleri dosyasında, yerel bir docker dosyası veya schema.json dosyası belirtebilirsiniz.

Web hizmeti artık dağıtılır.

Bir web hizmetini dağıtmak nasıl bir örnek aşağıda verilmiştir:

[![Bir web hizmetini dağıtma](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Klavye kısayollarını kullanma

Visual Studio code'da Azure Machine Learning özelliklerine erişmek için klavyeyi kullanabilirsiniz. Bilmeniz gereken en önemli klavye kısayolu Ctrl + Shift + komut paletini görüntüleyen P'dir. Komut paletinden tüm klavye kısayolları en yaygın işlemler de dahil olmak üzere Visual Studio Code işlevselliği erişebilirsiniz.

[![Visual Studio Code için Azure Machine Learning için klavye kısayolları](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* Visual Studio Code dışında Azure Machine Learning ile eğitmek hakkında kılavuz için bkz: [Öğreticisi: Azure Machine Learning ile modelleri eğitme](tutorial-train-models-with-aml.md).
* Düzenleme, çalıştırma ve yerel kodda hata ayıklama hakkında kılavuz için bkz: [Python hello-world öğretici](https://code.visualstudio.com/docs/python/python-tutorial).
