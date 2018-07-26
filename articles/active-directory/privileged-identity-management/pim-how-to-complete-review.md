---
title: Erişim değerlendirmesi tamamlama | Microsoft Docs
description: Azure AD Privileged Identity Management'ta erişim gözden geçirmesi başlatıldıktan sonra tamamlar ve sonuçları görüntüleme hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e8aacc95f73294420042549681aff5494dc63cce
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257819"
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ta erişim gözden geçirmesi tamamlama
Ayrıcalıklı rol yöneticileri gözden geçirebileceğiniz ayrıcalıklı erişim kez bir [erişim gözden geçirmesi çalışmaya](pim-how-to-start-security-review.md). Azure AD Privileged Identity Management (PIM) otomatik olarak kullanıcıların erişim gözden geçirilecek kullanıcılar isteyen bir e-posta gönderir. Bir kullanıcı bir e-posta alma değil, onlara yönergeleri gönderebilirsiniz [erişim gözden geçirmesi gerçekleştirme](pim-how-to-perform-security-review.md).

Erişim gözden geçirmesi dönemi bittikten veya tüm kullanıcılar, kendi kendini gözden tamamladıktan sonra gözden geçirme yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim gözden geçirmeleri yönetme
1. Git [Azure portalında](https://portal.azure.com/) seçip **Azure AD Privileged Identity Management** Panonuzda uygulama.
2. Seçin **erişim gözden geçirmeleriyle** Pano bölümü.
3. Yönetmek istediğiniz erişim gözden geçirmesi seçin.

Erişim gözden geçirmesi ait ayrıntıları dikey penceresinde, gözden geçirme yönetmek için bir sayı seçenek vardır.

![PIM erişimi gözden geçirme düğmeleri - ekran görüntüsü](./media/pim-how-to-complete-review/PIM_review_buttons.png)

### <a name="remind"></a>Anımsat
Kullanıcılar kendilerine gözden geçirin. böylece erişim gözden geçirmesi ayarlanıp ayarlanmadığını **Anımsat** düğmesi bir bildirim gönderir. 

### <a name="stop"></a>Durdur
Tüm erişim gözden geçirmeleri bir bitiş tarihi vardır, ancak kullanabileceğiniz **Durdur** düğmesini erken tamamlayın. Herhangi bir kullanıcı bu zamana kadar gözden geçirmediklerinizden, inceleme durdurduktan sonra kullanıcılar şunları yapamaz. Durdurulmuş sonra bir gözden geçirme yeniden başlatılamıyor.

### <a name="apply"></a>Uygula
Erişim gözden geçirmesi tamamlandığında, ya da son tarihe ya da el ile durduruldu çünkü **Uygula** gözden geçirme sonucunu düğmesi uygular. Kullanıcı erişimi incelemesindeki reddedildiyse, kullanıcıların rol atamasını kaldıracak adım budur.  

### <a name="export"></a>Dışarı Aktarma
Erişim gözden geçirmesi sonuçlarını el ile uygulamak istiyorsanız, gözden geçirme dışarı aktarabilirsiniz. **Dışarı** düğmesi, bir CSV dosyası yükleme başlar. Excel veya CSV dosyalarını açın. diğer programları sonuçlarını yönetebilirsiniz.

### <a name="delete"></a>Sil
Diğer incelemesindeki ilgilenmiyorsanız silin. **Sil** düğmesini gözden PIM uygulamadan kaldırır.

> [!IMPORTANT]
> Değil silme gerçekleşmeden önce bir uyarı alırsınız, bu nedenle bu incelemeyi silmek istediğinizden emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
