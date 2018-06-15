---
title: Azure Sanal Ağları için ortak PowerShell komutlarını | Microsoft Docs
description: Sağlamak için ortak PowerShell komutlarını VM'ler için sanal ağ ve onun ilişkili kaynakları oluşturma başladı.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: a5b3f84c27a0a5f6458808940b16a9001097b30b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526711"
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Azure Sanal Ağları için ortak PowerShell komutları

Bir sanal makine oluşturmak istiyorsanız, oluşturmak gereken bir [sanal ağ](../../virtual-network/virtual-networks-overview.md) veya hangi VM eklenebilir mevcut sanal ağda hakkında bilmeniz. Bir VM oluşturduğunuzda, genellikle, ayrıca bu makalede açıklanan kaynakların oluşturma göz önüne almanız gerekir.

Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Bazı değişkenler birden fazla komutların bu makalede çalıştırılıyorsa sizin için yararlı olabilir:

- $location - ağ kaynaklarına konumu. Kullanabileceğiniz [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) bulmak için bir [coğrafi bölge](https://azure.microsoft.com/regions/) için çalışır.
- $myResourceGroup - ağ kaynaklarına bulunduğu kaynak grubunun adı.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

| Görev | Komut |
| ---- | ------- |
| Alt ağ yapılandırmaları oluşturma |$subnet1 = [AzureRmVirtualNetworkSubnetConfig yeni](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) -"mySubnet1" - AddressPrefix XX adı. X.X.X/XX<BR>$Altağ2 AzureRmVirtualNetworkSubnetConfig yeni =-"mySubnet2" - AddressPrefix XX adı. X.X.X/XX<BR><BR>Tipik bir ağ için bir alt ağ olabilir bir [internet'e yönelik Yük Dengeleyici](../../load-balancer/load-balancer-internet-overview.md) ve ayrı bir alt ağ için bir [iç yük dengeleyici](../../load-balancer/load-balancer-internal-overview.md). |
| Sanal ağ oluşturma |$vnet = [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) -adı "myVNet" - ResourceGroupName $myResourceGroup-konum $location - AddressPrefix XX. X.X.X/XX-alt $subnet1, $Altağ2 |
| Test için benzersiz etki alanı adı |[Test-AzureRmDnsAvailability](https://docs.microsoft.com/powershell/module/azurerm.network/test-azurermdnsavailability) - DomainNameLabel "myDNS"-Konum $location<BR><BR>Bir DNS etki alanı adı için belirttiğiniz bir [genel IP kaynağı](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), Azure tarafından yönetilen DNS sunucuları domainname.location.cloudapp.azure.com ortak IP adresi için bir eşleme oluşturur. Ad yalnızca harf, rakam ve kısa çizgi içerebilir. İlk ve son karakter bir harf olmalıdır veya numarası ve etki alanı adını Azure konumuna içinde benzersiz olmalıdır. Varsa **True** döndürülür, önerilen adınız genel benzersiz. |
| Genel IP adresi oluşturma |$pip = [yeni AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) -- ResourceGroupName $myResourceGroup - DomainNameLabel "myDNS" "myPublicIp" ad-konum $location - AllocationMethod dinamik<BR><BR>Genel IP adresi daha önce test ve yük dengeleyici ön uç yapılandırma tarafından kullanılan etki alanı adını kullanır. |
| Bir ön uç IP yapılandırması oluştur |$frontendIP = [yeni AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) -adı "myFrontendIP" - Publicıpaddress $pip<BR><BR>Ön uç yapılandırma için gelen ağ trafiğini daha önce oluşturduğunuz genel IP adresini içerir. |
| Arka uç adres havuzu oluşturma |$beAddressPool = [yeni AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) -adı "myBackendAddressPool"<BR><BR>Bir ağ arabirimi üzerinden erişilen yük dengeleyici arka uç için iç adresleri sağlar. |
| Bir araştırma oluşturma |$healthProbe = [yeni AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) -adı "myProbe" - RequestPath 'HealthProbe.aspx'-protokolü http-bağlantı noktası 80 - Intervalınseconds 15 - ProbeCount 2<BR><BR>Sanal makineler durumlarda arka uç adres havuzu kullanılabilirliğini denetlemek için kullanılan sistem durumu araştırmalarının içerir. |
| Bir Yük Dengeleme kuralı oluştur |$lbRule = [yeni AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) -adı HTTP - Frontendıpconfiguration $frontendIP - BackendAddressPool $beAddressPool-araştırma $healthProbe-protokol Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Bir bağlantı noktası arka uç adres havuzundaki yük dengeleyici üzerinde genel bir bağlantı noktası atama kurallarını içerir. |
| Gelen NAT kuralı oluştur |$inboundNATRule = [yeni AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) -$frontendIP "myInboundRule1" - Frontendıpconfiguration ad-protokol TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Genel bir bağlantı noktası üzerinde yük dengeleyici arka uç adres havuzundaki belirli bir sanal makine için bir bağlantı noktası eşleme kurallarını içerir. |
| Yük dengeleyici oluşturma |$loadBalancer = [yeni AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) - ResourceGroupName $myResourceGroup-adı "myLoadBalancer"-Konum $location - Frontendıpconfiguration $frontendIP - Inboundnatrule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-araştırma $healthProbe |
| Bir ağ arabirimi oluştur |$nıc1 = [yeni AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) - ResourceGroupName $myResourceGroup-adı "myNIC" - Konum $location - PrivateIpAddress XX. X.X.X-alt $Altağ2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - Ipconfiguration'a $loadBalancer.InboundNatRules[0]<BR><BR>Daha önce oluşturduğunuz sanal ağ alt ağı ve genel IP adresi kullanarak bir ağ arabirimi oluşturur. |

## <a name="get-information-about-network-resources"></a>Ağ kaynakları hakkında bilgi edinin

| Görev | Komut |
| ---- | ------- |
| Sanal ağlar |[Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) - ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm sanal ağları listeler. |
| Bir sanal ağ hakkında bilgi edinin |Get-AzureRmVirtualNetwork-adı "myVNet" - ResourceGroupName $myResourceGroup |
| Bir sanal ağ alt ağların listesi |Get-AzureRmVirtualNetwork-adı "myVNet" - ResourceGroupName $myResourceGroup &#124; alt ağları seçin |
| Bir alt ağ hakkında bilgi edinin |[Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) -$vnet "mySubnet1" - VirtualNetwork adı<BR><BR>Belirtilen sanal ağ alt ağ bilgilerini alır. $Vnet değerini Get-AzureRmVirtualNetwork tarafından döndürülen nesne temsil eder. |
| IP adresleri listesi |[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubu içindeki ortak IP adresleri listelenir. |
| Liste yük Dengeleyiciler |[Get-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermloadbalancer) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm yük dengeleyicileri listeler. |
| Liste ağ arabirimleri |[Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) - ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm ağ arabirimlerini listeler. |
| Bir ağ arabirimi hakkında bilgi edinin |Get-AzureRmNetworkInterface-$myResourceGroup "myNIC" - ResourceGroupName adı<BR><BR>Belirli bir ağ arabiriminde hakkındaki bilgileri alır. |
| Bir ağ arabiriminin IP yapılandırmasını al |[Get-AzureRmNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterfaceipconfig) -Name "myNICIP" -NetworkInterface $nic<BR><BR>Belirtilen ağ arabiriminin IP yapılandırması hakkındaki bilgileri alır. $Nic değerini Get-AzureRmNetworkInterface tarafından döndürülen nesne temsil eder. |

## <a name="manage-network-resources"></a>Ağ kaynaklarını yönetme

| Görev | Komut |
| ---- | ------- |
| Bir sanal ağa bir alt ağ Ekle |[Ekleme AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) - AddressPrefix XX. X.X.X/XX-$vnet "mySubnet1" - VirtualNetwork adı<BR><BR>Bir alt ağı mevcut bir sanal ağa ekler. $Vnet değerini Get-AzureRmVirtualNetwork tarafından döndürülen nesne temsil eder. |
| Bir sanal ağı silme |[Remove-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetwork) -adı "myVNet" - ResourceGroupName $myResourceGroup<BR><BR>Belirtilen sanal ağ kaynak grubundan kaldırır. |
| Bir ağ arabirimi Sil |[Remove-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermnetworkinterface) -$myResourceGroup "myNIC" - ResourceGroupName adı<BR><BR>Belirtilen ağ arabiriminin kaynak grubundan kaldırır. |
| Yük dengeleyici silme |[Remove-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermloadbalancer) -adı "myLoadBalancer" - ResourceGroupName $myResourceGroup<BR><BR>Belirtilen yük dengeleyici kaynak grubundan kaldırır. |
| Bir ortak IP adresini Sil |[Remove-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress)-adı "myIpAddress" - ResourceGroupName $myResourceGroup<BR><BR>Belirtilen ortak IP adresi kaynak grubundan kaldırır. |

## <a name="next-steps"></a>Sonraki Adımlar
* Şimdi ne zaman oluşturduğunuz ağ arabirimini kullanma, [bir VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Hakkında öğrenin [birden çok ağ arabirimi ile bir VM oluşturma](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md).

