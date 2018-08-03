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
ms.date: 09/10/2017
ms.author: gokuma
ms.openlocfilehash: b749d8a904bc40eba3346cc03d9274236380c80d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449760"
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Azure üzerinde Windows veri bilimi sanal makinesi sağlama
Microsoft Veri bilimi sanal makinesi önceden yüklenmiş ve veri analizi ve makine öğrenimi için yaygın olarak kullanılan birkaç popüler Araçlar ile yapılandırılmış bir Windows Azure sanal makine (VM) görüntüsüdür. Dahil edilen araçlara şunlardır:

* [Azure Machine Learning](../service/index.yml) Workbench
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Geliştirici sürümü
* Anaconda Python dağıtım
* Jupyter Not Defteri (ile R, Python, PySpark çekirdekleri)
* Visual Studio Community Edition
* Power BI desktop
* SQL Server 2017 Developer Edition
* Tek başına Spark örneğinde yerel geliştirme ve test etme
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Makine öğrenimi ve veri analizi araçları
  * Derin öğrenme çerçeveleri: AI çerçevelerine dahil olmak üzere zengin bir dizi [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet, Keras VM'de dahil edilir.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): hızlı makine öğrenme çevrimiçi, karma, allreduce, indirimleri, learning2search, etkin, gibi teknikler destekleyen sistem ve etkileşimli öğrenme.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araç.
  * [Rattle](http://rattle.togaware.com/) (R analitik aracı için bilgi kolayca): veri analizi ve makine öğrenimi R kolay kullanmaya başlama sağlayan bir araç. Bu, GUI tabanlı bir veri keşfi ve modelleme ile otomatik olarak R kodu oluşturmayı içerir.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : bir görsel veri araştırma ve makine öğrenimi Java'da yazılım.
  * [Apache ayrıntıya](https://drill.apache.org/): bir şemasız SQL sorgu alt Hadoop, NoSQL ve bulut depolama.  ODBC ve JDBC arabirimleri sorgulanırken NoSQL ve dosyaları Power BI, Excel, Tableau gibi standart BI araçlarından etkinleştirmek için destekler.
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure Hizmetleri kullanma
* Git, GitHub, Visual Studio Team Services de dahil olmak üzere kaynak kod depoları ile çalışmak için Git Bash dahil olmak üzere ve (awk, sed, perl, grep, bulma, wget, curl, vb. dahil) çeşitli popüler Linux komut satırı yardımcı programlarını sağlar hem de git bash ve komut erişilebilir ister. 

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verilerin önceden işlenmesi
1. Oluşturma ve modelleri test etme
1. Akıllı uygulamalarda kullanılmak modelleri dağıtma

Veri bilimcileri, bu görevleri tamamlamak için çeşitli araçlar kullanın. Bu oldukça zaman yazılım uygun sürümleri bulun ve ardından indirin ve yükleyin alabilir. Microsoft Veri bilimi sanal makinesi, Azure üzerinde önceden yüklenmiş ve yapılandırılmış tüm çeşitli popüler araçlarla sağlanabilen kullanıma hazır bir görüntü sağlayarak bu yükü kolaylaştırabilir. 

Microsoft Veri bilimi sanal makinesi analiz projenizi jump-starts. R, Python, SQL ve C# gibi çeşitli dillerde görevler üzerinde çalışmanıza olanak tanır. Visual Studio geliştirme ve kullanımı kolay olan kodunuzu test etmek için bir IDE sağlar. Azure SDK'ın sanal Makineye dahil, Microsoft'un bulut platformu üzerinde çeşitli Hizmetleri kullanarak uygulamalarınızı oluşturmanıza olanak sağlar. 

Bu veri bilimi VM görüntüsü için hiçbir yazılım ücreti yoktur. Yalnızca boyutu, sağlamanız sanal makinenin hangi bağımlı için Azure kullanım ücretleri ödersiniz. Fiyatlandırma Ayrıntıları bölümünde daha ayrıntılı bilgi işlem ücretleri ulaşabilirsiniz [veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) sayfası. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi diğer sürümleri
Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntüsüdür de çok benzer araçların yanı sıra bazı ek derin öğrenme çerçeveleri ile kullanılabilir. A [CentOS](linux-dsvm-intro.md) görüntüsüdür de kullanılabilir. Ayrıca sunuyoruz bir [Windows Server 2012 sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) veri bilimi sanal makinesi ancak birkaç araç yalnızca Windows Server 2016 sürümünde kullanılabilir.  Aksi takdirde, bu makalede, Windows Server 2012 sürümüne de geçerlidir.

## <a name="prerequisites"></a>Önkoşullar
Bir Microsoft Veri bilimi sanal makinesi oluşturmadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**: Abonelik sahibi için bkz: [alma Azure ücretsiz deneme sürümü](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinenizi oluşturma
Bir örneği, Microsoft Veri bilimi sanal makinesi oluşturmak için aşağıdaki adımları izleyin:

1. Sanal makine üzerinde listeleme gidin [Azure portalında](https://portal.azure.com/#create/microsoft-ads.windows-data-science-vmwindows2016).
1. Seçin **Oluştur** düğmesi Sihirbazı'na alınması için alt kısımdaki.![ Yapılandırma-data-bilimi-vm](./media/provision-vm/configure-data-science-virtual-machine.png)
1. Microsoft Veri bilimi sanal makinesi oluşturmak için kullanılan sihirbaz gerektirir **girişleri** her biri için **dört adımı** bu şekilde sağ tarafındaki numaralandırılır. Bu adımların her biri yapılandırmak için gerekli girişleri şunlardır:
   
   1. **Temel Bilgiler**
      
      1. **Ad**: oluşturmakta olduğunuz veri bilimi sunucunuzun adını yazın.
      1. **VM Disk türü**: SSD veya HDD arasında seçim yapın. (NVIDIA Tesla tabanlı K80), bir örnek için NC_v1 GPU seçin **HDD** disk türü. 
      1. **Kullanıcı adı**: yönetici hesabı oturum açma kimliği.
      1. **Parola**: yönetici hesabının parolası.
      1. **Abonelik**: birden fazla aboneliğiniz varsa, bir makine olduğu faturalandırılır ve seçin.
      1. **Kaynak grubu**: yeni bir tane oluşturabilir veya varolan bir grubu kullanın.
      1. **Konum**: en uygun veri merkezi seçin. Genellikle, verilerinizden en iyi olduğundan veya en hızlı ağ erişimi için fiziksel konumunuza en yakın veri merkezi bulunur.
   1. **Boyutu**: işlev gereksinim ve maliyet kısıtlamalar karşılayan sunucusu türlerinden birini seçin. "Tümünü Görüntüle"'i seçerek daha fazla seçenek VM boyutlarının alabilirsiniz.
   1. **Ayarları**:
      
      1. **Yönetilen diskleri kullanmanız**: yönetilen VM diskleri yönetmek için Azure istiyorsanız seçin.  Aksi takdirde, yeni veya mevcut bir depolama hesabı belirtmeniz gerekir. 
      1. **Diğer parametreler**: genellikle yalnızca varsayılan değerleri kullanırsınız. Varsayılan olmayan değerleri kullanmayı deneyin istiyorsanız, belirli alanlar hakkında Yardım için bilgi bağlantı üzerine gelin.
    a. **Özet**: girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın ve tıklayın **Oluştur**. **Not**: VM herhangi bir ek ücret, seçtiğiniz sunucu boyutu için işlem ötesinde yok **boyutu** adım. 

> [!NOTE]
> Sağlama yaklaşık 10-20 dakika sürer. Sağlama durumunu Azure portalında görüntülenir.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinesi erişme
VM oluşturulduktan sonra Uzak Masaüstü uygulamasına önceki yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanarak yapabilecekleriniz **Temelleri** bölümü. 

Sanal makinenizin oluşturup sağlanan yüklenmiş ve yapılandırılmış araçları kullanmaya başlamak hazırsınız. Başlat menüsü kutucukları ve masaüstü simgelerini birçok araç vardır. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinede yüklü araçları



### <a name="microsoft-ml-server-developer-edition"></a>Microsoft ML Server Geliştirici sürümü
Analiz için ölçeklenebilir R veya Python için Microsoft Kurumsal kitaplıkları kullanmak istiyorsanız, VM'nin (daha önce Microsoft R Server'ı olarak da bilinir) Microsoft ML Server Geliştirici sürümü yüklü olduğundan. Microsoft ML Server, R ve Python için genel olarak dağıtılabilen kurumsal sınıf analiz platformu olan ve ölçeklenebilir, ticari olarak desteklenen ve güvenlidir. Birçok büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi özellikleri destekleyen, ML Server analytics – Keşif, analiz, Görselleştirme ve modelleme tam aralığını destekler. R ile kullanarak ve açık kaynak R ve Python genişletme, Microsoft ML Server tamamen uyumludur / Python betikleri, işlevleri ve CRAN / pip / Conda paketleri, Kurumsal verileri analiz etmek için ölçeklendirin. Ayrıca, veri paralel ve öbeklenmiş işlenmesini ekleyerek açık kaynak R bellek içi sınırlamaları da giderir. Bu ne ana belleğe sığamayacak kadar büyük veriler üzerinde analiz çalıştırmanıza olanak sağlar.  Sanal makinede bulunan Visual Studio Community Edition, R veya Python ile çalışmak için tam bir IDE sağlar. Visual Studio uzantısı için Visual Studio ve Python araçları için R araçları içerir. Ayrıca diğer Ide'leri de gibi bildirimde [RStudio](http://www.rstudio.com) ve [PyCharm Community edition](https://www.jetbrains.com/pycharm/) VM üzerinde. 

### <a name="python"></a>Python
Anaconda Python dağıtımı 2.7 ve 3.6 Python aracılığıyla geliştirmeye yönelik, yüklendi. Bu dağıtım, temel bir Python yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra içerir. Python araçları için Visual Studio (Visual Studio 2017 Community edition veya paketlenmiş Anaconda boşta veya Spyder gibi Ide'lerle biri yüklü PTVS) kullanabilirsiniz. Bunlardan biri arama çubuğunda arayarak başlatın (**Win** + **S** anahtar).

> [!NOTE]
> Anaconda Python 2.7, Visual Studio için Python Tools işaret etmek için her sürüm için özel ortamlarda oluşturmanız gerekir. Visual Studio 2017 Community Edition bu ortam yollarını ayarlamak için gidin **Araçları** -> **Python Araçları** -> **Python ortamları**ve ardından **+ özel**. 
> 
> 

Anaconda Python 3.6 C:\Anaconda altında yüklü olduğundan ve altında c:\Anaconda\envs\python2 Anaconda Python 2.7 yüklenir. Bkz: [PTVS dokümantasyonu](/visualstudio/python/installing-python-interpreters.md) ayrıntılı adımlar için. 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Anaconda dağıtım bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter notebook sunucusu Python Python 2.7 ile önceden yapılandırılmış 3.x, PySpark, Julia ve R çekirdekler. Jupyter sunucuyu başlatın ve not defteri sunucuya erişmek için tarayıcıyı başlatmak için "Jupyter Notebook" adlı bir masaüstü simgesi vardır. 

Biz, Python ve R'dir birkaç örnek not defterleri paketlediğinizden Jupyter not defterlerini Jupyter erişim sonra Microsoft ML Server, SQL Server ML Hizmetleri (veritabanı içi analiz), Python, Microsoft Bilişsel araç seti, Tensorflow ve diğer Azure teknolojileri ile çalışmaya nasıl gösterir. Bir önceki adımda oluşturduğunuz parola kullanarak Jupyter not defteri için kimlik doğrulama işleminden sonra not defteri giriş sayfasında örnekleri bağlantısını görebilirsiniz. 

### <a name="visual-studio-2017-community-edition"></a>Visual Studio 2017 Community edition
VM'de yüklü visual Studio Community sürümü. Popüler IDE Değerlendirme amacıyla ve küçük ekipler için kullanabileceğiniz Microsoft tarafından sunulan ücretsiz bir sürümüdür. Lisans koşullarını kontrol edebilirsiniz [burada](https://www.visualstudio.com/support/legal/mt171547).  Visual Studio'yu Masaüstü simgesine çift tıklayarak açın veya **Başlat** menüsü. Programları da arayabilirsiniz **Win** + **S** ve "Visual Studio" girerek. Bir kez burada C#, Python, R, node.js gibi dillerde projeleri oluşturabilirsiniz. Eklenti ayrıca Azure Hizmetleri gibi Azure veri Kataloğu, Azure HDInsight (Hadoop, Spark) ve Azure Data Lake ile çalışmak uygun hale getiren yüklenir. Şimdi de mevcuttur adlı bir eklenti ```Visual Studio Tools for AI``` sorunsuz bir şekilde Azure Machine Learning ile tümleşir ve derleme yapay ZEKA uygulamaları hızlı bir şekilde yardımcı olur. 

> [!NOTE]
> Değerlendirme süreniz doldu belirten bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin veya Visual Studio Community Edition erişim elde etmek için yeni bir ücretsiz hesap oluşturun. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer edition
ML hizmetleriyle veritabanı içi analiz çalıştırmak için SQL Server 2017 developer sürümü, R veya Python VM'de sağlanır. ML Hizmetleri akıllı uygulama geliştirme ve dağıtma için bir platform sağlar. Zengin ve güçlü bu diller ve topluluktan birçok paketi modeller oluşturun ve SQL Server verilerinizi Öngörüler oluşturmak için kullanabilirsiniz. ML Services (veritabanında) içinde SQL Server R ve Python dil tümleşir çünkü verilere yaklaştırılmasıyla analytics tutabilirsiniz. Bu maliyetleri ve veri taşıma ile ilişkili güvenlik riskleri ortadan kaldırır.

> [!NOTE]
> SQL Server Geliştirici sürümü, yalnızca geliştirme ve test amaçları için kullanılabilir. Üretim ortamında çalıştırmak için lisansa ihtiyacınız var. 
> 
> 

SQL server başlatarak erişebileceğiniz **SQL Server Management Studio**. Sanal Makinenizin adını sunucu adı olarak doldurulur. Windows üzerinde Windows Yönetici olarak oturum açtıktan sonra kimlik doğrulaması kullanın. SQL Server Management Studio'da olduktan sonra diğer kullanıcıları oluşturun, veritabanları oluşturma, veri içeri aktarın ve SQL sorgularını çalıştırın. 

Veritabanı içi analiz SQL ML Hizmetleri kullanarak etkinleştirmek için bir tane aşağıdaki komutu çalıştırın sunucu yöneticisi olarak oturum açtıktan sonra SQL Server management Studio'da eylem zaman. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Çeşitli Azure Araçları VM'de yüklenir:

* Azure SDK belgelerine erişmek için bir masaüstü kısayolu yok. 
* **AzCopy**: Microsoft Azure depolama hesabınıza içine ve dışına veri taşımak için kullanılır. Kullanım görmek için şunu yazın **Azcopy** kullanımını görmek için bir komut isteminde. 
* **Microsoft Azure Depolama Gezgini**: Azure depolama hesabı ve aktarım verilerinizi ve Azure depolama alanından içinde depolanan nesneleri göz atmak için kullanılır. Yazabilirsiniz **Depolama Gezgini** içinde arama yapın veya bu aracına erişmek için Windows Başlat menüsünde bulabilirsiniz. 
* **Adlcopy**: Azure Data Lake'e veri taşımak için kullanılır. Kullanım görmek için şunu yazın **adlcopy** bir komut isteminde. 
* **dtui**: Azure Cosmos DB, bulutta bir NoSQL veritabanı gelen ve giden veri taşımak için kullanılır. Tür **dtui** komut isteminde. 
* **Azure Data Factory Integration Runtime**: şirket içi veri kaynakları ile bulut arasında veri taşıma sağlar. Azure Data Factory gibi araçları içinde kullanılır. 
* **Microsoft Azure Powershell**: komut dosyası dili de sanal makinenizde yüklü PowerShell kullanarak Azure kaynaklarınızı yönetmek için kullanılan bir araçtır. 

### <a name="power-bi"></a>Power BI
Panolar ve harika görselleştirmeler oluşturmanıza yardımcı olmak üzere **Power BI Desktop** yüklendi. Bu aracı panolarınızı ve raporlarınızı yazmak ve bunları buluta yayımlama için farklı kaynaklardan veri çekmek için kullanın. Bilgi için [Power BI](http://powerbi.microsoft.com) site. Power BI desktop'ı Başlat menüsünde bulabilirsiniz. 

> [!NOTE]
> Power BI hizmetine erişmek için bir Office 365 hesabınız olmalıdır. 
> 
> 

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning Workbench masaüstü uygulaması ve komut satırı arabirimi var. Workbench yerleşik veri hazırlama adımlarınızı gerçekleştirirken, veri hazırlama adımları sizin öğrenir sahiptir. Ayrıca, proje yönetimi, çalıştırma geçmişi ve not defteri tümleştirmesi üretkenliğinizi artırmak için sağlar. TensorFlow, Cognitive Toolkit, Spark ML ve scikit dahil olmak üzere en iyi açık kaynak çerçeveleri yararlanabilir-Modellerinizi geliştirmeyi öğrenin. DSVM'nin tek tek kullanıcı % LOCALAPPDATA % dizinine Azure Machine Learning workbench'i yükleyin Masaüstü simgesi sunuyoruz. Workbench'i kullanabilmeniz için gereken her kullanıcının bir yapması gereken zaman çift tıklatma eylemi ```AzureML Workbench Setup``` Workbench örneğini yüklemek için Masaüstü simgesi. Azure Machine Learning ayrıca oluşturur ve % LOCALAPPDATA%\amlworkbench\python ayıklanan kullanıcı başına bir Python ortamı kullanır.

## <a name="additional-microsoft-development-tools"></a>Ek Microsoft geliştirme araçları
[ **Microsoft Web Platformu yükleyicisi** ](https://www.microsoft.com/web/downloads/platform.aspx) bulmak ve diğer Microsoft geliştirme araçlarını indirin için kullanılabilir. Microsoft Veri bilimi sanal makinesi masaüstünde sağlanan aracı için bir kısayol yoktur.  

## <a name="important-directories-on-the-vm"></a>VM üzerinde önemli dizinleri
| Öğe | Dizin |
| --- | --- |
| Jupyter not defteri sunucusu yapılandırmaları |C:\ProgramData\jupyter |
| Jupyter not defteri örnekleri giriş dizini |c:\dsvm\notebooks ve c:\users\<kullanıcıadı > \notebooks|
| Diğer örnekleri |c:\dsvm\samples |
| Anaconda (varsayılan: Python 3.6) |c:\Anaconda |
| Anaconda Python 2.7 ortamı |c:\Anaconda\envs\python2 |
| Microsoft ML Server tek başına Python  | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| R varsayılan örneği (ML Server tek başına) |C:\Program Files\Microsoft\ML Server\R_SERVER |
| ML Hizmetleri SQL veritabanı örnek dizini |C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Azure Machine Learning Workbench (kullanıcı başına) | %localappdata%\amlworkbench | 
| Çeşitli araçlar |c:\dsvm\tools |

> [!NOTE]
> DSVM ve Windows Server 2016 sürümü Mart 2018'den önce Windows Server 2012 sürümünde varsayılan Anaconda Python 2.7 ortamıdır. İkincil Python 3.5 c:\Anaconda\envs\py35 bulunan ortamdır. 
> 
> 

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıda, öğrenme ve araştırma devam etmek için sonraki adımlardan birkaçı verilmiştir. 

* Çeşitli veri bilimi araçları, Başlat menüsüne tıklayıp menüde listelenen araçlar kullanıma veri bilimi VM'si keşfedin.
* Ürün ziyaret ederek Azure Machine Learning Hizmetleri ve Workbench hakkında bilgi [hızlı başlangıç ve öğreticilerle sayfa](../service/index.yml). 
* Gidin **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** r ile veri analizi Kurumsal ölçekte destekleyen RevoScaleR kitaplığı kullanma örnekleri için.  
* Makaleyi okuyun: [veri bilimi sanal makinesi üzerinde yapabileceğiniz 10 şeyler](http://aka.ms/dsvmtenthings)
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](../team-data-science-process/index.yml).
* Ziyaret [Azure AI Gallery](http://gallery.cortanaintelligence.com) Azure'da Azure Machine learning ve ilgili verileri kullanan makine öğrenimi ve veri analizi için örnekleri Hizmetleri. Ayrıca bir simge üzerinde sağladık **Başlat** menü ve Bu galeri sanal makinenin masaüstündeki.

