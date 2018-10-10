---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: fb02496d9c107a2c21acca6c65ef69fdfceb4597
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904580"
---
<a name="paths"></a>
## <a name="paths"></a>Yollar

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>İstenen serisi veri noktaları süredir anomali noktaları algılayın
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parametreler

|Tür|Ad|Açıklama|Şema|
|---|---|---|---|
|**Gövde**|**Gövde**  <br>*Gerekli*|Zaman serisi veri noktaları ve gerekiyorsa süre.|[İstek](#request)|


#### <a name="responses"></a>Yanıtlar

|HTTP kodu|Açıklama|Şema|
|---|---|---|
|**200**|Başarılı bir işlem.|< [yanıt](#response) > dizi|
|**400**|JSON istek ayrıştırılamıyor.|İçerik Yok|
|**403**|Sağlanan sertifikanın sunucu tarafından kabul edilmiyor.|İçerik Yok|
|**405**|Yönteme izin verilmiyor.|İçerik Yok|
|**500**|İç Sunucu Hatası.|İçerik Yok|


#### <a name="consumes"></a>Tüketir

* `application/json`


#### <a name="produces"></a>Üretir

* `application/json`


#### <a name="tags"></a>Etiketler

* anomalydetectıon



