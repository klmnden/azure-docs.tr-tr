---
title: Paralel belgeleri nelerdir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Paralel belgeleri belgelerin bir diğer çeviri olduğu çiftleridir. Çifti tek bir belgede kaynak dili cümlelerde ve diğer belge Bu cümle hedef dile çevrilen içerir.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 3b7997adfa38fa24bdf41f22c99f06e9a0ba1bf0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447404"
---
# <a name="what-are-parallel-documents"></a>Paralel belgeleri nelerdir?

Paralel belgeleri belgelerin bir diğer çeviri olduğu çiftleridir. Çifti tek bir belgede kaynak dili cümlelerde ve diğer belge Bu cümle hedef dile çevrilen içerir.
Hangi dil "kaynak" olarak işaretlenmiş ve hangi dil "hedef" olarak işaretlenmiş önemi: paralel bir belge herhangi bir yönde bir çeviri sistemi eğitmek için kullanılabilir.

## <a name="requirements"></a>Gereksinimler

En az bir sistem eğitmek için 10.000 benzersiz paralel cümleler gerekir. En iyi uygulama, daha fazla paralel içerik sürekli olarak ekleyebilirsiniz ve sisteminizin çeviri kalitesini artırmak için retrain.

Microsoft, belgeler için özel Translator karşıya bir üçüncü tarafın telif hakkını veya fikri özellikleri ihlal etmemek gerektirir. Daha fazla bilgi için lütfen bkz [kullanım](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Portalı kullanarak belgeyi karşıya yükleme, fikri mülkiyet belgede sahipliğini değiştirmez.

## <a name="use-of-parallel-documents"></a>Paralel belgelerin kullanımı

Paralel belgeleri, sistem tarafından kullanılır:

1.  Nasıl sözcük ve tümcecikleri cümleler genellikle iki diller arasında eşlendiğine öğrenin.

2.  Çevreleyen tümcecikleri bağlı olarak uygun bağlam işlemi hakkında bilgi edinmek için. Bir sözcük her zaman başka bir dilde aynı tam sözcüğü çevrilmemesine.

En iyi uygulama, kaynak ve hedef dil sürümlerini belgeler arasında 1:1 cümle ilişkiyi olduğundan emin olun.

Etki alanı (kategori) belirli projenizi ise belgelerinizi terminolojisinde o kategorideki tutarlı olmalıdır. Sonuçta elde edilen çeviri sistem kalitesini belge kümenizdeki cümleler sayısı ve cümleleri kalitesini bağlıdır. Daha fazla örnek belgelerinizi, kategori için belirli bir sözcük için çeşitli kullanımları içeren vardır, daha iyi iş sistem çevirisi sırasında yapabilirsiniz.

Belgeler karşıya her çalışma alanına özel ve dilediğiniz sayıda projeleri veya eğitimleri kullanılabilir. Cümleleri belgelerinizden ayıklanan ayrı olarak, deponuzda düz Unicode metin dosyaları olarak depolanır ve silmeniz için kullanılabilir. Özel Translator bir belge deposu olarak kullanmaz, karşıya yüklediğiniz biçiminde belgelerini indirmek mümkün olmayacaktır bunları karşıya yüklendi.



## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin bir [sözlük](what-is-dictionary.md) özel Translator içinde.
