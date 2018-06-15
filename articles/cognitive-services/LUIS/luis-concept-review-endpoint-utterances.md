---
title: Gözden dil anlama (HALUK içinde) - Azure active öğrenme kullanmak için uç nokta utterances | Microsoft Docs
description: "'Uç noktası utterances gözden' adlı etkin öğrenme özelliğini kullanmak daha hızlı performans tahminleri geliştirmek için."
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: b9672e8e63fb601d4411a342b7f3c00e30f9e002
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356437"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Uç nokta utterances gözden geçirerek etkin öğrenme etkinleştir
Etkin öğrenme tahmin doğruluğunu ve en kolay uygulama geliştirmek için üç stratejileri biridir. 

## <a name="what-is-active-learning"></a>Etkin learning nedir
Etkin öğrenme iki adımlı bir işlemdir. İlk olarak, doğrulama gerekli uygulamanın uç noktada aldığı utterances HALUK seçer. İkinci adım uygulama sahibi veya ortak çalışanı için seçilen utterances doğrulamak için tarafından gerçekleştirilen [gözden](label-suggested-utterances.md)doğru amacını ve hedefi içinde herhangi bir varlık dahil olmak üzere. Utterances inceledikten sonra eğitmek ve uygulamayı yeniden yayımlayın. 

## <a name="which-utterances-are-on-the-review-list"></a>Hangi utterances gözden geçirme listede olan
HALUK utterances hedefi tetikleme en düşük bir puan sahip veya üst iki amacı puanları çok Kapat gözden geçirme listesine ekler. 

## <a name="where-are-the-utterances-from"></a>Gelen utterances nerede
Uç nokta utterances uygulamanın HTTP uç noktası üzerinde son kullanıcı sorgularından alınır. Uygulamanızı yayımlanmıyor veya isabet henüz almadığını varsa gözden geçirmek için hiçbir utterances yoktur. Uç noktası isabet belirli amaç veya varlık için aldıysanız, bunları içeren gözden geçirmek için utterances yok. 

## <a name="schedule-review-periodically"></a>Düzenli aralıklarla zamanlama gözden geçirme
Önerilen utterances gözden geçirme, her gün yapılması gerekmez, ancak HALUK ', düzenli bakım parçası olması gerekir. 

## <a name="delete-review-items-programmatically"></a>Gözden geçirme öğeleri program aracılığıyla silme
Uygulamanızı büyükse, bazı utterances gözden geçirin ve listeden rest program aracılığıyla silme tercih edebilirsiniz. Bu ilk tarafından yapılır [alma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) listesi ve ardından [silme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) utterances kimliğe göre

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [gözden](Label-Suggested-Utterances.md) uç nokta utterances
