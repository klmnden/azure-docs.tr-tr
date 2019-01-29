---
title: Dış kullanıcıları davet ve PIM Azure kaynak rolleri atama | Microsoft Docs
description: Dış kullanıcıları davet ve Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri atama hakkında bilgi edinin.
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
ms.date: 11/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: f3cfb792b9c9befcbc2ee869ef5b31903e5c10f9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188618"
---
# <a name="invite-external-users-and-assign-azure-resource-roles-in-pim"></a>Dış kullanıcıları davet ve PIM Azure kaynak rolleri atama

Azure Activity Directory (Azure AD)--işletmeler arası (B2B) işbirliği yapmak dış kullanıcılarla ve herhangi bir hesabı kullanarak satıcılar kuruluşların Azure AD'de olan bir özellik kümesi var. Azure AD Privileged Identity Management (PIM) ile B2B birleştirdiğinizde, uyumluluk ve idare gereksinimlerinizi dış kullanıcılara uygulamak devam edebilirsiniz. Örneğin, Azure kaynakları için PIM özelliklere dış kullanıcılarla kullanabilirsiniz:

- Belirli Azure kaynaklarına erişim atama
- Tam zamanında erişim etkinleştir
- Atama süresi ve bitiş tarihi belirtin
- Etkin atamada veya etkinleştirme MFA gerektirme
- Erişim gözden geçirmesi gerçekleştirme
- Denetim günlükleri ve Uyarıları kullanma

Bu makalede, bir dış kullanıcının dizininize davet edin ve kendi PIM kullanarak Azure kaynaklarına erişimi yönetmek açıklar.

## <a name="when-would-you-invite-external-users"></a>Ne zaman dış kullanıcıları davet?

Dış kullanıcılar dizininize davet edebilir, birkaç örnek senaryolar şunlardır:

- Yalnızca bir proje için Azure kaynaklarınıza erişmek için bir e-posta hesabı olan bir dış şirket içinde çalıştırılan satıcısı izin verir.
- Harici bir iş ortağı gider uygulamanıza erişmek için şirket içi Active Directory Federasyon Hizmetleri kullanan büyük bir kuruluş içinde izin verir.
- Destek mühendislerine geçici olarak sorunlarını gidermek için Azure kaynağınıza erişmek için değil (Microsoft desteği gibi), kuruluşunuzdaki izin verin.

## <a name="how-does-external-collaboration-using-b2b-work"></a>Dış işbirliği nasıl yaptığını kullanarak B2B iş?

B2B kullandığınızda, bir dış kullanıcının dizininize davet edebilirsiniz. Dizininizdeki dış kullanıcı görünüyor, ancak kullanıcının kendisiyle ilişkilendirilmiş herhangi bir kimlik bilgisi yok. Bir dış kullanıcının bağlanılabilir olduğunda, kendi giriş dizininde değil, dizin de doğrulanmalıdır. Bu, dış kullanıcı artık kendi giriş dizininde erişimi varsa, bunlar otomatik olarak dizininize erişimlerini olduğunu anlamına gelir. Dış kullanıcı kuruluşu terk ederse, örneğin, bunlar otomatik olarak erişim, dizininizde herhangi bir şey gerek kalmadan paylaşılan herhangi bir kaynağa kaybedersiniz. B2B hakkında daha fazla bilgi için bkz: [Azure Active Directory B2B Konuk kullanıcı erişimini nedir?](../b2b/what-is-b2b.md).

