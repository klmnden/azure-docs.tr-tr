---
title: "Azure maliyeti yönetimiyle Harcamaları tahmin etmenize | Microsoft Docs"
description: "Geçmiş kullanımı kullanarak ve veri harcama Harcamaları tahmin etmenize."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: d8b0cd2a3e5f9829f0844783aad22d375eb9d7a8
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="forecast-future-spending"></a>Gelecekteki Harcamaları tahmin etmenize

Azure maliyeti Yönetimi Cloudyn tarafından veri harcama ve geçmiş kullanımı aracılığıyla harcama gelecekteki tahmin yardımcı olur. Tüm maliyet projeksiyon verileri görüntülemek için Cloudyn raporları kullanın. Bu öğretici örneklerde, maliyet tahminleri raporları kullanarak gözden geçirme aracılığıyla yol. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Gelecekteki Harcamaları tahmin etmenize

## <a name="forecast-future-spending"></a>Gelecekteki Harcamaları tahmin etmenize

Cloudyn zaman içinde kullanımınıza bağlı Harcamaları tahmin etmenize yardımcı olması için maliyet projeksiyon raporları içerir. Kendi birincil amacı, kuruluşunuzun beklentilerini aşmaması maliyeti eğilimlerini olmanıza yardımcı olmaktır. Kullandığınız raporlar, geçerli ay tahmini maliyet ve yıllık tahmini maliyet içindir. Her ikisi de kullanımınızı ile son 30 gün kullanım görece tutarlı kalır tahmini gelecekteki Harcamaları gösterir.

Geçerli ay tahmini maliyet rapor hizmetlerinizi maliyetleri gösterir. Önceki ay ve ayın başlangıcını maliyetlerden tahmini maliyet göstermek için kullanır. Portal üstündeki raporları menüsünde **maliyet** > **projeksiyon ve bütçe** > **geçerli ay tahmini maliyet**. Aşağıdaki resimde bir örnek gösterilmektedir.

![Geçerli ay tahmini maliyet](./media/tutorial-forecast-spending/project-month01.png)

Örnekte, hangi hizmetlerin en harcanan görebilirsiniz. Azure maliyeti AWS maliyetlerinden daha düşük. Azure VM'ler için Maliyet Yansıtma ayrıntılarını görmek istediğiniz **filtre** listesinde **Azure/VM**.

![Azure VM geçerli ay Planlanan Maliyet](./media/tutorial-forecast-spending/project-month02.png)

İlgilendiğiniz diğer hizmetler için aylık maliyeti tahminleri bakmak için aynı temel yukarıdaki adımları izleyin.

Yıllık tahmini maliyet rapor hizmetlerinizi extrapolated maliyetini sonraki 12 ay içinde gösterir.

Portal üstündeki raporları menüsünde **maliyet** > **projeksiyon ve bütçe** > **yıllık tahmini maliyet**. Aşağıdaki resimde bir örnek gösterilmektedir.

![yıllık tahmini maliyeti raporu](./media/tutorial-forecast-spending/project-annual01.png)

Örnekte, hangi hizmetlerin en harcanan görebilirsiniz. Aylık örnekteki gibi Azure maliyetleri AWS maliyetlerinden daha düşük. Azure VM'ler için Maliyet Yansıtma ayrıntılarını görmek istediğiniz **filtre** listesinde **Azure/VM**.

![VM'lerin yıllık tahmini maliyeti](./media/tutorial-forecast-spending/project-annual02.png)

Yukarıdaki resimde yıllık tahmini Azure VM'ler $28,374 maliyetidir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Gelecekteki Harcamaları tahmin etmenize


Maliyet ayırma ve giderleri raporlarla maliyetlerini yönetme konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Maliyet ayırma ve giderleri raporlarla maliyetlerini yönetme](tutorial-manage-costs.md)
