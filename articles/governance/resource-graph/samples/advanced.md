---
title: Gelişmiş sorgu örnekleri
description: VMSS kapasite, kullanılan, tüm etiketleri listeleme gibi bazı gelişmiş sorgular çalıştırmak için Azure kaynak Graph eşleşen sanal makineleri ile normal ifadeler kullanın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 7684ae6b4ddb6320efc62ef6f9963bef1b9a66fa
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691977"
---
# <a name="advanced-resource-graph-queries"></a>Gelişmiş Kaynak Grafiği sorguları

Azure Kaynak Grafiği ile sorguları anlamanın il adımı, [Sorgu Dili](../concepts/query-language.md)’nin temel bir anlayışıdır. [Azure Veri Gezgini](../../../data-explorer/data-explorer-overview.md)’ne önceden aşina değilseniz, aradığınız kaynaklar için isteklerin nasıl oluşturulacağını anlamak üzere temelleri gözden geçirmeniz önerilir.

Aşağıdaki gelişmiş sorguları inceleyeceğiz:

> [!div class="checklist"]
> - [Sanal makine ölçek kümesi kapasitesini ve boyutunu alma](#vmss-capacity)
> - [Tüm etiket adlarını listeleme](#list-all-tags)
> - [Normal ifade tarafından eşleştirilen sanal makineler](#vm-regex)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="language-support"></a>Dil desteği

Azure CLI (bir uzantı yoluyla) ve Azure PowerShell (bir modül yoluyla), Azure Kaynak Grafiği’ni destekler. Aşağıdaki sorgulardan herhangi birini çalıştırmadan önce ortamınızın hazır olduğundan emin olun. Seçtiğiniz kabuk ortamını yükleme ve doğrulama adımları için, bkz. [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module).

## <a name="vmss-capacity"></a>Sanal makine ölçek kümesi kapasitesini ve boyutunu alma

Bu sorgu, sanal makine ölçek kümesi kaynaklarını arar ve sanal makine boyutu ve ölçek kümesinin kapasitesini içeren çeşitli ayrıntıları alır. Sorgu, sıralanabilmesi için ve kapasiteyi bir sayıya dönüştürmek amacıyla `toint()` işlevini kullanır. Son olarak sütunlar, özel olarak adlandırılmış özellikler kullanılarak yeniden adlandırılır.

```kusto
where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

```azurecli-interactive
az graph query -q "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

## <a name="list-all-tags"></a>Tüm etiket adlarını listeleme

Bu sorgu etiketle başlar ve tüm benzersiz etiket adlarını ve bunlara karşılık gelen türleri listeleyen bir JSON nesnesi oluşturur.

```kusto
project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "project tags | summarize buildschema(tags)"
```

```azurepowershell-interactive
Search-AzGraph -Query "project tags | summarize buildschema(tags)"
```

## <a name="vm-regex"></a>Normal ifade tarafından eşleştirilen sanal makineler

Bu sorgu, [normal ifadeyle](/dotnet/standard/base-types/regular-expression-language-quick-reference) (_regex_ olarak bilinir) eşleşen sanal makineleri arar.
**Eşleşen normal ifade \@**  olan eşleştirmek için normal ifade tanımlamak sağlıyor `^Contoso(.*)[0-9]+$`. Bu normal ifade tanımı şöyle açıklanmıştır:

- `^` - Eşleşme dizenin başında başlamalıdır.
- `Contoso` - Büyük/küçük harfe duyarlı dize.
- `(.*)` - Bir alt ifade eşleşmesi:
  - `.` - Herhangi bir tek karakterle eşleşir (yeni bir satır hariç).
  - `*` - Önceki öğeyle sıfır veya daha fazla kez eşleşir.
- `[0-9]` - 0’dan 9’a kadar olan sayılar için karakter grubu eşleşmesi.
- `+` - Önceki öğeyle bir veya daha fazla kez eşleşir.
- `$` - Önceki öğenin eşleşmesi dizenin sonunda gerçekleşmelidir.

Ada göre eşleşmeden sonra sorgu, adı ve siparişleri ada göre artan şekilde yansıtır.

```kusto
where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

```azurecli-interactive
az graph query -q "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorguları](starter.md) örnekleri
- [Sorgu dili](../concepts/query-language.md) hakkında daha fazla bilgi edinin
- [Kaynakları keşfetmeyi](../concepts/explore-resources.md) öğrenin
