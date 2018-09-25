---
title: Bilgi Bankası Analizine
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, soru-cevap Oluşturucu hizmetinizi oluşturma sırasında App Insights etkinleştirdiyseniz, tüm sohbet günlükleri ve diğer telemetriyi depolar. Uygulama anlayışları'ndan, sohbet günlüklerini almak için örnek sorgular çalıştırın.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim88
ms.openlocfilehash: dc363a3ba0d809a3307a6803993bdf500da45f1b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035428"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Bilgi bankanız hakkında analizler alma

Soru-cevap Oluşturucu, tüm sohbet günlükleri ve diğer telemetriyi App Insights sırasında etkinleştirdiyseniz depolar [, soru-cevap Oluşturucu hizmetini oluşturulmasını](./set-up-qnamaker-service-azure.md). Uygulama anlayışları'ndan, sohbet günlüklerini almak için örnek sorgular çalıştırın.

1. App Insights kaynağınıza gidin.

    ![Application ınsights kaynağınızı seçin](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Seçin **Analytics**. Yeni bir pencere açılır burada soru-cevap Oluşturucu telemetri sorgulayabilirsiniz.

    ![Analytics seçin](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Aşağıdaki sorguda yapıştırın ve çalıştırın.

    ```query
        requests
        | where url endswith "generateAnswer"
        | project timestamp, id, name, resultCode, duration
        | parse name with *"/knowledgebases/"KbId"/generateAnswer"
        | join kind= inner (
        traces | extend id = operation_ParentId
        ) on id
        | extend question = tostring(customDimensions['Question'])
        | extend answer = tostring(customDimensions['Answer'])
        | project KbId, timestamp, resultCode, duration, question, answer
    ```

    Seçin **çalıştırma** sorguyu çalıştırmak için.

    ![Sorgu çalıştırma](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu bankanızı üzerinde analiz sorguları çalıştırma

### <a name="total-90-day-traffic"></a>Toplam 90 günlük trafik

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
``` 

### <a name="total-question-traffic-in-a-given-time-period"></a>Toplam soru trafiği belirli bir süre içinde

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Kullanıcı trafiği

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse name with *"/knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Soru gecikme dağılımı

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Anahtarları Yönet](./key-management.md)
