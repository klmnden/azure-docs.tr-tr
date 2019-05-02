---
title: Mevcut bir Azure uygulama teklifi güncelleştirme | Azure Market
description: Mevcut bir Azure uygulama teklifi Azure Marketi'nde güncelleştirme yapma.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pabutler
ms.openlocfilehash: a36df757e3a2682af641101ed82583a0cd293e0a
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942814"
---
# <a name="update-an-existing-azure-application-offer"></a>Mevcut bir Azure uygulama teklifi güncelleştirme

Yayımlandı ve canlı sonra teklifinizi yapmak isteyebilirsiniz güncelleştirmeleri çeşitli tür vardır. Teklifinizi yeni sürümü için yaptığınız tüm değişiklikler kaydedilir ve Market'te yansıtacak biçimde yeniden yayımlandı. Bu makalede adımları üzerinden yönetilen uygulamayı teklifinizi güncelleştirme farklı yönlerini [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

Neden, teklifinizi gibi güncelleştirmek için isteyebileceğiniz birkaç neden vardır:

- Yeni bir görüntü sürümü mevcut SKU'lara ekleniyor.
- Yeni SKU'lara ekleniyor.
- Market meta verileri tek bir SKU'ları ve teklif güncelleştiriliyor.

Bu değişiklikler yardımcı olmak için portal sunar **karşılaştırma** ve **geçmişi** özellikleri.

## <a name="unpermitted-changes-to-an-azure-application-offer-or-sku"></a>Bir Azure uygulaması teklif veya SKU unpermitted değişiklikler

Bir kapsayıcı teklif ya da Azure Marketi'nde teklif Canlı olduktan sonra değiştirilemeyen SKU öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

- Teklif kimliği ve teklif yayımcı kimliği
- Mevcut bir SKU'ları SKU kimliği
- Sürüm, örneğin etiketler: `1.0.1`
- Mevcut SKU'lara faturalandırma/lisans modeli değiştirir

## <a name="common-update-operations"></a>Genel güncelleştirme işlemleri

Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-image-version-for-a-sku"></a>Görüntü sürümü için SKU güncelleştirme

Güvenlik düzeltme ekleri, ek özellikler ve benzeri düzenli olarak güncelleştirilmesi bir görüntü yaygındır. Bu senaryoda, aşağıdaki adımları kullanarak, SKU'nuzu başvuran görüntüsünü güncelleştirmek istediğiniz:

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. İçinde **SKU'ları** sekmesinde, güncelleştirilecek görüntüyle ilişkilendirilen SKU seçin.
4. Seçin **+ yeni görüntü sürümü** yeni bir görüntü eklemek için.
5. Yeni görüntü sürümleri sağlar. Görüntü sürümü, önceki sürümler olarak aynı etiketleri yönergeleri izlemeniz gerekir. Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır. Sağladığınız yeni sürümü önceki sürümlerin tümü büyük olduğundan emin olun.
6. Seçin **Yayımla** yeni kapsayıcı görüntüsü sürümü Azure Marketi'nde içerik yayımlamak için iş akışını başlatmak için.

### <a name="add-a-new-sku"></a>Yeni bir SKU ekleyin

Yeni bir SKU'ya teklifiniz için kullanılabilir hale getirmek için aşağıdaki adımları kullanın:

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. Altında **SKU'ları** sekmesinde **yeni bir SKU ekleyin** ve sağlayan bir **SKU kimliği** açılır pencerede.
4. İçinde açıklanan adımları kullanarak teklif yeniden yayımlamanız [Azure yayımlama uygulaması teklif](./cpp-publish-offer.md).
5. Seçin **Yayımla** yeni SKU'nuz yayımlamak için iş akışını başlatmak için.

### <a name="update-offer-marketplace-metadata"></a>Teklif Market meta verilerini güncelleştir

Teklifinizle ilişkili Market meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logolar, vs.)

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. Git **Market** sekmesi. Yönergeleri kullanın [Azure yayımlama uygulaması teklif](./cpp-publish-offer.md) meta veri değişiklikleri yapma.
4. Seçin **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.
 
>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](../../cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="deleting-an-existing-offer"></a>Var olan bir teklif siliniyor

Teklifinizi Market'ten kaldırmaya karar verebilirsiniz. Teklif siliniyor, bu teklifi satın alımlarında geçerli etkilemez. Bu müşteri satın alma işlemleri, önceki gibi çalışmaya devam eder. Silme işlemi tamamlandıktan sonra ancak teklif herhangi yeni satın alma işlemleri için kullanılabilir değil.

Teklif Sonlandırma, sizinle mevcut müşterileriniz arasındaki hizmet ve/veya lisans sözleşmesini iptal etme işlemidir.
Yönergeler ve teklif kaldırma ve sonlandırmayla ilgili ilkeler Microsoft Market yayımcı anlaşması tarafından yönetilir (bkz. Bölüm 7) ve katılım ilkeleri (bkz. Bölüm 6.2).

Daha fazla bilgi için [silin ve Azure marketi'ndeki teklif/SKU](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-managed-app-offer-delete).

## <a name="compare-feature"></a>Özellik karşılaştırması

Yayımlanan bir teklifi için değişiklik yaptığınızda yapmış olduğunuz değişiklikleri denetlemek için karşılaştırma özelliğini kullanabilirsiniz.

Compare özelliği kullanmak için:

1. Düzenleme işlemi içinde herhangi bir noktada karşılaştırma teklifiniz için seçin.
2. Pazarlama varlıkları ve meta verileri yan yana sürümlerin arayın.

## <a name="history-of-publishing-actions"></a>Eylemleri yayımlama geçmişi

Geçmiş yayımlama etkinlikleri görmek için seçin **geçmişi** sekmesi üzerinde sol gezinti menüsünde çubuğu, bulut iş ortağı portalı. Azure Marketi Teklifleriniz ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure uygulama teklifi](./cpp-azure-app-offer.md)