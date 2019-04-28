---
title: Kullanarak sanal ağları - Azure HDInsight Kafka'ya bağlanma
description: Bir Azure sanal ağ üzerinden HDInsight üzerinde Kafka doğrudan bağlanma hakkında bilgi edinin. Kafka için bir VPN ağ geçidi kullanarak geliştirme istemcilerinden ya da şirket içi ağınızda istemcilerden bir VPN ağ geçidi cihazı kullanarak bağlanmayı öğreneceksiniz.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 93b5aeafafdc6ab7ee233f6360bb5e09f45b387f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62115364"
---
# <a name="connect-to-apache-kafka-on-hdinsight-through-an-azure-virtual-network"></a>Bir Azure sanal ağı üzerinden HDInsight üzerinde Apache kafka'ya bağlanma

Bir Azure sanal ağ üzerinden HDInsight üzerinde Apache Kafka doğrudan bağlanma hakkında bilgi edinin. Bu belge Kafka'ya bağlanmaya ilişkin aşağıdaki yapılandırmaları kullanma bilgi sağlar:

* Bir şirket içi ağdaki kaynakları. Yerel ağınızdaki VPN cihazı (yazılım veya donanım) kullanarak bu bağlantı kurulur.
* Bir VPN yazılım istemcisini kullanarak bir geliştirme ortamından.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="architecture-and-planning"></a>Mimari ve planlama

HDInsight Kafka için doğrudan bağlantı genel internet üzerinden izin vermez. Bunun yerine, Kafka istemciler (üreticilerin ve tüketicilerin) aşağıdaki bağlantı yöntemlerden birini kullanmanız gerekir:

