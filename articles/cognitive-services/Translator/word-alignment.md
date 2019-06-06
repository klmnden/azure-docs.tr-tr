---
title: Word hizalama - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si Word hizalama bilgiler alır.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: v-pawal
ms.custom: seodec18
ms.openlocfilehash: 040d122810529daafcebc2e66f2fa7b2730a6a0d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514659"
---
# <a name="how-to-receive-word-alignment-information"></a>Word hizalama bilgi alma

## <a name="receiving-word-alignment-information"></a>Word hizalama bilgi alma
Hizalama bilgileri almak için çeviri yöntemini kullanın ve isteğe bağlı includeAlignment parametresini ekleyin.

## <a name="alignment-information-format"></a>Hizalama bilgileri biçimi
Hizalama, her bir sözcüğün kaynağı için aşağıdaki biçimde bir dize değeri olarak döndürülür. Her sözcüğün bilgilerini boşluk ayrılmış (betik) gibi dillerin Çince dahil olmak üzere, bir boşluk ile ayrılır:

[[SourceTextStartIndex]:[SourceTextEndIndex]-[TgtTextStartIndex]:[TgtTextEndIndex]] *

Örnek hizalama dizesi: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

Diğer bir deyişle, iki nokta üst üste başlangıç ayırır ve dilleri uç dizini, dash ayırır ve sözcükler alanı ayırır. Bir sözcük ile sıfır, bir veya birden çok sözcük başka bir dil yeteri kadar ve hizalanmış sözcükleri bitişik olmayan olabilir. Hizalama bilgi kullanılabilir duruma geldiğinde hizalama öğesi boş olur. Yöntemi, bu durumda hata döndürür.

## <a name="restrictions"></a>Kısıtlamalar
Hizalama dil çiftleri bir alt kümesi için yalnızca bu noktada döndürülür:
* herhangi bir dili İngilizce;
* herhangi diğer dili İngilizce dışında Basitleştirilmiş Çince, Geleneksel Çince ve Letonca-İngilizce
* Japonca-Kore dili veya Japonca, Korece cümlenin tamamlanmış bir çeviri ise hizalama bilgi almazsınız. Tamamlanmış bir çeviri örneği olan "Bu bir test", "I beğendiğiniz" ve diğer yüksek sıklık düzeyi cümleleri.

## <a name="example"></a>Örnek

Örnek JSON

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
