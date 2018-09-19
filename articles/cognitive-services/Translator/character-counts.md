---
title: Translator metin API'si karakter sayısı
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
ms.openlocfilehash: c6234a46ae55d73739dcc23110c5e0f6375c3f96
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128749"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Translator Text API karakter nasıl sayılır?

Translator metin çevirisi API'si, her karakter giriş sayar. Karakterleri Unicode anlamında, bayt sayısını değil. Unicode sayısı iki karakter olarak temsilciler. Boşluk ve biçimlendirme karakter olarak sayılır. Yanıt uzunluğu önemli değildir.

Algılama ve BreakSentence yöntemlerine yapılan çağrıda karakter tüketimini sayılmaz. Ancak, algılama ve BreakSentence yöntemlere yapılan çağrılar sayılan diğer işlevleri kullanımını kabul edilebilir bir oranda olduğunu bekliyoruz. Microsoft algılama ve BreakSentence sayımı Başlat hakkını saklı tutar. 

Karakter sayıları hakkında daha fazla bilgi yer [Microsoft Translator SSS](https://www.microsoft.com/en-us/translator/faq.aspx).
