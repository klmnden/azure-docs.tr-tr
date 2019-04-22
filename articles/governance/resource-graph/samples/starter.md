---
title: Başlangıç sorgu örnekleri
description: Bazı başlangıç çalıştırmak için Azure Kaynak Grafiği kullanın, kaynaklar,'sıralama sayım kaynaklar dahil olmak üzere veya belirli bir etikete göre sorgular.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/04/2019
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 2ba48e2a21bdee0c5698bdfa314dd3bf462c1c7e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59267778"
---
# <a name="starter-resource-graph-queries"></a>Başlangıç Kaynak Grafiği sorguları

Azure Kaynak Grafiği ile sorguları anlamanın il adımı, [Sorgu Dili](../concepts/query-language.md)’nin temel bir anlayışıdır. [Azure Veri Gezgini](../../../data-explorer/data-explorer-overview.md)’ne önceden aşina değilseniz, aradığınız kaynaklar için isteklerin nasıl oluşturulacağını anlamak üzere temelleri gözden geçirmeniz önerilir.

Aşağıdaki başlangıç sorgularını inceleyeceğiz:

> [!div class="checklist"]
> - [Azure kaynaklarını sayma](#count-resources)
> - [Ada göre sıralanmış kaynakları listeleme](#list-resources)
> - [Tüm sanal makineleri ada göre azalan düzende sıralı olarak gösterme](#show-vms)
> - [Ada ve işletim sistemi türüne göre ilk beş sanal makineyi gösterme](#show-sorted)
> - [Sanal makineleri işletim sistemi türüne göre sayma](#count-os)
> - [Depolama içeren kaynakları göster](#show-storage)
> - [Tüm genel IP adreslerini listele](#list-publicip)
> - [Aboneliğe göre yapılandırılmış IP adreslerine sahip kaynakları sayma](#count-resources-by-ip)
> - [Belirli bir etiket değerine sahip kaynakları listeleme](#list-tag)
> - [Belirli bir değerine sahip tüm depolama hesaplarını listeleme](#list-specific-tag)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="language-support"></a>Dil desteği

Azure CLI (bir uzantı yoluyla) ve Azure PowerShell (bir modül yoluyla), Azure Kaynak Grafiği’ni destekler. Aşağıdaki sorgulardan herhangi birini çalıştırmadan önce ortamınızın hazır olduğundan emin olun. Seçtiğiniz kabuk ortamını yükleme ve doğrulama adımları için, bkz. [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module).

## <a name="count-resources"></a>Azure kaynaklarını sayma

Bu sorgu, erişiminiz bulunan aboneliklerde var olan Azure kaynaklarının sayısını döndürür. Ayrıca, seçtiğiniz kabukta uygun Azure Kaynak Grafı bileşenlerinin yüklü ve çalışma düzeninde olduğunu doğrulamak için iyi bir sorgudur.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```azurepowershell-interactive
Search-AzGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Ada göre sıralanmış kaynakları listeleme

Bu sorgu herhangi bir türdeki kaynakların yalnızca **name**, **type** ve **location** özelliklerini döndürür. `order by` kullanarak özellikleri **name** özelliğine göre artan (`asc`) düzende sıralar.

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```azurepowershell-interactive
Search-AzGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>İsme göre sıralanmış tüm sanal makineleri azalan düzende göster

Yalnızca sanal makineleri (`Microsoft.Compute/virtualMachines` türündeki) listelemek için sonuçlarda **type** özelliğini eşleştirebiliriz. Önceki sorguya benzer şekilde `desc`, `order by`’yi azalan olması için değiştirir. Eşleme türündeki `=~`, Kaynak Grafiği’nin büyük/küçük harfe duyarlı olmadığını bildirir.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="show-sorted"></a>Ada ve işletim sistemi türüne göre ilk beş sanal makineyi gösterme

Bu sorgu `limit`’i yalnızca ada göre sıralanmış beş eşleşen kaydı almak için kullanır. Azure kaynağının türü `Microsoft.Compute/virtualMachines`’dir. `project`, Azure Kaynak Grafiği’ne hangi özelliklerin dahil edileceğini bildirir.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Sanal makineleri işletim sistemi türüne göre sayma

Bir önceki sorguyu oluşturmada, hâlâ `Microsoft.Compute/virtualMachines` türündeki Azure kaynaklarına göre sınırlandırıyoruz, ancak geri gönderilen kayıtların sayısını sınırlandırmıyoruz.
Bunu yerine, değerleri (bu örnekte `properties.storageProfile.osDisk.osType`) özelliğe göre gruplandırma ve toplamayı tanımlamak için `summarize` ve `count()` kullandık. Bu dizenin tam nesnede nasıl göründüğüne ilişkin bir örnek için, bkz. [ kaynakları keşfetme - sanal makine bulma](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Aynı sorguyu yazmanın farklı bir yolu da, bir özelliği `extend` yapmak ve sorgu içinde kullanılması için, bu durumda **os** olan geçici bir ad verilmesidir. **os** daha sonra, bir önceki örnekte olduğu gibi `summarize` ve `count()` tarafından kullanılır.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> `=~`, büyük/küçük harfe duyarlı olmayan eşleşmeye izin verirken, sorgudaki özelliklerin (örneğin, **properties.storageProfile.osDisk.osType** gibi) kullanımının, durumun doğru olmasını gerektirdiğini unutmayın. Özellik yanlış durumsa, yine de bir değer döndürebilir, ancak gruplandırma veya özetleme yanlış olur.

## <a name="show-storage"></a>Depolama içeren kaynakları göster

Eşleştirilecek türü şekilde açık olarak tanımlamak yerine, bu örnek sorgu, **depo** sözcüğünü `contains` eden tüm Azure kaynaklarını bulur.

```Query
where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "where type contains 'storage' | distinct type"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Tüm genel IP adreslerini listele

Önceki sorguya benzer şekilde, **publicIPAddresses** sözcüğünü içeren bir tür olan her şeyi bulun.
Bu sorgu sonuçları yalnızca dahil etmek için bu düzeni genişletir. burada **properties.ipAddress**
`isnotempty`yalnızca döndürülmesini **properties.ipAddress**ve `limit` üst sonuçları
100. Seçtiğiniz kabuğa bağlı olarak tırnak işaretlerinin dışına çıkmanız gerekebilir.

```Query
where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

## <a name="count-resources-by-ip"></a>Aboneliğe göre yapılandırılmış IP adreslerine sahip kaynakları sayma

Önceki örnek sorguyu kullanarak ve `summarize` ile `count()` ekleyerek, yapılandırılmış IP adreslerine sahip kaynakların aboneliğe göre listesini elde edebiliriz.

```Query
where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

## <a name="list-tag"></a>Belirli bir etiket değerine sahip kaynakları listeleme

Sonuçları, etiket gibi Azure kaynak türünden başka özelliklere göre de sınırlandırabiliriz. Bu örnekte, Azure kaynaklarını **Dahili** değerine sahip **Ortam** etiket adıyla filtreliyoruz.

```Query
where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name"
```

```azurepowershell-interactive
Search-AzGraph -Query "where tags.environment=~'internal' | project name"
```

Kaynağın sahip olduğu etiketleri ve değerlerini de sağlamak için `project` sözcüğüne **tags** özelliğini ekleyin.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```azurepowershell-interactive
Search-AzGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Belirli bir değerine sahip tüm depolama hesaplarını listeleme

Önceki örneğin filtre işleviyle birleştirerek Azure kaynak türünü **type** özelliğine göre filtreleyin. Bu sorgu da aramayı belirli bir etiket adına ve değerine sahip olan belirli Azure kaynağı türleriyle sınırlar.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> Bu örnek, eşleştirme için `=~` koşullu yerine `==` kullanır. `==` büyük küçük harfe duyarlı bir eşleşmedir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](../concepts/query-language.md) hakkında daha fazla bilgi edinin
- [Kaynakları keşfetmeyi](../concepts/explore-resources.md) öğrenin
- Bkz. [Gelişmiş sorgular](advanced.md) örnekleri