---
title: "PowerShell ile Azure çözümleri yönetmek | Microsoft Docs"
description: "Azure PowerShell ve Resource Manager kaynaklarınızı yönetmek için kullanın."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: tomfitz
ms.openlocfilehash: 96206482195cdcbd06ee2dafdc551f7b1f81d319
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="manage-resources-with-azure-powershell"></a>Azure PowerShell ile kaynakları yönetme

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Seçerseniz yükle ve yerel olarak PowerShell kullanın, bkz: [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="understand-scope"></a>Kapsam anlama

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

İşiniz bittiğinde, bu ayarları kolayca kaldırabilmeniz için bu makalede, tüm yönetim ayarlarını bir kaynak grubuna uygulayın.

Kaynak grubu oluşturalım.

```azurepowershell-interactive
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Kaynak grubu şu anda boştur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Rol atama

Bu makalede, bir sanal makine ve onun ilişkili sanal ağ dağıtın. Sanal makine çözümleri yönetmek için yaygın olarak gerekli erişim sağlayan üç kaynağa özel rollere vardır:

* [Sanal makine Katılımcısı](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor)
* [Ağ Katılımcısı](../active-directory/role-based-access-built-in-roles.md#network-contributor)
* [Depolama hesabı katkıda bulunan](../active-directory/role-based-access-built-in-roles.md#storage-account-contributor)

Tek tek kullanıcılara roller atama yerine genellikle daha kolay olur [bir Azure Active Directory grubu oluşturun](../active-directory/active-directory-groups-create-azure-portal.md) benzer önlemler almak için gereken kullanıcılar için. Ardından, bu grup için uygun rolü atayın. Bu makalede basitleştirmek için bir Azure Active Directory grubu üyeleri olmadan oluşturun. Hala bu grubun bir kapsam için bir rol atayabilirsiniz. 

Aşağıdaki örnek, bir grup oluşturur ve kaynak grubu için sanal makine katılımcı rolü atar. Çalıştırmak için `New-AzureAdGroup` komutunu ya da kullanım gerekir [Azure bulut Kabuk](/azure/cloud-shell/overview) veya [Azure AD PowerShell modülünü indirin](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Genellikle, işlem için yineleme **ağ Katılımcısı** ve **depolama hesabı katkıda bulunan** dağıtılan kaynakları yönetmek için atanan kullanıcılar emin olmak için. Bu makalede, bu adımı atlayabilirsiniz.

## <a name="azure-policies"></a>Azure ilkeleri

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>İlkelerini uygula

Birkaç ilke tanımları aboneliğiniz zaten vardır. Mevcut ilke tanımları görmek için kullanın:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Mevcut ilke tanımları bakın. İlke türü olan **yerleşik** veya **özel**. Ata istediğiniz bir koşul açıklayan olanları tanımlarında bakın. Bu makalede, ilkeler, ata:

* tüm kaynaklar konumlarını sınırla
* sanal makineler için SKU'ları sınırlayın
* sanal makineler, yönetilen diskleri kullanmayın denetleme

```azurepowershell-interactive
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzureRmResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
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

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>Bir kaynak Kilitle

Ağ güvenlik grubu ve sanal makine kilitlemek için kullanın:

```azurepowershell-interactive
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Özellikle kilidi kaldırırsanız, sanal makine yalnızca silinebilir. Bu adım gösterilen [kaynakları temizlemek](#clean-up-resources).

## <a name="tag-resources"></a>Etiket kaynakları

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Etiket kaynakları

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

Bir sanal makineye etiketleri uygulamak için kullanın:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Etikete göre kaynakları bulun

Bir etiketi ad ve değerli kaynakları bulmak için kullanın:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Döndürülen değerlerin bir etiket değeri olan tüm sanal makineleri durdurma gibi yönetim görevleri için kullanabilirsiniz.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Görünüm maliyetler etiket değerlerine göre

Kaynaklar için etiketler uyguladıktan sonra bu etiketlerin kaynaklarla maliyetlerini görüntüleyebilirsiniz. İşlem maliyetleri henüz göremeyebilirsiniz son kullanım görüntüleyecek şekilde maliyet analizi için biraz zaman alır. Maliyetleri kullanılabilir olduğunda, aboneliğinizde kaynak gruplarında kaynakları maliyetlerini görüntüleyebilirsiniz. Kullanıcılar [fatura bilgilerini abonelik düzeyinde erişime](../billing/billing-manage-access.md) maliyetleri görmek üzere.

Portal etiketinde tarafından maliyetleri görüntülemek için aboneliğinizi seçin ve seçin **maliyet analizi**.

![Maliyet analizi](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Sonra filtre tarafından etiket değeri ve seçin **Uygula**.

![Etiket görünüm maliyeti](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

Aynı zamanda [Azure faturalama API'leri](../billing/billing-usage-rate-card-overview.md) program aracılığıyla maliyetleri görüntülemek için.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kilit kaldırılana kadar kilitli ağ güvenlik grubu silinemiyor. Kilidi kaldırmak için kullanın:

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

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
* Sanal makinelerinizi izleme hakkında bilgi edinmek için [izlemek ve güncelleştirmek Azure PowerShell ile Windows sanal makine](../virtual-machines/windows/tutorial-monitoring.md).
* Önerilen güvenlik uygulamaları uygulamak için Azure Güvenlik Merkezi'ni kullanma hakkında bilgi edinmek için [Azure Güvenlik Merkezi'ni kullanarak sanal makine güvenliği izleyin](../virtual-machines/windows/tutorial-azure-security.md).
* Yeni bir kaynak grubu mevcut kaynakları taşıyabilirsiniz. Örnekler için bkz: [yeni kaynak grubuna veya aboneliğe taşıma kaynaklara](resource-group-move-resources.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).
