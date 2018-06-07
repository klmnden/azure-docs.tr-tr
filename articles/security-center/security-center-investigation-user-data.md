---
title: Bir Azure Güvenlik Merkezi araştırma bulunan kullanıcı verilerini yönetme | Microsoft Docs
description: " Azure Güvenlik Merkezi'nin araştırma özelliğinde bulunan kullanıcı verilerini yönetmeyi öğrenin. "
services: operations-management-suite
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: 6685db4eeda72928753c74c64b4b26539ccb1eb9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655813"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Bir Azure Güvenlik Merkezi araştırma bulunan kullanıcı verilerini yönetme
Bu makalede, Azure Güvenlik Merkezi'nin araştırma özelliğinde bulunan kullanıcı verilerini yönetme hakkında bilgi sağlar. Araştırma verileri depolanmaktadır [Azure günlük analizi](../log-analytics/log-analytics-overview.md) ve Güvenlik Merkezi'nde gösteriliyor. Kullanıcı verileri yönetme silin veya verileri dışarı aktarma özelliğini içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Arama ve kişisel veri tanımlama
Azure portalında Güvenlik Merkezi'nin kullanabilirsiniz [araştırma özelliği](../security-center/security-center-investigation.md) kişisel verileri için arama için. Araştırma özellik altında kullanılabilir **güvenlik uyarıları**.

Tüm varlıklar, kullanıcı bilgileri ve verileri altında araştırma özelliği gösterir **varlıklar** sekmesi.

## <a name="securing-and-controlling-access-to-personal-information"></a>Güvenli hale getirme ve kişisel bilgilere erişimi denetleme
Okuyucu, sahibi, katkıda bulunan rolü atanmış bir güvenlik merkezi kullanıcı veya Hesap Yöneticisi araç içinde müşteri verilerine erişebilir.

Bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md) okuyucusu, sahibi ve katkıda bulunan rolleri hakkında daha fazla bilgi edinmek için. Bkz: [Azure aboneliği yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md) hesap yöneticisi rolü hakkında daha fazla bilgi edinmek için.

## <a name="deleting-personal-data"></a>Kişisel verileri silme
Güvenlik Merkezi kullanıcı rol sahibinin katkıda bulunan, atanan veya Hesap Yöneticisi araştırma bilgileri silebilirsiniz.

Bir araştırma silmek için gönderebilmek için bir `DELETE` Azure Resource Manager REST API'si isteği:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

`incidentName` Giriş kullanarak tüm olayları listeleyerek bulunabilir bir `GET` isteği:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Kişisel verileri dışarı aktarma
Güvenlik Merkezi kullanıcı rolü sahibinin katkıda bulunan, atanmış veya Hesap Yöneticisi araştırma bilgilerini dışarı aktarabilirsiniz. Araştırma bilgilerini dışarı aktarmak için Git **varlıklar** ilgili bilgileri kopyalamasını ve yapıştırmasını için sekme.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı verileri yönetme hakkında daha fazla bilgi için bkz: [kullanıcı verilerini Azure Güvenlik Merkezi'nde yönetmek](security-center-privacy.md).
