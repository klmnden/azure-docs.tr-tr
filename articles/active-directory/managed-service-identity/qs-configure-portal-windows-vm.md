---
title: Azure Portalı'nı kullanarak bir Azure VM'deki MSI yapılandırma
description: Tarafından yönetilen hizmet kimliği (MSI) Azure Azure portalını kullanarak bir VM, yapılandırma için adım yönergeler adım.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 62b8504f5c10f338539d263bb231cf96eb405ba6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, etkinleştirme ve devre dışı kimlik Azure Azure portalını kullanarak VM için atanan sistem öğreneceksiniz. Şu anda atama ve kullanıcı kimlikleri Azure VM'lerin atanan kaldırma Azure Portal desteklenmiyor.

> [!NOTE]
> Şu anda kimlik işlemleri atanan kullanıcı desteklenmez Azure portalı yoluyla. Geri güncelleştirmeleri denetleyin. 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md).
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturulması sırasında yönetilen hizmet kimliği

Şu anda, VM oluşturma Azure Portalı aracılığıyla yönetilen hizmet kimliği işlemlerini desteklemiyor. Bunun yerine, Lütfen ilk olarak bir VM oluşturmak için aşağıdaki VM oluşturma Hızlı Başlangıç makaleleri birine bakın:

- [Azure portalı ile Windows sanal makine oluşturma](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portalıyla Linux sanal makine oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Sonra yönetilen hizmet kimliği VM'de etkinleştirme ile ilgili ayrıntılar için sonraki bölüme geçin.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Yönetilen hizmet kimliği mevcut bir Azure VM'de etkinleştir

Kimliği olmadan ilk olarak sağlanan bir VM üzerinde atanan sistem etkinleştirmek için:

1. Oturum [Azure portal](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesabı kullanarak. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin ve "Yapılandırma" sayfasını seçin.

3. VM atanan sistem kimliğini "Yönetilen hizmet kimliği altında" "Evet"'ı seçerek etkinleştirin ve ardından **kaydetmek**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

    > [!NOTE]
    > Bir VM'ye atanan kullanıcı kimliğini ekleme Azure Portal şu anda desteklenmiyor.

   ![Yapılandırma sayfası ekran görüntüsü](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Yönetilen hizmet kimliği bir Azure sanal makineden kaldırın

Bir sanal makine varsa, artık atanan sistem kimliği gerekir:

1. Oturum [Azure portal](https://portal.azure.com) VM içeren Azure aboneliği ile ilişkili bir hesabı kullanarak. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri.

2. İstenen sanal makineye gidin ve "Yapılandırma" sayfasını seçin.

3. "Yönetilen hizmet kimliği" altında "Hayır" seçerek VM'de kimliği atanır sisteminizi devre dışı sonra Kaydet'e tıklayın. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

    > [!NOTE]
    > Bir VM'ye atanan kullanıcı kimliğini ekleme Azure Portal şu anda desteklenmiyor.

   ![Yapılandırma sayfası ekran görüntüsü](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>İlgili içerik

- Yönetilen hizmet kimliği genel bakış için bkz: [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal kullanarak bir Azure VM'in MSI verin [erişim için başka bir Azure kaynağı](howto-assign-access-portal.md).

