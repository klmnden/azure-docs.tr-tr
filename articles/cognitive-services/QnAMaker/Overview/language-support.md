---
title: Dil desteği - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Kültür, doğal Bilgi Bankası'nda soru-cevap Oluşturucu tarafından desteklenen dillerin listesi. Aynı Bilgi Bankası dillerde karıştırmayın.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/09/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: d51af0f59ffc0189ddaba4f8197aca79becf779e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106371"
---
# <a name="language-and-region-support-for-qna-maker"></a>Soru-cevap Oluşturucu dil ve bölge desteği

Bilgi Bankası dili otomatik ayıklama soruların soru-cevap Oluşturucu'nın özelliğini etkiler ve gelen yanıtları [kaynakları](../Concepts/data-sources-supported.md), soru-cevap Oluşturucu sağlayan kullanıcı sorgularına yanıt sonuçlarının ilgi yanı sıra.

## <a name="auto-extraction"></a>Otomatik ayıklama
Soru-cevap Oluşturucu, herhangi bir dil sayfanın soru/yanıt ayıklama destekler, ancak soru-cevap Oluşturucu soruları tanımlamak için anahtar sözcükler kullandığından ayıklama verimliliğini aşağıdaki diller için çok daha yüksektir.

|Desteklenen diller| Yerel Ayar|
|-----|----|
|Türkçe|tr-*|
|Fransızca |FR-*|
|İtalyanca|BT-*|
|Almanca |de-*|
|İspanyolca |ES-*|

## <a name="query-matching-and-relevance"></a>Sorguyla eşleşen ve ilgi düzeyi
Soru-cevap Oluşturucu bağlıdır [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) sonuçları sağlamak için Azure Search'te. Özel yeniden sıralama özellikleri kullanılabilir tr - için * daha iyi uygunluğu sağlayan diller.

Azure arama özellikleri, desteklenen diller için nominal açık olduğunda, soru-cevap Oluşturucu, Azure arama sonuçlarının üstünde yer alan bir ek derecelendiricisini sahiptir. Derecelendiricisini Bu modelde bazı özel anlam kullanır ve word tabanlı özellikleri tr-*, henüz diğer diller için kullanılabilir değildir. İç çalışma derecelendiricisini'nın bir parçası olarak Biz bu kullanılabilir, değişiklik yapmayın. 

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
|Felemenkçe|
|Türkçe|
|Estonca|
|Fince|
|Fransızca |
|Galiçya dili|
|Almanca |
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
|Korece|
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
|İspanyolca |
|İsveççe|
|Tamil dili|
|Telugu dili|
|Tay Dili|
|Türkçe|
|Ukrayna dili|
|Urduca|
|Vietnam dili|
