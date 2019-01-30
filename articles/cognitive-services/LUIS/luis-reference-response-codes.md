---
title: API HTTP yanıt kodları
titleSuffix: Azure
description: Hangi HTTP yanıt kodları LUIS yazma ve uç nokta API'leri döndürülen anlama
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: fd0d8cc492a8a35d66aa4b20b792bf317b9e8c11
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55218861"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Yaygın API yanıt kodları ve anlamları

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

## <a name="next-steps"></a>Sonraki adımlar

* REST API [yazma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) ve [uç nokta](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) belgeleri
