---
title: Bir MSI erişim Azure Portalı'nı kullanarak bir Azure kaynak atama
description: Azure portalını kullanarak başka bir kaynak için bir MSI bir kaynağa erişim atamak için adım adım yönergeler.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 06f316a7c96ff266e9f4593fa3a9ac871b2979aa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portalı kullanarak bir kaynak için bir yönetilen hizmet kimliği erişimi atayın

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir yönetilen hizmet kimliği (MSI ile) bir Azure kaynağı yapılandırdıktan sonra hiçbir güvenlik sorumlusu gibi başka bir kaynak MSI erişim izni verebilirsiniz. Bu makalede Azure portalını kullanarak bir Azure depolama hesabı için bir Azure sanal makine veya sanal makine ölçek kümesinin MSI erişim vermek nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişim atamak için RBAC kullanın

Bir Azure kaynağı üzerinde gibi MSI etkinleştirdikten sonra bir [Azure VM](qs-configure-portal-windows-vm.md) veya [Azure VMSS](qs-configure-portal-windows-vmss.md):

1. Oturum [Azure portal](https://portal.azure.com) MSI altında yapılandırdığınız Azure aboneliği ile ilişkili bir hesabı kullanarak.

2. Erişim denetimi değiştirmek istediğiniz istenen kaynağa gidin. Bu örnekte, bir Azure sanal makinesi vermiş olursunuz ve biz depolama hesabınıza gidin Azure sanal makine ölçek erişimi bir depolama hesabına ayarlandı.

3. Bir Azure sanal makinesi için seçin **erişim denetimi (IAM)** sayfasında kaynak ve select **+ Ekle**. Ardından belirtin **rol**, **sanal makineye erişim atamak**ve karşılık gelen belirtin **abonelik** ve **kaynak grubu** kaynağın bulunduğu. Arama ölçütleri alanında kaynak görmeniz gerekir. Kaynak seçin ve Seç **kaydetmek**. 

   ![Erişim denetimi (IAM) ekran görüntüsü](../media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Bir Azure sanal makine ölçek kümesi için seçme **erişim denetimi (IAM)** sayfasında kaynak ve select **+ Ekle**. Ardından belirtin **rol**, **atamak için erişim**. Arama ölçütleri alanında, sanal makine ölçek kümesi için arama yapın. Kaynak seçin ve Seç **kaydetmek**.
   
   ![Erişim denetimi (IAM) ekran görüntüsü](../media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. Ana döndürülür **erişim denetimi (IAM)** gördüğünüz yeni bir giriş için kaynağın MSI sayfası.

    Azure sanal makinesi:

   ![Erişim denetimi (IAM) ekran görüntüsü](../media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Azure sanal makine ölçek kümesi:

    ![Erişim denetimi (IAM) ekran görüntüsü](../media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure sanal makineye geri dönün ve aşağıdakileri denetleyin:

- Bakmak **yapılandırma** sayfasında ve değeri emin **etkin MSI** olan **Evet**.
- Bakmak **uzantıları** sayfasında ve başarıyla dağıtılan MSI uzantısı emin olun (**uzantıları** sayfa bir Azure sanal makine ölçek kümesi için kullanılabilir değil).

Ya da yanlışsa, kaynakta MSI yeniden dağıtmanız veya dağıtım hatası sorun giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
- Bir Azure sanal makinede MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md).
- Bir Azure sanal makine ölçek kümesinde MSI etkinleştirmek için bkz: [bir Azure sanal makine ölçek kümesi yönetilen hizmet kimlik (Azure Portalı'nı kullanarak MSI) yapılandırma](qs-configure-portal-windows-vmss.md)


