---
title: Etiket mantıksal kuruluşunuz için Azure kaynaklarını | Microsoft Docs
description: Faturalama ve yönetmek için Azure kaynaklarını düzenlemek için etiketleri uygulamak gösterilmiştir.
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
ms.date: 05/16/2018
ms.author: tomfitz
ms.openlocfilehash: 6f9b2b04c3bdfc02065e2a01e1975d734a5f53ac
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Azure kaynaklarınızı düzenlemek için etiketleri kullanma

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="powershell"></a>PowerShell

Bu makaledeki örneklerde sürüm 6.0 veya üstü Azure PowerShell gerektirir. Sürüm 6.0 veya üstü, yoksa [sürümünüzü güncelleştirme](/powershell/azure/install-azurerm-ps).

Bir *kaynak grubunun* mevcut etiketlerini görmek şunu kullanın:

```powershell
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

```powershell
(Get-AzureRmResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

*Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu da kullanabilirsiniz:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

*Belirli bir etikete sahip kaynak gruplarını* almak için şunu kullanın:

```powershell
(Get-AzureRmResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

*Belirli bir etikete sahip kaynakları* almak için şunu kullanın:

```powershell
(Get-AzureRmResource -Tag @{ Dept="Finance"}).Name
```

Alınacak *bir özel etiket adına sahip kaynaklar*, kullanın:

```powershell
(Get-AzureRmResource -TagName Dept).Name
```

Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Bu nedenle, kaynakta veya kaynak grubunda etiketlerin mevcut olup olmamasına bağlı olarak farklı bir yaklaşım kullanmanız gerekir.

*Mevcut etiketi olmayan bir kaynak grubuna* etiket eklemek için şunu kullanın:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

*Mevcut etiketleri olan bir kaynak grubuna* etiket eklemek için, mevcut etiketleri alın, yeni etiketi ekleyin ve etiketleri yeniden uygulayın:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

*Mevcut etiketi olmayan bir kaynağa* etiket eklemek için şunu kullanın:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

*Mevcut etiketi olan bir kaynağa* etiket eklemek için şunu kullanın:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved") 
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Bir kaynak grubundaki tüm etiketleri *kaynaklardaki mevcut etiketleri korumadan* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Bir kaynak grubundaki tüm etiketleri *yinelenmeyen kaynaklardaki mevcut etiketleri koruyarak* gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```powershell
$group = Get-AzureRmResourceGroup "examplegroup"
if ($group.Tags -ne $null) {
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
    }
}
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirin:

```powershell
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

Veya varolan etiketleri için görmek için bir *belirtilen adını, türünü ve kaynak grubuna sahip kaynak*, kullanın:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Kaynaklar koleksiyonu döngü sırasında kaynak tarafından kaynak kimliği Göster isteyebilirsiniz Bu makalenin sonraki bölümlerinde tam bir örnek gösterilmektedir. *Kaynak kimliği belirtilmiş bir kaynağın* mevcut etiketlerini görmek için şunu kullanın:

```azurecli
az resource show --id <resource-id> --query tags
```

Belirli bir etikete sahip kaynak gruplarını elde etmek için kullanın `az group list`:

```azurecli
az group list --tag Dept=IT
```

Özel etiket ve değerine sahip tüm kaynakları almak için `az resource list`:

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

Etiketleri zaten olan bir kaynağın etiketleri eklemek için varolan etiketleri almak, bu değeri yeniden biçimlendirin ve var olan ve yeni etiketler yeniden uygulayın: 

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

Tüm etiketleri kaynaklarını için bir kaynak grubundan uygulamak için ve *kaynaklardaki varolan etiketleri korumak*, aşağıdaki komut dosyasını kullanın:

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

Azure portalı ve her ikisini de kullanmanız PowerShell [Resource Manager REST API'si](https://docs.microsoft.com/rest/api/resources/) arka planda. Başka bir ortama etiketleme tümleştirme gerekiyorsa, etiketleri kullanarak alabileceğiniz **almak** kaynak kimliği ile güncelleştirme kullanarak etiketleri kümesi üzerinde bir **düzeltme eki** çağırın.

## <a name="tags-and-billing"></a>Etiketleri ve faturalama

Faturalama verileriniz gruplandırmak için etiketler kullanın. Örneğin, birden çok VM farklı kuruluşlarda çalıştırıyorsanız, etiketleri Grup kullanım için maliyet merkezi tarafından kullanın. Etiketler, maliyetleri üretim ortamında çalışan sanal makineler için fatura kullanımı gibi çalışma zamanı ortamı tarafından kategorilere ayırmak için de kullanabilirsiniz.

Etiketler hakkında bilgi alabilir [Azure kaynak kullanımı ve RateCard API'leri](../billing/billing-usage-rate-card-overview.md) veya kullanım virgülle ayrılmış değerler (CSV) dosyası. Kullanım dosyasını indirin [Azure hesap portalı](https://account.windowsazure.com/) veya [EA portal](https://ea.azure.com). Faturalandırma bilgileri programlı erişim hakkında daha fazla bilgi için bkz: [Microsoft Azure kaynak tüketimini Öngörüler elde](../billing/billing-usage-rate-card-overview.md). REST API işlemleri için bkz: [Azure faturalama REST API Başvurusu](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Faturalama etiketleriyle destekleyen hizmetler için kullanım CSV yüklediğinizde etiketler görünür **etiketleri** sütun. Daha fazla bilgi için bkz: [faturanızı anlamak için Microsoft Azure](../billing/billing-understand-your-bill.md).

![Faturalama etiketlerinde bakın](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Sonraki adımlar

* Özelleştirilmiş ilkeler kullanarak aboneliğinizi arasında kısıtlamaları ve kuralları uygulayabilirsiniz. Tanımladığınız bir ilke tüm kaynakların belirli bir etiket için bir değere sahip gerektirebilir. Daha fazla bilgi için bkz: [Azure ilke nedir?](../azure-policy/azure-policy-introduction.md)
* Kaynakları dağıtırken Azure PowerShell kullanarak bir giriş için bkz: [Azure PowerShell kullanarak Azure Resource Manager ile](powershell-azure-resource-manager.md).
* Kaynakları dağıtırken Azure CLI kullanarak bir giriş için bkz: [Mac, Linux ve Windows Azure Resource Manager ile Azure CLI kullanarak](xplat-cli-azure-resource-manager.md).
* Portalı kullanarak bir giriş için bkz: [Azure kaynaklarınızı yönetmek için Azure portalını kullanarak](resource-group-portal.md).  
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).
