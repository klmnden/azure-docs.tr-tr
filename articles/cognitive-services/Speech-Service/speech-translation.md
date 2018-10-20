---
title: Konuşma çevirisi hakkında
titlesuffix: Azure Cognitive Services
description: Konuşma çevirisi özelliklerine genel bakış
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 04/28/2018
ms.author: erhopf
ms.openlocfilehash: af363a45ab113d89c7aad7a38c6e10d335bc6035
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469155"
---
# <a name="about-the-speech-translation-api"></a>Konuşma çevirisi API'si hakkında

Konuşma hizmeti API'sini uygulamaları, araçları ve cihazlar için uçtan uca, gerçek zamanlı, çok dilli konuşma çevirisi eklemenize olanak tanır. Aynı API hem konuşma okuma ve konuşma metin çevirisi için kullanılabilir.

Translator konuşma tanıma API'si ile istemci uygulamaları, hizmet konuşma sesine akışla aktarma ve geri sonuçları bir akışını alın. Bu sonuçları tanınan metin kaynak dili ve çevirisini hedef dil ekleyin. Bir utterance işlemi tamamlanana kadar aynı zamanda son çeviri sağlanan sağlanan geçici çevirileri olabilir.

İsteğe bağlı olarak doğru konuşma konuşma çevirisi etkinleştirme son çeviri Sentezlenen ses sürümünü hazırlanabilir.

Konuşma çevirisi API'si, istemci ve sunucu arasında bir tam çift yönlü iletişim kanalı sağlamak için WebSockets protokolü kullanır. Ancak WebSockets ile işlem gerekmez; Speech SDK'sı, sizin yerinize çözer.

Konuşma çevirisi API'si, çeşitli Microsoft ürünleri ve Hizmetleri güç teknolojiye kullanır. Bu hizmet, zaten binlerce işletme tarafından uygulamalarını ve iş akışları, dünya çapında kullanılır.

## <a name="about-the-technology"></a>Teknoloji hakkında

Microsoft'un çeviri altyapısı temel olan iki farklı yaklaşım: istatistiksel makine çevirisi (SMT) ve sinirsel makine çevirisi (NMT). İkincisi, sinir ağları kullanan bir yapay zeka yaklaşım makine çevirisi için daha modern bir yaklaşımdır. Daha iyi Çeviriler NMT sağlar — değil yalnızca daha doğru ancak aynı zamanda daha fluent ve doğal. Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır.

Bugün Microsoft NMT için en popüler diller için yalnızca daha az sık kullanılan dillerde SMT kullanan geçiş yaptı. Tüm [konuşma konuşma çevirisi için kullanılabilen dilleri](language-support.md#speech-translation) NMT tarafından desteklenir. Konuşma metin çeviri dili çifti bağlı olarak SMT veya NMT kullanabilir. Hedef dil NMT tarafından destekleniyorsa, çevirinin tamamı NMT destekli olur. Hedef Dil NMT tarafından desteklenmiyorsa, çeviri NMT ve İngilizce olarak bir "Özet" arasında iki dilleri kullanarak, SMT karmadır.

Modelleri arasındaki farkları çeviri Altyapısı'na iç. Son kullanıcılar yalnızca geliştirilmiş çeviri kalitesi, özellikle Çince, Japonca ve Arapça dikkat edin.

> [!NOTE]
> Microsoft'un çeviri altyapısı teknoloji hakkında daha fazla ilginizi çekiyor mu? Bkz: [makine çevirisi](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma Çevir öğrenin](how-to-translate-speech-csharp.md)
* [C++'ta konuşma Çevir öğrenin](how-to-translate-speech-cpp.md)
* [Java'da konuşma Çevir öğrenin](how-to-translate-speech-java.md)
