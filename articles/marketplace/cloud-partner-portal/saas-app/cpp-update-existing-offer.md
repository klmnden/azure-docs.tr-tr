---
title: Mevcut Azure SaaS uygulama teklifi güncelleştirme | Microsoft Docs
description: Mevcut bir SaaS uygulaması teklifi Azure Marketi'nde güncelleştirme yapma.
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
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: 9d490a0ec09e351e4cee00e7c30c9161bdfac3e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60594264"
---
# <a name="update-an-existing-saas-application-offer"></a>Var olan bir SaaS uygulaması teklifi güncelleştirme

Çeşitli yayımlanmış ve canlı sonra teklifinizi yapmak isteyebilirsiniz güncelleştirmeleri vardır. Teklifinizi yeni sürümü için yaptığınız tüm değişiklikler kaydedilir ve Market'te yansıtacak biçimde yeniden yayımlandı. Bu makalede adımları SaaS teklifinizi güncelleştirme farklı yönlerini aracılığıyla [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

Neden, teklifinizi gibi güncelleştirmek için isteyebileceğiniz birkaç neden vardır:

- Yeni bir sürüm mevcut bir uygulamaya ekleme.
- Bir uygulama güncelleştiriliyor.
- Bir uygulama için yeni özellikler eklemektedir.
- Teklif için Market meta veriler güncelleştiriliyor.

Bu değişiklikler yardımcı olmak için portal sunar **karşılaştırma** ve **geçmişi** özellikleri.

## <a name="unpermitted-changes-to-a-saas-offer"></a>Bir SaaS teklifi unpermitted değişiklikler

Azure Marketi'nde teklif Canlı olduktan sonra değiştirilemeyen bir SaaS teklif öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

- Teklif kimliği ve teklif yayımcı kimliği
- Sürüm, örneğin etiketler: `1.0.1`
- Faturalama/lisans modeli mevcut tekliflerin değiştirir.

## <a name="common-update-operations"></a>Genel güncelleştirme işlemleri
 
Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-offer-contacts"></a>Teklif kişilerini güncelleştirme

Teklifiniz için destek kişileri güncelleştirmek için aşağıdaki adımları kullanın.

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. Git **kişiler** sekmesi. Kişilerinizi güncelleştirin.
4. Seçin **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.


### <a name="update-offer-marketplace-metadata"></a>Teklif Market meta verilerini güncelleştir

Teklifinizle ilişkili Market meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logolar, vs.)

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3. Git **mağaza ayrıntıları** sekmesi. Yönergeleri kullanın [yayımlama SaaS teklifi](./cpp-publish-offer.md) makale meta verileri değişiklik yapma.
4. Seçin **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.

## <a name="compare-feature"></a>Özellik karşılaştırması

Yayımlanan bir teklifi için değişiklik yaptığınızda yapmış olduğunuz değişiklikleri denetlemek için karşılaştırma özelliğini kullanabilirsiniz. Sonraki ekran görüntüsü yakalamayı yayımlanan bir teklifi için karşılaştırma seçeneği gösterir.

![Görmek için karşılaştırma kullanın bulut iş ortağı portalı değişiklikleri sunar](./media/saas-offer-compare.png)

### <a name="to-use-the-compare-feature"></a>Compare özelliği kullanmak için:

1. Düzenleme işlemi içinde herhangi bir noktada karşılaştırma teklifiniz için seçin.
2. Pazarlama varlıkları ve meta verileri yan yana sürümlerin arayın.

## <a name="publishing-history"></a>Yayımlama geçmişi

Geçmiş yayımlama etkinlikleri görmek için seçin **geçmişi** sekmesi üzerinde sol gezinti menüsünde çubuğu, bulut iş ortağı portalı. Azure Marketi Teklifleriniz ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görebilirsiniz.

![Bkz: Bulut iş ortağı Portalı'nda geçmişi sunar](./media/saas-offer-history.png)

Denetim geçmişi sayfası, belirli bir teklif için arama yapın ve yayımcı, teklif ve olay türü (örneğin, OfferGoLiveRequested.) gibi filtreler uygulamak için kullanabilirsiniz Ayrıca, denetim geçmişini csv dosyası olarak indirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

[SaaS uygulama teklifi](./cpp-saas-offer.md)