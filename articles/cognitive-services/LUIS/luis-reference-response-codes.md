---
title: Dil anlama (HALUK) API HTTP yanıt kodlarını - Azure | Microsoft Docs
titleSuffix: Azure
description: Hangi HTTP yanıt kodları HALUK yazma ve uç nokta API döndürülen anlama
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 04/16/2018
ms.author: v-geberr
ms.openlocfilehash: 9c7381d9dc2ecf302c85c6b4f1f24b24a28d9ebe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352703"
---
# <a name="luis-api-http-response-codes"></a>HALUK API HTTP yanıt kodları
[Yazma](https://aka.ms/luis-authoring-apis) ve [endpoint](https://aka.ms/luis-endpoint-apis) API'leri HTTP yanıt kodlarını döndürür. HTTP yanıtı durum kodu için bir istek özgü bilgileri yanıt iletilerini içerir, ancak genel. 

## <a name="common-status-codes"></a>Genel durum kodları
Aşağıdaki tabloda bazı yaygın HTTP yanıtı durum kodları için listeler [yazma](https://aka.ms/luis-authoring-apis) ve [endpoint](https://aka.ms/luis-endpoint-apis) API'leri:

|Kod|API|Açıklama|
|:--|--|--|
|400|Geliştirme, uç nokta|isteğin parametreleri yanlış gerekli parametreler eksik, hatalı biçimlendirilmiş ya da çok büyük olduğu anlamına gelir|
|400|Geliştirme, uç nokta|isteğin gövdesi JSON eksik, hatalı biçimlendirilmiş ya da çok büyük deyişle yanlış|
|401|Yazma|uç nokta abonelik anahtarı, anahtar yazma yerine kullanılan|
|401|Geliştirme, uç nokta|Geçersiz, hatalı biçimlendirilmiş ya da boş anahtarı|
|401|Geliştirme, uç nokta| anahtar bölge eşleşmiyor|
|401|Yazma|sahibi veya ortak çalışanı yok|
|401|Yazma|Geçersiz sırasını API çağrıları|
|403|Geliştirme, uç nokta|Aylık toplam anahtar kota sınırı aşıldı|
|409|Uç Nokta|Uygulama yüklemeye devam ediyor|
|410|Uç Nokta|uygulamanın retrained ve yeniden yayımlanması gerekir|
|414|Uç Nokta|Sorgu maksimum karakter sınırını aşıyor|
|429|Geliştirme, uç nokta|(İstek/saniye) hız sınırı aşıldı|