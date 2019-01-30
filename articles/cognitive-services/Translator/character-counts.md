---
title: Karakter sayısı - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Nasıl Translator Text API karakter sayar.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c56622ee5e25fc422d02cec6ecfaa1f847e9e769
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226443"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Translator Text API karakter nasıl sayılır?

Translator metin çevirisi API'si, her karakter giriş sayar. Karakterleri Unicode anlamında, bayt sayısını değil. Unicode sayısı iki karakter olarak temsilciler. Boşluk ve biçimlendirme karakter olarak sayılır. Yanıt uzunluğu önemli değildir.

Algılama ve BreakSentence yöntemlerine yapılan çağrıda karakter tüketimini sayılmaz. Ancak, algılama ve BreakSentence yöntemlere yapılan çağrılar sayılan diğer işlevleri kullanımını kabul edilebilir bir oranda olduğunu bekliyoruz. Microsoft algılama ve BreakSentence sayımı Başlat hakkını saklı tutar.

Karakter sayıları hakkında daha fazla bilgi yer [Microsoft Translator SSS](https://www.microsoft.com/en-us/translator/faq.aspx).
