---
title: Teklif durumunun | Microsoft Docs
description: API, teklifin geçerli durumunu alır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 9233a5919ad86adcbb7947cd095945654ed015a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61093990"
---
<a name="retrieve-offer-status"></a>Teklif durumunu alma 
=====================

Teklif geçerli durumunu alır.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**       |   **Açıklama**                            |  **Veri türü** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherId    | Örneğin, yayımcı tanımlayıcısı `Contoso`  |     String     |
|  offerId        | Teklifin benzersiz olarak tanımlayan GUID      |     String     |
|  API sürümü    | API'sının en son sürümü                        |     Tarih       |
|  |  |


<a name="header"></a>Üst bilgi
------

|  Ad           |  Değer               |
|  -------------  | -------------------  |
|  Content-Type   |  `application/json`  |
|  Yetkilendirme  | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
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
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
      "notificationEmails": "jdoe@contoso.com"
  } 
```


### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Ad**             |    **Açıklama**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  durum               | Teklif durumu. Olası değerler listesi için bkz: [teklif durumu](#offer-status) aşağıda. |
|  sayısı             | Bir Teklifle ilişkili iletilerin dizisi                                                    |
|  adımlar                | Bir teklifi yayımlama sırasında teklif geçtiği adımlar dizisi                      |
|  estimatedTimeFrame   | Tahmini, kolay bir biçimde bu adımı tamamlamak için geçen süre                       |
|  id                   | Adım tanımlayıcısı                                                                         |
|  stepName             | Adım adı                                                                               |
|  açıklama          | Adım açıklaması                                                                        |
|  durum               | Adım durumu. Olası değerler listesi için bkz: [adım durumu](#step-status) aşağıda.    |
|  sayısı             | İlgili adıma ileti dizisi                                                          |
|  processPercentage    | Tamamlanma adımın                                                              |
|  previewLinks         | *Henüz uygulanmadı*                                                                    |
|  liveLinks            | *Henüz uygulanmadı*                                                                    |
|  notificationEmails   | İşlemin ilerleme durumunu gönderilecek e-posta adreslerini virgülle ayrılmış listesi        |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |   **Açıklama**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK` -İstek başarıyla işlendi ve önerinin geçerli durumu döndürüldü. |
|  400     | `Bad/Malformed request` -Hata yanıt gövdesi, daha fazla bilgi içeriyor olabilir.                 |
|  404     | `Not found` -Belirtilen varlık yok.                                                |
|  |  |


### <a name="offer-status"></a>Teklif durumu

|  **Ad**                    |    **Açıklama**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  NeverPublished              | Teklif, hiç yayımlanmadı.                          |
|  NotStarted                  | Teklif, yeni ve başlatılmadı.                            |
|  WaitingForPublisherReview   | Teklif, yayımcı onay bekliyor.                 |
|  Çalışıyor                     | Teklif gönderimi işleniyor.                     |
|  Başarılı oldu                   | Teklif gönderme işlemi tamamlandı.               |
|  İptal edildi                    | Teklif gönderim iptal edildi.                           |
|  Başarısız                      | Teklif gönderme başarısız oldu.                                 |
|  |  |


### <a name="step-status"></a>Adım durumu

|  **Ad**                    |    **Açıklama**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | Adım başlatılmadı.                        |
|  Devam Ediyor                  | Adım çalışıyor.                             |
|  WaitingForPublisherReview   | Adım yayımcı onay bekliyor.      |
|  WaitingForApproval          | Adım işlemi onay bekliyor.        |
|  Engellendi                     | Adım engellenir.                             |
|  Reddedildi                    | Adım reddedilir.                            |
|  Tamamlama                    | Adım tamamlandı.                            |
|  İptal edildi                    | Adım iptal edildi.                           |
|  |  |
