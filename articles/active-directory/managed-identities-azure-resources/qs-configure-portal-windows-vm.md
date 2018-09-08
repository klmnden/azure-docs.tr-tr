---
title: Yapılandırma kimlikleri, Azure portalını kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen
description: Adım adım yapılandırma yönergeleri kimlikleri Azure portalını kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: d1294f0e500bd3403e02fbfd6845629ff0929ee0
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158867"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Azure kaynakları için yönetilen kimlikleri Azure portalını kullanarak bir VM yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure sanal makine (Azure portalı kullanarak VM için), sistem ve kullanıcı tarafından atanan yönetilen kimlikler devre öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol ataması hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) etkinleştirip yönetilen kimlik sistem tarafından atanan bir Azure VM'den kaldırın.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure portalını kullanarak sanal makine için sistem tarafından atanan bir yönetilen kimlik devre öğrenin.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Yönetilen kimlik sistem tarafından atanan bir VM oluşturma sırasında etkinleştir

Şu anda Azure portalında bir VM oluşturma sırasında sistem tarafından atanan kimliği etkinleştirme desteklemez. Bunun yerine, öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın ve ardından sanal makine üzerindeki sistem tarafından atanan kimliği etkinleştirme ile ilgili ayrıntılar için sonraki bölüme devam edin:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Mevcut VM'yi yönetilen kimlik sistem tarafından atanan etkinleştir

İlk olarak bu olmadan sağlanan bir VM üzerindeki sistem tarafından atanan yönetilen kimlik etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makineye gidin ve seçin **kimlik**.

3. Altında **sistem tarafından atanan**, **durumu**seçin **üzerinde** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Yönetilen kimlik sistem tarafından atanan bir sanal makineden kaldırın

Bir sanal makine varsa, artık yönetilen kimlik sistem tarafından atanan gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. 

2. İstenen sanal makineye gidin ve seçin **kimlik**.

3. Altında **sistem tarafından atanan**, **durumu**seçin **kapalı** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

 Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure portalını kullanarak bir sanal makineden ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Bir VM oluşturma sırasında bir kullanıcı tarafından atanan kimliği atayın

Şu anda Azure portalında bir VM oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama desteklemez. Bunun yerine, öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın ve ardından VM için bir kullanıcı tarafından atanan bir yönetilen kimlik atama hakkında ayrıntılar için sonraki bölüme devam edin:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir VM'ye atayın

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen VM tıklatın gidip **kimlik**, **kullanıcıya atanan** ardından  **\+Ekle**.

   ![VM'ye kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. VM'ye ekleyin ve ardından istediğiniz kullanıcı tarafından atanan kimliği tıklatın **Ekle**.

    ![VM'ye kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir sanal makineden kaldırın

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen VM tıklatın gidip **kimlik**, **atanan kullanıcı**, kullanıcı tarafından atanan adı yönetilen silin ve ardından istediğiniz kimliği **Kaldır** ( tıklayın **Evet** onay bölmesinde).

   ![Yönetilen kimlik kullanıcı tarafından atanan bir sanal makineden kaldırın](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak, bir Azure sanal makinenin yönetilen kimlik vermek [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

