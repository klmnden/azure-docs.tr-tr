---
title: Sanal bir ağa - Azure HBase kümeleri oluşturma | Microsoft Docs
description: Azure Hdınsight'ta HBase kullanarak başlayın. Hdınsight HBase kümeleri Azure sanal ağ oluşturmayı öğrenin.
keywords: ''
services: hdinsight,virtual-network
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: edcfa47eee0f085bad415be0d9b112bbc33c3eca
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31521614"
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Azure sanal ağındaki hdınsight'ta HBase kümeleri oluşturma
Azure Hdınsight HBase kümelerini oluşturmayı öğrenin bir [Azure Virtual Network][1].

Uygulamalar HBase ile doğrudan iletişim kurabilmesi için sanal ağ tümleştirmesinin ile uygulamalarınızı aynı sanal ağ için HBase kümelerine dağıtılabilir. Avantajlara şunlar dahildir:

* Doğrudan bağlantı HBase Java uzaktan yordam üzerinden iletişimi sağlayan HBase küme düğümleri için web uygulamasının (RPC) API'larını çağırma.
* Geliştirilmiş performans trafiğinizi zorunluluğunu ortadan kaldırarak birden çok ağ geçitleri ve yük dengeleyicileri üzerine gidin.
* Genel bir uç nokta sokmadan hassas bilgileri daha güvenli bir şekilde işleme yeteneği.

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure PowerShell içeren bir iş istasyonu**. Bkz: [yükleme ve kullanma Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-hbase-cluster-into-virtual-network"></a>Sanal ağda HBase kümesi oluşturma
Bu bölümde, bir Azure sanal ağı kullanarak bağımlı Azure depolama hesabı ile Linux tabanlı HBase kümesi oluşturma bir [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma yöntemleri ve ayarlarını anlama, bkz: [Hdınsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md). Hdınsight'ta Hadoop kümeleri oluşturmak için bir şablon kullanma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları kullanarak Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)

> [!NOTE]
> Bazı özellikler şablonuna sabit kodlanmış. Örneğin:
>
> * **Konum**: Doğu ABD 2
> * **Küme sürümü**: 3.6
> * **Çalışan düğüm sayısı küme**: 2
> * **Varsayılan depolama hesabı**: benzersiz bir dize
> * **Sanal ağ adı**: &lt;küme adı >-vnet
> * **Sanal ağ adres alanı**: 10.0.0.0/16
> * **Alt ağ adı**: subnet1
> * **Alt ağ adres aralığı**: 10.0.0.0/24
>
> &lt;Küme adı > şablon kullanırken sağladığınız küme adı ile değiştirilir.
>
>

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın. Şablon bulunan [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Gelen **özel dağıtım** dikey penceresinde, aşağıdaki özellikleri girin:

   * **Abonelik**: Hdınsight küme, bağımlı depolama hesabı ve Azure sanal ağı oluşturmak için kullanılan Azure aboneliğini seçin.
   * **Kaynak grubu**: seçin **Yeni Oluştur**ve yeni bir kaynak grubu adı belirtin.
   * **Konum**: Kaynak grubu için bir konum seçin.
   * **ClusterName**: Oluşturulacak Hadoop kümesi için bir ad girin.
   * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı **admin** şeklindedir.
   * **SSH kullanıcı adı ve parolası**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz.
   * **Hüküm ve koşullar yukarıda belirtildiği ediyorum**: (seçin)
3. **Satın al**’a tıklayın. Bir küme oluşturmak yaklaşık 20 dakika sürer. Küme oluşturulduktan sonra küme dikey penceresini açmak için portalda tıklatabilirsiniz.

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Küme silme ilişkin yönergeler için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure portalını kullanarak](../hdinsight-administer-use-management-portal.md#delete-clusters).

Yeni HBase kümesi ile çalışmaya başlamak için bulunan yordamları kullanabilirsiniz [hdınsight'ta Hadoop ile HBase kullanmaya başlamanıza](./apache-hbase-tutorial-get-started-linux.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>HBase Java RPC API'lerini kullanarak HBase kümeye bağlanın
1. Bir altyapı (ıaas) sanal makine olarak aynı Azure sanal ağı ve aynı alt ağ olarak oluşturun. Yeni bir Iaas sanal makine oluşturma ile ilgili yönergeler için bkz: [bir sanal makine çalıştıran Windows Server oluşturma](../../virtual-machines/windows/quick-create-portal.md). Bu belgedeki adımları izleyerek, ağ yapılandırması için aşağıdaki değerleri kullanmanız gerekir:

   * **Sanal ağ**: &lt;küme adı >-vnet
   * **Alt ağ**: subnet1

   > [!IMPORTANT]
   > Değiştir &lt;küme adı > önceki adımlarda Hdınsight kümesi oluştururken, kullandığınız ada sahip.
   >
   >

   Bu değerleri kullanarak, sanal makineyi aynı sanal ağ ve alt Hdınsight kümesi olarak yerleştirilir. Bu yapılandırma doğrudan birbirleri ile iletişim kurmasına olanak sağlar. Boş kenar düğümüne ile Hdınsight kümesi oluşturmak için bir yol yoktur. Kenar düğümüne kümeyi yönetmek için kullanılabilir.  Daha fazla bilgi için bkz: [Hdınsight'ta boş kenar düğümünü kullanmak](../hdinsight-apps-use-edge-node.md).

2. Bir Java uygulaması HBase için uzaktan bağlanmak için kullanıldığında, tam etki alanı adı (FQDN) kullanmanız gerekir. Bunu belirlemek için HBase kümesi bağlantıya özgü DNS son ekini edinmeniz gerekir. Bunu yapmak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

   * Ambari arama yapmak için bir Web tarayıcısı kullanın:

     Https:// için Gözat&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / barındıran? minimal_response = true. Bir JSON dosyası DNS sonekleri etkinleştirir.
   * Ambari Web sitesi kullanın:

     1. Https:// için Gözat&lt;ClusterName >. azurehdinsight.net.
     2. Tıklatın **ana** üstteki menüden.
   * REST çağrı yapmak için Curl kullanın:

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     Döndürülen JavaScript nesne gösterimi (JSON) verileri "host_name" girdiyi bulun. Kümedeki düğümler için FQDN'yi içerir. Örneğin:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     Küme adı ile başlayan etki alanı adı DNS soneki bölümüdür. Örneğin, mycluster.b1.cloudapp.net.
   * Azure PowerShell kullanma

     Kaydetmek için aşağıdaki Azure PowerShell betiğini kullanın **Get-ClusterDetail** DNS soneki döndürmek için kullanılan işlev:

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

     Azure PowerShell Betiği çalıştırdıktan sonra DNS son ekini kullanarak döndürmek için aşağıdaki komutu kullanın **Get-ClusterDetail** işlevi. Bu komutu kullanırken Hdınsight HBase küme adı, yönetici adı ve yönetici parolasını belirtin.

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

Sanal makine HBase kümesi ile iletişim kurabildiğinden emin doğrulamak için komutunu kullanın `ping headnode0.<dns suffix>` sanal makineden. Örneğin, ping headnode0.mycluster.b1.cloudapp.net.

Bu bilgiler bir Java uygulamasında kullanmak için adımları izleyebilirsiniz [Hdınsight (Hadoop) ile HBase kullanan Java uygulamaları oluşturmak için Maven kullanmak](./apache-hbase-build-java-maven-linux.md) bir uygulama oluşturmak için. Uzak bir HBase sunucuya uygulama sağlamak için değiştirmek **hbase-site.xml** dosyası bu örnekte FQDN için Zookeeper kullanın. Örneğin:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> Azure ad çözümlemesi hakkında daha fazla bilgi için sanal ağlar kendi DNS sunucusunu kullanacak şekilde nasıl dahil olmak üzere, bkz: [adı çözümleme (DNS)](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir HBase kümesi oluşturmayı öğrendiniz. Daha fazla bilgi için bkz:

* [Hdınsight kullanmaya başlama](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Hdınsight'ta boş kenar düğümleri kullanın](../hdinsight-apps-use-edge-node.md)
* [HDInsight’ta HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight'ta Hadoop ile HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [Sanal Ağ’a Genel Bakış](../../virtual-network/virtual-networks-overview.md)

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

