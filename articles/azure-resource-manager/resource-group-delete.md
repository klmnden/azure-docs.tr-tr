---
title: Kaynak grubu ve Azure Resource Manager kaynaklarını - sil
description: Bir kaynak grubunun silinmesi, Azure Resource Manager kaynakların silinmesini nasıl sıralar açıklar. Bu, yanıt kodları ve Resource Manager silme başarılı olup olmadığını belirlemek için bunları nasıl işlediğini açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 12/09/2018
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 18990b51b5ff2184197db48fd139d63750626663
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204213"
---
# <a name="azure-resource-manager-resource-group-deletion"></a>Azure Resource Manager kaynak grubunu silme

Bu makalede, bir kaynak grubu sildiğinizde, Azure Resource Manager kaynakların silinmesini nasıl sıralar açıklanır.

## <a name="determine-order-of-deletion"></a>Silme sırasını belirleme

Bir kaynak grubu sildiğinizde, Resource Manager kaynakları silmek için sırasını belirler. Aşağıdaki sırayı kullanır:

1. Tüm alt (iç içe) kaynaklar silinir.

2. Diğer kaynakları yönetmek kaynaklar ardından silinir. Bir kaynak olabilir `managedBy` farklı bir kaynak, yönettiği belirtmek için özelliğini ayarlayın. Bu özelliği ayarlandığında, diğer kaynak yöneten kaynak önce diğer kaynaklar silinir.

3. Önceki iki kategoriye sonra kalan kaynaklar silinir.

## <a name="resource-deletion"></a>Kaynak silme

Sırasını belirlendikten sonra kaynak yöneticisi her kaynak için bir silme işlemi verir. Herhangi bir bağımlılığın devam etmeden önce tamamlanmasını bekler.

Zaman uyumlu işlemler için beklenen başarılı yanıt kodları şunlardır:

* 200
* 204
* 404

Zaman uyumsuz işlemleri için beklenen başarılı yanıt, 202. Kaynak Yöneticisi konum veya silme zaman uyumsuz işlemin durumunu belirlemek için azure-zaman uyumsuz işlemi başlığındaki izler.
  
### <a name="errors"></a>Hatalar

Bir hata silme işlemi geri döndüğünde, Resource Manager DELETE çağrısı yeniden dener. Yeniden denemeler için 429 ve 408 durum kodu 5xx gerçekleşir. Varsayılan olarak, yeniden deneme süre 15 dakikadır.

## <a name="after-deletion"></a>Silindikten sonra

Kaynak Yöneticisi bir GET çağrısı silmeye çalıştığınız her kaynaktaki verir. Yanıtını bu alma çağrısı, 404 olması beklenir. Resource Manager bir 404 girdiğinde, başarıyla tamamladınız silme göz önünde bulundurur. Resource Manager kaynak önbelleğinden kaldırır.

Ancak, kaynaktaki alma çağrısı, bir 200 ya da 201 döndürürse, Resource Manager kaynak yeniden oluşturur.

### <a name="errors"></a>Hatalar

GET işlemi hata verirse, Resource Manager aşağıdaki hata kodunu alma yeniden deneme:

* 100'den küçük
* 408
* 429
* 500'den büyük

Diğer hata kodları için Resource Manager kaynak silme işlemi başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* Resource Manager kavramları anlamak için bkz. [Azure Resource Manager'a genel bakış](resource-group-overview.md).
* Silme komutlar için bkz [PowerShell](/powershell/module/az.resources/Remove-AzResourceGroup), [Azure CLI](/cli/azure/group?view=azure-cli-latest#az-group-delete), ve [REST API](/rest/api/resources/resourcegroups/delete).
