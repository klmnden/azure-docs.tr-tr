---
title: Azure portalını kullanarak bir Azure VM yönetilen hizmet kimliği yapılandırma
description: Azure portalını kullanarak bir Azure sanal makinesinde bir yönetilen hizmet kimliği yapılandırmaya ilişkin yönergeleri adım adım.
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
ms.openlocfilehash: 552fce2ffd8b6bd786010da82e702ee98c3f8647
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888565"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Azure portalını kullanarak VM yönetilen hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve sistem ve kullanıcı tarafından atanan kimliği bir Azure sanal makinesi (Azure portalı kullanarak VM için), devre dışı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md).
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol ataması hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) etkinleştirmek ve sistem tarafından atanan kimlik bir Azure VM'den kaldırın.

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, Azure portalını kullanarak sanal makine için atanan kimliği sistemi devre öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-a-vm"></a>Bir VM oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

Şu anda Azure portalında bir VM oluşturma sırasında etkinleştirme sistem tarafından atanan kimlik desteklemez. Bunun yerine, öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın ve ardından sanal makine üzerindeki sistem tarafından atanan kimliği etkinleştirme ile ilgili ayrıntılar için sonraki bölüme devam edin:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-identity-on-an-existing-vm"></a>Mevcut VM'yi kimlik atanan Sistemi'ni etkinleştir

Sistem tarafından atanan kimliği olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makineye gidin ve seçin **kimlik**.

3. Altında **sistem tarafından atanan**, **durumu**seçin **üzerinde** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-identity-from-a-vm"></a>Sistemini atanan kimliği bir VM'den kaldırın

Bir sanal makine varsa, artık sistem tarafından atanan kimlik gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. 

2. İstenen sanal makineye gidin ve seçin **kimlik**.

3. Altında **sistem tarafından atanan**, **durumu**seçin **kapalı** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

 Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure portalını kullanarak bir sanal makineden ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Bir kullanıcı tarafından atanan kimliği bir VM oluşturma sırasında atama

Şu anda Azure portalında bir kullanıcı tarafından atanan kimliği bir VM oluşturma sırasında atama desteklemez. Bunun yerine, öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın ve ardından bir kullanıcı tarafından VM'ye atanan kimliği atama hakkında ayrıntılar için sonraki bölüme devam edin:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-identity-to-an-existing-vm"></a>Bir kullanıcı tarafından atanan kimliği mevcut bir VM'ye atayın

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen VM tıklatın gidip **kimlik**, **kullanıcıya atanan** ardından  **\+Ekle**.

   ![VM'ye atanmış kullanıcı kimliği ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. VM'ye ekleyin ve ardından istediğiniz kullanıcı tarafından atanan kimliği tıklatın **Ekle**.

    ![VM'ye atanmış kullanıcı kimliği ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-vm"></a>Bir kullanıcı tarafından atanan kimliği bir VM'den kaldırın

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen VM tıklatın gidip **kimlik**, **kullanıcıya atanan**, silin ve ardından istediğiniz kullanıcı tarafından atanan kimlik adı **Kaldır** ( tıklayın**Evet** onay bölmesinde).

   ![Kullanıcı tarafından atanan kimliği bir VM'den kaldırın](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="related-content"></a>İlgili içerik

- Yönetilen hizmet kimliği genel bakış için bkz. [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak bir Azure VM yönetilen hizmet kimliği vermek [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

