---
title: Toplu işe testi
titleSuffix: Language Understanding - Azure Cognitive Services
description: Toplu test uygulama geliştirebilirsiniz ve kendi dil anlama geliştirmek için sürekli olarak çalışmak için kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: diberry
ms.openlocfilehash: acb561970b6a8576d1219fc15758e21a3032c9e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813314"
---
# <a name="batch-testing-with-1000-utterances-in-luis-portal"></a>Batch ile 1000 konuşma LUIS portalında test etme

Toplu test doğrular, [etkin](luis-concept-version.md#active-version) tahmin doğruluğunun ölçmek için eğitilen modeli. Toplu test geçerli eğitilen modelinizde grafikteki her bir hedefi ve varlık doğruluğunu görüntülemenize yardımcı olur. Uygulamanız için doğru amacını tanımlamak sık sık başarısız olursa bir amaç için daha fazla örnek Konuşma ekleme gibi doğruluğunu, artırmak için uygun eylemde toplu test sonuçlarını gözden geçirin.

## <a name="group-data-for-batch-test"></a>Toplu test grubu verileri

Toplu test etmek için kullanılan konuşma LUIS için yeni önemlidir. Bir veri kümesi konuşma varsa, konuşma üç kümelerine ayırmak: bir amaç için eklenen konuşma, yayımlanan uç noktasından alınan konuşma ve onu eğitildi sonra LUIS toplu test için kullanılan konuşma. 

## <a name="a-dataset-of-utterances"></a>Konuşma bir veri kümesi

Bir toplu iş dosyası olarak bilinen, konuşma, gönderme bir *veri kümesi*, batch test etmek için. Veri kümesi en fazla 1.000 etiketli içeren JSON biçimli bir dosya olan **yinelenmeyen** konuşma. 10 adede kadar veri kümeleri, bir uygulamada test edebilirsiniz. Daha fazla test etmek gerekiyorsa, bir veri kümesini silin ve ardından yeni bir tane ekleyin.

|**kuralları**|
|--|
|\* Hiçbir yinelenen konuşma|
|Konuşma 1000 veya daha az|

\* Çoğaltmaları tam dize eşleşmeleri, ilk simgeleştirilmiş eşleşme olarak kabul edilir. 

## <a name="entities-allowed-in-batch-tests"></a>Batch testlerinde izin verilen varlıklar

Toplu dosya verilerine karşılık gelen hiçbir varlık olsa modelinde tüm özel varlıklar toplu test varlıkları filtrede görünür.

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>

## <a name="batch-file-format"></a>Toplu dosya biçimi

Toplu iş dosyası konuşma oluşur. Her utterance yanı sıra herhangi bir beklenen hedefi tahmin olmalıdır [makine öğrenilen varlıklar](luis-concept-entity-types.md#types-of-entities) algılanamayacak kadar bekler. 

## <a name="batch-syntax-template-for-intents-with-entities"></a>Varlıklarla hedefleri için toplu iş söz dizimini şablonu

Toplu iş dosyasında başlatmak için aşağıdaki şablonu kullanın:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": 
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

Toplu iş dosyasını kullanan **startPos** ve **endPos** özellikleri başlangıcını ve bitişini bir varlığın unutmayın. Değerleri sıfır tabanlı olduklarını ve başlamamalı veya boşluk ile bitemez. Bu, startIndex ve endIndex özelliklerini kullanma sorgu günlüklerinden farklıdır. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="batch-syntax-template-for-intents-without-entities"></a>Varlıkları olmadan hedefleri için toplu iş söz dizimini şablonu

Toplu işlem dosyanız varlıkları olmadan başlatmak için aşağıdaki şablonu kullanın:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": []
  }
]
```

Varlıkları test etmek istemiyorsanız dahil `entities` özellik ve değer olarak boş bir dizi kümesi `[]`.


## <a name="common-errors-importing-a-batch"></a>Sık karşılaşılan toplu içeri aktarma

Sık karşılaşılan hatalar şunlardır: 

> * 1\. 000'den fazla konuşma
> * Bir varlık özelliğine sahip olmayan bir utterance JSON nesnesi. Özelliği, boş bir dizi olabilir.
> * İçinde birden çok varlık etiketli sözcükler
> * Başlangıç ve bitiş boşluk varlık etiketi.

## <a name="batch-test-state"></a>Toplu test durumu

LUIS, her veri kümesinin son test durumunu izler. Bu, son çalıştırma boyutu (konuşma toplu iş sayısı), tarih ve son sonucu (başarılı bir şekilde tahmin edilen konuşma sayısı) içerir.

<a name="sections-of-the-results-chart"></a>

## <a name="batch-test-results"></a>Toplu test sonuçları

Toplu test sonucu bir hata matris bilinen bir dağılım grafiği olur. Toplu iş dosyası, geçerli modelin tahmin edilen amaç ve varlıkları konuşma 4 yönlü karşılaştırması grafiğidir. 

Veri noktası üzerinde **yanlış pozitif** ve **False negatif** bölümleri araştırılmalıdır hataları gösterir. Tüm veri noktaları kullanıyorsanız **gerçek pozitif** ve **True negatif** bölümler sonra uygulamanızın doğruluğu, bu veri kümesinde mükemmeldir.

![Grafik dört bölüm](./media/luis-concept-batch-test/chart-sections.png)

Bu grafik, geçerli eğitimle yanlış bağlı öngörür LUIS konuşma bulmanıza yardımcı olur. Sonuçlar, grafiğin bölge başına görüntülenir. Utterance bilgileri gözden geçirin veya bu bölgede utterance sonuçlarını gözden geçirmek için bölge adı seçmek için grafik üzerinde tek tek noktaları seçin.

![Toplu işe testi](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Hata sonuçları

Toplu test hataları, toplu iş dosyasında belirtildiği gibi tahmin değil hedefleri belirtin. Hataları, grafiğin kırmızı iki bölümde gösterilir. 

Yanlış pozitif bölümüne sahip olmamalıdır, bir utterance bir amacı ya da varlık eşleştiğini gösterir. False negatif sahip olmalıdır, bir utterance bir amacı ya da varlık eşleşmedi gösterir. 

## <a name="fixing-batch-errors"></a>Toplu iş hataları düzeltme

Hatalar varsa batch testinde, ya da bir amaç için daha fazla Konuşma ekleme ve/veya daha fazla konuşma varlıkla LUIS amaçları arasında Ayrımcılığı olun yardımcı olmak için etiket. Konuşma eklediniz ve onları ve hala get toplu test hataları tahmin etiketli olsa eklemeyi göz önünde bir [tümcecik listesi](luis-concept-feature.md) özelliğiyle daha hızlı bilgi LUIS yardımcı olmak için etki alanına özel sözlük. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [toplu test](luis-how-to-batch-test.md)
