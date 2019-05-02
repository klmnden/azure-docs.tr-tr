---
title: Bir teklifi yayımlama | Azure Market
description: Belirtilen teklif yayımlamak için API'ı tıklatın.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 117a4e5e238e754524ff813ce25ebc1105e2153c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64934972"
---
<a name="publish-an-offer"></a>Teklif yayımlama
================

Belirtilen teklif için yayımlama işlemi başlar. Bu çağrı bir uzun süren bir işlemdir.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

<a name="uri-parameters"></a>URI parametreleri
--------------

|  **Ad**      |    **Açıklama**                               |  **Veri türü** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Örneğin, yayımcı tanımlayıcısı `contoso`      |   String       |
|  offerId       | Teklif tanımlayıcısı                                 |   String       |
|  API sürümü   | API'nin en son sürümü                        |   Tarih         |
|  |  |


<a name="header"></a>Üst bilgi
------

|  **Ad**        |    **Değer**          |
|  --------        |    ---------          |
|  Content-Type    | `application/json`    |
|  Yetkilendirme   |  `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="request"></a>İstek

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>İstek gövdesi özellikleri

|  **Ad**               |   **Açıklama**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  bildirim e-postaları    | Yayımlama işleminin ilerleme durumunu gönderilecek e-posta adreslerini virgülle ayrılmış liste. |
|  |  |


### <a name="response"></a>Yanıt

   `Operation-Location: /api/operations/contoso$56615b67-2185-49fe-80d2-c4ddf77bb2e8$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>Yanıt Üst Bilgisi

|  **Ad**             |    **Değer**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| İşlem konumu    | Geçerli işlemin durumunu belirlemek için sorgulanabilir URL'si.    |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |  **Açıklama**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted` -İstek başarıyla kabul edildi. Başlatılan işlem izlemek için kullanılan bir konuma yanıtı içerir. |
| 400   | `Bad/Malformed request` -Hata yanıt gövdesi, daha fazla bilgi sağlayabilir.                                                               |
| 422   | `Un-processable entity` -Yayımlanacak varlık başarısız olduğunu gösterir doğrulama.                                                        |
| 404   | `Not found` -İstemci tarafından belirtilen varlık yok.                                                                              |
|  |  |
