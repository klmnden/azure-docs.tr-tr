---
title: Azure HALUK uygulamalardaki utterances | Microsoft Docs
description: Utterances dil anlama akıllı hizmet (HALUK) uygulamalarında ekleyin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/13/2018
ms.author: v-geberr
ms.openlocfilehash: 7c2cd84df8f1eccbd30a7c8054ec8d06336cf2dd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264655"
---
# <a name="utterances-in-luis"></a>HALUK utterances

**Utterances** yorumlamak için uygulamanız gereken kullanıcıdan giriş. Hedefleri ve varlıkları onlardan ayıklamak için HALUK eğitmek için her amaç için farklı girişleri çeşitli yakalamak önemlidir. Etkin öğrenme ya da yeni utterances üzerinde eğitmek etmeden işlemi HALUK sunar makineye öğrenilen Intelligence için gereklidir.

Kullanıcılar girer düşündüğünüz tümcecikleri toplayın. Aynı anlama gelir ancak oluşturulan utterances farklı sözcük uzunluğu ve word yerleştirme içerir. 

## <a name="how-to-choose-varied-utterances"></a>Değiştirilen utterances seçme
Ne zaman ilk başlamanıza tarafından [örnek utterances ekleme] [ add-example-utterances] HALUK modelinize göz önünde bulundurmanız bazı ilkeler aşağıda verilmiştir.

### <a name="utterances-arent-always-well-formed"></a>Utterances her zaman iyi biçimlendirilmiş değil
"Bana Paris bilet kitap" veya bir parçasını "Kayıt" gibi bir cümle gibi bir cümle olabilir ya da "Paris uçuş."  Kullanıcıları, genellikle yazım yapar. Uygulamanızı planlarken, kullanıcı girişi için HALUK geçirmeden önce yazım olup olmadığına göz önünde bulundurun. [Bing yazım denetleme API] [ BingSpellCheck] HALUK ile tümleşir. Yayımladığınızda Bing yazım denetleme API'si için bir dış anahtarla HALUK uygulamanızı ilişkilendirebilirsiniz. Onay kullanıcı utterances yazım değil, yazım hatalarını ve yazım hatalarını içeren utterances üzerinde HALUK eğitmek.

### <a name="use-the-representative-language-of-the-user"></a>Kullanıcının temsilcisi dilini kullanma
Utterances seçerken, bir ortak terimini veya tümceciğini olduğunu düşündüğünüz istemci uygulamanızı tipik kullanıcıya olmayabilir unutmayın. Etki alanı deneyimi olmayabilir. Bu nedenle terimleri veya Uzman olan bir kullanıcı yalnızca diyor tümcecikleri kullanırken dikkatli olun.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>İfade yanı sıra çeşitli terminolojisi seçin
Değiştirilen cümle desenleri oluşturmak üzere çabalarına yaptığınız olsa bile, bazı sözlük hala yinelenir bulacaksınız.

Bu örnek utterances alın:
```
how do I get a computer?
Where do I get a computer?
I want to get a computer, how do I go about it?
When can I have a computer? 
```
Burada, çekirdek terimi "bilgisayar" değil değişmesi. Bunlar, masaüstü bilgisayar, dizüstü bilgisayar, iş istasyonu veya hatta yalnızca makine diyebilirsiniz. HALUK akıllıca bağlam anlamlıları oluşturur, ancak eğitim utterances oluşturduğunuzda, bunları değiştirmek hala daha iyi olur.

## <a name="example-utterances-in-each-intent"></a>Her amacı, örnek utterances
Her amacı örnek utterances, en az 10-15 olmalıdır. Tüm örnek utterances olmayan amacına varsa, HALUK eğitmek mümkün olmaz. Bir veya çok az örnek utterances ile amacına varsa, HALUK doğru şekilde hedefi tahmin değil. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Her yazma yineleme için 10-15 utterances küçük grupları Ekle
Modelin her yinelemede utterances büyük miktarlarda eklemeyin. Utterances onlarca miktarlarında ekleyin. [Tren](luis-how-to-train.md), [yayımlama](publishapp.md), ve [test](interactive-test.md) yeniden.  

HALUK dikkatle seçili utterances etkili modelleriyle oluşturur. Çok fazla utterances ekleme, karışıklığı oluşturduğundan değerli değil.  

Birkaç utterances ile sonra başlatmak daha iyi [endpoint utterances gözden](label-suggested-utterances.md) doğru hedefi tahmin ve varlık ayıklama için.

## <a name="training-utterances"></a>Eğitim utterances
Eğitim belirleyici: utterance tahmin sürümleri veya uygulamalar arasında biraz farklı.

## <a name="testing-utterances"></a>Utterances test etme 

Geliştiriciler kendi HALUK uygulamayı gerçek trafiği ile uç noktasına utterances göndererek test başlamanız gerekir. Bu utterances amaçları ve varlıklarla performansını artırmak için kullanılan [gözden utterances](label-suggested-utterances.md). HALUK Web sitesi bölmesinde test gönderilen testler uç noktası aracılığıyla gönderilmez ve bu nedenle active öğrenme katkıda değil. 

## <a name="review-utterances"></a>Utterances gözden geçirin
Modelinizi eğitilen, yayımlanan ve alıcı sonra [endpoint](luis-glossary.md#endpoint) sorguları, [utterances gözden](label-suggested-utterances.md) HALUK tarafından önerilen. HALUK amacını veya varlık düşük puanlarını sahip uç nokta utterances seçer. 

## <a name="best-practices"></a>En iyi uygulamalar
Gözden geçirme [en iyi uygulamalar](luis-concept-best-practices.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [örnek utterances ekleme] [ add-example-utterances] kullanıcı utterances anlamak için HALUK uygulama eğitim hakkında bilgi için.

[add-example-utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-add-example-utterances
[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text