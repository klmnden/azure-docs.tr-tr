---
title: API HTTP yanıt kodları
titleSuffix: Azure
description: Hangi HTTP yanıt kodları LUIS yazma ve uç nokta API'leri döndürülen anlama
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 478727a4d3628fbe6a385e3beb28201c228f9e2d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078252"
---
# <a name="luis-api-http-response-codes"></a>LUIS API'si HTTP yanıt kodları
[Yazma](https://aka.ms/luis-authoring-apis) ve [uç nokta](https://aka.ms/luis-endpoint-apis) API'leri döndürülen HTTP yanıt kodları. Yanıt iletilerini istek özgü bilgileri içerirken, HTTP yanıtı durum kodunun geneldir. 

## <a name="common-status-codes"></a>Genel durum kodları
Aşağıdaki tabloda en yaygın HTTP yanıtı durum kodları için bazılarını listeler [yazma](https://aka.ms/luis-authoring-apis) ve [uç nokta](https://aka.ms/luis-endpoint-apis) API'leri:

|Kod|API|Açıklama|
|:--|--|--|
|400|Geliştirme, uç nokta|isteğin parametreleri yanlış gerekli parametreler eksik, hatalı biçimlendirilmiş ya da çok büyük olduğu anlamına gelir|
|400|Geliştirme, uç nokta|isteğin gövde JSON eksik, hatalı biçimlendirilmiş ya da çok büyük yani yanlış|
|401|Yazma|uç nokta abonelik anahtarı anahtar yazma yerine kullanılan|
|401|Geliştirme, uç nokta|Geçersiz, hatalı biçimlendirilmiş ya da boş anahtarı|
|401|Geliştirme, uç nokta| bölge anahtarı eşleşmiyor|
|401|Yazma|sahibi veya ortak çalışanı değil|
|401|Yazma|geçersiz çağrı sırasını, API|
|403|Geliştirme, uç nokta|Aylık toplam anahtar kota sınırı aşıldı|
|409|Uç Nokta|uygulama hala yükleniyor|
|410|Uç Nokta|Uygulama retrained ve yeniden yayımlanması gerekiyor|
|414|Uç Nokta|Sorgu en fazla karakter sınırını aşıyor|
|429|Geliştirme, uç nokta|Hız sınırı aşıldı (istek/saniye)|