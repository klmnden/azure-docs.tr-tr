---
title: Azure portalını kullanarak bir Azure sanal makinesinde MSI yapılandırma
description: Azure Azure portalını kullanarak bir VM'de, bir yönetilen hizmet kimliği (MSI) yapılandırma yönergeleri adım adım.
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
ms.openlocfilehash: 27ecb00bddb41ae45e790a54702c058ff3f1d24b
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035950"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure portalını kullanarak Azure VM için atanan kimliği sistemi devre öğreneceksiniz. Şu anda Azure Portal aracılığıyla atama ve kullanıcı tarafından atanan kimliklerle Azure Vm'lerinden kaldırma desteklenmiyor.

> [!NOTE]
> Şu anda, kullanıcı tarafından atanan kimlik işlemleri desteklenmez Azure Portal aracılığıyla. Geri güncelleştirmeleri denetleyin. 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında yönetilen hizmet kimliği

Şu anda, VM oluşturma Azure portal aracılığıyla yönetilen hizmet kimliği işlemlerini desteklemiyor. Bunun yerine, lütfen öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın:

- [Azure portalı ile bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Ardından sanal makine üzerindeki yönetilen hizmet kimliği etkinleştiriliyor ilişkin ayrıntılar için sonraki bölüme devam edin.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Yönetilen hizmet kimliği mevcut bir Azure sanal makinesinde etkinleştirin

Sistem tarafından atanan kimliği olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin ve "Yapılandırma" sayfasını seçin.

3. VM üzerindeki sistem tarafından atanan kimliği "Yönetilen hizmet kimliği altında" etkinleştir "Evet"'i seçerek ve ardından **Kaydet**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

    > [!NOTE]
    > Bir VM için bir kullanıcı tarafından atanan kimliği ekleme, Azure Portal aracılığıyla şu anda desteklenmiyor.

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Yönetilen hizmet kimliğini bir Azure VM'den kaldırın.

Bir sanal makine varsa, artık sistem tarafından atanan kimlik gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin ve "Yapılandırma" sayfasını seçin.

3. "Yönetilen hizmet kimliği" altında "Hayır" seçerek atanan sanal makine kimliği sistemi devre dışı bırakın, sonra Kaydet'e tıklayın. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

    > [!NOTE]
    > Bir VM için bir kullanıcı tarafından atanan kimliği ekleme, Azure Portal aracılığıyla şu anda desteklenmiyor.

   ![Yapılandırma sayfasında ekran görüntüsü](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>İlgili içerik

- Yönetilen hizmet kimliği genel bakış için bkz. [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak bir Azure VM'nin MSI vermek [başka bir Azure kaynağına erişim](howto-assign-access-portal.md).

