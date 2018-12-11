---
title: Geliştirme ortamını yapılandırma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışırken, bir geliştirme ortamı yapılandırmayı öğrenin. Bu belgede, Conda ortamları kullanma, yapılandırma dosyalarını oluşturma ve Jupyter not defterleri, Azure not defterleri, IDE, Kod Düzenleyicisi ve veri bilimi sanal makinesi yapılandırma hakkında bilgi edinin.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6e2222d56ea37983b1efafedaac8e01058cb44fa
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53098058"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı yapılandırma

Bu belgede, bir geliştirme ortamı, Azure Machine Learning hizmeti ile çalışacak şekilde yapılandırma konusunda bilgi edinin. Platform bağımsızlığı Azure Machine Learning hizmetidir. Geliştirme ortamınız için tek gerekenlerdir __Python 3__, __Conda__ (için Yalıtılmış ortamlarda) ve Azure Machine Learning çalışma alanı bilgilerinizi içeren bir yapılandırma dosyası.

Bu belge aşağıdaki belirli ortamlara ve araçları odaklanır:

* [Azure not defterleri](#aznotebooks): Azure bulutunda barındırılan A Jupyter Notebook hizmeti. Bu __kolay__ başlama, Azure Machine Learning SDK'sı zaten yüklü olduğu yolu.
* [Veri bilimi sanal makinesi](#dsvm): A __önceden yapılandırılmış geliştirme/deneme ortamı__ Azure bulut __veri bilimi iş için tasarlanmış__ ve ya da dağıtılabilir Yalnızca sanal makine örnekleri CPU veya GPU örnekleri temel. Python 3, Conda, Jupyter not defterleri ve Azure Machine Learning SDK'sı zaten yüklü. VM popüler ML ile gelir / ML çözümleri geliştirme çerçeveleri, Araçlar ve düzenleyicileri derin öğrenme. Büyük olasılıkla olan __en eksiksiz__ ML için Azure platformunda geliştirme ortamı.
* [Jupyter not defterleri](#jupyter): Jupyter not defterleri zaten kullanıyorsanız, SDK'yı yüklemeniz bazı ek özellikler vardır.
* [Visual Studio Code](#vscode): Visual Studio Code kullanırsanız, yüklemek için kullanabileceğiniz bazı kullanışlı uzantılar vardır.
* [Azure Databricks](#aml-databricks): Apache Spark tabanlı bir popüler veri analiz platformudur. Modelleri ucunuzun kümenizi üzerine Azure Machine Learning SDK'sı almayı öğrenin.

Python 3 ortam zaten var veya yalnızca SDK'yı yüklemek için temel adımlar istiyorsanız bkz [yerel bilgisayar](#local) bölümü.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning hizmeti çalışma alanı. Bağlantısındaki [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) oluşturmak için.

- Her iki [Continuum Anaconda](https://www.anaconda.com/download/) veya [Miniconda](https://conda.io/miniconda.html) Paket Yöneticisi.

    > [!IMPORTANT]
    > Anaconda ve Miniconda Azure not defterleri kullanırken gerekli değildir.

- Linux veya Mac OS, bash kabuğunu gerekir.

    > [!TIP]
    > Linux veya Mac OS ve bash (örneğin, zsh) dışında bir kabuk kullanıyorsanız bazı komutlarını çalıştırırken hata alabilirsiniz. Bu sorunu geçici olarak çözmek için kullanın `bash` yeni bir bash Kabuğu'nu başlatın ve orada komutları çalıştırmak için komutu.

- Windows üzerinde bir komut istemi veya Anaconda istemi (Anaconda ve Miniconda ile yüklenen) gerekir.

## <a id="anotebooks"></a>Azure Not Defterleri

[Azure not defterleri](https://notebooks.azure.com) (Önizleme) olan Azure bulutundaki bir etkileşimli bir geliştirme ortamı. Bu __kolay__ yolu Azure Machine Learning geliştirmeye başlayın.

* Azure Machine Learning SDK'sı __yüklü__.
* Azure portalında bir Azure Machine Learning hizmeti çalışma alanı oluşturduktan sonra otomatik olarak Azure not defteri ortamınızı çalışma alanı ile çalışacak şekilde yapılandırmak için bir düğmesine tıklayabilirsiniz.

Azure not defterleri ile geliştirmeye başlamak için izleyin [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) belge.

## <a id="dsvm"></a>Veri bilimi sanal makinesi

Veri bilimi sanal makinesi (DSVM) bir özelleştirilmiş sanal makine (VM) görüntüsüdür **veri bilimi iş için tasarlanmış** önceden yapılandırılmış olmasıdır:

  - Paketleri gibi Tensorflow, Pytorch, scikit-learn ve Xgboost ve Azure ML SDK'sı
  - Spark tek başına, detaylandırma gibi popüler veri bilimi araçları
  - CLI, Azcopy ve Depolama Gezgini gibi Azure Araçları
  - Visual Studio Code, PyCharm ve RStudio gibi tümleşik geliştirme ortamlarından (IDE'ler)
  - Jupyter Notebook sunucusu 

Azure Machine Learning SDK'sı, Windows veya Ubuntu DSVM sürümünde çalışır. Veri bilimi sanal makinesi bir geliştirme ortamı olarak kullanmak için aşağıdaki adımları kullanın:

1. Bir veri bilimi sanal makinesi oluşturmak için aşağıdaki yöntemlerden birini kullanın:

    * Azure portalını kullanarak:

        * [Oluşturma bir __Ubuntu__ veri bilimi sanal makinesi](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Oluşturma bir __Windows__ veri bilimi sanal makinesi](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Azure CLI kullanarak:

        > [!IMPORTANT]
        > Azure CLI kullanarak, ilk Azure aboneliğinizi kullanarak oturum açmalısınız `az login` komutu.
        >
        > Bu adımda komutlarını kullanarak, bir kaynak grubu adı, VM, bir kullanıcı adı ve parola için bir ad sağlamanız gerekir.

        * Oluşturmak için bir __Ubuntu__ veri bilimi sanal makinesi, aşağıdaki komutu kullanın:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Oluşturmak için bir __Windows__ veri bilimi sanal makinesi, aşağıdaki komutu kullanın:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. Azure Machine Learning SDK'sı **yüklü** DSVM üzerinde. SDK'sını içeren Conda ortama kullanmak için aşağıdaki komutlardan birini kullanın:

    * Üzerinde __Ubuntu__ DSVM, bu komutu kullanın:

        ```shell
        conda activate py36
        ```

    * Üzerinde __Windows__ DSVM, bu komutu kullanın:

        ```shell
        conda activate AzureML
        ```

1. SDK'sı erişmek ve sürümü denetlemek doğrulamak için aşağıdaki Python kodu kullanın:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. DSVM Azure Machine Learning hizmeti çalışma alanınızla kullanmak üzere yapılandırmak için bkz [çalışma yapılandırın](#workspace) bölümü.

Veri bilimi sanal makineler üzerinde daha fazla bilgi için [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Yerel bilgisayar

(Bu da uzak bir sanal makine olabilir) yerel bir bilgisayara kullanırken conda ortamı oluşturun ve SDK'yı yüklemek için aşağıdaki adımları kullanın:

1. Bir komut istemi veya kabuk açın.

1. Aşağıdaki komutlarla conda ortam oluşturun:

    ```shell
    # create a new conda environment with Python 3.6, numpy, and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # On Mac OS run
    source activate myenv
    ```

    İşlem ortamı Python 3.6 ve diğer bileşenlerin indirilmesi gerekiyorsa oluşturulması birkaç dakika sürebilir.

1. Not defteri ek özellikler ile Azure Machine Learning SDK'sı ve veri hazırlık SDK'sı aşağıdaki komutu kullanarak yükleyin:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   > [!NOTE]
   > Bir ileti alırsanız `PyYAML` olamaz kaldırılmış, aşağıdaki komutu kullanın:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Bu SDK'yı yüklemek için birkaç dakika sürebilir.

1. Makine öğrenimi denemesi paketleri yükleyin. Aşağıdaki komutu kullanın ve Değiştir `<new package>` yüklemek istediğiniz paket:

    ```shell
    conda install <new package>
    ```

1. SDK'sı olduğunu doğrulamak için yüklü, aşağıdaki Python kodu:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter Not Defterleri

Jupyter not defterleri parçası olan [Jupyter proje](https://jupyter.org/). Bunlar, Canlı kod anlatım metin ve grafikleri ile karışık belgeleri oluşturmak burada etkileşimli bir kodlama deneyimi sunar. Çıkış, kod bölümlerinin belgenin içine kaydetmek gibi Jupyter not defterleri de sonuçlarınızı başkalarıyla paylaşmak için harika bir yoludur. Jupyter not defterleri çeşitli platformlarda yükleyebilirsiniz.

Adımları [yerel bilgisayar](#local) bölümde, Jupyter not defterleri için isteğe bağlı bileşenlerini yükler. Bu bileşenler, Jupyter not defteri ortamınızda etkinleştirmek için aşağıdaki adımları kullanın:

1. Bir komut istemi veya kabuk açın.

1. Aşağıdaki komutu kullanarak conda kullanan bir Jupyter not defteri sunucusu yüklemek için:

    ```shell
    # install Jupyter
    conda install nb_conda
    ```

1. Jupyter not defterine aşağıdaki komutla açın:

    ```shell
    jupyter notebook
    ```

1. Jupyter not defteri SDK kullanabileceğinizi doğrulamak için yeni bir not defterini açın ve "myenv", çekirdek seçin. Ardından, Not Defteri hücreye aşağıdaki komutu çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Azure Machine Learning hizmeti çalışma alanınızla kullanmak üzere Jupyter not defterini yapılandırmak için bkz [çalışma yapılandırın](#workspace) bölümü.

### <a id="vscode"></a>Visual Studio kodu

Visual Studio Code, platformlar arası kod düzenleyicisidir. Python desteği için yerel bir Python 3 ve Conda yükleme kullanır, ancak yapay ZEKA ile çalışmak için ek araçlar sağlar. Kod Düzenleyicisi içinde Conda ortamından seçmek için desteği de sağlar.

Geliştirme için Visual Studio Code'u kullanmak için aşağıdaki adımları kullanın:

1. Visual Studio Code için Python geliştirme kullanmayı öğrenmek için bkz. [VSCode Python'da başlama](https://code.visualstudio.com/docs/python/python-tutorial) belge.

1. Conda ortam seçmek için VS Code açın ve ardından __Ctrl-Shift-P__ (Linux ve Windows) veya __komut SHIFT P__ almak için (Mac) __komut paleti__. Girin __Python: yorumlayıcı seçin__ ve ardından conda ortamı seçin.

1. SDK'sını kullanabilirsiniz doğrulamak için aşağıdaki kodu içeren yeni bir Python dosyası (.py) oluşturun. Ardından dosyayı çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Visual Studio Code için Azure Machine Learning uzantıyı yüklemek için bkz: [yapay ZEKA Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) sayfası.

    Daha fazla bilgi için [Visual Studio Code için Azure Machine Learning kullanarak](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Azure Databricks için uçtan uca özel machine learning için Azure Machine Learning SDK'sı, özel bir sürümünü kullanabilirsiniz. Veya, Databricks ve kullanım modelinizi eğitmek [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code) model dağıtma

Databricks kümenizi hazırlamak ve örnek not defterleri edinmek için:

1. Oluşturma bir [Databricks kümesine](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) 4.x (yüksek eşzamanlılık tercih edilir) bir Databricks çalışma zamanı sürümü ile birlikte **Python 3**. 

1. Kitaplığa oluşturma [yükleme ve ekleme](https://docs.databricks.com/user-guide/libraries.html#create-a-library) Python için Azure Machine Learning SDK'sı `azureml-sdk[databricks]` kümenize Pypı paket. İşiniz bittiğinde bu görüntüde gösterildiği gibi bağlı kitaplığı bakın. Bu unutmayın [Databricks sorunların ortak](resource-known-issues.md#databricks).

   ![Databricks üzerinde yüklü SDK'sı ](./media/how-to-azure-machine-learning-on-databricks/sdk-installed-on-databricks.jpg)

   Bu adım başarısız olursa, kümenizi yeniden başlatın:
   1. Seçin `Clusters`sol bölmesinde. Küme adınızı tabloda seçin. 
   1. Üzerinde `Libraries` sekmesinde `Restart`.

1. İndirme [Azure Databricks / Azure Machine Learning SDK'sı not defteri dosyası arşiv](https://github.com/Azure/MachineLearningNotebooks/blob/master/databricks/Databricks_AMLSDK_github.dbc).

   >[!Warning]
   > Birçok örnek not defterleri, Azure Machine Learning hizmeti ile kullanmak için kullanılabilir. Yalnızca bu örnek not defterleri ile Azure Databricks çalışma: https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks

1.  [Bu arşiv dosyasını içeri aktarma](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) , Databricks cluster ve olarak keşfetmeye başlayın [burada açıklanan](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks).


## <a id="workspace"></a>Çalışma alanı yapılandırma dosyası oluşturma

Çalışma alanı yapılandırma dosyası, SDK, Azure Machine Learning hizmeti çalışma alanı ile iletişim kurmak nasıl paylaşılacağını açıklayan bir JSON belgesi ' dir. Dosyanın nasıl adlandırıldığı `config.json` ve aşağıdaki biçime sahiptir:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Bu dosya, Python betikleri veya Jupyter not defterleri içeren dizin yapısına olmalıdır. Ya da aynı dizinde, adlı bir alt dizinde olabilir `aml_config`, veya bir üst dizin.

Bu dosyadan kodunuzu kullanmak için `ws=Workspace.from_config()`. Bu kod, bilgileri bir dosyadan yükler ve çalışma alanınıza bağlanır.

Yapılandırma dosyası oluşturmak için üç yolu vardır:

* İzlerseniz [Azure Machine Learning hızlı](quickstart-get-started.md), `config.json` Azure not defterleri kitaplığınızda dosyası oluşturulur. Bu dosya, çalışma alanınız için yapılandırma bilgilerini içerir. İndirin veya kopyalayın `config.json` diğer geliştirme ortamlarıyla.

* Yapabilecekleriniz **el ile dosya oluşturma** bir metin düzenleyicisi kullanarak. Çalışma alanınızda ziyaret ederek yapılandırma dosyasına gidin, değerleri bulabilirsiniz [Azure portalında](https://portal.azure.com). Kopyalama __çalışma alanı adı__, __kaynak grubu__, ve __abonelik kimliği__ değerleri ve bunları yapılandırma dosyasında kullanın.
        ![Azure portal](./media/how-to-configure-environment/configure.png)

* Yapabilecekleriniz **program aracılığıyla dosya oluşturma**. Aşağıdaki kod parçacığı, abonelik kimliği, kaynak grubu ve çalışma alanı adı sağlayarak çalışma alanına na nasıl bağlanacağınız gösterilmiştir. Ardından çalışma alanı yapılandırması dosyasına kaydeder:

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

    Bu kod yapılandırma dosyasına yazar `aml_config/config.json` dosya.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Azure Machine Learning modelini MNIST veri kümesi ile eğitme](tutorial-train-models-with-aml.md)
- [Azure Machine için Python SDK'sı Learning](https://aka.ms/aml-sdk)
- [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)
