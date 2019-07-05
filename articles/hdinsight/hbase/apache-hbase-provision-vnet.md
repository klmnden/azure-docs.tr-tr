---
title: Sanal ağ içinde - Azure HBase kümeleri oluşturma
description: Azure HDInsight içinde HBase kullanmaya başlayın. HDInsight HBase kümeleri Azure sanal ağ oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: 9d81e5e69837f6074d94278f4e54f9178a656335
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67433783"
---
# <a name="create-apache-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Azure sanal ağdaki HDInsight üzerinde Apache HBase kümeleri oluşturma
Azure HDInsight Apache HBase kümeleri oluşturmayı öğrenin bir [Azure sanal ağı][1].

Uygulamalar HBase ile doğrudan iletişim kurabilmesi için sanal ağ ile tümleştirme, uygulamalarınızı aynı sanal ağ için Apache HBase kümelerine dağıtılabilir. Avantajları şunlardır:

* Doğrudan bağlantı HBase Java uzak yordam üzerinden iletişim sağlayan HBase küme düğümlerine web uygulamasının (RPC) API'larını çağırma.
* Geliştirilmiş performans trafiğiniz zorunluluğunu ortadan kaldırarak birden çok ağ geçitleri ve yük dengeleyicileri üzerinden gider.
* Genel bir uç nokta sokmadan daha güvenli bir şekilde hassas bilgi işleme yeteneği.

