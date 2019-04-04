---
title: Küfür filtresini - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si filtreleme küfür kullanın.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: bd7a05f2f597d1882293387e5aac8e4d7367d051
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916640"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Translator metin çevirisi API'si ile filtreleme küfür Ekle

Normalde Translator hizmeti, mevcut çeviri kaynağında küfür korur. Küfür ve sözcükler Küfürlü yapan bağlam derecesini kültürler arasında farklılık gösterir. Sonuç olarak, hedef dilde küfür derecesini yükseltilmiş sınırlı veya olabilir.

Küfür kaynak metni mevcut olsa da, küfür çevirisini görmekten kaçınmak istiyorsanız, filtreleme seçeneği Translate() yöntemi sağlanır küfür kullanın. Bu seçenek, uygun etiketlerle işaretlenmiş silinmiş küfür görmek isteyip istemediğinizi seçin veya hiçbir eyleme girişilmedi sağlar.

Translate() yöntemi yeni bir öğe "ProfanityAction" içerir "options" parametresi alır. Kabul edilen ProfanityAction "NoAction", "Marked" ve "Silinmiş" değerleri

## <a name="accepted-values-of-profanityaction-and-examples"></a>Kabul edilen değerler ProfanityAction ve örnekler
|ProfanityAction değeri | Eylem | Örnek: Kaynak - Japonca | Örnek: Target - İngilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Aynı seçenek ayarı bulunamadı. Küfür kaynaktan hedefe aktarır. | 彼は変態です。 | He aptalın olur. |
| İşaretli | Küfürlü sözcükleri tarafından XML etiketleri arasına \<küfür >... \</profanity >. | 彼は変態です。 | He's bir \<küfür > jerk\</profanity >. |
| Silinen | Küfürlü sözcükleri değiştirme yapmadan çıktıdan kaldırıldı. | 彼は。 | He's bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrınızı filtreleme küfür Uygula](reference/v3-0-translate.md)
