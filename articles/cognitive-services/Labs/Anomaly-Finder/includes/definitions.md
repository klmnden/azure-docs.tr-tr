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
ms.openlocfilehash: 5ad589c4adb60369f81979e214935f73d9eb0755
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309542"
---
<a name="definitions"></a>
## <a name="definitions"></a>Tanımlar

<a name="point"></a>
### <a name="point"></a>Noktası

|Ad|Açıklama|Şema|
|---|---|---|
|**Zaman damgası**  <br>*İsteğe bağlı*|Veri noktası için zaman damgası. Bunu hizalar gece yarısı ve bir UTC tarih saat dizesi, örneğin, kullanım 2017 emin olun-08-01T00:00:00Z.|dize (tarih)|
|**Değer**  <br>*İsteğe bağlı*|Veri ölçü birimi değeri.|sayı (çift)|


<a name="request"></a>
### <a name="request"></a>İstek

|Ad|Açıklama|Şema|
|---|---|---|
|**Dönem**  <br>*İsteğe bağlı*|Veri noktalarının süre. Değeri null ya da mevcut değil, API dönemi otomatik olarak belirler.|sayı (çift)|
|**noktaları**  <br>*İsteğe bağlı*|Zaman serisi veri noktaları. Zaman damgası anomali sonucu eşleşecek şekilde artan veri sıralanmalıdır. Veriler düzgün şekilde sıralanır değil veya yinelenen zaman damgası API anomali noktaları doğru algılar, ancak giriş ile döndürülen noktaları da eşlenemedi. Böyle bir durumda bir uyarı iletisi yanıta eklenir.|< [işaret](#point) > dizi|


<a name="response"></a>
### <a name="response"></a>Yanıt

|Ad|Açıklama|Şema|
|---|---|---|
|**ExpectedValues**  <br>*İsteğe bağlı*|Tahmin edilen değer öğrenme modeli temel. Giriş noktaları zaman damgası tarafından artan düzende sıralanır, dizini dizinin beklenen değerini ve özgün değeri eşlemek için kullanılabilir.|dizi < sayı (çift) >|
|**IsAnomaly**  <br>*İsteğe bağlı*|Sonuç veri noktaları anomalileri olup olmadığı veya her iki yönde (ani artışlar veya Düşüşler) içinde değil. doğru anomali noktasıdır anlamına gelir, yanlış anomali olmayan noktasıdır anlamına gelir. Giriş noktaları zaman damgası tarafından artan düzende sıralanır, dizini dizinin beklenen değerini ve özgün değeri eşlemek için kullanılabilir.|< Boole > dizi|
|**IsAnomaly_Neg**  <br>*İsteğe bağlı*|Veri noktaları (dıps) olumsuz yönde anomalileri olup üzerinde sonucu. doğru anomali yönünü negatiftir anlamına gelir. Giriş noktaları zaman damgası tarafından artan düzende sıralanır, dizini dizinin beklenen değerini ve özgün değeri eşlemek için kullanılabilir.|< Boole > dizi|
|**IsAnomaly_Pos**  <br>*İsteğe bağlı*|Veri noktaları (ani) olumlu yönde anomalileri olup üzerinde sonucu. doğru anomali yönünü pozitiftir anlamına gelir. Giriş noktaları zaman damgası tarafından artan düzende sıralanır, dizini dizinin beklenen ve özgün değeri eşlemek için kullanılabilir.|< Boole > dizi|
|**LowerMargin**  <br>*İsteğe bağlı*|Veri noktası olan alt sınır hala normal olarak düşünülebilir (ExpectedValue - LowerMargin) belirler. Giriş noktaları zaman damgası tarafından artan düzende sıralanır, dizini dizinin beklenen değerini ve özgün değeri eşlemek için kullanılabilir.|dizi < sayı (çift) >|
|**Dönem**  <br>*İsteğe bağlı*|API anomali noktalarını algılamak için kullanılan süre.|sayı (kayan nokta)|
|**UpperMargin**  <br>*İsteğe bağlı*|Veri noktası üst sınırını hala normal olarak düşünülebilir ExpectedValue ve UpperMargin belirler. Giriş noktaları zaman damgası tarafından artan düzende sıralanır, dizini dizinin beklenen değerini ve özgün değeri eşlemek için kullanılabilir.|dizi < sayı (çift) >|
|**WarningText**  <br>*İsteğe bağlı*|Sağlanan giriş veri noktaları API gerekir ve veriler yine de API tarafından algılanabilir kural takip etmiyorsunuz, API verileri analiz etmek ve bu alan uyarı bilgileri ekleyin.|string|



