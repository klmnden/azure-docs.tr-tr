---
title: Azure Machine Learning için bir geliştirme ortamı yapılandırma | Microsoft Docs
description: Azure Machine Learning hizmeti ile çalışırken, bir geliştirme ortamı yapılandırmayı öğrenin. Bu belgede, Conda ortamları kullanma, yapılandırma dosyalarını oluşturma ve Jupyter not defterleri, Azure not defterleri, IDE, Kod Düzenleyicisi ve veri bilimi sanal makinesi yapılandırma hakkında bilgi edinin.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 11/6/2018
ms.openlocfilehash: 2fd2d35bde95a3e268f46b398f2163f9d40ab1ee
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613962"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı yapılandırma

Bu belgede, bir geliştirme ortamı, Azure Machine Learning hizmeti ile çalışacak şekilde yapılandırma konusunda bilgi edinin. Platform bağımsızlığı Azure Machine Learning hizmetidir. Geliştirme ortamınız için tek gerekenlerdir __Python 3__, __Conda__ (için Yalıtılmış ortamlarda) ve Azure Machine Learning çalışma alanı bilgilerinizi içeren bir yapılandırma dosyası.

Bu belge aşağıdaki belirli ortamlara ve araçları odaklanır:

* [Azure not defterleri](#aznotebooks): Azure bulutunda barındırılan A Jupyter Notebook hizmeti. Bu __kolay__ başlama, Azure Machine Learning SDK'sı zaten yüklü olduğu yolu.
* [Veri bilimi sanal makinesi](#dsvm): bir sanal makine, Azure bulutunda __veri bilimi iş için tasarlanmış__. Python 3, Conda, Jupyter not defterleri ve Azure Machine Learning SDK'sı zaten yüklü. VM, popüler ML çerçeveleri, Araçlar ve ML çözümleri geliştirme için düzenleyicileri ile birlikte gelir. Büyük olasılıkla olan __en eksiksiz__ ML için Azure platformunda geliştirme ortamı.
* [Jupyter not defterleri](#jupyter): Jupyter not defterleri zaten kullanıyorsanız, SDK'yı yüklemeniz bazı ek özellikler vardır.
* [Visual Studio Code](#vscode): Visual Studio Code kullanırsanız, yüklemek için kullanabileceğiniz bazı kullanışlı uzantılar vardır.

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

Veri bilimi sanal makinesi (DSVM) bir özelleştirilmiş sanal makine (VM) görüntüsüdür **veri bilimi iş için tasarlanmış**. Aşağıdakileri içerir:

  - Popüler veri bilimi araçları
  - PyCharm ve RStudio gibi tümleşik geliştirme ortamlarından (IDE'ler)
  - Jupyter Not defterlerinden ve Tensorflow gibi paketleri

Azure Machine Learning SDK'sı, Windows veya Ubuntu DSVM sürümünde çalışır. DSVM bir geliştirme ortamı olarak kullanmak için aşağıdaki adımları izleyin:

1. Bir veri bilimi sanal makinesi oluşturmak için aşağıdaki belgelere birindeki adımları izleyin:

    * [Bir Ubuntu veri bilimi sanal makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)
    * [Windows veri bilimi sanal makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

1. Azure Machine Learning SDK'sı **yüklü** DSVM üzerinde. SDK'sını içeren Conda ortama kullanmak için aşağıdaki komutlardan birini kullanın:

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

1. Conda algılayan bir Jupyter not defteri sunucusu yüklemek ve deneme pencere öğeleri etkinleştirmek için aşağıdaki komutları kullanın:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
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

1. Yapay ZEKA uzantısı için Visual Studio code araçları yüklemek için bkz [yapay ZEKA Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) sayfası.

    Daha fazla bilgi için [AI ile Azure Machine Learning için kullanım VS Code Araçları](how-to-vscode-tools.md) belge.

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