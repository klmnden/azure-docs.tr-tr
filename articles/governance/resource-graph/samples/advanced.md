---
title: Gelişmiş Azure Kaynak Grafiği sorguları
description: Bazı gelişmiş sorguları çalıştırmak için Azure Kaynak Grafiği’ni kullanın.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: a9281bc333b3edd12501a6813f12b9e7ad1bf59f
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219276"
---
# <a name="advanced-resource-graph-queries"></a>Gelişmiş Kaynak Grafiği sorguları

Azure Kaynak Grafiği ile sorguları anlamanın il adımı, [Sorgu Dili](../concepts/query-language.md)’nin temel bir anlayışıdır. [ Azure Veri Gezgini](../../../data-explorer/data-explorer-overview.md)’ne önceden aşina değilseniz, aradığınız kaynaklar için isteklerin nasıl oluşturulacağını anlamak üzere temelleri gözden geçirmeniz önerilir.

Aşağıdaki gelişmiş sorguları inceleyeceğiz:

> [!div class="checklist"]
> - [VMSS kapasite ve boyutunu alma](#vmss-capacity)
> - [Tüm etiket adlarını listeleme](#list-all-tags)
> - [Normal ifade tarafından eşleştirilen sanal makineler](#vm-regex)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="language-support"></a>Dil desteği

Azure CLI (bir uzantı yoluyla) ve Azure PowerShell (bir modül yoluyla), Azure Kaynak Grafiği’ni destekler. Aşağıdaki sorgulardan herhangi birini gerçekleştirmeden önce ortamınızın hazır olduğunu kontrol edin. Seçtiğiniz kabuk ortamını yükleme ve doğrulama adımları için, bkz. [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module).

## <a name="vmss-capacity"></a>VMSS kapasite ve boyutunu alma

Bu sorgu, sanal makine ölçek kümesi (VMSS) kaynaklarını arar ve sanal makine boyutu ve ölçek kümesinin kapasitesini içeren çeşitli ayrıntıları alır. Bu bilgi, sıralanabilmesi için, kapasiteyi bir sayıya dönüştürmek amacıyla `toint()` işlevini kullanır. Bu, ayrıca özel adlandırılmış özelliklere döndürülen değerleri yeniden adlandırır.

```Query
where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

```azurecli-interactive
az graph query -q "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

```powershell
Search-AzureRmGraph -Query "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

## <a name="list-all-tags"></a>Tüm etiket adlarını listeleme

Bu sorgu etiketle başlar ve tüm benzersiz etiket adlarını ve bunlara karşılık gelen türleri listeleyen bir JSON nesnesi oluşturur.

```Query
project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "project tags | summarize buildschema(tags)"
```

```powershell
Search-AzureRmGraph -Query "project tags | summarize buildschema(tags)"
```

## <a name="vm-regex"></a>Normal ifade tarafından eşleştirilen sanal makineler

Bu sorgu, [normal ifadeyle](/dotnet/standard/base-types/regular-expression-language-quick-reference) (_regex_ olarak bilinir) eşleşen sanal makineleri arar.
**Eşleşir normal ifade @**, eşleşecek normal ifadeyi, yani **^Contoso(.*)[0-9]+$** ifadesini tanımlamamızı sağlar. Bu normal ifade tanımı şöyle açıklanmıştır:

- `^` - Eşleşme dizenin başında başlamalıdır.
- `Contoso` - Eşleştirdiğimiz çekirdek dize (büyük/küçük harfe duyarlı).
- `(.*)` - Bir alt ifade eşleşmesi:
  - `.` - Herhangi bir tek karakterle eşleşir (yeni bir satır hariç).
  - `*` - Önceki öğeyle sıfır veya daha fazla kez eşleşir.
- `[0-9]` - 0’dan 9’a kadar olan sayılar için karakter grubu eşleşmesi.
- `+` - Önceki öğeyle bir veya daha fazla kez eşleşir.
- `$` - Önceki öğenin eşleşmesi dizenin sonunda gerçekleşmelidir.

Ada göre eşleşmeden sonra sorgu, adı ve siparişleri ada göre artan şekilde yansıtır.

```Query
where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

```azurecli-interactive
az graph query -q "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [başlangıç sorguları](starter.md) örnekleri
- [Sorgu dili](../concepts/query-language.md) hakkında daha fazla bilgi edinin
- [Kaynakları keşfetmeyi](../concepts/explore-resources.md) öğrenin