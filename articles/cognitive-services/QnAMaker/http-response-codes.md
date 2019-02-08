---
title: API HTTP yanıt kodları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu API'lerinden hangi HTTP yanıt kodları döndürülen anlayın. Bu, hataları çözün yardımcı olur.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: d5b97f9c3b29732ba901f783ac04a3ada338ab69
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866449"
---
# <a name="qna-maker-api-http-response-codes"></a>Soru-cevap Oluşturucu API'si HTTP yanıt kodları
[Yönetim](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) ve tahmin API HTTP yanıt kodları döndürür. Yanıt iletilerini istek özgü bilgileri içerirken, HTTP yanıtı durum kodunun geneldir. 

### <a name="management"></a>Yönetim

|Kod|Açıklama|
|:--|--|
|2xx|Başarılı|
|400|isteğin parametreleri yanlış gerekli parametreler eksik, hatalı biçimlendirilmiş ya da çok büyük olduğu anlamına gelir|
|400|isteğin gövde JSON eksik, hatalı biçimlendirilmiş ya da çok büyük yani yanlış|
|401|Geçersiz anahtar|
|403|Yasak - sizin doğru izinlere sahip değilsiniz|
|404|KB yok|
