---
title: Hadoop - Azure HDInsight için yüksek kullanılabilirlik
description: Nasıl HDInsight kümeleri güvenilirlik ve kullanılabilirlik ek bir baş düğüm tarafından öğrenin. Nasıl bu yanı sıra Ambari ve, Hive gibi Hadoop Hizmetleri tek tek her baş düğümüne SSH kullanarak bağlamak için etkiliyor öğrenin.
ms.reviewer: jasonh
author: hrasheed-msft
keywords: hadoop yüksek kullanılabilirlik
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: hrasheed
ms.openlocfilehash: 6cb72730ef3dbef81e2b2c9bc1c5cfd3bbd88b65
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704932"
---
# <a name="availability-and-reliability-of-apache-hadoop-clusters-in-hdinsight"></a>Kullanılabilirliği ve güvenilirliği HDInsight Apache Hadoop kümelerini

HDInsight kümeleri, kullanılabilirlik ve Apache Hadoop Hizmetleri ve çalışan işleri güvenilirliğini artırmak için iki baş düğümü sağlar.

Hadoop Hizmetleri ve veri kümesindeki birden çok düğüm arasında çoğaltarak yüksek kullanılabilirlik ve güvenilirlik elde eder. Ancak standardı dağıtımlarla hadoop genellikle yalnızca tek bir baş düğüm gerekir. Herhangi bir kesinti tek bir baş düğüm, küme çalışmayı durdurmasına neden olabilir. HDInsight, Hadoop'ın kullanılabilirliği ve güvenilirliği iyileştirmek için iki baş sağlar.

## <a name="availability-and-reliability-of-nodes"></a>Kullanılabilirlik ve güvenilirlik düğümleri

Azure sanal Makineler'i kullanarak bir HDInsight kümesindeki düğümlere uygulanır. Aşağıdaki bölümlerde, HDInsight ile kullanılan tek tek düğüm türleri açıklanmaktadır. 

> [!NOTE]  
> Tüm düğüm türleri, bir küme türü için kullanılır. Örneğin, bir Hadoop küme türü, herhangi bir Nimbus düğümü yok. HDInsight küme türleri tarafından kullanılan düğümler hakkında daha fazla bilgi için küme türleri bölümüne bakın. [oluşturma Linux tabanlı Hadoop kümeleri HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) belge.

### <a name="head-nodes"></a>Baş düğümler

Hadoop Hizmetleri yüksek kullanılabilirliğini sağlamak için iki baş düğüm HDInsight sağlar. İki baş düğüm, aynı anda etkin ve HDInsight kümesi içinde çalışır. Apache HDFS veya Apache Hadoop YARN gibi bazı hizmetler, belirli bir zamanda yalnızca 'bir baş düğüm üzerinde etkin'. HiveServer2 ya da Hive meta veri deposu gibi diğer hizmetleri aynı anda iki baş düğümler üzerinde etkindir.

Baş düğüm (ve diğer düğümleri HDInsight) ana bilgisayar adı düğümün bir parçası olarak sayısal bir değer var. Örneğin, `hn0-CLUSTERNAME` veya `hn4-CLUSTERNAME`.

> [!IMPORTANT]  
> Sayısal değer, bir düğüm birincil veya ikincil ile ilişkilendirmeyin. Sayısal değer yalnızca her düğüm için benzersiz bir ad sağlamak için mevcuttur.

### <a name="nimbus-nodes"></a>Nimbus Düğümleri

Nimbus düğümleri ile Apache Storm kümelerini mevcuttur. Nimbus düğümleri dağıtma ve çalışan düğümleri arasında işleme izleme Hadoop Jobtracker'a benzer bir işlevsellik sağlar. HDInsight için Storm kümeleri iki Nimbus düğümü sağlar.

### <a name="apache-zookeeper-nodes"></a>Apache Zookeeper düğümleri

