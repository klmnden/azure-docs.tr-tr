---
title: Microsoft Translator metin API'si karakter sayıları | Microsoft Docs
description: Nasıl Microsoft Translator metin çevirisi API'si karakter sayar.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: 1b4987509c17e4064d7c54608395e272efa8de3b
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41987590"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Microsoft Translator metin çevirisi API'si karakter nasıl sayılır?

Microsoft Translator, girişin her karakteri sayar. Karakterleri Unicode anlamında, bayt sayısını değil. Unicode sayısı iki karakter olarak temsilciler. Boşluk ve biçimlendirme karakter olarak sayılır. Yanıt uzunluğu önemli değildir.

Algılama ve BreakSentence yöntemlerine yapılan çağrıda karakter tüketimini sayılmaz. Ancak, algılama ve BreakSentence yöntemlere yapılan çağrılar sayılan diğer işlevleri kullanımını kabul edilebilir bir oranda olduğunu bekliyoruz. Microsoft algılama ve BreakSentence sayımı Başlat hakkını saklı tutar. 

Karakter sayıları hakkında daha fazla bilgi yer [Microsoft Translator SSS](https://www.microsoft.com/en-us/translator/faq.aspx).
