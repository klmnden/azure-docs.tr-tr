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
ms.openlocfilehash: d07873b34a41ff20b5007a88743f6b150d4d8a3d
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212837"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Bir Azure Veri Gezgini kümesinin durumunu denetleyin

CPU, bellek ve disk alt sistemi dahil olmak üzere bir Azure Veri Gezgini küme durumunu etkileyen çeşitli faktörler vardır. Bu makalede, bir küme durumunu ölçmek için uygulayabileceğiniz bazı temel adımları gösterir.

1. [https://dataexplorer.azure.com](https://dataexplorer.azure.com) adresinde oturum açın.

1. Sol bölmede, kümenizi seçin ve aşağıdaki komutu çalıştırın.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Bir çıkış 1 kötü durumda; bir çıkış 0 sağlam değil.

1. Oturum [Azure portalında](https://portal.azure.com)ve kümenize gidebilirsiniz.

1. Altında **izleme**seçin **ölçümleri**, ardından **tutulan**, aşağıdaki görüntüde gösterildiği gibi. 1'e yakın bir çıktı sağlıklı bir küme anlamına gelir.

    ![Küme tutulan ölçüm](media/check-cluster-health/portal-metrics.png)

1. Diğer ölçümleri grafiğe eklemek mümkündür. Grafiği seçip **ölçüm Ekle**. Bu örnek gösterir - başka bir ölçüm seçin **CPU**.

    ![Ölçüm ekleme](media/check-cluster-health/add-metric.png)

1. Küme durumu ile ilgili sorunları tanılamak yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).