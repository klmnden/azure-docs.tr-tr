---
title: Azure portalını kullanarak bir Azure kaynağına MSI erişimi atama
description: Azure portalını kullanarak başka bir kaynağa bir MSI bir kaynağa erişim atamak için adım adım yönergeler.
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
ms.openlocfilehash: 83a56793d08632918a75f6580360a9dd148d7316
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611077"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portalını kullanarak bir kaynak için bir yönetilen hizmet kimliği erişim atama

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bir Azure kaynağı bir yönetilen hizmet kimliği (MSI ile) yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak için MSI erişimi verebilirsiniz. Bu makalede Azure portalını kullanarak bir Azure depolama hesabı için bir Azure sanal makinenin MSI erişimi vermek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişimi atamak için RBAC kullanma

Bir Azure kaynağında MSI etkinleştirdikten sonra [Azure VM'deki gibi](msi-qs-configure-portal-windows-vm.md):

1. Oturum [Azure portalında](https://portal.azure.com) MSI altında yapılandırdığınız Azure aboneliği ile ilişkili bir hesap kullanarak.

2. Erişim denetimi değiştirmek istediğiniz istenen kaynağa gidin. Biz depolama hesabına gidin. Bu nedenle bu örnekte biz bir Azure VM erişimi bir depolama hesabına vermiş olursunuz.

3. Seçin **erişim denetimi (IAM)** seçin ve kaynak sayfasında **+ Ekle**. Ardından belirtin **rol**, **sanal makineye erişim atama**, karşılık gelen belirtin **abonelik** ve **kaynak grubu** kaynağın bulunduğu. Arama ölçütleri alanında kaynak görmeniz gerekir. Kaynak seçip **Kaydet**. 

   ![Erişim denetimi (IAM) ekran görüntüsü](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Ana döndürülür **erişim denetimi (IAM)** gördüğünüz yeni bir giriş için kaynağın MSI sayfası. Bu örnekte, "SimpleWinVM" tanıtım kaynak grubu VM'den sahip **katkıda bulunan** depolama hesabına erişim.

   ![Erişim denetimi (IAM) ekran görüntüsü](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure VM geri dönün ve aşağıdakileri denetleyin:

- Bakmak **yapılandırma** değeri olduğundan emin olun ve sayfa **MSI etkin** olduğu **Evet**.
- Bakmak **uzantıları** sayfasında ve MSI uzantı başarıyla dağıtıldığından emin olun.

Ya da yanlışsa kaynağınızda MSI yeniden dağıtmanız veya dağıtım hatasıyla ilgili sorunları giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Azure VM'deki MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md).


