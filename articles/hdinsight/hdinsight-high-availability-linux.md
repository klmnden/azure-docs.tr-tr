---
title: "Yüksek kullanılabilirlik için Hadoop - Azure Hdınsight | Microsoft Docs"
description: "Nasıl Hdınsight kümeleri güvenilirlik ve kullanılabilirlik ek bir baş düğüm kullanarak iyileştirmek öğrenin. Nasıl bu Ambari ve Hive gibi Hadoop Hizmetleri yanı sıra tek tek SSH kullanarak her baş düğümüne bağlanmak için etkilediğini öğrenin."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: "hadoop yüksek kullanılabilirlik"
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/19/2017
ms.author: larryfr
ms.openlocfilehash: 39894ba73c691ad547d8b5ab67ec9d5786a5229c
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Hadoop kümelerinin kullanılabilirliği ve güvenilirliği

Hdınsight kümeleri, kullanılabilirlik ve Hadoop Hizmetleri ve çalışan işleri güvenilirliğini artırmak için iki baş düğümler sağlar.

Hadoop bir kümede birden çok düğüm arasında Hizmetleri ve veri çoğaltma tarafından yüksek kullanılabilirlik ve güvenilirlik elde eder. Ancak standart dağıtımları hadoop genellikle yalnızca tek bir baş düğüm vardır. Tek baş düğümü herhangi kesinti, küme çalışmayı durdurmasına neden olabilir. Hdınsight Hadoop'ın kullanılabilirliği ve güvenilirliği iyileştirmek için iki headnodes sağlar.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="availability-and-reliability-of-nodes"></a>Kullanılabilirliği ve güvenilirliği düğümlerinin

Hdınsight kümesi düğümünde, Azure sanal makineleri kullanarak uygulanır. Aşağıdaki bölümlerde, Hdınsight ile kullanılan tek tek düğüm türleri açıklanmaktadır. 

> [!NOTE]
> Tüm düğüm türleri için bir küme türü kullanılır. Örneğin, Hadoop küme türü hiçbir Nimbus düğümü yok. Hdınsight küme türleri tarafından kullanılan düğümleri hakkında daha fazla bilgi için küme türleri bölümüne bakın [Hdınsight oluşturma Linux tabanlı Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) belge.

### <a name="head-nodes"></a>Baş düğümler

Hadoop Hizmetleri yüksek kullanılabilirliğini sağlamak için iki baş düğüm Hdınsight sağlar. Her iki baş düğümler aynı anda etkin ve Hdınsight kümesi içinde çalışır. HDFS veya YARN gibi bazı hizmetler, herhangi bir anda yalnızca bir baş düğüm üzerinde ' active'. HiveServer2 veya Hive meta depo gibi başka hizmetleri aynı anda hem baş düğümler üzerinde etkindir.

