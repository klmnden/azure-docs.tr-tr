---
title: Konuk kullanıcı - Azure Active Directory kuruluştan Ayrıl | Microsoft Docs
description: Erişim paneli kullanarak bir Azure AD B2B Konuk kullanıcı bir kuruluş nasıl bırakabilirsiniz gösterir.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 423de49faa20289a7c5663ae621f890b63a5752c
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428520"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Konuk kullanıcı olarak kuruluştan Ayrıl

Bir Azure Active Directory (Azure AD) B2B Konuk kullanıcı, uygulamaları söz konusu kuruluştaki kullanın veya herhangi bir ilişkisi korumak bunlar artık ihtiyacınız yoksa, herhangi bir zamanda kuruluştan Ayrıl geçmeye karar verebilirsiniz. Bir kullanıcı bir kuruluş, kendi yöneticiye başvurun gerek kalmadan bırakabilirsiniz.

## <a name="leave-an-organization"></a>Kuruluştan ayrılma

Bir kullanıcı oturum açtığı olarak bir kuruluştan ayrılmak için [erişim paneli](https://myapps.microsoft.com), aşağıdakileri yapın:

1. Ayrılmak istediğinizden kuruluş henüz oturum açmadıysanız, sağ üst köşede adınızı seçin ve bırakmak istediğiniz kuruluş'a tıklayın.
2. Sağ üst köşede adınızı seçin.
3. Yanındaki **kuruluşlar**, ayarları (dişli) simgesini seçin.
 
   ![Erişim panelinde kullanıcı ayarları gösteren ekran görüntüsü](media/leave-the-organization/UserSettings.png) 

3. Altında **kuruluşlar**, bulmak, bırakın ve istediğiniz kuruluş **bırakın kuruluş**.

   ![Kullanıcı arabiriminde bırakın kuruluş seçeneğini gösteren ekran görüntüsü](media/leave-the-organization/LeaveOrg.png)

4. Onaylamak için sorulduğunda seçin **bırakın**. 

## <a name="account-removal"></a>Hesabınızın kaldırılması

Bir kullanıcı, bir kuruluşun ayrıldığında, kullanıcı hesabının "yumuşak silinir" dizininde. Varsayılan olarak, kullanıcı nesnesinin taşır **silinmiş kullanıcılarını** Azure AD alanında ancak değil kalıcı olarak 30 gün boyunca silindi. Bu geçici silme yöneticisinin kullanıcı hesabını 30 günlük süre içinde geri yüklemek için bir istek yaparsa (grupları ve izinleri dahil) kullanıcı hesabının, geri yükleme sağlar.

İsterseniz, bir kiracı yöneticisi hesabı 30 günlük süre boyunca herhangi bir zamanda kalıcı olarak silebilirsiniz. Bunu yapmak için:

1. İçinde [Azure portalında](https://portal.azure.com)seçin **Azure Active Directory**.
2. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
3. Seçin **silinmiş kullanıcılarını**.
4. Silinen bir kullanıcıyı yanındaki onay kutusunu işaretleyin ve ardından **kalıcı olarak silmek**.

Kullanıcı kalıcı olarak siler, bu değiştirilemeyen bir eylemdir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B genel bakış için bkz. [Azure AD B2B işbirliği nedir?](what-is-b2b.md)



