---
title: Azure Active Directory'yi kullanarak Grup bilgilerinizi düzenlemek nasıl | Microsoft Docs
description: Azure Active Directory kullanarak bir grup bilgilerini düzenlemeyi öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a02987fdce3a15cd5d416234e3717df6d33622ec
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731351"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>Nasıl yapılır: Azure Active Directory'yi kullanarak, grup bilgilerini Düzenle

Azure Active Directory'yi kullanarak, kendi ad, açıklama veya üyelik türü güncelleştirme dahil olmak üzere grubun ayarlarını düzenleyebilir.

## <a name="to-edit-your-group-settings"></a>Grup ayarlarınızı düzenlemek için
1. Oturum [Azure portalında](https://portal.azure.com) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

    **Gruplar - tüm gruplar** sayfası görüntülenirse, etkin gruplarınızın tümü gösteriliyor.

3. Gelen **gruplar - tüm gruplar** sayfasında, grup adı kadar içine mümkün olduğunca **arama** kutusu. Bu makalenin amaçları ki aradığınız **MDM İlkesi - Batı** grubu.

    Arama sonuçları altında görüntülenir **arama** kutusunda daha fazla karakter türü olarak güncelleştiriliyor.

    ![Arama kutusuna arama metniyle tüm grupları sayfası](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Grup seçin **MDM İlkesi - Batı**ve ardından **özellikleri** gelen **Yönet** alan.

    ![Sayı, üyeleri ve üye seçeneğinin vurgulandığı grubu genel bakış sayfası](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Güncelleştirme **genel ayarlar** dahil olmak üzere, gerektiği gibi bilgileri:

    ![Bir grup için özellikleri ayarlar](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Grup adı.** Var olan grup adını düzenleyin.
    
    - **Grup açıklaması.** Mevcut Grup açıklamasını düzenleyin.

    - **Grup türü.** Oluşturulduktan sonra bir grup türünü değiştiremezsiniz. Değiştirilecek **grup türü**, grubunu silin ve yeni bir tane oluşturmanız gerekir.
    
    - **Üyelik türü.** Üyelik türünü değiştirin. Çeşitli kullanılabilir üyeliği türleri hakkında daha fazla bilgi için bkz. [nasıl yapılır: temel bir grup oluşturma ve Azure Active Directory portalı kullanarak üye ekleme](active-directory-groups-create-azure-portal.md)
    
    - **Nesne Kimliği** Nesne Kimliğini değiştiremezsiniz, ancak bu grup için PowerShell komutlarında kullanılacak kopyalayabilirsiniz. PowerShell cmdlet'leri kullanma hakkında daha fazla bilgi için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Gruplar ve üyeler görüntüleyin](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Ekleme veya gruptan üye kaldırma](active-directory-groups-members-azure-portal.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Bir grubun üyeliklerini yönetme](active-directory-groups-membership-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Azure Active Directory'ye bir Azure aboneliği ekleme veya ilişkilendirme](active-directory-how-subscriptions-associated-directory.md)
