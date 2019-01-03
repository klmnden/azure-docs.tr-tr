---
title: Azure özel yollar, zorlamalı tünel ile KMS etkinleştirmesi kullanmayı | Microsoft Docs
description: Azure özel yollar Azure'da zorlamalı tünel ile KMS etkinleştirmesi için nasıl kullanılacağını gösterir.
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
ms.openlocfilehash: f1e2ab6a954361a7807d78dc2baf5d24af52a679
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53798209"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Zorlamalı tünel senaryoda Windows etkinleştirme başarısız

Bu makalede çözümlemek siteden siteye VPN bağlantısı veya ExpressRoute senaryoları, etkinleştirildiğinde karşılaşabileceğiniz KMS etkinleştirme sorun zorlamalı açıklar.

## <a name="symptom"></a>Belirti

Etkinleştirdiğiniz [zorlamalı tünel](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) Azure'da, şirket içi ağınıza tüm İnternet'e bağlı trafiği yönlendirmek için sanal ağ alt ağları yedekleyin. Bu senaryoda, Azure Windows Server 2012 R2 veya sonraki sürümleri çalıştıran sanal makineleri (VM) başarıyla Windows etkinleştirebilirsiniz. Ancak, önceki bir Windows sürümünü çalıştıran Vm'lere Windows etkinleştirme başarısız. 

## <a name="cause"></a>Nedeni

Azure Windows sanal makinelerinin Windows etkinleştirme için Azure KMS sunucusuna bağlanmak. Etkinleştirme, etkinleştirme isteği bir Azure genel IP adresi gelmelidir gerektiriyor. Azure genel IP'LERİNDEN yerine, şirket içi ağ üzerinden etkinleştirme isteği olduğundan zorlamalı tünel oluşturma senaryosunda etkinleştirme başarısız olur. 

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için Azure özel rota için rota etkinleştirme trafiği Azure KMS sunucusuna (23.102.135.246) kullanın. 

IP adresi 23.102.135.246 Azure genel bulut için KMS sunucusunda IP adresidir. DNS kms.core.windows.net adıdır. Azure Almanya gibi diğer Azure platformları kullanırsanız correspond KMS sunucusunun IP adresini kullanmanız gerekir. Daha fazla bilgi için aşağıdaki tabloya bakın:

|Platform| KMS DNS|KMS IP|
|------|-------|-------|
|Azure genel|KMS.Core.Windows.NET|23.102.135.246|
|Azure Almanya|KMS.Core.cloudapi.de|51.4.143.248|
|Azure US Government|KMS.Core.usgovcloudapi.NET|23.97.0.13|
|Azure Çin 21Vianet|KMS.Core.chinacloudapi.CN|42.159.7.249|


Özel rota eklemek için aşağıdaki adımları izleyin:

### <a name="for-resource-manager-vms"></a>Kaynak Yöneticisi VM'ler için

1. Azure PowerShell'i açın ve ardından [Azure aboneliğinizde oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Aşağıdaki komutları çalıştırın:

    ```powershell
    # First, we will get the virtual network hosts the VMs that has activation problems. In this case, I get virtual network ArmVNet-DM in Resource Group ArmVNet-DM

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, we create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out

    $RouteTable = New-AzureRmRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzureRmRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzureRmRouteTable -RouteTable $RouteTable
    ```
3. Git kullanmak etkinleştirme sorunlu VM [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) KMS sunucusunda erişebiliyorsa test etmek için:

        psping kms.core.windows.net:1688

4. Windows etkinleştirme ve sorunun çözülüp çözülmediğine bakın deneyin.

### <a name="for-classic-vms"></a>Klasik VM'ler için

1. Azure PowerShell'i açın ve ardından [Azure aboneliğinizde oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Aşağıdaki komutları çalıştırın:

    ```powershell
    # First, we will create a new route table
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the routetable that was created
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply KMS route table to the subnet that host the problem VMs (in this case, I will apply it to the subnet named Subnet-1)
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Git kullanmak etkinleştirme sorunlu VM [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) KMS sunucusunda erişebiliyorsa test etmek için:

        psping kms.core.windows.net:1688

4. Windows etkinleştirme ve sorunun çözülüp çözülmediğine bakın deneyin.

## <a name="next-steps"></a>Sonraki adımlar

- [KMS istemcisi kurulum anahtarları](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Gözden geçirme ve seçme etkinleştirme yöntemleri](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)