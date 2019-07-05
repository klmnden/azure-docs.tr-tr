---
title: Azure HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme
description: Dağıtma, izleme ve Storm panosunu kullanarak Linux tabanlı HDInsight üzerinde Apache Storm topolojilerini yönetme hakkında bilgi edinin. Visual Studio için Hadoop araçlarını kullanın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.openlocfilehash: ac1a4c77589f4ef88c9ee862cb871b376ca8a0fe
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483846"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-azure-hdinsight"></a>Azure HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme 

Bu belgede, yönetmeye ve izlemeye ilişkin temel bilgileri alın [Apache Storm](https://storm.apache.org/) üzerinde Storm, HDInsight kümelerinde çalışan topolojileri.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde Apache Storm kümesi. Bkz: [Apache Hadoop kümeleri oluşturma Azure portalını kullanarak](../hdinsight-hadoop-create-linux-clusters-portal.md) seçip **Storm** için **küme türü**.


* (İsteğe bağlı) SSH ve SCP bilgisi: Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

* (İsteğe bağlı) Visual Studio: Azure SDK'sı 2.5.1 veya daha yeni ve Visual Studio için Data Lake araçları. Daha fazla bilgi için [Visual Studio için Data Lake araçları ile çalışmaya başlamak](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    Visual Studio sürümlerinden biri:

  * Visual Studio 2012 güncelleştirme 4

  * Güncelleştirme 4 ile Visual Studio 2013 veya [Visual Studio 2013 Community](https://go.microsoft.com/fwlink/?LinkId=517284)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * Visual Studio 2015 (herhangi bir sürümü)

  * Visual Studio 2017 (herhangi bir sürümü). Visual Studio 2017 için Data Lake araçları Azure iş yükünün parçası yüklenir.

## <a name="submit-a-topology-visual-studio"></a>Bir topoloji gönder: Visual Studio

HDInsight araçları C# veya karma topolojiler, Storm kümesine göndermek için kullanılabilir. Örnek uygulama aşağıdaki adımları kullanın. HDInsight Araçları'nı kullanarak oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio için HDInsight Araçları'nı kullanarak C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

1. Visual Studio için Data Lake Araçları'nın en son sürümü yüklemediyseniz, bkz. [Visual Studio için Data Lake araçları ile çalışmaya başlamak](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]  
    > Visual Studio için Data Lake araçları, Visual Studio için HDInsight Araçları'nı önceden çağrılan.
    >
    > Visual Studio için Data Lake araçları dahil edilecek __Azure iş yükü__ Visual Studio 2017 için.

2. Visual Studio'yu açın, **dosya** > **yeni** > **proje**.

3. İçinde **yeni proje** iletişim kutusunda **yüklü** > **şablonları**ve ardından **HDInsight**. Şablonlar listesinden **Storm örnek**. İletişim kutusunun en altında uygulama için bir ad yazın.

    ![image](./media/apache-storm-deploy-monitor-topology-linux/sample.png)

4. İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **HDInsight üzerindeki storm'a Gönder**.

   > [!NOTE]  
   > İstenirse, Azure aboneliğiniz için oturum açma kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, HDInsight kümesi üzerinde Storm'a içeren oturum açın.

5. HDInsight kümesi üzerinde Storm'a seçin **Storm kümesi** aşağı açılan liste ve ardından **Gönder**. Gönderim kullanarak başarılı olup olmadığını izleyebilirsiniz **çıkış** penceresi.

## <a name="submit-a-topology-ssh-and-the-storm-command"></a>Bir topoloji gönder: SSH ve Storm komutu

1. HDInsight kümesine bağlanmak için SSH kullanın. Değiştirin **kullanıcıadı** , SSH oturum açma adı. Değiştirin **CLUSTERNAME** ile HDInsight kümenizin adıdır:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    HDInsight kümenize bağlanmak için SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Örnek bir topoloji başlatmak için aşağıdaki komutu kullanın:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Bu komut, kümede örnek WordCount topolojisini başlatır. Bu topoloji, rastgele tümceler oluşturur ve ardından geçtiği her bir sözcüğün kaç kez geçtiğini sayar.

   > [!NOTE]  
   > Kümeye topoloji gönderirken `storm` komutunu kullanmadan önce kümeyi içeren jar dosyasını kopyalamanız gerekir. Dosya kümeye kopyalamak için kullanabileceğiniz `scp` komutu. Örneğin, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   >
   > WordCount örneği ve diğer storm starter örnekleri `/usr/hdp/current/storm-client/contrib/storm-starter/` konumunda kümenize zaten dahil edilmiştir.

## <a name="submit-a-topology-programmatically"></a>Bir topoloji gönder: program aracılığıyla

Nimbus hizmetini kullanarak bir topoloji program aracılığıyla dağıtabilirsiniz. [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) bir örnek gösterilmiştir dağıtıp Nimbus hizmeti aracılığıyla bir topoloji başlatmak Java uygulaması sağlar.

## <a name="monitor-and-manage-visual-studio"></a>İzleme ve yönetme: Visual Studio

Visual Studio kullanarak bir topoloji gönderildiğinde **Storm topolojilerini** görünümü görüntülenir. Topoloji, çalışan topolojiyi hakkındaki bilgileri görüntülemek için listeden seçin.

![Visual studio İzleyicisi](./media/apache-storm-deploy-monitor-topology-linux/vsmonitor.png)

> [!NOTE]  
> Ayrıca görüntüleyebilirsiniz **Storm topolojilerini** gelen **Sunucu Gezgini** genişleterek **Azure** > **HDInsight**ve ardından HDInsight kümesinde Storm sağ tıklayıp seçerek **Storm topolojilerini görüntüle**.

Spout veya bu bileşenler hakkındaki bilgileri görüntülemek için Cıvatalar şekli seçin. Her seçili öğe için yeni bir pencere açılır.

### <a name="deactivate-and-reactivate"></a>Devre dışı bırakıp yeniden etkinleştirin

Sonlandırılan veya yeniden etkinleştiren kadar bir topolojiyi devre dışı bırakma, duraklatır. Bu işlemleri gerçekleştirmek için __devre dışı bırak__ ve __yeniden__ en üstündeki düğmeleri __topoloji özeti__.

### <a name="rebalance"></a>Yeniden Dengeleme

Yeniden Dengeleme bir topoloji, topolojinin paralelliğini gözden geçirmek sistem sağlar. Daha fazla Not eklemek için küme yeniden boyutlandırılmış, örneğin, yeniden Dengeleme yeni düğümleri görmek bir imkan tanır.

Bir topolojiyi yeniden dengelemek için kullanın __yeniden dengelemek__ üst kısmındaki düğmeye __topoloji özeti__.

> [!WARNING]  
> Bir topoloji ilk yeniden Dengeleme topoloji, devre dışı bırakır çalışanları küme arasında eşit olarak yeniden dağıtır, sonra son topoloji yeniden Dengeleme oluşmadan önce edildi durumuna döndürür. Topoloji etkindi, bu nedenle onu tekrar etkin hale gelir. Devre dışı bırakıldı, devre dışı kalır.

### <a name="kill-a-topology"></a>Bir topolojiyi sonlandırmak

Storm topolojilerini, durdurulmuş olsa veya küme silinir kadar çalıştırmaya devam edin. Bir topoloji durdurmak için kullanın __KILL__ üst kısmındaki düğmeye __topoloji özeti__.

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a>İzleme ve yönetme: SSH ve Storm komutu

`storm` Yardımcı programını, komut satırından çalışan topolojilerle çalışmaya olanak sağlar. Kullanım `storm -h` komutların tam listesi için.

### <a name="list-topologies"></a>Liste topolojileri

Çalışan topolojilerini tüm listelemek için aşağıdaki komutu kullanın:

    storm list

Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Devre dışı bırakıp yeniden etkinleştirin

Sonlandırılan veya yeniden etkinleştiren kadar bir topolojiyi devre dışı bırakma, duraklatır. Devre dışı bırakın ve yeniden etkinleştirmek için aşağıdaki komutu kullanın:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Çalışan bir topoloji Sonlandır

Storm topolojileri, başlatıldığında, devam durdurulana kadar çalışıyor. Bir topoloji durdurmak için aşağıdaki komutu kullanın:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Yeniden Dengeleme

Yeniden Dengeleme bir topoloji, topolojinin paralelliğini gözden geçirmek sistem sağlar. Daha fazla Not eklemek için küme yeniden boyutlandırılmış, örneğin, yeniden Dengeleme yeni düğümleri görmek bir imkan tanır.

> [!WARNING]  
> Bir topoloji ilk yeniden Dengeleme topoloji, devre dışı bırakır çalışanları küme arasında eşit olarak yeniden dağıtır, sonra son topoloji yeniden Dengeleme oluşmadan önce edildi durumuna döndürür. Topoloji etkindi, bu nedenle onu tekrar etkin hale gelir. Devre dışı bırakıldı, devre dışı kalır.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>İzleme ve yönetme: Storm kullanıcı Arabirimi

Storm Kullanıcı Arabirimi çalışan topolojilerle çalışmaya yönelik bir web arabirimi sağlar ve HDInsight kümenize dahil edilir. Storm kullanıcı arabirimini görüntülemek için açmak için bir web tarayıcısı kullanın **https://CLUSTERNAME.azurehdinsight.net/stormui** burada **CLUSTERNAME** kümenizin adıdır.

> [!NOTE]  
> Bir kullanıcı adı ve parola girmeniz istenirse kümeyi oluştururken kullandığınız küme yöneticisi (yönetici) ve parolayı girin.

### <a name="main-page"></a>Ana sayfa

Storm kullanıcı arabirimini'nın ana sayfasında aşağıdaki bilgileri sağlar:

* **Küme özeti**: Storm kümesi ile ilgili temel bilgileri.
* **Topoloji özeti**: Topolojileri çalışan bir listesi. Bağlantıları bu bölümde belirli topolojiler hakkında daha fazla bilgi görüntülemek için kullanın.
* **Summary İdarecisi**: Storm gözetmen hakkında bilgi sağlar.
* **Nimbus yapılandırma**: Kümenin yapılandırmasını nimbus.

### <a name="topology-summary"></a>Topoloji özeti

Bir bağlantıdan seçerek **topoloji özeti** bölüm topoloji hakkında aşağıdaki bilgileri görüntüler:

* **Topoloji özeti**: Topoloji hakkında temel bilgiler.
* **Topoloji eylemleri**: Yönetim eylemleri için topoloji gerçekleştirebilirsiniz.

  * **Etkinleştirme**: Devre dışı bırakılan bir topolojiyi işlemeyi sürdürür.
  * **Devre dışı bırakma**: Çalışan topolojiyi duraklatır.
  * **Yeniden Dengeleme**: Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Bu işlem artan veya azalan kümedeki düğümlerin sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır.

    Daha fazla bilgi için <a href="https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Apache Storm topolojisinin paralelliğini anlama</a>.
  * **KILL**: Belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.
* **Topoloji istatistikleri**: Topoloji hakkındaki istatistiklerdir. Sayfada diğer girdiler için zaman çerçevesini belirlemek için bağlantıları kullanın. **penceresi** sütun.
* **Spout'lar**: Topoloji tarafından kullanılan spout. Bağlantıları bu bölümde belirli spout hakkında daha fazla bilgi görüntülemek için kullanın.
* **Cıvatalar**: Boltlar topolojiye göre kullanılır. Bağlantıları bu bölümde belirli Cıvatalar hakkında daha fazla bilgi görüntülemek için kullanın.
* **Topoloji Yapılandırması**: Seçili topoloji yapılandırması.

### <a name="spout-and-bolt-summary"></a>Spout ve Bolt özeti

Gelen bir spout seçerek **Spout'lar** veya **Cıvatalar** bölümleri seçili öğe hakkında aşağıdaki bilgileri görüntüler:

* **Bileşen özeti**: Spout veya Cıvata hakkındaki temel bilgileri.
* **Spout/Cıvata**: Spout veya Cıvata hakkındaki istatistiklerdir. Sayfada diğer girdiler için zaman çerçevesini belirlemek için bağlantıları kullanın. **penceresi** sütun.
* **Girdi istatistikleri** (yalnızca Cıvata): Cıvata tarafından kullanılan giriş akışları hakkında bilgi sağlar.
* **Çıktı istatistikleri**: Spout veya Cıvata tarafından yayılan akışları hakkında bilgi sağlar.
* **Yürütücü**: Spout veya Cıvata örnekleri hakkında bilgi. Seçin **bağlantı noktası** girişi tanılama bilgileri günlüğünü görüntülemek belirli bir yürütücü için bu örneği için üretilen.
* **Hataları**: Spout veya Cıvata için hata bilgileri.

## <a name="monitor-and-manage-rest-api"></a>İzleme ve yönetme: REST API

Storm kullanıcı arabirimini, benzer yönetim ve işlevsellik REST API kullanarak izleme gerçekleştirebilmesi için REST API temelinde oluşturulmuştur. REST API, yönetmeye ve izlemeye Storm Topolojileri için özel araçlar oluşturmak için kullanabilirsiniz.

Daha fazla bilgi için [Apache Storm kullanıcı Arabirimi REST API](https://storm.apache.org/releases/current/STORM-UI-REST-API.html). Aşağıdaki bilgileri, HDInsight üzerinde Apache Storm ile REST API kullanarak özeldir.

> [!IMPORTANT]  
> Storm REST API'si, internet üzerinden genel kullanıma açık değil ve HDInsight küme baş düğümüne SSH tüneli kullanılarak erişilmelidir. Bir SSH tüneli oluşturma ve kullanma hakkında daha fazla bilgi için bkz: [kullanım Apache Ambari web kullanıcı Arabirimi, ResourceManager, JobHistory, NameNode, Apache Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel](../hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Taban URI

Baş düğümde üzerinde Linux tabanlı HDInsight kümelerinde REST API için ana URI kullanılabilir **https:\//HEADNODEFQDN:8744/api/v1/** . Baş düğümün etki alanı adı, küme oluşturma sırasında oluşturulan ve statik değildir.

Küme baş düğümü için tam etki alanı adı (FQDN) birkaç farklı yolla bulabilirsiniz:

* **Bir SSH oturumundan**: Komutunu `headnode -f` küme için bir SSH oturumundan.
* **Ambari Web**: Seçin **Hizmetleri** sayfanın üst kısmından seçip **Storm**. Gelen **özeti** sekmesinde **Storm kullanıcı arabirimini sunucu**. REST API ve Storm kullanıcı arabirimini barındıran düğümü FQDN'si, sayfanın en üstünde görüntülenir.
* **Ambari REST API'sinden**: Komutunu `curl -u admin -G "https:\//CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` REST API ve Storm kullanıcı arabirimini çalıştığı düğüm hakkında bilgi almak için. Değiştirin **CLUSTERNAME** küme adı ile. İstendiğinde oturum açma (Yönetici) hesabı için parolayı girin. Yanıtta düğümü FQDN'si "host_name" giriş içerir.

### <a name="authentication"></a>Kimlik Doğrulaması

REST API istekleri kullanmalıdır **temel kimlik doğrulaması**, HDInsight küme yöneticisinin adı ve parola kullanın.

> [!NOTE]  
> Temel kimlik doğrulaması düz metin kullanarak gönderildiğinden, aşağıdakileri yapmalısınız **her zaman** küme ile güvenli iletişim için HTTPS kullanın.

### <a name="return-values"></a>Döndürülen değerler

REST API öğesinden geri döndürülen bilgiler, yalnızca küme içinde kullanılabilir olabilir. İçin döndürülen gibi tam etki alanı adı (FQDN) [Apache ZooKeeper](https://zookeeper.apache.org/) sunucularının Internet'ten erişilebilir değil.

## <a name="next-steps"></a>Sonraki Adımlar

Bilgi edinmek için nasıl [Apache Maven kullanarak geliştirme Java tabanlı topolojiler](apache-storm-develop-java-topology.md).

Daha fazla örnek topolojileri listesi için bkz. [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md).
