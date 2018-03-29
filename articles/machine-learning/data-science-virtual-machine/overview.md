---
title: Linux ve Windows Azure veri bilimi sanal makine için giriş | Microsoft Docs
description: Anahtar analytics senaryolar ve Windows ve Linux veri bilimi sanal makineleri için bileşenler.
keywords: Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: gokuma
ms.openlocfilehash: f62f6c4b2679457e8aaddb81e8a37622ca878ffc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="introduction-to-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows Azure veri bilimi sanal makine için giriş

Veri bilimi sanal makine (DSVM) veri bilimi özellikle yapmak için Microsoft Azure bulut üzerinde özelleştirilmiş bir VM görüntüsü ' dir. Gelişmiş analiz için akıllı uygulamalar derlemeye hızlı giriş yapmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi araçlarına ve diğer araçlara sahiptir. Windows Server ve Linux üzerinde kullanılabilir. DSVM'nin Windows sürümünü Server 2016 ve Server 2012'de sunuyoruz. Ubuntu 16.04 LTS ve CentOS 7.4 DSVM Linux sürümleri sunuyoruz.

Bu konuda veri bilimi VM ile neler yapabileceğinizi açıklar, bazı VM kullanmaya yönelik temel senaryolar açıklanmaktadır, Windows ve Linux sürümlerinde kullanılabilir anahtar özellikler maddeler halinde listelemektedir ve bunları kullanmaya başlamak yönergeler sağlar.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Veri bilimi sanal makine ile ne yapabilirim?
Hedef, veri bilimi sanal makine, bir uyuşmazlık boş veri bilimi ortamıyla tüm beceri düzeylerini ve rolleri veri uzmanları sağlamaktır. Bu VM, karşılaştırılabilir bir ortamda, kendi alındı, harcadığınız önemli ölçüde zaman kazandırır. Bunun yerine, veri bilimi projenizi hemen bir yeni oluşturulan VM örneğinde başlatın. 

Veri bilimi VM tasarlanmış ve çok çeşitli kullanım senaryoları ile çalışmak için yapılandırılmış. Projenizi gereksinimleriniz değiştikçe yukarı veya aşağı ortamınızı ölçeklendirebilirsiniz. Tercih ettiğiniz dili program veri bilimi görevleri için kullanabilirsiniz. Diğer Araçları'nı yüklemek ve sistem tam gereksinimlerinize göre özelleştirin.

## <a name="key-scenarios"></a>Anahtar senaryoları
Bu bölüm veri bilimi VM dağıtılabilir anahtar bazı senaryolar önerir.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Önceden yapılandırılmış analytics Masaüstü bulutta
Veri bilimi VM ile yönetilen bulut Masaüstü yerel masaüstlerine değiştirmek için arayan veri bilimi ekipleri için bir taban çizgisi yapılandırmasını sağlar. Bu taban takımda tüm veri bilimcilerine denemeler doğrulayın ve işbirliği yükseltmek için tutarlı bir kurulumla sahip olmasını sağlar. Sysadmin yükünü azaltma ve değerlendirmenize, yüklemenize ve gelişmiş analizler yapmak için gereken çeşitli yazılım paketlerini korumak için gerekli süreye kaydederek maliyetleri de düşürür.  

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi
Kurumsal eğitmenler ve eğitimcilere sınıflar genellikle kendi Öğrenciler tutarlı bir kurulum olmasını sağlamak için bir sanal makine görüntüsü sağlar ve örnekler beklendiği iş, veri bilimi öğretir. Veri bilimi VM bir isteğe bağlı ortam desteği ve uyumsuzluk sorunları kolaylaştırır için tutarlı bir kurulum oluşturur. Burada sık, özellikle kısa eğitim sınıfları için oluşturulacak bu ortamları gereken durumlarda önemli ölçüde yararlanır.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Büyük ölçekli projeleri için isteğe bağlı Esnek Kapasite
Veri bilimi hackathons/competitions veya büyük ölçekli veri modellemesi ve araştırması gerektiren ölçeklendirilmiş kısa süre için genellikle donanım kapasitesi çıkışı. Veri bilimi VM hızlı bir şekilde çalıştırılması için güçlü bilgi işlem kaynakları gerektiren denemeler izin genişletilmiş sunucularda isteğe bağlı veri bilimi ortamınızın çoğaltılması yardımcı olabilir.

