---
title: Konuklar davet edin ve Azure kaynağı rolleri PIM - Azure Active Directory atama | Microsoft Docs
description: Dış Konuk kullanıcıları davet ve Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri atama hakkında bilgi edinin.
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
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07476c9f5db64a5d107a493022fa3548fe0dae4c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476352"
---
# <a name="invite-guest-users-and-assign-azure-resource-roles-in-pim"></a>Konuk kullanıcıları davet ve PIM Azure kaynak rolleri atama

Azure Active Directory (Azure AD)--işletmeler arası (B2B) özellikleri, kuruluşların dış konuk kullanıcılara (konuk) ve herhangi bir hesabı kullanarak satıcılar ile işbirliği sağlayan Azure AD'de kümesidir. B2B ile Azure AD Privileged Identity Management (PIM) birleştirdiğinizde, Konuklar, uyumluluk ve idare gereksinimleri uygulamak devam edebilirsiniz. Örneğin, bu PIM özellikleri Konukları ile Azure kimlik görevler için kullanabilirsiniz:

- Belirli Azure kaynaklarına erişim atama
- Tam zamanında erişim etkinleştir
- Atama süresi ve bitiş tarihi belirtin
- Etkin atamada veya etkinleştirme MFA gerektirme
- Erişim gözden geçirmesi gerçekleştirme
- Denetim günlükleri ve Uyarıları kullanma

Bu makalede, kuruluşunuz için konuk davet et ve bunların Privileged Identity Management'ı kullanarak Azure kaynaklarına erişimi yönetmek açıklar.

## <a name="when-would-you-invite-guests"></a>Ne zaman Konukları davet?

Kuruluşunuz için Konukları davet edebilir, birkaç örnek senaryolar şunlardır:

- Yalnızca bir proje için Azure kaynaklarınıza erişmek için bir e-posta hesabı olan bir dış şirket içinde çalıştırılan satıcısı izin verir.
- Harici bir iş ortağı gider uygulamanıza erişmek için şirket içi Active Directory Federasyon Hizmetleri kullanan büyük bir kuruluş içinde izin verir.
- Destek mühendislerine geçici olarak sorunlarını gidermek için Azure kaynağınıza erişmek için değil (Microsoft desteği gibi), kuruluşunuzdaki izin verin.

## <a name="how-does-collaboration-using-b2b-guests-work"></a>Nasıl işbirliği yaptığını B2B kullanarak iş konukların?

B2B işbirliği kullandığınızda, bir dış kullanıcının kuruluşunuza bir konuk olarak davet edebilirsiniz. Konuk, kuruluşunuzda görünüyor ancak Konuk kendisiyle ilişkilendirilmiş herhangi bir kimlik bilgisi yok. Bir konuk bağlanılabilir olduğunda giriş kuruluşlarındaki ve, kuruluşunuzdaki doğrulanmalıdır. Bu, Konuk artık giriş kuruluşlarında erişimi varsa, bunlar aynı zamanda kuruluşunuza erişimlerini olduğunu anlamına gelir. Konuk kendi kuruluştan ayrılırsa, örneğin, bunlar otomatik olarak erişim, Azure AD'de herhangi bir şey gerek kalmadan paylaşılan herhangi bir kaynağa kaybedersiniz. B2B hakkında daha fazla bilgi için bkz: [Azure Active Directory B2B Konuk kullanıcı erişimini nedir?](../b2b/what-is-b2b.md).

