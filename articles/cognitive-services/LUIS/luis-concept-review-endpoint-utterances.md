---
title: Kullanıcı utterance gözden geçirin
titleSuffix: Language Understanding - Azure Cognitive Services
description: Etkin öğrenme, doğru amacı ve varlık için gözden geçirme uç nokta Konuşma ile. LUIS, uç nokta konuşma emin seçer.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: 2af11d7776a29288801e5db049262481ae27c102
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893699"
---
# <a name="concepts-for-enabling-active-learning-by-reviewing-endpoint-utterances"></a>Etkin öğrenme konuşma uç noktası inceleyerek etkinleştirmek için kavramları
Etkin öğrenme tahmin doğruluğunu ve kolay uygulama geliştirmek için üç strateji biridir. Etkin öğrenme, doğru amacı ve varlık için gözden geçirme uç nokta Konuşma ile. LUIS, uç nokta konuşma emin seçer.

## <a name="what-is-active-learning"></a>Etkin öğrenme nedir
Etkin öğrenme iki adımlı bir işlemdir. İlk olarak, doğrulama gerekli uygulamanın uç noktada aldığı konuşma LUIS seçer. İkinci adım için seçilen konuşma doğrulamak için uygulama sahibi veya ortak çalışanı tarafından gerçekleştirilen [gözden](luis-how-to-review-endpoint-utterances.md)doğru amaç ve amaç içindeki tüm varlıklar dahil olmak üzere. Konuşma inceledikten sonra eğitme ve uygulamayı yeniden yayımlayın. 

## <a name="which-utterances-are-on-the-review-list"></a>Hangi konuşma gözden geçirme listede yer
LUIS konuşma amacı tetikleme en düşük puan sahip veya üst iki amacı puanları çok Kapat gözden geçirme listesine ekler. 

## <a name="single-pool-for-utterances-per-app"></a>Tek bir havuz için uygulama başına konuşma
**Gözden geçirin, konuşma uç noktası** listesi temel sürümünü değiştirmez. Etkin olarak hangi sürümün ifadesini düzenlediğinizden veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak, gözden geçirilecek tek bir ifade havuzu vardır. 

## <a name="where-are-the-utterances-from"></a>Gelen sesleri nerede
Konuşma uç noktası son kullanıcı sorgularını uygulamanın HTTP uç noktasında alınmıştır. Uygulamanızı yayımlanmadı veya isabet henüz almadı, gözden geçirmek için herhangi bir konuşma yoktur. Belirli bir amaç veya varlık için hiçbir uç noktası İsabeti alınırsa, bunları içeren gözden geçirmek için konuşma yoktur. 

## <a name="schedule-review-periodically"></a>Zamanlama düzenli aralıklarla gözden geçirme
Önerilen konuşma gözden geçirme, her gün yapılması gerekmez, ancak, düzenli bakım LUIS'ın bir parçası olması gerekir. 

## <a name="delete-review-items-programmatically"></a>Gözden geçirme öğelerini program aracılığıyla silme
Kullanım **[unlabelled konuşma Sil](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9)** API. Bu konuşma silme işleminden önce yedekleme  **[günlük dosyalarını dışarı aktarma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)**.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [gözden](luis-how-to-review-endpoint-utterances.md) konuşma uç noktası
