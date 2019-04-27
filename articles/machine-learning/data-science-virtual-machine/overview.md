---
title: Linux ve Windows için Azure Veri Bilimi Sanal Makinesi’ne Giriş | Microsoft Docs
description: Windows ve Linux Veri Bilimi Sanal Makineleri için önemli analiz senaryoları ve bileşenler.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 10/27/2017
ms.author: gokuma
ms.openlocfilehash: 74fe2345af5d1249d6704c7560b42323561bc623
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60578445"
---
# <a name="introduction-to-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows için Azure Veri Bilimi Sanal Makinesi’ne Giriş

Veri Bilimi Sanal Makinesi (DSVM) veri bilimi için Microsoft Azure bulutunda derlenmiş olan özel bir sanal makine görüntüsüdür. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. Windows Server ve Linux üzerinde kullanılabilir. DSVM'nin Windows sürümünü Server 2016 ve Server 2012'de sunuyoruz. Ubuntu 16.04 LTS ve CentOS 7.4’te DSVM’in Linux sürümlerini sunuyoruz.

Bu konuda, Veri Bilimi Sanal Makinesi ile yapabilecekleriniz ele alınmakta, sanal makine kullanımına ilişkin bazı temel senaryolar özetlenmekte, Windows ve Linux sürümlerine kullanılabilen öemli özellikler sıralanmakta ve bunları kullanmaya başlamayla ilgili yönergeler verilmektedir.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi ile ne yapabilirim?
Veri Bilimi Sanal Makinesi’nin (DSVM) amacı, tüm beceri düzeylerindeki ve farklı sektörlerdeki veri uzmanlarına sorunsuz, önceden yapılandırılmış ve tam tümleşik bir veri bilimi ortamı sağlamaktır. Kendi başınıza benzer bir çalışma alanını kullanıma sunmak yerine bir DSVM sağlayabilir, böylece yükleme, yapılandırma ve paket yönetimi süreçlerinde günler ve hatta _haftalar_ kazanabilirsiniz. DSVM ayrıldıktan sonra veri bilimi projenizin üzerinde çalışmaya hemen başlayabilirsiniz.

Veri Bilimi Sanal Makinesi çok çeşitli kullanım senaryoları ile birlikte çalışacak şekilde tasarlanmış ve yapılandırılmıştır. İhtiyaçlarınız değiştikçe ortamınızın ölçeğini artırıp azaltabilir, veri bilimi görevlerini programlamak için istediğiniz dili kullanabilir ve sisteminizi ihtiyaçlarınıza uygun hale getirmek için farklı araçlar yükleyebilirsiniz.

## <a name="key-scenarios"></a>Önemli Senaryolar
Bu bölümde, Veri Bilimi Sanal Makinesinin dağıtılabileceği bazı önemli senaryolar önerilir.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Bulutta önceden yapılandırılmış analiz masaüstü
Veri Bilimi Sanal Makinesi, yerel masaüstlerini yönetilen bir bulut masaüstü ile değiştirmek isteyen veri bilimi ekipleri için bir temel yapılandırma sağlar. Bu temel, bir ekipteki tüm veri bilimcilerinin denemeleri doğrulamak ve iş birliğini desteklemek üzere tutarlı bir kuruluma sahip olmasını sağlar. Ayrıca, sysadmin yükünü azaltarak ve ileri analizler yapmak için gereken çeşitli yazılım paketlerini değerlendirmek, yüklemek ve bakımını yapmak üzere gereken süreyi azaltarak maliyetleri düşürür.

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi
Veri bilimi dersleri veren kurumsal eğitmenler genellikle öğrencilerinin tutarlı bir kuruluma sahip olduğundan ve örneklerin öngörülebilir bir şekilde çalıştığından emin olmak üzere bir sanal makine görüntüsü sağlar. Veri Bilimi Sanal Makinesi, destek ve uyumsuzluk sorunlarını kolaylaştıran tutarlı bir kurulumla isteğe bağlı bir ortam oluşturur. Başta daha kısa eğitim sınıfları olmak üzere bu ortamların sıklıkla oluşturulması gereken durumlar önemli ölçüde avantajlıdır.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Büyük ölçekli projeler için isteğe bağlı esnek kapasite
Veri bilimi yazılım yarışmaları/müsabakaları veya büyük ölçekli veri modelleme ve araştırmaları, genellikle kısa süreler için ölçeği genişletilmiş donanım kapasitesi gerektirir. Veri Bilimi Sanal Makinesi, yüksek güçlü bilgi işlem kaynakları gerektiren denemelerin yapılmasına izin veren ölçeği genişletilmiş sunucular üzerinde talep üzerine hızlıca veri bilimi ortamını çoğaltmaya yardımcı olabilir.

