---
title: Azure portalında - Azure Active Directory B2B işbirliği kullanıcı ekleme | Microsoft Docs
description: Bir yönetici Konuk kullanıcılar, dizinlerini Azure Active Directory (Azure AD) B2B işbirliği kullanarak bir iş ortağı kuruluştan eklemek için ne gösterir.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/02/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 34bd5b51089045c4cd20f29d179bb230e5e3fac2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="add-azure-active-directory-b2b-collaboration-users-in-the-azure-portal"></a>Azure portalında Azure Active Directory B2B işbirliği kullanıcı ekleme

Genel yönetici veya sınırlı yönetici dizin rolü atanmış bir kullanıcı olarak, B2B işbirliği kullanıcıları davet için Azure portalını kullanabilirsiniz. Konuk kullanıcılar dizin, bir grup veya uygulama davet edebilirsiniz. Bir kullanıcı bu yöntemlerin herhangi biriyle davet sonra davet edilen kullanıcı hesabı Azure Active Directory (Azure AD), bir kullanıcı türüyle eklenir *Konuk*. Konuk kullanıcı ardından kaynaklara erişmek için kendi davet almak gerekir.

## <a name="add-guest-users-to-the-directory"></a>Dizinine Konuk kullanıcılar ekleme

Dizine B2B işbirliği kullanıcılar eklemek için aşağıdaki adımları izleyin:

1. Oturum [Azure portal](https://portal.azure.com) Azure AD yönetici olarak.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
4. Seçin **yeni Konuk kullanıcı**.

   ![Yeni Konuk kullanıcı Arabiriminde nerede gösterir](./media/active-directory-b2b-admin-add-users/NewGuestUser-Directory.png) 
 
7. Altında **Konuk davet**, dış kullanıcı e-posta adresini girin. İsteğe bağlı olarak, bir Hoş Geldiniz iletisi içerir. Örneğin:

   ![Yeni Konuk kullanıcı Arabiriminde nerede gösterir](./media/active-directory-b2b-admin-add-users/InviteGuest.png) 

8. Seçin **davet** daveti Konuk kullanıcıya otomatik olarak göndermek için. İçinde **bildirim** alanında, görünüm için bir **başarıyla davet edilen kullanıcı** ileti. 
 
Davet gönderdikten sonra kullanıcı hesabı otomatik olarak dizinine konuk olarak eklendi.


![Konuk kullanıcı türü B2B kullanıcıyla gösterir](./media/active-directory-b2b-admin-add-users/GuestUserType.png)  

## <a name="add-guest-users-to-a-group"></a>Konuk kullanıcı bir gruba ekleme
Azure AD yönetici olarak bir gruba el ile B2B işbirliği kullanıcılar eklemek gerekiyorsa, şu adımları izleyin:

1. Oturum [Azure portal](https://portal.azure.com) Azure AD yönetici olarak.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **kullanıcılar ve gruplar** > **tüm grupları**.
4. Bir grup seçin (veya **yeni grup** yeni bir tane oluşturmak için). Grup içeren B2B Konuk kullanıcılar grubu eklemek için iyi bir fikirdir.
5. Seçin **üyeleri** > **üye eklemek**. 
6. Aşağıdakilerden birini yapın:
   - Konuk kullanıcı dizinde zaten varsa, B2B kullanıcıyı arayabilir. Kullanıcı Seç > tıklatın **seçin** kullanıcı grubuna eklemek için.
   - Konuk kullanıcı dizinde zaten mevcut değilse seçin **davet**.
   ![Konuk üye eklemek için davet et düğmesi ekleme](./media/active-directory-b2b-admin-add-users/GroupInvite.png)
   
      Altında **Konuk davet**, e-posta adresi ve isteğe bağlı kişisel bir ileti girin > seçin **davet**. Tıklatın **seçin** kullanıcı grubuna eklemek için.

      Daveti otomatik olarak davet edilen kullanıcıya gider. İçinde **bildirim** alanında, başarılı bir Ara **Invited kullanıcı** ileti. 

Ayrıca, dinamik grupların Azure AD B2B işbirliği ile kullanabilirsiniz. Daha fazla bilgi için bkz: [dinamik gruplar ve Azure Active Directory B2B işbirliği](active-directory-b2b-dynamic-groups.md).

## <a name="add-guest-users-to-an-application"></a>Konuk kullanıcılar için uygulama ekleme

B2B işbirliği kullanıcıları Azure AD yönetici olarak bir uygulama eklemek için aşağıdaki adımları izleyin:

1. Oturum [Azure portal](https://portal.azure.com) Azure AD yönetici olarak.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **kurumsal uygulamalar** > **tüm uygulamaları**.
4. Konuk kullanıcılar eklemek istediğiniz uygulamayı seçin.
5. Altında **Yönet**seçin **kullanıcılar ve gruplar**.
6. Seçin **Kullanıcı Ekle**.
7. Altında **eklemek atama**seçin **kullanıcı ve grupları**.
8. Aşağıdakilerden birini yapın:
   - Konuk kullanıcı dizinde zaten varsa, B2B kullanıcıyı arayabilir. Kullanıcıyı seçin ve ardından **seçin** kullanıcı uygulamaya eklemek için.
   - Konuk kullanıcı dizinde zaten mevcut değilse seçin **davet**.
   ![Konuk üye eklemek için davet et düğmesi ekleme](./media/active-directory-b2b-admin-add-users/AppInviteUsers.png)
   
      Altında **Konuk davet**, e-posta adresi ve isteğe bağlı kişisel bir ileti girin > seçin **davet**. Tıklatın **seçin** kullanıcı uygulamaya eklemek için.

      Daveti otomatik olarak davet edilen kullanıcıya gider. İçinde **bildirim** alanında, başarılı bir Ara **Invited kullanıcı** ileti.

9. Altında **eklemek atama**, tıklatın **rolü Seç** > (varsa) seçilen kullanıcı uygulamak için bir rol seçin > seçin **Tamam**.
10. **Ata**'ya tıklayın.
 
## <a name="resend-invitations-to-guest-users"></a>Konuk kullanıcılar Davetleri Gönder

Konuk kullanıcı henüz kendi davet kullanılan değil, daveti yeniden gönderebilirsiniz.

1. Oturum [Azure portal](https://portal.azure.com) Azure AD yönetici olarak.
2. Gezinti bölmesinde seçin **Azure Active Directory**.
3. Altında **Yönet**seçin **kullanıcılar ve gruplar**.
4. Seçin **tüm kullanıcılar**.
5. Kullanıcı hesabını seçin.
6. Altında **Yönet**seçin **profil**.
7. Kullanıcı henüz daveti kabul etmedi varsa bir **yeniden davet** seçeneği kullanılabilir. Yeniden göndermek için bu düğmeyi seçin.

   ![Kullanıcı profili yeniden gönder davet seçeneği](./media/active-directory-b2b-admin-add-users/Resend-Invitation.png)

> [!NOTE]
> İlk olarak belirli bir uygulamayı kullanıcıya yönelik bir davet yeniden gönderirseniz, yeni davet bağlantısını kullanıcı en üst düzey erişim paneli yerine aldığını anlayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure olmayan AD admins B2B Konuk kullanıcılar eklemek için ne öğrenmek için bkz: [nasıl bilgi çalışanları B2B işbirliği kullanıcı ekleme?](active-directory-b2b-iw-add-users.md)
- Davet e-posta hakkında daha fazla bilgi için bkz: [B2B işbirliği davet e-posta öğelerinden](active-directory-b2b-invitation-email.md).
- Davet alma işlemi hakkında daha fazla bilgi için bkz: [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md).


