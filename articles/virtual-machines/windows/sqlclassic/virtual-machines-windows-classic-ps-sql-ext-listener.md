---
title: "Always On kullanılabilirlik grupları için dış dinleyici yapılandırma | Microsoft Docs"
description: "Bu öğreticide, harici olarak erişilebilir olan ilişkili bulut hizmetinin genel sanal IP adresi kullanarak azure'da bir her zaman üzerinde kullanılabilirlik grubu dinleyicisi oluşturma adımları açıklanmaktadır."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 8e506be42aea4fb3c48c29b771a78dcf694f4518
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Always On kullanılabilirlik grupları için dış dinleyici Azure'da yapılandırın
> [!div class="op_single_selector"]
> * [İç dinleyicisi](../classic/ps-sql-int-listener.md)
> * [Dış dinleyici](../classic/ps-sql-ext-listener.md)
> 
> 

Bu konuda bir her zaman üzerinde kullanılabilirlik Internet'te harici olarak erişilebilir olan grup için bir dinleyici yapılandırma gösterilmektedir. Bu bulut hizmetinin ilişkilendirerek mümkün hale getirilir **ortak sanal IP (VIP)** dinleyicisi adresiyle.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Kullanılabilirlik grubu, şirket içi yalnızca Azure yalnızca çoğaltmaları içermiyor ya da şirket içi ve Azure karma yapılandırmalar için span. Azure çoğaltmaları aynı bölge içinde veya birden çok sanal ağlar (Vnet'ler) kullanarak birden çok bölgeye bulunabilir. Aşağıdaki adımları zaten sahip varsayın [bir kullanılabilirlik grubu yapılandırılmış](../classic/portal-sql-alwayson-availability-groups.md) ancak dinleyici yapılandırılmamış.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Kuralları ve sınırlamaları dış dinleyicileri
Bulut hizmeti pubic VIP adresi kullanarak dağıtıyorsanız Azure kullanılabilirlik grubu dinleyicisi hakkında aşağıdaki yönergeleri göz önünde bulundurun:

* Kullanılabilirlik grubu dinleyicisi, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 üzerinde desteklenir.
* İstemci uygulaması, kullanılabilirlik grubunuzun VM'ler içeren olandan farklı bir bulut hizmeti bulunması gerekir. Azure doğrudan sunucu dönüşünü istemci ve sunucu ile aynı bulut hizmetinde desteklemez.
* Varsayılan olarak, bu makaledeki adımları nasıl bulut hizmeti sanal IP (VIP) adresi kullanmak için bir dinleyici yapılandırılacağını gösterir. Ancak, ayırabilir ve bulut hizmetiniz için birden çok VIP adresleri oluşturmak mümkündür. Bu, her farklı bir VIP ile ilişkili birden çok dinleyici oluşturmak için bu makaledeki adımları kullanmanıza olanak sağlar. Birden çok VIP adresleri oluşturma hakkında daha fazla bilgi için bkz: [bulut hizmeti başına birden çok Vip](../../../load-balancer/load-balancer-multivip.md).
* Karma bir ortamınız için bir dinleyici oluşturuyorsanız, şirket içi ağ siteden siteye VPN Azure sanal ağı ile yanı sıra genel Internet bağlantısı olması gerekir. Azure alt olduğunda, kullanılabilirlik grubu dinleyicisi yalnızca genel IP adresiyle ilgili bulut hizmetinin erişilebilir değil.
* Dış dinleyici iç yük dengeleyici (ILB) kullanarak bir iç dinleyicisi de sahip olduğu aynı bulut hizmeti oluşturmak için desteklenmiyor.

