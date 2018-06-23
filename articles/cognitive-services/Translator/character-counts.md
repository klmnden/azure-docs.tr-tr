---
title: Microsoft Çeviricisi metin API karakter sayıları | Microsoft Docs
description: Nasıl Microsoft Çeviricisi metin API karakter sayar.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: ebe3e3606a0413730e1fbfd704a6403f77275f89
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352583"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Microsoft Çeviricisi metin API karakterleri nasıl sayar

Microsoft Translator her karakteri giriş sayar. Unicode algılama, bayt değil karakter. Unicode sayısı iki karakter olarak temsilciler. Boşluk ve biçimlendirme karakter olarak sayılır. Yanıt uzunluğu önemli değildir.

Algıla ve BreakSentence yöntemlerine karakter tüketimini sayılmaz. Algıla ve BreakSentence yöntemleri çağrıları sayılır diğer işlevleri kullanmak için makul bir oranda aynıdır ancak bekliyoruz. Microsoft sayım Algıla ve BreakSentence Başlat hakkını saklı tutar. 

From dil parametresini atlarsanız teklifleri otomatik algılama çevir. 

Karakter sayıları hakkında daha fazla bilgi yer [Microsoft Çeviricisi SSS](https://www.microsoft.com/en-us/translator/faq.aspx).
