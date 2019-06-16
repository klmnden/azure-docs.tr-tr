---
title: Canlı izleyin | Azure Market
description: Go Live API işlem listeleme Canlı teklif başlatır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: ac56f86bad132f3e00a4b5c2507d65c0722c628c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935484"
---
<a name="go-live"></a>Canlı izleyin
=======

Bu API, bir uygulamayı üretime göndermeden işlemi başlatır. Bu işlem genellikle uzun ömürlü. Bu çağrı bildirim e-posta listesinden kullanan [Yayımla](./cloud-partner-portal-api-publish-offer.md) API işlemi.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**      |   **Açıklama**                                                           | **Veri türü** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Örneğin, almak teklif için yayımcı tanımlayıcısı `contoso`       |  String       |
| OfferId        | Teklif almak için teklif tanımlayıcısı                                   |  String       |
| API sürümü    | API'nin en son sürümü                                                   |  Tarih         |
|  |  |  |


<a name="header"></a>Üstbilgi
------

|  **Ad**       |     **Değer**       |
|  ---------      |     ----------      |
| İçerik türü    | `application/json`  |
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

| **Kod** |  **Açıklama**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted` -İstek başarıyla kabul edildi. İşlem durumunu izlemek için bir konum yanıtı içerir. |
|  400     | `Bad/Malformed request` -Ek hata bilgileri, yanıt gövdesi içinde bulunur. |
|  404     |  `Not found` -Belirtilen varlık yok.                                       |
|  |  |
