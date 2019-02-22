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
ms.openlocfilehash: ea3f26d70c4a4ce07c988612890687504a4cf5ac
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594687"
---
<a name="go-live"></a>Canlı Yayına Geç
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

| **Kod** |  **Açıklama**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted` -İstek başarıyla kabul edildi. İşlem durumunu izlemek için bir konum yanıtı içerir. |
|  400     | `Bad/Malformed request` -Ek hata bilgileri, yanıt gövdesi içinde bulunur. |
|  404     |  `Not found` -Belirtilen varlık yok.                                       |
|  |  |
