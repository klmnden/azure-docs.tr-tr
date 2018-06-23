---
title: Tanıma modu seçme | Microsoft Docs
description: Nasıl en iyi tanıma modu seçin.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 4f02b683dde16b537ae5554e6435c068f0fcb808
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352174"
---
# <a name="speech-recognition-modes"></a>Konuşma tanıma modları

Microsoft'un *metin konuşma* API'leri konuşma tanıma birden çok modunu destekler. Uygulamanız için en iyi tanıma sonuçlar üretir modu seçin.

| Mod | Açıklama |
|---|---|
| *Etkileşimli* | Etkileşimli bir kullanıcı uygulama senaryoları için "Komut ve Denetim" tanıma. Kullanıcılar, komutları bir uygulama olarak hedeflenen kısa tümceleri konuşun. |
| *Yazdırma* | Sürekli tanıma dikte senaryoları için. Kullanıcıların metni olarak görüntülenmesi uzun cümleleri konuşun. Kullanıcılar daha resmi bir konuşma stili benimser. |
| *Konuşma* | İnsanlar arasındaki görüşmeleri çoğaltmaya için sürekli tanıma. Kullanıcılar daha az resmi bir konuşma stili benimseme ve uzun cümleleri ve kısa tümcecikleri arasında farklı.

> [!NOTE]
> Kullandığınızda bu modların geçerli [REST API'leri](../GetStarted/GetStartedREST.md). [İstemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) farklı parametreler tanıma modu belirtmek için kullanın. Daha fazla bilgi için tercih ettiğiniz istemci kitaplığına bakın.

Daha fazla bilgi için bkz: [tanıma modları](../concepts.md#recognition-modes) sayfası.