![B2B ve dış kullanıcı](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-external-collaboration-settings"></a>Dış işbirliği ayarlarını kontrol edin

Dış kullanıcılar dizininize davet emin olmak için dış işbirliği ayarları denetlemelisiniz.

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. Tıklayın **Azure Active Directory** > **kullanıcı ayarları**.

1. Tıklayın **dış işbirliği ayarlarını yönetin**.

    ![Dış işbirliği ayarları](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Emin olun **Yöneticiler ve Konuk davet eden rolündeki kullanıcı davet edebildiğiniz** anahtar ayarlanmıştır **Evet**.

## <a name="invite-an-external-user-and-assign-a-role"></a>Bir dış kullanıcıyı davet edin ve rol atama

PIM kullanarak, bir dış kullanıcıyı davet edin ve bir Azure Kaynak rolü gibi bir üye kullanıcı için uygun.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) veya [kullanıcı hesabı yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#user-account-administrator) rol.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Kullanım **kaynak filtresi** yönetilen kaynaklar listesine filtre uygulamak için.

1. Bir kaynak, kaynak grubu, abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağa tıklayın.

    Dış kullanıcı yeterlidir, kapsamı ayarlamanız gerekir.

1. Yönet altında **rolleri** Azure kaynakları için rolleri listesini görmek için.

    ![Azure kaynak rolleri](./media/pim-resource-roles-external-users/resources-roles.png)

1. Kullanıcı gereken en düşük rolü tıklatın.

    ![Seçilen rol](./media/pim-resource-roles-external-users/selected-role.png)

1. Rol sayfasında tıklayın **Üye Ekle** yeni atama bölmesini açmak için.

1. Tıklayın **üyesi veya Grup Seç**.

    ![Üye veya grup seçin](./media/pim-resource-roles-external-users/select-member-group.png)

1. Bir dış kullanıcıyı davet etmek için tıklayın **davet**.

    ![Konuk davet et](./media/pim-resource-roles-external-users/invite-guest.png)

1. Bir dış kullanıcının belirttikten sonra tıklayın **davet**.

    Dış kullanıcı seçili bir üyesi olarak eklenmelidir.

1. Bir üye veya grup bölmesi Seç'e tıklayın **seçin**.

1. Üyelik ayarları bölmesinde atama türü ve süresini seçin.

    ![Üyelik ayarları](./media/pim-resource-roles-external-users/membership-settings.png)

1. Atamayı tamamlamak için tıklatın **Bitti** ardından **Ekle**.

    Dış kullanıcı rol ataması rol listenizde görünecektir.

    ![Dış kullanıcı için rol ataması](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-an-external-user"></a>Bir dış kullanıcı olarak rol etkinleştirme

Dış kullanıcı olarak, ilk Azure AD dizinine daveti kabul etmek ve büyük olasılıkla, rolü etkinleştirmek için ihtiyaç duyduğunuz.

1. E-posta, dizin daveti ile açın. E-posta aşağıdakine benzer olacaktır.

    ![E-posta daveti](./media/pim-resource-roles-external-users/email-invite.png)

1. Tıklayın **Başlarken** e-postadaki bağlantıya.

1. İzinleri gözden geçirdikten sonra **kabul**.

    ![İzinleri gözden geçir](./media/pim-resource-roles-external-users/invite-accept.png)

1. Kullanım koşullarını kabul edin ve oturum açık kalsın isteyip istemediğinizi belirtmek için istenebilir.

    Azure portalı açılır. Yalnızca bir rol için uygun durumda kaynaklarına erişemezsiniz.

1. Rolü etkinleştirmek için e-posta ile açın, rol bağlantısını etkinleştirin. E-posta aşağıdakine benzer olacaktır.

    ![E-posta daveti](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Tıklayın **rolü etkinleştir** uygun rollerinizi PIM'de açın.

    ![Rollerim - uygun](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. Tıklama eylemi altında **etkinleştirme** bağlantı.

    Rol ayarlara bağlı olarak, rolü etkinleştirmek için bazı bilgiler belirtmeniz gerekecektir.

1. Rolü için ayarları belirttikten sonra tıklayın **etkinleştirme** rolü etkinleştirmek için.

    ![Rolü etkinleştir](./media/pim-resource-roles-external-users/activate-role.png)

    Yönetici isteğinizi onaylamanız gerekli değilse, belirtilen kaynaklara erişimi olmalıdır.

## <a name="view-activity-for-an-external-user"></a>Bir dış kullanıcı etkinlikleri görüntüle

Yalnızca bir üye kullanıcı gibi dış kullanıcıların ne yaptıklarını izlemek için denetim günlüklerini görüntüleyebilirsiniz.

1. Bir yönetici olarak PIM açın ve paylaşılan kaynağı seçin.

1. Tıklayın **kaynak denetim** etkinlik söz konusu kaynak görüntüleme. Aşağıdaki örnek bir kaynak grubu için etkinliğin gösterir.

    ![Kaynak denetimi](./media/pim-resource-roles-external-users/audit-resource.png)

1. Dış kullanıcı için etkinliğini görüntülemek için tıklayın **Azure Active Directory** > **kullanıcılar** > dış kullanıcı.

1. Tıklayın **denetim günlükleri** directory denetim günlüklerini görmek için. Gerekirse, filtreler belirtebilirsiniz.

    ![Dizin denetimi](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD dizin rollerini PIM atayın](pim-how-to-add-role-to-user.md)
- [Azure Active Directory B2B Konuk kullanıcı erişimini nedir?](../b2b/what-is-b2b.md)