![Nasıl bir Konuk kullanıcı dizininizde görünüyor, ancak kendi giriş dizininde doğrulanır gösteren diyagram](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-guest-collaboration-settings"></a>Konuk işbirliği ayarlarını kontrol edin

Kuruluşunuzun Konukları davet edebileceği emin olmak için konuk işbirliği ayarlarınızı denetlemelisiniz.

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. Tıklayın **Azure Active Directory** > **kullanıcı ayarları**.

1. Tıklayın **dış işbirliği ayarlarını yönetin**.

    ![Dış işbirliği Ayarları sayfasında gösterme işbirliği izni ve davet kısıtlama ayarları](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Emin olun **Yöneticiler ve Konuk davet eden rolündeki kullanıcı davet edebildiğiniz** anahtar ayarlanmıştır **Evet**.

## <a name="invite-a-guest-and-assign-a-role"></a>Konuk davet et ve rol atama

PIM kullanarak Konuk davet et ve bunları bir Azure Kaynak rolü gibi bir üye kullanıcı için uygun olarak belirleyemezsiniz.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) veya [Kullanıcı Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) rol.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Kullanım **kaynak filtresi** yönetilen kaynaklar listesine filtre uygulamak için.

1. Bir kaynak, kaynak grubu, abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağa tıklayın.

    Yalnızca Konuk hangi gerekiyor kapsamı ayarlamanız gerekir.

1. Yönet altında **rolleri** Azure kaynakları için rolleri listesini görmek için.

    ![Azure kaynak rolleri etkin ve uygun olan kullanıcıların sayısını gösteren liste](./media/pim-resource-roles-external-users/resources-roles.png)

1. Kullanıcı gereken en düşük rolü tıklatın.

    ![Seçili rolü sayfası, rolünün geçerli üyeleri listeleme](./media/pim-resource-roles-external-users/selected-role.png)

1. Rol sayfasında tıklayın **Üye Ekle** yeni atama bölmesini açmak için.

1. Tıklayın **üyesi veya Grup Seç**.

    ![Yeni atama - bir üye seçin veya kullanıcıları ve grupları davet seçeneği ile birlikte listeleyen bölmesinde Grup](./media/pim-resource-roles-external-users/select-member-group.png)

1. Konuk davet etmek için tıklayın **davet**.

    ![Bir konuk sayfa kutuları bir e-posta adresi girin ve kişisel bir ileti belirtmek için davet edin](./media/pim-resource-roles-external-users/invite-guest.png)

1. Bir konuk seçtikten sonra tıklayın **davet**.

    Konuk, seçili bir üyesi olarak eklenmelidir.

1. İçinde **üyesi veya Grup Seç** bölmesinde tıklayın **seçin**.

1. İçinde **üyelik ayarlarını** bölmesinde atama türü ve süresini seçin.

    ![Yeni atama - atama türünü belirtin, başlangıç tarihi ve bitiş tarihi için seçeneklerle üyeliği Ayarları sayfası](./media/pim-resource-roles-external-users/membership-settings.png)

1. Atamayı tamamlamak için tıklatın **Bitti** ardından **Ekle**.

    Konuk rol ataması rol listenizde görünecektir.

    ![Rol sayfası Konuk uygun olarak listeleme](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-a-guest"></a>Bir konuk olarak rol etkinleştirme

Dış kullanıcı olarak, ilk Azure AD kuruluşunuz için daveti kabul etmek ve büyük olasılıkla, rolü etkinleştirmek için ihtiyaç duyduğunuz.

1. E-posta daveti ile açın. E-posta aşağıdakine benzer olacaktır.

    ![Dizin adı, kişisel bir ileti ve Get Started bağlantısını içeren e-posta daveti](./media/pim-resource-roles-external-users/email-invite.png)

1. Tıklayın **Başlarken** e-postadaki bağlantıya.

1. İzinleri gözden geçirdikten sonra **kabul**.

    ![Kuruluş gözden geçirmek istediğiniz izinlerin bir listesi ile bir tarayıcıda izinleri sayfası gözden geçirin](./media/pim-resource-roles-external-users/invite-accept.png)

1. Kullanım koşullarını kabul edin ve oturum açık kalsın isteyip istemediğinizi belirtmek için istenebilir.

    Azure portalı açılır. Yalnızca bir rol için uygun durumda kaynaklarına erişemezsiniz.

1. Rolü etkinleştirmek için e-posta ile açın, rol bağlantısını etkinleştirin. E-posta aşağıdakine benzer olacaktır.

    ![Bu, uygun bir etkinleştir rol bağlantısına sahip bir rol için belirten PIM gelen e-posta iletisi](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Tıklayın **rolü etkinleştir** uygun rollerinizi PIM'de açın.

    ![Uygun rol listeleme PIM rolleri sayfam](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. Tıklama eylemi altında **etkinleştirme** bağlantı.

    Rol ayarlara bağlı olarak, rolü etkinleştirmek için bazı bilgiler belirtmeniz gerekecektir.

1. Rolü için ayarları belirttikten sonra tıklayın **etkinleştirme** rolü etkinleştirmek için.

    ![Sayfa listesi kapsamı ve başlangıç saatini, süresini ve neden belirtmek için seçenekleri etkinleştir](./media/pim-resource-roles-external-users/activate-role.png)

    Yönetici isteğinizi onaylamanız gerekli değilse, belirtilen kaynaklara erişimi olmalıdır.

## <a name="view-activity-for-a-guest"></a>Bir ziyaretçi için etkinlikleri görüntüle

Yalnızca bir üye kullanıcı gibi Konukları neler yaptığını izlemek için denetim günlüklerini görüntüleyebilirsiniz.

1. Bir yönetici olarak PIM açın ve paylaşılan kaynağı seçin.

1. Tıklayın **kaynak denetim** etkinlik söz konusu kaynak görüntüleme. Aşağıdaki örnek bir kaynak grubu için etkinliğin gösterir.

    ![Azure kaynakları - kaynak denetim sayfa süresi, istek sahibi ve eylem listeleme](./media/pim-resource-roles-external-users/audit-resource.png)

1. Etkinlik için konuk görüntülemek için tıklayın **Azure Active Directory** > **kullanıcılar** > Konuk adı.

1. Tıklayın **denetim günlükleri** kuruluş için denetim günlüklerini görmek için. Gerekirse, filtreler belirtebilirsiniz.

    ![Dizin denetim listesi tarih, tarafından başlatılan, hedef ve etkinlik günlükleri](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD PIM yönetici rolleri atama](pim-how-to-add-role-to-user.md)
- [Azure Active Directory B2B Konuk kullanıcı erişimini nedir?](../b2b/what-is-b2b.md)
