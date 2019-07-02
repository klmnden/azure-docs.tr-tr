---
title: Word hizalama - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si Word hizalama bilgiler alır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: 3db5e9651e307211e9dccee20bb8d69586bb9ef1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448155"
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
