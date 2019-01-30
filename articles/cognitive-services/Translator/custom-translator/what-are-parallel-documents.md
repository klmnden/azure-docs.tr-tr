---
title: Paralel belgeleri nelerdir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Paralel belgeleri belgelerin bir diğer çeviri olduğu çiftleridir. Çifti tek bir belgede kaynak dili cümlelerde ve diğer belge Bu cümle hedef dile çevrilen içerir.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: d7f479bbacef7270807d9292e7b91fe835485647
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217382"
---
# <a name="what-are-parallel-documents"></a>Paralel belgeleri nelerdir?

Paralel belgeleri belgelerin bir diğer çeviri olduğu çiftleridir. Çifti tek bir belgede kaynak dili cümlelerde ve diğer belge Bu cümle hedef dile çevrilen içerir.
Hangi dil "kaynak" olarak işaretlenmiş ve hangi dil "hedef" olarak işaretlenmiş önemi: paralel bir belge herhangi bir yönde bir çeviri sistemi eğitmek için kullanılabilir.

## <a name="use-of-parallel-documents"></a>Paralel belgelerin kullanımı

Paralel belgeleri, sistem tarafından kullanılır:

1.  Nasıl sözcük ve tümcecikleri cümleler genellikle iki diller arasında eşlendiğine öğrenin.

2.  Çevreleyen tümcecikleri bağlı olarak uygun bağlam işlemi hakkında bilgi edinmek için. Bir sözcük her zaman başka bir dilde aynı tam sözcüğü çevrilmemesine.

En iyi uygulama, kaynak ve hedef dil sürümlerini belgeler arasında 1:1 cümle ilişkiyi olduğundan emin olun.

Belgeler karşıya her çalışma alanına özel ve dilediğiniz sayıda projeleri veya eğitimleri kullanılabilir. Cümleleri belgelerinizden ayıklanan ayrı olarak, deponuzda düz Unicode metin dosyaları olarak depolanır ve silmeniz için kullanılabilir. Özel Translator bir belge deposu olarak kullanmaz, karşıya yüklediğiniz biçiminde belgelerini indirmek mümkün olmayacaktır bunları karşıya yüklendi.

## <a name="recommendations"></a>Öneriler

Etki alanı (kategori) belirli projenizi ise belgelerinizi terminolojisinde o kategorideki tutarlı olmalıdır. Sonuçta elde edilen çeviri sistem kalitesini belge kümenizdeki cümleler sayısı ve cümleleri kalitesini bağlıdır. Daha fazla örnek belgelerinizi, kategori için belirli bir sözcük için çeşitli kullanımları içeren vardır, daha iyi iş sistem çevirisi sırasında yapabilirsiniz.

En az bir sistem eğitmek için 10.000 paralel cümleler gerekir. En iyi uygulama, daha fazla paralel içerik sürekli olarak ekleyebilirsiniz ve sisteminizin çeviri kalitesini artırmak için retrain.

Microsoft, belgeler için özel Translator karşıya bir üçüncü tarafın telif hakkını veya fikri özellikleri ihlal etmemek gerektirir. Daha fazla bilgi için lütfen bkz [kullanım](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Portalı kullanarak belgeyi karşıya yükleme, fikri mülkiyet belgede sahipliğini değiştirmez.

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin bir [sözlük](what-is-dictionary.md) özel Translator içinde.
