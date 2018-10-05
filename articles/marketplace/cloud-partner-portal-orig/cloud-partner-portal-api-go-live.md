---
title: Canlı izleyin | Microsoft Docs
description: Go Live API işlem listeleme Canlı teklif başlatır.
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
ms.openlocfilehash: c7d643f0c7885e64636a107d22ce332b1ba9371c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811299"
---
<a name="go-live"></a>Canlı Yayına Geç
=======

Bu API, bir uygulamayı üretime göndermeden işlemi başlatır. Bu işlem genellikle uzun ömürlü. Bu çağrı bildirim e-posta listesinden kullanan [Yayımla](./cloud-partner-portal-api-publish-offer.md) API işlemi.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**      |   **Açıklama**                                                           | **Veri türü** |
|  --------      |   ---------------                                                           | ------------- |
| Publisherıd    | Örneğin, almak teklif için yayımcı tanımlayıcısı `contoso`       |  Dize       |
| OfferId        | Teklif almak için teklif tanımlayıcısı                                   |  Dize       |
| API sürümü    | API'nin en son sürümü                                                   |  Tarih         |
|  |  |  |


<a name="header"></a>Üst bilgi
------

|  **Ad**       |     **Değer**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Yetkilendirme   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Ad**             |      **Değer**                                                            |
|  --------             |      ----------                                                           |
| İşlem konumu    |  Geçerli işlemin durumunu belirlemek için sorgu URL'si            |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  ** Açıklaması **                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted` -İstek başarıyla kabul edildi. İşlem durumunu izlemek için bir konum yanıtı içerir. |
|  400     | `Bad/Malformed request` -Ek hata bilgileri, yanıt gövdesi içinde bulunur. |
|  404     |  `Not found` -Belirtilen varlık yok.                                       |
|  |  |
