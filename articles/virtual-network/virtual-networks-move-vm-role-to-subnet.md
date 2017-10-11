---
title: "Bir VM'yi (Klasik) veya Bulut Hizmetleri rol örneği farklı bir alt - Azure PowerShell taşıma | Microsoft Docs"
description: "PowerShell kullanarak farklı bir alt ağa VM'ler (Klasik) ve bulut Hizmetleri rol örnekleri taşıma öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b094f8338394ef2e84cad3070936d715411326a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a>PowerShell kullanarak farklı bir alt ağa bir VM (Klasik) veya Bulut Hizmetleri rol örneğini taşıma
Vm'leriniz (Klasik) bir alt ağdan başka bir programda aynı sanal ağ (VNet) taşımak için PowerShell kullanın. Rol örnekleri CSCFG dosyası düzenleme yerine PowerShell kullanarak taşınabilir.

> [!NOTE]
> Bu makalede VM'ler yalnızca klasik dağıtım modeli aracılığıyla dağıtılan taşıma konusunda açıklanmaktadır.
> 
> 

Neden sanal makineleri başka bir alt ağa taşıyın? Alt ağ geçiş eski alt ağı çok küçük ve bu alt ağdaki var olan çalışan sanal makinelerini nedeniyle genişletilemiyor faydalıdır. Bu durumda, yeni, büyük bir alt ağ oluşturmak ve yeni bir alt ağ için sanal makineleri geçirmek ardından geçiş tamamlandıktan sonra eski boş alt silebilirsiniz.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Başka bir alt ağ için bir VM taşıma
Bir VM taşımak için aşağıdaki örnekte bir şablon olarak kullanıp kümesi AzureSubnet PowerShell cmdlet'ini çalıştırın. Aşağıdaki örnekte, biz TestVM mevcut kendi alt ağdan alt ağ-2'ye taşıyor. Ortamınızı yansıtmak üzere örnek düzenlemek emin olun. Her bir yordam bir parçası olarak güncelleştirme-AzureVM cmdlet'ini çalıştırdığınızda, güncelleştirme işleminin bir parçası olarak, VM başlayacaktır unutmayın.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

VM için statik iç özel IP belirtilmişse, yeni bir alt ağ için VM taşımadan önce bu ayarı temizlemek gerekir. Bu durumda, aşağıdakileri kullanın:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Başka bir alt ağ için bir rol örneği taşımak için
Rol örneği taşımak için CSCFG dosyasını düzenleyin. Aşağıdaki örnekte, biz "Role0" sanal ağında taşıyor *vnetname adlı* mevcut kendi alt ağdan *alt ağ-2*. Rol örneği zaten dağıtılmış olduğundan, alt ağ adı yalnızca değiştireceğiz = alt ağ-2. Ortamınızı yansıtmak üzere örnek düzenlemek emin olun.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
