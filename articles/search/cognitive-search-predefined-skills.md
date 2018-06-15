---
title: Önceden tanımlanmış veri ayıklama, doğal dil, görüntü yetenekleri (Azure Search) işleme | Microsoft Docs
description: Veri ayıklama, doğal dil görüntü bilişsel becerileri işleme eklemeniz semantik ve yapısı bir Azure arama ardışık ham içeriği.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 870cf9629c7af8faee0ce5709199b64910b27ffb
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790961"
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>İçerik iyileştirmesini (Azure Search) için önceden tanımlanmış yetenekler

Bu makalede, Bilişsel arama ile sağlanan bilişsel yetenekleri hakkında bilgi edinin. A *bilişsel yetenek* herhangi bir şekilde içeriği dönüştürür bir işlemdir. Genellikle, bu verileri ayıklar veya yapısı oluşturur ve bu nedenle bizim anlama giriş verilerinin güçlendirir bir bileşendir. Neredeyse her zaman, çıktı metin tabanlı şeklindedir. A *skillset* iyileştirmesini ardışık düzen tanımlamak becerileri koleksiyonudur. 

## <a name="predefined-skills"></a>Önceden tanımlanmış yetenekleri

Birkaç becerileri ne bunlar kullanabilir veya üretmek esnektir. Genel olarak, çoğu becerileri kendi eğitim verilerini kullanarak modeli eğitmek edemiyor demektir önceden eğitilmiş modeller temel alır. Özel bir yetenek oluşturma ile ilgili yönergeler için bkz: [özel bir arabirim tanımlamak nasıl](cognitive-search-custom-skill-interface.md) ve [örnek: özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md). Aşağıdaki tabloda, sıralar ve Microsoft tarafından sağlanan yetenekleri açıklanmaktadır. 

| Yetenek | Açıklama |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Bu yetenek pretrained modeli terim yerleştirme, dil kuralları, diğer koşulları bir yerde konumlandırıldığında ve nasıl olağan dışı kaynak verileri içinde bir terimdir göre önemli tümcecikleri algılamak için kullanır. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Hangi dili Algıla pretrained modeldir Bu yetenek (belge her bir dil kimliği) kullanır. Birden çok dil aynı metin Segmentte kullanıldığında, çıktı daha kullanılan dilin LCID şeklindedir.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Koleksiyona tek bir alan alanlar metinden birleştirir.  |
| [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md) | Bu yetenek, varlıklar için kategoriler sabit kümesi oluşturmak için bir pretrained modeli kullanır: kişi, konum, kuruluş. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Bu yetenek, bir kayıt kayıt temelinde olumlu veya olumsuz düşünceleri Puanlama amacıyla bir pretrained modeli kullanır. Puan 0 ile 1 arasında ' dir. Dilden bağımsız puanları düşünceleri algılanamıyor ve metin için dilden bağımsız olarak kabul edilir için null durum oluşur.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Böylece zenginleştirmek veya içerik artımlı olarak büyütmek metin sayfalarına böler. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Bu yetenek, görüntü içeriğini tanımlamak ve bir metin tanımı oluşturmak için bir görüntü algılama algoritması kullanır. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optik karakter tanıma. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | MAPS çıkış karmaşık türü (tam adı, çok satırlı adresi veya soyadı ve bir kişisel kimlik birleşimi için kullanılabilir bir çok parçalı veri türü.) |

## <a name="see-also"></a>Ayrıca bkz.

+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Özel becerileri arabirim tanımı](cognitive-search-custom-skill-interface.md)
+ [Öğretici: ile zenginleştirilmiş ile kavrama arama dizini oluşturma](cognitive-search-tutorial-blob.md)