Baş düğümler (ve diğer düğümlere hdınsight'ta) ana bilgisayar adı düğümün bir parçası olarak sayısal bir değere sahip. Örneğin, `hn0-CLUSTERNAME` veya `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> Sayısal değer, bir düğüm birincil veya ikincil ile ilişkilendirmeyin. Sayısal değer yalnızca her düğüm için benzersiz bir ad sağlamak için mevcuttur.

### <a name="nimbus-nodes"></a>Nimbus Düğümleri

Nimbus düğümü Storm kümeleri ile kullanılabilir. Nimbus düğümü dağıtma ve çalışan düğümleri arasında işlem izleme Hadoop Jobtracker'a benzer işlevsellik sağlar. Hdınsight iki Nimbus düğümü Storm kümeleri için sağlar.

### <a name="zookeeper-nodes"></a>Zookeeper düğümleri

[ZooKeeper](http://zookeeper.apache.org/) düğümleri öncü seçim baş düğümler üzerinde ana Hizmetleri için kullanılır. Hizmetleri, veri (çalışan) düğümlerini ve ağ geçitlerini hangi baş düğüm hizmet ana etkin biliyor olması güvence altına için de kullanılır. Varsayılan olarak, Hdınsight üç ZooKeeper düğümleri sağlar.

### <a name="worker-nodes"></a>Çalışan düğümü

Bir işi kümeye gönderildiğinde çalışan düğümleri gerçek veri çözümlemesi gerçekleştirme. Çalışan düğüm başarısız olursa, bunu gerçekleştirme görev başka bir çalışan düğümüne gönderilir. Varsayılan olarak, dört alt düğüm Hdınsight oluşturur. Bu numarayı sırasında hem Küme oluşturulduktan sonra gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

### <a name="edge-node"></a>Kenar düğümüne

Bir kenar düğümüne, veri analizi kümedeki etkin olarak katılmıyor. Hadoop ile çalışırken, geliştiriciler veya veri bilimcilerine tarafından kullanılır. Kenar düğümüne kümedeki diğer düğümlere aynı Azure sanal ağ içinde yaşar ve diğer tüm düğümlere doğrudan erişebilirsiniz. Kenar düğümüne kritik Hadoop Hizmetleri veya analiz işleri çıktığınızda kaynakları bırakmadan kullanılabilir.

Şu anda, hdınsight'ta R Server varsayılan olarak bir kenar düğümüne sağlayan yalnızca küme türüdür. Kenar düğümüne hdınsight'ta R Server için kullanılan düğümde dağıtılan işleme için kümeye göndermeden önce yerel olarak test R kodu.

R Server dışındaki küme türleriyle bir kenar düğümüne kullanma hakkında daha fazla bilgi için bkz: [Hdınsight'ta kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md) belge.

## <a name="accessing-the-nodes"></a>Düğümlere erişme

Ortak ağ geçidi üzerinden internet üzerinden kümesine erişimi sağlanır. Erişim baş düğümler bağlanmakta sınırlı ve (varsa,) kenar düğümüne. Baş düğümler üzerinde çalışan hizmetlerine erişim birden çok baş düğümler sağlayarak parametreden etkilenir değil. Ortak ağ geçidi istekleri istenen hizmeti barındıran baş düğüme yönlendirir. Ambari şu anda ikincil baş düğüm üzerinde barındırılıyorsa, örneğin, ağ geçidi gelen istekleri için Ambari bu düğüme yönlendirir.

Ortak ağ geçidi üzerinden erişim bağlantı noktası 443 (HTTPS), 22 ve 23 sınırlıdır.

* Bağlantı noktası __443__ Ambari ve diğer web kullanıcı Arabirimi veya baş düğümler üzerinde barındırılan REST API'lerine erişmek için kullanılır.

* Bağlantı noktası __22__ birincil baş düğüm veya kenar düğümüne SSH ile erişmek için kullanılır.

* Bağlantı noktası __23__ ikincil baş düğüm SSH ile erişmek için kullanılır. Örneğin, `ssh username@mycluster-ssh.azurehdinsight.net` adlı Küme birincil baş düğüme bağlanan **mycluster**.

SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belge.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>İç tam etki alanı adları (FQDN)

Bir Hdınsight küme düğümlerinde bir iç IP adresi ve yalnızca kümeden erişilebilir FQDN vardır. İç FQDN veya IP adresi kullanarak kümeye Services'de erişilirken, IP veya FQDN hizmeti erişirken kullanılacak doğrulamak için Ambari kullanmanız gerekir.

Örneğin, Oozie hizmeti yalnızca bir baş düğüm ve kullanarak çalışabilir `oozie` bir SSH oturumu komuttan Hizmeti'ne URL gerektirir. Bu URL, aşağıdaki komutu kullanarak Ambari alınabilir:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Bu komut bir değer ile kullanılacak İç URL içeren aşağıdaki komutu benzer döndürür `oozie` komutu:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Ambari REST API ile çalışma hakkında daha fazla bilgi için bkz: [izleme ve yönetme Ambari REST API kullanarak Hdınsight'ta](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Diğer düğüm türleri erişme

Internet üzerinden aşağıdaki yöntemleri kullanarak doğrudan erişilemeyen düğümlere bağlanabilir:

* **SSH**: SSH kullanarak bir baş düğüm bağlandıktan sonra ardından SSH baş düğümünden kümedeki diğer düğümlere bağlamak için kullanabilirsiniz. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

* **SSH tüneli**: Internet'e gösterilmeyen düğümlerinden biri üzerinde barındırılan bir web hizmetine erişim gerekiyorsa, bir SSH tüneli kullanmanız gerekir. Daha fazla bilgi için bkz: [Hdınsight ile SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md) belge.

* **Azure sanal ağı**: Hdınsight kümenize bir Azure sanal ağının parçası ise, aynı sanal ağda herhangi bir kaynağa kümedeki tüm düğümlere doğrudan erişebilirsiniz. Daha fazla bilgi için bkz: [Azure sanal ağını kullanarak Hdınsight genişletmek](hdinsight-extend-hadoop-virtual-network.md) belge.

## <a name="how-to-check-on-a-service-status"></a>Bir hizmet durumunu denetleme

Baş düğümler üzerinde çalışan hizmetleri durumunu denetlemek için Ambari Web kullanıcı arabirimini veya Ambari REST API kullanın.

### <a name="ambari-web-ui"></a>Ambari Web kullanıcı Arabirimi

Ambari Web kullanıcı arabirimini https://CLUSTERNAME.azurehdinsight.net görülebilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstenirse, kümeniz için HTTP kullanıcı kimlik bilgilerini girin. Varsayılan HTTP kullanıcı adı **yönetici** ve küme oluştururken, girdiğiniz parola paroladır.

Ambari sayfasında geldiğinde, yüklü hizmetleri sayfanın sol tarafta listelenir.

![Yüklü hizmetleri](./media/hdinsight-high-availability-linux/services.png)

Durumu göstermek için bir hizmet yanındaki görünebilir simgeleri bir dizi vardır. Kullanarak bir hizmeti ile ilgili herhangi bir uyarı görüntülenebilir **uyarıları** sayfanın üst kısmındaki bağlantı. Daha fazla bilgi görüntülemek için her hizmet seçebilirsiniz.

Hizmet sayfasının durumu ve her hizmetin yapılandırma hakkında bilgi sağlarken, hangi baş düğümünde hizmetini çalıştıran bilgi sağlamaz. Bu bilgileri görüntülemek için kullanın **ana** sayfanın üst kısmındaki bağlantı. Bu sayfa baş düğümler de dahil olmak üzere küme içindeki konaklar görüntüler.

![ana bilgisayarların listesi](./media/hdinsight-high-availability-linux/hosts.png)

Baş düğümlerden birinde bağlantısını seçerek, o düğümde çalışan bileşenleri ve hizmetleri görüntüler.

![Bileşen Durumu](./media/hdinsight-high-availability-linux/nodeservices.png)

Ambari kullanarak daha fazla bilgi için bkz: [İzleyici ve Hdınsight Ambari Web kullanıcı arabirimini kullanarak yönetin](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST API

Ambari REST API Internet üzerinden kullanılabilir. Hdınsight ortak ağ geçidi şu anda REST API barındırma baş düğümüne yönlendirme isteklerini işler.

Ambari REST API aracılığıyla bir hizmetin durumunu denetlemek için aşağıdaki komutu kullanın:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Değiştir **parola** HTTP kullanıcı (Yönetici) hesabı parolası ile.
* **CLUSTERNAME** değerini kümenin adıyla değiştirin.
* Değiştir **SERVICENAME** durumunu denetlemek istediğiniz hizmetin adını.

Örneğin, durumunu denetlemek için **HDFS** adlı bir kümede hizmet **mycluster**, bir parola ile **parola**, şu komutu kullanırsınız:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

Yanıt aşağıdaki JSON benzer:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

URL bize Hizmet şu anda adlı bir baş düğüm üzerinde çalışan söyler **hn0 CLUSTERNAME**.

Durum Hizmet şu anda çalışıyor bize bildiren veya **başlatıldı**.

Hangi hizmetlerin kümeye yüklü bilmiyorsanız listesini almak için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Ambari REST API ile çalışma hakkında daha fazla bilgi için bkz: [izleme ve yönetme Ambari REST API kullanarak Hdınsight'ta](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Hizmet bileşenleri

Hizmetleri, ayrı ayrı durumunu denetlemek istediğiniz bileşenleri içeriyor olabilir. Örneğin, HDFS iş bileşeni içerir. Bir bileşenin bilgilerini görüntülemek için komut olur:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Hangi bileşenlerin bir hizmeti tarafından sağlanan bilmiyorsanız listesini almak için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Baş düğümler günlük dosyalarına erişmek nasıl

### <a name="ssh"></a>SSH

SSH aracılığıyla bir baş düğüm bağlıyken, günlük dosyalarını altında bulunabilir **/var/log**. Örneğin, **/var/log/hadoop-yarn/yarn** için YARN günlüklerini içerir.

Günlükleri, her ikisi de denetlemeniz gerekir böylece her baş düğüm benzersiz günlük girişlerini olabilir.

### <a name="sftp"></a>SFTP

Ayrıca SSH Dosya Aktarım Protokolü veya güvenli Dosya Aktarım Protokolü (SFTP) kullanarak baş düğümüne bağlanmak ve günlük dosyaları doğrudan indirin.

Benzer şekilde bir SSH istemcisi kullanarak, ne zaman kümeye bağlanarak SSH kullanıcı hesabı adını ve kümenin SSH adresi sağlamanız gerekir. Örneğin, `sftp username@mycluster-ssh.azurehdinsight.net`. İstendiğinde hesabı için parola sağlayın ya da bir ortak anahtar kullanarak sağlayın `-i` parametresi.

Bağlantı kurulduktan sonra size sunulan bir `sftp>` istemi. Bu isteminde dizinleri değiştirebilir, dosyaları yükleme ve indirme. Örneğin, aşağıdaki komutları dizinleri değiştirme **/var/log/hadoop/hdfs** dizin ve dizindeki tüm dosyaları sonra yükleme.

    cd /var/log/hadoop/hdfs
    get *

Kullanılabilir komutlar listesi için girin `help` adresindeki `sftp>` istemi.

> [!NOTE]
> Ayrıca, dosya sistemi SFTP kullanarak bağlandığında görselleştirmek izin grafik arabirimi vardır. Örneğin, [MobaXTerm](http://mobaxterm.mobatek.net/) Windows Explorer için benzer bir arabirim kullanarak dosya sistemi göz atmanızı sağlar.

### <a name="ambari"></a>Ambari

> [!NOTE]
> Ambari kullanarak günlük dosyalarına erişmek için bir SSH tüneli kullanmanız gerekir. Tek tek Hizmetleri için web arabirimleri, Internet üzerinde herkese açık şekilde sunulmaz. SSH tüneli kullanma hakkında daha fazla bilgi için bkz: [kullanım SSH tünel](hdinsight-linux-ambari-ssh-tunnel.md) belge.

Ambari Web kullanıcı arabirimini (örneğin, YARN için) günlüklerini görüntülemek istediğiniz hizmeti seçin. Ardından **hızlı bağlantılar** hangi baş düğüm için günlükleri görüntülemek için seçin.

![Günlükleri görüntülemek için hızlı bağlantıları kullanma](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a>Düğüm boyutu yapılandırma

Bir düğümün boyutu, küme oluşturma sırasında yalnızca seçilebilir. Hdınsight üzerinde farklı VM boyutlarının listesini bulabilirsiniz [Hdınsight fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/hdinsight/).

Bir küme oluştururken, düğümlerin boyutu belirtebilirsiniz. Aşağıdaki bilgileri boyutu kullanarak belirleme konusunda rehberlik sağlar [Azure portal][preview-portal], [Azure PowerShell][azure-powershell]ve [Azure CLI][azure-cli]:

* **Azure portal**: bir küme oluştururken, küme tarafından kullanılan düğümlerin boyutu ayarlayabilirsiniz:

    ![Düğüm boyutu seçimi ile küme oluşturma Sihirbazı'nın resmi](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Azure CLI**: kullanırken `azure hdinsight cluster create` komutunu kullanarak head, çalışan ve ZooKeeper düğümleri boyutunu ayarlayabilirsiniz `--headNodeSize`, `--workerNodeSize`, ve `--zookeeperNodeSize` parametreleri.

* **Azure PowerShell**: kullanırken `New-AzureRmHDInsightCluster` cmdlet'ini kullanarak head, çalışan ve ZooKeeper düğümleri boyutunu ayarlayabilirsiniz `-HeadNodeVMSize`, `-WorkerNodeSize`, ve `-ZookeeperNodeSize` parametreleri.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede belirtilen noktalar hakkında daha fazla bilgi için aşağıdaki bağlantıları kullanın.

* [Ambari REST başvurusu](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md)
* [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview)
* [Ambari kullanarak Hdınsight yönetme](hdinsight-hadoop-manage-ambari.md)
* [Linux tabanlı Hdınsight kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
