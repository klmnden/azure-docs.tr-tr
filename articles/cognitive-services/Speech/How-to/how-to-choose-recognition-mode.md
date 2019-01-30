---
title: Bing konuşma tanıma modu seçme | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma dilinde en iyi tanıma modu seçeceğinizi öğrenin.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: ed270ed19959240bc1b90ba6171792cf4369e273
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221609"
---
# <a name="bing-speech-recognition-modes"></a>Bing konuşma tanıma modları

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing konuşma metin API'lerine konuşma tanıma birden çok modlarını destekler. Uygulamanız için en iyi tanıma sonuçları üreten modunu seçin.

| Mod | Açıklama |
|---|---|
| *etkileşimli* | Etkileşimli kullanıcı uygulama senaryoları için "Komut ve Denetim" tanıma. Kullanıcılar, komutlar bir uygulamaya yönelik kısa deyimlerin konuşun. |
| *Yazdırma* | Sürekli tanıma dikte senaryolar için. Metni olarak görüntülenmesi uzun cümleler kullanıcılar konuşun. Kullanıcılar daha resmi bir konuşma tarzı benimseyin. |
| *Konuşma* | İnsanlar arasında yapılan görüşmeler fotoğrafını için sürekli tanıma. Kullanıcılar, daha az resmi bir konuşma tarzı benimseyin ve uzun cümleleri kısa tümcecikleri arasında alternatif.

> [!NOTE]
> Bu modu kullanırken geçerli [REST API'leri](../GetStarted/GetStartedREST.md). [İstemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) tanıma modunu belirtmek üzere farklı parametreler kullanın. Daha fazla bilgi için seçtiğiniz istemci kitaplığı bakın.

Daha fazla bilgi için [tanıma modları](../concepts.md#recognition-modes) sayfası.
