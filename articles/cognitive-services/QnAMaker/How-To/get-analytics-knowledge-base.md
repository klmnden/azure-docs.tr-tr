---
title: Bilgi Bankası Analizine
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, soru-cevap Oluşturucu hizmetinizi oluşturma sırasında App Insights etkinleştirdiyseniz, tüm sohbet günlükleri ve diğer telemetriyi depolar. Uygulama anlayışları'ndan, sohbet günlüklerini almak için örnek sorgular çalıştırın.
services: cognitive-services
author: tulasim88
manager: cgronlun
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim88
ms.openlocfilehash: e6c7cb4ca4265166a516adfda0fb9f70f4548a2e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213063"
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
