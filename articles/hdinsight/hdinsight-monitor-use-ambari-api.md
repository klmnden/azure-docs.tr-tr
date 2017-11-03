---
title: "-Azure Ambari API kullanarak hdınsight'ta Hadoop kümelerini izleme | Microsoft Docs"
description: "Apache Ambari API'ları oluşturmak, yönetmek ve Hadoop kümelerini izleme için kullanın. Sezgisel işleci Araçlar ve API'ler Hadoop karmaşıklığını gizleyin."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
editor: cgronlun
manager: jhubbard
ms.assetid: 052135b3-d497-4acc-92ff-71cee49356ff
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: b6fc2098027690eb76b69b1427f0e9541b8a7a69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Ambari API'sini kullanarak HDInsight'ta Hadoop kümelerini izleme
Ambari API kullanarak Hdınsight kümelerini izlemek öğrenin.

> [!NOTE]
> Bu makaledeki bilgiler Ambari REST API salt okunur bir sürümünü sağlayın öncelikle Windows tabanlı Hdınsight kümeleri için ' dir. Linux tabanlı kümeler için bkz: [yönetmek Hadoop kümeleri Ambari kullanarak](hdinsight-hadoop-manage-ambari.md).
> 
> 

## <a name="what-is-ambari"></a>Ambari nedir?
[Apache Ambari] [ ambari-home] hazırlamak, yönetmek ve Apache Hadoop kümelerini izleme için kullanılır. Bu, kümelerin çalışmasını kolaylaştırarak, işleç araçlarının sezgisel bir koleksiyonunu ve Hadoop’un karmaşıklığı gizleyen sağlam bir API kümesini içerir. API'ler hakkında daha fazla bilgi için bkz: [Ambari API Başvurusu][ambari-api-reference]. 

Hdınsight şu anda yalnızca Ambari izleme özelliğini destekler. Ambari API 1.0 Hdınsight sürüm 3.0 ve 2.1 kümeleri tarafından desteklenir. Bu makalede, Hdınsight sürüm 3.1 ve 2.1 kümelerinde erişilirken Ambari API yer almaktadır. İkisi arasındaki temel fark, bazı bileşenleri (örneğin, iş geçmişi sunucusu) yeni özellikler girişiyle değişmiş ' dir. 

**Önkoşullar**

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Azure PowerShell içeren bir iş istasyonu**.
* (İsteğe bağlı) [cURL][curl]. Yüklemek için bkz: [sürümleri ve indirmeleri cURL][curl-download].
  
  > [!NOTE]
  > Windows'da çift tırnak işaretleri kullan seçeneği değerleri için tek tırnak işareti yerine ne zaman cURL komutunu kullanın.
  > 
  > 
* **Azure Hdınsight kümesi**. Küme hazırlama hakkında yönergeler için bkz: [Hdınsight kullanmaya başlama] [ hdinsight-get-started] veya [Hdınsight kümeleri hazırlama][hdinsight-provision]. Öğreticiyi incelemek için aşağıdaki veriler gerekir:
  
  | Küme özelliği | Azure PowerShell değişken adı | Değer | Açıklama |
  | --- | --- | --- | --- |
  |   Hdınsight küme adı |$clusterName | |Hdınsight kümenizin adıdır. |
  |   Küme kullanıcı |$clusterUsername | |Küme oluşturulduğu belirtilen küme kullanıcı adı. |
  |   Küme parolası |$clusterPassword | |Küme kullanıcı parolası. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Hemen başlayın
Hdınsight kümeleri izlemek için Ambari kullanmak için birkaç yolu vardır.

**Azure PowerShell’i kullanma**

MapReduce işi İzleyicisi bilgileri aşağıdaki Azure PowerShell betiğini alır *bir Hdınsight 3.5 kümesindeki.*  Biz bu ayrıntıları YARN hizmet (yerine MapReduce) çekme anahtar farktır.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Aşağıdaki PowerShell betiğini MapReduce işi İzleyicisi bilgileri alır *bir Hdınsight 2.1 kümesindeki*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Çıktısı şöyledir:

![Jobtracker'a çıkış][img-jobtracker-output]

**cURL kullanma**

Aşağıdaki örnek, cURL kullanarak küme bilgilerini alır:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Çıktısı şöyledir:

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

Ambari uç nokta kullanılırken "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *host_name* alan ana bilgisayar adı yerine düğümün tam etki alanı adını (FQDN) döndürür. 8/10/2014 yayınlanmadan önce bu örnek döndürülen yalnızca "**headnode0**". 8/10/2014 yayınlanmasından sonra FQDN Al "**headnode0. { ClusterDNS} .azurehdinsight .net**", bir önceki örnekte gösterildiği gibi. Bu değişiklik, birden çok küme türleri (örneğin, HBase ve Hadoop) bir sanal ağ (VNET) burada dağıtılabilir senaryolarını kolaylaştırmak için gerekiyordu. Bu, örneğin, Hadoop için bir arka uç platform olarak HBase kullanarak olur.

## <a name="ambari-monitoring-apis"></a>Ambari API izleme
Aşağıdaki tabloda bazı yaygın Ambari API çağrıları izleme yer almaktadır. API'si hakkında daha fazla bilgi için bkz: [Ambari API Başvurusu][ambari-api-reference].

| İzleyici API çağrısı | URI | Açıklama |
| --- | --- | --- |
| Kümeleri Al |`/api/v1/clusters` | |
| Küme bilgilerini al. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |kümeler, hizmetleri, ana bilgisayarlar |
| Hizmetlerini Al |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Hizmetleri içerir: hdfs, mapreduce |
| Hizmetleri bilgi alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Hizmet bileşenlerini Al |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: iş, datanodeMapReduce: jobtracker'a; tasktracker |
| Bileşen bilgi alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo, ana bilgisayar bileşenleri, ölçümleri |
| Konaklar Al |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0, workernode0 |
| Ana bilgisayar bilgilerini al. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Ana bilgisayar bileşenlerini Al |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |İş, resourcemanager |
| Ana bileşeni bilgi alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles, bileşen, ana bilgisayar, ölçümleri |
| Yapılandırmalarını alma |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Yapılandırma türleri: çekirdek site, site hdfs, mapred site, hive site |
| Yapılandırma bilgilerini alın. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Yapılandırma türleri: çekirdek site, site hdfs, mapred site, hive site |

## <a name="next-steps"></a>Sonraki Adımlar
Şimdi Ambari API çağrıları izleme kullanmayı öğrendiniz. Daha fazla bilgi için bkz:

* [Azure portalını kullanarak Hdınsight kümelerini yönetme][hdinsight-admin-portal]
* [Azure PowerShell kullanarak Hdınsight kümelerini yönetme][hdinsight-admin-powershell]
* [Komut satırı arabirimi kullanarak Hdınsight kümelerini yönetme][hdinsight-admin-cli]
* [Hdınsight belgeleri][hdinsight-documentation]
* [Hdınsight kullanmaya başlama][hdinsight-get-started]

[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
