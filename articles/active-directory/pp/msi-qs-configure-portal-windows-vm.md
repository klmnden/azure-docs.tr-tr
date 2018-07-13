---
title: Azure portalını kullanarak bir Azure sanal makinesinde MSI yapılandırma
description: Azure Azure portalını kullanarak bir VM'de, bir yönetilen hizmet kimliği (MSI) yapılandırma yönergeleri adım adım.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 37710015904c8112e5d2de504ed5b42895ffb809
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610324"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve Azure portalını kullanarak Azure VM için MSI kaldırma öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında MSI etkinleştir

Bu makalenin yazıldığı zaman itibaren Azure portalında bir VM oluşturulurken MSI etkinleştirmesine desteklenmiyor. Bunun yerine, lütfen öncelikle bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makalelerini birine bakın:

- [Azure portalı ile bir Windows sanal makine oluşturun](~/articles/virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](~/articles/virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Ardından VM'deki MSI etkinleştirme hakkında daha fazla ayrıntı için sonraki bölüme devam edin.

## <a name="enable-msi-on-an-existing-azure-vm"></a>Mevcut Azure VM'deki MSI etkinleştir

İlk olarak bir MSI sağlanan bir VM'niz varsa:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin.

2. "Yapılandırma" sayfası, VM'deki MSI "Yönetilen hizmet kimliği altında" "Evet"'i seçerek Etkinleştir'ı tıklatın **Kaydet**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfasında ekran görüntüsü](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>Bir Azure VM'den MSI Kaldır

Bir sanal makine varsa, artık bir MSI gerekir:

1. Oturum [Azure portalında](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesap kullanarak. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin.

3. "Yapılandırma" sayfası, MSI "Yönetilen hizmet kimliği" altında "Hayır"'i seçerek sanal makineden kaldırın, ardından tıklatın **Kaydet**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfasında ekran görüntüsü](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak bir Azure VM'nin MSI vermek [başka bir Azure kaynağına erişim](msi-howto-assign-access-portal.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
