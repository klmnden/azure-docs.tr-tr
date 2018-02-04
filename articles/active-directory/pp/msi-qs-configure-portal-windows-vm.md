---
title: "Azure Portalı'nı kullanarak bir Azure VM'deki MSI yapılandırma"
description: "Tarafından yönetilen hizmet kimliği (MSI) Azure Azure portalını kullanarak bir VM, yapılandırma için adım yönergeler adım."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 37710015904c8112e5d2de504ed5b42895ffb809
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Azure portalını kullanarak VM için MSI kaldırma etkinleştirip öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında MSI etkinleştir

Bu yazma tarihinde Azure portalında bir VM oluşturulması sırasında MSI etkinleştirme desteklenmiyor. Bunun yerine, Lütfen ilk olarak bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makaleleri birine bakın:

- [Azure portalı ile Windows sanal makine oluşturma](~/articles/virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portalıyla Linux sanal makine oluşturma](~/articles/virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Ardından VM'de MSI etkinleştirme ile ilgili ayrıntılar için sonraki bölüme geçin.

## <a name="enable-msi-on-an-existing-azure-vm"></a>Var olan Azure VM'de MSI etkinleştir

İlk olarak bir MSI sağlanan bir VM varsa:

1. Oturum [Azure portal](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesabı kullanarak. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin.

2. "Yapılandırma" sayfasında, VM'de MSI "Yönetilen hizmet kimliği altında" "Evet"'i seçerek etkinleştirin, ardından tıklatın **kaydetmek**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfası ekran görüntüsü](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>MSI bir Azure sanal makineden kaldırın

Bir sanal makine varsa, artık bir MSI gerekir:

1. Oturum [Azure portal](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesabı kullanarak. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin.

3. "Yapılandırma" sayfasında, "Yönetilen hizmet kimliği" altında "Hayır" seçerek MSI sanal makineden kaldırın, ardından tıklatın **kaydetmek**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfası ekran görüntüsü](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal kullanarak bir Azure VM'in MSI verin [erişim için başka bir Azure kaynağı](msi-howto-assign-access-portal.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
