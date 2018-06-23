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
ms.openlocfilehash: ff36202b67f6262b7ba67fe48ef37f2b656b91fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353351"
---
<a name="definitions"></a>
## <a name="definitions"></a>Tanımlar

<a name="point"></a>
### <a name="point"></a>Noktası

|Ad|Açıklama|Şema|
|---|---|---|
|**Zaman damgası**  <br>*İsteğe bağlı*|Veri noktası için zaman damgası. Bunu hizalar gece ve bir UTC tarih saat dize, örneğin, kullanım 2017 emin olun-08-01T00:00:00Z.|dize (tarih)|
|**Değer**  <br>*İsteğe bağlı*|Bir veri ölçü birimi değeri.|sayı (double)|


<a name="request"></a>
### <a name="request"></a>İstek

|Ad|Açıklama|Şema|
|---|---|---|
|**Süresi**  <br>*İsteğe bağlı*|Veri noktalarının süre. Değeri null ya da mevcut değil ise API dönemi otomatik olarak belirleyin.|sayı (double)|
|**Noktaları**  <br>*İsteğe bağlı*|Zaman serisi veri noktası. Veri anomali sonuç eşleşecek şekilde artan zaman damgalarıyla sıralanmış. Verileri doğru sıralı değil ya da yinelenen zaman damgası API anomali noktaları doğru algılar ancak girişle döndürülen noktaları iyi eşlenemedi. Böyle bir durumda bir uyarı iletisi yanıta eklenir.|< [noktası](#point) > dizisi|


<a name="response"></a>
### <a name="response"></a>Yanıt

|Ad|Açıklama|Şema|
|---|---|---|
|**ExpectedValues**  <br>*İsteğe bağlı*|Bilgi edinerek tahmin edilen bir değer model tabanlı. Girdi veri noktası artan zaman damgası tarafından sıraladıysanız dizinin dizini beklenen değer ve özgün değeri eşlemek için kullanılabilir.|< sayı (double) > dizisi|
|**IsAnomaly**  <br>*İsteğe bağlı*|Sonuç veri noktalarını bozukluklar olup alan veya her iki yönde (ani veya dıps). doğru anomali noktasıdır anlamına gelir, yanlış anomali olmayan noktasıdır anlamına gelir. Girdi veri noktası artan zaman damgası tarafından sıraladıysanız dizinin dizini beklenen değer ve özgün değeri eşlemek için kullanılabilir.|< Boole > dizisi|
|**IsAnomaly_Neg**  <br>*İsteğe bağlı*|Veri noktaları (dıps) olumsuz yönde bozukluklar olup sonucu. TRUE anomali yönünü negatif olduğu anlamına gelir. Girdi veri noktası artan zaman damgası tarafından sıraladıysanız dizinin dizini beklenen değer ve özgün değeri eşlemek için kullanılabilir.|< Boole > dizisi|
|**IsAnomaly_Pos**  <br>*İsteğe bağlı*|Veri noktaları (ani) pozitif yönde bozukluklar olup sonucu. TRUE anomali yönünü pozitif olduğu anlamına gelir. Girdi veri noktası artan zaman damgası tarafından sıraladıysanız dizinin dizini beklenen ve özgün değeri eşlemek için kullanılabilir.|< Boole > dizisi|
|**LowerMargin**  <br>*İsteğe bağlı*|Veri noktası olan alt sınır hala normal olarak zorlayıcı (ExpectedValue - LowerMargin) belirler. Girdi veri noktası artan zaman damgası tarafından sıraladıysanız dizinin dizini beklenen değer ve özgün değeri eşlemek için kullanılabilir.|< sayı (double) > dizisi|
|**Süresi**  <br>*İsteğe bağlı*|API anomali noktalarını algılamak için kullanılan süre.|sayı (kayan nokta)|
|**UpperMargin**  <br>*İsteğe bağlı*|Veri noktası üst sınır hala normal olarak zorlayıcı ExpectedValue ve UpperMargin toplamı belirler. Girdi veri noktası artan zaman damgası tarafından sıraladıysanız dizinin dizini beklenen değer ve özgün değeri eşlemek için kullanılabilir.|< sayı (double) > dizisi|
|**WarningText**  <br>*İsteğe bağlı*|Sağlanan girdi veri noktası API gerektirir ve veri API'si tarafından hala algılanabilir kuralı değil izliyorsanız, API verileri çözümlemek ve bu alanda uyarı bilgilerini ekleyin.|dize|



