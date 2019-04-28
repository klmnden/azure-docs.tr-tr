---
title: PowerShell kullanarak bir ağ güvenlik grubu (Klasik) oluşturmak | Microsoft Docs
description: Oluşturma ve PowerShell kullanarak bir ağ güvenlik grubu (Klasik) dağıtma hakkında bilgi edinin
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 86810b0d-0240-46a2-8548-fca22daa56f3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: d685f395aa25580c009fe3be660a8afc42dc79d9
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125772"
---
# <a name="create-a-network-security-group-classic-using-powershell"></a>PowerShell kullanarak bir ağ güvenlik grubu (Klasik) oluşturun
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde Nsg'leri oluşturma](tutorial-filter-network-traffic.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Önceden oluşturulmuş basit bir ortam aşağıdaki komutları beklediğiniz PowerShell örneği, yukarıdaki senaryo temel alınarak. Bu belgede gösterildiği komutlarını çalıştırmak isterseniz, test ortamı tarafından derlediğinizden [sanal ağ oluşturma](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Ön uç alt ağı için bir NSG oluşturma

1. Azure PowerShell'i hiç kullanmadıysanız, [yükleme ve yapılandırma, Azure PowerShell](/powershell/azure/overview).

2. Adlı bir ağ güvenlik grubu oluşturma *NSG ön uç*:

    ```powershell   
    New-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" -Location uswest `
      -Label "Front end subnet NSG"
   ```

3. Bir güvenlik kuralı internetten 3389 numaralı bağlantı noktasına erişimine oluşturun:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name rdp-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
      -SourceAddressPrefix Internet  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '3389'
   ```

4. 80 numaralı bağlantı noktasına internet'ten erişim veren bir güvenlik kuralı oluşturun:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name web-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 200 `
      -SourceAddressPrefix Internet  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '80'
    ```

## <a name="create-an-nsg-for-the-back-end-subnet"></a>Arka uç alt ağı için bir NSG oluşturma

1. Adlı bir ağ güvenlik grubu oluşturma *NSG arka uç*:
   
    ```powershell
    New-AzureNetworkSecurityGroup -Name "NSG-BackEnd" -Location uswest `
      -Label "Back end subnet NSG"
    ```

2. Ön uç alt ağından gelen 1433 numaralı bağlantı noktasına (SQL Server tarafından kullanılan varsayılan bağlantı noktası) erişim veren bir güvenlik kuralı oluşturun:
   
    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name rdp-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
      -SourceAddressPrefix 192.168.1.0/24  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '1433'
    ```

3. Alt ağından internet erişimini engelleyen bir güvenlik kuralı oluşturun:
   
    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-BackEnd" `
      | Set-AzureNetworkSecurityRule -Name block-internet `
      -Action Deny -Protocol '*' -Type Outbound -Priority 200 `
      -SourceAddressPrefix '*'  -SourcePortRange '*' `
      -DestinationAddressPrefix Internet -DestinationPortRange '*'
   ```