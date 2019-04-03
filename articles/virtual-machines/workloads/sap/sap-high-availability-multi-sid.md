---
title: Azure'da bir SAP çoklu SID yapılandırmasını oluşturun | Microsoft Docs
description: Yüksek kullanılabilirlik SAP NetWeaver çoklu SID yapılandırma için sanal makineler üzerinde Windows Kılavuzu
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe9b70d74e326166afae366becc47fbcc8b2ea56
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58846687"
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a>Bir SAP NetWeaver çoklu SID yapılandırması oluştur

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

Eylül 2016'da, Microsoft, birden çok sanal IP adresleri Azure iç yük dengeleyici kullanarak yönetebileceğiniz bir özellik yayımladı. Bu işlev Azure dış yük dengeleyicide zaten var.

SAP dağıtımınızı varsa, iç yük dengeleyici SAP ASCS/SCS için bir Windows kümesi yapılandırması oluşturmak için açıklandığı gibi kullanabileceğiniz [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu] [ sap-ha-guide].

Bu makale, mevcut bir Windows Server Yük Devretme Kümelemesi (WSFC) kümesine ek SAP ASCS/SCS kümelenmiş örneklerini yükleyerek tek bir ASCS/SCS yüklemesinden bir SAP çoklu SID yapılandırmasına taşıma odaklanır. Bu işlem tamamlandığında, bir SAP çoklu SID küme yapılandırmış olmanız.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar
Bölümünde açıklandığı gibi bir SAP ASCS/SCS örneği için kullanılan bir WSFC kümesi zaten yapılandırdınız [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu] [ sap-ha-guide] ve bu diyagramda gösterildiği gibi.

![Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a>Hedef mimari

Hedef birden çok SAP ABAP ASCS yüklemektir veya SAP Java SCS örneği aynı WSFC kümesinde, Resimli olarak burada kümelenmiş:

![Azure'da birden çok SAP ASCS/SCS kümelenmiş örneği][sap-ha-guide-figure-6002]

> [!NOTE]
>Her Azure iç yük dengeleyici için özel ön uç IP sayısı için bir sınır yoktur.
>
>SAP ASCS/SCS örneği bir WSFC kümesinde en fazla sayısını özel ön uç IP'ler için her bir Azure iç Yük Dengeleyiciyi maksimum sayısına eşittir.
>

Yük Dengeleyici sınırları hakkında daha fazla bilgi için bkz: "Yük Dengeleyici başına özel ön uç IP" [ağ Limitleri: Azure Resource Manager][networking-limits-azure-resource-manager].

İki yüksek kullanılabilirlik SAP sistemlerini ile tam yatay şöyle görünür:

![SAP çoklu SID yüksek kullanılabilirlik Kurulum iki SAP sistemiyle SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> Kurulum, aşağıdaki koşulları karşılaması gerekir:
> - SAP ASCS/SCS örneği aynı WSFC kümesi paylaşmanız gerekir.
> - Her DBMS SID, kendi özel WSFC kümesine sahip olmalıdır.
> - Bir SAP sistemine SID ait SAP uygulama sunucuları, kendi özel VM'ler olması gerekir.


## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama
Altyapınızı hazırlamak için aşağıdaki parametrelerle bir ek SAP ASCS/SCS örneği yükleyebilirsiniz:

| Parametre adı | Değer |
| --- | --- |
| SAP ASCS/SCS SID |pr1 lb ascs |
| SAP DBMS iç yük dengeleyici | PR5 |
| SAP sanal ana bilgisayar adı | pr5 sap Temizle |
| SAP ASCS/SCS sanal ana bilgisayar IP adresi (ek Azure yük dengeleyici IP adresi) | 10.0.0.50 |
| SAP ASCS/SCS örneği sayısı | 50 |
| ILB araştırma bağlantı noktası ek SAP ASCS/SCS örneği için | 62350 |

> [!NOTE]
> SAP ASCS/SCS kümesi örnekleri için benzersiz bir araştırma bağlantı noktası her IP adresi gerektirir. Örneğin, herhangi bir IP adresi, yük dengeleyicideki bir Azure iç yük dengeleyici üzerindeki bir IP adresi, yoklama bağlantı noktası 62300 kullanıyorsa, yoklama bağlantı noktası 62300 kullanabilirsiniz.
>
>Araştırma bağlantı noktası 62300 zaten ayrılmış olduğundan, amaçlarımız doğrultusunda, yoklama bağlantı noktası 62350 kullanıyoruz.

Ek SAP ASCS/SCS örnekleri, mevcut WSFC kümesinde iki düğüm ile yükleyebilirsiniz:

| Sanal makine rolü | Sanal makine konak adı | Statik IP adresi |
| --- | --- | --- |
| ASCS/SCS örneği için 1. küme düğümü |pr1 ascs 0 |10.0.0.10 |
| ASCS/SCS örneği için 2 küme düğümü |pr1 ascs 1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a>Kümelenmiş SAP ASCS/SCS örneği için bir sanal ana bilgisayar adı DNS sunucusunda oluşturma

Aşağıdaki parametreleri kullanarak sanal ana bilgisayar adı ASCS/SCS örneği için bir DNS girişi oluşturabilirsiniz:

| Yeni SAP ASCS/SCS sanal ana bilgisayar adı | İlişkili IP adresi |
| --- | --- |
|pr5 sap Temizle |10.0.0.50 |

Yeni ana bilgisayar adı ve IP adresi DNS Yöneticisi'nde, aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![Küme sanal adı ve TCP/IP adresi DNS Yöneticisi listesi tanımlanmış DNS girişini yeni SAP ASCS/SCS için vurgulama][sap-ha-guide-figure-6004]

Bir DNS girişi oluşturma yordamı da ana ayrıntılı açıklanan [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu][sap-ha-guide-9.1.1].

> [!NOTE]
> Sanal ana bilgisayar adını ek ASCS/SCS örneği için atadığınız yeni IP adresini SAP Azure yük dengeleyiciye atanan yeni IP adresiyle aynı olmalıdır.
>
>Senaryomuzdaki ise 10.0.0.50 IP adresidir.

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a>PowerShell kullanarak bir IP adresi için var olan bir Azure iç yük dengeleyici Ekle

Birden fazla SAP ASCS/SCS örneği aynı WSFC kümesinde oluşturmak için bir IP adresi için var olan bir Azure iç yük dengeleyici eklemek için PowerShell kullanın. Her IP adresi, kendi Yük Dengeleme kuralları, araştırma bağlantı noktasını, ön uç IP havuzu ve arka uç havuzu gerektirir.

Aşağıdaki komut dosyasını yeni bir IP adresi için var olan bir yük dengeleyici ekler. PowerShell benzeri değişkenleri ortamınız için güncelleştirin. Betik, tüm SAP ASCS/SCS bağlantı noktaları için gereken tüm Yük Dengeleme kuralları oluşturur.

```powershell

# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure VNet and subnet
$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzLoadBalancer

# Get new updated configuration
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzLoadBalancer

# Get new updated config
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to backend pool
$BEPool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzNetworkInterface -NetworkInterface $NIC
        #start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzLoadBalancer

Write-Host "Successfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
Betiği çalıştırdıktan sonra sonuçları aşağıdaki ekran görüntüsünde gösterildiği gibi Azure Portalı'nda görüntülenir:

![Azure portalında yeni ön uç IP havuzu][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a>Küme makinelere disk ekleyin ve SIOS Küme Paylaşımı disk yapılandırma

Her ek SAP ASCS/SCS örneği için yeni bir küme paylaşımı disk eklemeniz gerekir. Windows Server 2012 R2 için WSFC Küme Paylaşımı disk şu anda kullanımda SIOS DataKeeper yazılım çözümüdür.

Şunları yapın:
1. Ek bir disk veya diskler (gereken stripe) aynı boyutta, her küme düğümlerine ekleyin ve biçimlendirir.
2. Depolama çoğaltma, SIOS DataKeeper ile yapılandırın.

Bu yordam WSFC küme makinelerde SIOS DataKeeper zaten yüklediğinizi varsayar. Yüklediyseniz, çoğaltma makineler arasında şimdi yapılandırmanız gerekir. İşlem, ana ayrıntı açıklanan [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu][sap-ha-guide-8.12.3.3].  

![Yeni SAP ASCS/SCS disk paylaşmak için yansıtma zaman uyumlu DataKeeper][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>SAP uygulama sunucuları ve DBMS küme için Vm'leri dağıtma

İkinci SAP sistemine altyapı hazırlığı tamamlamak için aşağıdakileri yapın:

1. SAP uygulama sunucuları için özel VM'ler dağıtın ve bunları kendi adanmış bir kullanılabilirlik grubuna ekleyin.
2. DBMS kümesi için ayrılmış Vm'leri dağıtma ve bunları kendi adanmış bir kullanılabilirlik grubuna ekleyin.


## <a name="install-the-second-sap-sid2-netweaver-system"></a>İkinci SID2 NetWeaver SAP sistemini yükleyin

İkinci bir SAP SID2 sistemi yüklemeden tam işleminin ana açıklanan [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu][sap-ha-guide-9].

Üst düzey yordam aşağıdaki gibidir:

1. [SAP ilk küme düğümüne yükleme][sap-ha-guide-9.1.2].  
 Bu adımda, SAP bir yüksek kullanılabilirlik ASCS/SCS örneği üzerinde yüklemekte olduğunuz **mevcut WSFC küme düğümü 1**.

2. [SAP ASCS/SCS örneği profilini değiştirmek][sap-ha-guide-9.1.3].

3. [Bir araştırma bağlantı noktasını yapılandırma][sap-ha-guide-9.1.4].  
 Bu adımda, PowerShell kullanarak SAP küme kaynağı SAP SID2 IP araştırma bağlantı noktası yapılandırmış olursunuz. Bu yapılandırma, SAP ASCS/SCS küme düğümlerinden biri üzerinde çalıştırın.

4. [Veritabanı örneğini yükleme] [sap-ha-guide-9.2].  
 Bu adımda, adanmış bir WSFC kümesinde DBMS yüklüyorsunuz.

5. [İkinci küme düğümüne yükleme] [sap-ha-guide-9.3].  
 Bu adımda, bir yüksek kullanılabilirlik ASCS/SCS örneği mevcut WSFC küme düğümünde 2 ile SAP yüklüyorsunuz.

6. ProbePort ve SAP ASCS/SCS örneği için Windows Güvenlik Duvarı bağlantı noktalarını açın.  
 SAP ASCS/SCS örneği için kullanılan her iki küme düğümleri üzerinde SAP ASCS/SCS tarafından kullanılan tüm Windows Güvenlik Duvarı bağlantı noktaları açıyoruz. Bu bağlantı noktaları listelenir [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu][sap-ha-guide-8.8].  
 Ayrıca senaryomuzdaki 62350 olan Azure iç yük dengeleyici araştırma bağlantı noktasını açın.

7. [SAP Ağıranlar Windows hizmet örneği başlangıç türünü değiştirme][sap-ha-guide-9.4].

8. [SAP birincil uygulama sunucusu yükleme] [ sap-ha-guide-9.5] yeni VM'nin ayrılmış.

9. [SAP ek uygulama sunucusu yükleme] [ sap-ha-guide-9.6] yeni VM'nin ayrılmış.

10. [SIOS çoğaltma ve SAP ASCS/SCS örneği yük devretme testi][sap-ha-guide-10].

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ Limitleri: Azure Resource Manager][networking-limits-azure-resource-manager]
- [Azure için birden çok Vıp'de yük dengeleyici][load-balancer-multivip-overview]
- [Windows vm'lerinde SAP NetWeaver yüksek kullanılabilirlik Kılavuzu][sap-ha-guide]
