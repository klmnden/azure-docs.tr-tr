---
title: Akademik bilgi API'si akademik grafik varlık öznitelikleri | Microsoft Docs
description: Akademik bilgi API'si akademik grafikte ile birlikte kullanabileceğiniz varlık öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f771969c3c6f05e5d2c99b1fbf593ae039ccb12c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351347"
---
# <a name="entity-attributes"></a>Varlık öznitelikleri

Akademik grafik 7 varlık türlerini oluşur. Tüm varlıklar, varlık kimliği, bir varlık türü olacaktır.

## <a name="common-entity-attributes"></a>Ortak varlık öznitelikleri
Ad    |Açıklama                |Tür       | İşlemler
------- | ------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                  |Int64      |Eşittir
Ty      |Varlık türü                |Enum   |Eşittir

## <a name="entity-type-enum"></a>Varlık türü enum
Ad                                                            |değer
----------------------------------------------------------------|-----
[Kağıt](PaperEntityAttributes.md)                               |0
[Yazar](AuthorEntityAttributes.md)                             |1
[Günlük](JournalEntityAttributes.md)                           |2
[Konferans serisi](JournalEntityAttributes.md)                 |3
[Konferans örneği](ConferenceInstanceEntityAttributes.md)    |4
[İlişkilendirme](AffiliationEntityAttributes.md)                   |5
[Çalışma alanı](FieldsOfStudyEntityAttributes.md)                      |6

