---
title: Bilgi Bankası - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Bilgi Bankası hakkında
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 5dfb96f454bf5ce3f030ebfc6a97482fcfc9469b
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760292"
---
# <a name="knowledge-base"></a>Bilgi bankası

Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.

## <a name="key-knowledge-base-concepts"></a>Bilgi Bankası anahtar kavramlar

* **Sorular** -soru kullanıcı sorgusu en iyi temsil eden metin içeriyor. 
* **Yanıtlar** -kullanıcı sorgusu ile ilişkili soru eşleştiğinde, döndürülen yanıt cevaptır.  
* **Meta veri** -meta verileri bir soru-cevap çifti ile ilişkili etiket ve anahtar-değer çiftleri olarak temsil edilir. Meta veri, soru-cevap çiftlerini filtrelemek ve eşleşen hangi sorgu üzerinde gerçekleştirilen kümesini sınırlamak için kullanılır.

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