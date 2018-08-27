---
title: Azure portalını kullanarak bir Azure sanal makine ölçek üzerinde yönetilen hizmet kimliği yapılandırma
description: Azure portalını kullanarak Azure VMSS üzerinde bir yönetilen hizmet kimliği yapılandırmaya ilişkin yönergeleri adım adım.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: cbe2e3d9f60ced5c707ce5a701a5aac937ccc072
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887998"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Sanal Makine Yapılandırma Yönetilen hizmet kimliği Azure portalını kullanarak ölçek kümesi

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, sistem ve kullanıcı tarafından atanan kimliği için Azure portalını kullanarak bir sanal makine ölçek kümesi devre öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md).
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol ataması hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) etkinleştirmek ve sistem tarafından yönetilen kimlik atanan bir sanal makine ölçek kümesinden kaldırmak için.

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik 

Bu bölümde, etkinleştirme ve devre dışı sistem tarafından atanan kimliği Azure portalını kullanarak bir sanal makine ölçek kümesi üzerinde öğreneceksiniz.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan kimlik etkinleştir

Şu anda Azure portalında sanal makine ölçek kümesi oluşturma sırasında atanan kimliği etkinleştirme sistem desteklemez. Bunun yerine, önce bir sanal makine ölçek kümesi oluşturun ve sistem tarafından atanan bir sanal makine ölçek kümesi kimliğini etkinleştirme hakkında daha fazla ayrıntı için sonraki bölüme devam etmek için aşağıdaki sanal makine ölçek kümesi oluşturma Hızlı Başlangıç makalesi bakın:

- [Azure portalını kullanarak bir sanal makine ölçek kümesi oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-identity-on-an-existing-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği var olan bir sanal makine ölçek kümesi üzerinde etkinleştir

Sistem tarafından atanan kimliği olmadan ilk olarak sağlanan bir sanal makine ölçek kümesi üzerinde etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makine ölçek kümesine gidin.

3. Altında **sistem tarafından atanan**, **durumu**seçin **üzerinde** ve ardından **Kaydet**:

   [![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

### <a name="remove-system-assigned-identity-from-a-virtual-machine-scale-set"></a>Sistem tarafından atanan kimlik bir sanal makine ölçek kümesinden Kaldır

Bir sanal makine ölçek kümesi artık varsa bir sistem tarafından atanan kimlik gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun sanal makine ölçek kümesi üzerinde yazma izinleri.

2. İstenen sanal makine ölçek kümesine gidin.

3. Altında **sistem tarafından atanan**, **durumu**seçin **kapalı** ve ardından **Kaydet**:

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure portalını kullanarak bir sanal makine ölçek kümesinden ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında bir kullanıcıya atanan kimlik atama

Şu anda Azure portalında bir kullanıcı tarafından atanan kimliği bir sanal makine ölçek kümesi oluşturma sırasında atama desteklemez. Bunun yerine, önce bir sanal makine ölçek kümesi oluşturun ve bir kullanıcı tarafından atanan kimliği atama ile ilgili ayrıntılar için sonraki bölüme geçmek için aşağıdaki sanal makine ölçek kümesi oluşturma Hızlı Başlangıç makalesi bakın:

- [Azure portalını kullanarak bir sanal makine ölçek kümesi oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-identity-to-an-existing-virtual-machine-scale-set"></a>Bir kullanıcı tarafından atanan kimliği mevcut bir sanal makine ölçek kümesine atama

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen sanal makine ölçek kümesine gelin ve tıklayın **kimlik**, **kullanıcıya atanan** ardından  **\+Ekle**.

   ![VMSS için kullanıcı tarafından atanan kimliği Ekle](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Sanal makine ölçek kümesine ekleyin ve ardından istediğiniz kullanıcı tarafından atanan kimliği tıklatın **Ekle**.
   
   ![VMSS için kullanıcı tarafından atanan kimliği Ekle](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Bir kullanıcı tarafından atanan kimliği bir sanal makine ölçek kümesinden Kaldır

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.
2. İstenen sanal makine ölçek kümesine gelin ve tıklayın **kimlik**, **kullanıcıya atanan**, silin ve ardından istediğiniz kullanıcı tarafından atanan kimlik adı **Kaldır** () tıklayın **Evet** onay bölmesinde).

   ![Kullanıcı tarafından atanan kimliği bir VMSS kaldırın](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="related-content"></a>İlgili İçerik

- Yönetilen hizmet kimliği genel bakış için bkz. [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Verin Azure portalını kullanarak bir Azure sanal makine ölçek kümesi yönetilen hizmet kimliği [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
