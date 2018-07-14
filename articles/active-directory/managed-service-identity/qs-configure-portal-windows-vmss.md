---
title: Azure portalını kullanarak bir Azure sanal makine ölçek üzerinde MSI'yı yapılandırma
description: Azure portalını kullanarak Azure VMSS üzerinde bir yönetilen hizmet kimliği (MSI) yapılandırma yönergeleri adım adım.
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
ms.openlocfilehash: 8779600f2c85a8bb309f7b2a8874608170de8877
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035250"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Sanal Makine Yapılandırma Yönetilen hizmet kimliği (MSI) Azure portalını kullanarak ölçek kümesi

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure portalını kullanarak nasıl etkinleştirileceği ve sistem tarafından atanan kimlik için bir sanal makine ölçek kümesi devre dışı öğreneceksiniz. Şu anda Azure portalı üzerinden atama ve kullanıcı tarafından atanan kimlikleri bir Azure sanal makine ölçek kümesinden kaldırılması desteklenmiyor.

> [!NOTE]
> Şu anda, kullanıcı tarafından atanan kimlik işlemleri desteklenmez Azure Portal aracılığıyla. Geri güncelleştirmeleri denetleyin.

## <a name="prerequisites"></a>Önkoşullar


- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturulması sırasında yönetilen hizmet kimliği

Şu anda, VM oluşturma Azure portal aracılığıyla yönetilen hizmet kimliği işlemlerini desteklemiyor. Bunun yerine, lütfen öncelikle bir Azure sanal makine ölçek kümesi oluşturmak için aşağıdaki Azure sanal makine ölçek kümesi oluşturma Hızlı başlangıcı makaleye başvurun:

- [Azure portalını kullanarak bir sanal makine ölçek kümesi oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)  

Ardından sanal makine ölçek kümesinde MSI etkinleştirme hakkında daha fazla ayrıntı için sonraki bölüme devam edin.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Mevcut bir Azure VMMS üzerinde yönetilen hizmet kimliğini etkinleştirme

Sistem tarafından atanan kimliği olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makine ölçek kümesine gidin.

3. VM üzerindeki sistem tarafından atanan kimliği "Yönetilen hizmet kimliği altında" etkinleştir "Evet"'i seçerek ve ardından **Kaydet**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   [![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Yönetilen hizmet kimliği bir Azure sanal makine ölçek kümesinden Kaldır

Artık bir MSI gereken sanal makine ölçek kümesi varsa:

1. Oturum [Azure portalında](https://portal.azure.com) sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun sanal makine ölçek kümesi üzerinde yazma izinleri.

2. İstenen sanal makine ölçek kümesine gidin.

3. "Yönetilen hizmet kimliği" altında "Hayır" seçerek atanan sanal makine kimliği sistemi devre dışı bırakın, sonra Kaydet'e tıklayın. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>İlgili İçerik

- Yönetilen hizmet kimliği genel bakış için bkz. [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- MSI verin Azure portalını kullanarak bir Azure sanal makine ölçek kümesi [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
