---
title: API işlemlerini alma | Azure Market
description: Tüm işlemler teklif veya belirli bir işlem için belirtilen Operationıd almak için alır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: 1fbcc1d50dbc4488c4123be64e85de612233ccc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935772"
---
<a name="retrieve-operations"></a>İşlemleri alma
===================

Tüm işlemler teklif veya belirli bir işlem için belirtilen Operationıd almak için alır. İstemci işlemleri üzerinde çalışan filtrelemek için sorgu parametrelerini kullanabilir.

``` https

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/submissions/?api-version=2017-10-31&status=<filteredStatus>

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/operations/<operationId>?api-version=2017-10-31

```


<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**          |      **Açıklama**                                                                                           | **Veri türü** |
|  ----------------  |     --------------------------------------------------------------------------------------------------------   |  -----------  |
|  publisherId       |  Örneğin, yayımcı tanımlayıcısı `Contoso`                                                                   |  String       |
|  OfferId           |  Teklif tanımlayıcısı                                                                                              |  String       |
|  operationId       |  Teklif işlemi benzersiz olarak tanımlayan GUID. Operationıd bu API kullanılarak alınabilir ve ayrıca herhangi bir uzun süre çalışan işlem için yanıtın HTTP üst bilgisindeki gibi döndürülür [Yayımla teklif](./cloud-partner-portal-api-publish-offer.md) API.  |   Guid   |
|  filteredStatus    | Duruma göre filtrelemek için kullanılan isteğe bağlı bir sorgu parametresi (örneğin `running`) bu API'si tarafından döndürülen koleksiyonu.  |   String |
|  API sürümü       | API'sının en son sürümü                                                                                           |    Tarih      |
|  |  |  |


<a name="header"></a>Üstbilgi
------

|  **Ad**          |  **Değer**           |
|  ---------------   | -------------------- |
|  İçerik türü      | `application/json`   |
|  Yetkilendirme     | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

#### <a name="get-operations"></a>İşlemleri Al

``` json
    [
        {
            "id": "5a63deb5-925b-4ee0-938b-7c86fbf287c5",
            "offerId": "56615b67-2185-49fe-80d2-c4ddf77bb2e8",
            "offerVersion": 1,
            "offerTypeId": "microsoft-azure-virtualmachines",
            "publisherId": "contoso",
            "submissionType": "publish",
            "submissionState": "running",
            "publishingVersion": 2,
            "slot": "staging",
            "version": 636576975611768314,
            "definition": {
                "metadata": {
                    "emails": "jdoe@contoso.com"
                }
            },
            "changedTime": "2018-03-26T21:46:01.179948Z"
        }
    ]
```

#### <a name="get-operation"></a>ALMA işlemi

``` json
    [
        {
            "status" : "running",
            "messages" : [],
            "publishingVersion" : 2,
            "offerVersion" : 1,
            "cancellationRequestState": "canCancel",
            "steps": [
                        {
                            "estimatedTimeFrame": "< 15 min",
                            "id": "displaydummycertify",
                            "stepName": "Validate Pre-Requisites",
                            "description": "Offer settings provided are validated",
                            "status": "complete",
                            "messages": 
                            [
                                {
                                    "messageHtml": "Step completed.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:36.500052Z"
                                }
                            ],
                            "progressPercentage": 100
                        },
                        {
                            "estimatedTimeFrame": "< 5 day",
                            "id": "displaycertify",
                            "stepName": "Certification",
                            "description": "Your offer is analyzed by our certification systems for issues.",
                            "status": "blocked",
                            "messages": 
                            [
                                {
                                    "messageHtml": "No virtual machine image was found for the plan contoso.",
                                    "level": "error",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                },
                                {
                                    "messageHtml": "This step has not started yet.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                }
                            ],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 day",
                            "id": "displayprovision",
                            "stepName": "Provisioning",
                            "description": "Your virtual machine is being replicated in our production systems.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 hour",
                            "id": "displaypackage",
                            "stepName": "Packaging and Lead Generation Registration",
                            "description": "Your virtual machine is packaged for being shown to your customers. Additionally, we hookup our lead generation systems to send leads for your offer.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "id": "publisher-signoff",
                            "stepName": "Publisher signoff",
                            "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "~2-5 days",
                            "id": "live",
                            "stepName": "Live",
                            "description": "Offer is publicly visible and is available for purchase.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        }
                    ],
                "previewLinks": [],
                "liveLinks": [],
                "notificationEmails": "jondoe@contoso.com"
            } 
        }
    ]
```


### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Ad**                    |  **Açıklama**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
|  id                          | İşlem benzersiz olarak tanımlayan GUID                                                       |
|  submissionType              | Teklif için örneğin bildirilen işlemi türünü tanımlar `Publish/GGoLive`      |
|  oluşturma tarihi/saati             | İşlemi oluşturulduğunda UTC tarih/saat                                                       |
|  lastActionDateTime          | Son güncelleştirme işlemi bittiğinde UTC tarih/saat                                       |
|  status                      | İşlemin durumu ya da `not started` \| `running` \| `failed` \| `completed`. Yalnızca tek bir işlem durumu olabilir `running` birer güncelleştirir. |
|  error                       | Başarısız işlemler için hata iletisi                                                               |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |   **Açıklama**                                                                                  |
|  -------- |   -------------------------------------------------------------------------------------------------|
|  200      | `OK` -İstek başarıyla işlendi ve istenen işlemleri döndürülmedi.        |
|  400      | `Bad/Malformed request` -Hata yanıt gövdesi, daha fazla bilgi içeriyor olabilir.                    |
|  403      | `Forbidden` -İstemcisi belirtilen ad alanı için erişime sahip değil.                          |
|  404      | `Not found` -Belirtilen varlık yok.                                                 |
|  |  |
