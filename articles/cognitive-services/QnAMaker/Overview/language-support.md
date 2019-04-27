---
title: Dil desteği - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Bilgi Bankası dili otomatik-extract sorular ve yanıtlar kaynaklardan yanı sıra, soru-cevap Oluşturucu, kullanıcı sorgularına yanıt sağlar sonuçlarının ilgi soru-cevap Oluşturucu'nın yeteneğini etkiler. Kültür, doğal Bilgi Bankası'nda soru-cevap Oluşturucu tarafından desteklenen dillerin listesi. Aynı Bilgi Bankası dillerde karıştırmayın.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: a6304a93b1409cff871ed1c4c1d7e66d6c8c6f53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60736719"
---
# <a name="language-support-for-qna-maker"></a>Soru-cevap Oluşturucu için dil desteği

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

## <a name="primary-language-detection"></a>Birincil dili algılama

Algılama için kullanılan birincil dil, soru-cevap Oluşturucu kaynak ve ilk Bilgi Bankası'na ilk belgeye veya URL'ye eklendiğinde bu kaynak üzerinde oluşturulan tüm bilgi bankaları için ayarlanır. Dil değiştirilemez. 

Birden fazla dili desteklemeye kullanıcı planları, her dil için yeni bir soru-cevap Oluşturucu kaynağı olması gerekir. Bilgi edinmek için nasıl [dil tabanlı soru-cevap Oluşturucu Bilgi Bankası oluşturma](../how-to/language-knowledge-base.md).  

Birincil dili aşağıdaki adımları doğrulayın:

1. [Azure Portal](http://portal.azure.com) oturum açın.  
1. Arayın ve soru-cevap Oluşturucu kaynağınızın bir parçası olarak oluşturulan Azure Search kaynağı seçin. Azure Search kaynak adı, soru-cevap Oluşturucu kaynakla aynı ada sahip başlar ve türü olacak **arama hizmetinizi**. 
1. Gelen **genel bakış** seçin arama kaynak sayfası **dizinleri**. 
1. Seçin **testkb** dizini.
1. Seçin **alanları** sekmesi. 
1. Görünüm **Çözümleyicisi** sütunu için **sorular** ve **yanıt** alanları. 


## <a name="query-matching-and-relevance"></a>Sorguyla eşleşen ve ilgi düzeyi
Soru-cevap Oluşturucu bağlıdır [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) sonuçları sağlamak için Azure Search'te. Özel yeniden sıralama özellikleri kullanılabilir tr - için * daha iyi uygunluğu sağlayan diller.

Azure arama özellikleri, desteklenen diller için nominal açık olduğunda, soru-cevap Oluşturucu, Azure arama sonuçlarının üstünde yer alan bir ek derecelendiricisini sahiptir. Bazı özel anlam ve word tabanlı özellikler tr-kullandığımız derecelendiricisini Bu modelde, *, henüz diğer diller için kullanılabilir değildir. Soru-cevap Oluşturucu'nın derecelendiricisini, iç çalışan bir parçası olarak Biz bu özellikler kullanılabilir, değişiklik yapmayın. 

Soru-cevap Oluşturucu [otomatik olarak Bilgi Bankası dilini algılar](#primary-language-detection) oluşturma sırasında ve Çözümleyicisi uygun şekilde ayarlar. Aşağıdaki dillerde bilgi bankalarından oluşturabilirsiniz. 

|Desteklenen diller|
|-----|
|Arapça|
|Ermenice|
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
