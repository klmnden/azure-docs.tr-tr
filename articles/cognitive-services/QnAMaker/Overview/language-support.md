---
title: Dil desteği - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Doğal soru-cevap Oluşturucu tarafından desteklenen dillerin listesi.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 1a61d8f4008b0183ab5ddb51332d887217f52f48
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435602"
---
# <a name="language-and-region-support-for-qna-maker"></a>Soru-cevap Oluşturucu dil ve bölge desteği

Bilgi Bankası dili otomatik ayıklama soruların soru-cevap Oluşturucu'nın özelliğini etkiler ve gelen yanıtları [kaynakları](../Concepts/data-sources-supported.md), soru-cevap Oluşturucu sağlayan kullanıcı sorgularına yanıt sonuçlarının ilgi yanı sıra.

## <a name="auto-extraction"></a>Otomatik ayıklama
Soru-cevap Oluşturucu, herhangi bir dil sayfanın soru/yanıt ayıklama destekler, ancak soru-cevap Oluşturucu soruları tanımlamak için anahtar sözcükler kullandığından ayıklama verimliliğini aşağıdaki diller için çok daha yüksektir.

|Desteklenen diller| Yerel Ayar|
|-----|----|
|Türkçe|tr-*|
|Fransızca|FR-*|
|İtalyanca|BT-*|
|Almanca|de-*|
|İspanyolca|ES-*|

## <a name="query-matching-and-relevance"></a>Sorguyla eşleşen ve ilgi düzeyi
Soru-cevap Oluşturucu bağlıdır [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) sonuçları sağlamak için Azure Search'te. Özel yeniden sıralama özellikleri kullanılabilir tr - için * daha iyi uygunluğu sağlayan diller.

Soru-cevap Oluşturucu, otomatik olarak Bilgi Bankası dili oluşturma sırasında algılar ve Çözümleyicisi uygun şekilde ayarlar. Aşağıdaki dillerde bilgi bankalarından oluşturabilirsiniz. Okuma [bu](../How-To/language-knowledge-base.md) soru-cevap Oluşturucu dillerin nasıl işlediği hakkında daha fazla ayrıntı için.


> [!Tip]
> Dil Çözümleyicileri kez ayarlandıktan sonra değiştirilemez. Ayrıca, bir dil Çözümleyicisi bilgi bankalarından tüm uygulandığı bir [soru-cevap Oluşturucu hizmetini](../How-To/set-up-qnamaker-service-azure.md). Farklı bir dilde bilgi bankalarından olmasını planlıyorsanız, bunları ayrı soru-cevap Oluşturucu hizmetler oluşturmalısınız.

|Desteklenen diller|
|-----|
|Arapça|
|Ermenice|, 
Bengali|
|Bask dili|
|Bulgarca|
|Katalanca|
|Chinese_Simplified|
|Chinese_Traditional|
|Hırvatça|
|Çekçe|
|Danca|
|Hollanda dili|
|Türkçe|
|Estonca|
|Fince|
|Fransızca|
|Galiçya dili|
|Almanca|
|Yunanca|
|Gucerat dili|
|İbranice|
|Hintçe|
|Macarca|
|İzlanda dili|
|Endonezya dili|
|İrlanda dili|
|İtalyanca|
|Japonca|
|Kannada dili|
|Kore dili|
|Letonca|
|Litvanca|
|Malayalam dili|
|Malay dili|
|Norveççe|
|Lehçe|
|Portekizce|
|Pencap dili|
|Rumence|
|Rusça|
|Serbian_Cyrillic|
|Serbian_Latin|
|Slovakça|
|Slovence|
|İspanyolca|
|İsveç dili|
|Tamil dili|
|Telugu dili|
|Tay Dili|
|Türkçe|
|Ukrayna dili|
|Urduca|
|Vietnam dili|
