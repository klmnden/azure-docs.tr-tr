---
title: Toplu test LUIS uygulamanızı - Azure | Microsoft Docs
description: Toplu test uygulama geliştirebilirsiniz ve kendi dil anlama geliştirmek için sürekli olarak çalışmak için kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/05/2018
ms.author: v-geberr
ms.openlocfilehash: f0366e805c9ae809a2800b0f4be53d08d9fc3d60
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857917"
---
# <a name="batch-testing-in-luis"></a>Batch LUIS test etme

Toplu test doğrular, [etkin](luis-concept-version.md#active-version) tahmin doğruluğunun ölçmek için eğitilen modeli. Toplu test geçerli eğitilen modelinizde grafikteki her bir hedefi ve varlık doğruluğunu görüntülemenize yardımcı olur. Uygulamanız için doğru amacını tanımlamak sık sık başarısız olursa bir amaç için daha fazla örnek Konuşma ekleme gibi doğruluğunu, artırmak için uygun eylemde toplu test sonuçlarını gözden geçirin.

## <a name="group-data-for-batch-test"></a>Toplu test grubu verileri
Toplu test etmek için kullanılan konuşma LUIS için yeni önemlidir. Bir veri kümesi konuşma varsa, konuşma üç kümelerine ayırmak: bir amaç için eklenen konuşma, yayımlanan uç noktasından alınan konuşma ve onu eğitildi sonra LUIS toplu test için kullanılan konuşma. 

## <a name="a-dataset-of-utterances"></a>Konuşma bir veri kümesi
Bir toplu iş dosyası olarak bilinen, konuşma, gönderme bir *veri kümesi*, batch test etmek için. Veri kümesi en fazla 1.000 etiketli içeren JSON biçimli bir dosya olan **yinelenmeyen** konuşma. 10 adede kadar veri kümeleri, bir uygulamada test edebilirsiniz. Daha fazla test etmek gerekiyorsa, bir veri kümesini silin ve ardından yeni bir tane ekleyin.

|**Kuralları**|
|--|
|* Hiçbir yinelenen konuşma|
|Hiyerarşik varlık alt öğesi yok|
|Konuşma 1000 veya daha az|

* Çoğaltmaları tam dize eşleşmeleri, ilk simgeleştirilmiş eşleşme olarak kabul edilir. 

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Toplu dosya biçimi
Toplu iş dosyası konuşma oluşur. Her utterance yanı sıra herhangi bir beklenen hedefi tahmin olmalıdır [makine öğrenilen varlıklar](luis-concept-entity-types.md#types-of-entities) algılanamayacak kadar bekler. 

Bir toplu iş dosyası örneği aşağıdadır:

   [!code-json[Valid batch test](~/samples-luis/documentation-samples/batch-testing/travel-agent-1.json)]


## <a name="common-errors-importing-a-batch"></a>Sık karşılaşılan toplu içeri aktarma
Sık karşılaşılan hatalar şunlardır: 

> * 1. 000'den fazla konuşma
> * Bir varlık özelliğine sahip olmayan bir utterance JSON nesnesi
> * İçinde birden çok varlık etiketli sözcükler

## <a name="batch-test-state"></a>Toplu test durumu
LUIS, her veri kümesinin son test durumunu izler. Bu, son çalıştırma boyutu (konuşma toplu iş sayısı), tarih ve son sonucu (başarılı bir şekilde tahmin edilen konuşma sayısı) içerir.

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>Toplu test sonuçları
Toplu test sonucu bir hata matris bilinen bir dağılım grafiği olur. Bu dosya, geçerli modelin tahmin edilen amaç ve varlıkları konuşma 4 yönlü karşılaştırması grafiğidir. 

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