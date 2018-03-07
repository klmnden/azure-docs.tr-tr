---
title: "Azure portalı kullanılarak ayarlanan bir Azure sanal makine ölçek üzerinde MSI yapılandırın"
description: "Azure portalını kullanarak Azure VMSS üzerinde bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım yönergeler tarafından adım."
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 4808165aa760be7e397bd3601e958419780e0004
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="configure-an-azure-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Bir Azure sanal makine ölçek kümesi yönetilen hizmet kimlik (Azure Portalı'nı kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Portalı'nı kullanarak etkinleştirin ve bir Azure sanal makine ölçek kümesi için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçek kümesi oluşturma sırasında MSI etkinleştir

Bu yazma tarihinde Azure portalında kümesi bir sanal makine ölçek oluşturulması sırasında MSI etkinleştirme desteklenmiyor. Bunun yerine, Lütfen ilk önce bir Azure sanal makine ölçek kümesi oluşturmak için aşağıdaki Azure sanal makine ölçek kümesi oluşturma Hızlı Başlangıç makalesine bakın:

- [Azure portalında bir sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-create-portal.md)  

Ardından sanal makine ölçek kümesinde MSI etkinleştirme ile ilgili ayrıntılar için sonraki bölüme geçin.

## <a name="enable-msi-on-an-existing-azure-vmms"></a>Var olan bir Azure VMMS üzerinde MSI etkinleştir

İlk olarak bir MSI sağlanan bir sanal makine ölçek kümesi varsa:

1. Oturum [Azure portal](https://portal.azure.com) sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili bir hesabı kullanarak.

2. İstenen sanal makine ölçek kümesine gidin.

3. Tıklatın **yapılandırma** sayfasında, sanal makineyi ölçeği seçerek Ayarla MSI etkinleştirmek **Evet** "Yönetilen hizmet kimliği altında", ardından **kaydetmek**. Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfası ekran görüntüsü](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden MSI kaldırma

Artık bir MSI gereken bir sanal makine ölçek kümesi varsa:

1. Oturum [Azure portal](https://portal.azure.com) sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili bir hesabı kullanarak. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun sanal makine ölçek kümesi üzerinde yazma izinleri.

2. İstenen sanal makine ölçek kümesine gidin.

3. Tıklatın **yapılandırma** sayfasında, sanal makineyi ölçeği seçerek Ayarla MSI kaldırma **Hayır** altında **yönetilen hizmet kimliği**, ardından **Kaydet** . Bu işlemi tamamlamak için 60 saniye veya daha fazlasını gerçekleştirebilirsiniz:

   ![Yapılandırma sayfası ekran görüntüsü](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Verin Azure portalını kullanarak bir Azure sanal makine ölçek MSI kümesi [erişim için başka bir Azure kaynağı](msi-howto-assign-access-portal.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
