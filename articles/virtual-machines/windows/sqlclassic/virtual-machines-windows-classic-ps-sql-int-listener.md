---
title: "Azure'da bir ILB dinleyicisi Always On kullanılabilirlik grupları için yapılandırma | Microsoft Docs"
description: "Bu öğretici Klasik dağıtım modeliyle oluşturulan kaynakları kullanır ve bir iç yük dengeleyici kullanan Azure'da bir Always On kullanılabilirlik grubu dinleyicisi oluşturur."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: fea70b389b1f1d6af963e3f14fdc48e8d857dd53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Azure'da bir ILB dinleyicisi Always On kullanılabilirlik grupları için yapılandırın
> [!div class="op_single_selector"]
> * [İç dinleyicisi](../classic/ps-sql-int-listener.md)
> * [Dış dinleyici](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Klasik dağıtım modeli kullanımını da kapsar. En yeni dağıtımların Resource Manager modelini kullanmasını öneririz.

Resource Manager modelinde bir Always On kullanılabilirlik grubu için bir dinleyici yapılandırmak için bkz: [Azure'da bir Always On kullanılabilirlik grubu için yük dengeleyici yapılandırma](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

Kullanılabilirlik grubu yalnızca şirket içinde veya Azure yalnızca olan ya da karma yapılandırmalar için şirket içi ve Azure span çoğaltmaları içerebilir. Azure çoğaltmaları aynı bölge içinde veya birden çok sanal ağ kullanan birden çok bölgeye bulunabilir. Bu makaledeki yordamlar zaten olduğunu varsayın [bir kullanılabilirlik grubu yapılandırılmış](../classic/portal-sql-alwayson-availability-groups.md) ancak henüz bir dinleyici oluşturmamış.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Kuralları ve sınırlamaları iç dinleyicileri
Azure'da bir kullanılabilirlik grubu dinleyicisiyle bir iç yük dengeleyici (ILB) aşağıdaki yönergeleri tabi kullanılır:

* Kullanılabilirlik grubu dinleyicisi, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 üzerinde desteklenir.
* Yalnızca bir iç kullanılabilirlik grubu dinleyicisi her bulut hizmeti için dinleyici ILB için yapılandırılır ve her bir bulut hizmeti için yalnızca bir ILB için desteklenir. Ancak, birden çok dış dinleyici oluşturmak mümkündür. Daha fazla bilgi için bkz: [Azure'da Always On kullanılabilirlik grupları için dış dinleyici yapılandırın](../classic/ps-sql-ext-listener.md).

## <a name="determine-the-accessibility-of-the-listener"></a>Dinleyici erişilebilirliğini belirleme
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Bu makale, ILB kullanan bir dinleyici oluşturmaya odaklıdır. Bir genel veya dış dinleyici ihtiyacınız varsa, yedekleme ayarı ele bu makalenin sürümü bkz bir [dış dinleyici](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Yük dengeli VM uç noktalar ile doğrudan sunucu dönüş oluşturma
Bir ILB ilk komut daha sonra bu bölümde çalıştırarak oluşturun.

Yük dengeli bir uç nokta için bir Azure çoğaltması barındıran her VM oluşturun. Birden çok bölgede çoğaltmalar varsa, o bölge için her çoğaltma aynı Azure sanal ağı aynı bulut hizmeti olması gerekir. Birden fazla Azure bölgesine yayılan grubu çoğaltmalarının kullanılabilirlik oluşturmak için birden çok sanal ağ yapılandırma gerekir. Sanal ağ bağlantısı yapılandırma hakkında daha fazla bilgi için bkz: [sanal ağ bağlantısı sanal ağa yapılandırma](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Azure portalında ayrıntılarını görüntülemek için bir çoğaltma barındıran her VM gidin.

2. Tıklatın **uç noktaları** sekmesini her VM için.

3. Doğrulayın **adı** ve **genel bağlantı noktası** dinleyicisi olarak kullanmak istediğiniz uç noktası olmayan zaten kullanımda. Bu bölümdeki örnekte addır *MyEndpoint*, ve bağlantı noktası *1433*.

4. Yerel istemcinize karşıdan yükle ve en son yükleme [PowerShell Modülü](https://azure.microsoft.com/downloads/).

5. Azure PowerShell'i başlatın.  
    Yeni bir PowerShell oturumunda, yüklenen Azure yönetim modülleri açar.

6. `Get-AzurePublishSettingsFile` öğesini çalıştırın. Bu cmdlet, yerel bir dizine yayımlama ayarları dosyasını indirmek için bir tarayıcı yönlendirir. Azure aboneliğiniz için oturum açma kimlik bilgileriniz istenebilir.

7. Aşağıdaki komutu çalıştırarak `Import-AzurePublishSettingsFile` komutunu indirdiğiniz yayımlama ayarları dosyası yolu:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Yayımlama ayarları dosyasını içeri aktarıldıktan sonra PowerShell oturumunda Azure aboneliğinizi yönetebilirsiniz.

8. İçin *ILB*, statik bir IP adresi atayın. Aşağıdaki komutu çalıştırarak geçerli sanal ağ yapılandırmasını inceleyin:

        (Get-AzureVNetConfig).XMLConfiguration
9. Not *alt* çoğaltmaları barındıran sanal makineleri içeren alt ağ için ad. Bu ad, betiği $SubnetName parametresinde kullanılır.

10. Not *VirtualNetworkSite* adı ve başlangıç *AddressPrefix* çoğaltmaları barındıran sanal makineleri içeren alt ağ için. Her iki değerin geçirerek kullanılabilir bir IP adresi arayın `Test-AzureStaticVNetIP` komut ve göre inceleyerek *AvailableAddresses*. Örneğin, sanal ağ adlı *MyVNet* ve başlar bir alt ağ adres aralığı *172.16.0.128*, aşağıdaki komut kullanılabilir adresleri listesi:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Kullanılabilir adresler birini seçin ve komut dosyasının bir sonraki adımda $ILBStaticIP parametresinde kullanın.

12. Aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve ortamınıza uygun değişken değerlerini ayarlayın. Varsayılanlar için bazı parametreler belirtildi.  

    Benzeşim grupları kullanan var olan dağıtımlar bir ILB ekleyemezsiniz. ILB gereksinimleri hakkında daha fazla bilgi için bkz: [iç yük dengeleyiciye genel bakış](../../../load-balancer/load-balancer-internal-overview.md).

    Ayrıca, kullanılabilirlik grubunuzun Azure bölgeleri yayılırsa, komut dosyası her veri merkezinde, veri merkezinde bulunan düğümler ve bulut hizmeti için bir kez çalıştırmanız gerekir.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. Değişkenleri ayarladıktan sonra komut dosyasını çalıştırmak için PowerShell oturumunuzun Metin Düzenleyicisi'nden kopyalayın. İstemi hala gösteriyorsa  **>>** , yeniden komut dosyasını başlatır çalıştıran emin olmak için Enter tuşuna basın.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>KB2854082 gerekirse yüklü olduğunu doğrulayın
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Kullanılabilirlik grubu düğümler güvenlik duvarı bağlantı noktalarını açın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi oluşturma

Kullanılabilirlik grubu dinleyicisi iki adımda oluşturun. İlk olarak, istemci erişim noktası küme kaynağı oluşturun ve bağımlılıklarını yapılandırın. İkinci olarak, küme kaynaklarını PowerShell'de yapılandırın.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>İstemci erişim noktası oluşturun ve küme bağımlılıklarını yapılandırın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>PowerShell'de küme kaynaklarını yapılandırma
1. ILB için daha önce oluşturulan ILB'nin IP adresini kullanmanız gerekir. PowerShell'de bu IP adresi almak için aşağıdaki komut dosyasını kullanın:

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Sanal makineleri birinde işletim sisteminiz için PowerShell komut dosyası bir metin düzenleyicisine kopyalayın ve ardından daha önce not ettiğiniz değerleri değişkenleri ayarlayın.

    Windows Server 2012 veya sonraki sürümlerde, aşağıdaki komut dosyasını kullanın:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    Windows Server 2008 R2 için aşağıdaki komut dosyasını kullanın:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Değişkenleri ayarladıktan sonra yükseltilmiş bir Windows PowerShell penceresi açın, metin düzenleyici komut dosyasını çalıştırmak için PowerShell oturumunuza yapıştırın. İstemi hala gösteriyorsa  **>>** , yeniden komut dosyası çalışmaya başladıktan emin olmak için ENTER'a basın.

4. Her VM için önceki adımları yineleyin.  
    Bu komut dosyası Bulut hizmeti IP adresi ile IP adresi kaynağı yapılandırır ve diğer parametreleri gibi araştırma bağlantı noktasını ayarlar. IP adresi kaynağı çevrimiçi duruma getirildiğinde, araştırma bağlantı noktası üzerinde sorgulama yük dengeli uç noktasından daha önce oluşturduğunuz yanıt verebilir.

## <a name="bring-the-listener-online"></a>Dinleyici çevrimiçi duruma getirin
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>İzleme öğeleri
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a>Kullanılabilirlik grubu dinleyicisini (aynı sanal ağda) test edin
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
