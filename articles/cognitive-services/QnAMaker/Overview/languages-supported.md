---
title: Desteklenen diller - soru-cevap Oluşturucu
titlesuffix: Azure Cognitive Services
description: Bilgi Bankası dili otomatik-extract sorular ve yanıtlar kaynaklardan yanı sıra, soru-cevap Oluşturucu, kullanıcı sorgularına yanıt sağlar sonuçlarının ilgi soru-cevap Oluşturucu'nın yeteneğini etkiler.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: ee04733064ec4e3d131b800fe1f18b27e5127fe8
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45572949"
---
# <a name="supported-languages"></a>Desteklenen diller

Bilgi Bankası dili otomatik ayıklama soruların soru-cevap Oluşturucu'nın özelliğini etkiler ve gelen yanıtları [kaynakları](../Concepts/data-sources-supported.md), soru-cevap Oluşturucu sağlayan kullanıcı sorgularına yanıt sonuçlarının ilgi yanı sıra.

## <a name="auto-extraction"></a>Otomatik ayıklama
Soru-cevap Oluşturucu, herhangi bir dil sayfanın soru/yanıt ayıklama destekler, ancak soru-cevap Oluşturucu soruları tanımlamak için anahtar sözcükler kullandığından ayıklama verimliliğini aşağıdaki diller için çok daha yüksektir.

|Desteklenen diller| Yerel ayar|
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
