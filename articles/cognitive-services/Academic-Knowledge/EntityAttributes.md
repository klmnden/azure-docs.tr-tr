---
title: Akademik graf varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si akademik grafikte ile kullanabileceğiniz varlık öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a203fdf6562dabb1b9d6e8ab5bb8f46ff6d5dc27
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902799"
---
# <a name="entity-attributes"></a>Varlık öznitelikleri

Akademik graf 7 varlık türleri oluşur. Tüm varlıkları bir varlık kimliği ve bir varlık türü olacaktır.

## <a name="common-entity-attributes"></a>Genel varlık öznitelikleri
Ad    |Açıklama                |Tür       | İşlemler
------- | ------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                  |Int64      |Eşittir
Ty      |Varlık türü                |Sabit listesi   |Eşittir

## <a name="entity-type-enum"></a>Varlık türü sabit listesi
Ad                                                            |değer
----------------------------------------------------------------|-----
[Kağıt](PaperEntityAttributes.md)                               |0
[Yazar](AuthorEntityAttributes.md)                             |1
[Günlük](JournalEntityAttributes.md)                           |2
[Konferans serisi](JournalEntityAttributes.md)                 |3
[Konferans örneği](ConferenceInstanceEntityAttributes.md)    |4
[İlişkilendirme](AffiliationEntityAttributes.md)                   |5
[Çalışma alanı](FieldsOfStudyEntityAttributes.md)                      |6

