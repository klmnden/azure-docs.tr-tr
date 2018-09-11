---
title: Azure Güvenlik Merkezi REST API kullanarak uyumluluğunu izleme | Microsoft Docs
description: Azure Güvenlik Merkezi REST API kullanarak otomatik uyumluluk taramaları sonuçlarını gözden geçirin.
services: security-center
documentationcenter: na
author: rloutlaw
manager: angerobe
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rloutlaw
ms.openlocfilehash: 3f07928897df01f5d654fa6a6bffce14290b24e4
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295319"
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






