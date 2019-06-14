---
title: Atamayı veya kaldırmayı lisanslar - Azure Active Directory | Microsoft Docs
description: Atama veya Azure Active Directory lisansları, kullanıcıları veya grupları kaldırma hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e7a3f80067adb3093bd27e34a45b3afd72b4993
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60247594"
---
# <a name="assign-or-remove-licenses-using-the-azure-active-directory-portal"></a>Atayın veya Azure Active Directory portalı kullanarak lisansları kaldırın
Birçok Azure Active Directory (Azure AD) Hizmetleri, Azure AD ürünü etkinleştirmek için ve her biri, kullanıcılar veya gruplar (ve ilişkili üyeler) lisans bu ürün için gerektirir. Etkin bir lisansa sahip kullanıcılar erişebilir ve lisanslı kullanma yalnızca Azure AD Hizmetleri.

## <a name="available-product-editions"></a>Kullanılabilir ürün sürümleri
Azure AD ürünü için kullanılabilen çeşitli sürümleri vardır.

- Azure AD Ücretsiz

- Azure AD Basic

- Azure AD Premium 1 (Azure AD P1)

- Azure AD Premium 2 (Azure AD P2)

Her ürün sürümünü ve ilişkili lisans ayrıntıları hakkında ayrıntılı bilgi için bkz. [lisans ne yapmam gerekir mi?](../authentication/concept-sspr-licensing.md).

## <a name="view-your-product-edition-and-license-details"></a>Ürün sürümü ve lisans bilgilerinizi görüntüleyin
Bağımsız lisans dahil olmak üzere ürünlerinizi kullanılabilir tüm bekleyen sona erme tarihleri ve kullanılabilir atamaları sayısı denetleniyor görüntüleyebilirsiniz.

### <a name="to-find-your-product-and-license-details"></a>Ürün ve lisans bilgilerinizi bulmak için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. Seçin **Azure Active Directory**ve ardından **lisansları**.

    **Lisansları** sayfası görüntülenir.

    ![Satın alınan ürünleri ve atanmış lisans sayısı lisans sayfası](media/license-users-groups/license-details-blade.png)
    
3. Seçin **satın alınan ürünlerle** bağlantısına **ürünleri** sayfası ve **atanan**, **kullanılabilir**, ve  **Yakında sona erecek** her belirli bir ürün sürümü için ayrıntıları.

    ![Ürün sürümleri ve ilişkili lisans bilgilerini içeren ürün sayfası](media/license-users-groups/license-products-blade-with-products.png)

4. Kendi lisanslı kullanıcılar ve grupları görmek için bir ürün sürümü adını seçin.

## <a name="assign-licenses-to-users-or-groups"></a>Kullanıcılara veya gruplara lisans atama
Emin olun, herhangi bir lisanslı kullanmaya gerek Azure AD hizmeti uygun lisansa sahip. Bu, lisans haklarını bireysel kullanıcılar veya tüm bir gruba eklemek istediğiniz aittir.

