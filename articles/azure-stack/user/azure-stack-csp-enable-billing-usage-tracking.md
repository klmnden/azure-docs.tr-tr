---
title: "Azure yığın aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştirme | Microsoft Docs"
description: "Bir abonelik Azure yığınında erişmek hizmet sağlayıcısı etkinleştirin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 4bc5644425aa11fb210d81095e4166baefc6432e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Azure yığın aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bir bulut hizmeti sağlayıcısı (CSP) ile Azure yığın kullanıyorsanız, Azure aboneliğinizin ve Azure yığın kaynaklarına erişiminiz sağlayıcı tarafından yönetiliyor olabilir. Veya kendi aboneliğinizi yönetmek. Bu makalede, aboneliğinizi sizin adınıza erişmek için ya da hizmet sağlayıcısı hizmetinizi yönetebilirsiniz emin olmak için hizmet sağlayıcınıza nasıl etkinleştirebilirsiniz adresindeki arar.

> [!Note]  
>  Aşağıdaki adımları atlanır ve CSP hesabınızı yönetme değil, CSP Azure yığın aboneliğinizi sizin adınıza yönetmek mümkün olmaz.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Aboneliğinizi bir bulut hizmeti sağlayıcısı ile yönetme

1. Kiracı dizininize kullanıcı rolüne sahip Konuk kullanıcı olarak, CSP ekleyin.  Kullanıcı ekleme adımları için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. CSP, ardından yerel Azure yığın abonelik sizin için oluşturur.
3. Azure yığın kullanmaya başlamak hazır olursunuz.
3. CSP sonra bunlar ayrıca kaynaklarınızı yönetebilir doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir. Örneğin, bir yönetici şunları yapabilir [Azure yığın portalı ile Windows sanal makine oluşturmak](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>RBAC hakları kullanarak aboneliğinizi yönetmek bulut hizmeti sağlayıcısı etkinleştir

CSP aboneliğinize sahibi olarak ekleyin. 

1. Konuk kullanıcı olarak, CSP ekleyin. Kiracı dizinine sahip rolüyle.  Kullanıcı ekleme adımları için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. Sahip rolü CSP Konuk kullanıcıya ekleyin. Aboneliğinize CSP kullanıcı ekleme hakkında daha fazla adımlar için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek üzere Use Role-Based erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)
3. CSP, ardından yerel Azure yığın abonelik sizin için oluşturur.
4. Azure yığın kullanmaya başlamak hazır olursunuz.
5. CSP aboneliğinizde kaynaklarınızı yönetebilir doğrulamak için bir kaynak sonra oluşturmanız gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

  - Azure yığınından kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve fatura Azure yığınında](../azure-stack-billing-and-chargeback.md).
