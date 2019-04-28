---
title: Mevcut bir Azure kapsayıcıları teklifi güncelleştirme | Microsoft Docs
description: Mevcut bir kapsayıcı teklifi Azure Marketi'nde güncelleştirme yapma.
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
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 2b568717b6656fb9ae15e9a6dbd27441689c4372
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61472715"
---
# <a name="update-an-existing-container-offer"></a>Mevcut bir kapsayıcı teklifi güncelleştirme

Bu makalede adımları kapsayıcı teklifinizi güncelleştirme farklı yönlerini aracılığıyla [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

Neden, teklifinizi gibi güncelleştirmek için isteyebileceğiniz birkaç neden vardır:

-  Yeni bir kapsayıcı görüntüsü sürümü mevcut SKU'lara ekleniyor.
-  Yeni SKU'lara ekleniyor.
-  Market meta verileri tek bir SKU'ları ve teklif güncelleştiriliyor.

Bu değişiklikler yardımcı olmak için portal sunar **karşılaştırma** ve **geçmişi** özellikleri.  


## <a name="unpermitted-changes-to-a-container-offer-or-sku"></a>Bir kapsayıcı teklif veya SKU unpermitted değişiklikleri

Bir kapsayıcı teklif ya da Azure Marketi'nde teklif Canlı olduktan sonra değiştirilemeyen SKU öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

-  **Teklif kimliği** ve **yayımcı kimliği** teklifi
-  **SKU kimliği** mevcut SKU'lar
-  Sürüm, örneğin etiketler: `1.0.1`
-  Mevcut SKU'lara faturalandırma/lisans modeli değiştirir

## <a name="common-update-operations"></a>Genel güncelleştirme işlemleri

Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-container-image-version-for-a-sku"></a>Kapsayıcı görüntü sürümü için SKU güncelleştirme

Güvenlik düzeltme ekleri, ek özellikler ve benzeri düzenli olarak güncelleştirilmesi bir kapsayıcı görüntüsü yaygındır. Bu senaryoda, aşağıdaki adımları kullanarak, SKU'nuzu başvuran kapsayıcı görüntüsünü güncelleştirmek istediğiniz:

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. İçinde **SKU'ları** sekmesinde, güncelleştirmek için kapsayıcı görüntüsü ile ilişkilendirilen SKU seçin.
4. Altında **kapsayıcı görüntüsü**seçin **+ yeni görüntü sürümü** yeni bir kapsayıcı görüntüsü eklemek için.
5. Yeni kapsayıcı **görüntü sürümleri**. Görüntü sürümü, önceki sürümler olarak aynı etiketleri yönergeleri izlemeniz gerekir. Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır. Sağladığınız yeni sürümü önceki sürümlerin tümü büyük olduğundan emin olun.
6. Seçin **Yayımla** yeni kapsayıcı görüntüsü sürümü Azure Marketi'nde içerik yayımlamak için iş akışını başlatmak için.

### <a name="add-a-new-sku"></a>Yeni bir SKU ekleyin

Yeni bir SKU'ya teklifiniz için kullanılabilir hale getirmek için aşağıdaki adımları kullanın:

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. Altında **SKU'ları** sekmesinde **yeni bir SKU ekleyin** ve sağlayan bir **SKU kimliği** açılır pencerede.
4. İçinde açıklanan adımları kullanarak kapsayıcıyı yeniden yayımlamanız [Yayımla kapsayıcı teklif](./cpp-publish-offer.md).
5. Seçin **Yayımla** yeni SKU'nuz yayımlamak için iş akışını başlatmak için.

### <a name="update-offer-marketplace-metadata"></a>Teklif Market meta verilerini güncelleştir

Teklifinizle ilişkili Market meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logoları ve VS.)

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. Git **Market** sekmesi. Yönergeleri kullanın [Yayımla kapsayıcı teklif](./cpp-publish-offer.md) meta verileri değişiklik yapmak için makale sunar.
4. Seçin **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.

## <a name="compare-feature"></a>Özellik karşılaştırması

Yayımlanan bir teklifi için değişiklik yaptığınızda kullanabilirsiniz **karşılaştırma** yapmış olduğunuz değişiklikleri denetleme özelliği.

### <a name="to-use-the-compare-feature"></a>Compare özelliği kullanmak için:

1. Düzenleme işlemi içinde herhangi bir noktada karşılaştırma teklifiniz için seçin.
2. Pazarlama varlıkları ve meta verileri yan yana sürümlerin arayın.


## <a name="history-of-publishing-actions"></a>Eylemleri yayımlama geçmişi

Geçmiş yayımlama etkinlikleri görmek için seçin **geçmişi** sekmesi üzerinde sol gezinti menüsünde çubuğu, bulut iş ortağı portalı. Azure Marketi Teklifleriniz ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görebilirsiniz.