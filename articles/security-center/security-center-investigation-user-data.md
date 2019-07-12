---
title: Azure Güvenlik Merkezi soruşturma bulunan kullanıcı verilerini yönetme | Microsoft Docs
description: " Azure Güvenlik Merkezi'nin araştırma özelliği bulunan kullanıcı verilerini yönetmeyi öğrenin. "
services: operations-management-suite
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rkarlin
ms.openlocfilehash: 1fd979be117104186b2dfce47cc79947a092eb9e
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672340"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Azure Güvenlik Merkezi soruşturma bulunan kullanıcı verilerini yönetme
Bu makalede, Azure Güvenlik Merkezi'nin araştırma özelliği bulunan kullanıcı verilerini yönetme hakkında bilgi sağlar. Veri araştırma depolanan [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md) ve Güvenlik Merkezi'nde gösterilen. Kullanıcı verileri yönetmek, silme veya verileri dışarı aktarma özelliğini içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Arama ve kişisel verileri tanımlama
Azure portalında, Güvenlik Merkezi'nin kullanabileceğiniz [araştırma özelliği](../security-center/security-center-investigation.md) kişisel verileri için arama yapmak üzere. Araştırma özelliği altında kullanılabilir **güvenlik uyarıları**.

Tüm varlıklar, kullanıcı bilgilerini ve altında veri araştırma özelliğini gösterir **varlıkları** sekmesi.

## <a name="securing-and-controlling-access-to-personal-information"></a>Güvenliğini sağlama ve kişisel bilgilere erişim denetleme
Güvenlik Merkezi kullanıcı Okuyucu, sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi, araç içinde müşteri verilerine erişebilir.

Bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md) Okuyucu, sahibi ve katkıda bulunan rolleri hakkında daha fazla bilgi edinmek için. Bkz: [Azure aboneliği yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md) hesap yöneticisi rolü hakkında daha fazla bilgi edinmek için.

## <a name="deleting-personal-data"></a>Kişisel verileri silme
Güvenlik Merkezi kullanıcı sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi araştırma bilgileri silebilirsiniz.

Araştırma silinemedi, gönderebilirsiniz bir `DELETE` Azure Resource Manager REST API'si isteği:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

`incidentName` Kullanarak tüm olayları listeleyerek giriş bulunabilir bir `GET` isteği:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Kişisel verileri dışarı aktarma
Güvenlik Merkezi kullanıcı sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi araştırma bilgi verebilirsiniz. Araştırma bilgilerini dışarı aktarmak için Git **varlıkları** kopyalamak ve yapıştırmak ilgili bilgiler için sekmesinde.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı verileri yönetme hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde kullanıcı verilerini yönetme](security-center-privacy.md).
Azure İzleyici günlüklerine içerisindeki özel verilere silme hakkında daha fazla bilgi edinmek için [dışarı aktarın ve özel veri silme](../azure-monitor/platform/personal-data-mgmt.md#how-to-export-and-delete-private-data).
