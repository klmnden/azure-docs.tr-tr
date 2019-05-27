---
title: Yapılandırma kimlikleri, Azure portalını kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen
description: Adım adım yapılandırma yönergeleri kimlikleri Azure portalını kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/10/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: ab0a5b021048f0f684473d3f54bbeadf870cd007
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66112823"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Azure kaynakları için yönetilen kimlikleri Azure portalını kullanarak bir VM yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure sanal makine (Azure portalı kullanarak VM için), sistem ve kullanıcı tarafından atanan yönetilen kimlikler devre öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure portalını kullanarak sanal makine için sistem tarafından atanan bir yönetilen kimlik devre öğrenin.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Yönetilen kimlik sistem tarafından atanan bir VM oluşturma sırasında etkinleştir

Yönetilen kimlik sistem tarafından atanan bir VM'de kendi oluşturma sırasında etkinleştirmek için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

- Altında **Yönetim** sekmesinde **kimlik** bölümünde, geçiş **yönetilen hizmet kimliği** için **üzerinde**.  

![VM oluşturma sırasında sistem tarafından atanan kimlik etkinleştir](./media/msi-qs-configure-portal-windows-vm/enable-system-assigned-identity-vm-creation.png)

Bir VM oluşturmak için şu hızlı Başlangıçlarda için bakın: 

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) 
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)


### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Mevcut VM'yi yönetilen kimlik sistem tarafından atanan etkinleştir

Sistem tarafından atanan yönetilen kimlik olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makineye gidin ve seçin **kimlik**.

3. Altında **sistem tarafından atanan**, **durumu**seçin **üzerinde** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Yönetilen kimlik sistem tarafından atanan bir sanal makineden kaldırın

Yönetilen kimlik sistem tarafından atanan bir sanal makineden kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

Bir sanal makine varsa, artık yönetilen kimlik sistem tarafından atanan gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. 

2. İstenen sanal makineye gidin ve seçin **kimlik**.

3. Altında **sistem tarafından atanan**, **durumu**seçin **kapalı** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

 Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure portalını kullanarak bir sanal makineden ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Bir VM oluşturma sırasında bir kullanıcı tarafından atanan kimliği atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

Şu anda Azure portalında bir VM oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama desteklemez. Bunun yerine, öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın ve ardından VM için bir kullanıcı tarafından atanan bir yönetilen kimlik atama hakkında ayrıntılar için sonraki bölüme devam edin:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir VM'ye atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen VM tıklatın gidip **kimlik**, **kullanıcıya atanan** ardından  **\+Ekle**.

   ![VM'ye kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. VM'ye ekleyin ve ardından istediğiniz kullanıcı tarafından atanan kimliği tıklatın **Ekle**.

    ![VM'ye kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir sanal makineden kaldırın

Bir kullanıcı tarafından atanan kimliği bir sanal makineden kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen VM tıklatın gidip **kimlik**, **atanan kullanıcı**, kullanıcı tarafından atanan adı yönetilen silin ve ardından istediğiniz kimliği **Kaldır** ( tıklayın **Evet** onay bölmesinde).

   ![Yönetilen kimlik kullanıcı tarafından atanan bir sanal makineden kaldırın](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak, bir Azure sanal makinenin yönetilen kimlik vermek [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

