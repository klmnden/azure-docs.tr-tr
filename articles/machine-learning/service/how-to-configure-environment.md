---
title: Azure Machine Learning için bir geliştirme ortamı yapılandırma | Microsoft Docs
description: Azure Machine Learning hizmeti ile çalışırken, bir geliştirme ortamı yapılandırmayı öğrenin. Bu belgede, Conda ortamları kullanma, yapılandırma dosyalarını oluşturma ve Jupyter not defterleri, Azure not defterleri, IDE, Kod Düzenleyicisi ve veri bilimi sanal makinesi yapılandırma hakkında bilgi edinin.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 8/6/2018
ms.openlocfilehash: 73cc346e882acab1c2c00cc49738a388927d3ccf
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248253"
---
# <a name="configure-a-development-environment-for-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için bir geliştirme ortamı yapılandırma

Geliştirme ortamınızı Azure Machine Learning hizmeti ile çalışacak şekilde yapılandırmayı öğrenin. Ortamınızı bir Azure Machine Learning hizmeti çalışma alanı ile ilişkilendiren bir yapılandırma dosyasının nasıl oluşturulacağını öğreneceksiniz. Ayrıca aşağıdaki geliştirme ortamlarını yapılandırmak öğreneceksiniz:

* Kendi bilgisayarınıza Jupyter Not Defterleri
* Visual Studio Code
* Tercih ettiğiniz Kod Düzenleyicisi

