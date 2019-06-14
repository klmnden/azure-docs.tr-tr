---
title: Azure portalını kullanarak bir Azure kaynağı için bir yönetilen kimlik erişim atama
description: Azure portalını kullanarak başka bir kaynak için bir yönetilen kimlik bir kaynağa erişim atamak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 958b3d72a3a8df4a3b67f62e7db788d7142ca667
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66112905"
---
# <a name="assign-a-managed-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portalını kullanarak bir kaynağa bir yönetilen kimlik erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen bir kimlik ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak yönetilen kimlik erişim izni verebilirsiniz. Bu makalede Azure portalını kullanarak bir Azure depolama hesabı için bir Azure sanal makine veya sanal makine ölçek kümesi'nin yönetilen kimlik erişim vermek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)** .
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynak için bir yönetilen kimlik erişimi atamak için RBAC kullanma

Etkinleştirdikten sonra bir Azure kaynak kimliği gibi yönetilen bir [Azure VM](qs-configure-portal-windows-vm.md) veya [Azure VMSS](qs-configure-portal-windows-vmss.md):

1. Oturum [Azure portalında](https://portal.azure.com) yönetilen kimlik altında yapılandırdığınız Azure aboneliği ile ilişkili bir hesap kullanarak.

2. Erişim denetimi değiştirmek istediğiniz istenen kaynağa gidin. Biz depolama hesabına gidin. Bu nedenle bu örnekte biz bir Azure sanal makine erişimini bir depolama hesabına vermiş olursunuz.

3. Seçin **erişim denetimi (IAM)** seçin ve kaynak sayfasında **+ rol ataması Ekle**. Ardından belirtin **rol**, **erişim Ata**, karşılık gelen belirtin **abonelik**. Arama ölçütleri alanında kaynak görmeniz gerekir. Kaynak seçip **Kaydet**. 

   ![Erişim denetimi (IAM) ekran görüntüsü](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
     
## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen kimlik Azure kaynaklarına genel bakış](overview.md)
- Bir Azure sanal makinesinde yönetilen kimlik etkinleştirmek için bkz: [yapılandırma kimlikleri Azure portalını kullanarak bir VM üzerindeki Azure kaynakları için yönetilen](qs-configure-portal-windows-vm.md).
- Bir Azure sanal makine ölçek kümesinde yönetilen kimlik etkinleştirmek için bkz: [yapılandırma yönetilen bir sanal makine ölçek kümesi Azure portalını kullanarak Azure kaynakları için kimlikleri](qs-configure-portal-windows-vmss.md).


