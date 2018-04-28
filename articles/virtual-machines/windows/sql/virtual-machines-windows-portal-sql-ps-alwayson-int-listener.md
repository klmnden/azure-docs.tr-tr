---
title: Her zaman kullanılabilirlik grubu dinleyicileri – Microsoft Azure yapılandırma | Microsoft Docs
description: Kullanılabilirlik grubu dinleyicileri iç yük dengeleyiciye sahip bir veya daha fazla IP adresi kullanarak Azure Resource Manager modeli yapılandırın.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 11aecd9b2bc1bc1521a0e27fc3cd06fe7426a26d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Bir veya daha fazla Always On kullanılabilirlik grubu dinleyicileri - Kaynak Yöneticisi'ni yapılandırma
Bu konuda gösterilmektedir nasıl yapılır:

* Bir iç yük dengeleyici için PowerShell cmdlet'lerini kullanarak SQL Server kullanılabilirlik gruplarını oluşturun.
* Birden çok kullanılabilirlik grubu için yük dengeleyici için ek IP adreslerini ekleyin. 

Bir kullanılabilirlik grubu dinleyicisi veritabanı erişimi için istemcilerin bağlandığı bir sanal ağ adı değil. Azure sanal makinelerde yük dengeleyici için dinleyici IP adresini tutar. Yük Dengeleyici araştırması bağlantı noktasında dinleme SQL Server örneğine trafiğini yönlendirir. Genellikle, bir iç yük dengeleyicisi bir kullanılabilirlik grubu kullanır. Azure iç yük dengeleyiciye bir veya daha çok IP adreslerini barındırabilir. Her IP adresi belirli araştırma bağlantı noktasını kullanır. Bu belgede bir yük dengeleyici oluşturmak veya mevcut bir yük dengeleyici SQL Server kullanılabilirlik grupları için IP adresleri eklemek için PowerShell kullanmayı gösterir. 

Bir iç yük dengeleyici için birden çok IP adresi atama olanağı Azure için yenidir ve yalnızca Resource Manager modelinde kullanılabilir. Bu görevi tamamlamak için Resource Manager modeli Azure sanal makinelerinde dağıtılan bir SQL Server kullanılabilirlik grubu olması gerekir. Her iki SQL Server sanal makineleri aynı kullanılabilirlik kümesine ait olması gerekir. Kullanabileceğiniz [Microsoft şablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) otomatik olarak Azure Kaynak Yöneticisi'nde kullanılabilirlik grubu oluşturmak için. Bu şablon, iç yük dengeleyicisi sizin için de dahil olmak üzere kullanılabilirlik grubu otomatik olarak oluşturur. İsterseniz, aşağıdakileri yapabilirsiniz [el ile bir Always On kullanılabilirlik grubu yapılandırmak](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

Bu konu, kullanılabilirlik grupları zaten yapılandırılmış olmasını gerektirir.  

İlgili Konular şunlardır:

