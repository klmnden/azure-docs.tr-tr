---
title: Microsoft Bilişsel hizmetler bilginiz analizleri temel - alma | Microsoft Docs
titleSuffix: Azure
description: Analytics, Bilgi Bankası alma
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 1588d0c5a8eaf4e161b5319c9f33a772dc56b247
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353999"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Analytics, Bilgi Bankası Al

QnA Maker, tüm sohbet günlükleri ve diğer telemetri App Insights sırasında etkinleştirilirse depolar [QnA Maker hizmetinizi oluşturulmasını](./set-up-qnamaker-service-azure.md). Sohbet günlüklerinizi uygulama ilişkin bilgiler almak için örnek sorgular çalıştırın.

1. App Insights kaynağınıza gidin.

    ![Uygulama ınsights kaynağınızı seçin](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Seçin **Analytics**. Yeni bir pencere açılır QnA Maker telemetri WHERE sorgu.

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

    Seçin **çalıştırmak** sorguyu çalıştırmak için.

    ![Sorgu çalıştırma](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Diğer analiz için sorguları, QnA Maker Bilgi Bankası Çalıştır

### <a name="total-90-day-traffic"></a>Toplam 90 günlük trafiği

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

### <a name="user-traffic"></a>Kullanıcı trafiğinin

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

### <a name="latency-distribution-of-questions"></a>Soruların gecikme dağılımı

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
