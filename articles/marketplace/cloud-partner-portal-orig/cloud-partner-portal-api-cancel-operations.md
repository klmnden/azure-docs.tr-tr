---
title: API işlemi iptal | Azure Market
description: İşlemleri iptal edin.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 70ffd13be4ba934b423e3bb5344eea0a9c36886c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935557"
---
# <a name="cancel-operation"></a>İşlemi iptal et 

Bu API, öneri şu anda devam eden bir işlem iptal eder. Kullanım [almak operations API'si](./cloud-partner-portal-api-retrieve-operations.md) almak için bir `operationId` bu API'ye geçirilecek. Mevcut bir iptal etmek için yeni bir işlem karmaşık bazı senaryolarda gerekebilir ancak iptal genellikle eşzamanlı bir işlem var. Bu durumda, HTTP yanıt gövdesi durumunu sorgulamak için kullanılması gereken işlem konumunu içerir.

İstekle e-posta adreslerini virgülle ayrılmış bir listesini sağlayabilir ve API bu adresleri işlemi ilerleme durumu hakkında bilgilendirir.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**    |      **Açıklama**                                  |    **Veri türü**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Yayıncı tanımlayıcısını, örneğin, `contoso`         |   String          |
| OfferId      |  Teklif tanımlayıcısı                                     |   String          |
| API sürümü  |  Geçerli API sürümü                               |    Tarih           |
|  |  |  |


<a name="header"></a>Üstbilgi
------

|  **Ad**              |  **Değer**         |
|  ---------             |  ----------        |
|  İçerik türü          |  uygulama/json  |
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
