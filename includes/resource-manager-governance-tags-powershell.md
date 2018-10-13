---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 5914789675edba0d56e6899728fc2c3c7768374a
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49312384"
---
Kaynak grubuna iki etiket eklemek için [Set-AzureRmResourceGroup](/powershell/module/azurerm.resources/set-azurermresourcegroup) komutunu kullanın:

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Şimdi üçüncü bir etiket istediğinizi varsayalım. Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Mevcut etiketleri kaybetmeden yeni bir etiket eklemek için, mevcut etiketleri almalı, yeni etiketi eklemeli ve etiket koleksiyonunu yeniden uygulamalısınız:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags.Add("Project", "Documentation")

# Reapply the updated set of tags 
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Kaynaklar, kaynak grubundan etiketleri devralmaz. Şu anda, kaynak grubunuzun üç etiketi vardır ama kaynakların hiç etiketi yoktur. Bir kaynak grubundaki tüm etiketleri yinelenmeyen kaynaklardaki mevcut etiketleri koruyarak gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzureRmResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        
        # If the resource has existing tags, add new ones
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }

            # Reapply the updated tags to the resource 
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Alternatif olarak, mevcut etiketleri korumadan kaynak grubundaki etiketleri kaynaklara uygulayabilirsiniz:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzureRmResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Birkaç değeri tek etikette birleştirmek için bir JSON dizesi kullanın.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Mevcut olan etiketlerin kaybetmeden birkaç değerlerle yeni bir etiket eklemek için mevcut etiketleri alın, yeni etiket için bir JSON dizesi kullanın ve etiket koleksiyonu yeniden uygulayın:

```azurepowershell-interactive
# Get existing tags and add a new tag
$ResourceGroup = Get-AzureRmResourceGroup -Name myResourceGroup
$Tags = $ResourceGroup.Tags
$Tags.Add("CostCenter", "{`"Dept`":`"IT`",`"Environment`":`"Test`"}")

# Reapply the updated set of tags
$ResourceGroup | Set-AzureRmResourceGroup -Tag $Tags
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirirsiniz.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
