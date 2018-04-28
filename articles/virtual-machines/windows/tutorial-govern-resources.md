---
title: Azure PowerShell ile Azure sanal makineleri yöneten | Microsoft Docs
description: Öğretici - Azure sanal makineleri RBAC uygulayarak yönetmek, ilkeler, kilitler ve Azure PowerShell ile etiketler
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: tomfitz
ms.openlocfilehash: d4e09eb11ea04c31b7e302b7f66f8e67c13e8252
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="virtual-machine-governance-with-azure-powershell"></a>Azure PowerShell ile sanal makine Yönetimi

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Seçerseniz yükle ve yerel olarak PowerShell kullanın, bkz: [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir. Ayrıca yerel yüklemelerinde gerekir [Azure AD PowerShell modülünü indirin](https://www.powershellgallery.com/packages/AzureAD/) yeni bir Azure Active Directory grubu oluşturun.

## <a name="understand-scope"></a>Kapsam anlama

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

İşiniz bittiğinde, bu ayarları kolayca kaldırabilmeniz için Bu öğreticide, tüm yönetim ayarlarını bir kaynak grubuna uygulayın.

Bu kaynak grubu oluşturalım.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Kaynak grubu şu anda boştur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Kuruluşunuzdaki kullanıcıların bu kaynaklara erişim doğru düzeyde sahip olduğunuzdan emin olmak istersiniz. Sınırsız erişimi kullanıcılara vermek istediğiniz yoktur, ancak işlerini yapmak için emin olmanız gerekir. [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) hangi kullanıcıların belirli eylemleri bir kapsamda tamamlamak için izni yönetmenizi sağlar.

Oluşturma ve rol atamalarını kaldırmak için kullanıcıların olmalıdır `Microsoft.Authorization/roleAssignments/*` erişim. Bu erişim sahibi veya kullanıcı erişimi yöneticisi rolleri aracılığıyla verilir.

Sanal makine çözümleri yönetmek için yaygın olarak gerekli erişim sağlayan üç kaynağa özel rollere vardır:

* [Sanal makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Ağ Katılımcısı](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Depolama hesabı katkıda bulunan](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Tek tek kullanıcılara roller atama yerine genellikle daha kolay olur [bir Azure Active Directory grubu oluşturun](../../active-directory/active-directory-groups-create-azure-portal.md) benzer önlemler almak için gereken kullanıcılar için. Ardından, bu grup için uygun rolü atayın. Bu makalede basitleştirmek için bir Azure Active Directory grubu üyeleri olmadan oluşturun. Hala bu grubun bir kapsam için bir rol atayabilirsiniz. 

Aşağıdaki örnek adlı bir Azure Active Directory grubu oluşturur *VMDemoContributors* bir posta takma adı ile *vmDemoGroup*. Posta takma ad grubu için bir diğer ad olarak görev yapar.

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
```

Azure Active Directory yayılmasına grubu için komut istemi döndükten sonra bir dakika sürer. 20 veya 30 saniye bekledikten sonra kullanın [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) yeni Azure Active Directory grubu kaynak grubu için sanal makine Katılımcısı rolüne atamak için komutu.  Bunu yayılmadan önce aşağıdaki komutu çalıştırırsanız, belirten bir hata alırsınız **asıl <guid> dizininde yok**. Komutu yeniden çalıştırmayı deneyin.

```azurepowershell-interactive
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Genellikle, işlem için yineleme *ağ Katılımcısı* ve *depolama hesabı katkıda bulunan* dağıtılan kaynakları yönetmek için atanan kullanıcılar emin olmak için. Bu makalede, bu adımı atlayabilirsiniz.

## <a name="azure-policies"></a>Azure ilkeleri

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>İlkelerini uygula

Birkaç ilke tanımları aboneliğiniz zaten vardır. Mevcut ilke tanımları görmek için [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) komutu:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Mevcut ilke tanımları bakın. İlke türü olan **yerleşik** veya **özel**. Ata istediğiniz bir koşul açıklayan olanları tanımlarında bakın. Bu makalede, ilkeler, ata:

* Tüm kaynaklar konumlarını sınırlayın.
* Sanal makineler için SKU'ları sınırlayın.
* Sanal makineler, yönetilen diskleri kullanmayın denetim.

Aşağıdaki örnekte, üç ilke tanımları görünen adını temel alarak alın. Kullandığınız [yeni AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) bu tanımları kaynak grubuna atamak için komutu. Bazı ilkeler için izin verilen değerleri belirtmek için parametre değerlerini sağlayın.

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

## <a name="deploy-the-virtual-machine"></a>Sanal makine dağıtma

Çözümünüzü dağıtmak hazırsınız rolleri ve ilkeleri atadınız. Varsayılan boyutu, izin verilen SKU'lar biri Standard_DS1_v2 ' dir. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

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

Dağıtımınız tamamlandıktan sonra çözüme daha fazla yönetim ayarlarını uygulayabilirsiniz.

## <a name="lock-resources"></a>Kaynakları kilitleme

[Kaynak kilitleri](../../azure-resource-manager/resource-group-lock-resources.md) yanlışlıkla silinmesi ya da kritik kaynaklara değiştirme kuruluşunuzdaki kullanıcıların engelleme. Rol tabanlı erişim denetimi farklı olarak, tüm kullanıcılar ve roller bir kısıtlama kaynak kilitleri uygulayın. Kilit düzeyini ayarlayabilirsiniz *CanNotDelete* veya *salt okunur*.

Ağ güvenlik grubu ve sanal makine kilitlemek için kullanmak [yeni AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) komutu:

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

Kilitler sınamak için aşağıdaki komutu çalıştırarak deneyin:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Silme işlemi nedeniyle kilit gerçekleştirilemiyor bildiren bir hata görürsünüz. Kaynak grubu, yalnızca özellikle kilitler kaldırırsanız silinebilir. Bu adım gösterilen [kaynakları temizlemek](#clean-up-resources).

## <a name="tag-resources"></a>Etiket kaynakları

Uyguladığınız [etiketleri](../../azure-resource-manager/resource-group-using-tags.md) Azure kaynaklarınızı mantıksal olarak kategorilere göre düzenlemek için. Her etiket bir ad ve değerden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Bir sanal makineye etiketleri uygulamak üzere kullanmak [kümesi AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) komutu:

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Etikete göre kaynakları bulun

Bir etiketi ad ve değerli kaynakları bulmak için [Bul AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) komutu:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Döndürülen değerlerin bir etiket değeri olan tüm sanal makineleri durdurma gibi yönetim görevleri için kullanabilirsiniz.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Görünüm maliyetler etiket değerlerine göre

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kilit kaldırılana kadar kilitli ağ güvenlik grubu silinemiyor. Kilidi kaldırmak için kullanın [Kaldır AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) komutu:

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
> * Kullanıcılar için rol atama
> * Standartlar zorunlu ilkelerini uygula
> * Kilitleri olan kritik kaynaklarını koruma
> * Faturalama ve Yönetim için etiket kaynaklar

Nasıl yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitoring.md)

