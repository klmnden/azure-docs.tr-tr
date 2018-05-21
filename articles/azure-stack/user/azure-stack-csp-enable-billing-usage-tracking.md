---
title: Azure yığın aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştirme | Microsoft Docs
description: Bir abonelik Azure yığınında erişmek hizmet sağlayıcısı etkinleştirin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: f0cff8f575b87872c0032854f1916b140d7fd62b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Azure yığın aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bir bulut hizmeti sağlayıcısı (CSP) ile Azure yığın kullanıyorsanız, Azure ve Azure yığın kaynaklara erişmek için kendi aboneliğinizi yönetmek seçin. Ayrıca, aboneliğinizi yönetmek sağlayıcısı sağlayabilirsiniz. Bu makale size nasıl gösterir için:

 * Hizmet sağlayıcısı erişiminizi aboneliğinizi verin.
 * Hizmet sağlayıcısı hizmetinizi yönetebilirsiniz emin olun.

> [!Note]
>  CSP hesabınızı yönetme değil ve aşağıdaki adımları atlayın, CSP Azure yığın aboneliğiniz yönetemez.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Aboneliğinizi bir bulut hizmeti sağlayıcısı ile yönetme

CSP olarak ekleme **kullanıcı** aboneliğinizde.

1. Kiracı dizininize kullanıcı rolüne sahip Konuk kullanıcı olarak, CSP ekleyin.  Kullanıcı ekleme adımları için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. CSP, yerel Azure yığın abonelik oluşturur.
3. Azure yığın kullanmaya başlamak hazır olursunuz.
4. CSP, bunlar ayrıca kaynaklarınızı yönetebilir doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir. Örneğin, bir yönetici şunları yapabilir [Azure yığın portalı ile Windows sanal makine oluşturmak](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>RBAC hakları kullanarak aboneliğinizi yönetmek bulut hizmeti sağlayıcısı etkinleştir

CSP olarak ekleme **sahibi** aboneliğinizde.

1. CSP, Kiracı dizinine Konuk kullanıcı olarak ekleyin.  Kullanıcı ekleme adımları için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. Sahip rolü CSP Konuk kullanıcıya ekleyin. Aboneliğinize CSP kullanıcı ekleme hakkında daha fazla adımlar için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek üzere Use Role-Based erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. CSP, yerel Azure yığın abonelik oluşturur.
4. Azure yığın kullanmaya başlamak hazır olursunuz.
5. CSP aboneliğinizde kaynaklarınızı yönetebilir doğrulamak için bir kaynak oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure yığınından kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve fatura Azure yığınında](../azure-stack-billing-and-chargeback.md).
