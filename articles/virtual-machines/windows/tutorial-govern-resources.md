---
title: Öğretici - Azure PowerShell ile Azure sanal makineleri yönetme | Microsoft Docs
description: Bu öğreticide RBAC, ilkeler, kilitler ve etiketler uygulayarak Azure sanal makinelerini yönetmek için Azure PowerShell’i kullanma hakkında bilgi edineceksiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: gwallace
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 12/05/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 74f8ad6935c0d6776dcad6f24e7e92318f7aba4a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708102"
---
# <a name="tutorial-learn-about-windows-virtual-machine-management-with-azure-powershell"></a>Öğretici: Azure PowerShell ile Windows sanal makine yönetimi hakkında bilgi edinin

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="understand-scope"></a>Kapsamı anlama

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

Bu öğreticide, işiniz bittiğinde kolayca silebilmeniz için tüm yönetim ayarlarını bir kaynak grubuna uygulayacaksınız.

Şimdi o kaynak grubunu oluşturalım.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

Kaynak grubu şu anda boştur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Kuruluşunuzdaki kullanıcıların bu kaynaklara erişmek için doğru düzeyde erişime sahip olduğundan emin olmak istersiniz. Kullanıcılara sınırsız erişim vermek istemezsiniz ancak işlerini halledebildiklerinden de emin olmanız gerekir. [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md), bir kapsamdaki belirli eylemleri tamamlamak için izinli olan kullanıcıları yönetmenizi sağlar.

Rol atamaları oluşturmak ve kaldırmak için kullanıcıların `Microsoft.Authorization/roleAssignments/*` erişimi olması gerekmektedir. Bu erişim, Sahip veya Kullanıcı Erişimi Yöneticisi rolleriyle verilir.

Sanal makine çözümlerini yönetmek için yaygın olarak gereken erişimi sağlayan üç adet kaynağa özgü rol vardır:

