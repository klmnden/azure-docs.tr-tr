---
title: Bir erişim incelemesi Privileged Identity Management Azure kaynakları için gerçekleştirme | Microsoft Docs
description: Bu belge, gerçekleştirmek ve PIM gözden geçirme Azure kaynakları için erişim açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4afb923058143dd1771043db8433aa3a65541bf7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---perform-access-review"></a>Privileged Identity Management - Kaynak rolü - gerçekleştirmek erişim gözden geçirme
Azure kaynakları için Privileged Identity Management, kuruluşların Azure kaynaklarına ayrıcalıklı erişimi nasıl yönettiğini basitleştirir. 

Bir yönetici rolü atanmışsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi düzenli olarak bu rol hala işiniz için ihtiyacınız doğrulamanızı isteyebilir. Bir bağlantı içeren bir e-posta alabilirsiniz veya doğrudan gidebilirsiniz [Azure portal](https://portal.azure.com). Atanan rollerinizi Self gözden gerçekleştirmek için bu makaledeki adımları izleyin.

Erişim incelemeler ilgilenen bir ayrıcalıklı rol yöneticisi değilseniz, daha fazla bilgi almak [erişim incelemesi başlatma](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management (PIM) uygulamasında kullanabileceğiniz [Azure portal](https://portal.azure.com/) incelemeniz gerçekleştirmek için.  Azure AD Privileged Identity Management uygulaması portalınızda yoksa başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalı ve burada şunları yapacaksınız, dizin seçin, sağ köşedeki kullanıcı adınıza işletim seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Onaylamak veya erişim engelle
Onaylama ya da erişimini yalnızca İnceleme bu rolü veya kullanmaya devam olup olmadığını belirten. Seçin **Onayla** rolünde olmak istiyorsanız veya **reddetme** erişim artık ihtiyaç duymuyorsanız. İnceleme sonuçları geçerli olana kadar durumunuzu hemen değiştirmez.
Bul ve erişim gözden geçirme tamamlamak için aşağıdaki adımları izleyin:
1. Azure AD Privileged Identity Management uygulamaya gidin.
2. Gözden geçirme erişim dikey seçin

![](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Tamamlamak için istediğiniz gözden geçirme seçin. 
4. Ya da seçin **onaylama** veya **Reddet**. Kararınızı içinde nedeni eklemeniz gerekebilir **bir gerekçe sağlamasını** metin kutusu.

![](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