### <a name="short-term-experimentation-and-evaluation"></a>Kısa süreli deneme ve değerlendirme
Veri Bilimi Sanal Makinesi; Microsoft ML Server, SQL Server, Visual Studio araçları, Jupyter, derin öğrenme / ML araç setleri gibi araçları ve toplulukta popüler olan yeni araçları çok az kurulum çabasıyla değerlendirmek veya öğrenmek için kullanılabilir. Veri Bilimi Sanal Makinesi hızlıca kurulabildiğinden, yayımlanmış denemeleri çoğaltma, tanıtımları uygulama, çevrimiçi oturumlarda ve konferans öğreticilerinde kılavuzları takip etme gibi diğer kısa süreli kullanım senaryolarında uygulanabilir.

### <a name="deep-learning"></a>Derin öğrenme
Veri bilimi sanal makinesi, GPU (Grafik işleme birimleri) tabanlı donanımlar üzerinde derin öğrenme algoritmalarını kullanan eğitim modelleri için kullanılabilir. Azure bulutunun VM ölçeklendirme özelliklerinden yararlanan DSVM, gereksinime göre bulutta GPU tabanlı donanımlar kullanmanıza yardımcı olur. Büyük modellerin eğitimi veya aynı işletim sistemi diskini tutarken yüksek hızlı hesaplamalara ihtiyaç duyulması halinde GPU tabanlı bir sanal makineye geçiş yapılabilir.  DSVM’nin Windows Server 2016 sürümünde GPU sürücüleri, çerçeveleri ve derin öğrenme çerçevelerinin GPU sürümleri önceden yüklenmiş olarak gelir. Linux sürümünde, GPU üzerinde derin öğrenme hem CentOS hem de Ubuntu DSVM’leri üzerinde etkindir. Veri Bilimi Sanal Makinesinin Ubuntu, CentOS veya Windows 2016 sürümünü GPU tabanlı olmayan bir Azure sanal makinesine dağıtabilirsiniz; bu durumda tüm derin öğrenme çerçeveleri CPU moduna geri döner.

## <a name="whats-included-in-the-data-science-vm"></a>Veri Bilimi Sanal Makinesi’ne neler dahildir?
Veri Bilimi Sanal Makinesi halihazırda yüklenmiş ve yapılandırılmış olan çok sayıda popüler veri bilimi ve derin öğrenme aracı içerir. Ayrıca tahmine dayalı model derlemek için Microsoft ML Server (R, Python) veya büyük ölçekli veri kümeleriyle araştırma yapmak için SQL Server 2017 gibi çeşitli Azure veri ve analiz ürünleriyle çalışmayı kolaylaştıran araçlar da içerir. Açık kaynak topluluğundan ve Microsoft’tan bir dizi başka aracın yanı sıra örnek kod ve not defterleri de sunulmaktadır. Aşağıdaki tabloda Veri Bilimi Sanal Makinesi’nin Windows ve Linux sürümlerine dahil olan ana bileşenler maddeler halinde verilmiş ve karşılaştırılmıştır.


