---
title: "Azure üzerinde Windows veri bilimi sanal makine sağlama | Microsoft Docs"
description: "Yapılandırma ve analizi için Azure üzerinde bir veri bilimi sanal makine oluşturun ve makine öğrenme."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: bradsev
ms.openlocfilehash: d0a9926f49e2be66a9d51a1bb0e4e19342205880
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="provision-the-windows-data-science-virtual-machine-on-azure"></a>Azure üzerinde Windows veri bilimi sanal makine sağlama
Microsoft Veri bilimi sanal makine önceden yüklenmiş ve veri analizi ve makine öğrenme için yaygın olarak kullanılan birkaç popüler araçları ile yapılandırılmış bir Windows Azure sanal makine (VM) görüntüsüdür. Bulunan araçlar şunlardır:

* [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning-services/) çalışma ekranı
* [Microsoft ML Server](https://docs.microsoft.com/machine-learning-server/index) Geliştirici sürümü
* Anaconda Python dağıtımı
* Jupyter Not Defteri (ile R, Python, PySpark tekrar)
* Visual Studio Community Edition
* Power BI desktop
* SQL Server 2017 Geliştirici sürümü
* Yerel geliştirme ve test için tek başına Spark örneği
* [JuliaPro](https://juliacomputing.com/products/juliapro.html)
* Machine learning ve veri analiz araçları
  * Derin öğrenme çerçeveler: AI çerçeveleri de dahil olmak üzere çeşitli [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [bağlayıcı](https://chainer.org/), mxNet, Keras VM dahil edilir.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): çevrimiçi, karma, allreduce, düşürülmesi, learning2search, etkin, gibi teknikler destekleme sistem öğrenme hızlı bir makine ve etkileşimli öğrenme.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): hızlı ve doğru boosted ağaç uygulama sağlayan bir araç.
  * [Rattle](http://rattle.togaware.com/) (R analitik aracı için bilgi kolayca): veri analizi ve R ile GUI tabanlı veri keşfi kolay öğrenme ve otomatik R kod oluşturma ile modelleme makine ile çalışmaya başlama sağlayan bir araç.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : görsel veri araştırma ve makine öğrenimi Java yazılım.
  * [Apache ayrıntıya](https://drill.apache.org/): bir şemasız SQL sorgu alt Hadoop, NoSQL ve bulut depolama.  Sorgulama NoSQL ve Power BI, Excel, Tableau gibi standart BI araçları dosyalarından etkinleştirmek için ODBC ve JDBC arabirimleri destekler.
* Azure Machine Learning ve diğer Azure hizmetleriyle R ve Python için kitaplıkları kullanma
* Git Bash'i GitHub, Visual Studio Team Services de dahil olmak üzere kaynak kodu depoları ile çalışmak için de dahil olmak üzere Git
* Windows bağlantı noktalarını (awk azaltılabilir, perl, grep, Bul, wget, curl vb. dahil) çeşitli popüler Linux komut satırı yardımcı programlarını komut istemi üzerinden erişilebilir. 

Veri Bilimi bulunurken bir dizi görev yineleme içerir:

1. Bulma, yükleme ve verilerin önceden işlenmesi
2. Derleme ve modelleri test etme
3. Akıllı uygulamaları tüketimini modellerini dağıtma

Veri bilimcilerine, bu görevleri tamamlamak için çeşitli araçları kullanın. Oldukça uygun yazılım sürümlerini bulmak ve ardından indirin ve yükleyin zun olabilir. Microsoft Veri bilimi sanal makine, önceden yüklenmiş ve yapılandırılmış tüm çeşitli popüler araçları ile Azure'da sağlanabilir bir kullanıma hazır görüntüsü sağlayarak bu yük kolaylaştırabilir. 

Microsoft Veri bilimi sanal makine analytics projenizi jump-starts. R, Python, SQL ve C# dahil olmak üzere çeşitli dillerde görevler üzerinde çalışmanıza olanak tanır. Visual Studio geliştirmek ve kullanımı kolay kodunuzu test etmek için bir IDE sağlar. Azure VM'de bulunan SDK'sı, Microsoft bulut platformu üzerinde çeşitli Hizmetleri kullanarak uygulamalarınızı oluşturmanıza olanak verir. 

Bu veri bilimi VM görüntüsü için yazılım harcamanız yok. Yalnızca hangi bağımlı sağlamanız sanal makine boyutu için Azure kullanım ücretleri ödersiniz. Daha ayrıntılı bilgi işlem ücretleri fiyatlandırma Ayrıntıları bölümünde bulunabilir [veri bilimi sanal makine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm?tab=PlansAndPrice) sayfası. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Veri bilimi sanal makinenin diğer sürümleri
Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntüdür de çok benzer araçları plus çerçeveleri öğrenme birkaç ek ayrıntılı kullanılabilir. A [CentOS](linux-dsvm-intro.md) görüntüdür de kullanılabilir. Ayrıca sunuyoruz bir [Windows Server 2012 sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.standard-data-science-vm) veri bilimi sanal makinenin yine de birkaç araçları yalnızca Windows Server 2016 Edition'da kullanılabilir.  Aksi takdirde, bu makale Windows Server 2012 sürümü için geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bir Microsoft Veri bilimi sanal makine oluşturmadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**: bir tane almak için bkz: [alma Azure ücretsiz deneme sürümü](http://azure.com/free).


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makine oluşturma
Örnek, Microsoft Veri bilimi sanal makine oluşturmak için adımlar şunlardır:

1. Sanal makine üzerinde listeleme gidin [Azure portal](https://portal.azure.com/#create/microsoft-ads.windows-data-science-vmwindows2016).
2. Seçin **oluşturma** Sihirbazı'na gerçekleştirilecek altındaki düğmesini.![ yapılandırma verileri-Bilim-vm](./media/provision-vm/configure-data-science-virtual-machine.png)
3. Microsoft Veri bilimi sanal makine oluşturmak için kullanılan sihirbazın gerektirir **girişleri** her biri için **dört adım** Bu şekil sağ tarafta numaralandırılır. Bu adımların her biri yapılandırmak için gereken girdiler şunlardır:
   
   1. **Temel Bilgiler**
      
      1. **Ad**: oluşturduğunuz veri bilimi sunucunuzun adını yazın.
      2. **VM Disk türü**: SSD veya HDD arasında seçim yapın. GPU (NC-serisi) seçin **HDD** disk türü. 
      3. **Kullanıcı adı**: Yönetici hesap oturum açma kimliği.
      4. **Parola**: yönetici hesabı parolası.
      5. **Abonelik**: birden fazla aboneliğiniz varsa, bir makine olduğu oluşturulur ve fatura için seçin.
      6. **Kaynak grubu**: yeni bir tane oluşturun veya varolan bir grubu kullanın.
      7. **Konum**: en uygun olan veri merkezi seçin. Genellikle verilerinizden en iyi veya en yakın fiziksel konumunuza en hızlı ağ erişimi için veri merkezi olur.
   2. **Boyutu**: işlev gereksinimi ve maliyet kısıtlamaları karşılayan sunucu türlerinden birini seçin. Daha fazla seçenek VM boyutları "Tümünü Görüntüle" seçerek alabilirsiniz.
   3. **Ayarları**:
      
      1. **Yönetilen diskler kullanan**: yönetilen VM için diskleri yönetmek için Azure istiyorsanız seçin.  Aksi halde yeni veya exitsting bir depolama hesabı belirtmeniz gerekir. 
      2. **Diğer parametreler**: yalnızca genellikle varsayılan değerleri kullanın. Durumunda, varsayılan olmayan değerleri kullanmak isteyip istemediğinize karar belirli alanlar hakkında Yardım için bilgi bağlantısı üzerinden gelebilirsiniz.
   4. **Özet**: girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın ve tıklatın **oluşturma**. **Not**: VM, seçtiğiniz sunucu boyutu işlem ötesinde herhangi bir ek ücret yok **boyutu** adım. 

> [!NOTE]
> Sağlama yaklaşık 10-20 dakika sürer. Sağlama durumu Azure portalda görüntülenir.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makine erişme
VM oluşturulduktan sonra Uzak Masaüstü önceki yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanarak içine yapabilecekleriniz **Temelleri** bölümü. 

VM oluşturup sağlanan sonra yüklenmiş ve yapılandırılmış Araçları'nı kullanmaya başlamak hazırsınız. Başlat menüsü döşeme ve araçlarının çoğu masaüstü simgelerini vardır. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Microsoft Veri bilimi sanal makinede yüklü araçları

### <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench

Azure Machine Learning çalışma ekranı, bir masaüstü uygulaması ve komut satırı arabirimi olur. Çalışma ekranı bunları gerçekleştirirken, veri hazırlık adımlarını öğrenir yerleşik veri hazırlık vardır. Ayrıca, geçmiş ve üretkenliğinizi destekleyecek not defteri tümleştirme çalıştıran proje yönetimi sağlar. TensorFlow, Bilişsel araç seti, Spark ML ve scikit dahil olmak üzere en iyi açık kaynak çerçeveleri yararlanabilir-Modellerinizi geliştirmeyi öğrenin. DSVM üzerinde bir masaüstü simgesi (yerel olarak Azure Machine Learning çalışma ekranı her kullanıcının % LOCALAPPDATA % dizine ayıklayın InstallAMLFromLocal) sağlayın. Çalışma ekranı kullanması gereken her bir kullanıcı bir yapması gereken zaman ekranının kendi örneği yüklemek için InstallAMLFromLocal masaüstü simgesini çift eylemi. Azure Machine Learning da oluşturur ve % LOCALAPPDATA%\amlworkbench\python ayıklanan bir kullanıcı başına Python ortamı kullanır.

### <a name="microsoft-ml-server-developer-edition"></a>Microsoft ML Server Geliştirici sürümü
Microsoft enterprise kitaplıkları analizi için ölçeklenebilir R veya Python için kullanmak istiyorsanız, VM'nin (daha önce Microsoft R sunucusu olarak da bilinir) Microsoft ML Server Geliştirici sürümü yüklü olduğundan. Microsoft ML Server kapsamlı dağıtılabilir kurumsal sınıf analytics Platform R ve Python için kullanılabilir ve ölçeklenebilir, ticari olarak ve güvenli desteklenir. Büyük veri istatistikleri, Tahmine dayalı modelleme ve makine öğrenimi yetenekleri çeşitli destekleyici, ML sunucusu analytics – keşfi, analizi, Görselleştirme ve modelleme tam aralığını destekler. R ile kullanarak ve açık kaynak R ve Python genişletme Microsoft ML Server tamamen uyumludur / Python komut dosyaları, İşlevler ve CRAN / PIP / Conda paketleri, Kurumsal verileri çözümlemek için ölçeklendirin. Ayrıca, açık kaynak R bellek içi sınırlamaları'nin veri paralel ve öbekli işlenmesini ekleyerek giderir. Bu verileri analytics çok daha büyük ne ana bellekte uygun çalıştırmanıza olanak sağlar.  Visual Studio Community Edition VM dahil R veya Python ile çalışmak için tam bir IDE sağlar Visual Studio uzantısı için Visual Studio ve Python araçları için R araçları içerir. Ayrıca diğer IDE de gibi sağladığımız [Rstudio'dan](http://www.rstudio.com) ve [PyCharm Community edition](https://www.jetbrains.com/pycharm/) VM üzerinde. 

### <a name="python"></a>Python
Python kullanarak geliştirme için Anaconda Python 2.7 ve 3.5 dağıtım yüklendi. Bu dağıtım yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python içerir. Python araçları için Visual Studio (Visual Studio 2015 Community edition veya paketlenmiş IDE boşta veya Spyder gibi Anaconda ile biri içinde yüklü PTVS) kullanabilirsiniz. Bu arama çubuğunda arayarak birini başlatabilirsiniz (**Win** + **S** anahtar).

> [!NOTE]
> Anaconda Python 2.7 ve 3.5 Visual Studio için Python araçları işaret etmek için her sürüm için özel ortamları oluşturmanız gerekir. Bu ortam yolları Visual Studio 2015 Community Edition ayarlamak için gidin **Araçları** -> **Python Araçları** -> **Python ortamları**ve ardından **+ özel**. 
> 
> 

Anaconda Python 2.7 C:\Anaconda altında yüklenir ve c:\Anaconda\envs\py35 altında Anaconda Python 3.5 yüklenir. Bkz: [PTVS belgelerine](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) ayrıntılı adımlar için. 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Anaconda dağıtım ayrıca bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortamı bulunur. Jupyter not defteri sunucu Python 2.7, Python 3.5, PySpark, Jale ve R tekrar ile önceden yapılandırılmış. "Jupyter Jupyter sunucunun başlatmak ve not defteri sunucusuna erişmek için tarayıcıyı başlatın için Not Defteri" adlı bir masaüstü simgesi yoktur. 

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin. 
> 
> 

Biz birkaç örnek not defterlerini r ve Python de paketlenmiş Jupyter not defterlerini Jupyter erişim sonra Microsoft ML Server, SQL Server ML Hizmetleri (veritabanı Analytics'i), Python, Microsoft Bilişsel araç seti, Tensorflow ve diğer Azure teknolojileri ile çalışmaya nasıl gösterir. Bir önceki adımda oluşturduğunuz parola kullanarak Jupyter not defteri için kimlik doğrulaması sonra not defteri giriş sayfasında örnekler bağlantısını görebilirsiniz. 

### <a name="visual-studio-2017-community-edition"></a>Visual Studio 2017 Community edition
Visual Studio Community edition VM'de yüklü. Küçük ekipleri ve değerlendirme amaçları için kullanabileceğiniz bir Microsoft popüler IDE boş bir sürümüdür. Lisans sözleşmesinin koşullarını denetleyebilirsiniz [burada](https://www.visualstudio.com/support/legal/mt171547).  Masaüstü simgesini çift tıklayarak Visual Studio'yu açın veya **Başlat** menüsü. Ayrıca programlarla arayabilirsiniz **Win** + **S** ve "Visual Studio" girme. C#, Python, R, node.js gibi dillerde projeleri burada oluşturabilirsiniz sonra. Eklentileri de Azure veri Kataloğu, Azure Hdınsight (Hadoop, Spark) ve Azure Data Lake gibi Azure hizmetleriyle çalışmak uygun hale yüklenir. 

> [!NOTE]
> Değerlendirme süreniz doldu bildiren bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin veya Visual Studio Community Edition erişmek için yeni bir boş hesabı oluşturun. 
> 
> 

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Geliştirici sürümü
Geliştirici sürümü SQL Server 2017 veritabanı Analytics'i çalıştırmak için ML hizmetleriyle, R veya Python VM'de sağlanır. ML hizmetler geliştirmek ve akıllı uygulamaları dağıtmak için bir platform sağlar. Zengin ve güçlü bu diller ve topluluktan birçok paketleri modelleri oluşturun ve SQL Server verileri için tahminleri oluşturmak için kullanabilirsiniz. ML Hizmetleri (veritabanı-) R ve Python dil SQL Server içinde tümleştirir çünkü analytics veri yakın kullanmaya devam edebilir. Bu veri taşıma ile ilişkili güvenlik riskleri ve maliyetlerini ortadan kaldırır.

> [!NOTE]
> SQL Server Geliştirici sürümü yalnızca geliştirme ve test amaçları için kullanılabilir. Üretimde çalıştırmak için bir lisansınız olmalıdır. 
> 
> 

SQL server başlatarak erişebilirsiniz **SQL Server Management Studio**. VM adı sunucu adı olarak doldurulur. Windows Windows Yönetici olarak oturum açıldığında kimlik doğrulaması kullanın. SQL Server Management Studio'da olduktan sonra diğer kullanıcıları oluşturun, veritabanları oluşturmak, veri içeri aktarın ve SQL sorguları çalıştırma. 

Veritabanı Analytics'i SQL ML Hizmetleri kullanarak etkinleştirmek için bir tane aşağıdaki komutu çalıştırın zaman sunucu yönetici olarak oturum açtıktan sonra SQL Server management Studio'da eylemi. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Birkaç Azure Araçları VM'de yüklü:

* Azure SDK Belgeleri erişmek için bir masaüstü kısayolu yoktur. 
* **AzCopy**: Microsoft Azure Storage hesabınız ve dışındaki veri taşımak için kullanılır. Kullanımını görmek için şunu yazın **Azcopy** kullanımını görmek için komut isteminde. 
* **Microsoft Azure Storage Gezgini**: Azure depolama hesabı ve aktarım verilerinizi Azure depolama gelen ve giden içinde depolanan nesneleri göz atmak için kullanılır. Yazabilirsiniz **Depolama Gezgini** içinde arama veya bu aracı erişmek için Windows Başlat menüsünden bulunamıyor. 
* **Adlcopy**: Azure Data Lake verileri taşımak için kullanılır. Kullanımını görmek için şunu yazın **adlcopy** bir komut isteminde. 
* **dtui**: verileri Azure Cosmos DB, bir NoSQL veritabanı bulut üzerinde gelen ve giden taşımak için kullanılır. Tür **dtui** komut istemi üzerinde. 
* **Azure veri fabrikası tümleştirmesi çalışma zamanı**: şirket içi veri kaynakları ve bulut arasında veri taşıma sağlar. Azure Data Factory gibi araçlar içinde kullanılır. 
* **Microsoft Azure Powershell**: komut dosyası dili de kendi VM'nizi yüklü PowerShell'de Azure kaynaklarınızı yönetmek için kullanılan bir araçtır. 

### <a name="power-bi"></a>Power BI
Panolar ve harika görselleştirmeleri oluşturmanıza yardımcı olmak için **Power BI Desktop** yüklendi. Bu aracı panolarınızı ve raporlarınızı yazmak ve buluta yayımlamak için farklı kaynaklardan veri çekmek için kullanın. Bilgi için bkz: [Power BI](http://powerbi.microsoft.com) site. Power BI desktop Başlat menüsünde bulabilirsiniz. 

> [!NOTE]
> Power BI erişmek için Office 365 hesabı gerekir. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>Ek Microsoft geliştirme araçları
[ **Microsoft Web Platformu yükleyicisi** ](https://www.microsoft.com/web/downloads/platform.aspx) bulmak ve diğer Microsoft geliştirme araçları'nı indirmek için kullanılabilir. Microsoft Veri bilimi sanal makine masaüstünde sağlanan aracı için bir kısayol yoktur.  

## <a name="important-directories-on-the-vm"></a>VM önemli dizinleri
| Öğe | Dizin |
| --- | --- |
| Jupyter not defteri sunucu yapılandırmaları |C:\ProgramData\jupyter |
| Jupyter not defteri örnekleri giriş dizini |c:\dsvm\notebooks |
| Diğer örnekleri |c:\dsvm\samples |
| Anaconda (varsayılan: Python 2.7) |c:\Anaconda |
| Anaconda Python 3.5 ortamı |c:\Anaconda\envs\py35 |
| Microsoft ML Server tek başına Python  | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Varsayılan R örneği (ML Server tek başına) |C:\Program Files\Microsoft\ML Server\R_SERVER |
| SQL ML Hizmetleri veritabanı örnek dizini |C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER |
| Azure Machine Learning çalışma ekranı (her bir kullanıcı) | %LocalAppData%\amlworkbench | 
| Çeşitli araçlar |c:\dsvm\tools |

> [!NOTE]
> Örnekleri, Microsoft Veri bilimi (önce 3 Eylül 2016) 1.5.0 önce oluşturulan sanal makinenin bir biraz farklı dizin yapısı yukarıdaki tabloda belirtilenden kullanılır. 
> 
> 

## <a name="next-steps"></a>Sonraki Adımlar
Öğrenme ve araştırması devam etmek için İleri bazı adımlar şunlardır. 

* Çeşitli veri bilimi araçları veri bilimi VM Başlat menüsünden tıklatıp menüde listelenen araçları kullanıma keşfedin.
* Ürün ziyaret ederek Azure Machine Learning Hizmetleri ve çalışma ekranı hakkında bilgi edinin [quickstart ve öğreticiler sayfası](https://docs.microsoft.com/azure/machine-learning/preview/). 
* Gidin **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts** veri analizi Kurumsal ölçekte destekler R RevoScaleR kitaplıkta kullanan örnekler için.  
* Makaleyi okuyun: [veri bilimi sanal makine yapabileceğiniz 10 şey](http://aka.ms/dsvmtenthings)
* Sistematik olarak kullanarak uçtan uca analitik çözümleri oluşturmayı öğrenin [takım veri bilimi işlemi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Ziyaret [Azure Machine Learning galeri](http://gallery.cortanaintelligence.com) Azure üzerinde Azure Machine learning ve ilgili verileri kullanan machine learning ve veri analizi için örnek Hizmetleri. Ayrıca bir simge üzerinde sağladık **Başlat** menüsünde ve masaüstünde sanal makinenin Bu galeri.

