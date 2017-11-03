---
title: "Azure üzerinde bir Linux CentOS veri bilimi sanal makine sağlama | Microsoft Docs"
description: "Yapılandırın ve analizi yapabilir ve makine Azure'da bir Linux veri bilimi sanal makine oluşturun."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: bradsev
ms.openlocfilehash: e36c28ef1c05dcdcebc7372316c7f144c92fd02f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="provision-a-linux-centos-data-science-virtual-machine-on-azure"></a>Azure üzerinde bir Linux CentOS veri bilimi sanal makine sağlama

Linux veri bilimi sanal makine bir CentOS tabanlı Azure sanal önceden yüklenmiş bir araç koleksiyonu ile birlikte gelen makinedir. Bu araçlar, veri analizi yapmak için yaygın olarak kullanılır ve makine öğrenme. Dahil önemli yazılım bileşenleri şunlardır:

* İşletim sistemi: Linux CentOS dağıtım.
* Microsoft R Server Geliştirici sürümü
* Popüler veri analiz kitaplıkları anaconda Python dağıtımı (sürüm 2.7 ve 3.5) dahil olmak üzere
* JuliaPro - Jale dili popüler bilimsel ve veri analizi kitaplıkları ile seçkin dağıtılması
* Tek başına Spark örneğinde ve tek düğümlü Hadoop (HDFS, Yarn)
* JupyterHub - R, Python, PySpark, Jale tekrar destekleyen çok kullanıcılı bir Jupyter not defteri sunucusu
* Azure Depolama Gezgini
* Azure komut satırı Azure kaynaklarını yönetmek için arabirimi (CLI)
* PostgresSQL veritabanı
* Machine learning araçları
  * [Bilişsel Araç Seti](https://github.com/Microsoft/CNTK): Microsoft Research yazılım araç Seti'nin öğrenme derin.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): çevrimiçi, karma, allreduce, düşürülmesi, learning2search, etkin, gibi teknikler destekleme sistem öğrenme hızlı bir makine ve etkileşimli öğrenme.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): hızlı ve doğru boosted ağaç uygulama sağlayan bir araç.
  * [Rattle](http://rattle.togaware.com/) (R analitik aracı için bilgi kolayca): veri analizi ve R ile GUI tabanlı veri keşfi kolay öğrenme ve otomatik R kod oluşturma ile modelleme makine ile çalışmaya başlama sağlayan bir araç.
* Azure SDK Java, Python, node.js, Ruby, PHP
* Azure Machine Learning ve diğer Azure hizmetleriyle R ve Python için kitaplıkları kullanma
* Geliştirme araçları ve Düzenleyicileri (Rstudio'dan, PyCharm, Intellij, Emacs, gedit, VI)


Veri Bilimi bulunurken bir dizi görev yineleme içerir:

1. Bulma, yükleme ve verilerin önceden işlenmesi
2. Derleme ve modelleri test etme
3. Akıllı uygulamaları tüketimini modellerini dağıtma

Veri bilimcilerine, bu görevleri tamamlamak için çeşitli araçları kullanın. Bu oldukça zaman yazılım uygun sürümlerini bulmak ve yüklemek için derleme için alabilir ve bu sürümleri yükleyin.

Linux veri bilimi sanal makine bu yük önemli ölçüde kolaylaştırabilir. Analytics projenizi hızla başlatmak için bunu kullanın. R, Python, SQL, Java ve C++ dahil olmak üzere çeşitli dillerde görevler üzerinde çalışmanıza olanak tanır. Eclipse geliştirmek ve kullanımı kolay kodunuzu test etmek için bir IDE sağlar. Azure VM'yi dahil SDK'sı, çeşitli hizmetlere Linux'ta Microsoft bulut platformu kullanarak uygulamalarınızı oluşturmanıza olanak verir. Ayrıca, aynı zamanda önceden yüklü olan diğer dillere Ruby, Perl, PHP ve node.js gibi erişebilirsiniz.

Bu veri bilimi VM görüntüsü için yazılım harcamanız yok. VM görüntüsü ile sağlamak sanal makine boyutuna göre uygunluk Azure donanım kullanım ücretleri ödersiniz. İşlem ücretleri hakkında daha fazla ayrıntı bulunabilir [VM listeleme Azure Marketi sayfasında](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Veri bilimi sanal makinenin diğer sürümleri
Bir [Ubuntu](dsvm-ubuntu-intro.md) görüntüdür de aynı araçları CentOS görüntü artı çerçeveleri öğrenme ayrıntılı olarak birçoğu ile kullanılabilir. A [Windows](provision-vm.md) görüntü kullanılabilir de.

## <a name="prerequisites"></a>Ön koşullar
Linux veri bilimi sanal makine oluşturmadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**: bir tane almak için bkz: [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).
* **Bir Azure depolama hesabı**: oluşturmak için bkz: [bir Azure depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account). Alternatif olarak, var olan bir hesabı kullanacak şekilde istemiyorsanız, depolama hesabı VM oluşturma işleminin bir parçası olarak oluşturulabilir.

## <a name="create-your-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makine oluşturma
Örnek, Linux veri bilimi sanal makine oluşturmak için adımlar şunlardır:

1. Sanal makine üzerinde listeleme gidin [Azure portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2. Tıklatın **oluşturma** (en altta) Kurma Sihirbazı getirilecek.![ yapılandırma verileri-Bilim-vm](./media/linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3. Aşağıdaki bölümler Microsoft Veri bilimi sanal makine oluşturmak için kullanılan girişleri her (önceki şekil sağ tarafta numaralandırılan) Sihirbazı'ndaki adımları sağlar. Bu adımların her biri yapılandırmak için gereken girdiler şunlardır:
   
   a. **Temel kavramları**:
   
   * **Ad**: oluşturduğunuz veri bilimi sunucunuzun adını yazın.
   * **Kullanıcı adı**: ilk hesap oturum açma kimliği.
   * **Parola**: ilk hesap parolası (kullanabilirsiniz SSH ortak anahtarı parola yerine).
   * **Abonelik**: birden fazla aboneliğiniz varsa, bir makine olduğu oluşturulur ve fatura için seçin. Bu abonelik için kaynak oluşturma ayrıcalıkları olmalıdır.
   * **Kaynak grubu**: yeni bir tane oluşturun veya varolan bir grubu kullanın.
   * **Konum**: en uygun olan veri merkezi seçin. Genellikle verilerinizden en iyi olan ya da fiziksel konumunuza en hızlı ağ erişimi için en yakın veri merkezinin olur.
   
   b. **Boyutu**:
   
   * İşlev gereksinimi ve maliyet kısıtlamaları karşılayan sunucu türlerinden birini seçin. Seçin **tümünü görüntüle** VM boyutları, daha fazla seçenek görmek için.
   
   c. **Ayarları**:
   
   * **Disk türü**: seçin **Premium** katı hal sürücüsü (SSD) tercih ederseniz. Aksi takdirde seçin **standart**.
   * **Depolama hesabı**: aboneliğinizde yeni bir Azure depolama hesabı oluşturun veya mevcut bir üzerinde seçildi aynı konumda kullanın **Temelleri** sihirbazın.
   * **Diğer parametreler**: Çoğu durumda, yalnızca varsayılan değerleri kullanırsınız. Varsayılan olmayan değerleri dikkate alınması gereken belirli alanlar hakkında Yardım için bilgi bağlantı üzerine gelerek.
   
   d. **Özet**:
   
   * Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın.
   
   e. **Satın**:
   
   * Sağlama başlatmak için tıklatın **satın**. Bağlantı işlem koşullarını sağlanır. VM, seçtiğiniz sunucu boyutu işlem ötesinde herhangi bir ek ücret yok **boyutu** adım.

Sağlama yaklaşık 10-20 dakika sürer. Sağlama durumu Azure portalda görüntülenir.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makine erişme
VM oluşturulduktan sonra kendisine SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesap kimlik bilgilerini kullanan **Temelleri** adım 3 metin kabuk arabirimi için bölüm. Windows, bir SSH istemcisi aracı gibi indirebilirsiniz [Putty](http://www.putty.org). Grafik Masaüstü (X Windows sistemi) tercih ederseniz, Putty iletme X11 kullanın veya X2Go istemcisi yükleyin.

> [!NOTE]
> X2Go istemci gerçekleştirilen önemli ölçüde testinde iletme X11 daha iyi. X2Go istemci için bir grafik Masaüstü arabirimi kullanmanızı öneririz.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Yükleme ve X2Go istemci yapılandırma
Linux VM X2Go sunucusu ile sağlanan ve istemci bağlantılarını kabul etmeye hazır zaten var. Linux VM grafik masaüstüne bağlanmak için istemci üzerinde aşağıdakileri yapın:

1. İstemci platformunuzu X2Go istemci yükleyip [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. X2Go istemci çalıştırmak ve seçmek **yeni oturum**. İle birden çok sekme yapılandırma penceresi açar. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Ana bilgisayar**: ana bilgisayar adı veya IP adresini, Linux veri bilimi VM.
     * **Oturum açma**: Linux VM kullanıcı adı.
     * **SSH bağlantı noktası**: 22, varsayılan değeri bırakın.
     * **Oturum türü**: XFCE için değeri değiştirin. Şu anda Linux VM yalnızca XFCE Masaüstü destekler.
   * **Ortam sekmesini**: ses desteği ve yazdırma istemcisi kullanmanız gerekmiyorsa, bunları kapatabilirsiniz.
   * **Paylaşılan Klasörler**: Linux VM'de bağlı istemci makinelerden dizinleri istiyorsanız bu sekmedeki VM paylaşmak istediğiniz istemci makine dizinleri ekleyin.

VM SSH istemcisi veya XFCE grafik Masaüstü X2Go istemcisinden kullanarak oturum açtıktan sonra yüklenmiş ve yapılandırılmış VM Araçları'nı kullanmaya başlamak hazırsınız. XFCE üzerinde uygulamaları menüsü kısayolları ve masaüstü simgelerini araçları çoğunu görebilirsiniz.

## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makinede yüklü araçları
### <a name="microsoft-r-server"></a>Microsoft R Server
R en popüler diller veri analizi ve makine öğrenme için biridir. R analizi için kullanmak istiyorsanız, VM matematik çekirdek kitaplığı (MKL) ve Microsoft R Aç (MRO) ile Microsoft R Server (MRS) sahiptir. MKL matematik işlemleri analitik algoritmaları ortak en iyi duruma getirir. MRO yüzde 100 CRAN R ile uyumlu olan ve içinde CRAN yayımlanan R kitaplıkları hiçbirini MRO yüklenebilir. MRS ölçekleme ve web hizmetlerine R modellerin operationalization sağlar. R programlarınızı Rstudio'dan VI, Emacs veya gedit gibi varsayılan düzenleyicileri birinde düzenleyebilirsiniz. Emacs Düzenleyicisi'ni kullanıyorsanız, Emacs basitleştirir (Emacs istatistikleri) konuşur, v paketini Not önceden yüklenmiş Emacs Düzenleyicisi'ni içinde R dosyalarıyla çalışma olmuştur.

Başlatma R konsolu, yalnızca yazın **R** Kabuğu'nda. Bu sizi, etkileşimli bir ortama götürür. R programınızı geliştirmek için genellikle Emacs veya VI veya gedit gibi bir düzenleyiciyi kullanın ve ardından R'ye içinde komut çalıştırın Rstudio'dan ile R programınızı geliştirmek için tam grafik IDE ortamına sahip.

Ayrıca bir R betiği yüklemeniz için olan [üst 20 R paketleri](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) istiyorsanız. (Belirtildiği gibi) yazarak girilebilir R etkileşimli arabiriminde olduktan sonra bu komut dosyasının çalıştırılması **R** Kabuğu'nda.  

### <a name="python"></a>Python
Python kullanarak geliştirme için Anaconda Python 2.7 ve 3.5 dağıtım yüklendi. Bu dağıtım yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra temel Python içerir. Varsayılan metin düzenleyicisi kullanabilirsiniz. Ayrıca, Spyder, Anaconda Python dağıtımları ile birlikte bir Python IDE kullanabilirsiniz. Spyder gereken bir grafik Masaüstü veya X11 iletme. Spyder kısayol grafik Desktop'ta sağlanır.

Biz, Python 2.7 ve 3.5 sahip olduğundan, özellikle geçerli oturumdaki çalışmak istediğiniz istediğiniz Python sürümü (conda ortamı) etkinleştirmeniz gerekir. Etkinleştirme işlemi, Python'un istenen sürümüyle yolu değişkenini ayarlar.

Python 2.7 conda ortamı etkinleştirmek için Kabuğu'ndan aşağıdaki komutu çalıştırın:

    source /anaconda/bin/activate root

Python 2.7 adresindeki yüklü */anaconda/bin*.

Python 3.5 conda ortamı etkinleştirmek için Kabuğu'ndan aşağıdaki komutu çalıştırın:

    source /anaconda/bin/activate py35


Python 3.5 yüklü adresindeki */anaconda/envs/py35/bin*.

Yalnızca bir Python etkileşimli oturum başlatmak için şunu yazın **python** Kabuğu'nda. Bir grafik arabiriminde olan veya yedekleme kümesi iletme X11 varsa, yazabilirsiniz **pycharm** PyCharm Python IDE başlatmak için.

Ek Python kitaplıkları yükleme için çalıştırmanız gerekir ```conda``` veya ````pip```` komut sudo altında ve Python Paket Yöneticisi (conda veya PIP) doğru Python ortamı yüklemek için tam yolunu girin. Örneğin:

    sudo /anaconda/bin/pip install <package> #for Python 2.7 environment
    sudo /anaconda/envs/py35/bin/pip install <package> # for Python 3.5 environment


### <a name="jupyter-notebook"></a>Jupyter not defteri
Anaconda dağıtım ayrıca bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortamı bulunur. Jupyter not defteri JupyterHub erişilir. Yerel Linux kullanıcı adı ve parola kullanarak oturum açın.

Jupyter not defteri sunucunun Python 2, Python 3 ve R tekrar önceden yapılandırıldı. "Jupyter not defteri sunucusuna erişmek için tarayıcı başlatmak için Not Defteri" adlı bir masaüstü simgesi yoktur. SSH veya X2Go istemcisi VM kullanıyorsanız, de ziyaret edebilirsiniz [https://localhost:8000 /](https://localhost:8000/) Jupyter not defteri sunucusuna erişmek için.

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin.
> 
> 

Herhangi bir ana bilgisayardan Jupyter not defteri sunucusuna erişebilir. Yalnızca yazın *https://\<VM DNS adı veya IP adresi\>: 8000 /*

> [!NOTE]
> VM sağlandığında bağlantı noktası 8000 Güvenlik Duvarı'nda varsayılan olarak açılır.
> 
> 

Biz örnek not defterlerini--bir söz Python ve r birinde paketlenmiş Yerel Linux kullanıcı adı ve parola kullanarak Jupyter not defteri için kimlik doğrulaması sonra not defteri giriş sayfasında örnekler bağlantısını görebilirsiniz. Seçerek yeni bir not defteri oluşturabilirsiniz **yeni**ve ardından uygun dil çekirdek. Görmüyorsanız, **yeni** düğmesini tıklatın, **Jupyter** not defteri sunucunun giriş sayfasına gitmek için sol üst simgesi.

### <a name="apache-spark-standalone"></a>Tek başına Apache Spark 
Apache Spark tek başına örneğini Spark uygulamalarında yerel olarak test etme ve büyük kümelerinde dağıtmadan önce ilk geliştirmenize yardımcı olması için bu Linux DSVM önceden yüklenir. PySpark programları Jupyter çekirdek çalıştırabilirsiniz. Ne zaman Jupyter açın ve tıklatın **yeni** düğmesi kullanılabilir tekrar listesini görmelisiniz. "Spark – Python" Spark Python dilini kullanarak uygulamalar oluşturmanıza olanak sağlayan PySpark Çekirdeği ' dir. Ayrıca, Spark programı oluşturmak için de bir Python IDE PyCharm veya Spyder gibi kullanabilirsiniz. Bu yana, bu tek başına bir örneğini, Spark yığını çağıran istemci programında çalışır. Bu daha hızlı ve Spark kümesinde geliştirme ile karşılaştırıldığında sorunlarını gidermek daha kolay hale getirir. 

Bir örnek PySpark not defteri Jupyter ($ giriş/not defterlerini/SparkML/pySpark) giriş dizininin altındaki "SparkML" dizininde bulabilirsiniz Jupyter üzerinde sağlanır. 

R için Spark programlama yapıyorsanız Microsoft R Server, SparkR veya sparklyr kullanabilirsiniz. 

Microsoft R Server Spark bağlamda çalıştırmadan önce kurulum adım yerel tek bir düğüm Hadoop HDFS ve Yarn örneğini etkinleştirmek için bir kez yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklü ancak DSVM üzerinde devre dışı. Bunu etkinleştirmek için aşağıdaki komutları kök olarak ilk kez çalıştırmanız gereken:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Hadoop durdurabilirsiniz, bunları çalıştırarak gerekmediğinde Hizmetleri ilgili ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` sağlanan ve kullanılabilir geliştirmek ve (DSVM tek başına Spark örneğinde olan) uzaktan Spark bağlamında MRS sınamak nasıl gösteren bir örnek `/dsvm/samples/MRS` dizin. 

### <a name="ides-and-editors"></a>IDE ve düzenleyiciler
Birkaç kod Düzenleyicileri'nin seçeneğiniz vardır. Bu, VI/VIM, Emacs, gEdit, PyCharm, Rstudio'dan, Eclipse ve Intellij içerir. gEdit, Eclipse, Intellij, Rstudio'dan ve PyCharm grafik düzenleyicilerden olan ve bunları kullanmak için bir grafik masaüstü oturum açmanız gerekir. Masaüstü ve uygulama bu düzenleyicilerin sahip menüsü kısayolları bunları başlatın.

**VIM** ve **Emacs** metin tabanlı düzenleyiciler şunlardır. Emacs üzerinde biz Emacs konuşur istatistikleri (, R ile çalışma Emacs Düzenleyicisi'ni kolaylaştırır v) adlı bir eklenti paketi yüklediniz. Daha fazla bilgi bulunabilir [v](http://ess.r-project.org/).

**Eclipse** açık kaynak, birden çok dili destekleyen Genişletilebilir IDE değil. Java geliştiricilerinin edition VM'de yüklü örneğidir. Eklenti ortamda genişletmek için yüklenebilen birkaç popüler diller için kullanılabilir. Ayrıca adlı Eclipse'te yüklü bir eklenti sahibiz **Eclipse için Azure Araç Seti**. Oluştur, geliştirmek, test ve Java gibi dilleri destekler Eclipse geliştirme ortamı kullanarak Azure uygulamalarını dağıtmak sağlar. Ayrıca bir **Java için Azure SDK** farklı Azure hizmetlerinden Java ortamında erişim sağlar. Eclipse için Azure araç hakkında daha fazla bilgi bulunabilir [Eclipse için Azure Araç Seti](../../azure-toolkit-for-eclipse.md).

**LaTex** Emacs eklenti birlikte texlive paket aracılığıyla yüklenen [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) LaTex belgelerinizi Emacs içinde yazma basitleştirir paket.  

### <a name="databases"></a>Veritabanları
#### <a name="postgres"></a>Postgres
Açık kaynak veritabanı **Postgres** VM üzerinde zaten tamamlanmış initdb ve çalışan hizmetler ile kullanılabilir. Hala veritabanları ve kullanıcılar oluşturmanız gerekir. Daha fazla bilgi için bkz: [Postgres belgelerine](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Grafik SQL istemcisi
**SQuirrel SQL**, (örneğin, Microsoft SQL Server, Postgres ve MySQL) farklı veritabanlarına bağlanmak için ve SQL sorguları çalıştırmak için bir grafik SQL istemci'nin sağlamış. Bu (örneğin X2Go istemci kullanarak) bir grafik Masaüstü oturumundan çalıştırabilirsiniz. SQuirrel SQL çağırmak için masaüstünde simgesinden başlatın veya Kabuğu aşağıdaki komutu çalıştırın.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

İlk kullanılmadan önce sürücüler ve veritabanı diğer adlar ayarlayın. JDBC sürücüleri şu adreste bulunabilir:

*/usr/Share/Java/jdbcdrivers*

Daha fazla bilgi için bkz: [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server erişmek için komut satırı araçları
SQL Server için ODBC sürücü paketi de iki komut satırı araçlarıyla birlikte gelir:

**BCP**: bcp yardımcı programı toplu bir kullanıcı tarafından belirtilen biçimde Microsoft SQL Server örneğini ve bir veri dosyası arasında veri kopyalar. Çok sayıda yeni satırı SQL Server tablolarına aktarmak ya da veri tabloları dışında veri dosyalarına veri aktarmak için bcp yardımcı programı kullanılabilir. Bir tabloya veri almak için bu tablo için oluşturulan bir biçim dosyası kullanmak, veya yapısını tablo ve sütunlarını için geçerli veri türlerini anlama.

Daha fazla bilgi için bkz: [bcp ile bağlanma](https://msdn.microsoft.com/library/hh568446.aspx).

**SQLCMD**: Transact-SQL deyimleri sqlcmd yardımcı programını yanı sıra ile sistem yordamları girin ve komut dosyaları komut isteminde. Bu yardımcı program ODBC Transact-SQL toplu işlemleri yürütmek için kullanır.

Daha fazla bilgi için bkz: [sqlcmd ile bağlanma](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Bu yardımcı programı, Linux ve Windows platformları arasında bazı farklar vardır. Ayrıntılar için belgelere bakın.
> 
> 

#### <a name="database-access-libraries"></a>Veritabanı erişimi kitaplıkları
Access veritabanları R ve Python kullanılabilir kitaplık yok.

* R içinde **RODBC** paket veya **dplyr** paket sorgulamak ya da veritabanı sunucusunda SQL deyimlerini yürütmek olanak tanır.
* Python içinde **pyodbc** kitaplığı, temel alınan katmanı olarak ODBC ile veritabanı erişimi sağlar.  

Erişim için **Postgres**:

* R: paketi kullanan **RPostgreSQL**.
* Python: Kullanma **psycopg2** kitaplığı.

### <a name="azure-tools"></a>Azure Araçları
Aşağıdaki Azure Araçları VM'de yüklü:

* **Azure komut satırı arabirimi**: Azure CLI oluşturup Kabuk komutları aracılığıyla Azure kaynaklarını yönetmek olanak tanır. Azure Araçları çağırmak için yalnızca yazın **azure Yardım**. Daha fazla bilgi için bkz: [Azure CLI belge sayfasının](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Storage Gezgini**: Microsoft Azure Storage Gezgini, Azure depolama hesabınızın depoladığınız nesnelerin göz atın ve karşıya yükleme ve Azure BLOB'ları gelen ve giden veri indirmek için kullanılan bir araçtır grafik. Depolama Gezgini Masaüstü kısayol simgesinden erişebilirsiniz. Yazarak, bir kabuk isteminde çağırabilirsiniz **StorageExplorer**. Yedekleme kümesi iletme X11 olması veya bir X2Go istemciden imzalanması gerekir.
* **Azure kitaplıkları**: önceden yüklenmiş kitaplıkları bazıları aşağıda verilmiştir.
  
  * **Python**: yüklü olan Python kitaplıkları Azure ile ilgili olan **azure**, **azureml**, **pydocumentdb**, ve **pyodbc**. İlk üç kitaplıklarıyla Azure depolama hizmetleri, Azure Machine Learning ve Azure Cosmos DB (Azure üzerinde bir NoSQL veritabanı) erişebilir. Dördüncü kitaplığı (yanı sıra Microsoft ODBC sürücüsü için SQL Server) pyodbc erişimi etkinleştirir SQL Server, Azure SQL Database ve Azure SQL Data Warehouse python'dan bir ODBC arabirimini kullanarak. Girin **PIP listesi** listelenen tüm kitaplıkları görmek için. Bu komutu hem Python 2.7 hem de 3.5 ortamlarında çalıştırdığınızdan emin olun.
  * **R**: yüklü olan Azure ile ilgili kitaplıklarında R olan **AzureML** ve **RODBC**.
  * **Java**: Azure Java kitaplıkları listesi dizininde bulunabilir **/dsvm/sdk/AzureSDKJava** VM üzerinde. Anahtar kitaplıkları SQL Server için Azure depolama ve Yönetimi API'leri, Azure Cosmos DB ve JDBC sürücüleri alır.  

Erişebileceğiniz [Azure portal](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısından. Azure Portal'da oluşturmak, yönetmek ve Azure kaynakları izle.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning oluşturun, dağıtın ve Tahmine dayalı analiz çözümlerini paylaşmak olanak tanıyan bir tam olarak yönetilen bir bulut hizmetidir. Azure Machine Learning Studio'dan denemeler ve modelleri oluşturun. Ziyaret ederek veri bilimi sanal makinede bir web tarayıcısından erişilebileceğini [Microsoft Azure Machine Learning](https://studio.azureml.net).

Azure Machine Learning Studio'da oturum açtıktan sonra bir deney tuvale makine öğrenimi algoritmaları için mantıksal bir akış burada yapı erişebilirsiniz. Ayrıca Azure Machine Learning üzerinde barındırılan bir Jupyter not defteri erişiminiz ve Machine Learning Studio'da denemeleri sorunsuz bir şekilde çalışabilirsiniz. Machine learning web hizmeti arabiriminde kaydırma tarafından oluşturulan modelleri faaliyete. Bu modeller öğrenme makineden tahminleri çağırmak herhangi bir dilde yazılan istemcileri sağlar. Daha fazla bilgi için bkz: [Machine Learning belge](https://azure.microsoft.com/documentation/services/machine-learning/).

Ayrıca VM Modellerinizi R veya Python derleme ve Azure Machine learning'de üretimde dağıtın. Biz kitaplıkları R yüklü (**AzureML**) ve Python (**azureml**) bu işlevselliği etkinleştirmek için.

Azure Machine Learning R ve Python modellerinde dağıtma hakkında daha fazla bilgi için bkz: [veri bilimi sanal makine yapabilir on nokta](vm-do-ten-things.md) (özellikle, bölüm "R veya Python kullanarak modelleri oluşturmak ve bunları faaliyete Azure Machine Learning kullanarak").

> [!NOTE]
> Bu yönergeler, veri bilimi VM Windows sürümü için yazılmıştır. Ancak Azure Machine Learning modellerini dağıtmayı yoktur Linux VM'ye ilgili bilgiler sağlanmıştır.
> 
> 

### <a name="machine-learning-tools"></a>Machine learning araçları
VM araçları ve önceden derlenmiş ve yerel olarak yüklenmiş algoritmaları öğrenme birkaç makineyle birlikte gelir. Bunlar:

* **Microsoft Bilişsel Araç Seti** : kapsamlı bir araç seti öğrenme.
* **Vowpal Wabbit**: hızlı çevrimiçi öğrenme algoritması.
* **xgboost**: en iyi duruma getirilmiş, boosted ağaç algoritmaları sağlayan bir araç.
* **Python**: Anaconda Python ile birlikte gelen machine learning algoritmaları Scikit öğrenin gibi kitaplıklarla birlikte gelir. Diğer kitaplıklarını kullanarak yükleyebileceğiniz `pip install` komutu.
* **R**: machine learning işlevleri içeren zengin bir kitaplık r için kullanılabilir Önceden yüklenen kitaplıklar bazıları lm, glm, randomForest, rpart. Diğer kitaplıkları çalıştırarak yüklenebilir:
  
        install.packages(<lib name>)

Burada, listedeki ilk üç machine learning Araçlar ile ilgili bazı ek bilgiler verilmiştir.

#### <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti
Bir açık kaynaklı, araç seti öğrenme derin budur. Bir komut satırı aracı (cntk) ve yolunda zaten.

Temel bir örneği çalıştırmak için Kabuğu'nda aşağıdaki komutları çalıştırın:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Daha fazla bilgi için bkz: CNTK bölümünü [GitHub](https://github.com/Microsoft/CNTK)ve [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit olan machine learning çevrimiçi, karma, allreduce, düşürülmesi, learning2search, etkin, gibi teknikler kullanır sistem ve etkileşimli öğrenme.

Temel bir örneği temel aracı çalıştırmak için aşağıdakileri yapın:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Bu dizinde diğer, daha büyük gösterileri vardır. VW hakkında daha fazla bilgi için bkz: [GitHub'un bu bölümünde](https://github.com/JohnLangford/vowpal_wabbit)ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Bu, tasarlanmış ve boosted (ağacı) algoritmaları için en iyi hale getirilmiş bir kitaplıktır. Bu kitaplık amacı, artırmanın büyük ölçekli ağaç sağlamak için gereken uç makineler hesaplama sınırları göndermek için ölçeklenebilir, taşınabilir ve doğru olmasıdır.

Bir R kitaplığı yanı sıra bir komut satırı sağlanır.

Bu kitaplıkta R kullanmak için etkileşimli bir R oturum başlatabilirsiniz (yazarak yalnızca **R** Kabuğu'nda) ve kitaplığı yüklenemiyor.

R isteminde çalıştırarak basit bir örnek aşağıda verilmiştir:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Xgboost komut satırını çalıştırmak için Kabuğu'nda yürütmek için komutlar şunlardır:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


.Model dosyası belirtilen dizin yazılır. Bu demo örnek hakkında bilgi bulunabilir [github'da](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Xgboost hakkında daha fazla bilgi için bkz: [xgboost belge sayfasının](https://xgboost.readthedocs.org/en/latest/)ve kendi [GitHub deposunu](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Çıngırağı
Rattle ( **R** **A**nalytical **T**aracı **T**o **L**kazanmak **E**asily) GUI tabanlı veri keşfi ve modelleme kullanır. Veri, taşımalarına modellenebilir, verileri Denetimsiz hem de denetimli modellerinden oluşturur, modelleri performansını grafik gösterir dönüşümler veri istatistiksel ve görsel özetini sunar ve puanları yeni verilerini ayarlar. Ayrıca, doğrudan R çalıştırmak veya daha fazla çözümleme için bir başlangıç noktası olarak kullanılan işlemler kullanıcı arabiriminde çoğaltma R kodu oluşturur.

Çıngırağı çalıştırmak için bir grafik Masaüstü Oturum açma oturumunda olmanız gerekir. Terminal üzerinde yazın ```R``` R ortam girmek için. R isteminde aşağıdaki komutları girin:

    library(rattle)
    rattle()

Sekmeleri bir dizi artık bir grafik arabirim açılır. Bir örnek hava veri kümesi kullanın ve bir model oluşturmak için gereken Çıngırağı içinde hızlı başlangıç adımlar şunlardır. Bazı adımları otomatik olarak yüklemek ve sistem üzerinde olmayan bazı gerekli R paketlerini yükleme istenir.

> [!NOTE]
> Sistem dizininde (varsayılan) paketini yüklemek için erişimi yoksa, kişisel kitaplığınıza paketleri yüklemek için R konsol penceresinde bir ileti görebilirsiniz. Yanıt *y* bu komut istemlerini görürseniz.
> 
> 

1. **Yürüt**’e tıklayın.
2. Örnek hava veri kümesi kullanmayı tercih soran bir iletişim kutusu açılır. Tıklatın **Evet** örnek yüklenemiyor.
3. Tıklatın **modeli** sekmesi.
4. Tıklatın **yürütme** karar ağacı oluşturmak için.
5. Tıklatın **çizin** karar ağacı görüntülemek için.
6. ' I tıklatın **orman** tıklayın ve radyo düğmesinin **yürütme** rastgele bir orman oluşturmak için.
7. Tıklatın **değerlendir** sekmesi.
8. ' I tıklatın **Risk** radyo düğmesinin öğesini tıklatıp **yürütme** iki Risk (kümülatif) performans çizimleri görüntülemek için.
9. Tıklatın **günlük** önceki işlemleri Oluştur R kodunu göstermek için sekmesi.
   (Eklemek için gerek Çıngırağı geçerli sürümünde bir hata nedeniyle, bir  *#*  önüne karakter *... Bu günlüğünü Dışarı Aktar*  günlük metninde.)
10. Tıklatın **verme** adlı R betiği kaydetmek için düğmesini *weather_script. R* giriş klasörü için.

Çıngırağı ve r çıkabilirsiniz Şimdi oluşturulan R betiği değiştirmek veya çalıştırmak için her zaman içinde Rattle UI yapıldığı her şeyi yinelemek için olduğu gibi kullanın. Özellikle yeni başlayanlar için R içinde bu hızlı bir şekilde analiz yapın ve basit bir grafik arabirim öğrenmede R değiştirmek ve/veya öğrenmek için otomatik kod oluşturma sırasında makine için kolay bir yoludur.

## <a name="next-steps"></a>Sonraki adımlar
İşte öğrenme ve araştırması nasıl devam edebilirsiniz:

* [Veri bilimi üzerinde Linux veri bilimi sanal makine](linux-dsvm-walkthrough.md) izlenecek Linux veri bilimi burada sağlanan VM ile birçok ortak veri bilimi görevleri gerçekleştirmek nasıl gösterir. 
* Çeşitli veri bilimi araçları, bu makalede açıklanan araçları deneyerek veri bilimi VM keşfedin. De çalıştırabilirsiniz *dsvm daha fazla bilgi* temel bir giriş ve işaretçiler VM'de yüklü araçları hakkında daha fazla bilgi için sanal makinedeki Kabuk.  
* Kullanarak uçtan uca analitik çözümler sistematik olarak oluşturmayı öğrenin [takım veri bilimi işlemi](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Ziyaret [Cortana Analytics Galerisi](http://gallery.cortanaanalytics.com) Cortana Analytics Suite kullanan machine learning ve veri analizi örnekleri için.

