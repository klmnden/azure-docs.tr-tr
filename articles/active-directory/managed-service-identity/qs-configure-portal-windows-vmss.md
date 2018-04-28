---
title: Azure portalı kullanılarak ayarlanan bir Azure sanal makine ölçek üzerinde MSI yapılandırın
description: Azure portalını kullanarak Azure VMSS üzerinde bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım yönergeler tarafından adım.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: af17bf716ce22bc7c02d40def36248facb6fbcc4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-the-azure-portal"></a>Bir VMSS yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, etkinleştirme ve devre dışı kimlik Azure portalını kullanarak bir VMSS için atanan sistem öğreneceksiniz. Şu anda atama ve kullanıcı kimlikleri Azure VMSS atanan kaldırma Azure Portal desteklenmiyor.

> [!NOTE]
> Şu anda kimlik işlemleri atanan kullanıcı desteklenmez Azure portalı yoluyla. Geri güncelleştirmeleri denetleyin.

## <a name="prerequisites"></a>Önkoşullar


- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md).
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinin oluşturulması sırasında yönetilen hizmet kimliği

Şu anda, VM oluşturma Azure Portalı aracılığıyla yönetilen hizmet kimliği işlemlerini desteklemiyor. Bunun yerine, Lütfen ilk önce bir Azure sanal makine ölçek kümesi oluşturmak için aşağıdaki Azure sanal makine ölçek kümesi oluşturma Hızlı Başlangıç makalesine bakın:

- [Azure portalında bir sanal makine ölçek kümesi oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)  

Ardından sanal makine ölçek kümesinde MSI etkinleştirme ile ilgili ayrıntılar için sonraki bölüme geçin.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Yönetilen hizmet kimliği mevcut bir Azure VMMS üzerinde etkinleştir

Kimliği olmadan ilk olarak sağlanan bir VM üzerinde atanan sistem etkinleştirmek için:

1. Oturum [Azure portal](https://portal.azure.com) sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili bir hesabı kullanarak.

2. İstenen sanal makine ölçek kümesine gidin.

3. VM atanan sistem kimliğini "Yönetilen hizmet kimliği altında" "Evet"'ı seçerek etkinleştirin ve ardından **kaydetmek**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   [![Yapılandırma sayfası ekran görüntüsü](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Yönetilen hizmet kimliği bir Azure sanal makine ölçek kümesinden Kaldır

Artık bir MSI gereken bir sanal makine ölçek kümesi varsa:

1. Oturum [Azure portal](https://portal.azure.com) sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili bir hesabı kullanarak. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun sanal makine ölçek kümesi üzerinde yazma izinleri.

2. İstenen sanal makine ölçek kümesine gidin.

3. "Yönetilen hizmet kimliği" altında "Hayır" seçerek VM'de kimliği atanır sisteminizi devre dışı sonra Kaydet'e tıklayın. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfası ekran görüntüsü](../media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>İlgili İçerik

- Yönetilen hizmet kimliği genel bakış için bkz: [genel bakış](overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Verin Azure portalını kullanarak bir Azure sanal makine ölçek MSI kümesi [erişim için başka bir Azure kaynağı](howto-assign-access-portal.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
