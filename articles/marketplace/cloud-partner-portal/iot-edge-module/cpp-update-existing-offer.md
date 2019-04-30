---
title: Mevcut bir Azure IOT Edge modülü teklifi güncelleştirme | Microsoft Docs
description: Mevcut bir IOT Edge modülü teklifi Azure Marketi'nde güncelleştirme yapma.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: ca7bed26d91c28304638e85d6da93708bfcfcada
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60914133"
---
# <a name="update-an-existing-iot-edge-module-offer"></a>Mevcut bir IOT Edge modülü teklifi güncelleştirme

Bu makalede adımları IOT Edge modülü teklifinizi güncelleştirme farklı yönlerini aracılığıyla [bulut iş ortağı portalı](https://cloudpartner.azure.com/) ve sonra teklifin yeniden yayımlanması.

Neden, teklifinizi gibi güncelleştirmek için isteyebileceğiniz birkaç neden vardır:

-  Yeni bir IOT Edge modülü görüntü sürümü mevcut SKU'lara ekleniyor.
-  Yeni SKU'lara ekleniyor.
-  Market meta verileri tek bir SKU'ları ve teklif güncelleştiriliyor.

Bu değişiklikler yardımcı olmak için portal sunar **karşılaştırma** ve **geçmişi** özellikleri.  


## <a name="unpermitted-changes-to-iot-edge-module-offer-or-sku"></a>IOT Edge modülü teklif veya SKU unpermitted değişiklikler

Bir IOT Edge modülü teklif ya da Azure Marketi'nde teklif Canlı olduktan sonra değiştirilemeyen SKU öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

-  **Teklif kimliği** ve **yayımcı kimliği** teklifi
-  **SKU kimliği** mevcut SKU'lar
-  Sürüm, örneğin etiketler: `1.0.1`
-  Mevcut SKU'lara faturalandırma/lisans modeli değiştirir

## <a name="common-update-operations"></a>Genel güncelleştirme işlemleri

Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-the-iot-edge-module-image-version-for-a-sku"></a>IOT Edge modülü görüntü sürümü için SKU güncelleştirme

Güvenlik düzeltme ekleri, ek özellikler ve benzeri düzenli olarak güncelleştirilmesi bir IOT Edge modülü görüntüsü yaygındır. Bu senaryoda, aşağıdaki adımları kullanarak, SKU'nuzu başvuran IOT Edge modülü görüntüsü güncelleştirmek istediğiniz:

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.

3.  İçinde **SKU'ları** sekmesinde, güncelleştirilecek IOT Edge modülü görüntüyle ilişkilendirilen SKU seçin.

4.  Altında **Edge modülü görüntüsü**seçin **+ yeni görüntü sürümü** yeni bir IOT Edge modülü görüntüsü eklemek için.

5.  Yeni IOT Edge modülü sağlayan **görüntü sürümleri**. Görüntü sürümü, önceki sürümler olarak aynı etiketleri yönergeleri izlemeniz gerekir. Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır. Sağladığınız yeni sürümü önceki sürümlerin tümü büyük olduğundan emin olun.

6.  Seçin **Yayımla** yeni IOT Edge modülü sürümü, Azure Marketi'nde içerik yayımlamak için iş akışını başlatmak için.

### <a name="add-a-new-sku"></a>Yeni bir SKU ekleyin

Yeni bir SKU'ya teklifiniz için kullanılabilir hale getirmek için aşağıdaki adımları kullanın: 

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.

3.  Altında **SKU'ları** sekmesinde **yeni bir SKU ekleyin** ve sağlayan bir **SKU kimliği** açılır pencerede.

4.  İçinde açıklanan adımları kullanarak IOT Edge modülü yeniden yayımlamanız [bir IOT Edge modülü, Azure Marketi'nde yayımlayabileceğiniz](./cpp-publish-offer.md).

5.  Seçin **Yayımla** yeni SKU'nuz yayımlamak için iş akışını başlatmak için.


### <a name="update-offer-marketplace-metadata"></a>Teklif Market meta verilerini güncelleştir

Teklifinizle ilişkili Market meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logolar, vs.)

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.

3.  Git **Market** sekmesi. Yönergeleri kullanın [bir IOT Edge modülü, Azure Marketi'nde yayımlayabileceğiniz](./cpp-publish-offer.md) makale meta verileri değişiklik yapma.

4.  Seçin **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.

## <a name="compare-feature"></a>Özellik karşılaştırması

Yayımlanan bir teklifi üzerinde değişiklik yaptığınızda kullanabilirsiniz **karşılaştırma** yapmış olduğunuz değişiklikleri denetleme özelliği. 

**Compare özelliği kullanmak için:**

1.  Düzenleme işlemi içinde herhangi bir noktada seçin **karşılaştırma** teklifiniz için.

    ![Özellik düğmesi karşılaştırın](./media/iot-edge-module-compare.png)


2.  Pazarlama varlıkları ve meta verileri yan yana sürümlerin arayın.


## <a name="history-of-publishing-actions"></a>Eylemleri yayımlama geçmişi

Geçmiş yayımlama etkinlikleri görmek için seçin **geçmişi** sekmesi üzerinde sol gezinti menüsünde çubuğu, bulut iş ortağı portalı. Azure Marketi Teklifleriniz ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görebilirsiniz.  <!-- Need to find correct link here:  legal time windowsFor more information, see [History page](cpp-history-page.md) -->
