---
title: "Sanal ağları - Azure Hdınsight kullanarak Kafka bağlanma | Microsoft Docs"
description: "Bir Azure sanal ağı üzerinden hdınsight'ta Kafka doğrudan bağlanmak öğrenin. Kafka için bir VPN ağ geçidi kullanarak geliştirme istemcileri veya istemciler, şirket içi ağınıza bir VPN ağ geçidi aygıtı kullanarak nasıl bağlayacağınızı öğrenin."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.custom: hdinsightactive
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: 2b55de4de6bb94be78649112161211346090b23a
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="connect-to-kafka-on-hdinsight-through-an-azure-virtual-network"></a>Bir Azure sanal ağı üzerinden hdınsight'ta Kafka bağlanma

Bir Azure sanal ağı üzerinden hdınsight'ta Kafka doğrudan bağlanmak öğrenin. Bu belge için Kafka bağlanma aşağıdaki yapılandırmaları kullanma bilgileri verilmiştir:

* Bir şirket ağında kaynaklardan. Bu bağlantı, yerel ağınızda bir VPN cihazı (yazılım veya donanım) kullanılarak oluşturulur.
* Geliştirme ortamından bir VPN yazılımı istemcisi kullanarak.

## <a name="architecture-and-planning"></a>Mimari ve planlama

Hdınsight genel internet üzerinden doğrudan bağlantı Kafka izin vermiyor. Bunun yerine, Kafka istemcileri (üreticileri ve tüketicileri) aşağıdaki bağlantı yöntemlerden birini kullanmanız gerekir:

* İstemciyi aynı sanal ağda Kafka olarak Hdınsight üzerinde çalıştırın. Bu yapılandırma kullanılan [Hdınsight üzerinde Apache Kafka başlayarak](apache-kafka-get-started.md) belge. İstemci Hdınsight küme düğümlerinde doğrudan veya aynı ağdaki başka bir sanal makine üzerinde çalışır.

