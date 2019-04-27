---
title: API işlemi iptal | Microsoft Docs
description: İşlemleri iptal edin.
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
ms.openlocfilehash: 18f00391beded0744c80eab73bb1efe1c6ab8dbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60625049"
---
<a name="cancel-operation"></a>İşlemi iptal et 
=================

Bu API, öneri şu anda devam eden bir işlem iptal eder. Kullanım [almak operations API'si](./cloud-partner-portal-api-retrieve-operations.md) almak için bir `operationId` bu API'ye geçirilecek. Mevcut bir iptal etmek için yeni bir işlem karmaşık bazı senaryolarda gerekebilir ancak iptal genellikle eşzamanlı bir işlem var. Bu durumda, HTTP yanıt gövdesi durumunu sorgulamak için kullanılması gereken işlem konumunu içerir.

İstekle e-posta adreslerini virgülle ayrılmış bir listesini sağlayabilir ve API bu adresleri işlemi ilerleme durumu hakkında bilgilendirir.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**    |      **Açıklama**                                  |    **Veri türü**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Yayıncı tanımlayıcısını, örneğin, `contoso`         |   String          |
| offerId      |  Teklif tanımlayıcısı                                     |   String          |
| API sürümü  |  Geçerli API sürümü                               |    Tarih           |
|  |  |  |


<a name="header"></a>Üst bilgi
------

|  **Ad**              |  **Değer**         |
|  ---------             |  ----------        |
|  Content-Type          |  uygulama/json  |
|  Yetkilendirme         |  Taşıyıcı YOUR BELİRTEÇ |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="request"></a>İstek

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>İstek gövdesi özellikleri

|  **Ad**                |  **Açıklama**                                               |
|  --------                |  ---------------                                               |
|  bildirim e-postaları     | Virgülle ayrılmış bir e-posta yayımlama işleminin ilerleme durumunu almak kimlikleri listesi. |
|  |  |


### <a name="response"></a>Yanıt

  `Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Ad**             |    **Değer**                       |
|  ---------            |    ----------                      |
| İşlem konumu    | URL, geçerli işlemin durumunu belirlemek için sorgulanabilir. |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |  **Açıklama**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | Tamam. İstek başarıyla işlendi ve işlem zaman uyumlu olarak iptal edilir. |
|  202      | Kabul edildi. İstek başarıyla işlendi ve iptal sürecinde işlemdir. İptal işlemi konumunu yanıt üst bilgisinde döndürülür. |
|  400      | Bozuk/Excel'de hatalı biçimlendirilmiş istek. Hata yanıt gövdesi, daha fazla bilgi sağlayabilir.  |
|  403      | Erişim yasaklandı. İstemci isteğinde belirtilen ad alanına erişimi yok. |
|  404      | Bulunamadı. Belirtilen varlık yok. |
|  |  |
