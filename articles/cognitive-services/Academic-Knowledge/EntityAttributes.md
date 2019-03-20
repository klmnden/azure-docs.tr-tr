---
title: Akademik graf varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si akademik grafikte ile kullanabileceğiniz varlık öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 485775660ecfdf2291365ab98c9188295ea2cbde
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58183501"
---
# <a name="entity-attributes"></a>Varlık öznitelikleri

Akademik graf 7 varlık türleri oluşur. Tüm varlıkları bir varlık kimliği ve bir varlık türü olacaktır.

## <a name="common-entity-attributes"></a>Genel varlık öznitelikleri
Ad    |Açıklama                |Type       | İşlemler
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

