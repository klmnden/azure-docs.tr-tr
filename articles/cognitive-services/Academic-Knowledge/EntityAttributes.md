---
title: Akademik graf varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si akademik grafikte ile kullanabileceğiniz varlık öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f227cc03578adcfbf73fec3ae8941045e8352513
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183008"
---
# <a name="entity-attributes"></a>Varlık öznitelikleri

Akademik graf 7 varlık türleri oluşur. Tüm varlıkları bir varlık kimliği ve bir varlık türü olacaktır.

## <a name="common-entity-attributes"></a>Genel varlık öznitelikleri
Name    |Açıklama                |Type       | İşlemler
------- | ------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                  |Int64      |Eşittir
Ty      |Varlık türü                |Sabit listesi   |Eşittir

## <a name="entity-type-enum"></a>Varlık türü sabit listesi
Name                                                            |değer
----------------------------------------------------------------|-----
[Kağıt](PaperEntityAttributes.md)                               |0
[Yazar](AuthorEntityAttributes.md)                             |1
[Günlük](JournalEntityAttributes.md)                           |2
[Konferans serisi](JournalEntityAttributes.md)                 |3
[Konferans örneği](ConferenceInstanceEntityAttributes.md)    |4
[İlişkilendirme](AffiliationEntityAttributes.md)                   |5
[Çalışma alanı](FieldsOfStudyEntityAttributes.md)                      |6

