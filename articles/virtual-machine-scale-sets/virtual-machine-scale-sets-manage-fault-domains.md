---
title: Azure sanal makine ölçek kümeleri, hata etki alanlarını yönetme | Microsoft Docs
description: Sanal makine ölçek kümesi oluşturma sırasında FD doğru sayıda seçmeyi öğrenebilirsiniz.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: drewm
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: rajraj
ms.openlocfilehash: bab264769576b6e5478236c452d7de920d887c1a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60617988"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Doğru sayıda sanal makine ölçek kümesi için hata etki alanlarını seçme
Sanal makine ölçek kümeleri, beş hata etki alanları ile Azure bölgelerindeki hiçbir bölge ile varsayılan olarak oluşturulur. Sanal makine ölçek kümeleri bölgesel dağıtımını destekleyen bölgeleri için hata etki alanı sayısı varsayılan değerini her bölge için 1 ' dir. FD = 1, bu durumda gelir ölçek kümesine ait olan sanal makine örnekleri en iyi çaba ilkesine göre birçok raf arasında yayılır.

Ayrıca, yönetilen disk hata etki alanları sayısı ile ölçek kümesi hata etki alanları sayısı hizalama göz önünde bulundurun. Bu hizalama, tüm yönetilen disk hata etki alanı dışı kalırsa, çekirdek kaybı önlemeye yardımcı olabilir. FD sayısı, yönetilen diskler sayısına eşit veya daha az hata etki alanları her bölge içinde ayarlanabilir. Bu [belge](../virtual-machines/windows/manage-availability.md) yönetilen diskler hata etki alanlarının sayısı bölgeye göre hakkında bilgi edinmek için.

## <a name="rest-api"></a>REST API
Özelliği ayarlayabilirsiniz `properties.platformFaultDomainCount` 1, 2 veya 3 (5. belirtilmezse, varsayılan). REST API belgelerine başvurmak [burada](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="azure-cli"></a>Azure CLI
Parametresini belirleyebilirsiniz `--platform-fault-domain-count` 1, 2 veya 3 (5. belirtilmezse, varsayılan). Azure CLI belgelerine başvurmak [burada](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create).

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [kullanılabilirlik ve yedeklilik özelliklerine](../virtual-machines/windows/regions-and-availability.md) Azure ortamları için.