Continuum Anaconda kullanmak için önerilen yaklaşımdır [sanal conda ortamları](https://conda.io/docs/user-guide/tasks/manage-environments.html) paketler arasında bağımlılık çakışmalarını önlemek için çalışma ortamınıza yalıtmak için. Bu makalede, bir conda ortamını ayarlama ve Azure Machine Learning için kullanma adımları gösterilmektedir.


## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Oluşturmak için adımları kullanın. [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) belge.

* [Continuum Anaconda](https://www.anaconda.com/download/) veya [Miniconda](https://conda.io/miniconda.html) Paket Yöneticisi.

 * Visual Studio Code ortamınıza [Python uzantısı](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> Bu belgede kullanılan Kabuk komutları, Linux ve macOS üzerinde bash ile birlikte test edilir. Ayrıca, komutlar Windows cmd.exe'de ile test edilmez.

## <a name="create-workspace-configuration-file"></a>Çalışma alanı yapılandırma dosyası oluşturma

Çalışma alanı yapılandırma dosyası, Azure Machine Learning hizmeti çalışma alanı ile iletişim kurmak için SDK'sı tarafından kullanılır.  Bu dosya almanın iki yolu vardır:

* Tamamlamak [hızlı](quickstart-get-started.md) bir çalışma alanı ve yapılandırma dosyası oluşturun. Dosya `config.json` Azure not defterlerinde sizin için oluşturulur.  Bu dosya, çalışma alanınız için yapılandırma bilgilerini içerir.  İndirme veya betikleri veya ona başvuran not defterleri ile aynı dizine kopyalayın.


* Yapılandırma dosyasını kendiniz adımları izleyerek oluşturun:

    1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com). Kopyalama __çalışma alanı adı__, __kaynak grubu__, ve __abonelik kimliği__. Bu değerler, yapılandırma dosyası oluşturmak için kullanılır.

        ![Azure portal](./media/how-to-configure-environment/configure.png) 
    
    1. Dosyayı bu Python kodu oluşturun. Betikleri veya çalışma alanına başvuru not defterlerini aynı dizinde kodu çalıştırın:

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
        Bu aşağıdaki Yazar `aml_config/config.json` dosyası: 
    
        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Kopyalayabilirsiniz `aml_config` dizin veya yalnızca `config.json` dosyasına çalışma başvuran başka bir dizin.

>[!NOTE] 
>Diğer betikleri ya da aynı dizinde veya aşağıda not defterleri ile çalışma yükler `ws=Workspace.from_config()`

## <a name="azure-notebooks-and-data-science-virtual-machine"></a>Azure not defterleri ve veri bilimi sanal makinesi

Azure Machine Learning hizmeti ile çalışacak şekilde önceden yapılandırılmış Azure not defterleri ve Azure veri bilimi sanal makineleri (DSVM). Azure Machine Learning SDK'yı gibi gerekli bileşenler bu ortamda önceden yüklenmiş durumdadır.

Azure not defterleri, Azure bulutunda bir Jupyter not defteri hizmetidir. Veri bilimi sanal makinesi için veri bilimi iş önceden yapılandırılmış olan bir sanal makine görüntüsüdür. VM, popüler Araçlar, Ide'leri ve Jupyter Notebook belgeleri, PyCharm ve Tensorflow gibi paketleri içerir.

Yine de bu ortamların kullanmak için bir çalışma alanı yapılandırma dosyası gerekir.

Veri bilimi sanal makineler üzerinde daha fazla bilgi için [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) belgeleri.

Azure Machine Learning hizmeti ile Azure not defterleri kullanma örneği için bkz: [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) belge.

## <a name="configure-jupyter-notebooks-on-your-own-computer"></a>Jupyter not defterleri, kendi bilgisayarınızda yapılandırma

1. Bir komut istemi veya kabuk açın.

2. Conda ortamı oluşturmak için aşağıdaki komutları kullanın:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

    Python 3.6 ve diğer bileşenlerin indirilmesi gerekebilir gibi uygulamanın ortamı oluşturulması birkaç dakika sürebilir.

3. Azure Machine Learning SDK'sı ile dizüstü ek özellikler yüklemek için aşağıdaki komutu kullanın:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl]
    ```

    > [!NOTE]
    > Bir ileti alırsanız, `PyYAML` olamaz kaldırılmış, aşağıdaki komutu kullanın:
    > 
    > `pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML`

    Uygulamanın, SDK'yı yüklemek için birkaç dakika sürebilir.

4. Paketler, machine learning denemesi için yüklemek için aşağıdaki komutu kullanın ve Değiştir `<new package>` yüklemek istediğiniz paket:

    ```shell
    conda install <new package>
    ```

5. Conda algılayan bir Jupyter not defteri sunucusu yüklemek ve (çalıştırma bilgileri görüntülemek), deneme pencere öğeleri etkinleştirmek için aşağıdaki komutları kullanın:

    ```shell
    # install Jupyter 
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

6. Jupyter not defteri başlatmak için aşağıdaki komutu kullanın:

    ```shell
    jupyter notebook
    ```

7. Yeni bir not defterini açın ve "myenv", çekirdek seçin. Ardından, Azure Machine Learning SDK'sı çalışan aşağıdaki komutla bir not defteri hücresinde yüklü olduğunu doğrulayın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Visual Studio Code'u yapılandırma

1. Bir komut istemi veya kabuk açın.

2. Conda ortamı oluşturmak için aşağıdaki komutları kullanın:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

2. Azure Machine Learning SDK'sını yüklemek için aşağıdaki komutu kullanın:
 
    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

4. Yapay ZEKA için Visual Studio code araçları yüklemek için Visual Studio Market girdisine bakın [yapay ZEKA Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai). 

5. Paketler, machine learning denemesi için yüklemek için aşağıdaki komutu kullanın ve Değiştir `<new package>` yüklemek istediğiniz paket:

    ```shell
    conda install <new package>
    ```

6. Visual Studio Code'u başlatın ve ardından __CTRL-SHIFT-P__ almak için __komut paleti__. Girin *Python: yorumlayıcı seçin*ve oluşturduğunuz conda ortamı seçin.

    > [!NOTE]
    > Visual Studio Code conda ortamları bilgisayarınızda otomatik olarak farkındadır. Daha fazla bilgi için [Visual Studio code belgeleri](https://code.visualstudio.com/docs/python/environments#_conda-environments).

7. Yapılandırmayı doğrulamak için aşağıdaki kod ile yeni bir Python betiği oluşturun ve sonra çalıştırmak için Visual Studio Code'u kullanın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-code-editor-of-your-choice"></a>Tercih ettiğiniz Kod Düzenleyicisi'ni yapılandırma

Azure Machine Learning SDK'sı ile bir özel Kod Düzenleyicisi'ni kullanmak için öncelikle conda ortam yukarıda açıklandığı gibi oluşturun. Her Düzenleyicisi conda ortamı için yönergeleri izleyin. Örneğin, PyCharm yönergeleri şu adreste bulunabilir [ https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html ](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).
 
## <a name="next-steps"></a>Sonraki adımlar

* [Bir Azure Machine Learning modelini MNIST veri kümesi ile eğitme](tutorial-train-models-with-aml.md)
