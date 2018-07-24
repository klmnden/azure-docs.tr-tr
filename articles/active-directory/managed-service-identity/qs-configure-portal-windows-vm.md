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
ms.openlocfilehash: 6ba090065b18a44cc1f01a62eefb5dcf52bcf356
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213274"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Azure portalını kullanarak VM yönetilen hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure portalını kullanarak Azure VM için atanan kimliği sistemi devre öğreneceksiniz. Şu anda Azure Portal aracılığıyla atama ve kullanıcı tarafından atanan kimliklerle Azure Vm'lerinden kaldırma desteklenmiyor.

> [!NOTE]
> Şu anda, kullanıcı tarafından atanan kimlik işlemleri desteklenmez Azure Portal aracılığıyla. Güncelleştirmeler için sonra yeniden denetleyin. 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md).
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol ataması hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) etkinleştirmek ve sistem tarafından atanan kimlik bir Azure VM'den kaldırın.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında yönetilen hizmet kimliği

Şu anda, VM oluşturma Azure portal aracılığıyla yönetilen hizmet kimliği işlemlerini desteklemiyor. Bunun yerine, lütfen öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Ardından sanal makine üzerindeki yönetilen hizmet kimliği etkinleştiriliyor ilişkin ayrıntılar için sonraki bölüme devam edin.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Yönetilen hizmet kimliği mevcut bir Azure sanal makinesinde etkinleştirin

Sistem tarafından atanan kimliği olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak.

2. İstenen sanal makineye gidin ve "Yapılandırma" sayfasını seçin.

3. VM üzerindeki sistem tarafından atanan kimliği "Yönetilen hizmet kimliği altında" etkinleştir "Evet"'i seçerek ve ardından **Kaydet**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   > [!NOTE]
   > Bir VM için bir kullanıcı tarafından atanan kimliği ekleme, Azure Portal aracılığıyla şu anda desteklenmiyor.

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Yönetilen hizmet kimliğini bir Azure VM'den kaldırın.

Bir sanal makine varsa, artık sistem tarafından atanan kimlik gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. 

2. İstenen sanal makineye gidin ve "Yapılandırma" sayfasını seçin.

3. "Yönetilen hizmet kimliği" altında "Hayır" seçerek atanan sanal makine kimliği sistemi devre dışı bırakın, sonra Kaydet'e tıklayın. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

    > [!NOTE]
    > Bir VM için bir kullanıcı tarafından atanan kimliği ekleme, Azure Portal aracılığıyla şu anda desteklenmiyor.

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>İlgili içerik

- Yönetilen hizmet kimliği genel bakış için bkz. [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak bir Azure VM yönetilen hizmet kimliği vermek [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