* HDInsight üzerinde Kafka ile aynı sanal ağdaki istemci çalıştırın. Bu yapılandırma kullanılan [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belge. İstemci, doğrudan HDInsight küme düğümlerinde veya aynı ağdaki başka bir sanal makinede çalıştırır.

* Şirket içi ağınız gibi özel bir ağ sanal ağa bağlayın. Bu yapılandırma, istemcilerin doğrudan Kafka ile çalışmak için şirket içi ağınızda sağlar. Bu yapılandırmayı etkinleştirmek için aşağıdaki görevleri gerçekleştirin:

  1. Sanal ağ oluşturun.
  2. Bir siteden siteye yapılandırması kullanan bir VPN ağ geçidi oluşturun. Bu belgede kullanılan yapılandırma bir VPN ağ geçidi cihazı şirket içi ağınıza bağlanır.
  3. Bir DNS sunucusu, sanal ağ oluşturun.
  4. Her bir ağın DNS sunucusu arasında iletim yapılandırın.
  5. Sanal ağda HDInsight kümesinde Kafka oluşturmak.

     Daha fazla bilgi için [Apache Kafka Bağlan bir şirket içi ağdan](#on-premises) bölümü. 

* Tek tek makineler bir VPN ağ geçidi ile VPN istemcisini sanal ağa bağlanır. Bu yapılandırmayı etkinleştirmek için aşağıdaki görevleri gerçekleştirin:

  1. Sanal ağ oluşturun.
  2. Noktadan siteye yapılandırması kullanan bir VPN ağ geçidi oluşturun. Bu yapılandırma, hem Windows hem de MacOS istemcileri ile kullanılabilir.
  3. Sanal ağda HDInsight kümesinde Kafka oluşturmak.
  4. Kafka IP reklam için yapılandırın. Bu yapılandırma aracı kullanarak bağlanmak için istemcinin etki alanı adları yerine IP adreslerini sağlar.
  5. İndirin ve geliştirme sisteminde VPN istemcisini kullanır.

     Daha fazla bilgi için [bir VPN istemcisi ile Apache Kafka Bağlan](#vpnclient) bölümü.

     > [!WARNING]  
     > Bu yapılandırma, yalnızca geliştirme amacıyla aşağıdaki sınırlamalar nedeniyle önerilir:
     >
     > * Her bir istemci, yazılım VPN istemcisi kullanarak bağlanmanız gerekir.
     > * IP adresleme Kafka ile iletişim kurmak için kullanmalısınız VPN istemcisini sanal ağa, ad çözümleme isteklerinin geçmez. IP iletişimi için Kafka kümesinin ek yapılandırma gerektirir.

Bir sanal ağda HDInsight kullanma hakkında daha fazla bilgi için bkz. [Azure sanal ağlarını kullanarak HDInsight genişletmek](../hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a> Bir şirket içi ağdan Apache Kafka'ya bağlanma

Şirket içi ağınız ile iletişim kuran bir Kafka kümesi oluşturmak için adımları [HDInsight'ı şirket içi ağınıza bağlama](./../connect-on-premises-network.md) belge.

> [!IMPORTANT]  
> HDInsight kümesi oluştururken, seçin __Kafka__ küme türü.

Bu adımlar aşağıdaki yapılandırmayı oluşturun:

* Azure Sanal Ağ
* Siteden siteye VPN ağ geçidi
* Azure depolama hesabı (HDInsight tarafından kullanılır)
* HDInsight üzerinde Kafka

Kafka istemci şirket içinden kümeye bağlantı kurabildiğimizi doğrulamak için içindeki adımları kullanın. [örneği: Python istemcisini](#python-client) bölümü.

## <a id="vpnclient"></a> Bir VPN istemcisi ile Apache Kafka için bağlama

Aşağıdaki yapılandırmayı oluşturmak için bu bölümdeki adımları kullanın:

* Azure Sanal Ağ
* Noktadan siteye VPN ağ geçidi
* Azure depolama hesabı'nı (HDInsight tarafından kullanılır)
* HDInsight üzerinde Kafka

1. Bağlantısındaki [noktadan siteye bağlantılar için otomatik olarak imzalanan sertifikalarla çalışma](../../vpn-gateway/vpn-gateway-certificates-point-to-site.md) belge. Bu belge, ağ geçidi için gerekli sertifikaları oluşturur.

2. Bir PowerShell istemi açın ve Azure aboneliğinizde oturum açmak için aşağıdaki kodu kullanın:

    ```powershell
    Connect-AzAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzSubscription -SubscriptionName "name of your subscription"
    ```

3. Yapılandırma bilgilerini içeren değişkenlerini oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is the resource group name?"
    $baseName = Read-Host "What is the base name? It is used to create names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want to create the resources in?"
    $rootCert = Read-Host "What is the file path to the root certificate? It is used to secure the VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter the HTTPS user name and password for the HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter the SSH user name and password for the HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.6"
    $hdiType = "Kafka"
    ```

4. Sanal ağ ve Azure kaynak grubu oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the resource group that contains everything
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
    New-AzVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]  
    > Bu, bu işlemin tamamlanması birkaç dakika sürebilir.

5. Azure depolama hesabı ve blob kapsayıcısı oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the storage account
    New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. HDInsight kümesi oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the HDInsight cluster
    New-AzHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -DisksPerWorkerNode 2 `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

   > [!WARNING]  
   > Bu işlemin tamamlanması yaklaşık 15 dakika sürer.

### <a name="configure-kafka-for-ip-advertising"></a>Kafka IP reklam için yapılandırma

Varsayılan olarak, Apache Zookeeper istemcilere Kafka aracılarına etki alanı adını döndürür. Bu yapılandırma, sanal ağdaki varlıklar için ad çözümlemesi kullanılamaz olarak VPN yazılım istemcisi ile çalışmaz. Bu yapılandırma için Kafka, etki alanı adları yerine IP adreslerini tanıtacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısı kullanarak Git https://CLUSTERNAME.azurehdinsight.net. Değiştirin __CLUSTERNAME__ HDInsight kümesinde Kafka adı.

    İstendiğinde, küme için HTTPS kullanıcı adını ve parolayı kullanın. Küme için Ambari Web kullanıcı Arabirimi görüntülenir.

2. Kafka hakkında bilgi görüntülemek için seçin __Kafka__ sol taraftaki listeden.

    ![Hizmet listesi Kafka ile vurgulanmış](./media/apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için seçin __yapılandırmaları__ üst ortasından.

    ![Kafka için yapılandırmaları bağlantıları](./media/apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. Bulunacak __kafka env__ yapılandırması girin `kafka-env` içinde __filtre__ alanında sağ üst köşede bulunan.

    ![Kafka env için Kafka yapılandırması](./media/apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. Kafka IP adresleri tanıtacak şekilde yapılandırmak için alt kısmına aşağıdaki metni ekleyin __kafka env şablon__ alan:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka dinlediği arabirimi yapılandırmak için girin `listeners` içinde __filtre__ sağ üst alan.

7. Kafka, tüm ağ arabirimleri üzerinde dinlemek üzere yapılandırmak için değerini değiştirmek __dinleyicileri__ alanı `PLAINTEXT://0.0.0.0:9092`.

8. Yapılandırma değişikliklerini kaydetmek için kullanın __Kaydet__ düğmesi. Değişiklikleri açıklayan bir mesaj girin. Seçin __Tamam__ değişiklikleri kaydettikten sonra.

    ![Kaydet düğmesi yapılandırma](./media/apache-kafka-connect-vpn-gateway/save-button.png)

9. Kafka yeniden başlatılırken hataları önlemek için __hizmet eylemleri__ düğmesini tıklatın ve seçin __bakım modunu aç__. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Hizmet eylemlerle vurgulanmış bakım etkinleştirin](./media/apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. Kafka yeniden başlatmak için kullanın __yeniden__ düğmesini tıklatın ve seçin __yeniden tüm etkilenen__. Yeniden başlatma işlemini onaylayın ve ardından __Tamam__ işlemi tamamlandıktan sonra düğme.

    ![Etkilenen yeniden başlatma ile tüm düğmesi vurgulanmış yeniden başlatın](./media/apache-kafka-connect-vpn-gateway/restart-button.png)

11. Bakım modunu devre dışı bırakmak için __hizmet eylemleri__ düğmesini tıklatın ve seçin __bakım modunu Kapat Kapat__. Seçin **Tamam** bu işlemi tamamlamak için.

### <a name="connect-to-the-vpn-gateway"></a>VPN ağ geçidine bağlanma

VPN ağ geçidine bağlanmak için __Azure'a bağlanma__ bölümünü [noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#connect) belge.

## <a id="python-client"></a> Örnek: Python istemcisi

Kafka bağlanabilirliği doğrulamak için oluşturup bir Python üretici ve tüketici çalıştırmak için aşağıdaki adımları kullanın:

1. Kafka kümesindeki düğümlere IP adreslerini ve tam etki alanı adı (FQDN) almak için aşağıdaki yöntemlerden birini kullanın:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Bu betik varsayar `$resourceGroupName` sanal ağı içeren Azure kaynak grubunun adıdır.

    Sonraki adımlarda kullanmak için döndürülen bilgi kaydedin.

2. Yüklemek için aşağıdakileri kullanın [kafka python](https://kafka-python.readthedocs.io/) istemci:

        pip install kafka-python

3. Kafka için veri göndermek için aşağıdaki Python kodu kullanın:

   ```python
   from kafka import KafkaProducer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # NOTE: you don't need the full list of worker nodes, just one or two.
   producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
   for _ in range(50):
      producer.send('testtopic', b'test message')
   ```

    Değiştirin `'kafka_broker'` döndürüldüğü adresleriyle girişleri bu bölümdeki 1. adım:

   * Kullanıyorsanız bir __yazılım VPN istemcisi__, değiştirin `kafka_broker` girişleri, çalışan düğümlerinin IP adresine sahip.

   * Varsa __ad çözümlemesi ile özel bir DNS sunucusu etkin__, değiştirin `kafka_broker` girişleri ile çalışan düğümü FQDN'si.

     > [!NOTE]
     > Bu kod, dize gönderir `test message` konuya `testtopic`. HDInsight üzerinde Kafka'nın varsayılan yapılandırma, mevcut değilse konu oluşturmaktır.

4. Kafka'dan iletileri almak için aşağıdaki Python kodu kullanın:

   ```python
   from kafka import KafkaConsumer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # Again, you only need one or two, not the full list.
   # Note: auto_offset_reset='earliest' resets the starting offset to the beginning
   #       of the topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    Değiştirin `'kafka_broker'` döndürüldüğü adresleriyle girişleri bu bölümdeki 1. adım:

    * Kullanıyorsanız bir __yazılım VPN istemcisi__, değiştirin `kafka_broker` girişleri, çalışan düğümlerinin IP adresine sahip.

    * Varsa __ad çözümlemesi ile özel bir DNS sunucusu etkin__, değiştirin `kafka_broker` girişleri ile çalışan düğümü FQDN'si.

## <a name="next-steps"></a>Sonraki adımlar

Bir sanal ağda HDInsight kullanma hakkında daha fazla bilgi için bkz. [Azure HDInsight kullanarak bir Azure sanal ağ genişletme](../hdinsight-extend-hadoop-virtual-network.md) belge.

Noktadan siteye VPN ağ geçidi ile bir Azure sanal ağ oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure portalını kullanarak noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Azure PowerShell kullanarak noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

HDInsight üzerinde Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](apache-kafka-get-started.md)
* [HDInsight üzerinde Apache Kafka ile yansıtma kullanın](apache-kafka-mirroring.md)
