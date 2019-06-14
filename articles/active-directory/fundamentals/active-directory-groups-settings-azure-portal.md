---
title: Grup bilgileri - Azure Active Directory Düzenle | Microsoft Docs
description: Azure Active Directory kullanarak grubunuzun bilgilerini düzenleme hakkında yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 691f705574050b15869a0ac8b7d128507e5aae10
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60248806"
---
# <a name="edit-your-group-information-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak, grup bilgilerini Düzenle

Azure Active Directory (Azure AD) kullanarak, ad, açıklama ve üyelik türünü güncelleştirme dahil olmak üzere grubun ayarlarını düzenleyebilir.

## <a name="to-edit-your-group-settings"></a>Grup ayarlarınızı düzenlemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

    **Gruplar - tüm gruplar** sayfası görüntülenirse, etkin gruplarınızın tümü gösteriliyor.

3. Gelen **gruplar - tüm gruplar** sayfasında, grup adı kadar içine mümkün olduğunca **arama** kutusu. Bu makalenin amaçları ki aradığınız **MDM İlkesi - Batı** grubu.

    Arama sonuçları altında görüntülenir **arama** kutusunda daha fazla karakter türü olarak güncelleştiriliyor.

    ![Arama kutusuna arama metniyle tüm grupları sayfası](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Grup seçin **MDM İlkesi - Batı**ve ardından **özellikleri** gelen **Yönet** alan.

    ![Üye seçeneği ve vurgulanmış bilgi grubu genel bakış sayfası](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Güncelleştirme **genel ayarlar** dahil olmak üzere, gerektiği gibi bilgileri:

    ![Bir grup için özellikleri ayarlar](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Grup adı.** Var olan grup adını düzenleyin.
    
    - **Grup açıklaması.** Mevcut Grup açıklamasını düzenleyin.

    - **Grup türü.** Oluşturulduktan sonra bir grup türünü değiştiremezsiniz. Değiştirilecek **grup türü**, grubunu silin ve yeni bir tane oluşturmanız gerekir.
    
    - **Üyelik türü.** Üyelik türünü değiştirin. Çeşitli kullanılabilir üyeliği türleri hakkında daha fazla bilgi için bkz. [nasıl yapılır: Temel bir grup oluşturma ve Azure Active Directory portalı kullanarak üye ekleme](active-directory-groups-create-azure-portal.md).
    
    - **Nesne Kimliği** Nesne Kimliğini değiştiremezsiniz, ancak bu grup için PowerShell komutlarında kullanılacak kopyalayabilirsiniz. PowerShell cmdlet'leri kullanma hakkında daha fazla bilgi için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Ekleme veya gruptan üye kaldırma](active-directory-groups-members-azure-portal.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Bir grubun üyeliklerini yönetme](active-directory-groups-membership-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Azure Active Directory’ye bir Azure aboneliğini ekleme veya ilişkilendirme](active-directory-how-subscriptions-associated-directory.md)
