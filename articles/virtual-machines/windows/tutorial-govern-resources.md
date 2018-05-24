---
title: Öğretici - Azure PowerShell ile Azure sanal makinelerini yönetme | Microsoft Docs
description: Bu öğreticide RBAC, ilkeler, kilitler ve etiketler uygulayarak Azure sanal makinelerini yönetmek için Azure PowerShell’i kullanma hakkında bilgi edineceksiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 154ba47881c65d963729f9074d93c7bb61020389
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-learn-about-linux-virtual-machine-governance-with-azure-powershell"></a>Öğretici: Azure PowerShell ile Linux sanal makine yönetimi hakkında bilgi edinin

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir. Yerel yüklemeler için aynı zamanda [Azure AD PowerShell modülünü indirerek](https://www.powershellgallery.com/packages/AzureAD/) yeni bir Azure Active Directory grubu oluşturmanız gerekir.

## <a name="understand-scope"></a>Kapsamı anlama

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

Bu öğreticide, işiniz bittiğinde kolayca silebilmeniz için tüm yönetim ayarlarını bir kaynak grubuna uygulayacaksınız.

Şimdi o kaynak grubunu oluşturalım.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Kaynak grubu şu anda boştur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Kuruluşunuzdaki kullanıcıların bu kaynaklara erişmek için doğru düzeyde erişime sahip olduğundan emin olmak istersiniz. Kullanıcılara sınırsız erişim vermek istemezsiniz ancak işlerini halledebildiklerinden de emin olmanız gerekir. [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md), bir kapsamdaki belirli eylemleri tamamlamak için izinli olan kullanıcıları yönetmenizi sağlar.

Rol atamaları oluşturmak ve silmek için kullanıcıların `Microsoft.Authorization/roleAssignments/*` erişimine sahip olması gerekir. Bu erişim, Sahip veya Kullanıcı Erişimi Yöneticisi rolleriyle verilir.

Sanal makine çözümlerini yönetmek için yaygın olarak gereken erişimi sağlayan üç adet kaynağa özgü rol vardır:

* [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Ağ Katılımcısı](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Depolama Hesabı Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Kullanıcılara rolleri tek tek atamak yerine, benzer eylemlerde bulunması gereken kullanıcılar için [bir Azure Active Directory grubu](../../active-directory/active-directory-groups-create-azure-portal.md) oluşturmak genellikle daha kolaydır. Ardından, bu grubu uygun role atayabilirsiniz. Bu makaleyi basitleştirmek için, üyeleri olmayan bir Azure Active Directory grubu oluşturun. Bu grubu hala bir kapsamın rolüne atayabilirsiniz. 

Aşağıdaki örnek, posta takma adı *vmDemoGroup* olan *VMDemoContributors* adlı bir Azure Active Directory grubu oluşturur. Posta takma adı, grubun diğer adı olarak görev yapar.

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
```

Komut istemi döndürüldükten kısa bir süre sonra grup Azure Active Directory’ye yayılır. 20 veya 30 saniye bekledikten sonra [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) komutunu kullanarak yeni Azure Active Directory grubunu kaynak grubu için Sanal Makine Katılımcısı rolüne atayabilirsiniz.  Aşağıdaki komutu yayılmadan önce çalıştırırsanız, **Dizinde <guid> sorumlusu yok** ifadesini içeren bir hata alırsınız. Komutu tekrar çalıştırmayı deneyin.

```azurepowershell-interactive
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Genellikle, kullanıcıların dağıtılmış kaynakları yönetmek için atandığından emin olmak üzere *Ağ Katılımcısı* ve *Depolama Hesabı Katılımcısı* için işlemi yinelemeniz gerekir. Bu makalede, söz konusu adımları atlayabilirsiniz.

## <a name="azure-policies"></a>Azure ilkeleri

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Azure ilkeleri

Aboneliğinizde zaten birkaç ilke tanımı mevcuttur. Kullanılabilir ilke tanımlarını görmek için [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) komutunu kullanın:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Mevcut ilke tanımlarını göreceksiniz. İlke türü **Yerleşik** veya **Özel**’dir. Atamak istediğiniz bir koşulu açıklayan ilke türlerinin tanımlarına bakın. Bu makalede, aşağıdakileri gerçekleştiren ilkeler atayacaksınız:

* Tüm kaynaklar için konumları sınırlama.
* Sanal makineler için SKU'ları sınırlama.
* Yönetilen diskler kullanmayan sanal makineleri denetleme.

Aşağıdaki örnekte, görünen ada göre üç ilke tanımı alırsınız. Bu tanımları kaynak grubuna atamak için [New-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) komutunu kullanın. Bazı ilkeler için, izin verilen değerleri belirtmek üzere parametre değerleri sağlayın.

```azurepowershell-interactive
# Values to use for parameters
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

# Get the resource group
$rg = Get-AzureRmResourceGroup -Name myResourceGroup

# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

# Assign policy for allowed locations
New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations

# Assign policy for allowed SKUs
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus

# Assign policy for auditing unmanaged disks
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Sanal makineyi dağıtma

Rol ve ilkeler atadıktan sonra çözümünüzü dağıtmaya hazırsınız. Varsayılan boyut, izin verilen SKU’larınızdan biri olan Standard_DS1_v2’dir. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

```azurepowershell-interactive
New-AzureRmVm -ResourceGroupName "myResourceGroup" `
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

Sanal makineyi ve ağ güvenlik grubunu kilitlemek için [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) komutunu kullanın:

```azurepowershell-interactive
# Add CanNotDelete lock to the VM
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup

# Add CanNotDelete lock to the network security group
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Kilitleri test etmek için aşağıdaki komutu çalıştırmayı deneyin:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Silme işleminin bir kilit nedeniyle gerçekleştirilemediğini belirten bir hata görürsünüz. Kaynak grubu yalnızca kilitleri spesifik olarak kaldırırsanız silinebilir. Bu adım [Kaynakları temizle](#clean-up-resources) bölümünde gösterilmektedir.

## <a name="tag-resources"></a>Kaynakları etiketleme

Azure kaynaklarınızı mantıksal olarak kategorilere ayırmak için [etiketler](../../azure-resource-manager/resource-group-using-tags.md) uygulayabilirsiniz. Her etiket bir ad ve değerden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Bir sanal makineye etiket uygulamak için [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) komutunu kullanın:

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Kaynakları etikete göre bulma

Kaynakları etiket adı ve değeriyle bulmak için [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) komutunu kullanın:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Tüm sanal makineleri bir etiket değeriyle durdurmak gibi yönetim görevleri için döndürülen değerleri kullanabilirsiniz.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Maliyetleri etiket değerlerine göre görüntüleme

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kilit kaldırılana kadar kilitli ağ güvenlik grubu silinemez. Kilidi kaldırmak için [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) komutunu kullanın:

```azurepowershell-interactive
Remove-AzureRmResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzureRmResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
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