## <a name="determine-the-accessibility-of-the-listener"></a>Dinleyici erişilebilirliğini belirleme
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Bu makalede kullanan bir dinleyici oluşturmaya odaklıdır **dış Yük Dengeleme**. Sanal ağınıza özeldir bir dinleyici istiyorsanız ayarlamak için adımları sağlar. Bu makalenin sürümü bakın bir [ILB ile dinleyicisi](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Yük dengeli VM uç noktalar ile doğrudan sunucu dönüş oluşturma
Dış Yük Dengeleme sanal genel sanal IP adresi, sanal makineleri barındıran bulut hizmetini kullanır. Bu nedenle oluşturmak veya yük dengeleyici bu durumda yapılandırmanız gerekmez.

Bir Azure çoğaltma barındıran her VM için bir yük dengeli uç noktası oluşturmanız gerekir. Birden çok bölgede çoğaltmalar varsa, her çoğaltma bu bölge için aynı bulut Hizmeti'nde aynı sanal ağda olması gerekir. Birden fazla Azure bölgesine yayılan kullanılabilirlik grubu oluşturma çoğaltmaları birden çok sanal ağlar yapılandırma gerektirir. Çapraz VNet bağlantı yapılandırma hakkında daha fazla bilgi için bkz: [VNet bağlantı yapılandırma Vnet'e](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Azure portalında bir çoğaltma barındıran her VM gidin ve ayrıntılarını görüntüleyin.
2. Tıklatın **uç noktaları** VM'lerin her birinde sekmesi.
3. Doğrulayın **adı** ve **genel bağlantı noktası** dinleyicisi olarak kullanmak istediğiniz uç noktası zaten kullanımda olup olmadığını. Aşağıdaki örnekte, adı "MyEndpoint" ve "1433" bağlantı noktasıdır.
4. Yerel istemcide, indirme ve yükleme [son PowerShell Modülü](https://azure.microsoft.com/downloads/).
5. Başlatma **Azure PowerShell**. Yüklenen Azure yönetim modülleri yeni bir PowerShell oturumu açılır.
6. Çalıştırma **Get-AzurePublishSettingsFile**. Bu cmdlet, yerel bir dizine yayımlama ayarları dosyasını indirmek için bir tarayıcı yönlendirir. Azure aboneliğiniz için oturum açma kimlik bilgileriniz istenebilir.
7. Çalıştırma **Import-AzurePublishSettingsFile** komutunu indirdiğiniz yayımlama ayarları dosyası yolu:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Yayımlama ayarları dosyasını içeri aktarıldığında, PowerShell oturumunda Azure aboneliğinizi yönetebilirsiniz.
    
1. Aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve (Varsayılanları için bazı parametreler sağlanmış olan) ortamınıza uygun değişken değerlerini ayarlayın. Kullanılabilirlik grubu Azure bölgeleri yayılıyorsa, komut dosyası bir kez her veri merkezinde bulut hizmeti ve bu veri merkezinde bulunan düğümler için çalıştırmanız gerektiğini unutmayın.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Değişkenleri ayarladıktan sonra komut dosyasını çalıştırmak için Metin Düzenleyicisi'nden Azure PowerShell oturumunuza kopyalayın. İstemi hala gösteriyorsa >>, yeniden komut dosyasını başlatır çalıştıran emin olmak için girin.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>KB2854082 gerekirse yüklü olduğunu doğrulayın
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Kullanılabilirlik grubu düğümler güvenlik duvarı bağlantı noktalarını açın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi oluşturma

Kullanılabilirlik grubu dinleyicisi iki adımda oluşturun. İlk olarak, istemci erişim noktası küme kaynağı oluşturun ve bağımlılıklarını yapılandırın. İkinci olarak, küme kaynaklarını PowerShell ile yapılandırın.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>İstemci erişim noktası oluşturun ve küme bağımlılıklarını yapılandırın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>PowerShell'de küme kaynaklarını yapılandırma
1. Dış Yük Dengeleme için çoğaltmalarınızı içeren bulut hizmeti genel sanal IP adresi edinmeniz gerekir. Azure portalında oturum açın. Kullanılabilirlik grubunuzun VM içeren bulut hizmetine gidin. Açık **Pano** görünümü.
2. Altında gösterilen adresini Not **ortak sanal IP (VIP) adresi**. Çözümünüzü sanal ağlara yayılıyorsa, bir çoğaltma barındıran bir VM içeren her bulut hizmeti için bu adımı yineleyin.
3. Sanal makineleri birinde aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve daha önce not ettiğiniz değerleri değişkenleri ayarlayın.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Sonra değişkenleri ayarladığınız, yükseltilmiş bir Windows PowerShell penceresi açın sonra metin Düzenleyicisi'nden betiği kopyalayın ve çalıştırmak için Azure PowerShell oturumunuza yapıştırın. İstemi hala gösteriyorsa >>, yeniden komut dosyasını başlatır çalıştıran emin olmak için girin.
5. Her VM yineleyin. Bu komut dosyası Bulut hizmeti IP adresi ile IP adresi kaynağı yapılandırır ve diğer parametreleri gibi araştırma bağlantı noktasını ayarlar. IP adresi kaynağı çevrimiçi duruma getirildiğinde, ardından sonda bağlantı noktası üzerinde sorgulama Bu öğreticide daha önce oluşturulan yük dengeli uç noktasından yanıt verebilir.

## <a name="bring-the-listener-online"></a>Dinleyici çevrimiçi duruma getirin
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>İzleme öğeleri
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Kullanılabilirlik grubu dinleyicisini (içinde aynı VNet) test edin
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Kullanılabilirlik grubu dinleyicisini (Internet üzerinden) test edin
Sanal Ağ dışından gelen dinleyicisi erişmek için (Bu konuda açıklanan) dış/ortak Yük Dengeleme ILB yerine, aynı sanal ağ içinden yalnızca erişilebilir olduğu kullanıyor olmanız gerekir. Bağlantı dizesinde, bulut hizmet adı belirtin. Örneğin, bu ada sahip bir bulut hizmet olsaydı *mycloudservice*, sqlcmd deyimi aşağıdaki gibi olur:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Çağıran internet üzerinden windows kimlik doğrulaması kullanamadığından önceki örneğin aksine, SQL kimlik doğrulaması, kullanılması gerekir. Daha fazla bilgi için bkz: [her zaman üzerindeki kullanılabilirlik grubu Azure VM içinde: istemci bağlantısı senaryoları](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). SQL kimlik doğrulaması kullanırken, her iki çoğaltmalarda aynı oturum açma oluşturduğunuzdan emin olun. Oturumları kullanılabilirlik grupları ile ilgili sorunları giderme hakkında daha fazla bilgi için bkz: [diğer çoğaltmalar için bağlanmak ve kullanılabilirlik veritabanlarına eşlemek için SQL veritabanı kullanıcı oturumlarını eşleştirmek veya kullanmak nasıl bulunan](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

İstemciler her zaman açık çoğaltmaları farklı alt ağlarda olup olmadığını belirtmeniz **MultisubnetFailover = True** bağlantı dizesinde. Bu, paralel bağlantı denemelerinde farklı alt ağlar yinelemeleri sonuçlanır. Bu senaryo bir çapraz bölge her zaman üzerindeki kullanılabilirlik grubu dağıtımı içerdiğine dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