>[!Note]
>Azure ad genel önizleme özelliği olan Grup tabanlı Lisanslama ve tüm kullanılabilir Ücretli Azure AD lisans planınız. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).<br><br>Kullanıcı ekleme hakkında ayrıntılı bilgi için bkz: [ekleyin veya Azure Active Directory'de kullanıcı silme](add-users-azure-active-directory.md). Grupları oluşturma ve üye ekleme hakkında ayrıntılı bilgi için bkz. [temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

### <a name="to-assign-a-license-to-a-specific-user"></a>Belirli bir kullanıcıya lisans atamak için
1. Üzerinde **ürünleri** sayfasında, kullanıcıya atamak istediğiniz sürüm adını belirleyin. Örneğin, _Azure Active Directory Premium Plan 2_.

    ![Vurgulanan ürün sürümü ile ürün sayfası](media/license-users-groups/license-products-blade-with-product-highlight.png)

2. Üzerinde **Azure Active Directory Premium Plan 2** sayfasında **atama**.

    ![Vurgulanan Ata seçeneğiyle ürün sayfası](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

3. Üzerinde **atama** sayfasında **kullanıcılar ve gruplar**, arayın ve lisans atama kullanıcı seçin. Örneğin, _Mary Parker_.

    ![Lisans sayfasında, vurgulanan arama ve seçim seçenekleri ile atayın](media/license-users-groups/assign-license-blade-with-highlight.png)

4. Seçin **atama seçenekleri**, lisans seçenekleri açık ve ardından uygun olduğundan emin olun **Tamam**.

    ![Lisans seçeneği sayfasında tüm seçenekleri Edition'da kullanılabilir](media/license-users-groups/license-option-blade-assignments.png)

    **Ata lisans** sayfasında bir kullanıcı seçili olduğunu ve atamaları yapılandırıldığını göstermek için güncelleştirmeler.

    >[!NOTE]
    >Tüm Microsoft Hizmetleri, tüm konumlardaki kullanılabilir. Bir kullanıcıya lisans atanabilmesi için önce belirtmelisiniz **kullanım konumu**. Bu değer ayarlayabilirsiniz **Azure Active Directory &gt; kullanıcılar &gt; profili &gt; ayarları** Azure AD alanı.

5. **Ata**'yı seçin.

    Kullanıcı lisanslı kullanıcılar listesine eklenir ve erişimi dahil olan Azure AD Hizmetleri.

### <a name="to-assign-a-license-to-an-entire-group"></a>Grubun tamamı için bir lisans atamak için
1. Üzerinde **ürünleri** sayfasında, kullanıcıya atamak istediğiniz sürüm adını belirleyin. Örneğin, _Azure Active Directory Premium Plan 2_.

    ![Vurgulanan ürün sürümü ile ürünleri dikey penceresi](media/license-users-groups/license-products-blade-with-product-highlight.png)

2. Üzerinde **Azure Active Directory Premium Plan 2** sayfasında **atama**.

    ![Vurgulanan Ata seçeneğiyle ürün sayfası](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

3. Üzerinde **atama** sayfasında **kullanıcılar ve gruplar**, arayın ve lisans atama bir grubu seçin. Örneğin, _MDM İlkesi - Batı_.

    ![Lisans sayfasında, vurgulanan arama ve seçim seçenekleri ile atayın](media/license-users-groups/assign-group-license-blade-with-highlight.png)

4. Seçin **atama seçenekleri**, lisans seçenekleri açık ve ardından uygun olduğundan emin olun **Tamam**.

    ![Lisans seçeneği sayfasında tüm seçenekleri Edition'da kullanılabilir](media/license-users-groups/license-option-blade-group-assignments.png)

    **Ata lisans** sayfasında bir kullanıcı seçili olduğunu ve atamaları yapılandırıldığını göstermek için güncelleştirmeler.

    >[!NOTE]
    >Tüm Microsoft Hizmetleri, tüm konumlardaki kullanılabilir. Bir gruba lisans atanabilmesi için önce belirtmelisiniz **kullanım konumu** tüm üyeleri için. Bu değer ayarlayabilirsiniz **Azure Active Directory &gt; kullanıcılar &gt; profili &gt; ayarları** Azure AD alanı. Kullanım konumu belirtilmezse herhangi bir kullanıcı, Kiracı konumunu alır.

5. **Ata**'yı seçin.

    Grubu lisanslı gruplar listeye eklenir ve tüm üyelerin dahil erişiminiz Azure AD Hizmetleri.

## <a name="remove-a-license"></a>Lisansı kaldırma
Bir kullanıcı ya da bir gruptan lisans kaldırmaya **lisansları** sayfası.

### <a name="to-remove-a-license-from-a-specific-user"></a>Belirli bir kullanıcı tarafından bir lisans kaldırmak için
1. Üzerinde **lisanslı kullanıcılar** sayfasında ürün sürümü için artık lisans olması gereken kullanıcıyı seçin. Örneğin, _Alain Charon_.

2. Seçin **Kaldır lisans**.

    ![Lisansı Kaldır seçeneğinin vurgulandığı lisanslı kullanıcılar sayfası](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

### <a name="to-remove-a-license-from-a-group"></a>Lisans bir gruptan kaldırmak için
1. Üzerinde **lisanslı gruplar** sayfasında ürün sürümü için artık lisans olmalıdır bir grup seçin. Örneğin, _MDM İlkesi - Batı_.

2. Seçin **Kaldır lisans**.

    ![Lisanslı gruplar sayfasıyla lisans seçeneği vurgulanmış olarak Kaldır](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)

>[!Important]
>Lisans bir gruptan bir kullanıcı tarafından devralınan doğrudan kaldırılamaz. Bunun yerine, kullanıcı, bunlar lisans devralan gruptan kaldırmak zorunda.

## <a name="next-steps"></a>Sonraki adımlar
Lisanslarınızı atadıktan sonra aşağıdaki işlemleri gerçekleştirebilirsiniz:

- [Lisans ataması sorunlarını tanımlama ve çözme](../users-groups-roles/licensing-groups-resolve-problems.md)

- [Lisanslı kullanıcılar için lisans gruba ekleme](../users-groups-roles/licensing-groups-migrate-users.md)

- [Senaryoları, sınırlamalar ve bilinen sorunlar Azure Active Directory'de lisanslama yönetmek için grupları kullanma](../users-groups-roles/licensing-group-advanced.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)
