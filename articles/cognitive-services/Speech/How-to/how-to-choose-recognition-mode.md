---
title: Bing konuşma tanıma modu seçme | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma dilinde en iyi tanıma modu seçeceğinizi öğrenin.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 21615b09a7b9599597706e38b55072cf80f1b69b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345042"
---
# <a name="bing-speech-recognition-modes"></a>Bing konuşma tanıma modları

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing konuşma metin API'lerine konuşma tanıma birden çok modlarını destekler. Uygulamanız için en iyi tanıma sonuçları üreten modunu seçin.

| Mod | Açıklama |
|---|---|
| *Etkileşimli* | Etkileşimli kullanıcı uygulama senaryoları için "Komut ve Denetim" tanıma. Kullanıcılar, komutlar bir uygulamaya yönelik kısa deyimlerin konuşun. |
| *Yazdırma* | Sürekli tanıma dikte senaryolar için. Metni olarak görüntülenmesi uzun cümleler kullanıcılar konuşun. Kullanıcılar daha resmi bir konuşma tarzı benimseyin. |
| *Konuşma* | İnsanlar arasında yapılan görüşmeler fotoğrafını için sürekli tanıma. Kullanıcılar, daha az resmi bir konuşma tarzı benimseyin ve uzun cümleleri kısa tümcecikleri arasında alternatif.

> [!NOTE]
> Bu modu kullanırken geçerli [REST API'leri](../GetStarted/GetStartedREST.md). [İstemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) tanıma modunu belirtmek üzere farklı parametreler kullanın. Daha fazla bilgi için seçtiğiniz istemci kitaplığı bakın.

Daha fazla bilgi için [tanıma modları](../concepts.md#recognition-modes) sayfası.
