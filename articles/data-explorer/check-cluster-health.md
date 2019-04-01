---
title: Bir Azure Veri Gezgini kümesinin durumunu denetleyin
description: Bu makalede, Azure Veri Gezgini kümenin sağlıklı olup olmadığını belirlemek için adımlar açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 8930c2a7538ca33622de68c9a888349b3301cd98
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755849"
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