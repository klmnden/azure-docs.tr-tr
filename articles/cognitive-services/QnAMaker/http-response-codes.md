---
title: API HTTP yanıt kodları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu API'lerinden hangi HTTP yanıt kodları döndürülen anlayın. Bu, hataları çözün yardımcı olur.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9eb5f1aefc8a36727e837af79f553108f624fc70
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207981"
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
