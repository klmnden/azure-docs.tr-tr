---
title: Azure özel yollar, zorlamalı tünel ile KMS etkinleştirmesi kullanmayı | Microsoft Docs
description: Azure özel yollar kullanarak Azure'da zorlamalı, KMS etkinleştirmesi için nasıl kullanılacağını gösterir.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: 6557649eb1b97ad4d88876906737f8249e18b958
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399797"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Zorlamalı tünel senaryoda Windows etkinleştirme başarısız

Bu makalede çözümlemek siteden siteye VPN bağlantısı veya ExpressRoute senaryoları, etkinleştirdiğinizde karşılaşabileceğiniz KMS etkinleştirme sorun zorlamalı açıklar.

## <a name="symptom"></a>Belirti

Etkinleştirdiğiniz [zorlamalı tünel](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) Azure'da, şirket içi ağınıza tüm İnternet'e bağlı trafiği yönlendirmek için sanal ağ alt ağları yedekleyin. Bu senaryoda, Azure Windows Server 2012 R2 (veya sonraki Windows sürümlerinde) çalışan sanal makineleri (VM'ler) başarıyla Windows etkinleştirebilirsiniz. Ancak, önceki bir Windows sürümünü çalıştıran sanal makineler Windows etkinleştirme başarısız.

## <a name="cause"></a>Nedeni

Azure Windows sanal makinelerinin Windows etkinleştirme için Azure KMS sunucusuna bağlanmanız gerekmez. Etkinleştirme, etkinleştirme isteği bir Azure genel IP adresinden gelen gerektiriyor. Etkinleştirme isteği, şirket içi ağdan Azure genel bir IP adresinden gelen geldiğinden zorlamalı tünel oluşturma senaryosunda etkinleştirme başarısız olur.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için Azure özel rota için rota etkinleştirme trafiği Azure KMS sunucusunu kullanın.

23.102.135.246 Azure genel bulut için KMS sunucunun IP adresidir. DNS kms.core.windows.net adıdır. Azure Almanya gibi diğer Azure platformları kullanırsanız, karşılık gelen bir KMS sunucusu IP adresini kullanmanız gerekir. Daha fazla bilgi için aşağıdaki tabloya bakın:

|Platform| KMS DNS|KMS IP|
|------|-------|-------|
|Azure genel|KMS.Core.Windows.NET|23.102.135.246|
|Azure Almanya|kms.core.cloudapi.de|51.4.143.248|
|Azure US Government|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure Çin 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Özel rota eklemek için aşağıdaki adımları izleyin:

### <a name="for-resource-manager-vms"></a>Kaynak Yöneticisi VM'ler için

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

1. Azure PowerShell'i açın ve ardından [Azure aboneliğinizde oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Aşağıdaki komutları çalıştırın:

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable

    # Next, attach the route table to the subnet that hosts the VMs

    Set-AzVirtualNetworkSubnetConfig -Name "Subnet01" -VirtualNetwork $vnet -AddressPrefix "10.0.0.0/24" -RouteTable $RouteTable

    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```
3. Etkinleştirme sorunlarını olan sanal Makineye gidin. Kullanım [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) KMS sunucusunda erişebiliyorsa test etmek için:

        psping kms.core.windows.net:1688

4. Windows etkinleştirme ve sorunun çözülüp çözülmediğine bakın deneyin.

### <a name="for-classic-vms"></a>Klasik VM'ler için

1. Azure PowerShell'i açın ve ardından [Azure aboneliğinizde oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Aşağıdaki komutları çalıştırın:

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Etkinleştirme sorunlarını olan sanal Makineye gidin. Kullanım [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) KMS sunucusunda erişebiliyorsa test etmek için:

        psping kms.core.windows.net:1688

4. Windows etkinleştirme ve sorunun çözülüp çözülmediğine bakın deneyin.

## <a name="next-steps"></a>Sonraki adımlar

- [KMS istemcisi kurulum anahtarları](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Gözden geçirme ve seçme etkinleştirme yöntemleri](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)
