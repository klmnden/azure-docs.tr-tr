---
title: Azure'da AlwaysOn Kullanılabilirlik grupları için ILB dinleyicisi yapılandırma | Microsoft Docs
description: Bu öğreticide Klasik dağıtım modeliyle oluşturulan kaynaklar kullanılır ve iç yük dengeleyici kullanan Azure'da bir Always On kullanılabilirlik grubu dinleyicisi oluşturur.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 0e6a52ea2fdd05546a4da9f8cd1165b41ed27944
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097735"
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Azure'da AlwaysOn Kullanılabilirlik grupları için ILB dinleyicisi yapılandırma
> [!div class="op_single_selector"]
> * [Dinleyici iç](../classic/ps-sql-int-listener.md)
> * [Dış dinleyici](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale Klasik dağıtım modeli kullanımı kapsar. En yeni dağıtımların Resource Manager modelini kullanmasını öneririz.

Resource Manager modelinde bir Always On kullanılabilirlik grubu için bir dinleyici yapılandırmak için bkz [Azure'da Always On kullanılabilirlik grubu için bir yük dengeleyici yapılandırma](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

Kullanılabilirlik grubunuzun yalnızca şirket içi veya Azure yalnızca olan veya karma yapılandırmalar için hem şirket içi hem de Azure span çoğaltma içerebilir. Azure çoğaltmaları aynı bölge içinde veya birden çok sanal ağ kullanan birden çok bölgede bulunabilir. Bu makalede yer alan yordamları, zaten sahip olduğunuzu varsaymaktadır [bir kullanılabilirlik grubunda yapılandırılmış](../classic/portal-sql-alwayson-availability-groups.md) ancak henüz bir dinleyici yapılandırmadınız.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Kılavuzlar ve sınırlamalar için dahili dinleyicileri
Azure'da bir kullanılabilirlik grubu dinleyicisi ile iç yük dengeleyici (ILB) tabi aşağıdaki yönergeleri kullanılır:

* Kullanılabilirlik grubu dinleyicisi, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 üzerinde desteklenir.
* Yalnızca bir iç kullanılabilirlik grubu dinleyicisi için ILB dinleyicisi yapılandırılır ve yalnızca bir ILB için her bir bulut hizmeti olduğundan, her bir bulut hizmeti için desteklenir. Ancak, birden çok dış dinleyici oluşturmak mümkündür. Daha fazla bilgi için [Azure'da AlwaysOn Kullanılabilirlik grupları için dış dinleyici yapılandırma](../classic/ps-sql-ext-listener.md).

## <a name="determine-the-accessibility-of-the-listener"></a>Dinleyici erişilebilirliğini belirleme
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Bu makale, ILB kullanan bir dinleyici oluşturmaya odaklıdır. Genel veya dış bir dinleyici gerekiyorsa, bu makalenin ayarı ayarlama açıklanır sürümü görmek bir [dış dinleyici](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Yük dengeli bir VM uç noktaları ile doğrudan sunucu dönüş oluşturma
Bir ILB ilk betik daha sonra bu bölümde çalıştırarak oluşturun.

Bir Azure çoğaltması barındıran her VM için yük dengeli uç nokta oluşturun. Birden çok bölgede çoğaltma varsa, her çoğaltma bu bölge için aynı Azure sanal ağı aynı bulut hizmetinde olması gerekir. Kullanılabilirlik grubu çoğaltmalarının, birden fazla Azure bölgesini kapsayan oluşturma, birden fazla sanal ağ yapılandırma gerektirir. Platformlar arası sanal ağ bağlantısını yapılandırmayla ilgili daha fazla bilgi için bkz: [sanal ağ bağlantısını için sanal ağ yapılandırma](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Azure portalında ayrıntılarını görüntülemek için bir çoğaltma barındıran her sanal Makineye gidin.

2. Tıklayın **uç noktaları** her VM için sekmesinde.

3. Doğrulayın **adı** ve **genel bağlantı noktası** dinleyicisi kullanmak istediğiniz uç noktası olmayan zaten kullanımda. Bu bölümdeki örnekte addır *MyEndpoint*, ve bağlantı noktası *1433*.

4. Yerel istemciniz indirin ve en son yükleme [PowerShell Modülü](https://azure.microsoft.com/downloads/).

5. Azure PowerShell'i başlatın.  
    Yüklenen Azure yönetim modülleri yeni bir PowerShell oturumu açılır.

6. `Get-AzurePublishSettingsFile` öğesini çalıştırın. Bu cmdlet bir yayımlama ayarları dosyasına yerel bir dizine indirmek için bir tarayıcıya yönlendirir. Azure aboneliğiniz için oturum açma kimlik bilgileriniz istenebilir.

7. Aşağıdaki komutu çalıştırın `Import-AzurePublishSettingsFile` komutunu indirdiğiniz yayımlama ayarları dosyası yolu:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Yayımlama ayarları dosyasını içeri aktardıktan sonra bir PowerShell oturumunda Azure aboneliğinizi yönetebilirsiniz.

8. İçin *ILB*, statik bir IP adresi atayın. Aşağıdaki komutu çalıştırarak geçerli sanal ağ yapılandırmasını inceleyin:

        (Get-AzureVNetConfig).XMLConfiguration
9. Not *alt* Vm'leri içeren alt ağ için çoğaltmaları barındıran ad. Bu ad, betik $SubnetName parametresinde kullanılır.

10. Not *VirtualNetworkSite* adı ve başlangıç *AddressPrefix* çoğaltmaları barındıran sanal makineleri içeren alt ağ. Her iki değerin geçirerek için kullanılabilir bir IP adresi Ara `Test-AzureStaticVNetIP` komut ve bunun İnceleme *AvailableAddresses*. Örneğin, sanal ağ adlandırılmışsa *MyVNet* ve başlar bir alt ağ adres aralığı *172.16.0.128*, aşağıdaki komut kullanılabilir adresleri listesi:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Kullanılabilir adreslerinden birini seçin ve sonraki adımda betiği $ILBStaticIP parametresini kullanın.

12. Aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve değişken değerlerini ortamınıza uyacak şekilde ayarlayın. Bazı parametreler için varsayılan değerleri sağlanmıştır.  

    Benzeşim grupları kullanan var olan dağıtımlar, ILB ekleyemezsiniz. ILB gereksinimleri hakkında daha fazla bilgi için bkz. [iç yük dengeleyiciye genel bakış](../../../load-balancer/load-balancer-internal-overview.md).

    Ayrıca, kullanılabilirlik grubunuzun Azure bölgesine yayılmış durumdaysa betiği, veri merkezinde bulunan düğümleri ve bulut hizmeti için her bir veri merkezi içinde bir kez çalıştırmanız gerekir.

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

13. Değişkenleri ayarladıktan sonra betiği Metin Düzenleyicisi'nden çalıştırmak için PowerShell oturumunuza kopyalayın. İstemi hala gösteriliyorsa **>>**, yeniden komut dosyasını başlatır çalıştıran emin olmak için Enter tuşuna basın.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Gerekirse KB2854082 yüklü olduğunu doğrulayın
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Kullanılabilirlik grubu düğümler güvenlik duvarı bağlantı noktalarını açın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi oluşturun

Kullanılabilirlik grubu dinleyicisi iki adımda oluşturun. İlk olarak, istemci erişim noktası küme kaynağı oluşturun ve bağımlılıklarını yapılandırın. İkinci olarak, PowerShell'de küme kaynaklarını yapılandırın.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>İstemci erişim noktası oluşturun ve küme bağımlılıklarını yapılandırın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>PowerShell'de küme kaynaklarını yapılandırma
1. ILB için daha önce oluşturulan ILB IP adresini kullanmanız gerekir. PowerShell'de bu IP adresini almak için aşağıdaki betiği kullanın:

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Bir VM işletim sisteminiz için PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve ardından daha önce not ettiğiniz değerleri değişkenleri ayarlayın.

    Windows Server 2012 veya sonraki sürümlerde, aşağıdaki betiği kullanın:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = "<X.X.X.X>" # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    Windows Server 2008 R2 için aşağıdaki betiği kullanın:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = "<X.X.X.X>" # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Değişkenleri ayarladıktan sonra yükseltilmiş bir Windows PowerShell penceresi açın, metin düzenleyici betiği çalıştırmak için PowerShell oturumunuza yapıştırın. İstemi hala gösteriliyorsa **>>**, yeniden betik çalışmaya başladıktan emin olmak için Enter tuşuna basın.

4. Her VM için önceki adımları yineleyin.  
    Bu betik, bulut hizmetinin IP adresi ile IP adresi kaynağını yapılandırır ve diğer parametreler, yoklama bağlantı noktası gibi ayarlar. IP adresi kaynağı çevrimiçi duruma getirildiğinde, yoklama bağlantı noktası üzerinde sorgulama, daha önce oluşturduğunuz yük dengeli uç noktasından yanıt verebilir.

## <a name="bring-the-listener-online"></a>Dinleyici Çevrimiçi Getir
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>İzleme öğeleri
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a>Kullanılabilirlik grubu dinleyicisini (aynı sanal ağ içinde) test etme
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
