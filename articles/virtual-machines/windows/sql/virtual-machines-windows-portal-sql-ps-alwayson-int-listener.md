---
title: Her zaman üzerinde kullanılabilirlik grubu dinleyicisi – Microsoft Azure'ı yapılandırma | Microsoft Docs
description: Bir veya daha fazla IP adresi ile iç yük dengeleyici kullanarak Azure Resource Manager modeli, kullanılabilirlik grubu dinleyicisi yapılandırma.
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
ms.date: 02/06/2019
ms.author: mikeray
ms.openlocfilehash: 5b647af7925ceb81c524deb0accf90f9e895080e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66165780"
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Bir veya daha fazla Always On kullanılabilirlik grubu dinleyicisi - Resource Manager'ı yapılandırma
Bu konu başlığı altında gösterilir nasıl yapılır:

* İç yük dengeleyici için PowerShell cmdlet'lerini kullanarak SQL Server kullanılabilirlik gruplarını oluşturun.
* Birden fazla kullanılabilirlik grubu için bir yük dengeleyiciye ek IP adreslerini ekleyin. 

Bir kullanılabilirlik grubu dinleyicisi veritabanı erişimi için istemcilerin bağlandığı bir sanal ağ adıdır. Azure sanal makinelerinde yük dengeleyici için dinleyici IP adresini tutar. Yük dengeleyicinin trafiği araştırma bağlantı noktasında dinleme SQL Server örneği için yönlendirir. Genellikle, bir kullanılabilirlik grubuna bir iç yük dengeleyici kullanır. Azure iç yük dengeleyici, bir veya daha çok IP adresleri barındırabilirsiniz. Her IP adresi, bir özel araştırma bağlantı noktasını kullanır. Bu belge, bir yük dengeleyici oluşturma veya SQL Server kullanılabilirlik grupları için mevcut bir yük dengeleyici IP adreslerini eklemek için PowerShell kullanma işlemi gösterilmektedir. 

İç yük dengeleyici için birden çok IP adresi atama özelliği Azure'a yeni ve yalnızca Resource Manager modelinde kullanılabilir. Bu görevi tamamlamak için Resource Manager modelinde Azure sanal makinelerinde dağıtılan bir SQL Server kullanılabilirlik grubu olması gerekir. Her iki SQL Server sanal makineleri aynı kullanılabilirlik kümesine ait olmalıdır. Kullanabileceğiniz [Microsoft şablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) otomatik olarak Azure Resource Manager, kullanılabilirlik grubunu oluşturun. Bu şablon, sizin için iç yük dengeleyicisi dahil, bir kullanılabilirlik grubu otomatik olarak oluşturur. İsterseniz, aşağıdakileri yapabilirsiniz [el ile Always On kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

Bu konuda, kullanılabilirlik gruplarını zaten yapılandırılmış olmasını gerektirir.  

İlgili Konular şunlardır:

