---
title: Grup sahipleri - Azure Active Directory Ekle Kaldır | Microsoft Docs
description: Azure Active Directory'yi kullanarak sahipleri ekleme veya kaldırma hakkında yönergeler grubu.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd684e1bd48f877a74280b33b4df65d7baaa0fe7
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507182"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Ekleme veya Azure Active Directory'de Grup sahipleri kaldırma
Azure Active Directory (Azure AD) gruplarına ait ve Grup sahipleri tarafından yönetilir. Grup sahipleri, kullanıcı veya hizmet sorumluları olabilir ve grubun üyeliği de dahil olmak üzere yönetebilir. Yalnızca var olan Grup sahipleri veya grup yöneten yöneticiler grubu onwers atayabilirsiniz. Grup sahipleri güvenlik grubunun üyesi olması gerekmez.

Bir grup sahibi yok, Grup Yönetimi grubunu yönetmek hala mümkün yöneticilerdir.

## <a name="add-an-owner-to-a-group"></a>Gruba sahip ekleme
Aşağıda bir gruba sahip olarak kullanıcı ekleme yönergeleri, Azure AD portalı kullanıyorsunuz. Bir grubun sahibi bir hizmet sorumlusu ekleme için kullanarak bunu yapmak için yönergeleri izleyin [PowerShell](https://docs.microsoft.com/powershell/module/Azuread/Add-AzureADGroupOwner?view=azureadps-2.0).

### <a name="to-add-a-group-owner"></a>Bir grup sahibi eklemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. Seçin **Azure Active Directory**seçin **grupları**, ardından sahip eklemek istediğiniz grubu seçin (Bu örnekte, *MDM İlkesi - Batı*).

3. Üzerinde **MDM İlkesi - Batı genel bakış** sayfasında **sahipleri**.

    ![MDM İlkesi - sahipleri seçeneğinin vurgulandığı Batı genel bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. Üzerinde **MDM İlkesi - Batı - sahipleri** sayfasında **sahipler eklemeyi**, yeni Grup sahibi olmanız ve ardından kullanıcıyı seçin ve ardından aramak **seçin**.

    ![MDM İlkesi - Batı - sahipler sayfası ile sahipleri seçeneği vurgulanmış olarak Ekle](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Yeni sahibi seçin sonra yenileyebilirsiniz **sahipleri** sayfasında ve sahipleri listesine eklenen adı görür.

## <a name="remove-an-owner-from-a-group"></a>Sahibi gruptan kaldırma
Sahibi Azure AD kullanarak bir gruptan kaldırın.

### <a name="to-remove-an-owner"></a>Sahibi kaldırmak için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. Seçin **Azure Active Directory**seçin **grupları**, ardından bir sahibi kaldırmak istediğiniz grubu seçin (Bu örnekte, *MDM İlkesi - Batı*).

3. Üzerinde **MDM İlkesi - Batı genel bakış** sayfasında **sahipleri**.

    ![MDM İlkesi - sahipleri seçeneğinin vurgulandığı Batı genel bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. Üzerinde **MDM İlkesi - Batı - sahipleri** sayfasında, bir grup sahibi kaldırmak için seçmek istediğiniz kullanıcıyı seçin **Kaldır** kullanıcı bilgileri sayfası ve select **Evet** onaylamak için siz karar verirsiniz.

    ![Kaldır seçeneğinin vurgulandığı kullanıcı bilgi sayfası](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Sahibi kaldırdıktan sonra dönebilirsiniz **sahipleri** sayfasında ve ad sahipleri listesinden kaldırıldı bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md)

- [Tümleşik bir SaaS uygulamasına erişim atamak için grupları kullanma](../users-groups-roles/groups-saasapps.md)

- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)

- [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-v2-cmdlets.md)
