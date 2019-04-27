---
title: Etkinleştirme ve Azure Data Lake Analytics için tanılama günlüklerini görüntüleme
description: Ayarlama ve Azure Data Lake Analytics için tanılama günlüklerine erişmek nasıl anlama
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.topic: conceptual
ms.date: 02/12/2018
ms.openlocfilehash: 7fd88383e909ebd6be64c22721b813946e37179e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60616526"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için tanılama günlüklerine erişme

Tanılama günlüğüne kaydetme, veri erişimi denetim kayıtlarını toplamanıza olanak tanır. Bu günlükler, bilgileri gibi sağlayın:

* Verilere erişen kullanıcıların listesi.
* Verilere ne sıklıkta erişildiğine.
* Ne kadar veri hesabında depolanır.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.

2. Data Lake Analytics hesabınızı açın ve seçin **tanılama günlükleri** gelen __İzleyici__ bölümü. Ardından, __tanılamayı Aç__.

    ![Denetim toplama ve günlükleri istemek için tanılamayı Aç](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Gelen __tanılama ayarları__, girin bir __adı__ için bu günlük kaydı yapılandırması ve ardından oturum açma seçenekleri.

    ![Denetim toplama ve günlükleri istemek için tanılamayı açın](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "tanılama günlüklerini etkinleştirme")

   * Depolama/verileri üç farklı yolla işlemi seçebilirsiniz.

     * Seçin __bir depolama hesabında arşivle__ bir Azure depolama hesabında günlüklerini depolamak için. Verileri arşivlemek istiyorsanız bu seçeneği kullanın. Bu seçeneği belirlerseniz, günlüklere kaydetmek için bir Azure depolama hesabı sağlamanız gerekir.

     * Seçin **Stream olay Hub'ına** Azure olay Hub'ına günlük veri akışı. Gerçek zamanlı olarak gelen günlüklerini analiz bir aşağı akış işleme işlem hattı varsa bu seçeneği kullanın. Bu seçeneği belirlerseniz, Azure Event Hub kullanmak istediğiniz için ayrıntıları sağlamanız gerekir.

     * Seçin __Log Analytics'e gönderme__ Azure İzleyici'hizmetine veri göndermek için. Azure İzleyici günlükleri toplayabilir ve bu günlükleri analiz etmek için kullanmak istiyorsanız bu seçeneği kullanın.
   * Denetim günlükleri veya istek günlükleri veya her ikisi de almak isteyip istemediğinizi belirtin.  İstek günlüğü, her API isteği yakalar. Bu API isteğiyle tetiklenen tüm işlemler bir denetim günlüğüne kaydeder.

   * İçin __bir depolama hesabında arşivle__, verileri saklanacağı gün sayısını belirtin.

   * __Kaydet__’e tıklayın.

        > [!NOTE]
        > Ya da seçmelisiniz __bir depolama hesabında arşivle__, __Stream olay Hub'ına__ veya __Log Analytics'e gönderme__ tıklatmadan önce __Kaydet__ düğmesi.

### <a name="use-the-azure-storage-account-that-contains-log-data"></a>Günlük verilerini içeren Azure depolama hesabı kullan

1. Günlük verileri tutmak blob kapsayıcıları görüntülemek için Data Lake Analytics için günlüğü için kullanılan Azure depolama hesabı'nı açın ve ardından __Blobları__.

   * Kapsayıcı **ınsights günlükleri denetim** denetim günlükleri içeriyor.
   * Kapsayıcı **ınsights günlükleri istekleri** istek günlükleri içerir.

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
   > `##` Yolunda girişleri yıl, ay, gün ve günlük oluşturulduğu saat. Data Lake Analytics bir dosya şekilde her saat oluşturur `m=` her zaman değerini içeren `00`.

    Örneğin, bir denetim günlüğüne tam yolunu olabilir:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Benzer şekilde, bir istek günlüğü tam yolunu olabilir:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Günlük yapısı

Denetim ve istek günlükler, yapılandırılmış bir JSON biçiminde olur.

### <a name="request-logs"></a>Günlükleri iste

JSON biçimli istek günlüğünde örnek giriş aşağıdadır. Her blob olarak adlandırılan bir kök nesnesinin sahip **kayıtları** günlük nesnelerinin bir dizisi içeren.

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
| time |String |Zaman damgası (UTC) günlüğü |
| resourceId |String |İşlem geçen kaynağının tanımlayıcısını yerleştireceğinize |
| category |String |Günlük kategorisi. Örneğin, **istekleri**. |
| operationName |String |Oturum açmış işlemin adı. Örneğin, GetAggregatedJobHistory. |
| resultType |String |Örneğin, 200 işlem durumu. |
| callerIpAddress |String |İsteği yapan istemcinin IP adresi |
| correlationId |String |Oturum tanımlayıcısı. Bu değer, bir dizi ilgili günlük girişlerini gruplandırmak için kullanılabilir. |
| identity |Object |Günlük oluşturulan kimlik |
| properties |JSON |Ayrıntılar için sonraki bölüme (istek günlüğü özellikleri şema) bakın |

#### <a name="request-log-properties-schema"></a>İstek günlüğü özellikleri şeması

| Ad | Tür | Açıklama |
| --- | --- | --- |
| HttpMethod |String |HTTP yöntemi, bir işlem için kullanılmaz. Örneğin, alın. |
| Yol |String |İşlem yolu üzerinde gerçekleştirildi |
| RequestContentLength |int |HTTP isteğinin içerik uzunluğu |
| Clientrequestıd'ye |String |Bu istek benzersiz olarak tanımlayan tanımlayıcısı |
| StartTime |String |Sunucu isteği aldığınız zaman |
| EndTime |String |Sunucu yanıt gönderme zamanı |

### <a name="audit-logs"></a>Denetim günlükleri

JSON biçimli bir denetim günlüğüne örnek girişini İşte. Her blob olarak adlandırılan bir kök nesnesinin sahip **kayıtları** günlük nesnelerinin bir dizisi içeren.

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
| time |String |Zaman damgası (UTC) günlüğü |
| resourceId |String |İşlem geçen kaynağının tanımlayıcısını yerleştireceğinize |
| category |String |Günlük kategorisi. Örneğin, **denetim**. |
| operationName |String |Oturum açmış işlemin adı. Örneğin, JobSubmitted. |
| resultType |String |İş durumu (operationName) için alt. |
| resultSignature |String |İş durumu (operationName) ilgili ek ayrıntılar. |
| identity |String |İstenen işlem kullanıcı. Örneğin, susan@contoso.com. |
| properties |JSON |Ayrıntılar için sonraki bölüme (Denetim günlüğü özellikleri şema) bakın |

> [!NOTE]
> **resulttype'ı** ve **resultSignature** bir işlemin sonucu hakkında bilgi sağlar ve bir işlemin tamamlanması yalnızca bir değer içerir. Örneğin, yalnızca bir değer içerdiği zaman **operationName** değerini içeren **JobStarted** veya **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şeması

| Ad | Tür | Açıklama |
| --- | --- | --- |
| JobId |String |Projeye atanan kimliği |
| JobName |String |İş için sağlanan adı |
| JobRunTime |String |İşi işlemek için kullanılan çalışma zamanı |
| SubmitTime |String |İş gönderildi, saat (UTC) |
| StartTime |String |Gönderisine (UTC) sonra çalışan işin başladığı saati |
| EndTime |String |İşin bitiş saati |
| Paralellik |String |Gönderim sırasında bu iş için istenen Data Lake Analytics birimi |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, ve **paralellik** işlem bilgileri sağlayın. Bu girişler yalnızca bir değer varsa işlemi başlatıldı veya tamamlandı içerir. Örneğin, **SubmitTime** yalnızca sonra bir değer içeren **operationName** değerine sahip **JobSubmitted**.

## <a name="process-the-log-data"></a>Günlük verilerini işleme

Azure Data Lake Analytics, günlük verilerini işlemek ve çözümlemek nasıl bir örnek sağlar. Örneğini şurada bulabilirsiniz [ https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample ](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
