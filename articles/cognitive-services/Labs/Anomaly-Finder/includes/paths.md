---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: a806cac410eb57e59dacb42da9be954b2f962956
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353332"
---
<a name="paths"></a>
## <a name="paths"></a>Yollar

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>İstenen serisi veri noktaları süredir anomali noktaları Algıla
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parametreler

|Tür|Ad|Açıklama|Şema|
|---|---|---|---|
|**Gövde**|**Gövde**  <br>*Gerekli*|Zaman serisi veri noktaları ve gerekirse süresi.|[İstek](#request)|


#### <a name="responses"></a>Yanıtlar

|HTTP kodu|Açıklama|Şema|
|---|---|---|
|**200**|Başarılı bir işlem.|< [yanıt](#response) > dizisi|
|**400**|JSON istek ayrıştıramıyor.|İçerik Yok|
|**403**|Belirttiğiniz sertifika sunucusu tarafından kabul edilmedi.|İçerik Yok|
|**405**|Yönteme izin verilmiyor.|İçerik Yok|
|**500**|İç Sunucu Hatası.|İçerik Yok|


#### <a name="consumes"></a>Kullanır

* `application/json`


#### <a name="produces"></a>Üretir

* `application/json`


#### <a name="tags"></a>Etiketler

* anomalydetection



