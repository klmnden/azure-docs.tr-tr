---
title: API HTTP yanıt kodları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu API'lerinden hangi HTTP yanıt kodları döndürülen anlayın. Bu, hataları çözün yardımcı olur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 0416b282a869536b0cea591ba39d7a3804697bac
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446532"
---
# <a name="qna-maker-api-http-response-codes"></a>Soru-cevap Oluşturucu API'si HTTP yanıt kodları
[Yönetim](https://go.microsoft.com/fwlink/?linkid=2092179) ve tahmin API HTTP yanıt kodları döndürür. Yanıt iletilerini istek özgü bilgileri içerirken, HTTP yanıtı durum kodunun geneldir. 

### <a name="management"></a>Yönetim

|Kod|Açıklama|
|:--|--|
|2xx|Başarılı|
|400|isteğin parametreleri yanlış gerekli parametreler eksik, hatalı biçimlendirilmiş ya da çok büyük olduğu anlamına gelir|
|400|isteğin gövde JSON eksik, hatalı biçimlendirilmiş ya da çok büyük yani yanlış|
|401|Geçersiz anahtar|
|403|Yasak - sizin doğru izinlere sahip değilsiniz|
|404|KB yok|
|410|Bu API kullanım dışıdır ve artık kullanılamıyor|
