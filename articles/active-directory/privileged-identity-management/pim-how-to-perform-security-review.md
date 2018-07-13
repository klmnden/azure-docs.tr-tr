---
title: Erişim gözden geçirmesi gerçekleştirme | Microsoft Docs
description: Azure Privileged Identity Management uygulaması ile bir inceleme gerçekleştirme konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4d19f9e04b6288fad70f62e95579475c16f2112c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590341"
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ta erişim gözden geçirmesi gerçekleştirme
Azure Active Directory (AD) Privileged Identity Management, kuruluşlara ayrıcalıklı Azure ad'deki ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi yönetme basitleştirir.  

Bir yönetici rolüne atanırsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi yine de bu rol için işinizi gerektiğini düzenli olarak doğrulamanızı isteyebilir. Bir bağlantısını içeren bir e-posta alabilir veya doğrudan gidebilirsiniz [Azure portalında](https://portal.azure.com). Bir kendi kendini gözden geçirin, atanan rollerinin gerçekleştirmek için bu makaledeki adımları izleyin.

Ayrıcalıklı Rol Yöneticisi veya genel yönetici erişim gözden geçirmelerine ilgilenen kullanıyorsanız daha fazla bilgi alın [erişim gözden geçirmesi başlatma](pim-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management (PIM) uygulamasında kullanabileceğiniz [Azure portalında](https://portal.azure.com/) , gözden geçirmek için.  Azure AD Privileged Identity Management uygulaması portalınızda yoksa, başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalı ve burada şunları yapacaksınız, dizini seçin, sağ üst köşedeki kullanıcı adınızı işletim seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Onaylayın veya reddedin erişim
Onaylayabilir ya da erişimi reddetmek yalnızca Gözden Geçiren, yine de bu rolü veya kullanıp kullanmadığını belirten. Seçin **Onayla** rolünde kalmak istiyorsanız veya **Reddet** erişim artık ihtiyacınız yoksa. Sonuçları Gözden Geçiren geçerli olana kadar durumu hemen değişmez.
Erişim değerlendirmesi tamamlama ve bulmak için aşağıdaki adımları izleyin:

1. PIM uygulamada seçin **ayrıcalıklı erişim incelemesi**. Tüm beklemedeki erişim gözden geçirmeleri varsa, bunlar Azure AD erişim gözden geçirmeleri dikey penceresinde görüntülenir.
2. Gözden geçirmeyi tamamlamak istiyorsanız seçin.
3. Gözden geçirmeyi oluşturduğunuz sürece gözden tek kullanıcısı olarak görünür. Adınızın yanındaki onay işaretini seçin.
4. Seçin ya da **onaylama** veya **Reddet**. İçinde kararınız için bir neden dahil etmeniz gerekebilir **nedeninizi belirtin** metin kutusu.  
5. Kapat **gözden geçirme Azure AD rolleri** dikey penceresi.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
