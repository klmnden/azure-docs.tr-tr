---
title: Ambari API - Azure kullanarak HDInsight Hadoop kümelerini izleme
description: Oluşturma, yönetme ve Hadoop kümelerini izleme için Apache Ambari API'lerini kullanın. Sezgisel operatör araçlarına ve API'leri, Hadoop'un karmaşıklığını gizler.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/07/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: ff6601042c82cef2b0101833117f17aca8b463dc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58223266"
---
# <a name="monitor-apache-hadoop-clusters-in-hdinsight-using-the-apache-ambari-api"></a>Apache Ambari API'sini kullanarak HDInsight Apache Hadoop kümelerini izleme
Apache Ambari API'lerini kullanarak HDInsight kümelerini izleme hakkında bilgi edinin.

> [!NOTE]  
> Bu makalede, Ambari REST API'yi salt okunur bir sürümünü sağlayın. öncelikle Windows tabanlı HDInsight kümeleri için bilgilerdir. Linux tabanlı kümeler için bkz: [Apache Ambari kullanarak yönetme Apache Hadoop kümelerini](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Ambari nedir?
[Apache Ambari] [ ambari-home] sağlama, yönetme ve Apache Hadoop kümelerini izleme için kullanılır. Bu, kümelerin çalışmasını kolaylaştırarak, işleç araçlarının sezgisel bir koleksiyonunu ve Hadoop’un karmaşıklığı gizleyen sağlam bir API kümesini içerir. API'leri hakkında daha fazla bilgi için bkz. [Ambari API Başvurusu][ambari-api-reference]. 

HDInsight şu anda yalnızca Ambari izleme özelliğini destekler. Ambari API 1.0, HDInsight sürüm 3.0 ve 2.1 kümeleri tarafından desteklenir. Bu makalede, HDInsight sürüm 3.1 ve 2.1 kümelerinde erişen Ambari API anlatılmaktadır. İkisi arasındaki temel fark, bazı bileşenler (örneğin, iş geçmişi sunucusu) yeni özellikleri kullanıma sunulması ile değiştirilmiştir ' dir. 

**Önkoşullar**

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Azure PowerShell içeren bir iş istasyonu**.
* (İsteğe bağlı) [cURL][curl]. Yüklemek için bkz: [sürümleri ve indirmeler cURL][curl-download].
  
  > [!NOTE]  
  > Ne zaman Windows, seçenek değerlerinin tek tırnak işaretleri yerine çift tırnak işaretleri kullan cURL komutunu kullanın.

* **Bir Azure HDInsight kümesi**. Küme sağlama hakkında yönergeler için bkz. [HDInsight kullanmaya başlama] [ hdinsight-get-started] veya [sağlama HDInsight kümeleri][hdinsight-provision]. Bu öğreticiyi incelemek için aşağıdaki veriler ihtiyacınız vardır:
  
  | Küme özelliği | Azure PowerShell değişken adı | Değer | Açıklama |
  | --- | --- | --- | --- |
  |   HDInsight küme adı |$clusterName | |HDInsight kümenizin adıdır. |
  |   Küme kullanıcı adı |$clusterUsername | |Küme kullanıcı adı, küme oluşturulduğu zaman belirtildi. |
  |   Küme parolası |$clusterPassword | |Küme kullanıcı parolası. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Hemen başlayın
HDInsight kümelerinizi izlemek için Ambari kullanmak için birkaç yol vardır.

**Azure PowerShell’i kullanma**

MapReduce işi İzleyicisi bilgileri aşağıdaki Azure PowerShell betiğini alır *HDInsight 3.5 kümesinde.*  En önemli fark, size bu ayrıntıları YARN hizmeti (yerine MapReduce) çekme ' dir.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

MapReduce işi İzleyicisi bilgileri aşağıdaki PowerShell betiğini alır *2.1 HDInsight kümesinde*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Çıktı.

![Jobtracker'a çıkış][img-jobtracker-output]

**cURL kullanma**

Aşağıdaki örnek, cURL kullanarak küme bilgileri alır:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Çıktı.

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**8/10/2014 sürümü için**:

Ambari uç noktası, kullanıldığında "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *host_name* alan ana bilgisayar adı yerine düğüm tam etki alanı adını (FQDN) döndürür. 8/10/2014 yayınlanmadan önce bu örnek döndürülen yalnızca "**headnode0**". 8/10/2014 sürümünden sonra FQDN Al "**headnode0. { ClusterDNS} .azurehdinsight .net**", bir önceki örnekte gösterildiği gibi. Bu değişikliğin nerede birden çok küme türleri (örneğin, HBase ve Hadoop gibi) bir sanal ağ (VNET) içinde dağıtılabilir senaryoları kolaylaştırmak için gereklidir. Bu, örneğin, Hadoop için bir arka uç platform HBase kullanırken gerçekleşir.

## <a name="ambari-monitoring-apis"></a>Ambari API izleme
Aşağıdaki tabloda en yaygın Ambari API çağrıları izleme bazıları listelenmektedir. API hakkında daha fazla bilgi için bkz. [Apache Ambari API Başvurusu][ambari-api-reference].

| İzleyici API çağrısı | URI | Açıklama |
| --- | --- | --- |
| Kümeleri Al |`/api/v1/clusters` | |
| Küme bilgilerini al. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |kümeler, hizmetleri, ana bilgisayarlar |
| Hizmetleri edinin |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Hizmetlere şunlar dahildir: hdfs, mapreduce |
| Hizmetler bilgi alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Hizmet bileşenleri |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: namenode, datanodeMapReduce: jobtracker'a; tasktracker |
| Bileşen Bilgisi alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo bileşenlerini barındıracak, ölçümleri |
| Konak alma |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0, workernode0 |
| Konak bilgilerini alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Ana bileşenleri |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |namenode, resourcemanager |
| Ana bileşen bilgisi alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles, bileşen, ana ölçümleri |
| Yapılandırmalarını alma |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Yapılandırma türü: çekirdek site, hdfs site, site mapred, hive-site |
| Yapılandırma bilgilerini alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Yapılandırma türü: çekirdek site, hdfs site, site mapred, hive-site |

## <a name="next-steps"></a>Sonraki Adımlar
Artık Apache Ambari izleme API çağrılarının nasıl kullanılacağını öğrendiniz. Daha fazla bilgi için bkz:

* [Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [Azure PowerShell kullanarak HDInsight kümelerini yönetme][hdinsight-admin-powershell]
* [Komut satırı arabirimi kullanarak HDInsight kümelerini yönetme][hdinsight-admin-cli]
* [HDInsight belgeleri][hdinsight-documentation]
* [HDInsight ile çalışmaya başlama][hdinsight-get-started]

[ambari-home]: https://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: https://curl.haxx.se
[curl-download]: https://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: https://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
