---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için bir erişim gözden geçirme tamamlayın | Microsoft Docs
description: Azure kaynakları için bir erişim gözden geçirme tamamlamak açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: ae64d9ebbca80f6c21b8c7f352022a0878518e65
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için bir erişim gözden geçirme tamamlayın
Ayrıcalıklı rol Yöneticiler ayrıcalıklı erişim sonra gözden geçirebileceğiniz bir [güvenlik incelemesi başlatıldıysa](pim-resource-roles-start-access-review.md). Azure kaynakları için ayrıcalıklı Kimlik Yönetimi (PIM) otomatik olarak erişimleri gözden geçirmek için kullanıcıların ister bir e-posta gönderir. Bir kullanıcı bir e-posta almazsa, onlara yönergeleri gönderebilirsiniz [güvenlik incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

Güvenlik değerlendirme süresi bittikten sonra veya tüm kullanıcılar kendi kendini gözden bitirdikten sonra gözden geçirme yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-security-reviews"></a>Güvenlik değerlendirmeleri yönetme
1. [Azure Portal](https://portal.azure.com/) gidin. Panoda seçip **Azure kaynaklarını** uygulama.

2. Kaynağınızı seçin.

3. Seçin **erişim incelemeler** bölümü.
![Erişim gözden geçirme](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Yönetmek istediğiniz erişim gözden geçirme seçin.

Erişim gözden geçirme ayrıntı dikey bu gözden geçirme yönetmek için birkaç seçenek vardır. Seçenekler şunlardır:

![Bir gözden geçirme yönetmek için Seçenekler](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur
Bitiş tarihi tüm erişim değerlendirmeleri sahip, ancak kullanabileceğiniz **durdurmak** erken bitirme düğmesi. Bu süreye göre gözden tamamlamadınız tüm kullanıcılar gözden durdurduktan sonra son mümkün olmayacaktır. Durdurulmuş sonra bir gözden geçirme yeniden başlatılamıyor.

### <a name="reset"></a>Sıfırla
Üzerinde yapılan tüm kararlar kaldırmak için bir erişim gözden geçirme sıfırlayabilirsiniz. Bir erişim gözden geçirme sıfırladınız sonra tüm kullanıcılar olarak işaretlenmiş yeniden gözden geçirilmeyen. 

### <a name="apply"></a>Uygula
Bir erişim gözden geçirme işlemi tamamlandıktan sonra kullanmak **Uygula** gözden sonucunu uygulamak için düğmesi. Bir kullanıcının erişimi incelemede reddedildiyse, bu adım, rol ataması kaldırır.  

### <a name="delete"></a>Sil
İncelemede daha ilgileniyor değil, silin. **Silmek** düğmesini gözden geçirme PIM uygulamasını kaldırır.

## <a name="results"></a>Sonuçlar
Üzerinde **sonuçları** sekmesinde, görüntüleme ve İnceleme sonuçlarınızı bir listesini indirir. 
![Sonuçları sekmesi](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden geçirenler
Görüntüleyin ve var olan erişim incelemeniz gözden geçirenleri ekleme. Gözden geçirenler geçirmeyi tamamlamak için şu aralıklarla uyar.
![Gözden geçirenler ekleme](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



