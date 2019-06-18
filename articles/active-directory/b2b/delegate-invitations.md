---
title: B2B dış işbirliği ayarları - Azure Active Directory etkinleştirme | Microsoft Docs
description: Dış Active Directory B2B işbirliği sağlar ve Konuk kullanıcılar davet edebilirsiniz yönetme hakkında bilgi edinin. Temsilci davetleri için konuk davet eden rolü kullanın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11dda7fc3760f468c094fb4cf4484a27895f83b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65812683"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>Dış B2B işbirliği sağlar ve Konukları davet edebileceği yönetme

Bu makalede, Azure Active Directory (Azure AD) B2B işbirliği etkinleştirmek ve Konukları davet edebileceği belirlemek açıklar. Bir yönetici rolüne atanmış oldukları değil olsa bile varsayılan olarak, tüm kullanıcılar ve konuklar, dizinde Konukları davet edebilirsiniz. Dış işbirliği ayarlarını kuruluşunuzdaki kullanıcılar farklı türleri için konuk davet açıp kapatma olanak tanır. Bireysel kullanıcılara davet Konukları davet olanak tanıyan rolleri atayarak da devredebilirsiniz.

## <a name="configure-b2b-external-collaboration-settings"></a>B2B dış işbirliği ayarlarını yapılandırma

Azure AD B2B işbirliği ile bir kiracı Yöneticisi aşağıdaki davet ilkeleri ayarlayabilirsiniz:

- Davet Kapat
- Yalnızca Yöneticiler ve Konuk davet eden rolündeki kullanıcılar davet edebilir
- Yöneticiler, Konuk davet eden rolü ve üyeler davet edebilir
- Konuklar, dahil olmak üzere tüm kullanıcıları davet edebilir

Varsayılan olarak, tüm kullanıcılar, Konuklar, dahil, Konuk kullanıcılar davet edebilirsiniz.

### <a name="to-configure-external-collaboration-settings"></a>Dış işbirliği ayarlarını yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) bir kiracı Yöneticisi olarak.
2. Seçin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
3. Altında **dış kullanıcılar**seçin **dış işbirliği ayarlarını yönetin**.
   > [!NOTE]
   > **Dış işbirliği ayarları** ayrıca kullanılabilir **kuruluş ilişkileri** sayfası. Azure Active Directory'de altında **Yönet**Git **kuruluş ilişkileri** > **ayarları**.
4. Üzerinde **dış işbirliği ayarları** sayfasında, etkinleştirmek istediğiniz ilke seçin.

   ![Dış işbirliği ayarları](./media/delegate-invitations/control-who-to-invite.png)

  - **Konuk kullanıcıların izinleri sınırlıdır**: Bu ilke, dizininizdeki konukların izinlerini belirler. Seçin **Evet** blok konukların kullanıcıları, grupları veya diğer dizin kaynaklarını numaralandırma gibi belirli dizin görevleri için. Seçin **Hayır** konuklar aynı dizin verilerini dizininizdeki normal kullanıcılarla erişmenizi sağlayacak.
   - **Yöneticiler ve Konuk davet eden rolündeki kullanıcı davet edebildiğiniz**: Yöneticiler ve kullanıcılar "Konuk davet eden" roldeki Konukları davet izin vermek için bu ilke ayarlanan **Evet**.
   - **Üyeler davet edebildiğiniz**: Konukları davet etmek dizininizin yönetici olmayan üyelerin izin vermek için bu ilke ayarlanan **Evet**.
   - **Konuklar davet edebildiğiniz**: Diğer Konukları davet Konukları izin vermek için bu ilke ayarlanan **Evet**.
   - **E-posta kerelik geçiş kodu konuklar için (Önizleme) etkinleştirme**: Bir kerelik geçiş kodu özelliği hakkında daha fazla bilgi için bkz. [e-posta bir kerelik geçiş kodu kimlik doğrulama (Önizleme)](one-time-passcode.md).
   - **İşbirliği kısıtlamaları**: İzin verme veya belirli etki alanlarına davetleri engelleme hakkında daha fazla bilgi için bkz. [B2B kullanıcıları için izin verilenler veya Engellenenler davetleri belirli kuruluşlardan](allow-deny-list.md).

## <a name="assign-the-guest-inviter-role-to-a-user"></a>Konuk davet eden rolü kullanıcıya atayın

Konuk davet eden rolü ile tek tek kullanıcılar bir genel yönetici veya başka bir yönetici rolü atamadan Konukları davet olanağı verebilirsiniz. Konuk davet edici rolüne kişilere atayın. Ayarladığınız emin **Yöneticiler ve Konuk davet eden rolündeki kullanıcı davet edebildiğiniz** için **Evet**.

Konuk davet eden rolüne kullanıcı eklemek için PowerShell'i kullanmayı gösteren bir örnek aşağıda verilmiştir:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği Konuk kullanıcıları davet etmeden ekleme](add-user-without-invite.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)


