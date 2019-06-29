---
title: Bilgi Bankası - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b9562a1686c4de4f4e2ef57a7d91bbf18dce63ef
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447589"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Temel bir soru-cevap Oluşturucu bilgi nedir?

Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.

## <a name="key-knowledge-base-concepts"></a>Bilgi Bankası anahtar kavramlar

* **Sorular** -soru kullanıcı sorgusu en iyi temsil eden metin içeriyor. 
* **Yanıtlar** -kullanıcı sorgusu ile ilişkili soru eşleştiğinde, döndürülen yanıt cevaptır.  
* **Meta veri** -meta verileri bir soru-cevap çifti ile ilişkili etiket ve anahtar-değer çiftleri olarak temsil edilir. Meta veri etiketleri, soru-cevap çiftlerini filtrelemek ve eşleşen hangi sorgu üzerinde gerçekleştirilen kümesini sınırlamak için kullanılır.

Sayısal bir soru-cevap kimliği tarafından temsil edilen bir tek soru-cevap, soru (diğer Sorular) tümünü tek bir yanıt harita birden çok çeşitlemesi vardır. Ayrıca, her bir çifti kendisiyle ilişkilendirilmiş birden fazla meta veri alanları olabilir: bir anahtarı ve tek bir değer.

![Soru-cevap Oluşturucu bilgi bankalarından](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Bilgi Bankası içerik biçimi

Bilgi Bankası zengin içerik içe alma, içerik markdown biçimine dönüştürmek soru-cevap Oluşturucu çalışır. Okuma [bu](https://aka.ms/qnamaker-docs-markdown-support) markdown anlamak için blog çoğu sohbet istemciler tarafından anlaşılır biçimlendirir.

Meta veri alanları, anahtar-değer çiftleri virgül ile ayrılmış oluşur **(ürün: öğütücü)** . Hem anahtar hem de değer salt metin olmalıdır. Meta verileri anahtar boşluk içermemelidir. Meta verileri anahtar başına yalnızca bir değer destekler.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Soru-cevap Oluşturucu en iyi cevabı seçmek için bir kullanıcı sorgu nasıl işler?

Eğitilen ve [yayımlanan](/quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) soru-cevap Oluşturucu Bilgi Bankası bir bot veya başka bir istemci uygulama bir kullanıcı sorgusu alır [GenerateAnswer API](/how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api). Kullanıcı sorgusu alındığında Aşağıdaki diyagramda işlemi gösterilmektedir.

![Kullanıcı sorgusu için sıralama işlemi](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

İşlem aşağıdaki tabloda açıklanmıştır:

|Adım|Amaç|
|--|--|
|1|İstemci uygulama için kullanıcı sorgusu gönderir [GenerateAnswer API](/how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api).|
|2|Soru-cevap Oluşturucu, dil algılama ve spellers Sözcük ayırıcılar sahip kullanıcı sorgusu ön işleme.|
|3|Kullanıcı sorgusu için en iyi arama sonuçları değiştirmek için bu ön işleme alınır.|
|4|Bu değiştirilen sorguyu Azure Search dizini için gönderilen alma `top` sonuç sayısı. Doğru yanıtı bu sonuçları değilse, değeri artırmak `top` biraz. Genel olarak 10 değerini `top` %90 sorguların çalışır.|
|5|Soru-cevap Oluşturucu, kullanıcı sorgusu için getirilen Azure arama sonuçlarını doğruluğunu belirlemek için gelişmiş özellik kazandırma sayesinde geçerlidir. |
|6|Özellik puan, Azure arama sonuçları için 5. adım eğitim derecelendiricisini uygulama modeli kullanır.|
|7|Yeni sonuçlarını istemci uygulamaya dereceli sırayla döndürülür.|
|||

Kullanılan özellikleri içerir ancak word düzeyinde semantiği, terim düzeyinde bir gövde ve benzerlik ve ilgi düzeyi arasında iki metin dizesini belirlemek için ayrıntılı öğrenilen anlam modelleri'ne önem sınırlı değildir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