### <a name="short-term-experimentation-and-evaluation"></a>Kısa vadeli deney ve değerlendirme
Veri bilimi VM olabilir Visual Studio Araçları, derin öğrenme Jupyter / ML araç takımları ve yeni araçları ile en az topluluğuna popüler Kurulum çaba değerlendirmek veya Microsoft ML Server, SQL Server gibi araçları öğrenmek için kullanılır. Veri bilimi VM hızlı bir şekilde ayarlanabilir beri tanıtımlar çevrimiçi oturumları veya konferans öğreticileri aşağıdaki yönergelerde yürütme yayımlanan denemeler çoğaltma gibi diğer kısa süreli kullanım senaryolarda uygulanabilir.

### <a name="deep-learning"></a>Derin öğrenme
Veri bilimi VM derin learning algoritmaları GPU (grafik işlem birimleri) tabanlı donanım kullanarak eğitim modeli için kullanılabilir. Azure bulut özellikleri ölçeklendirme VM, DSVM GPU tabanlı donanım gereksinimi göredir bulut üzerinde kullanmanıza yardımcı olur. Bir GPU tabanlı bir VM büyük modelleri eğitimindeki geçiş veya aynı işletim sistemi diski tutarken yüksek hızlı hesaplamalar gerekir.  DSVM Windows Server 2016 sürümünü GPU sürücüleri, çerçeveler ve derin öğrenme çerçeveleri GPU sürümleri önceden yüklenmiş olarak gelir. Linux'ta GPU üzerinde öğrenme derin CentOS ve Ubuntu DSVMs üzerinde etkindir. Tüm derin öğrenme çerçeveleri CPU moduna geri dönüş; bu durumda olur, veri bilimi VM Ubuntu, CentOS ya da Windows 2016 sürümü GPU tabanlı olmayan Azure sanal makinesi dağıtabilirsiniz. 

## <a name="whats-included-in-the-data-science-vm"></a>Veri bilimi VM'yi neler dahildir?
Veri bilimi sanal makine, birçok popüler veri bilimi ve zaten yüklenmiş ve yapılandırılmış araçları öğrenme derin vardır. Ayrıca, çeşitli Azure verileri ve çözümlemeler ürünleriyle çalışmak üzere kolaylaştıran araçlar içerir. Keşfetmek ve büyük ölçekli veri (R, Python) Microsoft ML Server veya SQL Server 2017 kullanarak kümeleri Tahmine dayalı modelleri oluşturun. Bir ana bilgisayar diğer Araçları'nın açık kaynak topluluktan ve Microsoft: de dahil, aynı zamanda örnek kod ve dizüstü bilgisayarlar. Aşağıdaki tabloda maddeler halinde listelemektedir ve Windows ve Linux sürümlerinde, veri bilimi sanal makine bulunan ana bileşenler karşılaştırır.


