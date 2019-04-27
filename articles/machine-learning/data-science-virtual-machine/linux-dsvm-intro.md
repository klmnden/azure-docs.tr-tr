---
title: CentOS Linux veri bilimi sanal makinesi oluşturma
titleSuffix: Azure
description: Yapılandırın ve analiz ve makine öğrenimi için Azure'da bir Linux veri bilimi sanal makinesi oluşturun.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: e7b67905c96495382536555b87772e4eefada250
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60502345"
---
# <a name="provision-a-linux-centos-data-science-virtual-machine-on-azure"></a>Azure'da bir Linux CentOS veri bilimi sanal makinesi sağlama

Linux veri bilimi sanal makinesi bir CentOS tabanlı Azure sanal bir dizi önceden yüklenmiş aracı ile birlikte gelen makinesidir. Bu araçlar, veri analizi yapmak için yaygın olarak kullanılan ve makine öğrenimi. Dahil edilen önemli yazılım bileşenleri şunlardır:

* İşletim Sistemi: Linux CentOS dağıtım.
* Microsoft R Server Geliştirici sürümü
* Anaconda Python dağıtım (sürüm 2.7 ve 3.5) gibi popüler veri analiz kitaplıkları
* JuliaPro - Julia diline popüler bilimsel ve verileri analiz kitaplıkları seçkin bir dağıtımı
* Tek başına Spark örneğinde ve tek düğümlü Hadoop (HDFS, Yarn)
* JupyterHub - R, Python, PySpark, Julia çekirdekler destekleyen çok kullanıcılı bir Jupyter notebook sunucusu
* Azure Depolama Gezgini
* Azure komut satırı Azure kaynaklarını yönetmek için arabirimi (CLI)
* PostgresSQL veritabanı
* Machine learning araçları
  * [Bilişsel Araç Seti](https://github.com/Microsoft/CNTK): Derin Microsoft Research'nden yazılımları Araç Seti öğrenme vm'si.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Çevrimiçi allreduce, indirimleri, learning2search, etkin, karma ve etkileşimli gibi teknikler destekleyen bir hızlı makine öğrenimi sistemi öğrenme.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araç.
  * [Rattle](https://togaware.com/rattle/) (kolayca öğrenmek için R analitik aracı): Veri analizi ve r ile GUI tabanlı veri araştırması ile verileri kolay öğrenme ve otomatik R kod oluşturma ile modelleme makine kullanmaya başlama sağlayan araç.
* Azure SDK'sı, Java, Python, node.js, Ruby, PHP
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure Hizmetleri kullanma
* Geliştirme araçları ve Düzenleyicileri (RStudio, PyCharm, Intellij, Emacs, gedit, olduğu gibi vi)


Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verilerin önceden işlenmesi
1. Oluşturma ve modelleri test etme
1. Akıllı uygulamalarda kullanılmak modelleri dağıtma

Veri bilimcileri, bu görevleri tamamlamak için çeşitli araçlar kullanın. Bu oldukça zaman yazılım uygun sürümleri bulun ve sonra yüklemek için derlemek için alıcı ve bu sürümleri yükleyin.

Linux veri bilimi sanal makinesi, bu yükü önemli ölçüde kolaylaştırabilir. Analiz projenizi hızlı giriş yapmak için kullanın. R, Python, SQL, Java ve C++ gibi çeşitli dillerde görevler üzerinde çalışmanıza olanak tanır. Eclipse geliştirme ve kullanımı kolay olan kodunuzu test etmek için bir IDE sağlar. Azure SDK'ın sanal Makineye dahil çeşitli hizmetlere Linux üzerinde Microsoft bulut platformunu kullanarak uygulamalarınızı oluşturmanıza olanak tanır. Ayrıca, ayrıca önceden yüklü olan diğer dillere Ruby, Perl, PHP ve node.js gibi erişebilirsiniz.

Bu veri bilimi VM görüntüsü için hiçbir yazılım ücreti yoktur. Yalnızca VM görüntüsüyle sağlama sanal makine boyutuna göre değerlendirilen Azure donanım kullanım ücretleri ödeme yaparsınız. İşlem ücretleri hakkında daha fazla ayrıntı bulunabilir [VM listesi Azure Marketi sayfasında](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi diğer sürümleri
Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntüsüdür Ayrıca aynı araçları bir CentOS görüntüsü artı derin öğrenme çerçeveleri olarak birçoğu ile kullanılabilir. A [Windows](provision-vm.md) görüntü kullanılabilir de.

## <a name="prerequisites"></a>Önkoşullar
Linux veri bilimi sanal makinesi oluşturmadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliğine**: Abonelik sahibi için bkz: [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).
* **Azure depolama hesabınız**: Oluşturmak için bkz: [bir Azure depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md). Alternatif olarak, var olan bir hesabı kullanacak şekilde istemiyorsanız, depolama hesabı VM oluşturma işleminin bir parçası olarak oluşturulabilir.

## <a name="create-your-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makinenizi oluşturma
Bir örneği, Linux veri bilimi sanal makinesi oluşturmak için adımlar şunlardır:

1. Sanal makine üzerinde listeleme gidin [Azure portalında](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
1. Tıklayın **Oluştur** (altındaki) Kurma Sihirbazı getirilecek.![ Yapılandırma-data-bilimi-vm](./media/linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
1. Aşağıdaki bölümler, Microsoft Veri bilimi sanal makinesi oluşturmak için kullanılan girişleri her (önceki şekilde sağ tarafındaki numaralandırılan) sihirbazdaki adımları sağlar. Bu adımların her biri yapılandırmak için gerekli girişleri şunlardır:
   
   a. **Temel**:
   
   * **Ad**: Oluşturmakta olduğunuz veri bilimi sunucunuzun adıdır.
   * **Kullanıcı adı**: İlk hesap oturum açma kimliği
   * **Parola**: İlk hesap parolası (parola yerine SSH ortak anahtarını kullanabilirsiniz).
   * **Abonelik**: Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
   * **Kaynak grubu**: Yeni bir tane oluşturabilir veya varolan bir grubu kullanın.
   * **Konum**: En uygun veri merkezi seçin. Genellikle, verilerinizden en iyi sahip veya bu fiziksel konumunuza en hızlı ağ erişimi için en yakın veri merkezi bulunur.
   
   b. **Boyutu**:
   
   * Maliyet kısıtlamaları ve işlevsel bir gereksinimi karşılayan sunucusu türlerinden birini seçin. Seçin **görünümü tüm** VM boyutları, daha fazla seçenek görmek için.
   
   c. **Ayarları**:
   
   * **Disk türü**: Seçin **Premium** katı hal sürücüsü (SSD) tercih ederseniz. Aksi takdirde seçin **standart**.
   * **Depolama hesabı**: Aboneliğinizde yeni bir Azure depolama hesabı oluşturun veya mevcut bir şirket seçildi aynı konumda **Temelleri** sihirbazın.
   * **Diğer parametreler**: Çoğu durumda, yalnızca varsayılan değerleri kullanın. Varsayılan olmayan değerleri dikkate alınması gereken belirli alanlar hakkında Yardım için bilgi bağlantı üzerine gelin.
   
   d. **Özet**:
   
   * Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın.
   
   e. **Satın alma**:
   
   * Sağlama başlatmak için tıklatın **satın**. İşlemin koşullarının bağlantısı sunulur. VM, seçtiğiniz sunucu boyutu için işlem ötesinde herhangi bir ek ücreti yok **boyutu** adım.

Sağlama yaklaşık 10-20 dakika sürer. Sağlama durumunu Azure portalında görüntülenir.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makinesi erişme
VM oluşturulduktan sonra ona SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesabı kimlik bilgilerini kullan **Temelleri** bölümünde metin kabuk arabirimi için adım 3. Windows'da [Putty](https://www.putty.org) gibi bir SSH istemcisi aracı indirebilirsiniz. Bir grafik desktop (X Windows sistemi) tercih ederseniz, Putty üzerinde iletme X11 kullanın veya X2Go istemciyi yükleyin.

> [!NOTE]
> X2Go istemci gerçekleştirilen önemli ölçüde testinde iletme X11 iyidir. X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Yükleme ve X2Go istemci yapılandırma
Linux sanal makinesi zaten X2Go sunucusu ile sağlanan ve istemci bağlantılarını kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için istemcinizi aşağıdakileri yapın:

1. İstemci platformunuza yönelik X2Go istemcisini indirme ve yükleme [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
1. X2Go istemci çalıştırıp seçeneğini **yeni oturumu**. Bu, birden fazla sekme ile bir yapılandırma penceresi açılır. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Konak**: Ana bilgisayar adı veya IP adresini Linux veri bilimi sanal makinesi.
     * **Oturum açma**: Linux VM kullanıcı adı.
     * **SSH bağlantı noktası**: 22, varsayılan değeri bırakın.
     * **Oturum türü**: Değeri için XFCE değiştirin. Şu anda yalnızca Linux VM XFCE Masaüstü destekler.
   * **Ortam sekmesini**: Ses desteği ve yazdırma istemcisi kullanmanız gerekmez, bunları kapatabilirsiniz.
   * **Paylaşılan Klasörler**: Linux VM'de bağlı istemci makinelerden dizinleri istiyorsanız bu sekmedeki VM ile paylaşmak istediğiniz istemci makine dizinlerine ekleyin.

VM'ye SSH istemcisi veya XFCE grafik Masaüstü X2Go istemcisi aracılığıyla kullanarak oturum açtıktan sonra yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız. XFCE üzerinde çok sayıda araçla menüsü kısayolları uygulamalar ve masaüstü simgelerini görebilirsiniz.

## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makinede yüklü araçları
### <a name="microsoft-r-server"></a>Microsoft R Server
R veri analizi ve makine öğrenimi için en popüler diller biridir. R analiz için kullanmak istiyorsanız, VM matematik çekirdek kitaplığı (MKL) ve Microsoft R Open (MRO) ile Microsoft R Server (MRS) sahiptir. Matematik işlemlerinden analitik algoritmaları ortak MKL iyileştirir. MRO yüzde 100'ün üzerinde CRAN R ile uyumlu olan ve herhangi bir CRAN'de yayımlanan R kitaplıkları MRO üzerinde yüklenebilir. MRS, ölçeklendirme ve kullanıma hazır hale getirme, R modellerinin web hizmetleri sağlar. RStudio, VI, Emacs veya gedit gibi varsayılan düzenleyicilerden biriyle R programlarınızın düzenleyebilirsiniz. Emacs Düzenleyicisi'ni kullanıyorsanız Emacs basitleştiren Sırala (Emacs konuşur istatistikleri) paketini Not önceden yüklenmiş ve Emacs Düzenleyici içindeki R dosyalarıyla çalışma olmuştur.

Başlatma için R konsolunda, yalnızca yazdığınız **R** Kabuğu'nda. Bu sizi, etkileşimli bir ortam götürür. R programınızı geliştirmek için genellikle Emacs veya olduğu gibi vi veya gedit gibi bir düzenleyici kullanın ve ardından içinde r betikleri çalıştırın RStudio ile R programınızı geliştirmek için tam grafik bir IDE ortamını sahip.

Yüklemeniz için bir R betiğini de mevcuttur [üst 20 R paketleri](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) istiyorsanız. (Belirtildiği gibi) yazarak girilebilir R etkileşimli arabiriminde olduktan sonra bu betiği çalıştırın **R** Kabuğu'nda.  

### <a name="python"></a>Python
Anaconda Python dağıtım 2.7 ve 3.5 Python aracılığıyla geliştirmeye yönelik, yüklendi. Bu dağıtım, temel bir Python yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra içerir. Varsayılan metin düzenleyicisi kullanabilirsiniz. Ayrıca, Spyder, Anaconda Python dağıtımları ile birlikte bir Python IDE kullanabilirsiniz. Grafik bir masaüstü veya X11 Spyder gereken iletme. Grafik Desktop'ta Spyder kısayolu sağlanır.

Biz hem Python 2.7 hem de 3.5 olduğundan, özellikle geçerli oturumda çalışmak istediğiniz istenen Python sürümünü (conda ortamı) etkinleştirmeniz gerekir. Etkinleştirme işlemi, Python'un istenen sürümüyle için PATH değişkenini ayarlar.

Python 2.7 conda ortam etkinleştirmek için aşağıdaki komut kabuğundan çalıştırın:

    source /anaconda/bin/activate root

Python 2.7 yüklenir */anaconda/bin*.

Python 3.5 conda ortam etkinleştirmek için aşağıdaki kabuğundan çalıştırın:

    source /anaconda/bin/activate py35


Python 3.5 kurulmuştur */anaconda/envs/py35/bin*.

Bir Python etkileşimli oturumu çağırmak için yazmanız yeterlidir **python** Kabuğu'nda. Bir grafik arabirimde olan veya yedekleme kümesi iletme X11 varsa yazabilirsiniz **pycharm** PyCharm Python IDE başlatmak için.

Ek Python kitaplıklarını yüklemek için çalıştırmanız gerekir ```conda``` veya ```pip``` komutu sudo altında ve Python Paket Yöneticisi (conda veya pip) için doğru Python ortamını yüklemek için tam yolunu sağlayın. Örneğin:

    sudo /anaconda/bin/pip install <package> #pip for Python 2.7
    sudo /anaconda/envs/py35/bin/pip install <package> #pip for Python 3.5
    sudo /anaconda/bin/conda install [-n py27] <package> #conda for Python 2.7, default behavior
    sudo /anaconda/bin/conda install -n py35 <package> #conda for Python 3.5


### <a name="jupyter-notebook"></a>Jupyter notebook
Anaconda dağıtım bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter not defteri JupyterHub erişilir. Yerel Linux kullanıcı adınızı ve parolanızı kullanarak oturum açın.

Jupyter notebook sunucusu Python 2, Python 3 ve R çekirdekler ile önceden yapılandırıldı. Not Defteri sunucuya erişmek için tarayıcıyı başlatmak için "Jupyter Notebook" adlı bir masaüstü simgesi vardır. VM X2Go ya da SSH istemcisi kullanıyorsanız, da ziyaret edebilirsiniz [ https://localhost:8000/ ](https://localhost:8000/) Jupyter notebook sunucusu erişmek için.

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin.
> 
> 

Jupyter notebook sunucusu herhangi bir ana bilgisayardan erişebilirsiniz. Yazmanız yeterlidir *https://\<VM DNS adı veya IP adresi\>: 8000 /*

> [!NOTE]
> VM hazırlandığında 8000 numaralı bağlantı noktasını Güvenlik Duvarı'nda varsayılan olarak açılır.
> 
> 

Size örnek not defterleri--bir da Python ve R'dir birinde paketlediğinizden Jupyter not defterine yerel Linux kullanıcı adı ve parola kullanarak kimlik doğrulama işleminden sonra not defteri giriş sayfasında örnekleri bağlantısını görebilirsiniz. Seçerek yeni bir not defteri oluşturabilirsiniz **yeni**ve ardından uygun dil çekirdek. Görmüyorsanız **yeni** düğmesini tıklatın, **Jupyter** notebook sunucusu giriş sayfasına dönmek için sol üstteki simgesi.

### <a name="apache-spark-standalone"></a>Tek başına Apache Spark 
Tek başına bir örneğini Apache Spark, Spark uygulamalarını yerel olarak test etme ve büyük kümelerde dağıtımı önce ilk geliştirmenize yardımcı olması için bu Linux DSVM'sini önceden yüklenir. PySpark programlar Jupyter çekirdek çalıştırabilirsiniz. Ne zaman Jupyter açın ve tıklayın **yeni** düğmesi kullanılabilir çekirdekler listesini görmeniz gerekir. Spark Python dil kullanan uygulamalar oluşturmanıza olanak tanıyan PySpark çekirdeği "Spark – Python" dir. Bir Python IDE PyCharm veya Spyder gibi Spark programını oluşturmak için kullanabilirsiniz. Sonra bu tek başına bir örneğini, Spark yığın içinde arama istemci programı çalıştırır. Bu, daha hızlı ve kolay bir Spark kümesi üzerinde geliştirme ile karşılaştırıldığında sorunlarını gidermek sağlar. 

Bir örnek PySpark Not Defteri, Jupyter ($ giriş/dizüstü/SparkML/pySpark) giriş dizininin altında "SparkML" dizininde bulabilirsiniz Jupyter üzerinde sağlanır. 

Spark için R programlama yapıyorsanız, Microsoft R Server, SparkR veya sparklyr kullanabilirsiniz. 

Microsoft R Server Spark bağlamında çalıştırmadan önce kurulum adımı yerel bir tek düğümlü Hadoop HDFS ve Yarn örneği etkinleştirmek için bir kere yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklendi ancak DSVM'nin devre dışı. Bunu etkinleştirmek için aşağıdaki komutları kök olarak ilk kez çalıştırma gerekir:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Hadoop durdurabilirsiniz çalıştırarak ihtiyacınız olduğunda ilgili hizmetler ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn``` geliştirip (Bu tek başına Spark örneğinde DSVM) uzaktan Spark bağlamında MRS test nasıl yazılacağını gösteren bir örnek sağlanan ve kullanılabilir `/dsvm/samples/MRS` Dizin. 

### <a name="ides-and-editors"></a>IDE'ler ve düzenleyicilerden
Birkaç kod düzenleyicilerinden, vardır. Bu, VI/VIM ve Emacs, gEdit, PyCharm, RStudio, Eclipse ve Intellij içerir. Eclipse, Intellij, RStudio ve PyCharm gEdit grafik düzenleyicilerden olan ve bunları kullanmak için bir grafik masaüstü oturum açmanız gerekir. Bu düzenleyicilerin Masaüstü ve uygulama vardır. bunları başlatmak için kısayol menüsü.

**VIM** ve **Emacs** olan metin tabanlı düzenleyiciler. Emacs üzerinde biz Emacs konuşur istatistikleri (Emacs Düzenleyici içindeki R ile çalışmayı kolaylaştırır, EES) adlı bir eklenti paketi yüklediniz. Daha fazla bilgi şu adreste bulunabilir: [Sırala](https://ess.r-project.org/).

**Eclipse** olan açık kaynak, birden çok dili destekleyen Genişletilebilir IDE. Java geliştiricileri edition VM'de yüklü örneğidir. Eklentiler için ortamı genişletmek için yüklenebilen birçok popüler dilde kullanılabilir. Adlı Eclipse'te yüklü bir eklenti de sahibiz **Eclipse için Azure Araç Seti**. Oluşturma, geliştirme, test ve Java gibi dilleri destekler Eclipse geliştirme ortamı ile Azure uygulamalarını dağıtma olanak tanır. Ayrıca bir **Java için Azure SDK'sı** , çeşitli Azure hizmetlerini bir Java ortamında erişim sağlar. Eclipse için Azure Araç Seti hakkında daha fazla bilgi şu adreste bulunabilir: [Eclipse için Azure Araç Seti](../../azure-toolkit-for-eclipse.md).

**LaTex** Emacs eklenti ile birlikte texlive paketi aracılığıyla yüklenen [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) paketini LaTex belgelerinizi Emacs içinde yazma basitleştirir.  

### <a name="databases"></a>Veritabanları
#### <a name="postgres"></a>Postgres
Açık kaynak veritabanı **Postgres** VM üzerinde çalışan hizmetleri ve zaten tamamlanmış initdb ile kullanılabilir. Yine de veritabanları ve kullanıcılar oluşturmanız gerekir. Daha fazla bilgi için [Postgres belgeleri](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Grafik SQL istemcisi
**SQuirrel SQL**, (örneğin, Microsoft SQL Server, Postgres ve MySQL gibi) farklı veritabanlarına bağlanmak için ve SQL sorguları çalıştırmak için bir grafik SQL istemci'nin sağlamış. Bu (örneğin X2Go istemci kullanarak) bir grafik Masaüstü oturumundan çalıştırabilirsiniz. SQuirrel SQL çağırmak için masaüstünde simgesinden başlatın veya kabuk aşağıdaki komutu çalıştırın.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

İlk kullanılmadan önce sürücüleri ve veritabanı diğer adlar ayarlayın. JDBC sürücüleri şu adreste bulunabilir:

*/usr/Share/Java/jdbcdrivers*

Daha fazla bilgi için [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server'a erişmek için komut satırı araçları
SQL Server için ODBC sürücü paketi ayrıca iki komut satırı araçları ile birlikte gelir:

**BCP**: Bcp yardımcı programını toplu bir kullanıcı tarafından belirtilen biçimde bir Microsoft SQL Server örneğini bir veri dosyası arasında veri kopyalar. Çok sayıda yeni satırı SQL Server tablolarına aktarmak veya veri tablolar dışında veri dosyalarına veri aktarmak için bcp yardımcı programı kullanılabilir. Bir tabloya veri almak için bu tabloda oluşturulmuş bir biçim dosyası kullanmak, veya tabloyu ve sütunlarını için geçerli olan veri türleri yapısını anlayın.

Daha fazla bilgi için [bcp ile bağlanma](https://msdn.microsoft.com/library/hh568446.aspx).

**SQLCMD**: Sqlcmd yardımcı programını, hem de sistem yordamlar ve komut dosyaları komut isteminde, Transact-SQL deyimleriyle girebilirsiniz. Bu yardımcı programı, Transact-SQL toplu işlerini yürütmek için ODBC kullanır.

Daha fazla bilgi için [sqlcmd ile bağlanma](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Bu yardımcı programı, Linux ve Windows platformlarındaki arasında bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.
> 
> 

#### <a name="database-access-libraries"></a>Veritabanı erişimi kitaplıkları
Access veritabanları için R ve Python kullanılabilen kitaplıkları vardır.

* R ile **RODBC** paket veya **dplyr** paket sorgulamak ya da veritabanı sunucusunda SQL deyimlerini yürütmek olanak tanır.
* Python **pyodbc** kitaplığı, temel katmanı olarak ODBC ile veritabanı erişimi sağlar.  

Erişim için **Postgres**:

* R: Paket kullanarak **RPostgreSQL**.
* Python'dan: Kullanım **psycopg2** kitaplığı.

### <a name="azure-tools"></a>Azure Araçları
Aşağıdaki Azure Araçları VM'de yüklü:

* **Azure komut satırı arabirimi**: Azure CLI, Kabuk komutları aracılığıyla Azure kaynaklarını oluşturmak ve yönetmek sağlar. Azure araçlarını çağırmak için yazmanız yeterlidir **azure Yardımı**. Daha fazla bilgi için [Azure CLI belgeleri sayfasını](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Depolama Gezgini**: Microsoft Azure Depolama Gezgini, Azure depolama hesabınızdaki depoladığınız nesnelerin göz atın ve karşıya yükleme ve verileri Azure bloblarından indirmek için kullanılan bir grafik aracıdır. Depolama Gezgini masaüstü kısayolu simgesinden erişebilirsiniz. Yazarak, bir kabuk isteminde çağırabilirsiniz **StorageExplorer**. Bir X2Go istemcisinden imzalanması veya yedekleme kümesi iletme X11 olması gerekir.
* **Azure kitaplıkları**: Önceden yüklenmiş kitaplıkları bazıları aşağıda verilmiştir.
  
  * **Python**: Azure ile ilgili kitaplıklar yüklenen python'da **azure**, **azureml**, **pydocumentdb**, ve **pyodbc**. İlk üç kitaplıkları ile Azure depolama hizmetleri, Azure Machine Learning ve Azure Cosmos DB (Azure üzerinde bir NoSQL veritabanı) erişebilir. Dördüncü kitaplığı pyodbc (yanı sıra Microsoft ODBC sürücüsü için SQL Server) erişimi etkinleştirir SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı python'dan ODBC arabirimini kullanarak. Girin **pip listesi** listelenen tüm kitaplıkları görmek için. Hem Python 2.7 hem de 3,5 ortamlarında bu komutu çalıştırmak emin olun.
  * **R**: Azure ile ilgili kitaplıklar r yüklü **AzureML** ve **RODBC**.
  * **Java**: Azure Java kitaplıkları listesini dizininde bulunabilir **/dsvm/sdk/AzureSDKJava** VM üzerinde. Anahtar kitaplıkları, SQL Server için Azure depolama ve Yönetimi API'leri, Azure Cosmos DB ve JDBC sürücüleri vardır.  

Erişebildiğiniz [Azure portalında](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısı. Azure portalında, oluşturmak, yönetmek ve Azure kaynaklarınızı izleyin.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning oluşturmanızı, dağıtmanızı ve Tahmine dayalı analiz çözümlerini sağlayan tam olarak yönetilen bir bulut hizmetidir. Azure Machine Learning Studio'dan denemeleri ve modeller oluşturun. Ziyaret ederek veri bilimi sanal makinesi bir web tarayıcısından erişilebileceğini [Microsoft Azure Machine Learning](https://studio.azureml.net).

Azure Machine Learning Studio'da oturum açtıktan sonra deneme tuvaline Burada, makine öğrenimi algoritmaları için mantıksal bir akış oluşturabilirsiniz erişebilirsiniz. Ayrıca Azure Machine Learning üzerinde barındırılan bir Jupyter not defteri erişimi ve Machine Learning Studio'da denemeleri ile sorunsuz bir şekilde çalışabilir. Machine learning web hizmeti arabiriminde sarmalama tarafından oluşturulmuş modelleri kullanıma hazır hale getirin. Bu, makine öğrenimi modellerini tahminleri çağırmak istemcilerin herhangi bir dilde yazılmış sağlar. Daha fazla bilgi için [Machine Learning belgeleri](https://azure.microsoft.com/documentation/services/machine-learning/).

Ayrıca VM üzerinde Modellerinizi R veya Python'ı oluşturun ve Azure Machine Learning'i üretim ortamında dağıtın. Biz R kitaplıkları yüklü (**AzureML**) ve Python (**azureml**) bu işlevselliği etkinleştirmek için.

R ve Python modeller Azure Machine Learning içine dağıtma hakkında daha fazla bilgi için bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz on işlem](vm-do-ten-things.md) (özellikle bölümü "R veya Python'ı kullanarak modelleri oluşturma ve bunları kullanıma hazır hale getirme kullanma Azure Machine Learning").

> [!NOTE]
> Bu yönergeler, veri bilimi sanal makinesi Windows sürümü için yazılmıştır. Ancak bilgileri var. için Azure Machine Learning modelleri dağıtma hakkında Linux VM'ye uygun sağlanır.
> 
> 

### <a name="machine-learning-tools"></a>Machine learning araçları
VM birkaç makine öğrenme araçları ve önceden derlenmiş ve yerel olarak yüklenmiş algoritmalar ile birlikte gelir. Bunlar:

* **Microsoft Bilişsel Araç Seti** : Derin öğrenme araç setidir.
* **Vowpal Wabbit**: Bir hızlı çevrimiçi öğrenme algoritması.
* **xgboost**: En iyi duruma getirilmiş, artırmalı ağacı algoritmalarını sağlayan bir araç.
* **Python**: Anaconda Python makine öğrenimi algoritmalarıyla Scikit-öğrenme gibi kitaplıkları ile sunulur. Diğer kitaplıkları kullanarak yükleyebileceğiniz `pip install` komutu.
* **R**: Machine learning işlevleri zengin kitaplığı r için kullanılabilir Önceden yüklenen kitaplıklar bazıları lm, glm, randomForest, rpart. Diğer kitaplıkları çalıştırarak yüklenebilir:
  
        install.packages(<lib name>)

Listedeki ilk üç machine learning araçları hakkında bazı ek bilgiler aşağıda verilmiştir.

#### <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti
Açık kaynaklı, derin öğrenme Araç Seti budur. Bir komut satırı aracı (cntk) ve yolunda zaten.

Temel bir örnek çalıştırmak için kabukta aşağıdaki komutları yürütün:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Daha fazla bilgi için bkz: CNTK bölümünü [GitHub](https://github.com/Microsoft/CNTK)ve [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit olan bir makine öğrenimi çevrimiçi, karma, allreduce, indirimleri, learning2search, etkin, gibi teknikler kullanan sistemi ve etkileşimli öğrenme.

Aracı üzerinde basit bir örneği çalıştırmak için aşağıdakileri yapın:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Bu dizinde büyük, diğer tanıtımlar vardır. VW hakkında daha fazla bilgi için bkz. [GitHub'un bu bölümünde](https://github.com/JohnLangford/vowpal_wabbit)ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Bu tasarlanmış ve artırmalı (ağaç) algoritmaları için en iyi duruma getirilmiş bir kitaplıktır. Amacı, bu kitaplık, büyük ölçekli ağacı, yükseltme sağlamak için gerekli uç makineler hesaplama sınırlarını göndermek için ölçeklenebilir, taşınabilir ve doğru ' dir.

Bir R kitaplığı yanı sıra bir komut satırı sağlanır.

Bu kitaplıkta bir R kullanmak için R etkileşimli bir oturum başlatabilirsiniz (yazarak yalnızca **R** Kabuğu'nda) ve Kitaplığı yükleyin.

İsteminde R çalıştırarak basit bir örnek aşağıda verilmiştir:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Xgboost komut satırını çalıştırmak için kabukta yürütülecek komutlar şunlardır:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


.Model dosya belirtilen dizine yazılır. Bu Tanıtım örnek hakkında daha fazla bilgi bulunabilir [github'da](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Xgboost hakkında daha fazla bilgi için bkz: [xgboost belgeleri sayfasını](https://xgboost.readthedocs.org/en/latest/)ve onun [GitHub deposu](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Çıngırağı
Rattle ( **R** **A**nalitik **T**aracı **T**o **L**kazanın **E** GUI tabanlı bir veri keşfi ve modelleme asily) kullanır. Veriler, kolayca modellenebilir, denetimli hem de Denetimsiz modellerinden veri yapıları, grafik performansını modelleri sunar dönüşümler veriler istatistiksel ve görsel özetlerini sunar ve puanları yeni veri kümeleri. Ayrıca R kodunu doğrudan R çalıştırmak veya daha fazla analiz için başlangıç noktası olarak kullanılan işlemleri kullanıcı arabiriminde çoğaltma oluşturur.

Çıngırağı çalıştırmak için bir grafik Masaüstü Oturum açma oturumunda olması gerekir. Terminalde yazın ```R``` R ortamı girmek için. R isteminde aşağıdaki komutları girin:

    library(rattle)
    rattle()

Bir sekme kümesi ile artık bir grafik arabirim açılır. Aşağıda bir örnek hava durumu veri kümesini kullanan ve bir model oluşturmak için gereken Çıngırağı içinde hızlı başlangıç adımlarını verilmiştir. Bazı adımları otomatik olarak yüklemesini ve sistemde olmayan bazı gerekli R paketlerini yük sorulur.

> [!NOTE]
> Sistem dizininde (varsayılan) paketini yüklemek için erişiminiz yoksa Kişisel kitaplığınıza paketleri yüklemek için R konsol penceresinde bir ileti görebilirsiniz. Yanıt *y* varsa, bu yönergeleri.
> 
> 

1. **Yürüt**'e tıklayın.
1. Hava durumu örnek veri kümesini kullanmak isteyip soran bir iletişim kutusu açılır. Tıklayın **Evet** örnek yüklenemedi.
1. Tıklayın **modeli** sekmesi.
1. Tıklayın **yürütme** karar ağacı oluşturmak için.
1. Tıklayın **çizmek** karar ağacı görüntülenecek.
1. Tıklayın **orman** radyo düğmesini tıklatıp tıklayın **yürütme** rastgele bir orman oluşturmak için.
1. Tıklayın **değerlendir** sekmesi.
1. Tıklayın **Risk** radyo düğmesini tıklatıp tıklayın **yürütme** iki Risk (toplu) performans çizimleri görüntülenecek.
1. Tıklayın **günlük** önceki işlemleri Oluştur R kodunu göstermek için sekmesinde.
   (Eklemek ihtiyacınız Çıngırağı geçerli sürümünde bir hata nedeniyle bir *#* önüne karakter *... Bu günlüğünü dışarı aktarma*  metin günlüğü.)
1. Tıklayın **dışarı** adlı R betiği kaydetmek için *weather_script. R* için giriş klasörü.

Çıngırağı ve R'dir Çık Şimdi oluşturulan R betiğini değiştirebilir veya çalıştırmak için her zaman içinde Rattle UI yapılmış her şeyi yinelemek için olduğu gibi kullanabilirsiniz. Özellikle yeni başlayanlar için r ile hızlı bir şekilde analizlerini ve otomatik olarak değiştirmek ve/veya öğrenmek için R kod oluşturma sırasında basit bir grafik arabirim learning'de makine için kolay bir yolu budur.

## <a name="next-steps"></a>Sonraki adımlar
İşte öğrenme ve araştırma nasıl devam edebilirsiniz:

* [Veri bilimi üzerinde Linux veri bilimi sanal makinesi](linux-dsvm-walkthrough.md) izlenecek yol, Linux veri bilimi buraya sağlanan VM ile çeşitli genel veri bilimi görevlerini gerçekleştirmek nasıl gösterir. 
* Çeşitli veri bilimi araçlarını, veri bilimi sanal makinesi, bu makalede açıklanan araçları deneyerek keşfedin. Ayrıca çalıştırabileceğiniz *dsvm daha fazla bilgi* Kabuk temel bir giriş ve işaretçileri VM'de yüklü araçları hakkında daha fazla bilgi için sanal makine içinde.  
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
* Ziyaret [Cortana Analytics Galerisi](https://gallery.cortanaanalytics.com) Cortana Analytics Suite kullanan makine öğrenimi ve veri analizi örnekleri için.

