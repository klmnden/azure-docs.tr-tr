---
title: "Azure Machine Learning çalışma ekranı içinde Jupyter not defterlerini kullanma | Microsoft Docs"
description: "Azure Machine Learning çalışma ekranının Jupyter not defterleri özelliğini kullanmak için kılavuz"
services: machine-learning
author: jopela
ms.author: jopela
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 93850a7c9e3d9d69b0da22ebd0656ae40cee2e63
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="how-to-use-jupyter-notebook-in-azure-machine-learning-workbench"></a>Azure Machine Learning çalışma Jupyter Not Defteri kullanma

Azure Machine Learning çalışma ekranı etkileşimli veri bilimi deneme Jupyter Not Defteri, kendi tümleştirme yoluyla destekler. Bu makalede, oranı ve etkileşimli veri bilimi deneme kalitesini artırmak için bu özellik etkili kullanımını sağlamak açıklar.

## <a name="prerequisites"></a>Ön koşullar
- [Yükleme ve Azure Machine Learning oluşturma](/machine-learning/preview/quickstart-installation.md).
- Tanımanız [Jupyter not defteri](http://jupyter.org/), bu makalede Jupyter kullanmayı öğretmek hakkında olmadığından.

## <a name="jupyter-notebook-architecture"></a>Jupyter not defteri mimarisi
Yüksek bir düzeyde Jupyter not defteri mimarisi üç bileşeni içerir, her farklı işlem ortamlarda çalıştırabilirsiniz:

- **İstemci**: kullanıcı giriş ve çıkış çizilir görüntüler alır
- **Sunucu**: Not defteri dosyaları (.ipynb) barındıran web sunucusu
- **Çekirdek**: Burada dizüstü bilgisayar hücreleri gerçek yürütülmesi olur çalışma zamanı ortamı

Daha fazla ayrıntı için lütfen resmi başvuru [Jupyter belgelerine](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Fowllowing nasıl bu istemci, sunucu ve çekirdek mimarisi Azure ML bileşenlerinde eşlemek gösteren bir diyagramıdır.

![Not Defteri mimarisi](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-ml-workbench-notebook"></a>Azure ML çalışma ekranı not defterlerinde çekirdekler
Birçok erişmek için Azure ML çalışma ekranındaki tarafından farklı tekrar sadece çalıştırma yapılandırmaları yapılandırın ve hedeflerin işlem `aml_config` projenizdeki klasöre. Yeni bir işlem hedef vererek ekleme `az ml computetarget attach` komut yeni bir çekirdek ekleme aynıdır.

>[!NOTE]
>Gözden geçirme [yapılandırma yürütme](experimentation-service-configuration.md) hakkında daha fazla ayrıntı yapılandırmaları çalıştırır ve hedefleri işlem.

### <a name="kernel-naming-convention"></a>Çekirdek adlandırma kuralı
Tekrar genellikle biçiminde adlandırılmış "\<proje adı > \<config çalıştırmasını >". Örneğin, adlı çalışma bir yapılandırmanız varsa _docker python_ adlı bir projede _myIris_, Jupyter not defteri açtığınızda "myIris docker-python" çekirdek listesinde adlı bir çekirdek bulabilirsiniz.

Şu anda, çalışma ekranı tekrar aşağıdaki türlerini destekler.

### <a name="local-python-kernel"></a>Yerel Python çekirdek
Bu Python çekirdek yürütme yerel makinede destekler. Bu tümleşik Azure Machine Learning'in çalıştırma geçmişi desteği. Çekirdek genellikle "my_project_name yerel" adıdır.

### <a name="python-kernel-in-docker-local-or-remote"></a>Python çekirdek Docker (yerel veya uzak)
Bu Python çekirdek yerel makinenizde ya da uzak bir Linux VM Docker kapsayıcısı içinde çalışır. Çekirdek genellikle "my_project docker" adıdır. İlişkili `docker.runconfig` dosyasının `Framework` alan kümesi'ne `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>Docker (yerel veya uzak) PySpark Çekirdeği
Bu PySpark çekirdeği komut dosyaları Docker kapsayıcısı, yerel makinenize, veya uzak bir Linux VM içinde çalışan Spark bağlamda yürütür. Çekirdek genellikle "my_project docker" adıdır. İlişkili `docker.runconfig` dosyasının `Framework` alan kümesi'ne `PySpark`.

### <a name="pyspark-kernel-on-hdinsight-cluster"></a>Hdınsight kümesinde PySpark Çekirdeği
Bu çekirdek projeniz için bir işlem hedefi olarak bağlı uzak Hdınsight kümesi çalıştırır. Çekirdek genellikle "my_project my_hdi" adıdır. 

>[!IMPORTANT]
>İçinde `.compute` HDI dosyasının işlem hedef, değiştirmeniz gerekir `yarnDeployMode` alanı `client` (varsayılan değer `cluster`) bu çekirdek kullanmak için. 

## <a name="start-jupyter-server-from-the-workbench"></a>Başlangıç Jupyter sunucudan çalışma ekranı
Azure Machine Learning çalışma ekranından çalışma ekranı kişinin not defterlerini erişilebilir **not defterlerini** sekmesi. _Sınıflandırma Iris_ örnek proje içeren bir `iris.ipynb` örnek dizüstü bilgisayar.

![not defterlerini sekmesi](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Bir not defteri Azure Machine Learning çalışma açıldığında, kendi belge sekmesinde görüntülenir **önizleme modunda**. Bu bir Jupyter server çalıştıran ve çekirdek gerektirmeyen salt okunur bir görünümdür.

![Not Defteri Önizleme](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Tıklatarak **Start not defteri Server** düğmesini Jupyter sunucusu başlatan ve not defterinize geçer **düzenleme modu**. Tanıdık Jupyter not defteri kullanıcı arabirimi içinde çalışma ekranı katıştırılmış görüntülenir. Artık bir çekirdekten ayarlayabilirsiniz **çekirdek** menü ve etkileşimli dizüstü bilgisayar oturumunuzu başlatın. 

>[!NOTE]
>Not yerel olmayan çekirdekleri için bir veya iki ilk kez kullanıyorsanız başlatmak için dakika sürebilir. Yürütebilirsiniz `az ml experiment prepare` işlem hedef hazırlandıktan sonra çekirdek çok daha hızlı başlatabilmeniz işlem hedef hazırlamak için CLI penceresinden komutu.

![Düzenleme modu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Bir tam olarak etkileşimli Jupyter not defteri deneyimi budur. Çalışma ekranı yapılabilir bu yana tüm normal dizüstü bilgisayar işlemlerini ve klavye kısayolları bazı dosya işlemleri hariç olmak üzere bu pencereden desteklenen **not defterlerini** sekmesi ve **dosya** sekmesi.

## <a name="start-jupyter-server-from-command-line"></a>Komut satırından Jupyter sunucu Başlat
Vererek bir not defteri oturumu başlatabilir bir `az ml notebook start` komut satırı penceresinde:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Varsayılan tarayıcınız proje giriş dizinine işaret eden Jupyter sunucusuyla otomatik olarak başlatılır. Diğer tarayıcı pencerelerini yerel olarak başlatmak için URL ve CLI penceresinde görüntülenen belirteci kullanabilirsiniz. 

![Proje Panosu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Şimdi tıklatabilirsiniz bir `.ipynb` not defteri dosyasını açmak ve çekirdek (ayarlanmamışsa) ayarlayın ve etkileşimli oturumunuzu başlatın.

![Proje Panosu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="next-steps"></a>Sonraki Adımlar
- Jupyter not defteri kullanmayı öğrenmek için ziyaret edin [Jupyter resmi belge](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Azure ML deneme Yürütme Ortamı'nın daha derinden anlayabilmek için gözden [Azure Machine Learning genel bakış deneme hizmeti](experimentation-service-configuration.md)

