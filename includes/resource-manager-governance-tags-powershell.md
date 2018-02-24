---
title: "include dosyası"
description: "include dosyası"
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 87b9bd74fc59c86bf177e45c18bfc4e104d9c996
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
Bir kaynak grubuna iki etiket eklemek için kullanın:

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Şimdi üçüncü bir etiket eklemek istediğinizi varsayalım. Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Varolan etiketleri kaybetmeden yeni bir etiket eklemek için varolan etiketleri almak, yeni bir etiket eklemek ve etiketler koleksiyonunu yeniden uygulayın:

```powershell
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Kaynakları kaynak grubundan etiketleri almazlar. Şu anda kaynak grubunuz üç etiketlidir ancak kaynakları herhangi bir etiket yoktur. Tüm etiketleri kaynaklarını için bir kaynak grubundan uygulamak ve tekrarı olmayan kaynaklar varolan etiketleri korumak için aşağıdaki komut dosyasını kullanın:

```powershell
$group = Get-AzureRmResourceGroup myResourceGroup
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Alternatif olarak, etiketleri kaynak grubundan kaynakları varolan etiketleri tutmadan uygulayabilirsiniz:

```powershell
$g = Get-AzureRmResourceGroup -Name myResourceGroup
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Tek bir etiket birkaç değerleri birleştirmek için bir JSON dizesi kullanın.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirin.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
