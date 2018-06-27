---
title: Bir erişim incelemesi gerçekleştirme | Microsoft Docs
description: Azure Privileged Identity Management uygulaması ile incelemesi gerçekleştirme öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7c536a4e7f93a2f1ef42b7600513994dd80826a0
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37020879"
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management erişim incelemesi gerçekleştirme
Azure Active Directory (AD) Privileged Identity Management, kuruluşların kaynaklarına Azure AD'de ve Office 365 veya Microsoft Intune gibi diğer Microsoft online services ayrıcalıklı erişimi nasıl yönetmek basitleştirir.  

Bir yönetici rolü atanmışsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi düzenli olarak bu rol hala işiniz için ihtiyacınız doğrulamanızı isteyebilir. Bir bağlantı içeren bir e-posta alabilirsiniz veya doğrudan gidebilirsiniz [Azure portal](https://portal.azure.com). Atanan rollerinizi Self gözden gerçekleştirmek için bu makaledeki adımları izleyin.

Ayrıcalıklı Rol Yöneticisi veya genel yönetici erişimi incelemeler ilgilenen değilseniz, daha fazla bilgi almak [erişim incelemesi başlatma](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management (PIM) uygulamasında kullanabileceğiniz [Azure portal](https://portal.azure.com/) incelemeniz gerçekleştirmek için.  Azure AD Privileged Identity Management uygulaması portalınızda yoksa başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalı ve burada şunları yapacaksınız, dizin seçin, sağ köşedeki kullanıcı adınıza işletim seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Onaylamak veya erişim engelle
Onaylama ya da erişimini yalnızca İnceleme bu rolü veya kullanmaya devam olup olmadığını belirten. Seçin **Onayla** rolünde olmak istiyorsanız veya **reddetme** erişim artık ihtiyaç duymuyorsanız. İnceleme sonuçları geçerli olana kadar durumunuzu hemen değiştirmez.
Bul ve erişim gözden geçirme tamamlamak için aşağıdaki adımları izleyin:

1. PIM uygulamada seçin **gözden geçirme ayrıcalıklı erişim**. Tüm bekleyen erişim incelemeler varsa, bunlar Azure AD erişim incelemeler dikey penceresinde görüntülenir.
2. Tamamlamak için istediğiniz gözden geçirme seçin.
3. Gözden geçirme oluşturduğunuz sürece, yalnızca kullanıcı gözden olarak görünür. Adınızın yanındaki onay işaretini seçin.
4. Ya da seçin **onaylama** veya **Reddet**. Kararınızı içinde nedeni eklemeniz gerekebilir **bir gerekçe sağlamasını** metin kutusu.  
5. Kapat **gözden geçirme Azure AD rolleri** dikey.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
