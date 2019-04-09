---
title: Büyük veri kümeleriyle çalışma
description: Ve büyük veri kümeleri ile Azure kaynak Graph çalışırken öğrenin.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/01/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 40aa8ca0ebfcc8eb5b686143960af1441768622a
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59058400"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Büyük bir Azure kaynak veri kümeleri ile çalışma

Azure kaynak grafiği ile çalışma ve Azure ortamınızda kaynakların hakkında bilgi almak için tasarlanmıştır. Kaynak Grafiği, bu verileri hızlanma hatta binlerce kayıt sorgulanırken hale getirir. Kaynak Grafiği, bu büyük veri kümeleriyle çalışmak için birkaç seçenek vardır.

## <a name="data-set-result-size"></a>Veri kümesi sonucu boyutu

Varsayılan olarak, kaynak grafiği yalnızca döndürmek için herhangi bir sorgu sınırlar **100** kaydeder. Bu denetim, büyük veri kümelerinde ortaya çıkabilecek yanlışlıkla sorgularından hem kullanıcı hem de hizmet korur. Bu olay, bir müşteri bulun ve kaynakları kendi belirli gereksinimlerine uygun şekilde filtrelemek için sorguları ile denemeler gibi en sık gerçekleşir. Bu denetim kullanmaktan farklıdır [üst](/azure/kusto/query/topoperator) veya [sınırı](/azure/kusto/query/limitoperator) sonuçları sınırlandırmak için Azure Veri Gezgini dil işleçleri.

> [!NOTE]
> Kullanırken **ilk**, sonuçları ile en az bir sütuna göre sıralamak için önerilir `asc` veya `desc`. Döndürülen sonuçlar, sıralama yapmadan rastgele ve yinelenebilir değil.

Varsayılan sınır, kaynak grafiği ile etkileşim kurmanın tüm yöntemleri aracılığıyla kılınabilir. Aşağıdaki örnekler için veri kümesi boyutu sınırı değiştirmek nasıl _200_:

```azurecli-interactive
az graph query -q "project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -First 200
```

İçinde [REST API](/rest/api/azureresourcegraph/resources/resources), Denetim **$top** parçası **QueryRequestOptions**.

Denetimi _en kısıtlayıcı_ kazanacak. Sorgunuzu kullanıyorsa, örneğin, **üst** veya **sınırı** işleçler ve daha fazla kayıt sonuçlanır **ilk**, en fazla kayıt döndürülen olacak eşit**İlk**. Benzer şekilde, varsa **üst** veya **sınırı** küçük olduğuna **ilk**, kayıt döndürülen kümesi tarafından yapılandırılan daha küçük değer olacaktır **üst** veya **sınırı**.

**İlk** değeri izin verilen en fazla'şu anda sahip _5000_.

## <a name="skipping-records"></a>Kayıtları

Büyük veri kümeleriyle çalışmak için sonraki seçeneği **atla** denetimi. Bu denetim, sorgunuzun üzerinden geçin veya sonuç döndürülmeden önce tanımlanan kayıt sayısını Atla sağlar. **Skip** hedefi olduğu sonuç kümesi ortasında yere kayıt sırasında almak için anlamlı bir şekilde sonuçları sıralamak sorgular için kullanışlıdır. Gerekli sonuç döndürülen veri kümesinin sonunda ise farklı bir sıralama yapılandırmasını kullanın ve sonuçları yerine veri kümesinin başından almak için daha verimlidir.

> [!NOTE]
> Kullanırken **atla**, sonuçları ile en az bir sütuna göre sıralamak için önerilir `asc` veya `desc`. Döndürülen sonuçlar, sıralama yapmadan rastgele ve yinelenebilir değil.

Aşağıdaki örnekler ilk atlama _10_ kayıtları bir sorgu neden, bunun yerine döndürülen sonuç başlangıç 11 kayıtla ayarlayın:

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

İçinde [REST API](/rest/api/azureresourcegraph/resources/resources), Denetim **$skip** parçası **QueryRequestOptions**.

## <a name="paging-results"></a>Disk belleği sonuçları

Bir sonuç kümesi işleme kayıtlarını içeren daha küçük kümelerine ayırmak gerekli olduğunda veya bir sonuç kümesi değeri izin verilen üst sınırı aşacağından _1000_ kayıtların döndürüleceği, disk belleği kullanın. [REST API](/rest/api/azureresourcegraph/resources/resources) **QueryResponse** kümesine parçalanmış sonuçlarının belirtmek için değerler sunar: **resultTruncated** ve **$skipToken** .
**resultTruncated** ek kayıtlar varsa tüketici değil yanıtta döndürülen bildiren bir Boole değeri. Bu durum da olabilir ne zaman tanımlanan **sayısı** özelliği küçüktür **totalRecords** özelliği. **totalRecords** kaç sorguyla eşleşen kayıt tanımlar.

Zaman **resultTruncated** olduğu **true**, **$skipToken** özelliği, yanıt olarak ayarlanır. Bu değer aynı sorgu ve abonelik değerlerle sonraki sorguyla eşleşen kayıt kümesini almak için kullanılır.

> [!IMPORTANT]
> Sorgu gerekir **proje** **kimliği** sırayla çalışması sayfalandırma alan. Sorgudan eksikse, REST API yanıtı içermez **$skipToken**.

Bir örnek için bkz. [sonraki sayfa sorgu](/rest/api/azureresourcegraph/resources/resources#next_page_query) REST API belgeleri.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md)
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md)
- [Kaynakları keşfetmeyi](explore-resources.md) öğrenin