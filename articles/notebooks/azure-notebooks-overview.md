---
title: Azure not defterleri genel bakış
description: Jupyter not defterleri hiçbir Kurulum veya yapılandırma gerekli olduğu ücretsiz Azure not defterleri hizmetini kullanarak bulutta çalıştırın.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 9cea5a8e-c52d-4bdc-9e4a-cecdc1ad02c1
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/05/2019
ms.author: kraigb
ms.openlocfilehash: 4840a9839fe1f2a31470d4a67b3755b82077fd90
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59280120"
---
# <a name="overview-of-azure-notebooks"></a>Azure not defterleri genel bakış

Azure Notebooks, yükleme yapmadan Jupyter not defterlerini geliştirmeye ve çalıştırmaya yönelik, ücretsiz ve barındırılan bir hizmettir. [Jupyter](https://jupyter.org/) (eski adıyla Ipython) kolayca olanak tanıyan bir açık kaynak projesi birleştirmek Markdown metin, yürütülebilir kod, kalıcı verileri, grafik ve görselleştirmeler paylaşılabilir, tek bir tuvaline olan *not defteri* (görüntü jupyter.org sayesinde):

[![Jupyter not defterleri örnekleri](https://jupyter.org/assets/jupyterpreview.png)](https://jupyter.org/assets/jupyterpreview.png#lightbox)

Jupyter kodu, grafik ve açıklayıcı metin içeren bu güçlü birleşim nedeniyle, veri bilimi yönergesi, veri temizleme ve dönüştürme, sayısal bir simülasyon, modelleme ve geliştirmeye yönelik dahil olmak üzere pek çok kullanımı için popüler hale gelmiştir Makine öğrenimi modelleri.

## <a name="hassle-free-experience"></a>Sorunsuz bir deneyim

Azure not defterleri, kullanmaya başlama hakkında hızlı bir şekilde prototip oluşturma, veri bilimi, akademik araştırmasını veya Python programa öğrenme almak için yardımcı olur:

- Bir veri Bilimcisi yükleme tam Anaconda ortamıyla anında erişebilir.
- Bir Öğretmen Öğrenciler için sorunsuz bir Python ortamı sağlar.
- Bir benzeri bir sunum verebilirsiniz konuşma veya yazılım yükleme 45 dakika harcayabileceğiniz katılımcıları isteyen olmadan Web Semineri.
- Bir geliştirici ya da hobi bir hızlı kod karalama defteri not defterlerini kullanabilirsiniz.

Kişiler üzerinde Azure Not Defterleri (önizlemede) gibi tarayıcı erişilebilir bir bulut hizmeti aracılığıyla çalışabileceğiniz yaptığınızda not defterlerini daha güçlü hale gelir. Bulutta, kullanıcıların Jupyter yerel olarak yüklemez veya kendilerini bir ortam bakımıyla ilgili. Bulut ayrıca paylaşım not defterlerini (ve ilişkili veri dosyaları) diğer yetkili kullanıcılarla not defterleri ile kaynak denetimi depoları gibi dış anlamına gelir paylaşımı zorluklar önleme kolaylaştırır. Azure not defterleri ile kullanıcılar ayrıca kopyalama (veya "kopyalama") not defterleri için değişiklik ya da yönerge amaçları için özellikle yararlıdır deneme, kendi hesabına denetleyebilirsiniz.

Azure not defterleri yazma genel bir kod olduğundan, yürütme ve platform, paylaşım, çok sayıda çeşitli senaryolar için kullanabilirsiniz:

- Yeni bir programlama dili öğrenmek – birini deneyin [frontpage öğreticiler](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb)
- Veri bilimi öğrenin: deneyin [Jake VanderPlas rehberi](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook)
- [Bir kurs öğretin](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas) yüzlerce, Öğrenciler için
- Yükleme zaman olmadan bir konferansta bir çevrimiçi olması veya bir Web seminerine verin 
- Doğrudan yüklemek ve Not Defterleri ile çalıştırmak GitHub kullanıcıların [GitHub başlatma rozeti oluşturma](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge)
- Vermek [PowerPoint gibi gösterilerini](https://notebooks.azure.com/help/jupyter-notebooks/slides) slaytları kodda olduğu yürütülebilir!

Kısacası, Azure not defterleri işlerinizi daha verimli bir şekilde gerçekleştirmek ve bu nedenle daha fazla bilgi elde etmenize yardımcı olur.

> [!Note]
> Jupyter kendisi hakkında daha fazla bilgi bulunabilir [jupyter.org](https://jupyter.org/) ve [Jupyter belgeleri](https://jupyter-notebook.readthedocs.io/en/latest/).

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve kotalar

Azure not defterleri, ücretsiz bir hizmettir ancak her proje, kötüye kullanımı önlemek için 4GB bellek ve 1GB veri için sınırlıdır. Bu sınırları aşan kullanıcıların dizüstü çalıştırmaya devam etmek için bir Captcha testini bakın.

Tüm sınırları serbest bırakmak için Azure Active Directory (bir kurumsal hesap gibi) kullanarak bir hesapla Azure not defterlerine oturum. Bu hesap bir Azure aboneliği ile ilişkili ise, bu Abonelikteki herhangi bir Azure veri bilimi sanal makinesi örneklerine bağlanabilirsiniz. Daha fazla bilgi için [yönetme ve projeleri - bilgi işlem katmanı yapılandırma](configure-manage-azure-notebooks-projects.md#compute-tier).

Not Defteri sunucuları en fazla 8 saat boyunca var garanti edilir. Çoğu durumda kapsayıcınızı bu sınıra tabi değildir ve bu süre çalışmaya devam eder, ancak uzun süreli oturumları bazen sistem kararlılığı için kapatılması.

## <a name="available-kernels-and-environments"></a>Kullanılabilen çekirdekler ve ortamları

Her not defteri için herhangi bir kod hücreleri çalıştırmak için kullanılan çekirdek (diğer bir deyişle, çalışma zamanı ortamı) seçin. Azure not defterleri aşağıdaki çekirdeklere destekler:

- Python 2.7 + Anaconda2 5.3.0
- Python 3.6 + Anaconda3 5.3.0
- Python 3.5 + Anaconda3 4.2.0 (kullanım dışı bırakılacak)
- R 3.4.1 + Microsoft R 3.4.1 açın
- F#4.1.9

Azure not defterleri, ayrıca temel dağıtımları ötesinde ek paketleri içerir. Python çekirdekler, örneğin numpy, pandas scikit-learn ve matplotlib ve bokeh kitaplıkları.

Ayrıca, o proje içinde tüm dizüstü bilgisayarlar için bir ortam oluşturmaya yönelik bir proje özelleştirebilirsiniz. Daha fazla bilgi için [hızlı başlangıç: Bir proje ile özel bir ortam oluşturma](quickstart-create-jupyter-notebook-project-environment.md).

Temel dağıtımların yanı sıra Azure not defterleri veri uzmanları için faydalı olan birçok ek paketlerin önceden yüklü olarak gelen. Ayrıca, her dil için tipik bir işlem kullanarak kendi paketlerini yükleyebilirsiniz.

## <a name="pre-configured-jupyter-extensions"></a>Önceden yapılandırılmış Jupyter uzantıları

Azure not defterleri ile aşağıdaki Jupyter uzantıları önceden yapılandırılmış:

- [ARTIŞ](https://github.com/damianavila/RISE): Bir Jupyter slayt gösterisi uzantısı (live_reveal olarak da bilinir). Daha fazla bilgi için [bir not defteri slayt gösterisi çalıştırma](present-jupyter-notebooks-slideshow.md).
- [JupyterLab](https://github.com/jupyterlab/jupyterlab): Jupyter not defterleri ile çalışmak için bir tam hesaplama ortamı.
- [Altair](https://github.com/ellisonbg/altair): Python için bir bildirim temelli istatistiksel görselleştirme kitaplığı.
- [BQPlot](https://github.com/bloomberg/bqplot): Jupyter not defterleri için etkileşimli bir çizim çerçevesi.
- [IpyWidgets](https://github.com/jupyter-widgets/ipywidgets): Jupyter not defterleri için etkileşimli HTML pencere öğeleri.

## <a name="issues-and-getting-help"></a>Sorunlar ve Yardım alma

Azure not defterleri hala Önizleme aşamasında olduğundan, hizmet daha sık veya diğer Azure Hizmetleri artık kalıcı olabilecek geçici kesinti yaşayabilirsiniz. Bazı özellikler eksik veya hatalar içeriyor.

Şu anda, iş açısından kritik uygulamalar veya hassas not defterlerini ve verileri için Azure not defterleri Önizleme kullanılmasından öneririz.

Sorularınızı Azure not defterleri hakkında tartışmak için üzerinde bir sorun dosya [GitHub deposu](https://github.com/Microsoft/AzureNotebooks/issues).

## <a name="next-steps"></a>Sonraki adımlar  

- [Örnek Not Defterleri keşfedin](azure-notebooks-samples.md)

- Hızlı Başlangıçlar:

  - [Oluşturma ve bir not defteri paylaşma](quickstart-create-share-jupyter-notebook.md)
  - [Bir not defteri kopyalama](quickstart-clone-jupyter-notebook.md)
  - [Yerel Jupyter not defterini geçirme](quickstart-migrate-local-jupyter-notebook.md)
  - [Özel bir ortam kullanma](quickstart-create-jupyter-notebook-project-environment.md)
  - [Oturum açın ve bir kullanıcı kimliği ayarlayın](quickstart-sign-in-azure-notebooks.md)

- Öğreticiler:

  - [Oluşturma ve bir not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md  )

- Nasıl yapılır makaleleri:
  
  - [Oluşturma ve projeleri kopyalama](create-clone-jupyter-notebooks.md)
  - [Yapılandırma ve projeleri yönetme](configure-manage-azure-notebooks-projects.md)
  - [İçinde bir not defteri paketleri yükleme](install-packages-jupyter-notebook.md)
  - [Mevcut bir slayt gösterisi](present-jupyter-notebooks-slideshow.md)
  - [Veri dosyaları ile çalışma](work-with-project-data-files.md)
  - [Veri kaynaklarına erişim](access-data-resources-jupyter-notebooks.md)
  - [Azure Machine Learning Hizmetleri kullanma](use-machine-learning-services-jupyter-notebooks.md)
