---
title: Always On kullanılabilirlik grupları için dış dinleyici yapılandırma | Microsoft Docs
description: Bu öğretici azure'da ilişkili bulut hizmeti genel sanal IP adresini kullanarak harici olarak erişilebilen bir Always On kullanılabilirlik grubu dinleyicisi oluşturma adımlarında size kılavuzluk eder.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 89623adbddce07cbc3c3ead811f5174d108c9b0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62101634"
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Azure'da AlwaysOn Kullanılabilirlik grupları için dış dinleyici yapılandırma
> [!div class="op_single_selector"]
> * [Dinleyici iç](../classic/ps-sql-int-listener.md)
> * [Dış dinleyici](../classic/ps-sql-ext-listener.md)
> 
> 

Bu konuda, bir Always On kullanılabilirlik Internet'te harici olarak erişilebilen grubu için bir dinleyici yapılandırma işlemini göstermektedir. Bu bulut hizmetinin ilişkilendirerek mümkün hale getirilir **genel sanal IP (VIP)** adresini dinleyici ile.

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Kullanılabilirlik grubunuzun, şirket içinde yalnızca, yalnızca Azure yinelemeler içeriyor ya da karma yapılandırmalar için hem şirket içi hem de Azure span. Azure çoğaltmaları aynı bölge içinde veya birden çok sanal ağlar (Vnet'ler) kullanarak birden çok bölgede bulunabilir. Aşağıdaki adımlar varsayılmaktadır [bir kullanılabilirlik grubunda yapılandırılmış](../classic/portal-sql-alwayson-availability-groups.md) ancak bir dinleyici yapılandırılmadı.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Kılavuzlar ve sınırlamalar için dış dinleyici
Azure'da kullanılabilirlik grubu dinleyicisi hakkında aşağıdaki yönergelere, bulut hizmeti genel VIP adresini kullanarak dağıtırken dikkat edin:

* Kullanılabilirlik grubu dinleyicisi, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 üzerinde desteklenir.
* İstemci uygulama, kullanılabilirlik grubunuzun Vm'leri içeren olandan farklı bir bulut hizmeti üzerinde bulunmalıdır. Azure aynı bulut hizmetinde istemci ve sunucu ile doğrudan sunucu döndürmeyi desteklemez.
* Varsayılan olarak, bu makaledeki adımlarda, bulut hizmeti sanal IP (VIP) adresini kullanmak için bir dinleyici yapılandırma işlemini göstermektedir. Ancak, ayırabilir ve bulut hizmeti için birden çok VIP adresleri oluşturmak mümkündür. Bu, her biri farklı bir VIP ile ilişkili birden çok dinleyici oluşturmak için bu makaledeki adımları kullanmayı sağlar. Birden çok VIP adresleri oluşturma hakkında daha fazla bilgi için bkz: [bulut hizmeti başına birden çok VIP](../../../load-balancer/load-balancer-multivip.md).
* Şirket içi ağ, karma bir ortamınız için bir dinleyici oluşturuyorsanız, genel İnternet'e ek Azure sanal ağı ile siteden siteye VPN bağlantısı olması gerekir. Azure alt ağındaki olduğunda, kullanılabilirlik grubu dinleyicisi yalnızca genel IP adresini ilgili bulut hizmeti tarafından erişilebilir.
* İç yük dengeleyici (ILB) kullanarak iç dinleyici de sahip olduğu aynı bulut hizmetinde dış dinleyici oluşturmak için desteklenmiyor.

## <a name="determine-the-accessibility-of-the-listener"></a>Dinleyici erişilebilirliğini belirleme
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Bu makalede kullanan bir dinleyici oluşturmaya odaklıdır **dış Yük Dengeleme**. Sürümü ayarlama adımları sağlar. Bu makalede, sanal ağınıza özeldir bir dinleyici isterseniz bkz bir [ILB dinleyicisi](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Yük dengeli bir VM uç noktaları ile doğrudan sunucu dönüş oluşturma
Dış Yük Dengeleme sanal makinelerinizi barındıran bulut hizmetini genel sanal IP adresini kullanır. Bu nedenle oluşturmak veya bu durumda yük dengeleyici yapılandırmanız gerekmez.

Bir Azure çoğaltması barındıran her VM için yük dengeli uç nokta oluşturmanız gerekir. Birden çok bölgede çoğaltma varsa, her çoğaltma bu bölge için aynı bulut hizmetinde aynı sanal ağda olmalıdır. Birden fazla Azure bölgesini kapsayan oluşturma kullanılabilirlik grubu çoğaltmalarının birden fazla sanal ağ yapılandırma gerektirir. Çapraz sanal ağa bağlantı yapılandırma hakkında daha fazla bilgi için bkz. [Sğdan sanal ağa bağlantı yapılandırma](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Azure portalında bir çoğaltmasını barındıran her sanal Makineye gidin ve ayrıntılarını görüntüleyin.
2. Tıklayın **uç noktaları** her VM için sekmesinde.
3. Doğrulayın **adı** ve **genel bağlantı noktası** dinleyicisi kullanmak istediğiniz uç noktası zaten kullanımda olup olmadığını. Aşağıdaki örnekte, adı "MyEndpoint" ve "1433" bağlantı noktasıdır.
4. Yerel istemciniz üzerinde yükleyip [en son PowerShell modülünün](https://azure.microsoft.com/downloads/).
5. Başlatma **Azure PowerShell**. Yüklenen Azure yönetim modülleri yeni bir PowerShell oturumu açılır.
6. Çalıştırma **Get-AzurePublishSettingsFile**. Bu cmdlet bir yayımlama ayarları dosyasına yerel bir dizine indirmek için bir tarayıcıya yönlendirir. Azure aboneliğiniz için oturum açma kimlik bilgileriniz istenebilir.
7. Çalıştırma **Import-AzurePublishSettingsFile** komutunu indirdiğiniz yayımlama ayarları dosyası yolu:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Yayımlama ayarları dosyasını içeri aktarıldıktan sonra bir PowerShell oturumunda Azure aboneliğinizi yönetebilirsiniz.
    
1. Aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve değişken değerlerini (Varsayılanları bazı parametreler için sağlanmış olan) ortamınıza uyacak şekilde ayarlayın. Kullanılabilirlik grubunuzun Azure bölgesine yayılmış durumdaysa, betiği bir kez, veri merkezinde bulunan düğümleri ve bulut hizmeti için her bir veri merkezinde çalıştırmanız gerektiğini unutmayın.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Değişkenleri ayarladıktan sonra komut dosyasını çalıştırmak için metin düzenleyiciden Azure PowerShell oturumunuza kopyalayın. İstemi hala gösteriliyorsa >>, yeniden komut dosyasını başlatır çalıştıran emin olmak için girin.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Gerekirse KB2854082 yüklü olduğunu doğrulayın
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Kullanılabilirlik grubu düğümler güvenlik duvarı bağlantı noktalarını açın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi oluşturun

Kullanılabilirlik grubu dinleyicisi iki adımda oluşturun. İlk olarak, istemci erişim noktası küme kaynağı oluşturun ve bağımlılıklarını yapılandırın. İkinci olarak, küme kaynaklarını PowerShell ile yapılandırın.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>İstemci erişim noktası oluşturun ve küme bağımlılıklarını yapılandırın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>PowerShell'de küme kaynaklarını yapılandırma
1. Dış Yük Dengeleme için çoğaltmalarınızın içeren bulut hizmeti genel sanal IP adresini almanız gerekir. Azure portalında oturum açın. Kullanılabilirlik grubunuzun VM içeren bulut hizmetine gidin. Açık **Pano** görünümü.
2. Altında gösterilen adresi **genel sanal IP (VIP) adresi**. Çözümünüzü sanal ağlara yayılıyorsa bir çoğaltmasını barındıran bir VM içeren her bulut hizmeti için bu adımı yineleyin.
3. VM'lerin birinde aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve daha önce not ettiğiniz değerleri değişkenleri ayarlayın.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Bir kez, değişkenleri ayarladıktan, yükseltilmiş bir Windows PowerShell penceresi açın ardından betiği Metin Düzenleyicisi'nden kopyalayın ve çalıştırmak için Azure PowerShell oturumunuza yapıştırın. İstemi hala gösteriliyorsa >>, yeniden komut dosyasını başlatır çalıştıran emin olmak için girin.
5. Bu, her sanal makinede işlemi yineleyin. Bu betik, bulut hizmetinin IP adresi ile IP adresi kaynağı yapılandırır ve diğer parametreler, yoklama bağlantı noktası gibi ayarlar. IP adresi kaynağı çevrimiçi duruma getirildiğinde, ardından yoklama yoklama bağlantı noktası üzerinde Bu öğreticide daha önce oluşturduğunuz yük dengeli uç noktasından yanıt verebilir.

## <a name="bring-the-listener-online"></a>Dinleyici Çevrimiçi Getir
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>İzleme öğeleri
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Kullanılabilirlik grubu dinleyicisini (aynı VNet içinde) test
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Kullanılabilirlik grubu dinleyicisini (internet üzerinden) test etme
Sanal ağ dışındaki dinleyicisinden erişmek için (Bu konuda açıklanan) dış/genel Yük Dengeleme ILB yerine yalnızca aynı sanal ağ içinde erişilebilir kullanıyor olmanız gerekir. Bağlantı dizesinde, bulut hizmeti adı belirtin. Örneğin, bir bulut hizmeti adı ile olsaydı *mycloudservice*, sqlcmd deyim şu şekilde olacaktır:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Çağıran, Internet üzerinden windows kimlik doğrulaması kullanamadığından önceki örnekte farklı olarak, SQL kimlik doğrulaması, kullanılmalıdır. Daha fazla bilgi için [Always On kullanılabilirlik grubu Azure VM'de: İstemci Bağlantı Senaryoları](https://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). SQL kimlik doğrulaması kullanırken, hem çoğaltmalarda aynı oturum açma oluşturduğunuzdan emin olun. Kullanılabilirlik grupları ile oturum açma sorunlarını giderme hakkında daha fazla bilgi için bkz. [diğer yinelemeler için bağlanmak ve kullanılabilirlik veritabanlarına eşlemek için SQL veritabanı kullanıcı oturumlarını eşleştirmek veya kullanmak nasıl bulunan](https://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

İstemciler her zaman açık çoğaltma farklı alt ağlarda olup olmadığını belirtmeniz **MultisubnetFailover = True** bağlantı dizesindeki. Bu, paralel bağlantı denemelerinde de çoğaltmaları, farklı alt ağlarda sonuçlanır. Bu senaryo bölgeler arası Always On kullanılabilirlik grubu dağıtım içerdiğine dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

