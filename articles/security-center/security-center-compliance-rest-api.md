---
title: Azure Güvenlik Merkezi REST API kullanarak uyumluluğunu izleme | Microsoft Docs
description: Azure Güvenlik Merkezi REST API kullanarak otomatik uyumluluk taramaları sonuçlarını gözden geçirin.
services: security-center
documentationcenter: na
author: rloutlaw
manager: angerobe
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rloutlaw
ms.openlocfilehash: 651d7737258f1b1b17c8392a09882388ddf95635
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42819905"
---
# <a name="review-security-center-compliance-results-using-the-azure-rest-apis"></a>Azure REST API'lerini kullanarak Güvenlik Merkezi uyumluluk sonuçlarını gözden geçirin

Bu makalede, Azure REST API'lerini kullanarak abonelikleri bir listesi için güvenlik uyumluluk bilgilerini almak öğrenin.

## <a name="review-compliance-for-each-subscription"></a>Her abonelik için uyumluluğunu gözden geçirme

Aşağıdaki örnekte bir verilen uyumluluk ve aboneliği kullanmak için güvenlik değerlendirme bilgilerini alır [alma özellikleri](/rest/api/securitycenter/compliances/get) işlemi.

```http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/compliances/{complianceName}?api-version=2017-08-01-preview
```

`{complianceName}` Parametresi gereklidir ve değerlendirilen uyumluluğunun adını içermelidir `{subscription-id}`. Çıkış değerlendirme sonuçları gibi içerir:

```json
{
...
  "properties": {
    "assessmentResult": [
      {
        "segmentType": "Compliant",
        "percentage": 77.777777777777786
      }
    ],
    "resourceCount": 18,
    "assessmentTimestampUtcDate": "2018-01-01T00:00:00Z"
  }
}
```

## <a name="review-compliance-for-multiple-subscriptions"></a>Birden fazla aboneliğiniz için uyumluluğunu gözden geçirme

Birden çok abonelik için veri almak için önce kullanarak Aboneliklerin listesini almak için aşağıdaki çağrı yapmak [listesi aboneliklerini](/rest/api/resources/subscriptions/list) işlemi. Yukarıdaki Invoke [alma özellikleri](/rest/api/securitycenter/compliances/get) her döndürülen aboneliğin kimliği.

API çağrısıdır:

```http
GET https://management.azure.com/subscriptions?api-version=2016-06-01
```

Aşağıdaki gibi değerleriyle bir dizi döndürür. Subscriptionıd değerleri yukarıdaki çağrısında, tüm abonelikler için uyumluluk bilgilerini gözden geçirmek için kullanın.

```json
"value": [
    {
      "id": "/subscriptions/{subscription-id}",
      "subscriptionId": "{subscription-id}",
      "displayName": "Demo subscription",
      ...
    }
```






