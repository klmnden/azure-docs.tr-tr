---
title: "SAP çoklu SID yapılandırma oluşturma | Microsoft Docs"
description: "Yüksek kullanılabilirlik SAP NetWeaver çoklu SID yapılandırma için Windows sanal makineleri Kılavuzu"
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b48df78df9f53ac7bf0804f55a8d36a2fe2f86b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a>SAP NetWeaver çoklu SID yapılandırması oluştur

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

Eylül 2016'da, Microsoft, birden çok sanal IP adreslerini Azure iç yük dengeleyicisi kullanarak yönetebileceğiniz bir özellik yayımladı. Bu işlev Azure dış yük dengeleyicide zaten var.

Bir SAP dağıtımınız varsa, bir iç yük dengeleyici SAP ASCS/SCS için bir Windows Küme yapılandırması oluşturmak için açıklandığı gibi kullanabileceğiniz [yüksek kullanılabilirlik SAP NetWeaver Kılavuzu Windows vm'lerde][sap-ha-guide].

Bu makalede, varolan bir Windows Server Yük Devretme Kümelemesi (WSFC) kümesine ek SAP ASCS/SCS kümelenmiş örneklerini yükleyerek tek bir ASCS/SCS yüklemesinden bir SAP çoklu SID yapılandırmasına taşıma odaklanır. Bu işlem tamamlandığında, bir SAP çoklu SID küme yapılandırılmış.


## <a name="prerequisites"></a>Ön koşullar
Zaten bir SAP ASCS/SCS örnek için kullanılan bir WSFC kümesinin anlatıldığı gibi yapılandırdığınız [yüksek kullanılabilirlik SAP NetWeaver Kılavuzu Windows vm'lerde] [ sap-ha-guide] ve bu diyagramda gösterildiği gibi.

![Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a>Hedef mimari

Hedef birden çok SAP ABAP ASCS yüklemektir veya SAP Java SCS burada Resimli olarak aynı WSFC kümesinde örneklerinin kümelenmiş:

![Azure birden çok SAP ASCS/SCS kümelenmiş örneğinde][sap-ha-guide-figure-6002]

> [!NOTE]
>Her Azure iç yük dengeleyici için özel ön uç IP sayısı için bir sınır yoktur.
>
>Bir WSFC kümesinin SAP ASCS/SCS örneği maksimum sayısı üst sınırını her Azure iç yük dengeleyici için özel ön uç IP eşittir.
>

Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: "özel ön uç IP yük dengeleyici başına" [ağ sınırları: Azure Resource Manager][networking-limits-azure-resource-manager].

İki yüksek kullanılabilirlik SAP sistemleriyle tam yatay şöyle olabilir:

![SAP yüksek kullanılabilirlik multi-SID Kurulum iki SAP sistemiyle SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> Kurulum, aşağıdaki koşulları karşılaması gerekir:
> - SAP ASCS/SCS örnekleri aynı WSFC küme paylaşması gerekir.
> - Her DBMS SID kendi adanmış WSFC kümesi olması gerekir.
> - Bir SAP sistem SID'si ait SAP uygulama sunucuları, kendi özel VM'ler olması gerekir.


## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama
Altyapınızı hazırlamak için aşağıdaki parametrelerle ek SAP ASCS/SCS örneğini yükleyebilirsiniz:

| Parametre adı | Değer |
| --- | --- |
| SAP ASCS/SCS SID |pr1 lb ascs |
| SAP DBMS iç yük dengeleyici | PR5 |
| SAP sanal ana bilgisayar adı | pr5 sap cl |
| SAP ASCS/SCS sanal ana bilgisayar IP adresi (ek Azure yük dengeleyici IP adresi) | 10.0.0.50 |
| SAP ASCS/SCS örnek sayısı | 50 |
| Ek SAP ASCS/SCS örnek için ILB araştırmasını bağlantı noktası | 62350 |

> [!NOTE]
> SAP ASCS/SCS küme örnekleri için her IP adresi benzersiz sonda bağlantı noktası gerektirir. Örneğin, bir IP adresinde bir Azure iç yük dengeleyici araştırması bağlantı noktası 62300 kullanıyorsa, herhangi bir IP adresi, yük dengeleyicide sonda bağlantı noktası 62300 kullanabilirsiniz.
>
>Sonda bağlantı noktası 62300 zaten ayrılmış olduğundan bizim amaçlar için araştırma bağlantı noktası 62350 kullanıyoruz.

Mevcut WSFC kümesinde iki düğümü ek SAP ASCS/SCS örnekleri yükleyebilirsiniz:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Statik IP adresi |
| --- | --- | --- |
| 1. küme düğümü ASCS/SCS örneği için |pr1 ascs 0 |10.0.0.10 |
| ASCS/SCS örneği için 2 küme düğümü |pr1 ascs 1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a>Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı DNS sunucusunda oluşturun

Aşağıdaki parametreleri kullanarak ASCS/SCS örneğinin sanal ana bilgisayar adı için bir DNS girişi oluşturabilirsiniz:

| Yeni SAP ASCS/SCS sanal ana bilgisayar adı | İlişkili IP adresi |
| --- | --- | --- |
|pr5 sap cl |10.0.0.50 |

Yeni ana bilgisayar adı ve IP adresi DNS Yöneticisi'nde, aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![DNS Yöneticisi listesini yeni SAP ASCS/SCS için tanımlanmış DNS girişi vurgulama küme sanal adı ve TCP/IP adresi][sap-ha-guide-figure-6004]

Bir DNS girişi oluşturma yordamı da ana ayrıntılı açıklanan [yüksek kullanılabilirlik SAP NetWeaver Kılavuzu Windows vm'lerde][sap-ha-guide-9.1.1].

> [!NOTE]
> Ek ASCS/SCS örneğinin sanal ana bilgisayar adı atamak yeni IP adresi SAP Azure yük dengeleyiciye atanan yeni IP adresi ile aynı olması gerekir.
>
>Senaryomuzda 10.0.0.50 IP adresidir.

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a>PowerShell kullanarak bir IP adresi var olan bir Azure iç yük dengeleyici ekleme

Aynı WSFC kümesinde birden fazla SAP ASCS/SCS örneği oluşturmak için bir IP adresi var olan bir Azure iç yük dengeleyici eklemek için PowerShell kullanın. Her IP adresi, kendi Yük Dengeleme kuralları, araştırma bağlantı noktası, ön uç IP havuzu ve arka uç havuzu gerektirir.

Aşağıdaki komut dosyasını yeni bir IP adresi için var olan bir yük dengeleyici ekler. Ortamınız için PowerShell değişkenleri güncelleştirin. Komut dosyası tüm SAP ASCS/SCS bağlantı noktaları için gereken tüm Yük Dengeleme kuralları oluşturacaksınız.

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to backend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
Betik çalıştıktan sonra sonuçları aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında görüntülenir:

![Azure portalında yeni ön uç IP havuzu][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a>Diskleri küme makinelere ekleyin ve SIOS küme paylaşım disk yapılandırma

Her ek SAP ASCS/SCS örneği için yeni bir küme paylaşım disk eklemeniz gerekir. Windows Server 2012 R2 için WSFC küme paylaşım disk şu anda kullanımda SIOS DataKeeper yazılım çözümüdür.

Şunları yapın:
1. Bir ek disk veya diskler (hangi şeritler gerek) aynı boyutta her küme düğümleri eklemek ve biçimlendirir.
2. Depolama çoğaltma SIOS DataKeeper ile yapılandırın.

Bu yordam WSFC küme makinelerde SIOS DataKeeper zaten yüklediyseniz varsayar. Yüklediyseniz, makineler arası çoğaltma şimdi yapılandırmanız gerekir. İşlem ana ayrıntılı açıklanan [yüksek kullanılabilirlik SAP NetWeaver Kılavuzu Windows vm'lerde][sap-ha-guide-8.12.3.3].  

![DataKeeper eş zamanlı disk yeni SAP ASCS/SCS paylaşmak için yansıtma][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>SAP uygulama sunucuları ve DBMS küme için sanal makineleri dağıtma

Altyapı hazırlık ikinci SAP sistemi için tamamlamak için aşağıdakileri yapın:

1. SAP uygulama sunucuları için özel VM'ler dağıtın ve bunları kendi özel kullanılabilirlik grubunda yerleştirin.
2. DBMS küme için özel VM'ler dağıtın ve bunları kendi özel kullanılabilirlik grubunda yerleştirin.


## <a name="install-the-second-sap-sid2-netweaver-system"></a>İkinci SAP SID2 NetWeaver sistemini yükleyin

İkinci bir SAP SID2 sistemi yükleme tam işlemi ana açıklanan [yüksek kullanılabilirlik SAP NetWeaver Kılavuzu Windows vm'lerde][sap-ha-guide-9].

Üst düzey yordam aşağıdaki gibidir:

1. [SAP ilk küme düğümüne yüklemek][sap-ha-guide-9.1.2].  
 Bu adımda, SAP bir yüksek kullanılabilirlik ASCS/SCS örneğiyle üzerinde yüklemekte olduğunuz **mevcut WSFC küme düğümü 1**.

2. [ASCS/SCS örneğinin SAP profilini değiştirmek][sap-ha-guide-9.1.3].

3. [Bir araştırma bağlantı noktası yapılandırın][sap-ha-guide-9.1.4].  
 Bu adımda, PowerShell kullanarak bir SAP küme kaynağı SAP SID2 IP sonda bağlantı noktası yapılandırmış olursunuz. Bu yapılandırma SAP ASCS/SCS küme düğümlerinden birinin yürütün.

4. [Veritabanı örneği yükleme] [sap-ha-Kılavuzu-9.2].  
 Bu adımda, adanmış bir WSFC kümesinde DBMS yüklüyorsunuz.

5. [İkinci küme düğümü yükleme] [sap-ha-Kılavuzu-9.3].  
 Bu adımda, mevcut WSFC Küme düğüm 2 üzerinde bir yüksek kullanılabilirlik ASCS/SCS örneği SAP yüklüyorsunuz.

6. SAP ASCS/SCS örneği ve ProbePort için Windows Güvenlik Duvarı bağlantı noktalarını açın.  
 SAP ASCS/SCS örnekleri için kullanılan her iki küme düğümlerinde SAP ASCS/SCS tarafından kullanılan tüm Windows Güvenlik Duvarı bağlantı noktaları açıyorsunuz. Bu bağlantı noktaları listelenir [yüksek kullanılabilirlik SAP NetWeaver Kılavuzu Windows vm'lerde][sap-ha-guide-8.8].  
 Ayrıca bizim senaryoda 62350 olan Azure iç yük dengeleyici araştırması bağlantı açın.

7. [SAP ERS Windows hizmet örneği başlangıç türünü değiştirme][sap-ha-guide-9.4].

8. [SAP birincil uygulama sunucusu yüklemeniz] [ sap-ha-guide-9.5] yeni VM ayrılmış.

9. [SAP ek uygulama sunucusu yüklemeniz] [ sap-ha-guide-9.6] yeni VM ayrılmış.

10. [SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi][sap-ha-guide-10].

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ sınırları: Azure Resource Manager][networking-limits-azure-resource-manager]
- [Azure için birden çok Vip yük dengeleyici][load-balancer-multivip-overview]
- [Windows sanal makineleri üzerinde yüksek kullanılabilirlik SAP NetWeaver Kılavuzu][sap-ha-guide]
