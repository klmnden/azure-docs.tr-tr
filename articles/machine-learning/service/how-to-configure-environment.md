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
ms.date: 8/6/2018
ms.openlocfilehash: 657a762874f7c2fb40553552ef6c17d9b5b6da0f
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958627"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı yapılandırma

Bu makalede bir geliştirme ortamı, Azure Machine Learning hizmeti ile çalışacak şekilde yapılandırmak öğretir dahil olmak üzere:

- Ortamınızı bir Azure Machine Learning hizmeti çalışma alanı ile ilişkilendiren bir yapılandırma dosyası oluşturma
- Aşağıdaki geliştirme ortamlarını yapılandırmak nasıl:
  - Bilgisayarınızda Jupyter Not Defterleri
  - Visual Studio Code
  - Özel Kod Düzenleyicisi
- Nasıl yapılır bir [conda sanal ortam](https://conda.io/docs/user-guide/tasks/manage-environments.html) ve Azure Machine Learning için kullanın. Continuum Anaconda arasında paketleri bağımlılık çakışmaları önlemek için çalışma ortamınıza ayırmak için kullanmanızı öneririz.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning hizmeti çalışma alanı ayarlayın. Bağlantısındaki [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md).
- Ya da yükleme [Continuum Anaconda](https://www.anaconda.com/download/) veya [Miniconda](https://conda.io/miniconda.html) Paket Yöneticisi.
- Visual Studio Code kullanıyorsanız alma [Python uzantısı](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> Bash'te (Linux ve Mac OS) kullanarak bu makalede gösterilen Kabuk komutları test edebilir veya komut satırında (Windows).

## <a name="create-a-workspace-configuration-file"></a>Çalışma alanı yapılandırma dosyası oluşturma

Azure Machine Learning SDK'sı, Azure Machine Learning hizmeti çalışma alanı ile iletişim kurmak için çalışma alanı yapılandırma dosyasını kullanır.

- Yapılandırma dosyası oluşturmak için tamamlayın [Azure Machine Learning hızlı](quickstart-get-started.md).
  - Hızlı Başlangıç işlemi oluşturur bir `config.json` Azure not defterleri dosyasında. Bu dosya, çalışma alanınız için yapılandırma bilgilerini içerir.
  - İndirme veya kopyalama `config.json` betikleri veya ona başvuran not defterleri ile aynı dizine.

- Alternatif olarak, aşağıdaki adımları izleyerek dosyayı el ile oluşturabilirsiniz:

    1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com). Kopyalama __çalışma alanı adı__, __kaynak grubu__, ve __abonelik kimliği__. Bu değerler, yapılandırma dosyası oluşturmak için kullanılır.
        ![Azure portal](./media/how-to-configure-environment/configure.png)

    1. Aşağıdaki Python kod ile dosya oluşturun ve komut dosyaları veya çalışma alanına başvuru not defterleri ile aynı dizinde kod çalıştırıldığından emin olun:

        ```python
        from azureml.core import Workspace

        subscription_id ='<subscription-id>'
        resource_group ='<resource-group>'
        workspace_name = '<workspace-name>'

        try:
           ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
           ws.write_config()
           print('Library configuration succeeded')
        except:
           print('Workspace not found')
        ```
        Aşağıdaki kod yazar `aml_config/config.json` dosyası:

        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Kopyalayabilirsiniz `aml_config` dizin veya yalnızca `config.json` dosyasına çalışma başvuran başka bir dizin.

       > [!NOTE]
       > Diğer betikleri ya da aynı dizinde veya aşağıdaki Not Defterleri ile çalışma yük `ws=Workspace.from_config()`.

## <a name="azure-notebooks-and-data-science-virtual-machines"></a>Azure not defterleri ve veri bilimi sanal makineleri

Azure not defterleri ve Azure veri bilimi sanal makineleri (Dsvm'leri) Azure Machine Learning hizmeti ile çalışacak şekilde yapılandırılmış olarak sunulur. Bu ortamların Azure Machine Learning SDK'sı gibi gerekli bileşenleri içerir.

- Azure not defterleri, Azure bulutunda bir Jupyter not defteri hizmetidir.
- Veri bilimi sanal makinesi, veri bilimi iş için tasarlanmış bir özelleştirilmiş sanal makine (VM) görüntüsüdür. Aşağıdakileri içerir:
  - Popüler Araçlar
  - Tümleşik geliştirme ortamlarından (IDE'ler)
  - Jupyter Notebook belgeleri, PyCharm ve Tensorflow gibi paketleri
- Yine de bu ortamların kullanmak için bir çalışma alanı yapılandırma dosyası gerekir.

Azure Machine Learning hizmeti ile Azure not defterleri kullanma örneği için bkz: [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md).

Veri bilimi sanal makineler üzerinde daha fazla bilgi için [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="configure-jupyter-notebooks-on-your-computer"></a>Jupyter not defterleri bilgisayarınızda yapılandırma

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

1. Conda algılayan bir Jupyter not defteri sunucusu yüklemek ve (çalıştırma bilgileri görüntülemek), deneme pencere öğeleri etkinleştirin. Aşağıdaki komutları kullanın:

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

1. Yeni bir not defterini açın, "myenv", çekirdek seçin ve ardından, Azure Machine Learning SDK'ın yüklü olduğunu doğrulayın. Bir not defteri hücreye aşağıdaki komutu çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Visual Studio Code'u yapılandırma

1. Bir komut istemi veya kabuk açın.

1. Aşağıdaki komutlarla conda ortam oluşturun:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

1. Veri hazırlama SDK'sını ve Azure Machine Learning SDK'sı şu komutla yükleyin:

    ```shell
    pip install --upgrade azureml-sdk[automl] azureml-dataprep
    ```

1. Yapay ZEKA uzantısı için Visual Studio code araçları yükleyin. Bkz: [yapay ZEKA Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

1. Makine öğrenimi denemesi paketleri yükleyin. Aşağıdaki komut, değiştirme `<new package>` yüklemek istediğiniz paket:

    ```shell
    conda install <new package>
    ```

1. Visual Studio Code'u açın ve ardından **CTRL-SHIFT-P** (Windows içinde) veya **komut SHIFT P** (Mac OS'ta) almak için **komut paleti**. Girin _Python: yorumlayıcı seçin_ ve oluşturduğunuz conda ortamı seçin.

   > [!NOTE]
   > Visual Studio Code conda ortamları bilgisayarınızda otomatik olarak farkındadır. Daha fazla bilgi için [Visual Studio code belgeleri](https://code.visualstudio.com/docs/python/environments#_conda-environments).

1. Aşağıdaki kod ile yeni bir Python betiği oluşturun ve sonra çalıştırmak için Visual Studio Code kullanarak yapılandırmayı doğrula:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-a-custom-code-editor"></a>Bir özel Kod Düzenleyicisi'ni yapılandırma

Azure Machine Learning SDK ile kendi tercih ettiğiniz bir kod düzenleyicisi kullanabilirsiniz.

1. Conda ortamınızı 2 adımda açıklandığı gibi oluşturmak [yapılandırma Visual Studio Code](#configure-visual-studio-code) yukarıda.
1. Her Düzenleyicisi conda ortamı için yönergeleri izleyin. Örneğin, izleyebilirsiniz [PyCharm yönergeleri](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Azure Machine Learning modelini MNIST veri kümesi ile eğitme](tutorial-train-models-with-aml.md)
