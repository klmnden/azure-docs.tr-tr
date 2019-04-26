---
title: PIM - Azure Active Directory yönetemezsiniz rolleri | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) yönetemez rolleri açıklanır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 01/18/2019
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa5fb632ee5fd9c18bde7443e81fe2ef6e5335e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60437282"
---
# <a name="roles-you-cannot-manage-in-pim"></a>Rolleri PIM'de yönetemez.

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) tüm yönetmenize imkan sağlar [Azure AD rolleri](../users-groups-roles/directory-assign-admin-roles.md) ve tüm [Azure kaynağı rolleri](../../role-based-access-control/built-in-roles.md). Bu roller, Yönetim grupları, abonelik, kaynak grupları ve kaynaklara bağlı özel rollerinizi de içerir. Ancak, yönetemezsiniz birkaç rol vardır. Bu makalede PIM'de yönetemez rolleri açıklanır.

## <a name="classic-subscription-administrator-roles"></a>Klasik abonelik yönetici rolleri

PIM aşağıdaki Klasik Abonelik Yöneticisi rolleri yönetemezsiniz:

- Hesap Yöneticisi
- Hizmet Yöneticisi
- Ortak Yönetici

Klasik Abonelik Yöneticisi rolleri hakkında daha fazla bilgi için bkz: [Klasik Abonelik Yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rollerini](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>Office 365 Yönetici rolleri hakkında neler diyeceksiniz?

Exchange Online veya SharePoint Online, Exchange yönetici ve SharePoint Yöneticisi rolleri Azure AD'de temsil edilmez ve PIM'de yönetilemez. Bu Office 365 hizmetleri hakkında daha fazla bilgi için bkz. [Office 365 Yönetici rolleri](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> SharePoint Yöneticisi, SharePoint Online Yönetim Merkezi SharePoint Online yönetimsel erişim sahibi ve hemen her görevi SharePoint Online'da gerçekleştirebilirsiniz. Uygun kullanıcılar PIM'de etkinleştirdikten sonra SharePoint içinde bu rolü kullanarak gecikme.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD PIM Rolleri Ata](pim-how-to-add-role-to-user.md)
- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
