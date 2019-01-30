---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 80503ad154a9fc4d01614ffd2816f9d5fd497fdb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228910"
---
<a name="paths"></a>
## <a name="paths"></a>Yollar

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>İstenen serisi veri noktaları süredir anomali noktaları algılayın
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parametreler

|Type|Name|Açıklama|Şema|
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



