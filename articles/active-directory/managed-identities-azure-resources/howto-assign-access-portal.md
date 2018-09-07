---
title: Azure portalını kullanarak bir Azure kaynağına MSI erişimi atama
description: Azure portalını kullanarak başka bir kaynağa bir MSI bir kaynağa erişim atamak için adım adım yönergeler.
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
ms.openlocfilehash: b6423a80b0405bcaabd4f1fc8e26dc3be0adadb0
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028622"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portalını kullanarak bir kaynak için bir yönetilen hizmet kimliği erişim atama

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir Azure kaynağı bir yönetilen hizmet kimliği (MSI ile) yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak için MSI erişimi verebilirsiniz. Bu makalede Azure portalını kullanarak bir Azure depolama hesabı için bir Azure sanal makine veya sanal makine ölçek kümesi'nin MSI erişimi vermek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişimi atamak için RBAC kullanma

MSI gibi bir Azure kaynağında etkinleştirdikten sonra bir [Azure VM](qs-configure-portal-windows-vm.md) veya [Azure VMSS](qs-configure-portal-windows-vmss.md):

1. Oturum [Azure portalında](https://portal.azure.com) MSI altında yapılandırdığınız Azure aboneliği ile ilişkili bir hesap kullanarak.

2. Erişim denetimi değiştirmek istediğiniz istenen kaynağa gidin. Bu örnekte, biz de Azure sanal makinesi vermiş olursunuz ve biz depolama hesabına gidin. Bu nedenle Azure sanal makine ölçek bir depolama hesabına erişim ayarlayın.

3. Bir Azure sanal makinesi için seçin **erişim denetimi (IAM)** seçin ve kaynak sayfasında **+ Ekle**. Ardından belirtin **rol**, **sanal makineye erişim atama**, karşılık gelen belirtin **abonelik** ve **kaynak grubu** kaynağın bulunduğu. Arama ölçütleri alanında kaynak görmeniz gerekir. Kaynak seçip **Kaydet**. 

   ![Erişim denetimi (IAM) ekran görüntüsü](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Bir Azure sanal makine ölçek kümesi için seçin **erişim denetimi (IAM)** seçin ve kaynak sayfasında **+ Ekle**. Ardından belirtin **rol**, **erişim Ata**. Arama ölçütleri alanında, sanal makine ölçek kümesi için arama yapın. Kaynak seçip **Kaydet**.
   
   ![Erişim denetimi (IAM) ekran görüntüsü](./media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. Ana döndürülür **erişim denetimi (IAM)** gördüğünüz yeni bir giriş için kaynağın MSI sayfası.

    Azure sanal makine:

   ![Erişim denetimi (IAM) ekran görüntüsü](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Azure sanal makine ölçek kümesi:

    ![Erişim denetimi (IAM) ekran görüntüsü](./media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure sanal makinesi için geri dönün ve aşağıdakileri denetleyin:

- Bakmak **yapılandırma** değeri olduğundan emin olun ve sayfa **MSI etkin** olduğu **Evet**.
- Bakmak **uzantıları** sayfasında ve MSI uzantı başarıyla dağıtıldığından emin olun (**uzantıları** sayfa bir Azure sanal makine ölçek kümesi için kullanılabilir değil).

Ya da yanlışsa kaynağınızda MSI yeniden dağıtmanız veya dağıtım hatasıyla ilgili sorunları giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
- Azure sanal makinesinde MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md).
- Bir Azure sanal makine ölçek kümesinde MSI etkinleştirmek için bkz: [bir Azure sanal makine ölçek kümesi yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vmss.md)


