---
title: Azure Machine Learning Workbench'te Jupyter not defterleri kullanma | Microsoft Docs
description: Azure Machine Learning Workbench, Jupyter not defteri özelliğini kullanmak için bir kılavuz
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 712cdaa65487620b2f8af4a0ad57c01c24b9a965
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651250"
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench'te Jupyter not defterleri kullanma

Azure Machine Learning Workbench, etkileşimli veri bilimi deneme Jupyter not defterleri ile tümleştirmesi aracılığıyla destekler. Bu makalede, etkileşimli veri bilimi deneme kalitesini ve hızını artırmak için bu özellik etkili kullanımını sağlamak nasıl açıklar.

## <a name="prerequisites"></a>Önkoşullar
- [Azure Machine Learning hesapları oluşturma ve Azure Machine Learning Workbench'i yükleme](../service/quickstart-installation.md).
- Tanımanız [Jupyter not defteri](http://jupyter.org/). Bu makalede Jupyter kullanmayı öğrenme hakkında değil.

## <a name="jupyter-notebook-architecture"></a>Jupyter not defteri mimarisi
Jupyter not defteri mimari, yüksek bir düzeyde üç bileşeni içerir. Her farklı işlem ortamlarında çalıştırabilirsiniz:

- **İstemci**: kullanıcı girişi ve çıkışı işlenen görüntüler alır.
- **Sunucu**: dizüstü bilgisayar dosyalarını (.ipynb) barındıran Web sunucusu.
- **Çekirdek**: çalışma zamanı ortamı not defterinin hangi yürütme hücreleri olur.

Daha fazla bilgi için bkz: resmi [Jupyter belgeleri](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Aşağıdaki diyagram, istemci ve Sunucu Çekirdeği bu mimari Azure Machine learning'de bileşenlerine eşlemelerini nasıl gösterir:

![Jupyter not defteri mimarisi](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Azure Machine Learning Workbench not defterlerinde çekirdekler
Çalıştırma yapılandırmaları tanımlayarak Azure Machine Learning Workbench uygulamasında farklı çekirdekler erişmek ve hedef işlem `aml_config` projenizdeki klasör. Yeni bir işlem hedefine göndererek ekleme `az ml computetarget attach` komut yeni bir çekirdek ekleme aynıdır.

>[!NOTE]
>Gözden geçirme [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md) yapılandırmaları Çalıştır ve hedef işlem üzerinde daha fazla ayrıntı için.

### <a name="kernel-naming-convention"></a>Çekirdek adlandırma kuralı
Azure Machine Learning Workbench özel Jupyter çekirdekleri oluşturur. Bu çekirdekler adlı  *\<proje adı > \<config çalıştırmasını >*. Örneğin, adında bir çalıştırma yapılandırma varsa _docker-python_ adlı bir projedeki _Myıris_, Azure Machine Learning yapar kullanılabilir çekirdek adlı *Myıris docker-python.* Jupyter not defteri çalışan çekirdek ayarladığınız **çekirdek** menü, **değişiklik çekirdek** alt. Sağ menü çubuğu üzerinde çalışan çekirdek adı görüntülenir.
 
Şu anda Azure Machine Learning Workbench çekirdekler aşağıdaki türlerini destekler.

### <a name="local-python-kernel"></a>Yerel Python çekirdek
Bu Python çekirdek yürütme yerel makine üzerinde destekler. Tümleşik Azure Machine Learning çalıştırma geçmişi desteği. Çekirdek genellikle adıdır *my_project_name yerel.*

>[!NOTE]
>Python 3 Çekirdek kullanmayın. Varsayılan olarak Jupyter tarafından sağlanan bir tek başına çekirdek olduğundan ve Azure Machine Learning özellikleri ile tümleşik değil. Örneğin, `%azureml` Jupyter Sihirli işlevleri "bulunamadı" hatalarını döndürür. 

### <a name="python-kernel-in-docker-local-or-remote"></a>Python çekirdek docker'da (yerel veya uzak)
Bu Python çekirdek bir Docker kapsayıcısında yerel makinenizde veya uzak bir Linux sanal makinesi (VM) çalışır. Çekirdek genellikle adıdır *my_project docker.* İlişkili `docker.runconfig` dosyasının `Framework` alan kümesine `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>PySpark çekirdeği docker'da (yerel veya uzak)
Bu PySpark çekirdeği betikleri yerel makinenizde veya uzak Linux VM'de bir Docker kapsayıcısı içinde çalışan Spark bağlamında yürütür. Çekirdek genellikle addır *my_project docker.* İlişkili `docker.runconfig` dosyasının `Framework` alan kümesine `PySpark`.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>Azure HDInsight kümesinde PySpark Çekirdeği
Bu çekirdek projeniz için bir işlem hedefi olarak bağlı uzak Azure HDInsight kümesinde çalışır. Çekirdek genellikle addır *my_project my_hdi.* 

>[!IMPORTANT]
>İçinde `.compute` HDI dosyasını, hedef işlem, değiştirmeniz gerekir `yarnDeployMode` alanı `client` (varsayılan değer `cluster`) bu çekirdek kullanılacak. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Jupyter sunucu Azure Machine Learning Workbench uygulamasını başlatın.
Azure Machine Learning Workbench'ten not defterlerini aracılığıyla erişebileceğiniz **not defterlerini** sekmesi. _Iris sınıflandırma_ örnek proje içeren bir `iris.ipynb` örnek not defteri.

![Not defterlerini sekmesi](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Azure Machine Learning Workbench'te bir not defteri açtığınızda, kendi belge sekmesini görüntüleneceğini **Önizleme modunu**. Bunu Jupyter server çalıştıran ve çekirdek gerektirmeyen salt okunur bir görünümüdür.

![Not Defteri Önizleme](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Seçme **Başlat Notebook sunucusu** düğmesi Jupyter sunucuyu başlatır ve not defterinize geçer **düzenleme modu**. Workbench'te katıştırılmış tanıdık Jupyter not defteri kullanıcı arabirimi görüntülenir. Artık bir çekirdekten ayarlayabilirsiniz **çekirdek** menü ve etkileşimli bir not defteri oturumunuzu başlatın. 

>[!NOTE]
>Yerel olmayan çekirdekleri ile veya iki ilk kez kullanıyorsanız, başlatmak için bir dakika sürebilir. Yürütebilirsiniz `az ml experiment prepare` komutu CLI penceresinden çekirdek çok daha hızlı işlem hedef hazırladıktan sonra başlar bu nedenle işlem hedefini hazırlayın.

![Düzenleme modu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Bu, tam etkileşimli bir Jupyter not defteri deneyimi sunar. Bu pencereden Workbench yapılabilir bazı dosya işlemleri hariç desteklenen tüm normal not defteri işlemleri ve klavye kısayolları **not defterlerini** sekmesi ve **dosya** sekmesi.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Komut satırından bir Jupyter sunucusu Başlat
Yayımlayarak bir not defteri oturumu da başlatabilirsiniz `az ml notebook start` komut satırı penceresinde:
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
Varsayılan tarayıcınız proje giriş dizinine işaret eden Jupyter sunucusu ile otomatik olarak açılır. Yerel olarak diğer tarayıcı pencerelerini açmak için URL'yi ve CLI penceresinde görüntülenen belirteci kullanabilirsiniz. 

![Proje Panosu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Artık seçebileceğiniz bir `.ipynb` not defteri dosyası açın (ayarlanmamışsa) çekirdek ayarlayın ve etkileşimli oturumunuzu başlatın.

![Proje Panosu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Magic komutları denemeleri yönetmek için kullanın

Kullanabileceğiniz [magic komutları](http://ipython.readthedocs.io/en/stable/interactive/magics.html) çalıştırma geçmişini izleme ve çıktıları modelleri veya veri kümeleri gibi kaydetmek için Not Defteri hücreleri içinde.

Tek bir not defteri hücre çalıştırmaları izlemek için `%azureml history on` Sihirli komutu. Geçmiş etkinleştirdikten sonra her bir hücre çalıştırmanın çalıştırma geçmişini bir girdi olarak görünür:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Çalıştırma hücre izlemeyi devre dışı için kullanın `%azureml history off` Sihirli komutu.

Kullanabileceğiniz `%azureml upload` Sihirli komutu, Çalıştır modeli ve veri dosyalarını kaydetmek için. Çalıştırma geçmişi Görünümü'nde çıktı olarak kaydedilmiş olan nesneler görüntülenir:

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>Adlı bir klasöre çıkışları kaydedilmelidir *çıkarır.*

## <a name="next-steps"></a>Sonraki adımlar
- Jupyter not defterini kullanma konusunda bilgi almak için bkz: [Jupyter resmi belgelerine](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Azure Machine Learning deneme yürütme ortamı daha derin bir anlayış kazanmak için bkz: [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md).

