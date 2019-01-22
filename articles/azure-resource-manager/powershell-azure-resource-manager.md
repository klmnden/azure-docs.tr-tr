---
title: PowerShell ile Azure çözümlerini yönetme | Microsoft Docs
description: Azure PowerShell ve Resource Manager kaynaklarınızı yönetmek için kullanın.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 933c7e5b73abf533250072680160d5a5caab6523
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435448"
---
# <a name="manage-resources-with-azure-powershell"></a>Azure PowerShell ile kaynakları yönetme

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="understand-scope"></a>Kapsamı anlama

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

İşiniz bittiğinde bu ayarları kolayca kaldırabilmeniz için bu makalede, tüm yönetim ayarları bir kaynak grubu için geçerlidir.

Kaynak grubu oluşturalım.

```azurepowershell-interactive
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Kaynak grubu şu anda boştur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>Rol atama

Bu makalede, bir sanal makineyi ve ilgili kendi sanal ağı dağıtın. Sanal makine çözümlerini yönetmek için yaygın olarak gereken erişimi sağlayan üç adet kaynağa özgü rol vardır:

* [Sanal Makine Katılımcısı](../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor)
* [Depolama Hesabı Katılımcısı](../role-based-access-control/built-in-roles.md#storage-account-contributor)

Kullanıcılara rolleri tek tek atamak yerine, benzer eylemlerde bulunması gereken kullanıcılar için [bir Azure Active Directory grubu](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) oluşturmak genellikle daha kolaydır. Ardından, bu grubu uygun role atayabilirsiniz. Bu makaleyi basitleştirmek için, üyeleri olmayan bir Azure Active Directory grubu oluşturun. Bu grubu hala bir kapsamın rolüne atayabilirsiniz.

Aşağıdaki örnek, bir grup oluşturur ve kaynak grubu için sanal makine Katılımcısı rolü atar. Çalıştırılacak `New-AzureAdGroup` komutunu kullanın gerekir [Azure Cloud Shell](/azure/cloud-shell/overview) veya [Azure AD PowerShell modülünü indirme](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Genellikle, kullanıcıların dağıtılmış kaynakları yönetmek için atandığından emin olmak üzere **Ağ Katılımcısı** ve **Depolama Hesabı Katılımcısı** için işlemi yinelemeniz gerekir. Bu makalede, söz konusu adımları atlayabilirsiniz.

## <a name="azure-policy"></a>Azure İlkesi

[Azure İlkesi](../azure-policy/azure-policy-introduction.md) abonelikteki tüm kaynakların şirket standartlarına uyduğundan emin olmanıza yardımcı olur. Aboneliğinizde zaten birkaç ilke tanımı mevcuttur. Kullanılabilir ilke tanımlarını görmek için bu seçeneği kullanın:

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Mevcut ilke tanımlarını göreceksiniz. İlke türü **Yerleşik** veya **Özel**’dir. Atamak istediğiniz bir koşulu açıklayan ilke türlerinin tanımlarına bakın. Bu makalede, aşağıdakileri gerçekleştiren ilkeler atayacaksınız:

* tüm kaynaklar için konumları sınırlandırmanıza
* sanal makineler için SKU'ları sınırlayın
* Yönetilen disk kullanmayan sanal makineleri denetle

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

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>Bir kaynağı Kilitle

Sanal makine ve ağ güvenlik grubu kilitlemek için kullanın:

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

Sanal makine, yalnızca özellikle kilit kaldırırsanız silinebilir. Bu adım [Kaynakları temizle](#clean-up-resources) bölümünde gösterilmektedir.

## <a name="tag-resources"></a>Kaynakları etiketleme

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>Kaynakları etiketleme

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

Bir sanal makineye etiketleri uygulamak için bu seçeneği kullanın:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Kaynakları etikete göre bulma

Kaynakları bir etiket adı ve değeri bulmak için kullanın:

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

Tüm sanal makineleri bir etiket değeriyle durdurmak gibi yönetim görevleri için döndürülen değerleri kullanabilirsiniz.

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Maliyetleri etiket değerlerine göre görüntüleme

Kaynaklara etiket uyguladıktan sonra, bu etiketlerle kaynakların maliyetlerini görüntüleyebilirsiniz. Maliyet analizinin en son kullanımları göstermesi biraz zaman aldığından maliyetleri henüz göremeyebilirsiniz. Maliyetler kullanılabilir olduğunda, aboneliğinizdeki kaynak gruplarında kaynakların maliyetlerini görüntüleyebilirsiniz. Kullanıcıların maliyetleri görmeleri için [fatura bilgilerine abonelik düzeyinde erişimlerinin](../billing/billing-manage-access.md) olması gerekir.

Portalda maliyetleri etikete göre görüntülemek için aboneliğinizi ve sonra **Maliyet Analizi**’ni seçin.

![Maliyet analizi](./media/powershell-azure-resource-manager/select-cost-analysis.png)

Ardından etiket değerine göre filtreleyin ve **Uygula**’yı seçin.

![Etikete göre maliyeti görüntüleme](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

Ayrıca maliyetleri programlı olarak görüntülemek için [Azure Faturalama API'lerini](../billing/billing-usage-rate-card-overview.md) kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kilit kaldırılana kadar kilitli ağ güvenlik grubu silinemez. Kilidi kaldırmak için kullanın:

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

* Sanal makinelerinizi izleme hakkında bilgi edinmek için [izlemek ve güncelleştirmek Azure PowerShell ile Windows sanal makine](../virtual-machines/windows/tutorial-monitoring.md).
* Önerilen güvenlik uygulamaları, uygulamak için Azure Güvenlik Merkezi'ni kullanma hakkında bilgi edinmek için [Azure Güvenlik Merkezi'ni kullanarak sanal makine Güvenlik İzleme](../virtual-machines/windows/tutorial-azure-security.md).
* Var olan kaynakları yeni kaynak grubuna taşıyabilirsiniz. Örnekler için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
