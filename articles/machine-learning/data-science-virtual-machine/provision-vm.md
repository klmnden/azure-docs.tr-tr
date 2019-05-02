---
title: Windows veri bilimi sanal makinesi oluşturma
titleSuffix: Azure
description: Yapılandırma ve analiz için Azure'da bir veri bilimi sanal makinesi oluşturma ve makine öğrenimi.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: gokuma
ms.openlocfilehash: 1f9ee5cf28de8fdb824bebf222e5e8d80e22c34f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64712423"
---
# <a name="provision-a-windows-data-science-virtual-machine-on-azure"></a>Azure'da Windows veri bilimi sanal makinesi sağlama

Microsoft Windows veri bilimi sanal makinesi (DSVM), azure'da önceden yüklenmiştir ve veri analizi ve makine öğrenimi için Araçlar ile yapılandırılmış gelen Windows Server 2016 sanal makine (VM) görüntüsüdür.

## <a name="included-data-science-tools"></a>Dahil edilen veri bilimi araçları

Aşağıdaki araçlar bir DSVM içinde dahil edilmiştir:

* [Azure Machine Learning hizmeti](../service/index.yml) Python SDK'sı
* [Microsoft Machine Learning sunucusu](https://docs.microsoft.com/machine-learning-server/index) Geliştirici sürümü
* Anaconda Python dağıtım
* Jupyter not defteri ile R, Python ve PySpark çekirdekleri
* Microsoft Visual Studio Community
* Microsoft Power BI Desktop
* Microsoft SQL Server 2017 Developer edition
* Yerel geliştirme ve test için tek başına bir Apache Spark örneği
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Makine öğrenimi ve veri analizi araçları:
  * VM üzerinde derin öğrenme çerçeveleri - zengin yapay ZEKA çerçevesini dahildir: [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet ve Keras
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) - hızlı makine çevrimiçi karma, allreduce, indirimleri, learning2search ve etkin ve etkileşimli öğrenme tekniklerle destekleyen sistemi öğrenme
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/) -hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araç
  * [Rattle](https://togaware.com/rattle/) -alır, R analitik aracı kullanmaya ile veri analizi ve makine öğrenimi r'de Bu, GUI tabanlı bir veri keşfi ve modelleme ile otomatik olarak R kodu oluşturmayı içerir.
  * [Weka](https://www.cs.waikato.ac.nz/ml/weka/) -görsel veri madenciliği ve makine öğrenimi Java'da yazılım
  * [Apache ayrıntıya](https://drill.apache.org/) -bir şemasız SQL sorgu alt Apache Hadoop, NoSQL ve bulut depolama. ODBC ve JDBC arabirimleri, NoSQL ve Power BI, Microsoft Excel ve Tableau gibi standart BI araçlarından dosyaları'nı sorgulamak için destekler.
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure Hizmetleri kullanma
* Git, GitHub ve Azure DevOps içeren kaynak kodu depoları ile çalışmak için Git Bash dahil. Git, Git Bash ve bir komut istemi erişilebilir çeşitli popüler Linux komut satırı yardımcı programları sağlar. Awk, sed, perl, grep, bulma, wget ve curl örnek verilebilir.

### <a name="about-data-science"></a>Veri bilimi hakkında

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verileri ön işleme
1. Yapı ve test modelleri
1. Akıllı uygulamalarda kullanılmak modelleri dağıtma

Veri bilimcileri bu görevler için çeşitli araçlar kullanın. Zaman yazılım uygun sürümleri bulun ve sonra da indirin ve yükleyin alıcı olabilir. DSVM, önceden yüklenmiş ve yapılandırılmış çeşitli popüler araçlarla Azure üzerinde sağlanabilir bir kullanıma hazır görüntüsü sağlayarak zaman tasarrufu sağlar.

Analiz projenizi DSVM jump-starts. R, Python, SQL ve C# gibi çeşitli dillerde görevler üzerinde çalışabilir. Visual Studio geliştirme ve kodunuzu test etmek için kullanımı kolay tümleşik geliştirme ortamı (IDE) sağlar. Microsoft'un bulut platformu üzerinde çeşitli Hizmetleri kullanarak uygulamalarınızı oluşturmanıza yardımcı olacak Azure SDK'sı sanal Makineye eklenir.

Bu veri bilimi VM görüntüsü için hiçbir yazılım ücreti yoktur. Yalnızca Azure kullanım ücretleri ödeme yaparsınız. Bunlar, sağlamanız sanal makinenin boyutuna bağlıdır. Daha ayrıntılı bilgi işlem ücretleri bulunan **fiyatlandırma ayrıntıları** bölümünde [veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) sayfası.

### <a name="other-dsvm-versions"></a>Diğer DSVM sürümleri

* Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntü. Bunun yanı sıra bazı ek derin öğrenme çerçeveleri DSVM'ye benzer birçok araç vardır.
* A [Linux CentOS](linux-dsvm-intro.md) görüntü.
* [Windows Server 2012 sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) , veri bilimi sanal makinesi. Bazı araçlar, yalnızca Windows Server 2016 sürümünde kullanılabilir. Aksi takdirde, bu makalede, Windows Server 2012 sürümüne de geçerlidir.

## <a name="prerequisite"></a>Önkoşul

Bir Microsoft Veri bilimi sanal makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.com/free).

## <a name="create-your-dsvm"></a>DSVM oluşturma

DSVM örneği oluşturmak için:

1. Sanal makine üzerinde listeleme Git [Azure portalında](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016). Zaten oturum değil, Azure hesabınızda oturum açmak için istenebilir.
1. Seçin **Oluştur** altındaki düğmesini.

   ![Yapılandırma-data-bilimi-vm](./media/provision-vm/configure-data-science-virtual-machine.png)

1. Ekran görüntüsünde, sağdaki bölmede gösterilen adımların her biri yapılandırmak için aşağıdaki bilgileri girmeniz gerekir:

   1. **Temel**:
      * **Ad**: DSVM adı
      * **VM Disk türü**: ya da **SSD** veya **HDD**. NVIDIA Tesla K80 tabanlı gibi NC_v1 GPU örnek için Seç **HDD** disk türü.
      * **Kullanıcı adı**: yönetici hesabı kimliği
      * **Parola**: yönetici hesabı parolası
      * **Abonelik**: Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin.
      * **Kaynak Grubu**. Yeni bir tane oluşturabilir veya varolan bir grubu kullanın.
      * **Konum**. En uygun veri merkezi seçin. En hızlı ağ erişimi için verilerinizden en iyi veya fiziksel konumunuza en yakın veri merkezidir.
   1. **Boyutu**: Maliyet kısıtlamaları ve işlevsel gereksinimlerini karşılayan sunucusu türlerinden birini seçin. Daha fazla VM boyutları için seçim **görünümü tüm**.
   1. **Ayarları**:  
      * **Yönetilen diskleri kullanma**. Seçin **yönetilen** VM için diskleri yönetmek için Azure istiyorsanız. Aksi takdirde, yeni veya mevcut bir depolama hesabı belirtmeniz gerekir.  
      * **Diğer parametreler**. Varsayılan değerleri kullanabilirsiniz. Varsayılan olmayan değerleri kullanmak istiyorsanız, belirli alanlar hakkında Yardım için bilgi bağlantı üzerine gelin.
   1. **Özet**: Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. **Oluştur**’u seçin.

> [!NOTE]
> * VM işlem maliyeti, seçtiğiniz sunucu boyutu ötesinde herhangi bir ek ücret ödemeye değil **boyutu** adım.
> * Sağlama yaklaşık 10-20 dakika sürer. Azure portalında, sanal Makinenizin durumunu görüntüleyebilirsiniz.

## <a name="how-to-access-the-dsvm"></a>DSVM erişme

Sanal makine oluşturulup sağlanan sonra Uzak Masaüstü uygulamasına önceki yapılandırdığınız yönetici hesabının kimlik bilgilerini kullanarak yapabilecekleriniz **Temelleri** bölümü. Yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız demektir. Çok sayıda araçla Başlat menüsü kutucukları ve masaüstü simgelerini aracılığıyla erişilebilir.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinede yüklü araçları

DSVM'nin yüklü gelen araçları hakkında daha fazla bilgi edinin:

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Geliştirici sürümü

Machine Learning Server Developer edition VM üzerinde yüklü olmadığından, Microsoft Enterprise Library ölçeklenebilir R veya Python için analiz için kullanabilirsiniz. Daha önce Microsoft R Server'ı olarak bilinen, Machine Learning sunucusu bir problem-dağıtılabilir, kurumsal sınıf analiz platformudur. R ve Python için kullanılabilir. Ayrıca ölçeklenebilir, ticari olarak desteklenen ve güvenli bir hizmettir.

Machine Learning sunucusu, çeşitli büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi görevlerini destekler. Analytics tam aralığını destekler: Keşif, analiz, Görselleştirme ve modelleme. Kullanarak ve açık kaynak R ve Python, R ve Python betikleri ve işlevler ile Machine Learning sunucusu uyumludur. Kurumsal ölçekte verileri çözümlemek için CRAN, pip ve Conda paketleri ile de uyumludur.

Machine Learning sunucusu ekleme paralel ve öbeklenmiş veri işleme tarafından açık kaynaklı R'nin bellek içi sınırlamaları ele alır. Bu analiz daha ne ana belleğe sığamayacak kadar büyük veri çubuğunda çalıştırabileceğiniz anlamına gelir. Visual Studio Community, VM'de dahil edilir. Bunu R araçları Visual Studio ve Python araçları için tam IDE R veya Python ile çalışmaya yönelik sağlayan Visual Studio (PTVS) uzantıları için vardır. Ayrıca, diğer Ide'leri ister bildirimde [RStudio](https://www.rstudio.com) ve [PyCharm Community edition](https://www.jetbrains.com/pycharm/) VM üzerinde.

### <a name="python"></a>Python

Python kullanarak geliştirme için Anaconda Python dağıtımlarını 2.7 ve 3.6 yüklenir. Bu dağıtımların yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python var. Visual Studio Community 2017 içinde yüklenen PTVS kullanabilirsiniz. Veya boşta veya Spyder gibi Anaconda birlikte IDE'ler birini kullanabilirsiniz. Arayın ve bu paketlerin (Win + S) başlatın.

> [!NOTE]
> Anaconda Python 2.7, Visual Studio için Python Tools işaret etmek için her sürüm için özel ortamlarda oluşturmanız gerekir. Visual Studio 2017 Community bu ortam yollarını ayarlamak için gidin **Araçları** > **Python Araçları** > **Python ortamları**. Ardından **+ özel**.

Anaconda Python 3.6 altında yüklü **C:\Anaconda**. Anaconda Python 2.7 altında yüklü **c:\Anaconda\envs\python2**. Ayrıntılı adımlar için bkz. [PTVS dokümantasyonu](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

### <a name="the-jupyter-notebook"></a>Jupyter not defteri

Anaconda dağıtım Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter Notebook sunucusu ile Python 2.7 ve Python 3.x, PySpark, Julia ve R defterleri önceden yapılandırılmıştır. Jupyter sunucuyu başlatın ve not defteri sunucuya erişmek için tarayıcıyı başlatmak için kullanın **Jupyter not defteri** Masaüstü simgesi.

Biz, Python ve R'dir birkaç örnek not defterleri paketi Jupyter eriştikten sonra dizüstü aşağıdaki teknolojilerle çalışmanıza işlemini gösterir:

* Machine Learning Server
* SQL Server Machine Learning Hizmetleri, veritabanı içi analiz
* Python
* Microsoft Bilişsel Araç Seti
* Tensorflow
* Diğer Azure teknolojileri

Jupyter not defteri için daha önceki bir adımda oluşturduğunuz parola kullanarak kimlik doğrulama işleminden sonra not defteri giriş sayfasında örnekleri bağlantısını bulabilirsiniz.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

Visual Studio Community DSVM içerir. Bu değerlendirme amacıyla ve küçük ekipler için kullanabileceğiniz Microsoft tarafından sunulan popüler IDE ücretsiz bir sürümüdür. Bkz: [lisanslama koşulları](https://www.visualstudio.com/support/legal/mt171547).

Masaüstü simgesini kullanarak Visual Studio'yu açın veya **Başlat** menüsü. Programlar (Win + S), arayın ve ardından **Visual Studio**. Burada, C#, Python, R ve node.js gibi dillerde projeleri oluşturabilirsiniz. Yüklü eklentiler aşağıdaki Azure Hizmetleri ile çalışmak uygun yapın:

* Azure Veri Kataloğu
* Azure HDInsight Hadoop ve Spark
* Azure Data Lake

De mevcuttur bir eklenti olarak adlandırılan **Visual Studio Code için Azure Machine Learning** sorunsuz bir şekilde Azure Machine Learning ile tümleşir ve derleme yapay ZEKA uygulamaları hızlı bir şekilde yardımcı olur.

> [!NOTE]
> Değerlendirme süreniz doldu bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Veya Visual Studio Community erişim elde etmek için yeni bir ücretsiz hesap oluşturun.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer sürümü

DSVM, Machine Learning Hizmetleri ile SQL Server 2017 developer sürümü ile birlikte gelir. Bu SQL Server sürümü, R veya Python ile gelir ve veritabanı içi analiz çalıştırabilirsiniz. Machine Learning Hizmetleri akıllı uygulama geliştirme ve dağıtma için bir platform sunar. Bu diller ve topluluktan gelen çok sayıda paketleri, modeller oluşturun ve SQL Server verilerinizi Öngörüler oluşturmak için kullanabilirsiniz. Machine Learning Hizmetleri veritabanında, SQL Server R ve Python dilleri tümleşir çünkü verilere yaklaştırılmasıyla analytics tutabilirsiniz. Bu tümleştirme, maliyet ve veri taşıma ile ilişkili güvenlik riskleri ortadan kaldırır.

> [!NOTE]
> SQL Server Developer sürümü yalnızca geliştirme ve test amaçları içindir. Üretim ortamında çalıştırmak için lisansa ihtiyacınız var.

SQL Server, Microsoft SQL Server Management Studio'yu başlatarak erişebilirsiniz. Sanal Makinenizin adını olarak doldurulur **sunucu adı**. Windows üzerinde yönetici olarak oturum açtığınızda, Windows kimlik doğrulaması kullanın. SQL Server Management Studio'da olduğunuzda, diğer kullanıcılar oluşturma, veritabanları oluşturma, veri içeri aktarın ve SQL sorgularını çalıştırın.

Sunucu Yöneticisi oturum açtıktan sonra veritabanı içi analiz SQL Machine Learning hizmetlerini kullanarak etkinleştirmek için aşağıdaki komutu SQL Server Management Studio'da tek seferlik bir eylem olarak çalıştırın:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Değiştirin `%COMPUTERNAME%` VM adınızla.

### <a name="azure"></a>Azure

Çeşitli Azure Araçları VM'de yüklenir:

* Bir masaüstü kısayolu için Azure SDK Belgeleri gider.
* Kullanım **AzCopy** veriyi Azure depolama hesabınıza kopyalamak için. Kullanımını görmek için girin **Azcopy** bir komut isteminde.
* Kullanım **Azure Depolama Gezgini** Azure depolama hesabınızdaki depoladığınız nesnelerin gidin. Ayrıca veri ve Azure depolama alanından kopyalar. Bu araç erişmek için enter **Depolama Gezgini** içinde **arama** alan. Veya Windows içinde Bul **Başlat** menüsü.
* **Adlcopy** Azure Data Lake'e veri kopyalar. Kullanımını görmek için girin **adlcopy** bir komut isteminde.
* **dtui** için ve Azure Cosmos DB, NoSQL veritabanı bulutta veri kopyalar. Girin **dtui** bir komut isteminde.
* **Azure Data Factory Integration Runtime** şirket içi veri kaynakları ile bulut arasında veri kopyalar. Azure Data Factory gibi araçları içinde kullanılır.
* Kullanım **Microsoft Azure PowerShell** PowerShell komut dosyası dili Azure kaynaklarınızı yönetmek için. Ayrıca, sanal makinenizde de yüklenir.

### <a name="power-bi"></a>Power BI

DSVM birlikte **Power BI Desktop** panolar ve görselleştirmeler oluşturmanıza yardımcı olmak için yüklü. Bu aracı panolarınızı ve raporlarınızı yazmak ve bunları buluta yayımlama için farklı kaynaklardan veri çekmek için kullanın. Daha fazla bilgi için [Power BI](https://powerbi.microsoft.com) site. Power BI Desktop bulabilirsiniz **Başlat** menüsü.

> [!NOTE]
> Power BI hizmetine erişmek için bir Microsoft Office 365 hesabınız olması.

### <a name="azure-machine-learning-service-python-sdk"></a>Azure Machine Learning hizmeti Python SDK'sı

Veri bilimcileri ve yapay ZEKA geliştiricilerine oluşturmak ve machine learning ile iş akışlarını çalıştırmak için Python için Azure Machine Learning SDK kullanma [Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md). Jupyter not defterleri veya başka bir Python IDE hizmetinde TensorFlow ve scikit gibi açık kaynak altyapılarını kullanarak etkileşim kurabileceğiniz-öğrenin.

Python SDK'sını kullanmaya başlamak için bkz: [Python kullanarak Azure Machine Learning'i kullanmaya başlamak için](../service/quickstart-create-workspace-with-python.md).

Python SDK'sı, Microsoft Veri bilimi sanal makinesi üzerinde önceden yüklenir.

## <a name="more-microsoft-development-tools"></a>Daha fazla Microsoft geliştirme araçları

Kullanabileceğiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) bulup diğer Microsoft geliştirme araçlarını indirin. Araç Microsoft Veri bilimi sanal makinesi masaüstünde bir kısayolu yok.  

## <a name="important-directories-on-the-vm"></a>VM üzerinde önemli dizinleri

| Öğe | Dizin |
| --- | --- |
| Jupyter not defteri sunucusu yapılandırmaları | C:\ProgramData\jupyter |
| Jupyter not defteri örnekleri giriş dizini | C:\dsvm\notebooks ve c:\users\\< kullanıcı adı\>\notebooks |
| Diğer örnekleri | C:\dsvm\samples |
| Anaconda, varsayılan: Python 3.6 | C:\Anaconda |
| Anaconda Python 2.7 ortamı | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning sunucusu (tek başına) Python | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Varsayılan R örnek, Machine Learning sunucusu (tek başına) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| SQL Machine Learning Hizmetleri veritabanında örneği dizini | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Çeşitli araçlar | C:\dsvm\tools |

> [!NOTE]
> DSVM ve Windows Server 2016 sürümü Mart 2018'den önce Windows Server 2012 sürümünde varsayılan Anaconda Python 2.7 ortamıdır. Python 3.5 konumunda bulunan, ikincil ortamıdır **C:\Anaconda\envs\py35**.

## <a name="next-steps"></a>Sonraki adımlar

* Veri bilimi VM'si seçerek keşfedin **Başlat** menüsü.
* Azure Machine Learning hizmeti hakkında bilgi edinmek [Azure Machine Learning hizmeti nedir?](../service/overview-what-is-azure-ml.md) ve aşımına [hızlı başlangıçlar ve öğreticiler](../service/index.yml) kullanılabilir.
* Dosya Gezgini'nde gidin **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** r ile veri analizi Kurumsal ölçekte destekleyen RevoScaleR kitaplığı kullanma örnekleri için.  
* Makaleyi okuyun [veri bilimi sanal makinesi üzerinde yapabileceğiniz on işlem](https://aka.ms/dsvmtenthings).
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](../team-data-science-process/index.yml).
* Ziyaret [Azure AI Gallery](https://gallery.cortanaintelligence.com) Azure'da Azure Machine Learning ve ilgili verileri kullanan makine öğrenimi ve veri analizi için örnekleri Hizmetleri. Ayrıca bir simge için Bu galeri üzerinde sağladık **Başlat** menüsünde ve masaüstünde sanal makinenin.
