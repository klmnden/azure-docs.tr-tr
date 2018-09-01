---
title: Azure üzerinde Windows veri bilimi sanal makinesi sağlama | Microsoft Docs
description: Yapılandırma ve analiz için Azure'da bir veri bilimi sanal makinesi oluşturma ve makine öğrenimi.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: gokuma
ms.openlocfilehash: b01ef3701ffb46da57c52e5fe73828ec4252b074
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344769"
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Azure üzerinde Windows veri bilimi sanal makinesi sağlama
Microsoft Veri bilimi sanal makinesi (DSVM), bir Windows Azure sanal makine (VM) görüntüsüdür. Bu önceden yüklenmiş ve veri analizi ve makine öğrenimi için kullanılan çeşitli araçlar ile yapılandırılmış. Aşağıdaki araçları dahil edilir:

* [Azure Machine Learning](../service/index.yml) Workbench.
* [Microsoft Machine Learning sunucusu](https://docs.microsoft.com/machine-learning-server/index) Geliştirici sürümü.
* Anaconda Python dağıtımı.
* R, Python ve PySpark çekirdekleri ile Jupyter not defteri.
* Microsoft Visual Studio topluluğu.
* Microsoft Power BI desktop.
* Microsoft SQL Server 2017 Developer edition.
* Tek başına Apache Spark örneğinde yerel geliştirme ve test için.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html).
* Makine öğrenimi ve veri analizi araçları:
  * Derin öğrenme çerçeveleri. Zengin yapay ZEKA çerçevesini VM'de dahil edilir: [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet ve Keras.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit). Hızlı makine çevrimiçi karma, allreduce, indirimleri, learning2search ve etkin ve etkileşimli öğrenme tekniklerle destekleyen sistemi öğrenme.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/). Hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araç.
  * [Rattle](https://togaware.com/rattle/), R analitik araç kolayca öğrenin. Veri analizi ve makine öğrenimi r'de alır, bir aracı kullanmaya Bu, GUI tabanlı bir veri keşfi ve modelleme ile otomatik olarak R kodu oluşturmayı içerir.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/). Görsel verileri araştırma ve makine öğrenimi Java'da yazılım.
  * [Apache ayrıntıya](https://drill.apache.org/). Bir şemasız SQL sorgu alt Apache Hadoop, NoSQL ve bulut depolama.  ODBC ve JDBC arabirimleri, NoSQL ve Power BI, Microsoft Excel ve Tableau gibi standart BI araçlarından dosyaları'nı sorgulamak için destekler.
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure hizmetlerini kullanın.
* Git, GitHub ve Visual Studio Team Services içeren kaynak kodu depoları ile çalışmak için Git Bash dahil. Git, Git Bash ve bir komut istemi erişilebilir çeşitli popüler Linux komut satırı yardımcı programları sağlar. Awk, sed, perl, grep, bulma, wget ve curl örnek verilebilir.

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verileri ön işleme.
1. Yapı ve model test edin.
1. Akıllı uygulamalarda kullanılmak modelleri dağıtın.

Veri bilimcileri bu görevler için çeşitli araçlar kullanın. Zaman yazılım uygun sürümleri bulun ve sonra da indirin ve yükleyin alıcı olabilir. Microsoft Veri bilimi sanal makinesi, önceden yüklenmiş ve yapılandırılmış çeşitli popüler araçlarla Azure üzerinde sağlanabilir bir kullanıma hazır görüntüsü sağlayarak zaman tasarrufu sağlar. 

Microsoft Veri bilimi sanal makinesi analiz projenizi jump-starts. R, Python, SQL ve C# gibi çeşitli dillerde görevler üzerinde çalışabilir. Visual Studio geliştirme ve kodunuzu test etmek için kullanımı kolay tümleşik geliştirme ortamı (IDE) sağlar. Azure SDK'sı sanal Makineye eklenir. Bu nedenle, uygulamalarınızı Microsoft'un bulut platformu üzerinde çeşitli Hizmetleri kullanarak oluşturabilirsiniz. 

Bu veri bilimi VM görüntüsü için hiçbir yazılım ücreti yoktur. Yalnızca Azure kullanım ücretleri ödeme yaparsınız. Bunlar, sağlamanız sanal makinenin boyutuna bağlıdır. Daha ayrıntılı bilgi işlem ücretleri bulunan **fiyatlandırma ayrıntıları** bölümünde [veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) sayfası. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Diğer sürümler, veri bilimi sanal makinesi
* Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntü. Bunun yanı sıra bazı ek derin öğrenme çerçeveleri DSVM'ye benzer birçok araç vardır. 
* A [Linux CentOS](linux-dsvm-intro.md) görüntü.
* [Windows Server 2012 sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) , veri bilimi sanal makinesi. Bazı araçlar, yalnızca Windows Server 2016 sürümünde kullanılabilir. Aksi takdirde, bu makalede, Windows Server 2012 sürümüne de geçerlidir.

## <a name="prerequisite"></a>Önkoşul
Bir Microsoft Veri bilimi sanal makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinenizi oluşturma
Bir örneği, Microsoft Veri bilimi sanal makinesi oluşturmak için aşağıdaki adımları izleyin:

1. Sanal makine üzerinde listeleme gidin [Azure portalında](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016). Zaten oturum değil, Azure hesabınızda oturum açmanız istenebilir.
1. Seçin **Oluştur** Sihirbazı'na alınması için alt kısımdaki düğmesi.

  ![Yapılandırma-data-bilimi-vm](./media/provision-vm/configure-data-science-virtual-machine.png) 

1. Microsoft Veri bilimi sanal makinesi oluşturur Sihirbazı'nı gerektirir **giriş**. Şu giriş, Şekil sağ tarafta gösterilen adımların her biri yapılandırmak için gereklidir:

  a. **Temel**:

    i. **Ad**. Oluşturmakta olduğunuz veri bilimi sunucusunun adı.  

    ii. **VM Disk türü**. Seçin **SSD** veya **HDD**. NVIDIA Tesla K80 tabanlı gibi NC_v1 GPU örnek için Seç **HDD** disk türü.   

    iii. **Kullanıcı adı**. Oturum açmak için Yönetici hesap kimliği.   

    IV. **Parola**. Yönetici hesabı parolası.  

    v. **Abonelik**. Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin.   

    VI. **Kaynak Grubu**. Yeni bir tane oluşturabilir veya varolan bir grubu kullanın.   

    VII. **Konum**. En uygun veri merkezi seçin. En hızlı ağ erişimi için verilerinizden en iyi veya fiziksel konumunuza en yakın veri merkezidir.   

  b. **Boyutu**. Maliyet kısıtlamaları ve işlevsel gereksinimlerini karşılayan sunucusu türlerinden birini seçin. Daha fazla VM boyutları için seçim **görünümü tüm**.  

  c. **Ayarları**:  

    i. **Yönetilen diskleri kullanma**. Seçin **yönetilen** VM için diskleri yönetmek için Azure istiyorsanız. Aksi takdirde, yeni veya mevcut bir depolama hesabı belirtmeniz gerekir.  

    ii. **Diğer parametreler**. Varsayılan değerleri kullanabilirsiniz. Varsayılan olmayan değerleri kullanmak istiyorsanız, belirli alanlar hakkında Yardım için bilgi bağlantı üzerine gelin.  

  d. **Özet**. Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. **Oluştur**’u seçin. 

> [!NOTE]
> * VM işlem maliyeti, seçtiğiniz sunucu boyutu ötesinde herhangi bir ek ücret yoktur **boyutu** adım. 
> * Sağlama yaklaşık 10-20 dakika sürer. Azure portalında durumunu görüntüler.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinesi erişme
Sanal makine oluşturulup sağlanan sonra Uzak Masaüstü uygulamasına önceki yapılandırdığınız yönetici hesabının kimlik bilgilerini kullanarak yapabilecekleriniz **Temelleri** bölümü. Yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız demektir. Çok sayıda araçla, Başlat menüsü kutucukları ve simgelere sahiptir. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinede yüklü araçları

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Geliştirici sürümü
Machine Learning Server Developer edition VM üzerinde yüklü olmadığından, Microsoft Enterprise Library ölçeklenebilir R veya Python için analiz için kullanabilirsiniz. Daha önce Microsoft R Server'ı olarak bilinen, Machine Learning sunucusu bir yaygın olarak dağıtılabilen kurumsal sınıf analiz platformudur. R ve Python için kullanılabilir ve ölçeklenebilir, ticari olarak desteklenen ve güvenlidir. 

Machine Learning sunucusu, çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi görevlerini destekler. Analytics tam aralığını destekler: Keşif, analiz, Görselleştirme ve modelleme. Kullanarak ve açık kaynak R ve Python, R ve Python betikleri ve işlevler ile Machine Learning sunucusu uyumludur. Kurumsal ölçekte verileri çözümlemek için CRAN, pip ve Conda paketleri ile de uyumludur. 

Machine Learning sunucusu ekleme paralel ve öbeklenmiş veri işleme tarafından açık kaynaklı R'nin bellek içi sınırlamaları ele alır. Bu nedenle daha ne ana belleğe sığamayacak kadar büyük veri analizi çalıştırabilirsiniz. Visual Studio Community, VM'de dahil edilir. Bunu R araçları Visual Studio ve Python araçları için tam IDE R veya Python ile çalışmaya yönelik sağlayan Visual Studio (PTVS) uzantısı için vardır. Ayrıca, diğer Ide'leri ister bildirimde [RStudio](http://www.rstudio.com) ve [PyCharm Community edition](https://www.jetbrains.com/pycharm/) VM üzerinde. 

### <a name="python"></a>Python
Python kullanarak geliştirme için Anaconda Python dağıtımlarını 2.7 ve 3.6 yüklendi. Bu dağıtımların yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python var. Visual Studio Community 2017 içinde yüklenen PTVS kullanabilirsiniz. Veya boşta veya Spyder gibi Anaconda birlikte IDE'ler birini kullanabilirsiniz. Arayın ve bu paketlerin (Win + S) başlatın.

> [!NOTE]
> Anaconda Python 2.7, Visual Studio için Python Tools işaret etmek için her sürüm için özel ortamlarda oluşturmanız gerekir. Visual Studio 2017 Community bu ortam yollarını ayarlamak için gidin **Araçları** > **Python Araçları** > **Python ortamları**. Ardından **+ özel**. 
> 
> 

Anaconda Python 3.6 altında yüklü **C:\Anaconda**. Anaconda Python 2.7 altında yüklü **c:\Anaconda\envs\python2**. Ayrıntılı adımlar için bkz. [PTVS dokümantasyonu](/visualstudio/python/installing-python-interpreters.md). 

### <a name="the-jupyter-notebook"></a>Jupyter not defteri
Anaconda dağıtım Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter Notebook sunucusu ile Python 2.7 ve Python 3.x, PySpark, Julia ve R defterleri önceden yapılandırılmıştır. Jupyter sunucuyu başlatın ve vardır not defteri sunucusuna erişmek için tarayıcıyı başlatmak için Masaüstü simgesi çağrılan **Jupyter not defteri**. 

Biz, Python ve R'dir birkaç örnek not defterleri paketi Jupyter eriştikten sonra dizüstü aşağıdaki teknolojilerle çalışmanıza işlemini gösterir:

* Machine Learning sunucusu.
* SQL Server Machine Learning Hizmetleri, veritabanı içi analiz. 
* Python.
* Microsoft Bilişsel Araç Seti.
* Tensorflow.
* Diğer Azure teknolojileri. 

Jupyter not defteri için daha önceki bir adımda oluşturduğunuz parola kullanarak kimlik doğrulama işleminden sonra not defteri giriş sayfasında örnekleri bağlantısına bakın. 

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017
Visual Studio Community, VM'de yüklüdür. Popüler IDE Değerlendirme amacıyla ve küçük ekipler için kullanabileceğiniz Microsoft tarafından sunulan ücretsiz bir sürümüdür. Bkz: [lisanslama koşulları](https://www.visualstudio.com/support/legal/mt171547). 

Visual Studio'yu Masaüstü simgesine çift tıklayarak açın veya **Başlat** menüsü. Programlar (Win + S), arayın ve ardından **Visual Studio**. Burada, C#, Python, R ve node.js gibi dillerde projeleri oluşturabilirsiniz. Yüklü eklentiler aşağıdaki Azure Hizmetleri ile çalışmak uygun yapın:
* Azure Veri Kataloğu
* Azure HDInsight Hadoop ve Spark
* Azure Data Lake 

De mevcuttur bir eklenti olarak adlandırılan ```Visual Studio Tools for AI``` sorunsuz bir şekilde Azure Machine Learning ile tümleşir ve derleme yapay ZEKA uygulamaları hızlı bir şekilde yardımcı olur. 

> [!NOTE]
> Değerlendirme süreniz doldu bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Veya Visual Studio Community erişim elde etmek için yeni bir ücretsiz hesap oluşturun. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition
Veritabanı içi analiz çalıştırmak için Machine Learning Hizmetleri ile SQL Server 2017 developer sürümü, R veya Python VM'de sağlanır. Machine Learning Hizmetleri akıllı uygulama geliştirme ve dağıtma için bir platform sunar. Bu diller ve topluluktan gelen çok sayıda paketleri, modeller oluşturun ve SQL Server verilerinizi Öngörüler oluşturmak için kullanabilirsiniz. Machine Learning Hizmetleri veritabanında, SQL Server R ve Python dilleri tümleşir çünkü verilere yaklaştırılmasıyla analytics tutabilirsiniz. Bu tümleştirme, maliyet ve veri taşıma ile ilişkili güvenlik riskleri ortadan kaldırır.

> [!NOTE]
> SQL Server Developer sürümü yalnızca geliştirme ve test amaçları içindir. Üretim ortamında çalıştırmak için lisansa ihtiyacınız var. 
> 
> 

SQL Server, Microsoft SQL Server Management Studio'yu başlatarak erişebilirsiniz. Sanal Makinenizin adını olarak doldurulur **sunucu adı**. Windows üzerinde yönetici olarak oturum açtığınızda, Windows kimlik doğrulaması kullanın. SQL Server Management Studio'da olduğunuzda, diğer kullanıcılar oluşturma, veritabanları oluşturma, veri içeri aktarın ve SQL sorgularını çalıştırın. 

Sunucu Yöneticisi oturum açtıktan sonra veritabanı içi analiz SQL Machine Learning hizmetlerini kullanarak etkinleştirmek için aşağıdaki komutu SQL Server Management Studio'da tek seferlik bir eylem olarak çalıştırın: 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Çeşitli Azure Araçları VM'de yüklenir:

* Bir masaüstü kısayolu için Azure SDK Belgeleri gider. 
* **AzCopy** verileri Azure depolama hesabınızın içine ve dışına taşımak için kullanılır. Kullanımını görmek için girin **Azcopy** bir komut isteminde. 
* Kullanım **Azure Depolama Gezgini** Azure depolama hesabınızdaki depoladığınız nesnelerin gidin. Ayrıca, için ve Azure Depolama'dan veri aktarır. Bu aracına erişmek için girebilirsiniz **Depolama Gezgini** içinde **arama** alan. Veya Windows üzerinde Bul **Başlat** menüsü. 
* **Adlcopy** Azure Data Lake'e veri taşır. Kullanımını görmek için girin **adlcopy** bir komut isteminde. 
* **dtui** için ve Bulutu bir NoSQL veritabanı olan Azure Cosmos DB'den verileri taşır. Girin **dtui** bir komut isteminde. 
* **Azure Data Factory Integration Runtime** şirket içi veri kaynakları ile bulut arasında veri taşır. Azure Data Factory gibi araçları içinde kullanılır. 
* **Microsoft Azure PowerShell** PowerShell komut dosyası dili Azure kaynaklarınızı yönetmek için kullanılan bir araçtır. Ayrıca, sanal makinenizde de yüklenir. 

### <a name="power-bi"></a>Power BI
**Power BI Desktop** panolar ve görselleştirmeler oluşturmanıza yardımcı olmak için yüklenir. Bu aracı panolarınızı ve raporlarınızı yazmak ve bunları buluta yayımlama için farklı kaynaklardan veri çekmek için kullanın. Daha fazla bilgi için [Power BI](http://powerbi.microsoft.com) site. Power BI desktop bulabilirsiniz **Başlat** menüsü. 

> [!NOTE]
> Power BI hizmetine erişmek için bir Microsoft Office 365 hesabınız olması. 
> 
> 

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning Workbench masaüstü uygulaması ve komut satırı arabirimi var. Workbench, bunları alırken, veri hazırlama adımları sizin öğrenir yerleşik veri hazırlama sahiptir. Ayrıca, proje yönetimi, çalıştırma geçmişi ve not defteri tümleştirmesi üretkenliğinizi artırmak için sağlar. 

TensorFlow, Cognitive Toolkit, Spark ML ve scikit dahil olmak üzere, açık kaynak çerçeveleri kullanabilirsiniz-, Modellerinizi geliştirmeyi öğrenin. DSVM'nin sağladığımız Azure Machine Learning Workbench bireysel kullanıcı yüklemek için bir masaüstü simgesi **% LOCALAPPDATA %** dizin. 

Workbench, her bir kullanıcı, tek seferlik bir eylem gerçekleştirmeniz gerekir. Çift ```AzureML Workbench Setup``` workbench örneği yüklemek için Masaüstü simgesi. Azure Machine Learning ayrıca oluşturduğu ve kullandığı bir de ayıklanan kullanıcı Python ortamını başına **%LOCALAPPDATA%\amlworkbench\python** dizin.

## <a name="more-microsoft-development-tools"></a>Daha fazla Microsoft geliştirme araçları
[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) bulmak ve diğer Microsoft geliştirme araçları'nı indirmek için kullanılır. Microsoft Veri bilimi sanal makinesi masaüstünde sağlanan aracı için bir kısayol yoktur.  

## <a name="important-directories-on-the-vm"></a>VM üzerinde önemli dizinleri
| Öğe | Dizin |
| --- | --- |
| Jupyter not defteri sunucusu yapılandırmaları | C:\ProgramData\jupyter |
| Jupyter not defteri örnekleri giriş dizini | c:\dsvm\notebooks ve c:\users\<kullanıcıadı > \notebooks |
| Diğer örnekleri | c:\dsvm\samples |
| Anaconda, varsayılan: Python 3.6 | c:\Anaconda |
| Anaconda Python 2.7 ortamı | c:\Anaconda\envs\python2 |
| Microsoft Machine Learning sunucusu (tek başına) Python | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Varsayılan R örnek, Machine Learning sunucusu (tek başına) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| SQL Machine Learning Hizmetleri veritabanında örneği dizini | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Azure Machine Learning Workbench kullanıcı başına | %localappdata%\amlworkbench | 
| Çeşitli araçlar | c:\dsvm\tools |

> [!NOTE]
> DSVM ve Windows Server 2016 sürümü Mart 2018'den önce Windows Server 2012 sürümünde varsayılan Anaconda Python 2.7 ortamıdır. Python 3.5 konumunda bulunan, ikincil ortamıdır **c:\Anaconda\envs\py35**. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

* Veri bilimi VM'si seçerek keşfedin **Başlat** menüsü.
* Ürün ziyaret ederek Azure Machine Learning Hizmetleri ve Workbench hakkında bilgi [hızlı başlangıç ve öğreticilerle sayfa](../service/index.yml). 
* Gidin **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** r ile veri analizi Kurumsal ölçekte destekleyen RevoScaleR kitaplığı kullanma örnekleri için.  
* Makaleyi okuyun [veri bilimi sanal makinesi üzerinde yapabileceğiniz on işlem](http://aka.ms/dsvmtenthings).
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](../team-data-science-process/index.yml).
* Ziyaret [Azure AI Gallery](http://gallery.cortanaintelligence.com) Azure'da Azure Machine Learning ve ilgili verileri kullanan makine öğrenimi ve veri analizi için örnekleri Hizmetleri. Ayrıca bir simge için Bu galeri üzerinde sağladık **Başlat** menüsünde ve masaüstünde sanal makinenin.