| **Aracı**                                                           | **Windows sürümü** | **Linux sürümü** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R açık](https://mran.microsoft.com/open/) önceden yüklenmiş popüler paketleriyle   |E                      | E             |
| [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) içeren Geliştirici sürümü <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) parallel and distributed high-performance framework (R & Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) -Microsoft'tan yeni durumu resim ML algoritmaları <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R ve Python Operationalization](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |E                      | E |
| [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) paylaşılan etkinleştirme - Excel, Word ve PowerPoint profesyonel artı   |E                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, önceden yüklenmiş popüler paketleriyle 3.5    |E                      |E              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) önceden yüklenmiş Jale dil için popüler paketleriyle                         |E                      |E              |
| İlişkisel Veritabanları                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Geliştirici sürümü| [PostgreSQL](https://www.postgresql.org/)(yalnızca CentOS) |
| Veritabanı Araçları                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC drivers| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (sorgulama. aracı), <br /> * bcp, sqlcmd <br /> * ODBC/JDBC drivers|
| Ölçeklenebilir veritabanı analytics ile SQL Server ML Hizmetleri (R, Python) | E     |N              |
| **[Jupyter not defteri sunucu](http://jupyter.org/) aşağıdaki çekirdekleri ile**                                  | E     | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Jale | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | E | E |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (yalnızca Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | E |
| JupyterHub (çok kullanıcılı not defterlerini sunucusu)| N | E |
| **Geliştirme araçları, IDE ve Kod Düzenleyicisi**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) > Git eklentisi, Azure Hdınsight (Hadoop), Data Lake, SQL Server veri araçları ile [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), ve [Visual için R araçları Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Jale IDE)](http://junolab.org/)| E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* VIM ve Emacs | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git ve Gitbash'i | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* .net framework | E | N |
| PowerBI Desktop | E | N |
| Azure ve hizmetlerin Cortana Intelligence Suite erişmek için SDK'ları | E | E |
| **Veri taşıma ve Yönetim Araçları** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Storage Gezgini | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure CLI](https://docs.microsoft.com/cli/azure) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [BLOB SİGORTASI sürücüsü](https://github.com/Azure/azure-storage-fuse) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake Store)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB veri geçiş aracı](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Veri Yönetimi ağ geçidi](https://msdn.microsoft.com/library/dn879362.aspx): OnPrem ve bulut arasında veri taşıma | E | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* UNIX/Linux komut satırı yardımcı programları | E | E |
| [Apache ayrıntıya](http://drill.apache.org) veri keşfi için | E | E |
| **Machine Learning araçları** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* İle tümleştirme [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (yalnızca Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | Y (yalnızca Ubuntu) |
| **GPU tabanlı derin araçları öğrenme** |Windows Server 2016 sürümü  | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Bilişsel (önceki adıyla CNTK da bilinir) Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | E | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | E | E|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](http://pytorch.org/)| N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVIDIA basamak](https://github.com/NVIDIA/DIGITS) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model sunucu](https://github.com/awslabs/mxnet-model-server) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow sunma](https://www.tensorflow.org/serving/) | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, CUDNN, NVIDIA sürücüsü](https://developer.nvidia.com/cuda-toolkit) | E | E |
| **Büyük veri Platformu (yalnızca Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Yerel [Spark](http://spark.apache.org/) tek başına | N | E |
| &nbsp;&nbsp;&nbsp;&nbsp;* Yerel [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | E |



## <a name="get-started-with-the-windows-data-science-vm"></a>Windows veri bilimi VM ile çalışmaya başlama
* İstenen Windows DSVM edition örneği giderek oluşturun
  * [Windows Server 2016 DSVM dayalı](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.windows-data-science-vm)
  
  or 
  * [Windows Server 2012 DSVM dayalı](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) 
* Tıklatın **BT almak artık** düğmesi.
* VM VM oluşturduğunuz sırada belirttiğiniz kimlik bilgilerini kullanarak, Uzak Masaüstü üzerinden oturum açın.
* Bul ve kullanılabilir araçları başlatmak için tıklatın **Başlat** menüsü.

## <a name="get-started-with-the-linux-data-science-vm"></a>Linux veri bilimi VM ile çalışmaya başlama
* İstenen Linux DSVM edition örneği giderek oluşturun 
  * [Ubuntu DSVM dayalı](http://aka.ms/dsvm/ubuntu)

  or

  * [CentOS DSVM dayalı](http://aka.ms/dsvm/centos)

  
* Tıklatın **Şimdi Al** düğmesi.
* Putty veya SSH VM oluştururken belirtilen kimlik bilgilerini kullanarak komut, gibi bir SSH istemciden VM'ye oturum açın.
* Kabuk isteminde dsvm daha fazla bilgi girin.
* İçin grafik bir masaüstü, istemci platformunuza X2Go istemci indirme [burada](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) ve Linux veri bilimi VM belge'ndaki yönergeleri izleyin [Linux veri bilimi sanal makine sağlama](linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Sonraki adımlar
### <a name="for-the-windows-data-science-vm"></a>Windows veri bilimi VM
* Windows sürümünde kullanılabilir belirli araçlarını çalıştırma hakkında daha fazla bilgi için bkz: [Microsoft Veri bilimi sanal makine sağlama](provision-vm.md) ve
* Windows VM üzerinde veri bilimi projeniz için gereken çeşitli görevleri gerçekleştirme hakkında daha fazla bilgi için bkz: [veri bilimi sanal makine yapabilir on nokta](vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Linux veri bilimi VM
* Linux sürümünde bulunan belirli araçları çalıştırma hakkında daha fazla bilgi için bkz: [Linux veri bilimi sanal makine sağlama](linux-dsvm-intro.md).
* Linux VM ile birçok ortak veri bilimi görevlerinin nasıl gerçekleştirileceğini gösterir bir anlatım için bkz: [veri bilimi üzerinde Linux veri bilimi sanal makine](linux-dsvm-walkthrough.md).

