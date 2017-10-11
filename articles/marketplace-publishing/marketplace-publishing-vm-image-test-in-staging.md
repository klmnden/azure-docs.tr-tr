---
title: "VM teklifiniz Market için test | Microsoft Docs"
description: "VM görüntüsü için Azure Marketi test etme anlayın."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: 26f856059b381be91b9cdd1f98a11dc90813c0c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>VM teklifiniz için Azure Marketi hazırlamada test etme
Hazırlama, SKU özel ", test ve Market'te dağıtmadan önce işlevselliğini doğrulamak sandbox" dağıtma anlamına gelir. SKU dağıtmış olan bir müşteriye gibi hazırlama görüntülenir. VM görüntüsü için hazırlama edilmesini sertifikalı gerekir.

## <a name="step-1-push-your-offer-to-staging"></a>1. adım: anında iletme teklifiniz için hazırlama
1. Üzerinde **Yayımla** sekmesini tıklatın, **anında hazırlık**.
   
    ![Çizim](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Yayımlama Portalı, tüm hataları bildirirse, bunları düzeltin.
3. İçinde **kimin hazırlanmış teklifiniz erişebilir?** iletişim kutusunda, kullanımınız teklifinize önizlemek için kullanacağınız Azure Aboneliklerin listesini girin [Azure Önizleme portalı](https://portal.azure.com).
   
   > [!NOTE]
   > Sanal makineler durumunda ve çözüm şablonları, lütfen **sağlamadığı** beyaz liste abonelikleri CSP, DreamSpark veya Open ile Azure türü.
   > 
   > 

    > Düğmeyi tıkladığınızda, sanal makineleri durumunda **hazırlama anında**, aşağıdaki adımları Sahne gerçekleştirilir. Yayımlama Yayımla sekmesi altında her adımı ilerlemesini görüntülemek kuramaz portal. (Hazırlanmış durumu gösterilir) kadar bu sayfayı düzenli aralıklarla, uçtan düzeltmesi gereken tüm hata bilgileri için işaretlemeniz gerekir.

    > - İlk bakışta hazırlama isteğiniz vhd doğrulamak sertifika takımın gider. İsteğiniz değişiklik yalnızca pazarlama aldı, ancak sonra sertifika adım atlanır.
    > - Sertifika tamamlandıktan sonra çoğaltma önerinin tüm Azure veri merkezi arasında başlatın. Genellikle, 24-48hours çoğaltmanın tamamlanmasını alır ancak vhd öğesinin boyutuna bağlı olarak bir hafta sürebilir. İsteğiniz değişiklik yalnızca pazarlama aldı, ancak sonra çoğaltma daha hızlıdır.
    > - Çoğaltma işlemi tamamlandıktan sonra teklif kullanılabilir olması [Azure portal](http:/portal.azure.com). Durum zaman at yayımlama HAZIRLANAN hale portal. Aşamalı bir teklif görünür [Azure portal](http:/portal.azure.com) yalnızca ile teklif hazırlanan abonelikle ilişkili e-posta Kimlikleri'ni kullanarak.

1. Oturum [Azure Önizleme portalını](https://portal.azure.com) Azure aboneliklerini birini kullanarak, önceki adımda listelenen.
2. Teklifiniz bulun ve VM görüntü noktaları doğrula:
   
   * İçerik pazarlama doğru markette gösterildiğini doğrulayın.
   * VM görüntüsü uçtan uca dağıtımı.
     
      ![img harita portalı](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Teklifiniz Yayımlama Portalı aracılığıyla Microsoft'a bildirmeyi kadar hazırlama kalacak [**Yayımla** sekmesini > düğmesine tıklayın **"İsteği onayı için anında iletme için üretim"**] için göndermek hazır Üretim. Bu, her şeyi listelenen giderek teklifiniz için hazırlık üzerinden takım onay tüm üyeleri için ideal bir zamandır.
> 
> Hazırlama platform yayımcı tarafından önizleme modunda teklif test etmek için tasarlanmıştır. Bu platofrm commerical amaçlı kullanan önleyin.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Artık, teklifiniz "hazırlanan" ve işlevselliğini test ettikten ve içerik pazarlama, son yayımlama aşamasına geçebilirsiniz **4. adım**: [teklifiniz Market'te dağıtma](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

