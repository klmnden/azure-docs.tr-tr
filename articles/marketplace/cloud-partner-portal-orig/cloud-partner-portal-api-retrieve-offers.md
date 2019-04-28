---
title: Alma API sunar | Microsoft Docs
description: API teklifleri yayımcı ad alanı altında bir Özet listesini alır.
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
ms.openlocfilehash: de9261548ec79e206b0db87caabc1fa4c9ad6771
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61094514"
---
<a name="retrieve-offers"></a>Teklifleri alma
===============

Teklifler yayımcı ad alanı altında bir Özet listesini alır.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>URI parametreleri
--------------

| **Ad**         |  **Açıklama**                         |  **Veri türü** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Örneğin, yayımcı tanımlayıcısı `contoso` |   String    |
|  API sürümü     | API'sının en son sürümü                    |    Tarih        |
|  |  |


<a name="header"></a>Üst bilgi
------

|  **Ad**        |         **Değer**       |
|  --------------- |       ----------------  |
|  Content-Type    | `application/json`      |
|  Yetkilendirme   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Ad**       |       **Açıklama**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Teklif türünü tanımlar                                                                                           |
|  publisherId    | Yayımcı benzersiz olarak tanımlayan tanımlayıcısı                                                                      |
|  durum         | Teklif durumu. Olası değerler listesi için bkz: [teklif durumu](#offer-status) aşağıda.                         |
|  id             | Teklif yayımcı ad alanında benzersiz olarak tanımlayan GUID.                                                    |
|  version        | Teklifin geçerli sürümü. İstemci tarafından version özelliği değiştirilemez. Bu, her yayımladıktan sonra artırılır. |
|  tanım     | Gerçek iş yükü tanımını bir Özet görünümünü içerir. Ayrıntılı bir tanımı almak için kullanın [alma belirli teklif](./cloud-partner-portal-api-retrieve-specific-offer.md) API. |
|  changedTime    | Teklif en son değiştirildiği UTC saati                                                                              |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |  **Açıklama**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` -İstek başarıyla işlendi ve yayımcı altındaki tüm teklifleri istemciye döndürülmedi.  |
|  400      | `Bad/Malformed request` -Hata yanıt gövdesi, daha fazla bilgi içeriyor olabilir.                                    |
|  403      | `Forbidden` -İstemcisi belirtilen ad alanı için erişime sahip değil.                                          |
|  404      | `Not found` -Belirtilen varlık yok.                                                                 |
|  |  |


### <a name="offer-status"></a>Teklif durumu

|  **Ad**                    | **Açıklama**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | Teklif, hiç yayımlanmadı.                  |
|  NotStarted                  | Teklif, yeni ancak başlatılmadı.                 |
|  WaitingForPublisherReview   | Teklif, yayımcı onay bekliyor.         |
|  Çalışıyor                     | Teklif gönderimi işleniyor.             |
|  Başarılı oldu                   | Teklif gönderme işlemi tamamlandı.       |
|  İptal edildi                    | Teklif gönderim iptal edildi.                   |
|  Başarısız                      | Teklif gönderme başarısız oldu.                         |
|  |  |
