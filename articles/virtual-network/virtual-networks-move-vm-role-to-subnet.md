---
title: Bir VM (Klasik) veya Cloud Services rol örneği - Azure PowerShell gibi farklı bir alt ağa taşıma | Microsoft Docs
description: PowerShell kullanarak farklı bir alt ağa VM'ler (Klasik) ve Cloud Services rol örneklerini taşımayı öğreneceksiniz.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 787a50a0cbf16089cd15f922b494cd12d680cb43
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640404"
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a>PowerShell kullanarak farklı bir alt ağa bir VM (Klasik) veya Cloud Services rol örneği Taşı
Sanal makinelerinizin (Klasik) bir alt ağdan diğerine aynı sanal ağdaki (VNet) taşımak için PowerShell kullanabilirsiniz. Rol örnekleri CSCFG dosyası düzenleme yerine PowerShell kullanarak taşınabilir.

> [!NOTE]
> Bu makalede, VM'ler yalnızca klasik dağıtım modeliyle dağıtılan gitme açıklanmaktadır.
> 
> 

Neden Vm'leri, başka bir alt ağa taşıma? Alt ağ geçiş daha eski bir alt ağ çok küçük ve bu alt ağdaki mevcut çalıştırılan sanal makinelerin nedeniyle genişletilemez yararlı olur. Bu durumda, yeni, daha büyük bir alt ağ oluşturun ve yeni alt ağ ile sanal makineleri geçirme ve ardından geçiş tamamlandıktan sonra eski boş alt ağ silebilirsiniz.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Bir sanal makine başka bir alt ağa taşıma
Bir VM'yi taşıma için aşağıdaki örnekte bir şablon olarak kullanılarak küme AzureSubnet PowerShell cmdlet'ini çalıştırın. Aşağıdaki örnekte, biz TestVM mevcut kendi alt ağdan alt ağ-2'ye taşınıyor. Örneği, ortamınızı yansıtacak şekilde düzenlemek emin olun. Bir yordam bir parçası olarak güncelleştirme-AzureVM cmdlet'ini çalıştırdığınız zaman, güncelleştirme işleminin bir parçası sanal makinenizin başlayacaktır unutmayın.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Sanal Makineniz için statik iç özel IP belirttiyseniz, yeni bir alt ağ için VM taşımadan önce bu ayarı temizlemek gerekir. Bu durumda, aşağıdakini kullanın:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Bir rol örneği için başka bir alt ağ taşımak için
Bir rol örneği taşımak için CSCFG dosyasını düzenleyin. Aşağıdaki örnekte, biz "Role0" sanal ağ içinde taşıyor *VNETName* , mevcut bir alt ağa gelen *alt ağı 2*. Rol örneği zaten dağıtıldığından, yalnızca alt ağ adı değiştireceksiniz alt ağ-2 =. Örneği, ortamınızı yansıtacak şekilde düzenlemek emin olun.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
