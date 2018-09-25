---
title: Bir Azure Veri Gezgini kümesinin durumunu denetleyin
description: Bu makalede, Azure Veri Gezgini kümenin sağlıklı olup olmadığını belirlemek için adımlar açıklanmaktadır.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0746247d2c912ba66e81b95f45b168e32b522130
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988436"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Bir Azure Veri Gezgini kümesinin durumunu denetleyin

CPU, bellek ve disk alt sistemi dahil olmak üzere bir Azure Veri Gezgini küme durumunu etkileyen çeşitli faktörler vardır. Bu makalede, bir küme durumunu ölçmek için uygulayabileceğiniz bazı temel adımları gösterir.

1. Oturum [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com).

1. Sol bölmede, kümenizi seçin ve aşağıdaki komutu çalıştırın.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Bir çıkış 1 kötü durumda; bir çıkış 0 sağlam değil.

1. Oturum [Azure portalında](https://portal.azure.com)ve kümenize gidebilirsiniz.

1. Altında **izleme**seçin **ölçümleri**, ardından **tutulan**, aşağıdaki görüntüde gösterildiği gibi. 1'e yakın bir çıktı sağlıklı bir küme anlamına gelir.

    ![Küme tutulan ölçüm](media/check-cluster-health/portal-metrics.png)

1. Küme için kaynak kullanımını ölçmek için CPU ve bellek önbelleğe alma gibi diğer ölçümleri ekleyin.

1. Küme durumu ile ilgili sorunları tanılamak yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com).