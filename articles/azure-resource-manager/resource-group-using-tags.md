---
title: Etiket Azure kaynakları için mantıksal kuruluş | Microsoft Docs
description: Faturalama ve yönetmek için Azure kaynaklarını düzenlemek için etiketleri uygulamak gösterilmektedir.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: 27ba79e9168e098717e91e5a7179b5bc419ef86c
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438417"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Azure kaynaklarınızı düzenlemek için etiketleri kullanma

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

Etiketler kaynaklara uygulamak için kullanıcının bu kaynak türü için yazma erişimi olmalıdır. Tüm kaynak türleri için etiketleri uygulamak için [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rol. Yalnızca bir kaynak türü için etiketleri uygulamak için bu kaynak için katkıda bulunan rolü kullanın. Örneğin, sanal makinelere etiketleri uygulamak için kullanma [sanal makine Katılımcısı](../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="powershell"></a>PowerShell

Bu makaledeki örnekler Azure PowerShell’in 6.0 veya üzeri bir sürümünü gerektirir. 6.0 veya sonraki sürüme sahip değilseniz [sürümünüzü güncelleştirin](/powershell/azure/azurerm/install-azurerm-ps).

Bir *kaynak grubunun* mevcut etiketlerini görmek şunu kullanın:

```azurepowershell-interactive
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Bu betik aşağıdaki biçimde veri döndürür:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

*Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu kullanın:

```azurepowershell-interactive
(Get-AzureRmResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

*Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu da kullanabilirsiniz:

```azurepowershell-interactive
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

*Belirli bir etikete sahip kaynak gruplarını* almak için şunu kullanın:

```azurepowershell-interactive
(Get-AzureRmResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

*Belirli bir etikete sahip kaynakları* almak için şunu kullanın:

```azurepowershell-interactive
(Get-AzureRmResource -Tag @{ Dept="Finance"}).Name
```

Alınacak *belirli bir etiket adı olan kaynakları*, kullanın:

```azurepowershell-interactive
(Get-AzureRmResource -TagName Dept).Name
```

Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Bu nedenle, kaynakta veya kaynak grubunda etiketlerin mevcut olup olmamasına bağlı olarak farklı bir yaklaşım kullanmanız gerekir.

*Mevcut etiketi olmayan bir kaynak grubuna* etiket eklemek için şunu kullanın:

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

*Mevcut etiketleri olan bir kaynak grubuna* etiket eklemek için, mevcut etiketleri alın, yeni etiketi ekleyin ve etiketleri yeniden uygulayın:

```azurepowershell-interactive
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

*Mevcut etiketi olmayan bir kaynağa* etiket eklemek için şunu kullanın:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

*Mevcut etiketi olan bir kaynağa* etiket eklemek için şunu kullanın:

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved")
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Bir kaynak grubundaki tüm etiketleri *kaynaklardaki mevcut etiketleri korumadan* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```azurepowershell-interactive
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Bir kaynak grubundaki tüm etiketleri *yinelenmeyen kaynaklardaki mevcut etiketleri koruyarak* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```azurepowershell-interactive
$group = Get-AzureRmResourceGroup "examplegroup"
if ($null -ne $group.Tags) {
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirin:

```azurepowershell-interactive
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>Azure CLI

Bir *kaynak grubunun* mevcut etiketlerini görmek şunu kullanın:

```azurecli
az group show -n examplegroup --query tags
```

Bu betik aşağıdaki biçimde veri döndürür:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Veya mevcut etiketlerini görmek için bir *belirtilen bir adı, türü ve kaynak grubu kaynağın*, kullanın:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Bir kaynak koleksiyonu döngü sırasında kaynak tarafından kaynak kimliği göstermek isteyebilirsiniz Bu makalenin sonraki bölümlerinde tam bir örnek gösterilmektedir. *Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu kullanın:

```azurecli
az resource show --id <resource-id> --query tags
```

Belirli bir etikete sahip kaynak gruplarını almak için kullanın `az group list`:

```azurecli
az group list --tag Dept=IT
```

Belirli bir etiket ve değere sahip tüm kaynakları almak için kullanın `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Bu nedenle, kaynakta veya kaynak grubunda etiketlerin mevcut olup olmamasına bağlı olarak farklı bir yaklaşım kullanmanız gerekir.

*Mevcut etiketi olmayan bir kaynak grubuna* etiket eklemek için şunu kullanın:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

*Mevcut etiketi olmayan bir kaynağa* etiket eklemek için şunu kullanın:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Etiketler zaten olan bir kaynağa etiket eklemek için mevcut etiketleri alın, bu değeri yeniden biçimlendirme ve mevcut ve yeni etiketleri yeniden uygulayın:

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Bir kaynak grubundaki tüm etiketleri *kaynaklardaki mevcut etiketleri korumadan* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Bir kaynak grubundan kaynaklara tüm etiketleri uygulamak ve *kaynaklardaki mevcut etiketleri korumak*, aşağıdaki betiği kullanın:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Şablonlar

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST API

Azure portalı ve ikisi de PowerShell [Resource Manager REST API'si](https://docs.microsoft.com/rest/api/resources/) arka planda. Başka bir ortama etiketleme tümleştirme gerekiyorsa, etiketleri kullanarak alabilirsiniz **alma** kaynak kimliği ve güncelleştirme kullanarak etiket kümesinin bir **düzeltme eki** çağırın.

## <a name="tags-and-billing"></a>Etiketleri ve faturalandırma

Faturalama verilerinize gruplandırmak için etiketleri kullanabilirsiniz. Örneğin, birden çok VM farklı kuruluşlarda çalıştırıyorsanız, maliyet merkezi tarafından grubu kullanımı için etiketleri kullanın. Etiketleri, maliyetler üretim ortamında çalışan VM'ler için fatura kullanımı gibi çalışma zamanı ortamı tarafından kategorilere ayırmak için de kullanabilirsiniz.

Etiketler hakkında bilgi alabilirsiniz [Azure kaynak kullanım ve RateCard API'leri](../billing/billing-usage-rate-card-overview.md) veya kullanım virgülle ayrılmış değerler (CSV) dosyası. Kullanım dosyası indirin [Azure hesap Merkezi](https://account.azure.com/Subscriptions) veya Azure portalında. Daha fazla bilgi için [indirin veya Azure faturanızı ve günlük kullanım verilerinizi görüntüleme](../billing/billing-download-azure-invoice-daily-usage-date.md). Azure hesap Merkezi'nden kullanım dosyayı indirmeyi seçin **sürüm 2**. Faturalandırma ile etiketler destekleyen hizmetler için etiketler görünür **etiketleri** sütun.

REST API işlemleri için bkz: [Azure faturalandırma REST API Başvurusu](/rest/api/billing/).

## <a name="next-steps"></a>Sonraki adımlar

* Tüm kaynak türleri etiketleri destekler. Bir kaynak türü için bir etiket uygulamak, belirlemek için bkz: [etiket Azure kaynakları için destek](tag-support.md).
* Özelleştirilmiş ilkeler kullanarak, aboneliğinizi arasında kısıtlamaları ve kuralları uygulayabilirsiniz. Tanımladığınız bir ilke, tüm kaynakların belirli bir etiket için bir değer olmasını gerektirebilir. Daha fazla bilgi için [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
* Portalı kullanarak bir giriş için bkz. [Azure kaynaklarınızı yönetmek için Azure portalını kullanarak](resource-group-portal.md).  