* [Azure VM (GUI) AlwaysOn Kullanılabilirlik Grupları Yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Windows Güvenlik Duvarı'nı yapılandırma
Windows Güvenlik Duvarı'nı SQL Server erişime izin verecek şekilde yapılandırın. Güvenlik duvarı kuralları SQL Server örneğini ve dinleyicisi yoklama bağlantı noktalarını kullandığı için TCP bağlantılarını izin verir. Ayrıntılı yönergeler için bkz: [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı Yapılandırma](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Sonda bağlantı noktası ve SQL sunucusu bağlantı noktası için bir gelen kuralı oluşturun.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Örnek komut dosyası: PowerShell ile bir iç yük dengeleyici oluşturma
> [!NOTE]
> Kullanılabilirlik grubu ile oluşturduysanız [Microsoft şablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), iç yük dengeleyicisi zaten oluşturuldu. 
> 
> 

Aşağıdaki PowerShell betiğini bir iç yük dengeleyici oluşturur, Yük Dengeleme kuralları yapılandırır ve yük dengeleyici için bir IP adresini ayarlar. Komut dosyasını çalıştırmak için Windows PowerShell ISE açın ve komut dosyası betik bölmesine yapıştırın. Kullanım `Connect-AzureRmAccount` PowerShell oturum açmak için. Birden çok Azure aboneliğiniz varsa, kullanmak `Select-AzureRmSubscription ` aboneliği ayarlamak için. 

```powershell
# Connect-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a> Örnek komut dosyası: PowerShell ile var olan bir yük dengeleyici için bir IP adresi Ekle
Birden çok kullanılabilirlik grubu kullanmak için ek bir IP adresi yük dengeleyiciye ekleyin. Her IP adresi, kendi Yük Dengeleme kuralı, araştırma bağlantı noktası ve ön bağlantı noktası gerektirir.

Ön uç bağlantı uygulamaları SQL Server örneğine bağlanmak için kullandığınız bağlantı noktasıdır. IP adresleri farklı kullanılabilirlik grupları için aynı ön uç bağlantı noktasını kullanabilirsiniz.

> [!NOTE]
> SQL Server kullanılabilirlik grupları için her IP adresi belirli sonda bağlantı noktası gerektirir. Örneğin, bir IP adresi bir yük dengeleyicideki sonda bağlantı noktası 59999 kullanıyorsa, herhangi bir IP adresi üzerinde bu yük dengeleyici araştırması bağlantı noktası 59999 kullanabilirsiniz.

* Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: **özel ön uç IP yük dengeleyici başına** altında [ağ sınırları - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Kullanılabilirlik grubu sınırları hakkında daha fazla bilgi için bkz: [kısıtlamaları (kullanılabilirlik grupları)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Aşağıdaki komut dosyasını yeni bir IP adresi için var olan bir yük dengeleyici ekler. ILB, yük dengeleyici ön uç bağlantı noktası için dinleyici bağlantı noktasını kullanır. Bu bağlantı noktası, SQL Server üzerinde dinleme bağlantı noktası olabilir. SQL Server'ın varsayılan örnekleri için bağlantı noktası 1433'tür. Arka uç bağlantı noktası ön uç bağlantı noktası ile aynı olacak şekilde yük dengeleme kuralını bir kullanılabilirlik grubu için bir kayan IP (doğrudan sunucu dönüşü) gerektirir. Değişkenleri ortamınız için güncelleştirin. 

```powershell
# Connect-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-the-listener"></a>Dinleyici yapılandırın

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>SQL Server Management Studio'da dinleyici bağlantı noktasını ayarla

1. SQL Server Management Studio'yu başlatın ve birincil kopyaya bağlanın.

1. Gidin **AlwaysOn yüksek kullanılabilirlik** | **kullanılabilirlik grupları** | **kullanılabilirlik grubu dinleyicileri**. 

1. Yük Devretme Kümesi Yöneticisi'nde oluşturulan dinleyici adı görmelisiniz. Dinleyici adına sağ tıklatın ve **özellikleri**.

1. İçinde **bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak için kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarası belirtin (1433 olduğu varsayılan), ardından **Tamam**.

## <a name="test-the-connection-to-the-listener"></a>Dinleyici bağlantıyı sınayın

Bağlantıyı sınamak için:

1. RDP bir SQL Server'a aynı sanal ağda bulunan, ancak çoğaltma kendisine ait değil. Bu kümedeki diğer SQL Server olabilir.

1. Kullanım **sqlcmd** yardımcı programını kullanarak bağlantıyı sınayın. Örneğin, aşağıdaki komut dosyasını oluşturur. bir **sqlcmd** Windows kimlik doğrulaması dinleyicisiyle aracılığıyla birincil çoğaltma bağlantısı:
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    Dinleyici varsayılan dışında bir bağlantı noktası kullanıyorsa (1433) bağlantı noktası, bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komut bir dinleyici bağlantı noktası 1435 bağlanır: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

SQL Server'ın hangi örneğinin birincil çoğaltmayı barındıran için SQLCMD bağlantı otomatik olarak bağlanır. 

> [!NOTE]
> Belirttiğiniz bağlantı noktasının Güvenlik Duvarı'nı her iki SQL sunucularının açık olduğundan emin olun. Her iki sunucuyu kullandığınız TCP bağlantı noktası için bir gelen kuralı gerektirir. Bkz: [Ekle veya güvenlik duvarı kuralını Düzenle](http://technet.microsoft.com/library/cc753558.aspx) daha fazla bilgi için. 
> 
> 

## <a name="guidelines-and-limitations"></a>Kılavuzlar ve sınırlamalar
Kullanılabilirlik grubu dinleyicisi iç kullanarak azure'da aşağıdaki yönergelere yük dengeleyici dikkat edin:

* Bir iç yük dengeleyici ile gelen dinleyicisi aynı sanal ağda yalnızca erişin.


## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
Daha fazla bilgi için bkz: [yapılandırma Always On kullanılabilirlik grubu Azure VM'de el ile](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
Azure sanal makineler için bir iç yük dengeleyici oluşturmak için aşağıdaki PowerShell cmdlet'lerini kullanın.

* [AzureRmLoadBalancer yeni](http://msdn.microsoft.com/library/mt619450.aspx) bir yük dengeleyici oluşturur. 
* [AzureRMLoadBalancerFrontendIpConfig yeni](http://msdn.microsoft.com/library/mt603510.aspx) bir yük dengeleyici için bir ön uç IP yapılandırmasını oluşturur. 
* [AzureRmLoadBalancerRuleConfig yeni](http://msdn.microsoft.com/library/mt619391.aspx) bir yük dengeleyici için bir kural yapılandırma oluşturur. 
* [AzureRmLoadBalancerBackendAddressPoolConfig yeni](http://msdn.microsoft.com/library/mt603791.aspx) bir yük dengeleyici için arka uç adres havuzu yapılandırması oluşturur. 
* [AzureRmLoadBalancerProbeConfig yeni](http://msdn.microsoft.com/library/mt603847.aspx) bir yük dengeleyici için bir araştırma yapılandırma oluşturur.
* [Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) bir yük dengeleyici Azure kaynak grubundan kaldırır.
