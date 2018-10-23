---
title: Karakter sayısı - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Nasıl Translator Text API karakter sayar.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: 8e04158d34765b8a077fc56f2108ea6d7b4b03a6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649203"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Translator Text API karakter nasıl sayılır?

Translator metin çevirisi API'si, her karakter giriş sayar. Karakterleri Unicode anlamında, bayt sayısını değil. Unicode sayısı iki karakter olarak temsilciler. Boşluk ve biçimlendirme karakter olarak sayılır. Yanıt uzunluğu önemli değildir.

Algılama ve BreakSentence yöntemlerine yapılan çağrıda karakter tüketimini sayılmaz. Ancak, algılama ve BreakSentence yöntemlere yapılan çağrılar sayılan diğer işlevleri kullanımını kabul edilebilir bir oranda olduğunu bekliyoruz. Microsoft algılama ve BreakSentence sayımı Başlat hakkını saklı tutar.

Karakter sayıları hakkında daha fazla bilgi yer [Microsoft Translator SSS](https://www.microsoft.com/en-us/translator/faq.aspx).
