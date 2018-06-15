---
title: Konuk kullanıcı - Azure Active Directory kuruluştan Ayrıl | Microsoft Docs
description: Erişim paneli kullanarak bir Azure AD B2B Konuk kullanıcı bir kuruluş nasıl bırakabilirsiniz gösterir.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 942439da3e116415c77a28950df69d44744b2dd8
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077546"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Kuruluş Konuk kullanıcı olarak bırakın

Bir Azure Active Directory (Azure AD) B2B Konuk kullanıcı artık bu kuruluş uygulamalardan kullanın veya herhangi bir ilişkisi korumak ihtiyaç duydukları herhangi bir anda bir kuruluş bırakın karar verebilirsiniz. Bir kullanıcı bir kuruluş, kendi, bir yöneticiye başvurun gerek kalmadan bırakabilirsiniz.

## <a name="leave-an-organization"></a>Kuruluştan Ayrıl

Kuruluş, açan bir kullanıcı olarak bırakmak için [erişim paneli](https://myapps.microsoft.com), aşağıdakileri yapın:

1. Ayrılmak istediğinizden kuruluş zaten oturum açtınız, adınızı seçin sağ üst köşedeki ve ayrılmak istiyor kuruluş'ı tıklatın.
2. Sağ üst köşedeki adınızın seçin.
3. Yanına **kuruluşların**, ayarları (dişli) simgesini seçin.
 
   ![Kullanıcı ayarlarını erişim panelinde gösteren ekran görüntüsü](media/active-directory-b2b-leave-the-organization/UserSettings.png) 

3. Altında **kuruluşların**, bırakın ve seçmek için istediğiniz kuruluş Bul **bırakın kuruluş**.

   ![Kullanıcı arabiriminde bırakın kuruluş seçeneğini gösteren ekran görüntüsü](media/active-directory-b2b-leave-the-organization/LeaveOrg.png)

4. Onaylamanız istendiğinde seçin **bırakın**. 

## <a name="account-removal"></a>Hesabınızın kaldırılması

Bir kullanıcı bir kuruluş ayrıldığında, kullanıcı hesabı "yumuşak silinir" dizininde. Varsayılan olarak, kullanıcı nesnesi taşır **kullanıcıların silinen** Azure AD alanında ancak değil kalıcı olarak 30 gün boyunca silindi. Bu yazılım silme yöneticinin kullanıcı hesabını 30 günlük dönem içinde geri yüklemek için bir istek yapıyorsa (gruplar ve izinler dahil) kullanıcı hesabını geri yüklemek sağlar.

İsterseniz, bir kiracı yöneticisi hesabı 30 günlük süre içinde herhangi bir zamanda kalıcı olarak silebilir. Bunu yapmak için:

1. İçinde [Azure portal](https://portal.azure.com)seçin **Azure Active Directory**.
2. Altında **Yönet**seçin **kullanıcılar**.
3. Seçin **kullanıcıların silinen**.
4. Silinmiş bir kullanıcı yanındaki onay kutusunu seçin ve ardından **kalıcı olarak silmek**.

Bir kullanıcıyı kalıcı olarak silerseniz, bu değiştirilemeyen bir eylemdir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B genel bakış için bkz: [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)



