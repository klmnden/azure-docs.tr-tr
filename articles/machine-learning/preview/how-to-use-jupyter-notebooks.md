---
title: "Azure Machine Learning çalışma Jupyter not defterlerini kullanma | Microsoft Docs"
description: "Azure Machine Learning çalışma ekranının Jupyter not defteri özelliğini kullanmak için bir kılavuz"
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: c21b7096f689efedacd6e7d55d83912d35dff803
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Azure Machine Learning çalışma ekranı içinde Jupyter not defterlerini kullanma

Azure Machine Learning çalışma ekranı etkileşimli veri bilimi deneme kendi Jupyter not defterleri ile tümleştirme yoluyla destekler. Bu makalede oranı ve etkileşimli veri bilimi deneme kalitesini artırmak için bu özellik etkili kullanımını nasıl oluşturulacağı açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar
- [Azure Machine Learning hesapları oluşturun ve Azure Machine Learning çalışma ekranı yükleme](quickstart-installation.md).
- Tanımanız [Jupyter not defteri](http://jupyter.org/). Bu makale Jupyter kullanmayı öğrenme ilgili değildir.

## <a name="jupyter-notebook-architecture"></a>Jupyter not defteri mimarisi
Yüksek bir düzeyde Jupyter not defteri mimarisi üç bileşeni içerir. Her farklı işlem ortamlarda çalıştırabilirsiniz:

- **İstemci**: kullanıcı giriş ve çıkış çizilir görüntüler alır.
- **Sunucu**: Not defteri dosyaları (.ipynb) barındıran Web sunucusu.
- **Çekirdek**: çalışma zamanı ortamı not defteri hangi yürütülmesi hücreleri olur.

Daha fazla bilgi için bkz: resmi [Jupyter belgelerine](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Aşağıdaki diyagram, Azure Machine Learning bileşenlerinde nasıl bu istemci, sunucu ve çekirdek mimarisi eşlendiği gösterir:

![Jupyter not defteri mimarisi](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Azure Machine Learning çalışma ekranı not defterlerini tekrar
Azure Machine Learning çalışma ekranındaki farklı tekrar çalıştırma yapılandırmaları tanımlayarak erişmek ve hedeflerin işlem `aml_config` projenizdeki klasöre. Yeni bir işlem hedef vererek ekleme `az ml computetarget attach` komut yeni bir çekirdek ekleme aynıdır.

>[!NOTE]
>Gözden geçirme [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md) hakkında daha fazla ayrıntı yapılandırmaları çalıştırır ve hedefleri işlem.

### <a name="kernel-naming-convention"></a>Çekirdek adlandırma kuralı
Azure Machine Learning çalışma ekranı özel Jupyter tekrar oluşturur. Bu tekrar adlı  *\<proje adı > \<config çalıştırmasını >*. Örneğin, adlı çalışma bir yapılandırmanız varsa _docker python_ adlı bir projede _myIris_, Azure Machine Learning bir çekirdek adlı kullanılabilir yapar *myIris docker-python.* Jupyter not defteri çalışan çekirdek ayarladığınız **çekirdek** menüsü, **değişiklik çekirdek** alt. Çalışan çekirdek adını sağ ucundaki menü çubuğu üzerinde görüntülenir.
 
Şu anda, Azure Machine Learning çalışma ekranı tekrar aşağıdaki türlerini destekler.

### <a name="local-python-kernel"></a>Yerel Python çekirdek
Bu Python çekirdek yürütme yerel makine üzerinde destekler. Bu tümleşik Azure Machine Learning çalıştırma geçmişi desteği. Çekirdek genellikle adıdır *my_project_name yerel.*

>[!NOTE]
>Python 3 Çekirdek kullanmayın. Varsayılan olarak Jupyter tarafından sağlanan bir tek başına çekirdeğidir ve Azure Machine Learning özellikleriyle tümleşik değildir. Örneğin, `%azureml` Jupyter Sihirli işlevler "bulunamadı" hatalarını döndürür. 

### <a name="python-kernel-in-docker-local-or-remote"></a>Python çekirdek Docker (yerel veya uzak)
Bu Python çekirdek Docker kapsayıcısı içinde yerel makinenizde veya uzak bir Linux sanal makine (VM) çalışır. Çekirdek genellikle adıdır *my_project docker.* İlişkili `docker.runconfig` dosyasının `Framework` alan kümesi'ne `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>Docker (yerel veya uzak) PySpark Çekirdeği
Bu PySpark çekirdeği komut dosyaları Docker kapsayıcısı yerel makinenizde veya uzak bir Linux VM içinde çalışan Spark bağlamda yürütür. Çekirdek genellikle adıdır *my_project docker.* İlişkili `docker.runconfig` dosyasının `Framework` alan kümesi'ne `PySpark`.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>Azure Hdınsight kümesi PySpark Çekirdeği
Bu çekirdek projeniz için bir işlem hedefi olarak bağlı uzaktan Azure Hdınsight kümesi çalıştırır. Çekirdek genellikle adıdır *my_project my_hdi.* 

>[!IMPORTANT]
>İçinde `.compute` HDI dosyasını hedef işlem, değiştirmeniz gerekir `yarnDeployMode` alanı `client` (varsayılan değer `cluster`) bu çekirdek kullanılacak. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Azure Machine Learning çalışma ekranından bir Jupyter sunucusu Başlat
Azure Machine Learning çalışma ekranından not defterlerini aracılığıyla erişebilirsiniz **not defterlerini** sekmesi. _Sınıflandırma Iris_ örnek proje içeren bir `iris.ipynb` örnek dizüstü bilgisayar.

![not defterlerini sekmesi](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Azure Machine Learning çalışma ekranı içinde bir not defteri açtığınızda, kendi belge sekmesinde görüntülenir **önizleme modunda**. Bu bir Jupyter server çalıştıran ve çekirdek gerektirmeyen salt okunur bir görünümdür.

![Not Defteri Önizleme](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Seçme **Start not defteri Server** düğmesini Jupyter sunucu başlar ve not defterinize geçer **düzenleme modu**. Tanıdık Jupyter not defteri kullanıcı arabirimi çalışma ekranı katıştırılmış görüntülenir. Artık bir çekirdekten ayarlayabilirsiniz **çekirdek** menü ve etkileşimli dizüstü bilgisayar oturumunuzu başlatın. 

>[!NOTE]
>Yerel olmayan çekirdekleri ile bir veya iki ilk kez kullanıyorsanız başlatmak için dakika sürebilir. Yürütebilirsiniz `az ml experiment prepare` işlem hedef hazırlandıktan sonra çekirdek çok daha hızlı başlatılmasını işlem hedef hazırlamak için CLI penceresinden komutu.

![Düzenleme modu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Bu tamamen etkileşimli bir Jupyter not defteri deneyimidir. Bu pencereden, çalışma ekranı yapılabilir bazı dosya işlemleri hariç desteklenen tüm normal dizüstü bilgisayar işlemlerini ve klavye kısayolları **not defterlerini** sekmesi ve **dosya** sekmesi.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Komut satırından bir Jupyter sunucusu Başlat
Vererek bir not defteri oturum başlatabilirsiniz `az ml notebook start` komut satırı penceresinde:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
Copy and paste this URL into your browser when you connect for the first time, to login with a token: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Varsayılan tarayıcınız proje giriş dizinine işaret eden Jupyter sunucusu ile otomatik olarak açılır. Yerel olarak diğer tarayıcı pencerelerini açmak için URL ve CLI penceresinde görüntülenen belirteci kullanabilirsiniz. 

![Proje Panosu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Şimdi seçebileceğiniz bir `.ipynb` not defteri dosyasını açın, çekirdek (ayarlanmamışsa) ayarlayabilir ve etkileşimli oturumunuzu başlatın.

![Proje Panosu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Sihirli komutları denemeler yönetmek için kullanın

Kullanabileceğiniz [Sihirli komutları](http://ipython.readthedocs.io/en/stable/interactive/magics.html) çalıştırma geçmişi izlemek ve modelleri veya veri kümeleri gibi çıkışları kaydetmek için Not Defteri hücreleri içinde.

Tek tek not defteri hücre çalıştırır izlemek için `%azureml history on` Sihirli komutu. Geçmiş etkinleştirdikten sonra her hücre çalıştırmayı girişe çalıştırma geçmişi gibi görünür:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Hücre çalıştırmak izlemeyi devre dışı için kullanmak `%azureml history off` Sihirli komutu.

Kullanabileceğiniz `%azureml upload` Sihirli çalışacak durumundan modeli ve veri dosyalarını kaydetmek için komutu. Çalıştırma geçmişi görünümünde çıktı olarak kaydedilmiş nesneler görüntülenir:

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>Adlı bir klasöre çıkışları kaydedilmelidir *çıkarır.*

## <a name="next-steps"></a>Sonraki adımlar
- Jupyter not defteri kullanmayı öğrenmek için bkz: [Jupyter resmi belge](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Azure Machine Learning deneme Yürütme Ortamı'nın daha derinden anlayabilmek için bkz: [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md).

