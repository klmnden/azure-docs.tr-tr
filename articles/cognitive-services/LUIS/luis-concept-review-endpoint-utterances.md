---
title: Etkin öğrenmeyi kullanma Language Understanding (LUIS içinde) - Azure için uç nokta konuşma gözden geçirin | Microsoft Docs
description: "'Uç noktası konuşma gözden geçirme' adlı etkin olarak öğrenmeye özelliğiyle daha hızlı performans tahminleri geliştirmek için."
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 4144db7474f20f464d14722431d5d4a2546a8c5a
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949858"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Etkin öğrenme konuşma uç noktası inceleyerek etkinleştir
Etkin öğrenme tahmin doğruluğunu ve kolay uygulama geliştirmek için üç strateji biridir. 

## <a name="what-is-active-learning"></a>Etkin öğrenme nedir
Etkin öğrenme iki adımlı bir işlemdir. İlk olarak, doğrulama gerekli uygulamanın uç noktada aldığı konuşma LUIS seçer. İkinci adım için seçilen konuşma doğrulamak için uygulama sahibi veya ortak çalışanı tarafından gerçekleştirilen [gözden](luis-how-to-review-endoint-utt.md)doğru amaç ve amaç içindeki tüm varlıklar dahil olmak üzere. Konuşma inceledikten sonra eğitme ve uygulamayı yeniden yayımlayın. 

## <a name="which-utterances-are-on-the-review-list"></a>Hangi konuşma gözden geçirme listede yer
LUIS konuşma amacı tetikleme en düşük puan sahip veya üst iki amacı puanları çok Kapat gözden geçirme listesine ekler. 

## <a name="single-pool-for-utterances-per-app"></a>Tek bir havuz için uygulama başına konuşma
**Gözden geçirin, konuşma uç noktası** listesi temel sürümünü değiştirmez. Konuşma gözden geçirmek için hangi sürümünden bağımsız olarak utterance etkin olarak düzenlediğiniz veya uygulamanın hangi sürümünün uç noktada yayımlanan tek bir havuzu yok. 

## <a name="where-are-the-utterances-from"></a>Gelen sesleri nerede
Konuşma uç noktası son kullanıcı sorgularını uygulamanın HTTP uç noktasında alınmıştır. Uygulamanızı yayımlanmadı veya isabet henüz almadı, gözden geçirmek için herhangi bir konuşma yoktur. Belirli bir amaç veya varlık için hiçbir uç noktası İsabeti alınırsa, bunları içeren gözden geçirmek için konuşma yoktur. 

## <a name="schedule-review-periodically"></a>Zamanlama düzenli aralıklarla gözden geçirme
Önerilen konuşma gözden geçirme, her gün yapılması gerekmez, ancak, düzenli bakım LUIS'ın bir parçası olması gerekir. 

## <a name="delete-review-items-programmatically"></a>Gözden geçirme öğelerini program aracılığıyla silme
Uygulamanızı büyükse, bazı konuşma gözden geçirin ve listeye geri kalanından program aracılığıyla silme tercih edebilirsiniz. Bu ilk tarafından yapılır [alma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) listesini ve ardından [silme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) konuşma kimliğine göre

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [gözden](luis-how-to-review-endoint-utt.md) konuşma uç noktası
