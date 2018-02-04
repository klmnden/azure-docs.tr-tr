---
title: "Bir MSI erişim Azure Portalı'nı kullanarak bir Azure kaynak atama"
description: "Azure portalını kullanarak başka bir kaynak için bir MSI bir kaynağa erişim atamak için adım adım yönergeler."
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
ms.openlocfilehash: 83a56793d08632918a75f6580360a9dd148d7316
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portalı kullanarak bir kaynak için bir yönetilen hizmet kimliği erişimi atayın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bir yönetilen hizmet kimliği (MSI ile) bir Azure kaynağı yapılandırdıktan sonra hiçbir güvenlik sorumlusu gibi başka bir kaynak MSI erişim izni verebilirsiniz. Bu makalede Azure portalını kullanarak bir Azure depolama hesabı için bir Azure sanal makinenin MSI erişmesini sağlamak nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişim atamak için RBAC kullanın

Bir Azure kaynağı üzerinde MSI etkinleştirdikten sonra [bir Azure VM gibi](msi-qs-configure-portal-windows-vm.md):

1. Oturum [Azure portal](https://portal.azure.com) MSI altında yapılandırdığınız Azure aboneliği ile ilişkili bir hesabı kullanarak.

2. Erişim denetimi değiştirmek istediğiniz istenen kaynağa gidin. Biz depolama hesabınıza gidin şekilde bu örnekte, size bir Azure VM erişimi bir depolama hesabına vermiş olursunuz.

3. Seçin **erişim denetimi (IAM)** sayfasında kaynak ve select **+ Ekle**. Ardından belirtin **rol**, **sanal makineye erişim atamak**ve karşılık gelen belirtin **abonelik** ve **kaynak grubu** kaynağın bulunduğu. Arama ölçütleri alanında kaynak görmeniz gerekir. Kaynak seçin ve Seç **kaydetmek**. 

   ![Erişim denetimi (IAM) ekran görüntüsü](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Ana döndürülür **erişim denetimi (IAM)** gördüğünüz yeni bir giriş için kaynağın MSI sayfası. Bu örnekte, "SimpleWinVM" VM Demo kaynak grubundan sahip **katkıda bulunan** depolama hesabına erişim izni.

   ![Erişim denetimi (IAM) ekran görüntüsü](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure VM'ye geri dönün ve aşağıdakileri denetleyin:

- Bakmak **yapılandırma** sayfasında ve değeri emin **etkin MSI** olan **Evet**.
- Bakmak **uzantıları** sayfasında ve başarıyla dağıtılan MSI uzantısı emin olun.

Ya da yanlışsa, kaynakta MSI yeniden dağıtmanız veya dağıtım hatası sorun giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Azure VM'de MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md).


