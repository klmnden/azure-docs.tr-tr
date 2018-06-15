---
title: Dağıtma ve Linux tabanlı Hdınsight üzerinde Apache Storm topolojilerini yönetme | Microsoft Docs
description: Dağıtma, izleme ve Linux tabanlı Hdınsight üzerinde Storm panosunu kullanarak Apache Storm topolojilerini yönetme öğrenin. Visual Studio Hadoop araçlarını kullanın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: larryfr
ms.openlocfilehash: 9dd63e1f3ec381dd99495ebc6193198611c76c88
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31417046"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a>Dağıtma ve Hdınsight üzerinde Apache Storm topolojilerini yönetme

Bu belgede, yönetme ve izleme üzerinde Storm Hdınsight kümelerinde çalışan Storm topolojilerini temellerini öğrenin.

> [!IMPORTANT]
> Bu makaledeki adımlarda Hdınsight kümesinde Linux tabanlı Storm gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement). 
>
> Dağıtma ve Windows tabanlı Hdınsight üzerinde topolojileri izleme hakkında daha fazla bilgi için bkz: [dağıtma ve Windows tabanlı Hdınsight üzerinde Apache Storm topolojilerini yönetme](apache-storm-deploy-monitor-topology.md)


## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight kümesinde Linux tabanlı Storm**: bkz [Hdınsight üzerinde Apache Storm ile çalışmaya başlama](apache-storm-tutorial-get-started-linux.md) küme oluşturma adımları

* (İsteğe bağlı) **SSH ve SCP**: daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

