---
title: Ekleme veya Azure Active Directory Grup sahipleri Kaldır | Microsoft Docs
description: Azure Active Directory'yi kullanarak Grup sahipleri ekleyip öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: f546ea5b5f9288849334d27cd1721f0c22fb8806
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46297784"
---
# <a name="how-to-add-or-remove-group-owners-in-azure-active-directory"></a>Nasıl yapılır: ekleme veya Azure Active Directory'de Grup sahipleri kaldırma
Azure Active Directory (Azure AD) gruplarına ait ve Grup sahipleri tarafından yönetilir. Grup sahipleri bir grup ve üyelerini yönetmek için bir kaynak sahibi (Yönetici) atanır. Grup sahipleri güvenlik grubunun üyesi olması gerekmez. Bir grup sahibi atandıktan sonra yalnızca bir kaynak sahibi ekleyebilir veya sahipleri kaldırabilirsiniz.

Bazı durumlarda, yönetici olarak bir grup sahibi atamamayı karar verebilirsiniz. Bu durumda, Grup sahibi olur. Ayrıca, bu grubu ayarlarında kısıtlı sürece sahipleri için Grup, diğer sahipleri atayabilirsiniz.

## <a name="add-an-owner-to-a-group"></a>Gruba sahip ekleme
Ek Grup sahipleri, Azure AD kullanarak bir gruba ekleyin.

### <a name="to-add-a-group-owner"></a>Bir grup sahibi eklemek için
1. Oturum [Azure portalında](https://portal.azure.com) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**seçin **grupları**, ardından sahip eklemek istediğiniz grubu seçin (Bu örnekte, _MDM İlkesi - Batı_).

3. Üzerinde **MDM İlkesi - Batı genel bakış** sayfasında **sahipleri**.

    ![MDM İlkesi - sahipleri seçeneğinin vurgulandığı Batı genel bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. Üzerinde **MDM İlkesi - Batı - sahipleri** sayfasında **sahipler eklemeyi**, yeni Grup sahibi olmanız ve ardından kullanıcıyı seçin ve ardından aramak **seçin**.

    ![MDM İlkesi - Batı - sahipler sayfası ile sahipleri seçeneği vurgulanmış olarak Ekle](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Yeni sahibi seçin sonra yenileyebilirsiniz **sahipleri** sayfasında ve sahipleri listesine eklenen adı görür.

## <a name="remove-an-owner-from-a-group"></a>Sahibi gruptan kaldırma
Sahibi Azure AD kullanarak bir gruptan kaldırın.

### <a name="to-remove-an-owner"></a>Sahibi kaldırmak için
1. Oturum [Azure portalında](https://portal.azure.com) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**seçin **grupları**, ardından sahip eklemek istediğiniz grubu seçin (Bu örnekte, _MDM İlkesi - Batı_).

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
