---
title: Dil analiz API yönteminde çözümleme | Microsoft Docs
description: Belirli doğal dil girişleri çözümlemenizi dil analiz API Çözümle yöntemi kullanmak nasıl.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 12/13/2016
ms.author: lesun
ms.openlocfilehash: b17a00f31845bfa05572dff7ca94e9a1ffd69586
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351736"
---
# <a name="analyze-method"></a>Analiz yöntemi

**Analiz** REST API verilen doğal dil giriş çözümlemek için kullanılır.
Yalnızca bulma ilgili [cümleleri ve belirteçleri](Sentences-and-Tokens.md) , giriş, bulma içinde [konuşma bölümü etiketleri](POS-tagging.md), veya bulma [constitutency ağaç](Constituency-Parsing.md).
İlgili çözümleyiciler seçerek istediğiniz hangi sonuçları belirtebilirsiniz.
Tüm kullanılabilir çözümleyiciler listelemek için bakmak  **[çözümleyiciler](AnalyzersMethod.md)**.

Giriş dizesi dilini belirtme gerektiğini unutmayın.

**REST uç noktası:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>İstek parametreleri

Ad | Tür | Gerekli | Açıklama
-----|-------|----------|------------
**Dil**    | dize | Evet | İki harfli ISO dil kod çözümleme için kullanılacak. Örneğin, İngilizce "tr" dir.
**analyzerIds** | dize listesi | Evet | Uygulanacak çözümleyiciler GUID'lerini listesi. Daha fazla bilgi için çözümleyiciler belgelerine bakın.
**metin**        | dize | Evet | Çözümlenecek ham giriş. Bu bir sözcük veya tümcecik, tam bir cümle veya tam paragraf veya discourse gibi kısa bir dize olabilir.

<br>
## <a name="response-json"></a>Yanıt (JSON)
Bir dizi çözümlemesi çıkarır, istekte belirtilen her bir öznitelik için bir tane.

Sonuçlar şu şekilde görünür:

Ad | Tür | Açıklama
-----|------|--------------
analyzerId | dize | Belirtilen çözümleyicisinin GUID
Sonuç | object | Çözümleyici sonucu

Sonuç türü giriş Çözümleyicisi türüne bağlı olduğunu unutmayın.

### <a name="tokens-response-json"></a>Belirteçleri yanıt (JSON)

Ad | Tür | Açıklama
-----|------|-------------
Sonuç | tümce nesnelerinin listesi | metnin içinde tanımlanan cümle sınırları |
[x] sonucu. Uzaklık | int | Her tümcenin başlangıç karakteri uzaklığı |
[x] sonucu. Len | int | Her tümcenin karakter cinsinden uzunluğu |
[x] sonucu. Belirteçleri | belirteç nesnelerinin listesi | tümce içinde tanımlanan belirteci sınırları |
[x] sonucu. Belirteçler [y]. Uzaklık | int | belirtecin başlangıç karakteri uzaklığı |
[x] sonucu. Belirteçler [y]. Len | int | belirteç karakter cinsinden uzunluğu |
[x] sonucu. Belirteçler [y]. RawToken | dize | normalleştirme önce bu belirteci içindeki karakterler |
[x] sonucu. Belirteçler [y]. NormalizedToken | dize | Normalleştirilmiş bir form, kullanılmak üzere güvenli şekilde karakterinin bir [ayrıştırma ağacı](Constituency-Parsing.md); örneği için bir açık parantez karakteri ' (' - LRB - olur |

Örnek Giriş: ' bunu denemedir. Merhaba.'
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

Sonuç dizeleri listelerinde bir listesidir.
Her bir cümle için POS etiketlerin listesini, her belirteç bir POS etiketi yok.
Belirteç karşılık gelen her POS etiketi bulmak için de bir simgeleştirme nesnesi isteyin istersiniz.

### <a name="constituency-tree-response-json"></a>Constituency ağaç yanıt (JSON)

Sonucu olan bir dize listesi, girdide bulunan her tümce için bir ayrıştırma ağacı.
Ayrıştırma ağacı parantez içine alınmış biçiminde gösterilir.

<br>

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