* Şirket içi ağınız gibi özel ağ sanal ağa bağlayın. Bu yapılandırma, Kafka ile doğrudan çalışmak için şirket içi ağınızdaki istemcilerin sağlar. Bu yapılandırmayı etkinleştirmek için aşağıdaki görevleri gerçekleştirin:

    1. Sanal ağ oluşturun.
    2. Siteden siteye yapılandırmasını kullanan bir VPN ağ geçidi oluşturun. Bu belgede kullanılan yapılandırma bir VPN ağ geçidi aygıtı, şirket içi ağınıza bağlanır.
    3. Bir DNS sunucusu sanal ağ oluşturun.
    4. Her ağ DNS sunucusu arasında iletim yapılandırın.
    5. Kafka Hdınsight'ta sanal ağınıza yükleyin.

    Daha fazla bilgi için bkz: [Kafka Bağlan bir şirket ağından](#on-premises) bölümü. 

* Makineleri tek tek bir VPN ağ geçidi ve VPN istemcisi kullanarak sanal ağa bağlayın. Bu yapılandırmayı etkinleştirmek için aşağıdaki görevleri gerçekleştirin:

    1. Sanal ağ oluşturun.
    2. Bir noktadan siteye yapılandırması kullanan bir VPN ağ geçidi oluşturun. Bu yapılandırma Windows istemcilerinde yüklü bir VPN istemcisi sağlar.
    3. Kafka Hdınsight'ta sanal ağınıza yükleyin.
    4. Kafka IP reklam için yapılandırın. Bu yapılandırma istemcinin etki alanı adı yerine IP adresleme kullanarak bağlan olanak tanır.
    5. Karşıdan yükle ve geliştirme sisteminde VPN İstemcisi'ni kullanın.

    Daha fazla bilgi için bkz: [Kafka VPN istemcisi ile Bağlan](#vpnclient) bölümü.

    > [!WARNING]
    > Bu yapılandırma, yalnızca Geliştirme amaçlı aşağıdaki sınırlamalar nedeniyle önerilir:
    >
    > * Her bir istemci bir VPN yazılımı istemcisi kullanarak bağlanmanız gerekir. Azure yalnızca Windows tabanlı bir istemci sağlar.
    > * IP adresi Kafka ile iletişim kurmak için kullanmalısınız VPN istemcisi sanal ağa ad çözümleme isteklerinin geçmez. IP iletişimi Kafka küme üzerinde ek yapılandırma gerektirir.

Sanal bir ağa Hdınsight kullanma hakkında daha fazla bilgi için bkz: [genişletmek Azure sanal ağları kullanarak Hdınsight](../hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a>Kafka için bir şirket içi ağ üzerinden bağlanma

Şirket içi ağınız ile iletişim kuran Kafka küme oluşturmak için adımları [şirket içi ağınıza bağlanmak Hdınsight](./../connect-on-premises-network.md) belge.

> [!IMPORTANT]
> Hdınsight kümesi oluştururken seçin __Kafka__ küme türü.

Aşağıdaki yapılandırma adımları oluşturun:

* Azure Sanal Ağ
* Siteden siteye VPN ağ geçidi
* Azure depolama hesabı (Hdınsight tarafından kullanılır)
* HDInsight üzerinde Kafka

Şirket içi Kafka istemci kümeye bağlanabildiğini doğrulamak için içindeki adımları kullanın [örnek: Python istemci](#python-client) bölümü.

## <a id="vpnclient"></a>Bir VPN istemcisi için Kafka Bağlan

Aşağıdaki yapılandırma oluşturmak için bu bölümdeki adımları kullanın:

* Azure Sanal Ağ
* Noktadan siteye VPN ağ geçidi
* Azure depolama hesabı (Hdınsight tarafından kullanılır)
* HDInsight üzerinde Kafka

1. Adımları [noktadan siteye bağlantıları için otomatik olarak imzalanan sertifikalar ile çalışma](../../vpn-gateway/vpn-gateway-certificates-point-to-site.md) belge. Bu belge ağ geçidi için gerekli sertifikaları oluşturur.

2. PowerShell komut istemini açın ve Azure aboneliğinizde oturum açmak için aşağıdaki kodu kullanın:

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. Yapılandırma bilgilerini içeren değişkenler oluşturmak için aşağıdaki kodu kullanın:

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
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
    New-AzureRmVirtualNetworkGateway -Name $vpnName `
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
    > Bu işlemin tamamlanması birkaç dakika sürebilir.

5. Azure depolama hesabı ve blob kapsayıcısı oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. Hdınsight kümesi oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the HDInsight cluster
    New-AzureRmHDInsightCluster `
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
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

  > [!WARNING]
  > Bu işlemi tamamlamak için yaklaşık 15 dakika sürer.

8. Sanal ağ için Windows VPN istemcisi URL'sini almak için aşağıdaki cmdlet'i kullanın:

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    Windows VPN istemcisi yüklemek için web tarayıcınızda döndürülen URI kullanın.

### <a name="configure-kafka-for-ip-advertising"></a>Kafka IP reklam için yapılandırma

Varsayılan olarak, Zookeeper istemcilere Kafka aracıların etki alanı adını döndürür. Bu yapılandırma tarafından ad çözümlemesi sanal ağ içindeki varlıklar için kullanılamaz olarak VPN yazılımı istemcisi ile çalışmaz. Bu yapılandırma için IP adresleri etki alanı adları yerine tanıtmak için Kafka yapılandırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısı kullanarak https://CLUSTERNAME.azurehdinsight.net için gidin. Değiştir __CLUSTERNAME__ Hdınsight kümesinde Kafka adı.

    İstendiğinde, küme için HTTPS kullanıcı adı ve parola kullanın. Küme için Ambari Web kullanıcı Arabirimi görüntülenir.

2. Kafka hakkında bilgileri görüntülemek için seçin __Kafka__ sol taraftaki listeden.

    ![Hizmet listesi Kafka ile vurgulanan](./media/apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için seçin __yapılandırmalar__ üst ortasından.

    ![Kafka için yapılandırmalar bağlantıları](./media/apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. Bulunacak __kafka env__ yapılandırma, girin `kafka-env` içinde __filtre__ sağ üst alanını.

    ![Kafka env için Kafka yapılandırma](./media/apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. IP adresleri tanıtmak için Kafka yapılandırmak için aşağıdaki metni altına ekleyin __kafka env şablonu__ alan:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka dinlediği arabirimi yapılandırmak için girin `listeners` içinde __filtre__ sağ üst alanını.

7. Tüm ağ arabirimleri üzerinde dinlenecek Kafka yapılandırmak için değeri değiştirin __dinleyicileri__ alanı `PLAINTEXT://0.0.0.0:9092`.

8. Yapılandırma değişikliklerini kaydetmek için kullanın __kaydetmek__ düğmesi. Değişiklikleri açıklayan bir metin iletisi girin. Seçin __Tamam__ değişiklikler kaydedildikten sonra.

    ![Kaydet Yapılandırma düğmesi](./media/apache-kafka-connect-vpn-gateway/save-button.png)

9. Kafka yeniden başlatılırken hataları önlemek için kullanmak __hizmet eylemleri__ düğmesine tıklayın ve ardından __kapatma üzerinde Bakım modu__. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Hizmet eylemlerle vurgulanmış bakım etkinleştirin](./media/apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. Kafka yeniden başlatmak için kullanın __yeniden__ düğmesine tıklayın ve ardından __yeniden tüm etkilenen__. Yeniden başlatma onaylayın ve ardından __Tamam__ işlemi tamamlandıktan sonra düğmesine tıklayın.

    ![Etkilenen yeniden başlatma ile tüm düğmesi vurgulanmış yeniden başlatın](./media/apache-kafka-connect-vpn-gateway/restart-button.png)

11. Bakım modu devre dışı bırakmak için __hizmet eylemleri__ düğmesine tıklayın ve ardından __Bakım modu devre dışı bırakma__. Seçin **Tamam** bu işlemi tamamlamak için.

### <a name="connect-to-the-vpn-gateway"></a>VPN ağ geçidine bağlanmak

VPN ağ geçidi'nden bağlanmak için bir __Windows İstemcisi__, kullanın __Azure Bağlan__ bölümünü [noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) belge.

## <a id="python-client"></a>Örnek: Python istemci

Kafka bağlantısını doğrulamak için oluşturmak ve Python üretici ve tüketici çalıştırmak için aşağıdaki adımları kullanın:

1. Tam etki alanı adı (FQDN) ve Kafka kümedeki düğümlerin IP adreslerini almak için aşağıdaki yöntemlerden birini kullanın:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

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

    Bu komut dosyası varsayar `$resourceGroupName` sanal ağı içeren Azure kaynak grubunun adı.

    Döndürülen bilgilerin kullanmak için sonraki adımlarda kaydedin.

2. Yüklemek için aşağıdakileri kullanın [kafka python](http://kafka-python.readthedocs.io/) istemci:

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

    Değiştir `'kafka_broker'` döndürülen adresleriyle girişleri bu bölümdeki 1. adım:

    * Kullanıyorsanız bir __yazılım VPN istemcisi__, yerine `kafka_broker` girişleri çalışan düğümlerinizin IP adresine sahip.

    * Varsa __ad çözümlemesinin özel bir DNS sunucusu üzerinden etkin__, yerine `kafka_broker` çalışan düğümleri FQDN'sini girişleri.

    > [!NOTE]
    > Bu kod dizesi gönderir `test message` konusuna `testtopic`. Varsayılan yapılandırması hdınsight'ta Kafka, henüz yoksa konu oluşturmaktır.

4. Kafka iletileri almak için aşağıdaki Python kodu kullanın:

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

    Değiştir `'kafka_broker'` döndürülen adresleriyle girişleri bu bölümdeki 1. adım:

    * Kullanıyorsanız bir __yazılım VPN istemcisi__, yerine `kafka_broker` girişleri çalışan düğümlerinizin IP adresine sahip.

    * Varsa __ad çözümlemesinin özel bir DNS sunucusu üzerinden etkin__, yerine `kafka_broker` çalışan düğümleri FQDN'sini girişleri.

## <a name="next-steps"></a>Sonraki adımlar

Bir sanal ağ ile Hdınsight kullanma hakkında daha fazla bilgi için bkz: [genişletmek Azure bir Azure sanal ağı kullanarak Hdınsight](../hdinsight-extend-hadoop-virtual-network.md) belge.

Noktadan siteye VPN ağ geçidi ile bir Azure sanal ağı oluşturma konusunda daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Portalı'nı kullanarak bir noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Azure PowerShell kullanarak bir noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

HDInsight üzerinde Kafka ile çalışma hakkında daha fazla bilgi için şu belgelere göz atın:

* [HDInsight'ta Kafka kullanmaya başlama](apache-kafka-get-started.md)
* [Hdınsight üzerinde Kafka ile yansıtma kullanın](apache-kafka-mirroring.md)
