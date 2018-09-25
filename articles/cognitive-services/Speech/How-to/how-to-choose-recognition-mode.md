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
ROBOTS: NOINDEX
ms.openlocfilehash: a39b357a26823e322d4e902f2d99b67488bbf2df
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950888"
---
# <a name="bing-speech-recognition-modes"></a>Bing konuşma tanıma modları

Bing konuşma metin API'lerine konuşma tanıma birden çok modlarını destekler. Uygulamanız için en iyi tanıma sonuçları üreten modunu seçin.

| Mod | Açıklama |
|---|---|
| *Etkileşimli* | Etkileşimli kullanıcı uygulama senaryoları için "Komut ve Denetim" tanıma. Kullanıcılar, komutlar bir uygulamaya yönelik kısa deyimlerin konuşun. |
| *Yazdırma* | Sürekli tanıma dikte senaryolar için. Metni olarak görüntülenmesi uzun cümleler kullanıcılar konuşun. Kullanıcılar daha resmi bir konuşma tarzı benimseyin. |
| *Konuşma* | İnsanlar arasında yapılan görüşmeler fotoğrafını için sürekli tanıma. Kullanıcılar, daha az resmi bir konuşma tarzı benimseyin ve uzun cümleleri kısa tümcecikleri arasında alternatif.

> [!NOTE]
> Bu modu kullanırken geçerli [REST API'leri](../GetStarted/GetStartedREST.md). [İstemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) tanıma modunu belirtmek üzere farklı parametreler kullanın. Daha fazla bilgi için seçtiğiniz istemci kitaplığı bakın.

Daha fazla bilgi için [tanıma modları](../concepts.md#recognition-modes) sayfası.