[ZooKeeper](https://zookeeper.apache.org/) düğümleri öncü seçimi baş düğümlere ana Hizmetleri için kullanılır. Hizmetler, veri (çalışan) düğümleri ve ağ geçitleri hangi baş düğüm ana hizmet etkin olduğunu bilmeniz sağlamak için de kullanılır. Varsayılan olarak, HDInsight üç ZooKeeper düğümü sağlar.

### <a name="worker-nodes"></a>Çalışan düğümleri

Bir işi kümeye gönderildiğinde çalışan düğümleri gerçek veri analizi gerçekleştirebilir. Bir çalışan düğümü başarısız olursa, görevin yapmakta olduğu başka bir çalışan düğümüne gönderilir. Varsayılan olarak, dört alt düğüm HDInsight oluşturur. Bu sayı sırasında hem Küme oluşturulduktan sonra ihtiyaçlarınıza uyacak şekilde değiştirebilirsiniz.

### <a name="edge-node"></a>Uç düğüm

Bir kenar düğümü, veri analizi kümedeki etkin bir şekilde almaz. Hadoop ile çalışırken, geliştiriciler veya veri bilimcileri tarafından kullanılır. Kenar düğümüne kümedeki diğer düğümlere aynı Azure sanal ağ içinde yer alan ve diğer tüm düğümlere doğrudan erişebilirsiniz. Kenar düğümüne kritik Hadoop Hizmetleri veya analiz işleri uzağa kaynakları yapmadan kullanılabilir.

Şu anda, ML Hizmetleri HDInsight üzerinde kenar düğümüne varsayılan olarak sağlayan tek bir küme türü budur. Kenar düğümüne HDInsight üzerinde ML Hizmetleri için kullanılan işleme için kümeye göndermeden önce düğümünde yerel olarak test R kodu.

Bir kenar düğümü diğer küme türleri ile kullanma hakkında daha fazla bilgi için bkz: [HDInsight içinde kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md) belge.

## <a name="accessing-the-nodes"></a>Düğümleri erişme

Genel bir ağ geçidi üzerinden kümeye internet üzerinden erişim sağlanır. Baş düğümlerine bağlanmak için erişim sınırlıdır ve (varsa varsa) kenar düğümüne. Baş düğümler üzerinde çalışan hizmetlere erişimi birden çok baş düğüm sağlayarak parametreden değil. Ortak ağ geçidi istekleri istenen hizmeti barındıran baş düğüme yönlendirir. Apache Ambari şu anda ikincil baş düğüm üzerinde barındırılıyorsa, örneğin, ağ geçidi gelen istekleri için Ambari bu düğüme yönlendirir.

Ortak ağ geçidi üzerinden erişim bağlantı noktası 443 (HTTPS), 22 ve 23 sınırlıdır.

* Bağlantı noktası __443__ Ambari ve diğer web kullanıcı Arabirimi veya REST API'leri baş düğümler üzerinde barındırılan erişmek için kullanılır.

* Bağlantı noktası __22__ birincil baş düğüme ya da SSH ile kenar düğümüne erişmek için kullanılır.

* Bağlantı noktası __23__ SSH ile ikincil baş düğümüne erişmek için kullanılır. Örneğin, `ssh username@mycluster-ssh.azurehdinsight.net` adlı Küme için birincil baş düğüme bağlanan **mycluster**.

SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belge.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Dahili tam etki alanı adlarını (FQDN)

Bir iç IP adresi ve kümeden erişilebilir yalnızca FQDN bir HDInsight kümesindeki düğümlere sahip. İç FQDN veya IP adresini kullanarak kümedeki hizmetlerin erişirken, IP veya FQDN hizmeti erişirken kullanılacak doğrulamak için Ambari kullanmanız gerekir.

Örneğin, Apache Oozie hizmet yalnızca bir baş düğüm ve kullanarak çalışabilen `oozie` komutu bir SSH oturumundan hizmetin URL'sini gerektirir. Bu URL, aşağıdaki komutu kullanarak Ambari'den alınabilir:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Bu komut bir değer ile kullanmak üzere iç URL içeren aşağıdaki komutu benzer döndürür `oozie` komutu:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Ambari REST API'si ile çalışma hakkında daha fazla bilgi için bkz. [izleme ve yönetme Apache Ambari REST API'yi kullanarak HDInsight](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Diğer düğüm türleri erişme

İnternet üzerinden aşağıdaki yöntemler kullanılarak doğrudan erişilebilir olmayan düğüm bağlanabilirsiniz:

* **SSH**: Bir baş düğüm için SSH kullanarak bağlandıktan sonra SSH baş düğümünden kümedeki diğer düğümlere bağlanmak için kullanabilirsiniz. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

* **SSH tüneli**: İnternet'e açık değil düğümlerinden biri üzerinde barındırılan bir web hizmetine erişmesi gerekiyorsa, bir SSH tüneli kullanmanız gerekir. Daha fazla bilgi için [HDInsight ile SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md) belge.

* **Azure sanal ağı**: HDInsight kümenizi Azure sanal ağının parçası ise, aynı sanal ağdaki herhangi bir kaynağa kümedeki tüm düğümlere doğrudan erişebilirsiniz. Daha fazla bilgi için [Azure sanal ağ kullanarak HDInsight genişletmek](hdinsight-extend-hadoop-virtual-network.md) belge.

## <a name="how-to-check-on-a-service-status"></a>Bir hizmet durumunu kontrol etme

Baş düğümler üzerinde çalışan hizmetler durumunu denetlemek için Ambari Web kullanıcı arabirimini veya Ambari REST API'yi kullanın.

### <a name="ambari-web-ui"></a>Ambari Web UI

Ambari Web kullanıcı arabirimini görülebilir `https://CLUSTERNAME.azurehdinsight.net`. **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstenirse, kümeniz için HTTP kullanıcısı kimlik bilgilerini girin. Varsayılan HTTP kullanıcı adı **yönetici** ve kümeyi oluştururken girdiğiniz parola paroladır.

Ambari sayfada geldiğinde, yüklü hizmetlerin sayfanın sol tarafında listelenir.

![Yüklü hizmetleri](./media/hdinsight-high-availability-linux/services.png)

Bir dizi durumunu göstermek için bir hizmet yanındaki görünebilir simgeler vardır. Kullanarak bir hizmet için ilgili bir uyarı görüntülenebilir **uyarılar** sayfanın üstündeki bağlantısı.  Ambari, önceden tanımlanmış çeşitli uyarılar sağlar.

Aşağıdaki uyarılar, bir kümenin kullanılabilirliğini izlemek yardımcı olur:

| Uyarı Adı                               | Açıklama                                                                                                                                                                                  |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ölçüm İzleyici durumu                    | Bu uyarı izleme durumu komut dosyası tarafından belirlenen şekilde ölçümleri izleme işleminin durumunu gösterir.                                                                                   |
| Ambari aracı sinyali                   | Sunucu bir aracı kişiyle kaybederse, bu uyarı tetiklenir.                                                                                                                        |
| ZooKeeper sunucusu işlemi                 | ZooKeeper sunucu işlemi kadar belirlenen ve ağ üzerinde dinleme yoksa, bu konak düzeyinde uyarı tetiklenir.                                                               |
| IOCache meta verileri sunucu durumu           | IOCache meta veri sunucusu belirlenen kadar ve istemci isteklerine yanıt yoksa, bu konak düzeyinde uyarı tetiklenir                                                            |
| JournalNode Web UI                       | JournalNode Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                                 |
| Spark2 Thrift sunucusu                     | Spark2 Thrift sunucusu kadar belirlenemiyorsa, bu konak düzeyinde uyarı tetiklenir.                                                                                                |
| Geçmiş sunucu işlemi                   | Geçmiş sunucu işlemi kadar oluşturulmuş ve ağ üzerinde dinleme yoksa, bu konak düzeyinde uyarı tetiklenir.                                                                |
| Geçmiş sunucu Web kullanıcı Arabirimi                    | Geçmiş sunucu Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                              |
| ResourceManager Web kullanıcı Arabirimi                   | ResourceManager Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                             |
| NodeManager sistem durumu özeti               | Sağlıksız NodeManagers varsa bu hizmet düzeyi uyarı tetiklenir                                                                                                                    |
| Uygulama zaman çizelgesi Web kullanıcı Arabirimi                      | Uygulama zaman çizelgesi sunucusu Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                         |
| DataNode sistem durumu özeti                  | Sağlıksız DataNodes varsa bu hizmet düzeyi uyarı tetiklenir                                                                                                                       |
| NameNode Web kullanıcı Arabirimi                          | NameNode Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                                    |
| ZooKeeper yük devretme denetleyicisi işlemi    | ZooKeeper yük devretme denetleyicisi işlemi kadar Onaylandı ve ağ üzerinde dinleme yoksa, bu konak düzeyinde uyarı tetiklenir.                                                   |
| Oozie Server Web UI                      | Web kullanıcı arabirimini Oozie sunucu ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                                |
| Oozie sunucu durumu                      | Oozie sunucunun kadar belirlenen ve istemci isteklerine yanıt yoksa, bu konak düzeyinde uyarı tetiklenir.                                                                      |
| Hive meta veri deposu işlemi                   | Hive meta veri deposu işlemi kadar belirlenen ve ağ üzerinde dinleme yoksa, bu konak düzeyinde uyarı tetiklenir.                                                                 |
| HiveServer2 işlemi                      | HiveServer kadar belirlenen ve istemci isteklerine yanıt yoksa, bu konak düzeyinde uyarı tetiklenir.                                                                        |
| WebHCat sunucu durumu                    | Templeton da sunucu durumu iyi değilse, bu konak düzeyinde uyarı tetiklenir.                                                                                                            |
| Yüzde ZooKeeper sunucusu yok      | Kümedeki sunucuların ZooKeeper aşağı sayısı kritik yapılandırılan eşik değerinden büyükse, bu uyarı tetiklenir. Bu, ZooKeeper işlem denetimlerin sonuçlarını toplar.     |
| Spark2 Livy sunucusu                       | Livy2 sunucunun kadar belirlenemiyorsa, bu konak düzeyinde uyarı tetiklenir.                                                                                                        |
| Spark2 geçmiş sunucusu                    | Spark2 geçmiş sunucusu kadar belirlenemiyorsa, bu konak düzeyinde uyarı tetiklenir.                                                                                               |
| Ölçüm toplama işlemi                | Ölçümleri Toplayıcı üzerinde yapılandırılan bağlantı noktası için eşik eşit saniye sayısı kadar Onaylandı ve dinleme yoksa, bu uyarı tetiklenir.                                 |
| Ölçümleri Toplayıcı - HBase ana işlem | Ölçümleri Toplayıcı'nın HBase master işlemleri kadar Onaylandı ve saniyeler içinde verilen yapılandırılmış bir kritik eşik için ağda dinleme yoksa, bu uyarı tetiklenir. |
| Yüzde ölçümleri izleyiciler kullanılabilir       | Ölçümleri izleme işlemleri yukarı değildir ve yapılandırılmış uyarı ve kritik eşiklerini için ağda dinleme yüzdesi değilse bu uyarı tetiklenir.                             |
| Yüzde NodeManagers kullanılabilir           | Sayısını NodeManagers aşağı kümedeki kritik yapılandırılan eşik değerinden büyükse, bu uyarı tetiklenir. Bunu NodeManager işlem denetimlerin sonuçlarını toplar.        |
| NodeManager sistem durumu                       | Bu konak düzeyinde uyarı NodeManager bileşeninden kullanılabilir düğüm durumu özelliği denetler.                                                                                              |
| NodeManager Web UI                       | NodeManager Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                                 |
| NameNode yüksek kullanılabilirlik sistem durumu        | Etkin NameNode ya da bekleme NameNode çalışmıyorsa, bu hizmet düzeyi uyarısı tetiklenir.                                                                                     |
| DataNode işlemi                         | Bağımsız DataNode işlemlere kadar oluşturulmuş ve ağ üzerinde dinleme yoksa, bu konak düzeyinde uyarı tetiklenir.                                                         |
| DataNode Web kullanıcı Arabirimi                          | DataNode Web kullanıcı arabirimini ulaşılamaz durumdaysa bu konak düzeyinde uyarı tetiklenir.                                                                                                                    |
| Yüzde JournalNodes kullanılabilir           | Sayısını JournalNodes aşağı kümedeki kritik yapılandırılan eşik değerinden büyükse, bu uyarı tetiklenir. Bunu JournalNode işlem denetimlerin sonuçlarını toplar.        |
| Yüzde DataNodes kullanılabilir              | Sayısını DataNodes aşağı kümedeki kritik yapılandırılan eşik değerinden büyükse, bu uyarı tetiklenir. Bunu DataNode işlem denetimlerin sonuçlarını toplar.              |
| Zeppelin sunucu durumu                   | Zeppelin sunucunun kadar belirlenen ve istemci isteklerine yanıt yoksa, bu konak düzeyinde uyarı tetiklenir.                                                                   |
| HiveServer2 etkileşimli işlem          | HiveServerInteractive kadar belirlenen ve istemci isteklerine yanıt yoksa, bu konak düzeyinde uyarı tetiklenir.                                                             |
| LLAP uygulama                         | LLAP uygulama kadar belirlenen ve isteklere yanıt yoksa, bu uyarı tetiklenir.                                                                                    |

Daha fazla bilgi görüntülemek için her hizmeti seçebilirsiniz.

Hizmet sayfasını durumu ve her hizmetin yapılandırma bilgileri sağlarken, hangi baş düğümde hizmeti çalışan bilgi sağlamaz. Bu bilgileri görüntülemek için kullanın **konakları** sayfanın üstündeki bağlantısı. Bu sayfa, baş düğümler dahil olmak üzere, küme içindeki konaklar görüntüler.

![Konaklar listesine](./media/hdinsight-high-availability-linux/hosts.png)

Baş düğümlerinden biri için bağlantıyı seçerek ilgili düğümde çalışan bileşenleri ve hizmetleri görüntüler.

![Bileşen Durumu](./media/hdinsight-high-availability-linux/nodeservices.png)

Ambari kullanarak daha fazla bilgi için bkz: [İzleyici ve HDInsight Apache Ambari Web kullanıcı arabirimini kullanarak yönetme](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST API

Ambari REST API internet üzerinden kullanılabilir. HDInsight ortak ağ geçidi şu anda REST API'sini barındıran baş düğüme Yönlendirme isteklerini işler.

Ambari REST API aracılığıyla bir hizmetin durumunu denetlemek için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Değiştirin **parola** ile HTTP (Yönetici) kullanıcı hesabının parolası.
* **CLUSTERNAME** değerini kümenin adıyla değiştirin.
* Değiştirin **SERVICENAME** durumunu denetlemek istediğiniz hizmetin adı.

Örneğin, durumunu denetlemek için **HDFS** adlı bir küme hizmetinin **mycluster**, bir parola ile **parola**, şu komutu kullanırsınız:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

Yanıt aşağıdaki JSON ile benzer:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

URL bize adlı bir baş düğüm üzerinde şu anda hizmetinin çalışıp çalışmadığını gösterir. **hn0 CLUSTERNAME**.

Durum bize Hizmet şu anda çalışıyor gösterir veya **başlatıldı**.

Hangi hizmetlerin kümeye yüklü bilmiyorsanız listesini almak için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Ambari REST API'si ile çalışma hakkında daha fazla bilgi için bkz. [izleme ve yönetme Apache Ambari REST API'yi kullanarak HDInsight](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Hizmet bileşenleri

Hizmetleri, ayrı ayrı durumunu denetlemek istediğiniz bileşenleri içeriyor olabilir. Örneğin, HDFS, NameNode bileşeni içerir. Bir bileşen hakkında bilgi görüntülemek için komut şöyle olabilir:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Hangi bileşenlerin bir hizmeti tarafından sağlanan bilmiyorsanız listesini almak için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Baş düğümler hakkında günlük dosyalarına erişmek nasıl

### <a name="ssh"></a>SSH

Altında bir baş düğüm SSH üzerinden bağlıyken, günlük dosyalarını bulunabilir **/var/log**. Örneğin, **/var/log/hadoop-yarn/yarn** için YARN günlükleri içerir.

Hem de günlüklerini denetlemeniz gerekir böylece her bir baş düğüm benzersiz günlük girişlerini olabilir.

### <a name="sftp"></a>SFTP

Ayrıca, SSH Dosya Aktarım Protokolü veya güvenli dosya aktarım protokolünü (SFTP) kullanarak baş düğümüne bağlanın ve günlük dosyalarını doğrudan karşıdan yüklemek.

Benzer şekilde, bir SSH istemcisi kullanarak, ne zaman, kümeye bağlanarak SSH kullanıcı hesabı adını ve küme SSH adresini sağlamanız gerekir. Örneğin, `sftp username@mycluster-ssh.azurehdinsight.net`. İstendiğinde hesabı için parolayı belirtin veya bir ortak anahtarınızı kullanarak sağlamak `-i` parametresi.

Bağlantı kurulduktan sonra ile sunulan bir `sftp>` istemi. Bu İstemi'nden dizinleri değiştirebilirsiniz yükleme ve dosyaları indirme. Örneğin, aşağıdaki komutları dizinleri **/var/log/hadoop/hdfs** dizin ve ardından indirme dizindeki tüm dosyaları.

    cd /var/log/hadoop/hdfs
    get *

Kullanılabilir komutların listesini girin `help` adresindeki `sftp>` istemi.

> [!NOTE]  
> SFTP kullanarak bağlı dosya sistemi görselleştirmenize olanak tanıyan grafik arabirimleri vardır. Örneğin, [MobaXTerm](https://mobaxterm.mobatek.net/) Windows Gezgini için benzer bir arabirim kullanarak dosya sistemine göz atmanızı sağlar.

### <a name="ambari"></a>Ambari

> [!NOTE]  
> Ambari kullanarak günlük dosyalarına erişmek için bir SSH tüneli kullanmanız gerekir. Bireysel hizmet web arabirimleri Internet üzerinde herkese açık değildir. SSH tüneli kullanma hakkında daha fazla bilgi için bkz. [SSH tünel oluşturmayı kullanma](hdinsight-linux-ambari-ssh-tunnel.md) belge.

Ambari Web kullanıcı arabirimini (örneğin, YARN için) günlüklerini görüntülemek istediğiniz hizmeti seçin. Ardından **hızlı bağlantılar** hangi baş düğüm için günlükleri görüntülemek için seçin.

![Günlükleri görüntülemek için hızlı bağlantıları kullanma](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a>Düğüm boyutunu yapılandırma

Bir düğümün boyutu yalnızca küme oluşturma sırasında seçilir. HDInsight üzerinde farklı VM boyutlarının listesini bulabilirsiniz [HDInsight fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/hdinsight/).

Bir küme oluştururken, düğümlerin boyutunu belirtebilirsiniz. Aşağıdaki bilgileri boyutu kullanarak belirleme konusunda rehberlik sağlar [Azure portalında][preview-portal], [Azure PowerShell modülü Az][azure-powershell], ve [Azure CLI][azure-cli]:

* **Azure portalında**: Bir küme oluştururken, küme tarafından kullanılan düğümlerin boyutu ayarlayabileceğiniz:

    ![Düğüm boyutu seçimi ile küme oluşturma Sihirbazı'nı görüntüsü](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Azure CLI**: Kullanırken [az hdınsight oluşturma](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) komutunu kullanarak baş, çalışan ve ZooKeeper düğümleri boyutu ayarlayabileceğiniz `--headnode-size`, `--workernode-size`, ve `--zookeepernode-size` parametreleri.

* **Azure PowerShell**: Kullanırken [yeni AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) cmdlet'ini kullanarak baş, çalışan ve ZooKeeper düğümleri boyutunu ayarlayabilirsiniz `-HeadNodeSize`, `-WorkerNodeSize`, ve `-ZookeeperNodeSize` parametreleri.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede belirtilen noktalar hakkında daha fazla bilgi için aşağıdaki bağlantıları kullanın.

* [Apache Ambari REST başvurusu](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Azure CLI'yi yükleme ve yapılandırma](https://docs.microsoft.com//cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure PowerShell modülünün Az yüklenmesi ve yapılandırılması](/powershell/azure/overview)
* [Apache Ambari kullanarak HDInsight'ı yönetme](hdinsight-hadoop-manage-ambari.md)
* [Linux tabanlı HDInsight kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
