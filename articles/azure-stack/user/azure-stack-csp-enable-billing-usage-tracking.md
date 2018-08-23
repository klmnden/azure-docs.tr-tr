---
title: Azure Stack aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştirme | Microsoft Docs
description: Azure Stack'te bir abonelik erişmek hizmet sağlayıcısı'nı etkinleştirin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: f309b86578f340040927735c067656158f3198fc
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056560"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Azure Stack aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştir

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Azure Stack bir bulut hizmeti sağlayıcısı (CSP) ile kullanıyorsanız, azure'da ve Azure Stack kaynaklara erişmek için kendi aboneliğinizi yönetmek seçin. Ayrıca, aboneliğinizi yönetmek sağlayıcısı sağlayabilirsiniz. Bu makalede gösterilmektedir için:

 * Hizmet sağlayıcısı erişiminizi aboneliğinize verin.
 * Hizmet sağlayıcısı hizmetinizi yönetebilmeniz için emin olun.

> [!Note]
>  CSP hesabınızı yönetme değil ve aşağıdaki adımları atlayın, CSP sizin için Azure Stack aboneliğine yönetemez.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Bir bulut hizmeti sağlayıcısına aboneliğinizi yönetin

CSP olarak ekleme **kullanıcı** aboneliğinize.

1. CSP olarak Konuk kullanıcı kullanıcı rolüne sahip Kiracı dizininize ekleyin.  Bir kullanıcı ekleme adımları için bkz [Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. CSP, yerel Azure Stack aboneliğine sizin için oluşturur.
3. Azure Stack kullanmaya başlamak hazır olursunuz.
4. CSP, bunlar Ayrıca, kaynakları yönetebilir doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir. Örneğin, yapabilirler [Azure Stack portal ile bir Windows sanal makinesi oluşturma](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>RBAC haklarını kullanarak aboneliğinizi yönetmek bulut hizmeti sağlayıcısı etkinleştir

CSP olarak ekleme **sahibi** aboneliğinize.

1. CSP, Kiracı dizinine Konuk kullanıcı olarak ekleyin.  Bir kullanıcı ekleme adımları için bkz [Azure Active Directory'ye yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. CSP Konuk kullanıcı için sahip rolü ekleyin. Aboneliğinizde CSP kullanıcı ekleme adımları için bkz [Use Role-Based erişim denetimi, Azure abonelik kaynaklarınıza erişimi yönetmek için](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. CSP, yerel Azure Stack aboneliğine sizin için oluşturur.
4. Azure Stack kullanmaya başlamak hazır olursunuz.
5. CSP kaynaklarınızı yönetebildiğini doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](../azure-stack-billing-and-chargeback.md).