### <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure PowerShell içeren bir iş istasyonu**. Bkz: [yükleme ve Azure PowerShell kullanma](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-apache-hbase-cluster-into-virtual-network"></a>Sanal ağına Apache HBase kümesi oluşturma
Bu bölümde, bir Azure sanal ağı kullanarak bağımlı Azure depolama hesabı ile bir Linux tabanlı Apache HBase kümesi oluşturma bir [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma yöntemleri ve ayarları hakkında bilgi edinmek bkz [oluşturma HDInsight kümeleri](../hdinsight-hadoop-provision-linux-clusters.md). HDInsight Apache Hadoop kümelerini oluşturmak için bir şablon kullanma hakkında daha fazla bilgi için bkz. [Apache Hadoop kümeleri oluşturma Azure Resource Manager şablonlarını kullanarak HDInsight](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)

> [!NOTE]  
> Bazı özellikler, şablona sabit kodlanmış. Örneğin:
>
> * **Konum**: Doğu ABD 2
> * **Küme sürümü**: 3.6
> * **Çalışan düğümü sayısı küme**: 2
> * **Varsayılan depolama hesabı**: benzersiz bir dize
> * **Sanal ağ adı**: &lt;Küme adı >-vnet
> * **Sanal ağ adres alanı**: 10.0.0.0/16
> * **Alt ağ adı**: subnet1
> * **Alt ağ adres aralığı**: 10.0.0.0/24
>
> &lt;Küme adı > şablon kullanırken sağladığınız küme adı ile değiştirilir.
>
>

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın. Şablonuna [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Gelen **özel dağıtım** dikey penceresinde, aşağıdaki özellikleri girin:

   * **Abonelik**: HDInsight küme, bağımlı depolama hesabı ve Azure sanal ağı oluşturmak için kullanılan bir Azure aboneliği seçin.
   * **Kaynak grubu**: Seçin **Yeni Oluştur**, yeni bir kaynak grubu adı belirtin.
   * **Konum**: Kaynak grubu için bir konum seçin.
   * **ClusterName**: Oluşturulacak Hadoop kümesi için bir ad girin.
   * **Küme oturum açma adı ve parola**: Varsayılan oturum açma adı **admin**’dir.
   * **SSH kullanıcı adı ve parola**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz.
   * **Hüküm ve yukarıdaki koşulları kabul ediyorum**: (Seç)
3. **Satın al**’a tıklayın. Bir küme oluşturmak yaklaşık 20 dakika sürer. Küme oluşturulduktan sonra küme dikey penceresini açmak için portalda tıklayabilirsiniz.

Makaleyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Küme silme yönergeleri için bkz: [yönetme Apache Hadoop, Azure portalını kullanarak HDInsight kümeleri](../hdinsight-administer-use-portal-linux.md#delete-clusters).

Yeni bir HBase kümesi ile çalışmaya başlamak için bulunan yordamları kullanabilirsiniz [HDInsight, Apache Hadoop ile Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md).

## <a name="connect-to-the-apache-hbase-cluster-using-apache-hbase-java-rpc-apis"></a>Apache HBase Java RPC API'lerini kullanarak Apache HBase kümeye bağlanma
1. Bir altyapı (ıaas) sanal makine olarak aynı Azure sanal ağı ve aynı alt ağda oluşturun. Yeni bir Iaas sanal makine oluşturma ile ilgili yönergeler için bkz: [sanal makine çalıştıran Windows Server oluşturma](../../virtual-machines/windows/quick-create-portal.md). Bu belgede yer alan adımları uygulayarak, ağ yapılandırması için aşağıdaki değerleri kullanmanız gerekir:

   * **Sanal ağ**: &lt;Küme adı >-vnet
   * **Alt ağ**: subnet1

   > [!IMPORTANT]  
   > Değiştirin &lt;küme adı > önceki adımlarda HDInsight kümesi oluştururken kullandığınız ada sahip.

   Bu değerleri kullanarak, sanal makineyi aynı sanal ağ ve alt ağ HDInsight kümesi olarak yerleştirilir. Bu yapılandırma, bunları doğrudan birbirleri ile iletişim kurmasına olanak sağlar. Bir boş kenar düğümü bir HDInsight kümesi oluşturmak için bir yol yoktur. Kenar düğümüne kümeyi yönetmek için kullanılabilir.  Daha fazla bilgi için [HDInsight içinde boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md).

2. HBase için uzaktan bağlanmak için bir Java uygulaması kullanarak, tam etki alanı adı (FQDN) kullanmanız gerekir. Bunu belirlemek için HBase kümesi bağlantıya özgü DNS son eki almanız gerekir. Bunu yapmak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

   * Bir Web tarayıcısı yapma bir [Apache Ambari](https://ambari.apache.org/) çağırın:

     Https:// Gözat&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / barındıran? minimal_response = true. Bu, DNS son eklerini içeren bir JSON dosyası kapatır.
   * Ambari Web sitesi kullanın:

     1. Https:// Gözat&lt;ClusterName >. azurehdinsight.net.
     2. Tıklayın **konakları** üstteki menüden.
   * REST çağrıları yapmak için Curl kullanın:

     ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
     ```

     Döndürülen JavaScript nesne gösterimi (JSON) veriler "host_name" girdisini bulun. Bu, kümedeki düğümler için FQDN'yi içerir. Örneğin:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     Küme adı ile başlayan etki alanı adı DNS son eki bölümüdür. Örneğin, mycluster.b1.cloudapp.net.
   * Azure PowerShell kullanma

     Kaydetmek için aşağıdaki Azure PowerShell betiğini kullanın **Get-ClusterDetail** DNS son eki döndürmek için kullanılan işlev:

     ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
            .Description
            This command shows the following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run the HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows the FQDN suffix of hosts in the cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows the value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run the HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows the FQDN suffix of hosts in the cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
     ```

     Azure PowerShell Betiği çalıştırdıktan sonra DNS son eki kullanarak döndürmek için aşağıdaki komutu kullanın **Get-ClusterDetail** işlevi. Bu komutu kullanırken, kendi HDInsight HBase küme adı, yönetici adı ve yönetici parolasını belirtin.

     ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
     ```

     Bu komut, DNS son eki döndürür. Örneğin, **yourclustername.b4.internal.cloudapp.net**.


<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix](./media/apache-hbase-provision-vnet/PrimaryDNSSuffix.png)
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Sanal makine HBase kümesi ile iletişim kurabildiğini doğrulamak için komutu kullanın. `ping headnode0.<dns suffix>` sanal makineden. Örneğin, headnode0.mycluster.b1.cloudapp.net ping atın.

Bu bilgiler bir Java uygulamasında kullanmak için adımları izleyebilirsiniz [HDInsight (Hadoop) ile Apache HBase kullanan Java uygulamaları oluşturmak için Apache Maven](./apache-hbase-build-java-maven-linux.md) bir uygulama oluşturmak için. Uygulama uzak bir HBase sunucusuna bağlanmak için değişiklik **hbase-site.xml** Zookeeper için bir FQDN kullanmak için bu örnek dosyasında. Örneğin:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]  
> Kendi DNS sunucunuzu kullanmak nasıl dahil olmak üzere sanal ağlar, azure'da ad çözümlemesi hakkında daha fazla bilgi için bkz [adı çözümleme (DNS)](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Apache HBase kümesi oluşturmayı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight ile çalışmaya başlama](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight içinde boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md)
* [HDInsight Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
* [HDInsight Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight, Apache Hadoop ile Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [Sanal Ağ’a Genel Bakış](../../virtual-network/virtual-networks-overview.md)

[1]: https://azure.microsoft.com/services/virtual-network/
[2]: https://technet.microsoft.com/library/ee176961.aspx
[3]: https://technet.microsoft.com/library/hh847889.aspx

