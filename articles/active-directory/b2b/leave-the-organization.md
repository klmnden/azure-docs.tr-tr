---
title: Konuk kullanıcı - Azure Active Directory kuruluştan Ayrıl | Microsoft Docs
description: Erişim paneli kullanarak bir Azure AD B2B Konuk kullanıcı bir kuruluş nasıl bırakabilirsiniz gösterir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26d9eb883cc014c1bea092a12e22b6d144a37994
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112984"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Konuk kullanıcı olarak kuruluştan Ayrıl

Bir Azure Active Directory (Azure AD) B2B Konuk kullanıcı, uygulamaları söz konusu kuruluştaki kullanın veya herhangi bir ilişkisi korumak bunlar artık ihtiyacınız yoksa, herhangi bir zamanda kuruluştan Ayrıl geçmeye karar verebilirsiniz. Bir kullanıcı bir kuruluş, kendi yöneticiye başvurun gerek kalmadan bırakabilirsiniz.

> [!NOTE]
> Giriş kiracısında veya kaynak Kiracı hesabını devre dışı bırakılırsa, Konuk kullanıcı bir kuruluştan ayrılamazsınız. Hesabını devre dışı bırakılırsa, Konuk kullanıcı kullanan Konuk hesabı silebilir veya kullanıcının kuruluşu terk etmesi için konuk hesabını etkinleştirin Kiracı yöneticisine başvurun gerekecektir.

## <a name="leave-an-organization"></a>Kuruluştan ayrılma

Bir kuruluş olarak bırakmak için aşağıdaki adımları izleyin.

1. Aşağıdakilerden birini yaparak erişim paneli profil sayfanıza gidin:
   
   - İçinde [Azure portalında](https://portal.azure.com), adınıza sağ tıklayın ve seçin **hesabı görüntüle**.
   - Açık, [erişim paneli](https://myapps.microsoft.com), üst, sağ ve bir sonraki adınıza tıklayın **kuruluşlar**, ayarları (dişli) simgesini seçin.
 
   ![Erişim panelinde kullanıcı ayarları gösteren ekran görüntüsü](media/leave-the-organization/UserSettings.png) 

   > [!NOTE]
   > Kuruluşa henüz oturum açmadıysanız, altında ayrılmak istediğinizden **kuruluşlar**, tıklayın **kuruluştan ayrılmak için oturum** kuruluşun adının yanındaki bağlantı. Açtıktan sonra tekrar sağ üst köşedeki ve yanındaki adınızı tıklatın **kuruluşlar**, ayarları (dişli) simgesini seçin.

3. Altında **kuruluşlar**, bulmak, bırakın ve istediğiniz kuruluş **bırakın kuruluş**.

   ![Kullanıcı arabiriminde bırakın kuruluş seçeneğini gösteren ekran görüntüsü](media/leave-the-organization/LeaveOrg.png)

4. Onaylamak için sorulduğunda seçin **bırakın**. 

## <a name="account-removal"></a>Hesabınızın kaldırılması

Bir kullanıcı, bir kuruluşun ayrıldığında, kullanıcı hesabının "yumuşak silinir" dizininde. Varsayılan olarak, kullanıcı nesnesinin taşır **silinmiş kullanıcılarını** Azure AD alanında 30 gün boyunca kalıcı olarak silinmez, ancak. Bu geçici silme yöneticisinin kullanıcı hesabını 30 günlük süre içinde geri yüklemek için bir istek yaparsa (grupları ve izinleri dahil) kullanıcı hesabının, geri yükleme sağlar.

İsterseniz, bir kiracı yöneticisi hesabı 30 günlük süre boyunca herhangi bir zamanda kalıcı olarak silebilirsiniz. Bunu yapmak için:

1. İçinde [Azure portalında](https://portal.azure.com)seçin **Azure Active Directory**.
2. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
3. Seçin **silinmiş kullanıcılarını**.
4. Silinen bir kullanıcıyı yanındaki onay kutusunu işaretleyin ve ardından **kalıcı olarak silmek**.

Kullanıcı kalıcı olarak siler, bu değiştirilemeyen bir eylemdir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B genel bakış için bkz. [Azure AD B2B işbirliği nedir?](what-is-b2b.md)



