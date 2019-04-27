---
title: Azure Sanal Ağları için ortak PowerShell komutlarını | Microsoft Docs
description: Çalışmanız için ortak PowerShell komutları, VM'ler için sanal ağ ve ilişkili kaynakları oluşturma başladı.
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
ms.openlocfilehash: 020f2a4171a5bd656e53c91e59edb16931b20d0d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60597673"
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Azure Sanal Ağları için ortak PowerShell komutları

Bir sanal makine oluşturmak istiyorsanız, oluşturmak gereken bir [sanal ağ](../../virtual-network/virtual-networks-overview.md) veya içinde sanal makine eklenebilir bir sanal ağınız hakkında bilmeniz. Bir VM oluşturduğunuzda, genellikle, ayrıca bu makalede açıklanan kaynakların oluşturma göz önünde bulundurmanız gerekir.

Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Birden fazla komutlar bu makaledeki çalıştırılıyorsa, bazı değişkenler için yararlı olabilir:

- $location - ağ kaynaklarını konumu. Kullanabileceğiniz [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) bulmak için bir [coğrafi](https://azure.microsoft.com/regions/) için çalışır.
- $myResourceGroup - ağ kaynaklarını bulunduğu kaynak grubunun adı.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

| Görev | Komut |
| ---- | ------- |
| Alt ağ yapılandırmaları oluşturma |$subnet1 = [AzVirtualNetworkSubnetConfig yeni](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) -"mySubnet1" - AddressPrefix XX adlandırın. X.X.X/XX<BR>$subnet2 = New-AzVirtualNetworkSubnetConfig -Name "mySubnet2" -AddressPrefix XX.X.X.X/XX<BR><BR>Bir alt ağ için tipik bir ağ olabilir bir [internet'e yönelik Yük Dengeleyici](../../load-balancer/load-balancer-internet-overview.md) ve ayrı bir alt ağ için bir [iç yük dengeleyici](../../load-balancer/load-balancer-internal-overview.md). |
| Sanal ağ oluşturma |$vnet = [yeni AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) -adı "myVNet" - ResourceGroupName $myResourceGroup-konum $location - AddressPrefix XX. X.X.X/XX-alt $subnet1, $subnet2 |
| Test için benzersiz etki alanı adı |[Test-AzDnsAvailability](https://docs.microsoft.com/powershell/module/az.network/test-azdnsavailability) - Etkialanıadetiketi "myDNS"-$location konumu<BR><BR>Bir DNS etki alanı adı için belirttiğiniz bir [genel IP kaynağı](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), Azure tarafından yönetilen DNS sunucuları domainname.location.cloudapp.azure.com genel IP adresine yönelik bir eşleme oluşturur. Ad yalnızca küçük harf, sayı ve kısa çizgi içerebilir. İlk ve son karakterin bir harf olmalıdır veya sayı ile etki alanı adını kendi Azure konumunda benzersiz olmalıdır. Varsa **True** döndürülür, önerilen adınız, genel olarak benzersiz. |
| Genel IP adresi oluşturma |$pip = [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) -Name "myPublicIp" -ResourceGroupName $myResourceGroup -DomainNameLabel "myDNS" -Location $location -AllocationMethod Dynamic<BR><BR>Genel IP adresini daha önce test ve yük dengeleyici ön uç yapılandırması tarafından kullanılan etki alanı adını kullanır. |
| Bir ön uç IP yapılandırması oluştur |$frontendIP = [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) -Name "myFrontendIP" -PublicIpAddress $pip<BR><BR>Ön uç yapılandırması, gelen ağ trafiği için daha önce oluşturduğunuz genel IP adresini içerir. |
| Arka uç adres havuzu oluşturma |$beAddressPool = [yeni AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) -adı "myBackendAddressPool"<BR><BR>Dahili adresleri bir ağ arabirimi üzerinden erişilen yük dengeleyicinin arka uç sağlar. |
| Bir araştırma oluşturma |$healthProbe = [yeni AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) -adı "myProbe" - RequestPath 'HealthProbe.aspx'-protokolü http-bağlantı noktası 80 - İntervalınseconds 15 - ProbeCount 2<BR><BR>Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir. |
| Yük Dengeleme kuralı oluşturma |$lbRule = [yeni AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) -adı HTTP - Frontendıpconfiguration $frontendIP - BackendAddressPool $beAddressPool-araştırma $healthProbe-Protocol Tcp - FrontendPort 80 - Backendport'a 80<BR><BR>Yük Dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına atama kurallarını içerir. |
| Gelen NAT kuralı oluşturma |$inboundNATRule = [yeni AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) -$frontendIP "myInboundRule1" - frontendıpconfiguration'ı ad-Protocol TCP - FrontendPort 3441 - Backendport'a 3389<BR><BR>Yük Dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleme kurallarını içerir. |
| Yük dengeleyici oluşturma |$loadBalancer = [yeni AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) - ResourceGroupName $myResourceGroup-adı "myLoadBalancer"-Konum $location - Frontendıpconfiguration $frontendIP - Inboundnatrule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-$healthProbe araştırma |
| Ağ arabirimini oluşturun |$nic1 = [yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) - ResourceGroupName $myResourceGroup-adı "Mynıc" - Konum $location - Privateıpaddress XX. X.X.X-alt $subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Daha önce oluşturduğunuz sanal ağ alt ağı ve genel IP adresi kullanarak bir ağ arabirimi oluşturun. |

## <a name="get-information-about-network-resources"></a>Ağ kaynakları hakkında bilgi edinin

| Görev | Komut |
| ---- | ------- |
| Sanal ağlar |[Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) - ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm sanal ağlar listelenir. |
| Bir sanal ağ hakkında bilgi edinin |Get-AzVirtualNetwork-adı "myVNet" - ResourceGroupName $myResourceGroup |
| Bir sanal ağ alt ağların listesi |Get-AzVirtualNetwork-adı "myVNet" - ResourceGroupName $myResourceGroup &#124; alt ağları seçin |
| Bir alt ağ hakkında bilgi edinin |[Get-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetworksubnetconfig) -"mySubnet1" - VirtualNetwork $vnet adı<BR><BR>Belirtilen sanal ağ alt ağı hakkında bilgi alır. Get-AzVirtualNetwork tarafından döndürülen nesne $vnet değeri gösterir. |
| IP adreslerini |[Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki genel IP adresleri listelenir. |
| Liste yük Dengeleyiciler |[Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer) - ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm yük dengeleyicileri listeler. |
| Liste ağ arabirimleri |[Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterface) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm ağ arabirimleri listeler. |
| Bir ağ arabirimi hakkında bilgi edinin |Get-AzNetworkInterface-"Mynıc" - ResourceGroupName $myResourceGroup adı<BR><BR>Belirli bir ağ arabirimine hakkındaki bilgileri alır. |
| Bir ağ arabirimi IP Yapılandırması Al |[Get-AzNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterfaceipconfig) -adı "myNICIP" - Networkınterface $nic<BR><BR>Belirtilen ağ arabirimi IP yapılandırması hakkındaki bilgileri alır. $Nic değeri, Get-AzNetworkInterface tarafından döndürülen nesne temsil eder. |

## <a name="manage-network-resources"></a>Ağ kaynaklarını yönetme

| Görev | Komut |
| ---- | ------- |
| Bir sanal ağa bir alt ağ Ekle |[Ekleme AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig) - AddressPrefix XX. X.X.X/XX-"mySubnet1" - VirtualNetwork $vnet adı<BR><BR>Bir alt ağ, mevcut bir sanal ağa ekler. Get-AzVirtualNetwork tarafından döndürülen nesne $vnet değeri gösterir. |
| Bir sanal ağı silme |[Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork) -adı "myVNet" - ResourceGroupName $myResourceGroup<BR><BR>Belirtilen sanal ağ, kaynak grubundan kaldırır. |
| Ağ arabirimini Sil |[Remove-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/remove-aznetworkinterface) -"Mynıc" - ResourceGroupName $myResourceGroup adı<BR><BR>Belirtilen ağ arabirimi, kaynak grubundan kaldırır. |
| Yük dengeleyici silme |[Remove-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer) -adı "myLoadBalancer" - ResourceGroupName $myResourceGroup<BR><BR>Belirtilen yük dengeleyici kaynak grubundan kaldırır. |
| Bir genel IP adresini Sil |[Remove-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress)-Name "myIPAddress" -ResourceGroupName $myResourceGroup<BR><BR>Belirtilen genel IP adresi kaynak grubundan kaldırır. |

## <a name="next-steps"></a>Sonraki Adımlar
* Şimdi ne zaman oluşturduğunuz ağ arabirimini kullanan, [VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Hakkında öğrenin [birden çok ağ arabirimi ile VM oluşturma](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md).

