---
title: Microsoft Graph API'leri için PIM (Önizleme) - Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) (Önizleme) Microsoft Graph API'lerini kullanma hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 11/13/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e54ec4049b2b0cd67c148d881a64a40efff438a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60440475"
---
# <a name="microsoft-graph-apis-for-pim-preview"></a>Microsoft Graph API'leri için PIM (Önizleme)

Azure Active Directory (Azure AD) Privileged Identity Management (Azure portalını kullanarak PIM içinde) gerçekleştirebileceğiniz görevlerin çoğunu için ayrıca kullanarak gerçekleştirebileceğiniz [Microsoft Graph API'lerini](https://developer.microsoft.com/graph/docs/concepts/overview). Bu makalede, Microsoft Graph API'leri için PIM kullanırken bazı önemli kavramlar açıklanmaktadır. Microsoft Graph API'leri hakkında daha fazla ayrıntı için kullanıma [Azure AD Privileged Identity Management API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> /Beta sürümünü Microsoft Graph API'leri Önizleme aşamasındadır ve değiştirilebilir. Bu API'leri üretim uygulamalarında kullanılması desteklenmiyor.

## <a name="required-permissions"></a>Gerekli izinler

PIM için Microsoft Graph API'leri çağırmak için olmalıdır **bir veya daha fazla** aşağıdaki izinleri:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>İzin ayarla

PIM için Microsoft Graph API'leri çağırmak uygulamalar için gerekli izinlere sahip olmalıdır. Gerekli izinleri belirlemek için en kolay yolu kullanmaktır [Azure AD'ye onay çerçevesine](../develop/consent-framework.md).

### <a name="set-permissions-in-graph-explorer"></a>Graph Explorer'da izinleri ayarlama

Aramalarınız test etmek için Graph Gezgini kullanıyorsanız Aracı'nda izinleri belirtebilir.

1. Oturum [Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) genel Yöneticisi olarak.

1. Tıklayın **değiştirme izinleri**.

    ![Graph Gezgini - izinleri değiştirme](./media/pim-apis/graph-explorer.png)

1. Dahil etmek istediğiniz izinleri yanına onay işaretleri koyacağız ekleyin. `PrivilegedAccess.ReadWrite.AzureAD` henüz Graph Explorer'da kullanılamıyor.

    ![Graph Gezgini - izinleri değiştirme](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Tıklayın **değiştirme izinlerini** izin değişiklikleri uygulamak için.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Privileged Identity Management API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