| **Araç**                                                           | **Windows Sürümü** | **Linux Sürümü** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| Popüler paketlerin önceden yüklü olduğu [Microsoft R Open](https://mran.microsoft.com/open/)   |E                      | E             |
| [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Geliştirici Sürümünün içeriği, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) paralel ve dağıtılmış yüksek performanslı çerçeve (R ve Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) - Microsoft’un yeni modern ML algoritmaları <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R ve Python İşlemleştirme](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |E                      | E |
| Ortak etkinleştirme özelliğine sahip [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) Pro-Plus - Excel, Word ve PowerPoint   |E                      |N              |
| Popüler paketlerin önceden yüklü olduğu [Anaconda Python](https://www.continuum.io/) 2.7, 3.5    |E                      |E              |
| Julia diline yönelik popüler paketlerin önceden yüklü olduğu [JuliaPro](https://juliacomputing.com/products/juliapro.html)                         |E                      |E              |
| İlişkisel Veritabanları                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Veritabanı araçları                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC sürücüleri| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (sorgulama aracı), <br /> * bcp, sqlcmd <br /> * ODBC/JDBC sürücüleri|
| SQL Server ML hizmetleri (R, Python) ile ölçeklenebilir veritabanı içi analiz | E     |N              |
| Aşağıdaki çekirdeklere sahip **[Jupyter Notebook Sunucusu](https://jupyter.org/),**                                  | E     | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (sadece Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | E |
| JupyterHub (Çok kullanıcılı not defteri sunucusu)| N | E |
| JupyterLab (Çok kullanıcılı not defteri sunucusu) | N | Y (sadece Ubuntu) |
| **Geliştirme araçları, IDE ve Kod düzenleyiciler**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) ve Git Eklentisi, Azure HDInsight (Hadoop), Data Lake, SQL Server Veri araçları, [Node.js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs) ve [Visual Studio için R Araçları (RTVS)](https://microsoft.github.io/RTVS-docs/) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](https://junolab.org/)| E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim ve Emacs | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git ve GitBash | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* .NET framework | E | N |
| Power BI Desktop | E | N |
| Azure ve Cortana Intelligence Hizmet paketine erişim SDK’ları | E | E |
| **Veri Taşıma ve yönetim araçları** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Depolama Gezgini | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure CLI](https://docs.microsoft.com/cli/azure) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Blob FUSE sürücüsü](https://github.com/Azure/azure-storage-fuse) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy(Azure Data Lake Depolama)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB Veri Taşıma Aracı](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Veri Yönetimi ağ geçidi](https://msdn.microsoft.com/library/dn879362.aspx): Şirket içi ile bulut arasında veri taşıma | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Unix/Linux Komut Satırı Yardımcı Programları | E | E |
| Veri keşfi için [Apache Drill](https://drill.apache.org) | E | E |
| **Machine Learning Araçları** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) ile tümleştirme (R, Python) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](https://www.cs.waikato.ac.nz/ml/weka/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](https://togaware.com/rattle/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (sadece Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CatBoost](https://tech.yandex.com/catboost/) | N | Y (sadece Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/) | N | Y (sadece Ubuntu) |
| **Derin Öğrenme Araçları** <br>Tüm araçlar GPU veya CPU üzerinde çalışır |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | Y (Windows 2016) | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Horovod](https://github.com/uber/horovod) | N | Y (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](https://mxnet.io/) | Y (Windows 2016) | E|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Chainer](https://chainer.org/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](https://pytorch.org/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorRT](https://developer.nvidia.com/tensorrt) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, cuDNN, NVIDIA Sürücüsü](https://developer.nvidia.com/cuda-toolkit) | E | E |
| **Büyük Veri Platformu (yalnızca Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Yerel [Spark](https://spark.apache.org/) Tek Başına | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Yerel [Hadoop](https://hadoop.apache.org/) (HDFS, YARN) | N | E |

## <a name="get-started"></a>başlarken

### <a name="windows-data-science-vm"></a>Windows Veri Bilimi Sanal Makinesi
* Windows DSVM oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [Windows Veri Bilimi Sanal Makinesi Sağlama](provision-vm.md). Windows DSVM üzerinde veri bilimi projeniz için gereken çeşitli görevleri gerçekleştirme hakkında daha fazla bilgi için bkz. [Veri Bilimi Sanal Makinesi üzerinde yapabileceğiniz on işlem](vm-do-ten-things.md).

### <a name="linux-data-science-vm"></a>Linux Veri Bilimi Sanal Makinesi
* Ubuntu DSVM oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [Linux için Veri Bilimi Sanal Makinesi Sağlama (Ubuntu)](dsvm-ubuntu-intro.md). CentOS DSVM oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [Azure’da Linux CentOS Veri Bilimi Sanal Makinesi Sağlama](linux-dsvm-intro.md).
* Linux VM (hem CentOS hem de Ubuntu) ile çeşitli genel veri bilimi görevlerini nasıl gerçekleştireceğinizi gösteren bir kılavuz için bkz. [Linux Veri Bilimi Sanal Makinesi üzerinde veri bilimi](linux-dsvm-walkthrough.md).

## <a name="next-steps"></a>Sonraki adımlar
[R geliştiricisi Azure kılavuzu](/azure/architecture/data-guide/technology-choices/r-developers-guide)
