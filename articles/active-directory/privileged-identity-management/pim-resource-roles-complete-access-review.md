---
title: Azure kaynak rolleri PIM - Azure Active Directory erişim gözden geçirmesi tamamlama | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri, erişim değerlendirmesi tamamlama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9903bb82a82291febf571829fb9874ba66d2eab2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476373"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-pim"></a>Azure kaynak rolleri pım'de erişim değerlendirmesi tamamlama
Ayrıcalıklı rol yöneticileri, ayrıcalıklı erişim sonra gözden geçirebileceğiniz bir [erişim gözden geçirmesi çalışmaya](pim-resource-roles-start-access-review.md). Azure Active Directory (Azure AD) Privileged Identity Management (PIM) otomatik olarak kullanıcıların erişimini gözden geçirmek için kullanıcıların ister bir e-posta gönderir. Bir kullanıcı bir e-posta almazsa, bunları yönergeleri gönderebilirsiniz [erişim gözden geçirmesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

Erişim gözden geçirmesi dönemi bittikten sonra veya tüm kullanıcılar, kendi kendini gözden bitirdikten sonra gözden geçirme yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim gözden geçirmeleri yönetme
1. [Azure Portal](https://portal.azure.com/) gidin. Panoda, ardından **Azure kaynaklarını** uygulama.

2. Kaynağınızı seçin.

3. Seçin **erişim gözden geçirmeleriyle** Pano bölümü.

    ![Azure kaynakları - listesini gösteren rolü, sahibi, başlangıç tarihi, bitiş tarihi ve durumu erişim gözden geçirmeleri](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Yönetmek istediğiniz erişim gözden geçirmesi seçin.

Erişim gözden geçirmesi ayrıntıları dikey penceresinde, gözden geçirme yönetmek için birkaç seçenek vardır. Seçenekleri aşağıdaki gibidir:

![Bir gözden geçirme - yönetmek için seçenekleri durdurma, sıfırlama, silme Uygula](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur
Tüm erişim gözden geçirmeleri bir bitiş tarihi vardır, ancak kullanabileceğiniz **Durdur** düğmesini erken tamamlayın. Bu süreye göre gözden geçirmelerini tamamlamadınız tüm kullanıcılar gözden durdurduktan sonra bitirmek mümkün olmayacaktır. Durdurulmuş sonra bir gözden geçirme yeniden başlatılamıyor.

### <a name="reset"></a>Sıfırla
Üzerinde yapılan tüm kararları kaldırmak için erişim gözden geçirmesi sıfırlayabilirsiniz. Erişim gözden geçirmesi sıfırladık sonra tüm kullanıcılar olarak işaretlenmiş yeniden gözden geçirilmeyen. 

### <a name="apply"></a>Uygula
Erişim gözden geçirmesi tamamlandığında, kullanın **Uygula** gözden geçirme sonucunu uygulamak için düğme. İncelemede kullanıcı erişimi reddedildiyse, bu adım, rol ataması kaldırır.  

### <a name="delete"></a>Sil
İncelemede daha ilgileniyor olmayan değilse silebilirsiniz. **Sil** düğmesini gözden PIM uygulamadan kaldırır.

## <a name="results"></a>Sonuçlar
Üzerinde **sonuçları** sayfasında görüntüleyin ve sonuçlarını gözden geçirme listesini indirin. 

![Sonuçlar sayfası listeleme kullanıcılar, sonucu, neden, gözden geçiren tarafından uygulanan ve sonucu Uygula](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden geçirenler
Görüntüleyebilir ve mevcut erişim gözden geçirmeniz için gözden geçirenleri ekleyin. Geçirmeyi tamamlamak için gözden geçirenler hatırlatın.

![Gözden geçirenler listesi adı ve kullanıcı asıl adı sayfası](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rolleri için erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md)
- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
