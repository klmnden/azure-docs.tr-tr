---
title: 'Öğretici: Azure’da Cloudyn ile harcama tahmini | Microsoft Docs'
description: Bu öğreticide, geçmiş kullanımı ve harcanan verileri kullanarak harcama tahmini yapmayı öğrenirsiniz.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/18/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: benshy
ms.openlocfilehash: 25fa099bcd6fa3843deadb3dc39d4250624ead06
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274595"
---
# <a name="tutorial-forecast-future-spending"></a>Öğretici: Gelecek harcamaları tahmin etme

Cloudyn, geçmiş kullanımı ve harcanan verileri kullanarak gelecek harcamaları tahmin etmenize yardımcı olur. Tüm maliyet yansıtma verilerini görüntülemek için Cloudyn raporlarını kullanırsınız. Bu öğreticideki örnekler, raporları kullanarak maliyet yansımalarını gözden geçirmeniz konusunda size yol gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Gelecek harcamaları tahmin etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Azure hesabınız olmalıdır.
- Cloudyn için bir deneme kaydına veya ücretli aboneliğe sahip olmanız gerekir.

## <a name="forecast-future-spending"></a>Gelecek harcamaları tahmin etme

Cloudyn, zaman içindeki kullanımınızı temel alarak harcama tahmini yapmanıza yardımcı olan maliyet yansıtma raporlarını içerir. Birincil amaçları, maliyet eğilimlerinizin kuruluş beklentilerinizi aşmamasına yardımcı olmaktır. Kullandığınız raporlar, Geçerli Ay Öngörülen Maliyet ve Yıllık Öngörülen Maliyet içindir. Her ikisi de, mevcut kullanımınızın son 30 günlük kullanımınızla görece tutarlı olup olmayacağına göre tahmini gelecek harcamalarınızı gösterir.

Geçerli Ay Öngörülen Maliyet raporu, hizmetlerinizin maliyetini gösterir. Ayın başlangıcına ve önceki aya ait maliyetleri kullanarak tahmini maliyeti gösterir. Portalın üstündeki raporlar menüsünde **Maliyetler** > **Öngörü ve Bütçe** > **Geçerli Ay Öngörülen Maliyet** seçeneklerine tıklayın. Aşağıdaki resimde bir örnek gösterilir.

![Geçerli ay öngörülen maliyet](./media/tutorial-forecast-spending/project-month01.png)

Örnekte, en fazla harcama yapılan hizmetleri görebilirsiniz. Azure maliyetleri, AWS maliyetlerinden daha düşüktü. Azure VM'ler için maliyet öngörü ayrıntılarını görmek istiyorsanız, **Filtre** listesinde **Azure/VM**’i seçin.

![Azure VM geçerli ay öngörülen maliyet](./media/tutorial-forecast-spending/project-month02.png)

İlgilendiğiniz diğer hizmetlere yönelik aylık maliyeti öngörülerine bakmak için önceki temel adımların aynılarını izleyin.

Yıllık Öngörülen Maliyet raporu, hizmetlerinizin sonraki 12 ay boyunca ortaya çıkabilecek tahmini maliyetini gösterir.

Portalın üstündeki raporlar menüsünde **Maliyetler** > **Öngörü ve Bütçe** > **Yıllık Öngörülen Maliyet** seçeneklerine tıklayın. Aşağıdaki resimde bir örnek gösterilir.

![Yıllık öngörülen maliyet raporu](./media/tutorial-forecast-spending/project-annual01.png)

Örnekte, en fazla harcama yapılan hizmetleri görebilirsiniz. Aylık örnekteki gibi, Azure maliyetleri AWS maliyetlerinden daha düşüktü. Azure VM'ler için maliyet öngörü ayrıntılarını görmek istiyorsanız, **Filtre** listesinde **Azure/VM**’i seçin.

![VM’lerin yıllık öngörülen maliyeti](./media/tutorial-forecast-spending/project-annual02.png)

Yukarıdaki resimde, Azure VM’lerinin yıllık öngörülen maliyeti 28.374 ABD dolarıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Gelecek harcamaları tahmin etme


Maliyet dağıtımı ve geriye dönük raporlar ile maliyetleri yönetmeyi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Maliyet dağıtımı ve geriye dönük raporlar ile maliyetleri yönetme](tutorial-manage-costs.md)
