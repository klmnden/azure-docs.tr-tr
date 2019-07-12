---
title: Önceden derlenmiş etki alanı başvurusu
titleSuffix: Azure
description: Önceden oluşturulmuş koleksiyonları hedefleri ve varlıkların gelen Language Understanding Intelligent Services (LUIS) önceden oluşturulmuş etki alanları için başvuru.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b615185472ede25fd6ceef9cc3e7325375e16ac7
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846673"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>LUIS uygulamanızı için önceden oluşturulmuş etki alanı başvurusu
Bu başvuru, hakkında bilgi sağlar. [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md), önceden oluşturulmuş koleksiyon hedefleri ve LUIS sunan varlıkların olduğu.

[Özel etki alanları](luis-how-to-start-new-app.md), aksine, hiçbir hedefleri ve modelleri başlayın. Herhangi bir önceden oluşturulmuş etki alanı hedefleri ve varlıklar için özel bir model ekleyebilirsiniz.

# <a name="supported-domains-across-cultures"></a>Desteklenen kültürler alanlarında

Yalnızca desteklenen kültürü İngilizce. 

<!--

The table below summarizes the currently supported domains. Support for English is usually more complete than others.


| Entity Type       | EN-US      | ZH-CN   | DE    | FR     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| Calendar    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -      | -    | -    | -     | -  |
| Communication   | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Email           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| HomeAutomation           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Notes      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Places    | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| RestaurantReservation   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo_IPA        | ✓    | ✓       | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Utilities          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Weather        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Web    | ✓    | -        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
||||||||||||| 

-->

<br><br>

|varlık türü|description|
|--|--|
|Takvim|Takvim olduğundan, hiçbir şey, kişisel toplantılar ve randevuları hakkında _değil_ genel olaylar (örneğin, dünya Kupası zamanlamalar, Seattle olay takvimleri) veya genel takvimleri (hangi gününde bugün olduğu gibi hangi fall başlıyor, işçilik gün olduğunda).|
|İletişim|Çağrısı, istekleri metinleri ya da anlık ileti bulun ve kişiler ekleyip (genellikle Giden) çeşitli diğer iletişimle ilgili istekleri gönderin. İlgili kişi adı yalnızca sorgularını iletişimi etki alanına ait değil.|
|Email|E-posta iletişimi etki alanının alt etki alanı var. Esas olarak, e-posta üzerinden ileti göndermek ve almak için istekleri içerir.|
|HomeAutomation|Hedefleri ve Akıllı Giriş cihazları denetlemek için ilgili varlıkları HomeAutomation etki alanı sağlar. Çoğunlukla ışıkları ve yapının klimaları ilgili denetim komutu destekler ancak bazı diğer elektrikli aletler için genelleştirme yeteneklerini sahiptir.|
|Notlar|Amaç ve varlıkları not oluşturma ve kullanıcılar için öğelere yazmak için Not etki alanı sağlar.|
|Yerler|İşletmeler, kurumlar, Restoran, ortak alanları ve adresleri basamak içerir. Etki alanı bulma ve ortak bir yerde işletim saat ve uzaklık konumu gibi bilgiler hakkında soran bir yerde destekler.|
|RestaurantReservation|Restoran ayırma etki alanı hedefleri restoranlar için ayırmaları işlemek için destekler.|
|ToDo|ToDo etki alanı kullanıcılar ekleme, işaretleyin ve kendi Yapılacaklar öğelerini silmek için görev listesi türleri sağlar.|
|Altyapı Hizmetleri|Yardımcı programları etki alanıdır genel bir etki alanı tüm LUIS arasında ortak hedefleri ve konuşma fark senaryolarda içeren önceden oluşturulmuş modelleri.|
|Hava durumu|Hava durumu ve önerileri yer ve zaman ile denetimi veya hava koşulları tarafından denetlenmesi hava durumu etki alanı odaklanır.|
|Web|Web etki alanı, bir Web sitesi için arama için amaç ve varlıkları sağlar.|