* [Azure VM'de (GUI) AlwaysOn Kullanılabilirlik Grupları Yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="verify-powershell-version"></a>PowerShell sürümünü doğrula

Bu makaledeki örneklerde, Azure PowerShell modülünün 5.4.1 kullanarak test edilmez.

Doğrulayın, PowerShell modülü 5.4.1 veya üzeri.

Bkz: [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="configure-the-windows-firewall"></a>Windows Güvenlik duvarını yapılandırma

Windows Güvenlik Duvarı'nı SQL Server erişimine izin verecek şekilde yapılandırın. Güvenlik duvarı kuralları, SQL Server örneği ve dinleyici araştırma bağlantı noktalarını kullandığı için TCP bağlantılara izin verin. Ayrıntılı yönergeler için bkz. [veritabanı altyapısı erişimi için Windows Güvenlik duvarını yapılandırma](https://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). SQL Server bağlantı noktası için ve araştırma bağlantı noktası için bir gelen kuralı oluşturun.

Eğer bir Azure ağ güvenlik grubu ile erişimi kısıtlama olun arka uç SQL Server VM IP adreslerine izin verme kuralları içerir ve yük dengeleyici kayan IP adresleri /AG dinleyicisi ve küme çekirdek IP adresi için varsa.

## <a name="determine-the-load-balancer-sku-required"></a>Gerekli SKU yük dengeleyici belirleme

[Azure yük dengeleyici](../../../load-balancer/load-balancer-overview.md) 2 Sku'da kullanılabilir: Temel ve standart. Standart load balancer önerilir. Temel yük dengeleyici, bir kullanılabilirlik kümesindeki sanal makineler varsa izin verilir. Standart load balancer, tüm sanal makine IP adresleri standart IP adreslerini kullanmanız gerekir.

Geçerli [Microsoft şablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) için bir kullanılabilirlik grubu temel IP adreslerine sahip bir temel yük dengeleyici kullanır.

Bu makaledeki örneklerde standart load balancer'ı belirtin. Örneklerde, komut dosyasını içeren `-sku Standard`.

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe -sku Standard
```

Temel yük dengeleyici oluşturmak için kaldırmak `-sku Standard` satırından yük dengeleyici oluşturur. Örneğin:

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe
```

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Örnek betiği: PowerShell ile iç yük dengeleyici oluşturma

> [!NOTE]
> Kullanılabilirlik grubunuzun oluşturduysanız [Microsoft şablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), iç load balancer'ın zaten oluşturuldu.

Aşağıdaki PowerShell betiğini bir iç yük dengeleyici oluşturur, Yük Dengeleme kuralları yapılandırır ve bir IP adresi yük dengeleyici için ayarlar. Betiği çalıştırmak için Windows PowerShell ISE'yi açın ve komut dosyası betik bölmesine yapıştırın. Kullanım `Connect-AzAccount` PowerShell oturum açmak için. Birden çok Azure aboneliğiniz varsa, `Select-AzSubscription` aboneliği ayarlamak için. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

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

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzNetworkInterface -NetworkInterface $NIC
        start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a> Örnek betiği: PowerShell ile mevcut bir yük dengeleyici için bir IP adresi ekleyin
Birden fazla kullanılabilirlik grubunu kullanmak üzere ek bir IP adresi yük dengeleyiciye ekleyin. Her IP adresi, kendi Yük Dengeleme kuralı, araştırma bağlantı noktasını ve ön bağlantı noktası gerektirir.

Ön uç bağlantı noktası, uygulamaların SQL Server örneğine bağlanmak için kullandığı bağlantı noktasıdır. Farklı kullanılabilirlik grupları için IP adresleri aynı ön uç bağlantı noktasını kullanabilirsiniz.

> [!NOTE]
> SQL Server kullanılabilirlik grupları için özel bir araştırma bağlantı noktası her IP adresi gerektirir. Örneğin, bir yük dengeleyicideki bir IP adresi, yoklama bağlantı noktası 59999 kullanıyorsa, bu yük dengeleyici üzerindeki diğer IP adresi yok araştırma bağlantı noktası 59999 kullanabilirsiniz.

* Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz. **yük dengeleyici başına özel ön uç IP** altında [ağ limitleri - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Kullanılabilirlik grubu sınırları hakkında daha fazla bilgi için bkz. [kısıtlamaları (kullanılabilirlik grupları)](https://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Aşağıdaki komut dosyasını yeni bir IP adresi için var olan bir yük dengeleyici ekler. ILB dinleyicisi bağlantı noktası, Yük Dengeleme ön uç bağlantı noktası için kullanır. Bu bağlantı noktası, SQL sunucusunun dinleme yaptığı bağlantı noktası olabilir. SQL Server'ın varsayılan örnekleri için bağlantı noktası 1433'dür. Ön uç bağlantı noktasıyla aynı arka uç bağlantı noktası, bu nedenle, Yük Dengeleme kuralını kullanılabilirlik grubu için bir kayan IP (doğrudan sunucu dönüşü) gerektirir. Benzeri değişkenleri ortamınız için güncelleştirin. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzLoadBalancer 

$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzLoadBalancer   
```

## <a name="configure-the-listener"></a>Dinleyici yapılandırma

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>SQL Server Management Studio'da dinleyici bağlantı noktasını ayarlayın

1. SQL Server Management Studio'yu başlatın ve birincil kopyaya bağlanın.

1. Gidin **AlwaysOn yüksek kullanılabilirlik** | **kullanılabilirlik grupları** | **kullanılabilirlik grubu dinleyicisi**. 

1. Yük Devretme Kümesi Yöneticisi'nde, oluşturduğunuz adlı dinleyiciyi görmelisiniz. Dinleyici adına sağ tıklayıp **özellikleri**.

1. İçinde **bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak için kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarası belirtin (1433 varsayılan olduğu), ardından **Tamam**.

## <a name="test-the-connection-to-the-listener"></a>Sınama bağlantısı dinleyicisi

Bağlantıyı test etmek için:

1. RDP bir SQL Server'a aynı sanal ağda bulunan, ancak çoğaltma sahibi değildir. Bu, bir SQL Server kümesinde olabilir.

1. Kullanım **sqlcmd** yardımcı programını kullanarak bağlantıyı test edin. Örneğin, aşağıdaki komut dosyası oluşturur bir **sqlcmd** Windows kimlik doğrulaması ile dinleyicisi aracılığıyla birincil kopyanın bağlantısı:
   
    ```
    sqlcmd -S <listenerName> -E
    ```
   
    Dinleyici varsayılan dışında bir bağlantı noktası kullanıyorsa (1433) bağlantı noktası, bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komutunu bir dinleyici bağlantı noktası 1435 bağlanır: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Hangi SQL Server örneğini birincil çoğaltmayı barındıran için SQLCMD bağlantı otomatik olarak bağlanır. 

> [!NOTE]
> Belirttiğiniz bağlantı noktası hem de SQL Server güvenlik duvarının açık olduğundan emin olun. Her iki sunucuyu kullandığınız TCP bağlantı noktası için bir gelen kuralı gerektirir. Bkz: [Ekle veya güvenlik duvarı kuralını Düzenle](https://technet.microsoft.com/library/cc753558.aspx) daha fazla bilgi için. 
> 
> 

## <a name="guidelines-and-limitations"></a>Kılavuzlar ve sınırlamalar
Yük Dengeleyici dahili kullanarak azure'da kullanılabilirlik grubu dinleyicisi aşağıdaki yönergeleri göz önünde bulundurun:

* Bir iç yük dengeleyiciyle yalnızca aynı sanal ağda dinleyicisinden erişin.

* Eğer bir Azure ağ güvenlik grubu ile erişimi kısıtlama olun arka uç SQL Server VM IP adreslerine izin verme kuralları içerir ve yük dengeleyici kayan IP adresleri /AG dinleyicisi ve küme çekirdek IP adresi için varsa.

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
Daha fazla bilgi için [yapılandırma Always On kullanılabilirlik grubu Azure VM'de el ile](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
Azure sanal makineler için iç yük dengeleyici oluşturmak için aşağıdaki PowerShell cmdlet'lerini kullanın.

* [Yeni AzLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) bir yük dengeleyici oluşturur. 
* [Yeni AzLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) bir yük dengeleyici için bir ön uç IP yapılandırması oluşturur. 
* [Yeni AzLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) bir yük dengeleyici kuralı yapılandırması oluşturur. 
* [Yeni AzLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) bir yük dengeleyici için bir arka uç adres havuzu yapılandırması oluşturur. 
* [Yeni AzLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) bir yük dengeleyici için bir araştırma yapılandırması oluşturur.
* [Remove-AzLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) bir yük dengeleyici bir Azure kaynak grubundan kaldırır.
