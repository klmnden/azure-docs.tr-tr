---
title: Python geliştirme ortamını ayarlama
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışırken, bir geliştirme ortamı yapılandırmayı öğrenin. Bu makalede, Conda ortamları kullanma, yapılandırma dosyalarını oluşturma ve kendi bulut tabanlı bir not defteri sunucusu, Jupyter not defterleri, Azure Databricks, Azure not defterleri, IDE, Kod Düzenleyicisi ve veri bilimi sanal makinesi yapılandırma hakkında bilgi edinin.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 02/24/2019
ms.custom: seodec18
ms.openlocfilehash: 4aabf15478a6f8e688ea591832ca325f53144df8
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263205"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı yapılandırma

Bu makalede, bir geliştirme ortamı, Azure Machine Learning hizmeti ile çalışacak şekilde yapılandırma konusunda bilgi edinin. Machine Learning hizmeti platformu belirsiz ' dir.

Yalnızca geliştirme ortamınız için Python 3 Anaconda (için yalıtılmış ortamlara) ve Azure Machine Learning çalışma alanı bilgilerinizi içeren bir yapılandırma dosyası gereksinimleridir.

Bu makalede aşağıdaki ortamları ve Araçlar üzerinde odaklanır:

* Kendi [bulut tabanlı bir not defteri sunucusu](#workstation): Bir işlem kaynağı, Jupyter not defterlerini çalıştırmak için iş istasyonunda kullanın. Azure Machine Learning SDK'sı zaten yüklü olduğu için bunu kullanmaya başlamak için en kolay yoludur.

* [Veri bilimi sanal makinesi (DSVM)](#dsvm): Veri bilimi iş için tasarlanmış olan ve yalnızca sanal makine örnekleri CPU veya GPU tabanlı örnekler için dağıtılabilir Azure bulutta önceden yapılandırılmış bir geliştirme veya deneme ortamı. Python 3, Conda, Jupyter not defterleri ve Azure Machine Learning SDK'sı zaten yüklü. Sanal makine, öğrenme ve makine öğrenimi çözümleri geliştirmek için çerçeveleri, Araçlar ve düzenleyicileri derin öğrenme popüler makine ile birlikte gelir. Machine learning Azure platformunda için en eksiksiz geliştirme ortamı olabilir.

* [Jupyter not defteri](#jupyter): Jupyter Not Defteri kullanıyorsanız, SDK'yı yüklemeniz bazı ek özellikler vardır.

* [Visual Studio Code'u](#vscode): Visual Studio Code kullanırsanız, yüklemek için kullanabileceğiniz bazı kullanışlı uzantılar vardır.

* [Azure Databricks](#aml-databricks): Apache Spark tabanlı bir popüler veri analiz platformu. Modelleri dağıtabilmeniz adına o kümenizi üzerine Azure Machine Learning SDK'sı almayı öğrenin.

* [Azure not defterleri](#aznotebooks): Azure bulutunda barındırılan Jupyter Notebook hizmeti. Başlama, Azure Machine Learning SDK'sı zaten yüklü olduğu için de kolay bir yoludur.  

Python 3 ortam zaten var veya yalnızca SDK'yı yüklemek için temel adımlar istiyorsanız bkz [yerel bilgisayar](#local) bölümü.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning hizmeti çalışma alanı. Çalışma alanı oluşturmak için bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md).

Bir çalışma alanı kendi ile başlamak için ihtiyacınız olan [bulut tabanlı bir not defteri sunucusu](#workstation), [DSVM](#dsvm), [Azure Databricks](#aml-databricks), veya [Azure not defterleri](#aznotebooks).

SDK'sı ortamını yüklemek için [yerel bilgisayar](#local), [Jupyter Notebook sunucusu](#jupyter) veya [Visual Studio Code](#vscode) ayrıca gerekir:

- Her iki [Anaconda](https://www.anaconda.com/download/) veya [Miniconda](https://conda.io/miniconda.html) Paket Yöneticisi.

- Linux veya macOS üzerinde bash kabuğunu gerekir.

    > [!TIP]
    > Linux veya Macos'ta yapıyorsanız ve bash (örneğin, zsh) dışında bir kabuk kullanıyorsanız, bazı komutlar çalıştırdığınızda hatalar alabilirsiniz. Bu sorunu geçici olarak çözmek için kullanın `bash` yeni bir bash Kabuğu'nu başlatın ve orada komutları çalıştırmak için komutu.

- Windows üzerinde bir komut istemi veya Anaconda istemi (Anaconda ve Miniconda ile yüklenen) gerekir.

## <a id="workstation"></a>Kendi bulut tabanlı bir not defteri sunucusu

Azure Machine Learning geliştirmeye başlamanın en kolay yolu için bir Azure Machine Learning çalışma alanınızdaki bir not defteri sunucusu oluşturun.

* Azure Machine Learning SDK'sı zaten yüklüdür.
* İş istasyonu ortamı, çalışma alanı ile çalışmak için otomatik olarak yapılandırılır.
* Kaynak kullanılabilir ve çalışma alanınızda yönetilebilir.

Bulut tabanlı bir not defteri sunucunuz ile geliştirmeye başlamak için bkz: [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-run-cloud-notebook.md).


## <a id="dsvm"></a>Veri bilimi sanal makinesi

Özelleştirilmiş sanal makine (VM) görüntüsü dsvm'dir. Önceden yapılandırılmış bir veri bilimi iş için tasarlanmıştır:

  - TensorFlow, PyTorch, Scikit-öğrenme, XGBoost ve Azure Machine Learning SDK gibi paketleri
  - Spark tek başına ve detaylandırma gibi popüler veri bilimi araçları
  - Azure CLI, AzCopy ve Depolama Gezgini gibi Azure Araçları
  - Visual Studio Code ve PyCharm gibi tümleşik geliştirme ortamlarından (IDE'ler)
  - Jupyter Notebook sunucusu

Azure Machine Learning SDK'sı, Windows veya Ubuntu DSVM sürümünde çalışır. Ancak yalnızca Ubuntu DSVM da işlem hedefi kullanmayı planlıyorsanız, desteklenir.

DSVM bir geliştirme ortamı olarak kullanmak için aşağıdakileri yapın:

1. Bir DSVM aşağıdaki ortamları birini oluşturun:

    * Azure portalı:

        * [Bir Ubuntu veri bilimi sanal makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Windows veri bilimi sanal makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Azure CLI:

        > [!IMPORTANT]
        > * Azure CLI kullandığınızda, ilk Azure aboneliğinizi kullanarak oturum açmalısınız `az login` komutu.
        >
        > * Bu adımda komutları kullandığınızda, bir kaynak grubu adı, VM, bir kullanıcı adı ve parola için bir ad sağlamanız gerekir.

        * Ubuntu veri bilimi sanal makinesi oluşturmak için aşağıdaki komutu kullanın:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Windows veri bilimi sanal makinesi oluşturmak için aşağıdaki komutu kullanın:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```

2. Azure Machine Learning SDK'sı DSVM üzerinde zaten yüklü. SDK'sını içeren Conda ortama kullanmak için aşağıdaki komutlardan birini kullanın:

    * Ubuntu DSVM için:

        ```shell
        conda activate py36
        ```

    * Windows DSVM için:

        ```shell
        conda activate AzureML
        ```

1. SDK'sı erişmek ve sürümü denetlemek doğrulamak için aşağıdaki Python kodu kullanın:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. DSVM Azure Machine Learning hizmeti çalışma alanınızla kullanmak üzere yapılandırmak için bkz [çalışma alanı yapılandırma dosyası oluşturma](#workspace) bölümü.

Daha fazla bilgi için [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Yerel bilgisayar

(Bu da uzak bir sanal makine olabilir) yerel bir bilgisayar kullanırken Anaconda ortam oluşturma ve aşağıdakileri yaparak SDK'sını yükleyin:

1. İndirme ve yükleme [Anaconda](https://www.anaconda.com/distribution/#download-section) (Python 3.7 Sürüm) zaten sahip değilseniz.

1. Anaconda istemi açın ve aşağıdaki komutlarla bir ortam oluşturun:

    Ortam oluşturmak için aşağıdaki komutu çalıştırın.

    ```shell
    conda create -n myenv python=3.6.5
    ```

    Ardından ortamın etkinleştirin.

    ```shell
    conda activate myenv
    ```

    Bu örnek python 3.6.5 kullanarak bir ortam oluşturur, ancak herhangi bir belirli subversions seçilebilir. SDK'sı uyumluluk (3.5 + önerilir) belirli ana sürümlerle garantili ve hatalarla karşılaşırsanız çalıştırırsanız Anaconda ortamınızdaki farklı bir sürüm/subversion denemeniz önerilir. Bileşenleri ve paketleri indirilen ortamı oluşturmak için birkaç dakika sürer.

1. Yeni ortamınızda ortama özgü ıpython çekirdekler etkinleştirmek için aşağıdaki komutları çalıştırın. Bu, beklenen çekirdek ve paket davranışı Jupyter not defterleri ile Anaconda ortamlar içinde çalışırken alma şunları sağlar:

    ```shell
    conda install notebook ipykernel
    ```

    Ardından çekirdek oluşturmak için aşağıdaki komutu çalıştırın:

    ```shell
    ipython kernel install --user
    ```

1. Paketleri yüklemek için aşağıdaki komutları kullanın:

    Bu komut, not defteri ve automl ek özellikler ile temel Azure Machine Learning SDK'sını yükler. `automl` Çok büyük bir yükleme ve otomatik makine öğrenimi denemeleri yapmak istemiyorsanız, noktalarından kaldırılacak. `automl` Ek bağımlılık olarak varsayılan olarak, ayrıca Azure Machine Learning veri hazırlığı SDK'sı içerir.

     ```shell
    pip install azureml-sdk[notebooks,automl]
    ```

    Azure Machine Learning veri hazırlığı SDK'sı, kendi yüklemek için bu komutu kullanın:

    ```shell
    pip install azureml-dataprep
    ```

   > [!NOTE]
   > PyYAML kaldırılamaz bir ileti alırsanız, aşağıdaki komutu kullanın:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   SDK yüklemek için birkaç dakika sürer. Bkz: [Yükleme Kılavuzu](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) yükleme seçenekleri hakkında daha fazla bilgi için.

1. Makine öğrenimi denemesi için diğer paketleri yükleyin.

    Aşağıdaki komutlardan birini kullanın ve Değiştir  *\<yeni paket >* yüklemek istediğiniz paket. Paketleri aracılığıyla yükleme `conda install` paket (yeni kanallar Anaconda bulutta eklenebilir) geçerli kanalların parçası olmasını gerektirir.

    ```shell
    conda install <new package>
    ```

    Alternatif olarak, paketler aracılığıyla yükleyebilirsiniz `pip`.

    ```shell
    pip install <new package>
    ```

### <a id="jupyter"></a>Jupyter Not Defterleri

Jupyter not defterleri parçası olan [Jupyter proje](https://jupyter.org/). Bunlar, Canlı kod anlatım metin ve grafikleri ile karışık belgeleri oluşturmak burada etkileşimli bir kodlama deneyimi sunar. Çıkış, kod bölümlerinin belgenin içine kaydetmek için Jupyter not defterleri de sonuçlarınızı başkalarıyla paylaşmak için harika bir yoludur. Jupyter not defterleri çeşitli platformlarda yükleyebilirsiniz.

Yordamda [yerel bilgisayar](#local) bölümü Jupyter not defterleri Anaconda ortamında çalıştırmak için gerekli bileşenleri yükler. Bu bileşenler, Jupyter not defteri ortamınızda etkinleştirmek için aşağıdakileri yapın:

1. Anaconda istemi açın ve ortamınızı etkinleştirin.

    ```shell
    conda activate myenv
    ```

1. Jupyter not defteri sunucusu aşağıdaki komutla başlatın:

    ```shell
    jupyter notebook
    ```

1. Jupyter not defteri SDK kullanabileceğinizi doğrulamak için oluşturun bir **yeni** Not Defteri, select **Python 3** çekirdek ve not defteri hücreye aşağıdaki komutu çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Modülleri içeri aktarma sorunlarıyla karşılaşırsınız ve alırsanız bir `ModuleNotFoundError`, Jupyter çekirdek bağlı olduğundan doğru yola ortamınız için bir not defteri hücreye aşağıdaki kodu çalıştırarak emin olun.

    ```python
    import sys
    sys.path
    ```

1. Azure Machine Learning hizmeti çalışma alanınızla kullanmak üzere Jupyter not defterini yapılandırmak için Git [çalışma alanı yapılandırma dosyası oluşturma](#workspace) bölümü.

### <a id="vscode"></a>Visual Studio kodu

Visual Studio Code, platformlar arası kod düzenleyicisidir. Python desteği için yerel bir Python 3 ve Conda yükleme kullanır, ancak yapay ZEKA ile çalışmak için ek araçlar sağlar. Kod Düzenleyicisi içinde Conda ortamından seçmek için desteği de sağlar.

Geliştirme için Visual Studio Code'u kullanmak için aşağıdakileri yapın:

1. Visual Studio Code için Python geliştirme kullanmayı öğrenmek için bkz. [VSCode Python'da başlama](https://code.visualstudio.com/docs/python/python-tutorial).

1. Conda ortam seçmek için VS Code açın ve ardından Ctrl + Shift + P (Linux ve Windows) veya komutu + Shift + P (Mac) seçin.
    __Komut paleti__ açılır.

1. Girin __Python: Yorumlayıcıyı seçme__ve ardından Conda ortamı seçin.

1. SDK'sını kullanabilirsiniz doğrulamak için oluşturun ve aşağıdaki kodu içeren yeni bir Python dosyası (.py) çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Visual Studio Code için Azure Machine Learning uzantıyı yüklemek için bkz: [yapay ZEKA Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Daha fazla bilgi için [kullanımı Azure Machine Learning için Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks
Azure Databricks, Azure bulutta Apache Spark tabanlı bir ortam olan. Bu, CPU veya GPU tabanlı işlem kümesi ile işbirliğine dayalı bir not defteri tabanlı ortamı sağlar.

Azure Databricks Azure Machine Learning hizmeti ile nasıl çalıştığı:
+ Spark MLlib kullanarak bir model eğitme ve modeli Azure databricks'te ACI/AKS dağıtın. 
+ Ayrıca [machine learning otomatik](concept-automated-ml.md) Azure Databricks ile özel bir Azure ML SDK'da özellikleri.
+ Azure Databricks işlem hedef olarak kullanabileceğiniz bir [Azure Machine Learning işlem hattı](concept-ml-pipelines.md). 

### <a name="set-up-your-databricks-cluster"></a>Databricks kümenizi ayarlayın

Oluşturma bir [Databricks kümesine](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). Yalnızca otomatik makine öğrenimi Databricks üzerinde SDK'yı yüklerseniz bazı ayarlar uygulanır.
**Kümeyi oluşturmak için birkaç dakika sürer.**

Bu ayarları kullanın:

| Ayar |Uygulandığı öğe| Değer |
|----|---|---|
| Küme adı |her zaman| yourclustername |
| Databricks Çalışma Zamanı |her zaman| Tüm olmayan ML çalışma zamanı (olmayan ML 4.x, 5.x) |
| Python sürümü |her zaman| 3 |
| Çalışanlar |her zaman| 2 veya üzeri |
| Çalışan düğümü VM türleri <br>(en fazla eş zamanlı yineleme sayısı belirler) |Otomatik ML<br>Yalnızca| Tercih edilen VM bellek için iyileştirilmiş |
| Otomatik Ölçeklendirmeyi Etkinleştirme |Otomatik ML<br>Yalnızca| Seçeneğinin işaretini kaldırın |

Devam etmeden önce küme çalışan kadar bekleyin.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Bir Databricks kitaplığa doğru SDK'yı yükleme
Küme çalışmaya başladıktan sonra [bir kitaplığı oluşturma](https://docs.databricks.com/user-guide/libraries.html#create-a-library) kümeniz için uygun Azure Machine Learning SDK paketi eklemek için. 

1. Seçin **tek** seçeneği (diğer bir SDK yüklemesi desteklenir)

   |SDK'sı&nbsp;paket&nbsp;ek özellikler|Kaynak|Pypı&nbsp;adı&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Databricks için| Python yükleme Yumurta veya Pypı | azureml-sdk[databricks]|
   |-With - için Databricks<br> Otomatik ML özellikleri| Python yükleme Yumurta veya Pypı | azureml-sdk[automl_databricks]|

   > [!Warning]
   > Yok, diğer bir SDK'sı ek özellikler yüklenebilir. Yukarıdaki seçeneklerden [databricks] veya [automl_databricks] yalnızca birini seçin.

   * Seçmeyin **ekleme otomatik olarak tüm kümelere**.
   * Seçin **iliştirme** , küme adının yanındaki.

1. İzleyici durum olana kadar hataları **iliştirildiği**, birkaç dakika sürebilir.  Bu adım başarısız olursa aşağıdakileri denetleyin: 

   Kümeniz tarafından yeniden başlatmayı deneyin:
   1. Sol bölmede seçin **kümeleri**.
   1. Tabloda, küme adınızı seçin.
   1. Üzerinde **kitaplıkları** sekmesinde **yeniden**.
      
   Ayrıca göz önünde bulundurun:
   + Automl yapılandırmada kullanırken Azure Databricks, lütfen şu parametreleri ekleyin:
    1. ```max_concurrent_iterations``` Kümenizde çalışan düğümlerinin sayısını temel alır. 
    2. ```spark_context=sc``` #databricks/spark varsayılan spark bağlamı. 
   + Veya eski bir SDK sürümü varsa, kümenin yüklü libs seçimini kaldırmak ve çöp kutusuna taşınacak. Yeni SDK sürümünü yükleyin ve küme yeniden başlatın. Bir sorun olduğunda bundan sonra ayırma ve kümenizi yeniden bağlayın.

Yükleme başarılı olduysa, içeri aktarılan kitaplık bunlardan biri gibi görünmelidir:
   
Databricks için SDK'sı **_olmadan_** machine learning otomatik ![Databricks için Azure Machine Learning SDK](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

Databricks için SDK'sı **ile** machine learning otomatik ![SDK'sı ile makine öğrenimi Databricks üzerinde yüklü otomatik](./media/how-to-configure-environment/automlonadb.jpg)

### <a name="start-exploring"></a>Keşfetmeye başlayın

Deneyin:
+ İndirme [arşiv dosyasını Not Defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/Databricks_AMLSDK_1-4_6.dbc) Azure Databricks/Azure Machine Learning SDK'sı ve [arşiv dosyasını içeri aktarma](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) , Databricks kümesinde.  
  Birçok örnek not defterleri kullanılabilir ancak **yalnızca [Bu örnek not defterleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) Azure Databricks ile çalışır.**
  
+ Bilgi edinmek için nasıl [eğitim işlem olarak Databricks ile işlem hattı oluşturma](how-to-create-your-first-pipeline.md).

## <a id="workspace"></a>Çalışma alanı yapılandırma dosyası oluşturma

Çalışma alanı yapılandırma dosyası, SDK, Azure Machine Learning hizmeti çalışma alanıyla iletişim kurma bildiren bir JSON dosyasıdır. Dosyanın nasıl adlandırıldığı *config.json*, ve şu biçime sahiptir:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Bu JSON dosyası, Python betikleri veya Jupyter not defterleri içeren dizin yapısına olması gerekir. Adlı bir alt dizinde aynı dizinde olabilir *aml_config*, veya bir üst dizin.

Bu dosyadan kodunuzu kullanmak için `ws=Workspace.from_config()`. Bu kod, bilgileri bir dosyadan yükler ve çalışma alanınıza bağlanır.

Yapılandırma dosyası üç şekilde oluşturabilirsiniz:

* **Bağlantısındaki [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md#sdk)**: A *config.json* Azure not defterleri kitaplığınızda dosyası oluşturulur. Dosyanın çalışma alanınız için yapılandırma bilgilerini içerir. İndirin veya kopyalayın *config.json* diğer geliştirme ortamlarıyla.

* **Dosyayı el ile oluşturmak**: Bu yöntemle, bir metin düzenleyicisi kullanın. Çalışma alanınızda ziyaret ederek yapılandırma dosyasına gidin değerleri bulabilirsiniz [Azure portalında](https://portal.azure.com). Çalışma alanı adı, kaynak grubu ve abonelik kimliği değerleri kopyalayın ve bunları yapılandırma dosyasında kullanın.

     ![Azure portal](./media/how-to-configure-environment/configure.png)

* **Program aracılığıyla dosya oluşturma**: Aşağıdaki kod parçacığında, abonelik kimliği, kaynak grubu ve çalışma alanı adı sağlayarak bir çalışma alanına bağlayın. Ardından çalışma alanı yapılandırması dosyasına kaydeder:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Bu kod yapılandırma dosyasına yazar *aml_config/config.json* dosya.

## <a id="aznotebooks"></a>Azure Not Defterleri

[Azure not defterleri](https://notebooks.azure.com) (Önizleme) olan Azure bulutundaki bir etkileşimli bir geliştirme ortamı. Azure Machine Learning ile geliştirmeye başlamak için kolay bir yoludur.

* Azure Machine Learning SDK'sı zaten yüklüdür.
* Azure portalında bir Azure Machine Learning hizmeti çalışma alanı oluşturduktan sonra otomatik olarak Azure not defteri ortamınızı çalışma alanı ile çalışacak şekilde yapılandırmak için bir düğmesine tıklayabilirsiniz.

Kullanım [Azure portalında](https://portal.azure.com) Azure not defterleri ile kullanmaya başlamak için.  Çalışma alanını açın ve **genel bakış** bölümünden **Get Started Azure not defterlerinde**.

Varsayılan olarak, Azure not defterleri veri 1 GB ile 4 GB bellek ile sınırlı olan ücretsiz hizmet katmanı kullanır. Ancak, bu limitleri Azure not defterleri projeye bir veri bilimi sanal makinesi örneği ekleyerek kaldırabilirsiniz. Daha fazla bilgi için [yönetme ve Azure not defterleri projeleri - bilgi işlem katmanı yapılandırma](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğitip](tutorial-train-models-with-aml.md) MNIST veri kümesi ile Azure Machine Learning hakkında
- Görünüm [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) başvurusu
- Hakkında bilgi edinin [Azure Machine Learning veri hazırlama SDK'sı](https://aka.ms/data-prep-sdk)
