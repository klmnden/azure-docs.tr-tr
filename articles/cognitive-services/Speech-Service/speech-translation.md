---
title: Konuşma çeviri hakkında | Microsoft Docs
description: Konuşma çeviri özelliklerine genel bakış
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 43fcde887c21794989aa43540a214ef34893a630
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355618"
---
# <a name="about-the-speech-translation-api"></a>Konuşma çeviri API hakkında

Microsoft konuşma API uygulamaları, araçları ve aygıtlar için uçtan uca, gerçek zamanlı, çok dilli çeviri konuşma eklemenizi sağlar. Aynı API konuşma konuşma ve konuşma metin çevirisi için kullanılabilir.

Microsoft Çeviricisi konuşma API ile istemci uygulamaları hizmetine konuşma ses akışı ve sonuçları bir akış geri alabilirsiniz. Bu sonuçları tanınan metni kaynak dil ve hedef dilde kendi çevirisi içerir. Bir utterance tamamlanana kadar aynı zamanda bir son çeviri sağlanan sağlanan geçici çevirileri olabilir.

İsteğe bağlı olarak doğru konuşma konuşma çeviri etkinleştirme son çeviri birleştirilen bir ses sürümü hazırlanabilir.

Konuşma çeviri API WebSockets Protokolü istemci ve sunucu arasında bir tam çift yönlü iletişim kanalı sağlamak için kullanır. Ancak WebSockets ile işlem gerekmez; Konuşma SDK'sı, sizin için işler.

Konuşma çeviri API güç çeşitli Microsoft ürünleri ve Hizmetleri aynı teknolojileri kullanır. Bu hizmet, zaten işletmeler binlerce tarafından kendi uygulamaları ve iş akışlarında dünya çapında kullanılır.

## <a name="about-the-technology"></a>Teknolojisi hakkında

Microsoft'un çeviri altyapısı temel olan iki farklı yaklaşım: istatistiksel makine çevirisi (SMT) ve sinir makine çevirisi (NMT). İkincisi, sinir ağları kullanan bir yapay zeka yaklaşım için makine çevirisi daha modern bir yaklaşımdır. NMT daha iyi Çeviriler sunar — değil yalnızca daha doğru ancak aynı zamanda daha fluent ve doğal. Bu ortadan anahtar nedeni NMT tam bir cümle bağlamında kelimeleri kullanmasıdır.

Bugün, Microsoft NMT için en popüler diller için yalnızca daha az sık kullanılan dillerin SMT kullanan geçiş yaptı. Tüm [konuşma konuşma çevirisi kullanılabilir diller](supported-languages.md#speech-translation) NMT tarafından sağlanmıştır. Konuşma metin çeviri SMT veya NMT dil çifti bağlı olarak kullanabilir. Hedef Dil NMT tarafından destekleniyorsa, tam çeviri NMT sağlanmıştır. Hedef Dil NMT tarafından desteklenmiyorsa, çeviri NMT ve SMT iki diller arasında İngilizce bir "Özet" olarak kullanarak, bir karma ' dir.

Modelleri arasındaki farklar çeviri altyapısı iç. Son kullanıcılar yalnızca geliştirilmiş çeviri kalitesini özellikle Çince, Japonca ve Arapça için dikkat edin.

> [!NOTE]
> Microsoft'un çeviri altyapısı arkasında teknolojisi hakkında daha fazla bilgi ilginizi çekiyor mu? Bkz: [makine çevirisi](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
