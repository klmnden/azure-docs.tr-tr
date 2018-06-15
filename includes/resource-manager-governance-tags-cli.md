---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b9484336add0719749e9f0af56bdd70fa3906ef5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
ms.locfileid: "29532348"
---
Bir kaynak grubuna iki etiket eklemek için kullanın [az grup güncelleştirme](/cli/azure/group#az_group_update) komutu:

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Şimdi üçüncü bir etiket eklemek istediğinizi varsayalım. Komutu, yeni etiketle yeniden çalıştırın. Varolan etiketleri eklenir.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Kaynakları kaynak grubundan etiketleri almazlar. Şu anda kaynak grubunuz üç etiketlidir ancak kaynakları herhangi bir etiket yoktur. Tüm etiketleri bir kaynak grubundan kaynaklarını için uygulama ve kaynaklarına varolan etiketleri korumak için aşağıdaki komut dosyasını kullanın:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

Alternatif olarak, etiketleri kaynak grubundan kaynakları varolan etiketleri tutmadan uygulayabilirsiniz:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Tek bir etiket birkaç değerleri birleştirmek için bir JSON dizesi kullanın.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Bir kaynak grubu üzerinde tüm etiketleri kaldırmak için kullanın:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
