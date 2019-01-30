---
title: Bilgi Bankası - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/18/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: d036b93bc043f74dedae24455ca0a43cd808a936
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227123"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Temel bir soru-cevap Oluşturucu bilgi nedir?

Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.

## <a name="key-knowledge-base-concepts"></a>Bilgi Bankası anahtar kavramlar

* **Sorular** -soru kullanıcı sorgusu en iyi temsil eden metin içeriyor. 
* **Yanıtlar** -kullanıcı sorgusu ile ilişkili soru eşleştiğinde, döndürülen yanıt cevaptır.  
* **Meta veri** -meta verileri bir soru-cevap çifti ile ilişkili etiket ve anahtar-değer çiftleri olarak temsil edilir. Meta veri etiketleri, soru-cevap çiftlerini filtrelemek ve eşleşen hangi sorgu üzerinde gerçekleştirilen kümesini sınırlamak için kullanılır.

Sayısal bir soru-cevap kimliği tarafından temsil edilen bir tek soru-cevap, soru (diğer Sorular) tümünü tek bir yanıt harita birden çok çeşitlemesi vardır. Ayrıca, her bir çifti kendisiyle ilişkilendirilmiş birden fazla meta veri alanları olabilir.

![Soru-cevap Oluşturucu bilgi bankalarından](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Bilgi Bankası içerik biçimi

Bilgi Bankası zengin içerik içe alma, içerik markdown biçimine dönüştürmek soru-cevap Oluşturucu çalışır. Okuma [bu](https://aka.ms/qnamaker-docs-markdown-support) markdown anlamak için blog çoğu sohbet istemciler tarafından anlaşılır biçimlendirir.

Meta veri alanları, anahtar-değer çiftleri virgül ile ayrılmış oluşur **(ürün: öğütücü)**. Hem anahtar hem de değer salt metin olmalıdır. Meta verileri anahtar boşluk içermemelidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