* [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Ağ Katılımcısı](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Depolama Hesabı Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Kullanıcılara rolleri tek tek atamak yerine, benzer eylemlerde bulunması gereken kullanıcılar için bir Azure Active Directory grubu kullanmak genellikle daha kolaydır. Ardından, bu grubu uygun role atayabilirsiniz. Bu makalede sanal makineyi yönetmek için var olan bir grubu kullanın veya portalı kullanarak [bir Azure Active Directory grubu oluşturun](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Yeni grup oluşturma veya var olan bir tek, kullanım bulma sonra [yeni AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment) kaynak grubu için sanal makine Katılımcısı rolü için Azure Active Directory grubu atamak için komutu.  

```azurepowershell-interactive
$adgroup = Get-AzADGroup -DisplayName <your-group-name>

New-AzRoleAssignment -ObjectId $adgroup.id `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Belirten bir hata alırsanız **asıl \<GUID > dizinde yok**, yeni Grup Azure Active Directory yayılan edilmemiş. Komutu tekrar çalıştırmayı deneyin.

Genellikle, kullanıcıların dağıtılmış kaynakları yönetmek için atandığından emin olmak üzere *Ağ Katılımcısı* ve *Depolama Hesabı Katılımcısı* için işlemi yinelemeniz gerekir. Bu makalede, söz konusu adımları atlayabilirsiniz.

## <a name="azure-policy"></a>Azure İlkesi

[Azure İlkesi](../../governance/policy/overview.md) abonelikteki tüm kaynakların şirket standartlarına uyduğundan emin olmanıza yardımcı olur. Aboneliğinizde zaten birkaç ilke tanımı mevcuttur. Kullanılabilir ilke tanımlarını görmek için [Get-AzPolicyDefinition](https://docs.microsoft.com/powershell/module/az.resources/Get-AzPolicyDefinition) komutu:

```azurepowershell-interactive
(Get-AzPolicyDefinition).Properties | Format-Table displayName, policyType
```

Mevcut ilke tanımlarını göreceksiniz. İlke türü **Yerleşik** veya **Özel**’dir. Atamak istediğiniz bir koşulu açıklayan ilke türlerinin tanımlarına bakın. Bu makalede, aşağıdakileri gerçekleştiren ilkeler atayacaksınız:

* Tüm kaynaklar için konumları sınırlama.
* Sanal makineler için SKU'ları sınırlama.
* Yönetilen diskler kullanmayan sanal makineleri denetleme.

Aşağıdaki örnekte, görünen ada göre üç ilke tanımı alırsınız. Kullandığınız [yeni AzPolicyAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azpolicyassignment) tanımlar kaynak grubuna atamak için komutu. Bazı ilkeler için, izin verilen değerleri belirtmek üzere parametre değerleri sağlayın.

```azurepowershell-interactive
# Values to use for parameters
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

# Get the resource group
$rg = Get-AzResourceGroup -Name myResourceGroup

# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
$locationDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

# Assign policy for allowed locations
New-AzPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations

# Assign policy for allowed SKUs
New-AzPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus

# Assign policy for auditing unmanaged disks
New-AzPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Sanal makineyi dağıtma

Rol ve ilkeler atadıktan sonra çözümünüzü dağıtmaya hazırsınız. Varsayılan boyut, izin verilen SKU’larınızdan biri olan Standard_DS1_v2’dir. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

```azurepowershell-interactive
New-AzVm -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -VirtualNetworkName "myVnet" `
     -SubnetName "mySubnet" `
     -SecurityGroupName "myNetworkSecurityGroup" `
     -PublicIpAddressName "myPublicIpAddress" `
     -OpenPorts 80,3389
```

Dağıtımınız tamamlandıktan sonra çözüme daha fazla yönetim ayarı uygulayabilirsiniz.

## <a name="lock-resources"></a>Kaynakları kilitleme

[Kaynak kilitleri](../../azure-resource-manager/resource-group-lock-resources.md), kuruluşunuzdaki kullanıcıların kritik kaynakları yanlışlıkla silmesini veya değiştirmesini önler. Rol tabanlı erişim denetiminin aksine, kaynak kilitleri tüm kullanıcılar ve roller için bir kısıtlama uygular. Kilit düzeyini *CanNotDelete* veya *ReadOnly* olarak ayarlayabilirsiniz.

Sanal makine ve ağ güvenlik grubu kilitlemek için kullanmak [yeni AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock) komutu:

```azurepowershell-interactive
# Add CanNotDelete lock to the VM
New-AzResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup

# Add CanNotDelete lock to the network security group
New-AzResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Kilitleri test etmek için aşağıdaki komutu çalıştırmayı deneyin:

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroup
```

Silme işleminin bir kilit nedeniyle tamamlanamadığını belirten bir hata görürsünüz. Kaynak grubu yalnızca kilitleri spesifik olarak kaldırırsanız silinebilir. Bu adım [Kaynakları temizle](#clean-up-resources) bölümünde gösterilmektedir.

## <a name="tag-resources"></a>Kaynakları etiketleme

Azure kaynaklarınızı mantıksal olarak kategorilere ayırmak için [etiketler](../../azure-resource-manager/resource-group-using-tags.md) uygulayabilirsiniz. Her etiket bir ad ve değerden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Bir sanal makineye etiketleri uygulamak için [kümesi AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) komutu:

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Kaynakları etikete göre bulma

Kaynakları bir etiket adı ve değeri bulmak için kullanın [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) komutu:

```azurepowershell-interactive
(Get-AzResource -Tag @{ Environment="Test"}).Name
```

Tüm sanal makineleri bir etiket değeriyle durdurmak gibi yönetim görevleri için döndürülen değerleri kullanabilirsiniz.

```azurepowershell-interactive
Get-AzResource -Tag @{ Environment="Test"} | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzVM
```

### <a name="view-costs-by-tag-values"></a>Maliyetleri etiket değerlerine göre görüntüleme

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kilit kaldırılana kadar kilitli ağ güvenlik grubu silinemez. Kilidi kaldırmak için [Remove-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcelock) komutu:

```azurepowershell-interactive
Remove-AzResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Artık gerekli değilse [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) komutunu kaynak grubunu, VM'yi ve tüm ilgili kaynakları.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kullanıcıları bir role atama
> * Standartları uygulamaya zorlayan ilkeler uygulama
> * Kilitlerle kritik kaynakları koruma
> * Fatura ve yönetim için kaynakları etiketleme

Yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitoring.md)

