---
title: Analiz yöntemi - dil analizi API'si
titlesuffix: Azure Cognitive Services
description: Analiz yöntemi belirli doğal dil Girişleri analiz etmek için dil analizi API'si kullanma
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 12/13/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 02c41e2510fd77f4bb65143faf62737f0985d2b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61401196"
---
# <a name="analyze-method"></a>Analiz yöntemi

> [!IMPORTANT]
> Dilbilimsel Analiz önizleme sürümü 9 Ağustos 2018 tarihinde kullanımdan kaldırılmıştır. Metin işleme ve analiz için [Azure Machine Learning metin analizi modüllerini](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) kullanmanızı öneririz.

**Analiz** REST API, verilen doğal dil giriş analiz etmek için kullanılır.
Yalnızca bulma ilgili [tümce ve belirteçlere ayırmaktır](Sentences-and-Tokens.md) içinde giriş, bulma [konuşma bölümü etiketleri](POS-tagging.md), veya bulma [constituency ağaç](Constituency-Parsing.md).
İlgili Çözümleyicileri seçerek istediğiniz hangi sonuçları belirtebilirsiniz.
Tüm kullanılabilir Çözümleyicileri listesine bakmak  **[Çözümleyicileri](AnalyzersMethod.md)** .

Giriş dizesinin dilini belirtmek gerektiğini unutmayın.

**REST uç noktası:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>İstek parametreleri

Ad | Type | Gerekli | Açıklama
-----|-------|----------|------------
**Dil**    | string | Evet | İki harfli ISO dil kod'analiz için kullanılacak. Örneğin, "en" İngilizce olarak belirlenmiştir.
**analyzerIds** | dize listesi | Evet | Çözümleyiciler, uygulamak için bir GUID'ler listesi. Daha fazla bilgi için Çözümleyicileri belgelerine bakın.
**text**        | string | Evet | Analiz edilecek ham giriş. Bu, bir sözcük veya tümceciği, tam bir cümle veya paragraf tam veya discourse gibi kısa bir dize olabilir.

## <a name="response-json"></a>Yanıt (JSON)

Analiz çıkış dizisi, istekte belirtilen her bir öznitelik için bir tane.

Sonuçlar şu şekilde görünür:

Ad | Tür | Açıklama
-----|------|--------------
Analyzerıd | string | Belirtilen Çözümleyicisi GUİD'si
Sonuç | object | Çözümleyici sonucu

Sonuç türü giriş Çözümleyicisi türüne bağlı olduğunu unutmayın.

### <a name="tokens-response-json"></a>Belirteçler yanıt (JSON)

Ad | Tür | Açıklama
-----|------|-------------
Sonuç | tümce nesnelerinin listesi | içinde bulunan metinde tanımlanmış cümleyi sınırları |
[x] sonucu. Uzaklık | int | Her cümle başlangıç karakteri uzaklığı |
[x] sonucu. Len | int | Her cümle karakter uzunluğu |
[x] sonucu. Belirteçleri | belirteç nesnelerinin listesi | belirteç sınırları tanımlanmış cümleyi içinde |
[x] sonucu. Belirteçler [y]. Uzaklık | int | belirtecin başlangıç karakteri uzaklığı |
[x] sonucu. Belirteçler [y]. Len | int | belirtecin karakter uzunluğu |
[x] sonucu. Belirteçler [y]. RawToken | string | normalleştirme önce bu belirteci içindeki karakterler |
[x] sonucu. Belirteçler [y]. NormalizedToken | string | Normalleştirilmiş bir form karakterin kullanmak üzere güvenli bir [ayrıştırma ağacı](Constituency-Parsing.md); Örneğin, bir açık parantez karakteri ' (' '- LRB-' olur |

Örnek Giriş: ' Bu bir denemedir. Merhaba.'
Örnek JSON yanıtı:
```json
[
  {
    "Len": 15,
    "Offset": 0,
    "Tokens": [
      {
        "Len": 4,
        "NormalizedToken": "This",
        "Offset": 0,
        "RawToken": "This"
      },
      {
        "Len": 2,
        "NormalizedToken": "is",
        "Offset": 5,
        "RawToken": "is"
      },
      {
        "Len": 1,
        "NormalizedToken": "a",
        "Offset": 8,
        "RawToken": "a"
      },
      {
        "Len": 4,
        "NormalizedToken": "test",
        "Offset": 10,
        "RawToken": "test"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 14,
        "RawToken": "."
      }
    ]
  },
  {
    "Len": 6,
    "Offset": 16,
    "Tokens": [
      {
        "Len": 5,
        "NormalizedToken": "Hello",
        "Offset": 16,
        "RawToken": "Hello"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 21,
        "RawToken": "."
      }
    ]
  }
]
```


### <a name="pos-tags-response-json"></a>POS etiketleri yanıt (JSON)

Sonuç, dizeleri listesini bir listesidir.
Her bir cümle için listesini POS etiketleri, her bir belirteç için bir POS etiketi yoktur.
Belirteç karşılık gelen her bir POS etiketi bulmak için de simgeleştirme nesne için sorun isteyebilirsiniz.

### <a name="constituency-tree-response-json"></a>Constituency ağaç yanıt (JSON)

Sonucu olan bir dize listesi, girişte her bir cümle için bir ayrıştırma ağacı.
Ayrıştırma ağaçlarını, parantez içine alınmış biçimde temsil edilir.

## <a name="example"></a>Örnek

`POST /analyze`

İstek gövdesi: JSON yükü
```json
{
  "language": "en",
  "analyzerIds": [
    "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
    "22A6B758-420F-4745-8A3C-46835A67C0D2" ],
  "text": "Hi, Tom! How are you today?"
}
```

Yanıt: JSON
```json
[
  {
    "analyzerId": "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
    "result": [ ["NNP",",","NNP","."], ["WRB","VBP","PRP","NN","."] ]
  },
  {
    "analyzerId": "22A6B758-420F-4745-8A3C-46835A67C0D2",
    "result":["(TOP (S (NNP Hi) (, ,) (NNP Tom) (. !)))","(TOP (SBARQ (WHADVP (WRB How)) (SQ (VP (VBP are)) (NP (PRP you)) (NN today) (. ?))))"]
  }
]
```
