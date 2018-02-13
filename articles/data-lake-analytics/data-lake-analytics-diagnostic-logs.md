---
title: "Azure Data Lake Analytics için tanılama günlükleri görüntüleme | Microsoft Docs"
description: "Kurulum ve Azure Data Lake analytics için tanılama günlüklerine erişmek öğrenme "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/12/2018
ms.author: larryfr
ms.openlocfilehash: e6cc5fd3d45691dbdc004f346c10d7b4568ae9aa
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için tanılama günlüklerine erişme

Tanılama günlük veri erişimi denetim izleri toplamanızı sağlar. Bu günlükler gibi bilgileri sağlayın:

* Veri erişilen kullanıcıların listesi.
* Verileri ne sıklıkla erişilebilir.
* Ne kadar veri hesabında depolanır.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.

2. Data Lake Analytics hesabınızı açın ve seçin **tanılama günlükleri** gelen __İzleyici__ bölümü. Ardından, __tanılamayı açın__.

    ![Denetim toplama ve günlükleri istemek için tanılamayı açın](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Gelen __tanılama ayarları__, girin bir __adı__ bu günlüğü yapılandırması ve ardından günlük seçenekleri.

    ![Denetim toplama ve günlükleri istemek için tanılamayı açın](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "tanılama günlüklerini etkinleştirme")

   * Depolama/verileri üç farklı yolla işlemi seçebilirsiniz.

     * Seçin __bir depolama hesabı arşive__ bir Azure depolama hesabında günlükleri depolamak için. Verileri arşivlemek istiyorsanız bu seçeneği kullanın. Bu seçeneği seçerseniz, günlüklere kaydetmek için bir Azure depolama hesabı sağlamanız gerekir.

     * Seçin **bir olay Hub'ına akış** Azure olay Hub'ına günlük veri akışı için. Gerçek zamanlı gelen günlüklerini analiz bir aşağı akış işleme ardışık varsa bu seçeneği kullanın. Bu seçeneği belirlerseniz, kullanmak istediğiniz Azure olay Hub ayrıntılarını sağlamanız gerekir.

     * Seçin __için günlük analizi Gönder__ için günlük analizi hizmeti veri göndermek için. Günlük analizi toplamak ve günlüklerini çözümlemek için kullanmak istiyorsanız bu seçeneği kullanın.
   * Denetim günlükleri veya istek günlüklerini veya her ikisini de almak isteyip istemediğinizi belirtin.  İstek günlüğü her API isteği yakalar. Bir denetim günlüğü bu API isteğiyle tetiklenen tüm işlemleri kaydeder.

   * İçin __bir depolama hesabı arşive__, verileri korumak için gün sayısını belirtin.

   * __Kaydet__’e tıklayın.

        > [!NOTE]
        > Ya da seçmelisiniz __Arşiv bir depolama hesabı__, __bir olay Hub'ına akış__ veya __için günlük analizi Gönder__ tıklatmadan önce __kaydetmek__ düğmesi.

### <a name="use-the-azure-storage-account-that-contains-log-data"></a>Günlük verilerini içeren Azure depolama hesabı kullan

1. Günlük verilerini tutun blob kapsayıcıları görüntülemek için Data Lake Analytics için günlüğü için kullanılan Azure Storage hesabı açın ve ardından __BLOB'lar__.

   * Kapsayıcı **Öngörüler günlükleri denetim** denetim günlüklerini içerir.
   * Kapsayıcı **Öngörüler günlükleri istekleri** isteği günlükleri içerir.

2. Kapsayıcılara günlükleri aşağıdaki dosya yapısı altında depolanır:

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > `##` Yolundaki girişleri yıl, ay, gün ve günlük oluşturulduğu saat. Data Lake Analytics bir dosya kadar her saat oluşturur `m=` her zaman değerini içeren `00`.

    Örnek olarak, Denetim günlüğü tam yolunu olabilir:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Benzer şekilde, bir istek günlüğü tam yolunu olabilir:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Günlük yapısı

Denetim ve istek yapılandırılmış bir JSON biçiminde günlüklerin.

### <a name="request-logs"></a>İstek günlükleri

Burada JSON biçimli istek günlüğüne örnek girişi verilmiştir. Her bir blob olarak adlandırılan bir kök nesnesi vardır **kayıtları** günlük nesnelerinin bir dizisi içerir.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>İstek günlüğü şeması

| Ad | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Timestamp (UTC) günlük |
| resourceId |Dize |İşlemi sürdü kaynak tanıtıcısı yerleştirin |
| category |Dize |Günlük kategorisi. Örneğin, **istekleri**. |
| operationName |Dize |Oturum açmış işlemin adı. Örneğin, GetAggregatedJobHistory. |
| resultType |Dize |Örneğin, 200 işlem durumu. |
| callerIpAddress |Dize |İsteği yapan istemcinin IP adresi |
| correlationId |Dize |Oturum tanımlayıcısı. Bu değer, ilgili günlük girdileri kümesini gruplamak için kullanılabilir. |
| identity |Nesne |Günlük oluşturulan kimliği |
| properties |JSON |Ayrıntılar için (istek günlük özellikleri Şeması) sonraki bölümüne bakın |

#### <a name="request-log-properties-schema"></a>İstek günlüğü özellikleri şeması

| Ad | Tür | Açıklama |
| --- | --- | --- |
| HttpMethod |Dize |İşlem için kullanılan HTTP yöntem. Örneğin, alın. |
| Yol |Dize |Yolun işlemi üzerinde gerçekleştirildi |
| RequestContentLength |Int |HTTP isteğinin içerik uzunluğu |
| ClientRequestId |Dize |Bu istek benzersiz olarak tanıtan bir tanımlayıcı |
| StartTime |Dize |Sunucu isteği aldığınız zaman |
| EndTime |Dize |Sunucu yanıt gönderilme saati |

### <a name="audit-logs"></a>Denetim günlükleri

Burada JSON biçimli Denetim günlüğüne örnek girişi verilmiştir. Her bir blob olarak adlandırılan bir kök nesnesi vardır **kayıtları** günlük nesnelerinin bir dizisi içerir.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Denetim günlüğü şeması

| Ad | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Timestamp (UTC) günlük |
| resourceId |Dize |İşlemi sürdü kaynak tanıtıcısı yerleştirin |
| category |Dize |Günlük kategorisi. Örneğin, **denetim**. |
| operationName |Dize |Oturum açmış işlemin adı. Örneğin, JobSubmitted. |
| resultType |Dize |İş durumu (operationName) için bir alt durum. |
| resultSignature |Dize |İş durumu (operationName) ilgili ek ayrıntılar. |
| identity |Dize |İstenen işlemi kullanıcı. Örneğin, susan@contoso.com. |
| properties |JSON |Ayrıntılar için (Denetim günlüğü özellikleri Şeması) sonraki bölümüne bakın |

> [!NOTE]
> **resultType** ve **resultSignature** bir işlemin sonucunu bilgileri sağlayın ve bir işlemin tamamlanması, yalnızca bir değer içerebilir. Örneğin, yalnızca bir değer içerir, **operationName** değerini içeren **JobStarted** veya **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şeması

| Ad | Tür | Açıklama |
| --- | --- | --- |
| JobId |Dize |Projeye atanan kimliği |
| JobName |Dize |İş için sağlanan adı |
| JobRunTime |Dize |İş işlemek için kullanılan çalışma zamanı |
| SubmitTime |Dize |İş gönderilen süreyi (UTC) |
| StartTime |Dize |İş gönderisine (UTC) sonra çalışmaya başladığı saat |
| EndTime |Dize |İşin bitiş saati |
| Paralellik |Dize |Gönderme sırasında bu iş için istenen Data Lake Analytics birim sayısı |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, ve **paralellik** üzerinde bir işlemi bilgileri sağlayın. Bu girişler yalnızca bir değer varsa, işlemi başlatıldı veya tamamlanmış olan içerir. Örneğin, **SubmitTime** yalnızca sonra bir değer içeriyor **operationName** değerine sahip **JobSubmitted**.

## <a name="process-the-log-data"></a>İşlem günlük verileri

Azure Data Lake Analytics hakkında işlemek ve günlük verileri analiz etmek bir örnek sağlar. Örnek: bulabilirsiniz [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
