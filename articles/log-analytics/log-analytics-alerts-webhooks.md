---
title: "Web kancası uyarı eylemi örnek OMS günlük analizi | Microsoft Docs"
description: "Günlük analizi uyarı yanıt çalıştırabilirsiniz eylemlerden biri olan bir * bir dış işlem üzerinden tek bir HTTP isteği çağırma sayesinde Web kancası *. Bu makalede bir Web kancası eylem kayma kullanarak günlük analizi uyarıda oluşturma örneği anlatılmaktadır."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: 55b66132f7ec5c26c0a7cac1ec0a5c403dbd1082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-to-send-message-to-slack"></a>Bir uyarı Web kancası eylem Slack'e ileti göndermek için OMS günlük analizi oluşturun
Eylemlerden birini çalıştırabilirsiniz yanıt olarak bir [günlük analizi uyarı](log-analytics-alerts.md) olan bir *Web kancası*, bir dış işlem üzerinden tek bir HTTP isteği çağırma sağlar.  Uyarılar ve Web kancalarını içinde ayrıntıları hakkında bilgi edinebilirsiniz [günlük analizi uyarıları](log-analytics-alerts.md)

Bu makalede, sizi bir ileti sistemi hizmeti olan kayma kullanarak günlük analizi uyarıda bir Web kancası eylem oluşturma örneği adım geçireceğiz.

> [!NOTE]
> Bu örnek tamamlamak için Slack bir hesabınızın olması gerekir.  Ücretsiz bir hesap için kaydolabilirsiniz [slack.com](http://slack.com).
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a>1. adım - kayma içinde etkinleştir Web kancaları
1. Oturumu sırasında Slack'e [slack.com](http://slack.com).
2. Bir kanal seçmeniz **kanalları** sol bölmede bölümü.  İleti gönderilir kanal budur.  Varsayılan kanalları birini gibi seçebilirsiniz **genel** veya **rastgele**.  Bir üretim senaryosunda, büyük olasılıkla özel kanal gibi oluşturacak **criticalservicealerts**. <br>
   
   ![Slack kanallarında](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. Tıklatın **bir uygulama veya özel tümleştirme ekleme** uygulama dizini açmak için.
4. Tür *kancalarını* arama kutusuna ve ardından **gelen Web Kancalarını**. <br>
   
   ![Slack kanallarında](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. Tıklatın **yükleme** takım adınızın yanındaki.
6. Tıklatın **yapılandırması eklemek**.
7. Seçin, bu örnek için kullanın ve ardından oluşturacağız kanal **eklemek gelen Web Kancalarını tümleştirme**.  
8. Kopya **Web kancası URL'si**.  Bu uyarı yapılandırma yapıştırdığınız. <br>
   
    ![Slack kanallarında](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>2. adım - günlük analizi uyarı kuralı oluştur
1. [Bir uyarı kuralı oluştur](log-analytics-alerts.md) aşağıdaki ayarlara sahip.
   * Sorgu:```    Type=Event EventLevelName=error ```
   * Bu uyarıyı denetleme her: 5 dakika
   * Sonuç sayısı: 10'dan büyük
   * Bu zaman penceresi üzerinden: 60 dakika
   * Seçin **Evet** için **Web kancası** ve **Hayır** diğer eylemler için.
2. İçine kayma URL'sini yapıştırın **Web kancası URL'si** alan.
3. Seçeneğini **özel JSON yükünü dahil et**.
4. Kayma JSON'adlı bir parametre ile biçimlendirilmiş bir yükü bekliyor *metin*.  Bu iletinin oluşturduğu görüntüleyecek metindir.  Bir veya daha fazla kullanarak uyarı parametrelerini kullanabilirsiniz  *#*  aşağıdaki örnekteki gibi simge.
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```
   
    ![Örnek JSON yükü](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. Tıklatın **kaydetmek** uyarı kuralını kaydetmek için.
6. Bir uyarının oluşturulması ve aşağıdakine benzer bir ileti için kayma denetlemek yeterli bir süre bekleyin.
   
   ![Örnek Web kancası kayma içinde](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a>Web kancası yükü kayma için Gelişmiş
Gelen iletiler kayma ile kapsamlı bir şekilde özelleştirebilirsiniz. Daha fazla bilgi için bkz: [gelen Web Kancalarını](https://api.slack.com/incoming-webhooks) Slack Web sitesinde. Biçimlendirme ile zengin bir ileti oluşturmak için daha karmaşık bir yükü aşağıdadır:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Bu kayma içinde bir ileti oluşturur aşağıdakine benzer.

![Kayma örnek iletisi](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Özet
Bu uyarı kuralı ile yerinde ölçütleri karşılanıyorsa her zaman Slack'e gönderilen bir ileti gerekir.  

Bu yalnızca bir yanıt olarak bir uyarı oluşturmak bir eylem örneğidir.  Başka bir dış hizmet çağıran bir Web kancası eylemi, Azure Automation'da bir runbook'u başlatmak için bir runbook eylemi ya da kendinizin veya diğer alıcılar için bir posta göndermek için e-posta eylem oluşturabilirsiniz.   

## <a name="next-steps"></a>Sonraki Adımlar
* Diğer hakkında bilgi edinin [uyarı günlük analizi Eylemler](log-analytics-alerts-actions.md) diğer Eylemler dahil olmak üzere.


