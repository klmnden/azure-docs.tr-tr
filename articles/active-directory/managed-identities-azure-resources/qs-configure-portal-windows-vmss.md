---
title: Yapılandırma kimlikleri, bir sanal makine ölçek kümesinde Azure kaynakları için yönetilen
description: Adım adım yapılandırma yönergeleri için Azure portalını kullanarak bir sanal makine ölçek kümesi Azure kaynaklarında kimlik yönetilen.
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
ms.date: 02/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57f0ec91bd5c72b593d9b28f7d47f691181a6a0f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60290660"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-the-azure-portal"></a>Azure kaynakları için yönetilen kimlikleri, Azure portalını kullanarak bir sanal makine ölçek üzerinde yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak, Azure kaynaklarını bir sanal makine ölçek kümesi işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğreneceksiniz:

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki Azure rol tabanlı erişim denetimi atamalarını hesabınızın gerekir:

    > [!NOTE]
    > Hiçbir ek Azure AD dizini rol atamaları gerekli.

    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) etkinleştirmek ve yönetilen kimlik sistem tarafından atanan bir sanal makine ölçek kümesinden kaldırmak için.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure portalını kullanarak sistem tarafından atanan yönetilen kimlik devre öğreneceksiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Yönetilen kimlik sistem tarafından atanan bir sanal makine ölçek kümesi oluşturma sırasında etkinleştir

Şu anda Azure portalında sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştirme desteklemez. Bunun yerine, aşağıdaki sanal makine ölçek kümesi oluşturma işlemi için öncelikle bir sanal makine ölçek kümesi oluşturma ve yönetilen kimlik sistem tarafından atanan bir sanal makine ölçek kümesinde etkinleştirme hakkında daha fazla bilgi için sonraki bölüme devam etmek için hızlı başlangıç makalesi bakın:

- [Azure portalını kullanarak bir sanal makine ölçek kümesi oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği var olan bir sanal makine ölçek kümesi üzerinde yönetilen etkinleştir

İlk olarak bu olmadan sağlanan sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimlik etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makine ölçek kümesine gidin.

3. Altında **sistem tarafından atanan**, **durumu**seçin **üzerinde** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png) 

### <a name="remove-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Yönetilen kimlik sistem tarafından atanan bir sanal makine ölçek kümesinden Kaldır

Artık sistem tarafından atanan bir yönetilen kimlik gereken sanal makine ölçek kümesi varsa:

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun sanal makine ölçek kümesi üzerinde yazma izinleri.

2. İstenen sanal makine ölçek kümesine gidin.

3. Altında **sistem tarafından atanan**, **durumu**seçin **kapalı** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure portalını kullanarak bir sanal makine ölçek kümesinden ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

Şu anda Azure portalında sanal makine ölçek kümesi oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama desteklemez. Bunun yerine, aşağıdaki sanal makine ölçek kümesi oluşturma işlemi için öncelikle bir sanal makine ölçek kümesi oluşturma ve kullanıcı tarafından atanan bir yönetilen kimlik için atama ile ilgili ayrıntılar için sonraki bölüme geçmek için hızlı başlangıç makalesi bakın:

- [Azure portalını kullanarak bir sanal makine ölçek kümesi oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan bir yönetilen kimlik var olan bir sanal makine ölçek kümesine atama

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen sanal makine ölçek kümesine gelin ve tıklayın **kimlik**, **kullanıcıya atanan** ardından  **\+Ekle**.

   ![VMSS için kullanıcı tarafından atanan kimliği Ekle](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Sanal makine ölçek kümesine ekleyin ve ardından istediğiniz kullanıcı tarafından atanan kimliği tıklatın **Ekle**.
   
   ![VMSS için kullanıcı tarafından atanan kimliği Ekle](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesinden bir kullanıcı tarafından atanan bir yönetilen kimlik Kaldır

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen sanal makine ölçek kümesine gelin ve tıklayın **kimlik**, **kullanıcıya atanan**, kullanıcı tarafından atanan adı yönetilen silin ve ardından istediğiniz kimliği **Kaldır** (tıklayın **Evet** onay bölmesinde).

   ![Bir VMSS kimlikten kullanıcı tarafından atanan Kaldır](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="next-steps"></a>Sonraki adımlar

- Verin Azure portalını kullanarak bir Azure sanal makine ölçek kümesi yönetilen kimlik [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).