* (İsteğe bağlı) **Visual Studio**: Azure SDK'sı 2.5.1 ya da daha yeni ve Visual Studio için Data Lake araçları. Daha fazla bilgi için bkz: [Visual Studio için Data Lake araçları kullanarak çalışmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    Visual Studio aşağıdaki sürümlerinden biri:

  * Visual Studio 2012 ile [güncelleştirme 4](http://www.microsoft.com/download/details.aspx?id=39305)

  * Visual Studio 2013 [güncelleştirme 4](http://www.microsoft.com/download/details.aspx?id=44921) veya [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * Visual Studio 2015 (herhangi bir sürümünü)

  * Visual Studio 2017 (herhangi bir sürümünü). Visual Studio 2017 için Data Lake araçları, Azure iş yükü bir parçası olarak yüklenir.

## <a name="submit-a-topology-visual-studio"></a>Bir topoloji gönderin: Visual Studio

Hdınsight araçları C# veya karma topolojiler Storm kümenize göndermek için kullanılabilir. Örnek bir uygulamayı aşağıdaki adımları kullanın. Hdınsight araçları kullanarak oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio için Hdınsight araçları kullanarak C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

1. Visual Studio için Data Lake Araçları'nın en son sürümünü yüklemediyseniz, bkz: [Visual Studio için Data Lake araçları kullanarak çalışmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]
    > Visual Studio için Data Lake araçları Visual Studio için Hdınsight araçları adıysa.
    >
    > Visual Studio için Data Lake araçları dahil edilmiştir __Azure iş yükü__ Visual Studio 2017 için.

2. Visual Studio'ni açın, **dosya** > **yeni** > **proje**.

3. İçinde **yeni proje** iletişim kutusunda, genişletin **yüklü** > **şablonları**ve ardından **Hdınsight**. Şablonları listesinden seçin **Storm örnek**. İletişim kutusunun altında uygulama için bir ad yazın.

    ![görüntü](./media/apache-storm-deploy-monitor-topology-linux/sample.png)

4. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Hdınsight üzerinde Storm Gönder**.

   > [!NOTE]
   > İstenirse, Azure aboneliğiniz için oturum açma kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, Hdınsight kümesi üzerinde Storm'a içeren oturum açın.

5. Hdınsight küme üzerinde Storm'a seçin **Storm kümesi** aşağı açılan listeyi ve ardından **gönderme**. Gönderim kullanarak başarılı olup olmadığını izleyebilirsiniz **çıkış** penceresi.

## <a name="submit-a-topology-ssh-and-the-storm-command"></a>Bir topoloji gönderin: SSH ve Storm komutu

1. SSH Hdınsight kümesine bağlanın. Değiştir **kullanıcıadı** SSH oturum açma adı. Değiştir **CLUSTERNAME** Hdınsight küme adıyla:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Hdınsight kümenize bağlanmak için SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Örnek bir topoloji başlatmak için aşağıdaki komutu kullanın:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Bu komut, kümede örnek WordCount topolojisini başlatır. Bu topoloji rastgele cümleleri oluşturur ve cümleleri her sözcüğün geçtiği sayar.

   > [!NOTE]
   > Kümeye topoloji gönderirken `storm` komutunu kullanmadan önce kümeyi içeren jar dosyasını kopyalamanız gerekir. Dosyayı kümeye kopyalamak için kullanabileceğiniz `scp` komutu. Örneğin, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   >
   > WordCount örneği ve diğer storm starter örnekleri `/usr/hdp/current/storm-client/contrib/storm-starter/` konumunda kümenize zaten dahil edilmiştir.

## <a name="submit-a-topology-programmatically"></a>Bir topoloji gönderin: program aracılığıyla

Nimbus hizmetini kullanarak bir topoloji programlı olarak dağıtabilirsiniz. [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) bir örnek nasıl dağıtıp Nimbus hizmeti aracılığıyla bir topoloji başlatmak gösteren bir Java uygulama sağlar.

## <a name="monitor-and-manage-visual-studio"></a>İzleme ve yönetme: Visual Studio

Visual Studio kullanarak bir topoloji gönderildiğinde **Storm topolojilerini** görünümü görüntülenir. Topoloji çalışan topolojisi ile ilgili bilgileri görüntülemek için listeden seçin.

![Visual studio İzleyicisi](./media/apache-storm-deploy-monitor-topology-linux/vsmonitor.png)

> [!NOTE]
> De görüntüleyebilirsiniz **Storm topolojilerini** gelen **Sunucu Gezgini** genişleterek **Azure** > **Hdınsight**ve Hdınsight kümesinde bir Storm sağ tıklayıp seçerek **görünüm Storm topolojilerini**.

Şekil spout'lar veya Cıvatalar bu bileşenler hakkındaki bilgileri görüntülemek için seçin. Yeni bir pencere için seçilen her öğenin açar.

### <a name="deactivate-and-reactivate"></a>Devre dışı bırakın ve yeniden etkinleştirin

Sonlandırıldı veya yeniden kadar bir topoloji devre dışı bırakma, duraklatır. Bu işlemleri gerçekleştirmek için kullanın __etkinliğini__ ve __yeniden__ üst tarafındaki düğmeleri __topoloji özeti__.

### <a name="rebalance"></a>Yeniden Dengele

Bir topoloji dengelenmesi topolojisinin paralellik gözden geçirmek sistem izin verir. Daha fazla Not eklemek için küme yeniden boyutlandırılabilir, örneğin, yeniden dengelenmesi yeni düğümleri görmek için topoloji imkan tanır.

Bir topoloji amacıyla kullanmak __yeniden dengelemeniz__ en üstündeki düğmesi __topoloji özeti__.

> [!WARNING]
> Bir topoloji ilk dengelenmesi topoloji, devre dışı bırakır çalışanları eşit bir kümede yeniden dağıtır, sonra son olarak yeniden dengelenmesi oluşmadan önce durumla durum topoloji döndürür. Topoloji etkin olduğunda, bu nedenle onu yeniden etkin hale gelir. Devre dışı bırakıldı, devre dışı kalır.

### <a name="kill-a-topology"></a>Bir topoloji Sonlandır

Storm topolojilerini durdurulmuş veya küme silinene kadar çalıştırmaya devam edin. Bir topoloji durdurmak için kullanma __KILL__ en üstündeki düğmesi __topoloji özeti__.

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a>İzleme ve yönetme: SSH ve Storm komutu

`storm` Yardımcı program komut satırından çalışan topolojilerle çalışmanıza olanak sağlar. Kullanım `storm -h` komutları tam listesi için.

### <a name="list-topologies"></a>Liste topolojileri

Çalışan topolojilerle tüm listelemek için aşağıdaki komutu kullanın:

    storm list

Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Devre dışı bırakın ve yeniden etkinleştirin

Sonlandırıldı veya yeniden kadar bir topoloji devre dışı bırakma, duraklatır. Devre dışı bırakın ve yeniden etkinleştirmek için aşağıdaki komutu kullanın:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Çalışan bir topoloji Sonlandır

Storm topolojileri, başlatıldığında, devam durdurulana kadar çalışıyor. Bir topoloji durdurmak için aşağıdaki komutu kullanın:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Yeniden Dengele

Bir topoloji dengelenmesi topolojisinin paralellik gözden geçirmek sistem izin verir. Daha fazla Not eklemek için küme yeniden boyutlandırılabilir, örneğin, yeniden dengelenmesi yeni düğümleri görmek için topoloji imkan tanır.

> [!WARNING]
> Bir topoloji ilk dengelenmesi topoloji, devre dışı bırakır çalışanları eşit bir kümede yeniden dağıtır, sonra son olarak yeniden dengelenmesi oluşmadan önce durumla durum topoloji döndürür. Topoloji etkin olduğunda, bu nedenle onu yeniden etkin hale gelir. Devre dışı bırakıldı, devre dışı kalır.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>İzleme ve yönetme: Storm kullanıcı Arabirimi

Storm Kullanıcı Arabirimi çalışan topolojilerle çalışmaya yönelik bir web arabirimi sağlar ve HDInsight kümenize dahil edilir. Storm kullanıcı arabirimini görüntülemek için açmak üzere bir web tarayıcısı kullanın **https://CLUSTERNAME.azurehdinsight.net/stormui**, burada **CLUSTERNAME** kümenizin adıdır.

> [!NOTE]
> Bir kullanıcı adı ve parola girmeniz istenirse kümeyi oluştururken kullandığınız küme yöneticisi (yönetici) ve parolayı girin.

### <a name="main-page"></a>Ana sayfa

Storm kullanıcı Arabirimi ana sayfasında aşağıdaki bilgileri sağlar:

* **Küme Özet**: Storm kümesi hakkında temel bilgiler.
* **Topoloji özeti**: topolojileri çalışan bir listesi. Belirli topolojileri hakkında daha fazla bilgi görüntülemek için bu bölümdeki bağlantıları kullanın.
* **Yönetici Özeti**: Storm İdarecisi hakkında bilgi.
* **Nimbus yapılandırma**: kümenin Nimbus yapılandırması.

### <a name="topology-summary"></a>Topoloji özeti

Bir bağlantıdan seçme **topoloji özeti** bölüm topolojisi hakkında aşağıdaki bilgileri görüntüler:

* **Topoloji özeti**: topolojisi hakkında temel bilgiler.
* **Topoloji eylemleri**: topolojisini gerçekleştirdiğiniz yönetim işlemleri.

  * **Etkinleştirme**: devre dışı bırakılan bir topolojiyi işlemeyi sürdürür.
  * **Devre dışı**: çalışan topolojiyi duraklatır.
  * **Yeniden dengelemeniz**: topolojisinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Bu işlem düğümleri artan veya azalan sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır.

    Daha fazla bilgi için bkz. <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Bir Storm topolojisinin paralelliğini anlama</a>.
  * **KILL**: belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.
* **Topoloji istatistikleri**: topoloji hakkındaki istatistiklerdir. Sayfada diğer girdiler için zaman dilimini ayarlamak için bağlantıları kullanın **penceresi** sütun.
* **Spout'lar**: topolojisi tarafından kullanılan spout'lar. Belirli spout'lar hakkında daha fazla bilgi görüntülemek için bu bölümdeki bağlantıları kullanın.
* **Cıvatalar**: topolojisi tarafından kullanılan Cıvatalar. Belirli Cıvatalar hakkında daha fazla bilgi görüntülemek için bu bölümdeki bağlantıları kullanın.
* **Topoloji Yapılandırması**: Seçili topoloji yapılandırması.

### <a name="spout-and-bolt-summary"></a>Spout ve Cıvata özeti

Spout gelen seçme **Spout'lar** veya **Cıvatalar** bölümleri seçili öğe hakkında aşağıdaki bilgileri görüntüler:

* **Bileşen özeti**: spout veya Cıvata hakkında temel bilgiler.
* **Spout/Cıvata istatistikleri**: spout veya Cıvata hakkındaki istatistiklerdir. Sayfada diğer girdiler için zaman dilimini ayarlamak için bağlantıları kullanın **penceresi** sütun.
* **Giriş İstatistikleri** (yalnızca Cıvata): Cıvata tarafından kullanılan giriş akışları hakkında bilgi.
* **Çıkış istatistikleri**: spout veya Cıvata tarafından yayılan akışları hakkında bilgi.
* **Yürütücüler**: spout veya Cıvata örnekleri hakkında bilgi. Seçin **bağlantı noktası** tanılama bilgilerinin günlüğünü görüntülemek belirli bir yürütücü için giriş üretilen Bu örneği için.
* **Hataları**: spout veya Cıvata için herhangi bir hata bilgi.

## <a name="monitor-and-manage-rest-api"></a>İzleme ve yönetme: REST API'si

Benzer yönetim ve izleme işlevselliği REST API'sini kullanarak gerçekleştirebilmek için Storm kullanıcı Arabirimi REST API üzerinde oluşturulmuştur. REST API, yönetmek ve Storm topolojilerini izlemek için özel araçlar oluşturmak için kullanabilirsiniz.

Daha fazla bilgi için bkz: [Storm kullanıcı Arabirimi REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Aşağıdaki bilgiler, REST API kullanarak hdınsight'ta Apache Storm özeldir.

> [!IMPORTANT]
> Storm REST API internet üzerinden genel kullanıma açık değildir ve Hdınsight küme baş düğümüne SSH tüneli kullanılarak erişilebilir olmalıdır. Oluşturma ve SSH tüneli kullanma hakkında daha fazla bilgi için bkz: [kullanım SSH Ambari web kullanıcı Arabirimi, ResourceManager, kaynak, iş, Oozie ve diğer web Uı'lar erişmek için tünel](../hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Taban URI

Linux tabanlı Hdınsight kümeleri REST API için taban URI konumundaki baş düğüm üzerinde kullanılabilir **https://HEADNODEFQDN:8744/api/v1/**. Baş düğüm etki alanı adı, küme oluşturma sırasında oluşturulur ve statik değil.

Küme baş düğüm için tam etki alanı adı (FQDN) birkaç farklı şekilde bulabilirsiniz:

* **Bir SSH oturumundan**: komutunu `headnode -f` küme için bir SSH oturumundan.
* **Ambari Web**: seçin **Hizmetleri** sayfanın üst kısmından seçip **Storm**. Gelen **Özet** sekmesine **Storm kullanıcı Arabirimi sunucu**. REST API ve Storm kullanıcı arabirimini barındıran düğümün FQDN'sini sayfanın üst kısmında görüntülenir.
* **Ambari REST API öğesinden**: komutunu `curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` Storm kullanıcı Arabirimi ve REST API çalıştığı düğüm hakkında bilgi almak için. Değiştir **CLUSTERNAME** küme adı ile. İstendiğinde, oturum açma (Yönetici) hesabı için parolayı girin. Yanıtta, düğümün FQDN'sini "host_name" giriş içeriyor.

### <a name="authentication"></a>Kimlik Doğrulaması

REST API istekleri kullanmalıdır **temel kimlik doğrulaması**, Hdınsight Küme Yönetici adı ve parola kullanın.

> [!NOTE]
> Temel kimlik doğrulaması düz metin kullanarak gönderildiğinden, aşağıdakileri yapmalısınız **her zaman** küme ile iletişimin güvenliğini sağlamak için HTTPS kullanın.

### <a name="return-values"></a>Dönüş değerleri

REST API öğesinden geri döndürülen bilgileri yalnızca küme içinde kullanışlı olabilir. Örneğin, Zookeeper sunucuları için döndürülen tam etki alanı adı (FQDN) Internet'ten erişilebilir değil.

## <a name="next-steps"></a>Sonraki Adımlar

Bilgi edinmek için nasıl [Maven kullanarak geliştirme Java tabanlı topolojiler](apache-storm-develop-java-topology.md).

Daha fazla örnek topolojileri listesi için bkz: [Hdınsight üzerinde Storm için örnek topolojiler](apache-storm-example-topology.md).
