---
title: Market'e VM teklifinizi sınamak | Microsoft Docs
description: VM görüntünüzdeki test için Azure Market hakkında bilgi edinin.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: 26f856059b381be91b9cdd1f98a11dc90813c0c5
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715881"
---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Hazırlık döneminde, VM teklifi Azure Marketi için test etme
Hazırlama, SKU'nuzu özel ", test ve Market'te dağıtmadan önce işlevselliğini doğrulamak korumalı alan" içinde dağıtma anlamına gelir. SKU hazırlama dağıtmış olan bir müşteri için olduğu gibi görüntülenir. VM görüntünüzdeki hazırlamaya için sertifika gerekir.

## <a name="step-1-push-your-offer-to-staging"></a>1. adım: teklifinizi hazırlamaya Gönder
1. Üzerinde **Yayımla** sekmesinde **hazırlama için anında iletme**.
   
    ![Çizim](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Yayımlama Portalı, herhangi bir hata bildirirse, bunları düzeltin.
3. İçinde **hazırlanmış teklifinizi kimler erişebilir?** iletişim kutusunda, teklife önizlemesi için kullanacağınız Azure abonelikleri listesinde girin [Azure Önizleme portalı](https://portal.azure.com).
   
   > [!NOTE]
   > Sanal makineler durumunda ve çözüm şablonları, lütfen **olmayan** beyaz liste abonelikleri CSP, DreamSpark veya Open ile Azure türü.
   > 
   > 

    > Sanal makineler düğmesine tıkladığınızda, durumunda **hazırlamaya Gönder AŞAMASINA**, aşağıdaki adımları Sahne gerçekleştirilir. Yayımlama yayımlama sekmesindeki her bir adımın ilerleme durumunu görüntülemek mümkün olacaktır portalı. (Aşamalı durum gösterilene kadar), sona düzeltme gereken tüm hata bilgileri için bu sayfayı düzenli aralıklarla denetlemelisiniz.

    > - İlk başta hazırlama isteğiniz vhd doğrulamak sertifika takımın gider. İsteğiniz değişiklik yalnızca pazarlama alındı, ancak sonra onaylama adımı atlanır.
    > - Sertifika işlemi tamamlandıktan sonra çoğaltma teklifi tüm Azure veri merkezleri arasında başlatır. Genellikle, 24-48hours çoğaltmanın tamamlanmasını alır ancak VHD'nin boyutuna bağlı olarak bir hafta sürebilir. İsteğiniz değişiklik yalnızca pazarlama alındı, ancak çoğaltma daha hızlı olur.
    > - Çoğaltma tamamlandığında, daha sonra teklifin kullanılabilir [Azure portalında](http:/portal.azure.com). Yayımlama durumu süresi en aşamalı hale portalı. Aşamalı bir teklif görülebilir [Azure portalında](http:/portal.azure.com) yalnızca ile teklif aşamalı aboneliğiyle ilişkili e-posta kimlikleri kullanarak.

1. Oturum [Azure Önizleme portalı](https://portal.azure.com) önceki adımda listelenen Azure aboneliklerinin birini kullanarak.
2. Teklifinizi bulun ve VM görüntüsü noktalarınızı doğrulayın:
   
   * Pazarlama içeriği doğru Market'te göründüğünü doğrulayın.
   * VM görüntüsü uçtan uca dağıtımı.
     
      ![img-map-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Teklifiniz, Yayımlama Portalı aracılığıyla Microsoft'a bildirmediğiniz sürece hazırlama aşamasında kalır [**Yayımla** sekmesi > düğmesine tıklayın **"İstek onay için anında iletme için üretim"**] için göndermeye hazır Üretim. Bu, listelenen teklifiniz için hazırlık aşamasındaki her şeyin üzerinden takım iade tüm üyeleri için ideal bir süredir.
> 
> Hazırlama platform yayımcı tarafından teklif önizleme modunda test etmek için tasarlanmıştır. Bu çalışabilen si amaçlarla kullanmasını engelleyin.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Teklifinizi "hazırlanılır" ve işlevselliğini test ettiğiniz ve içerik pazarlama, son yayımlama aşamasına geçebilirsiniz **4. adım**: [teklifiniz Market'te dağıtma](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

