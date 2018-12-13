---
title: Hizmet sorumlusu yönetilen bir kimlik, Azure portalında görüntüleme
description: Hizmet sorumlusu yönetilen bir kimlik, Azure portalında görüntülemek için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: daveba
ms.openlocfilehash: 123cd1c808ad833d9b10148e4cb292b335075a8d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52883344"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Hizmet sorumlusu yönetilen bir kimlik, Azure portalında görüntüleme

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, hizmet sorumlusu, Azure portalını kullanarak bir yönetilen kimlik görüntüleme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/).
- Etkinleştirme [bir sanal makinede sistem tarafından atanan kimlik](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulama](/azure/app-service/app-service-managed-service-identity#adding-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>Hizmet sorumlusu görüntüleyin

Bu yordam, hizmet sorumlusu, bir VM ile sistem tarafından atanan kimliği etkinleştirildi görüntülemek gösterilmektedir (aynı adımlar, bir uygulama için geçerlidir).

1. Tıklayın **Azure Active Directory** ve ardından **kurumsal uygulamalar**.
2. Altında **uygulama türü**, seçin **tüm uygulamaları**.
3. Arama filtre kutusuna adını yazın VM veya yönetilen kimliği uygulama etkin veya sunulan listeden seçin.

   ![Yönetilen kimlik hizmet sorumlusu portalında görüntüleme](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure kaynakları için yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview)

