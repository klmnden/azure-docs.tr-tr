---
title: Toplu test HALUK uygulamanızı - Azure | Microsoft Docs
description: Sürekli olarak geliştirebilirsiniz ve kendi dil anlama geliştirmek için uygulamanızın üzerinde çalışmak için toplu sınama'ı kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 3803df32d6431b8413e8df0837ed62b2e4344cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353350"
---
# <a name="batch-testing-in-luis"></a>Toplu iş içinde HALUK test etme

Toplu sınama doğrular, [etkin](luis-concept-version.md#active-version) tahmin doğruluğunu ölçmek için eğitilen model. Bir toplu test modelinizdeki geçerli eğitilen grafikteki her bir hedefi ve varlık doğruluğunu görüntülemenize yardımcı olur. Uygulamanız doğru hedefi tanımlamak sık sık başarısız olursa bir amaç için daha fazla örnek utterances ekleme gibi doğruluğunu artırmak için uygun eylemi gerçekleştirin için toplu test sonuçlarını gözden geçirin.

## <a name="group-data-for-batch-test"></a>Toplu test grubu verileri
Toplu test etmek için kullanılan utterances HALUK için yeni önemlidir. Bir veri kümesi utterances, varsa, üç adet içine utterances bölmek: bir amaç için eklenen utterances, yayımlanan uç noktasından alınan utterances ve onu eğitildi sonra toplu test HALUK kullanılan utterances. 

## <a name="a-dataset-of-utterances"></a>Utterances, bir veri kümesi
Bir toplu iş dosyası olarak bilinen utterances, gönderme bir *dataset*, toplu test etmek için. Veri kümesi en çok 1.000 etiketli içeren bir JSON biçimli dosyasıdır **yinelenmeyen** utterances. 10 adede kadar veri kümeleri bir uygulamayı sınayabilirsiniz. Daha fazla test ihtiyacınız varsa, bir veri kümesini silmek ve ardından yeni bir tane ekleyin.

|**Kuralları**|
|--|
|* Hiçbir yinelenen utterances|
|Hiyerarşik varlık alt öğesi|
|1000 utterances veya daha az|

* Çoğaltmaları tam dizeyi eşleşirse, ilk simgeleştirilmiş eşleşme olarak kabul edilir. 

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Toplu iş dosyası biçimi
Toplu iş dosyası utterances oluşur. Her utterance birlikte herhangi bir beklenen hedefi tahmin olmalıdır [makine öğrenilen varlıklar](luis-concept-entity-types.md#types-of-entities) algılanmış bekler. 

Örnek bir toplu iş dosyası aşağıdaki gibidir:

   [!code-json[Valid batch test](~/samples-luis/documentation-samples/batch-testing/travel-agent-1.json)]


## <a name="common-errors-importing-a-batch"></a>Sık karşılaşılan Toplu içe aktarma
Sık karşılaşılan hatalar şunlardır: 

> * 1. 000'den fazla utterances
> * Bir varlık özelliğine sahip olmayan bir utterance JSON nesnesi

## <a name="batch-test-state"></a>Toplu test durumu
HALUK her veri kümesi'nin son test durumunu izler. Bu, son çalıştırma boyutu (utterances toplu iş sayısı), tarih ve son sonucu (başarılı bir şekilde tahmin edilen utterances sayısı) içerir.

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>Toplu test sonuçları
Toplu test sonucu bir hata matris bilinen bir dağılım grafiği olur. Bu dosya, geçerli modelinin tahmin edilen amacını ve varlıkları utterances 4 yönlü karşılaştırması grafiktir. 

Veri noktası **yanlış pozitif** ve **False negatif** bölümleri araştırılmalıdır hataları gösterir. Tüm veri noktaları kullanıyorsanız **True pozitif** ve **True negatif** uygulamanızın doğruluğu bu veri kümesine mükemmeldir sonra bölümler.

![Grafiğin dört bölüm](./media/luis-concept-batch-test/chart-sections.png)

Bu grafik, yanlış üzerinde geçerli kendi eğitim dayalı HALUK tahmin utterances bulmanıza yardımcı olur. Sonuçlar grafik bölge başına görüntülenir. Tek tek noktalarında utterance bilgileri gözden geçirin veya bölgede utterance sonuçlarını gözden geçirmek için bölge adı seçmek için grafiği seçin.

![Toplu test etme](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Sonuçları hataları
Toplu testindeki hataları toplu iş dosyasında belirtildiği gibi tahmin değil hedefleri belirtin. Hataları grafik kırmızı iki bölümlerini belirtilir. 

Yanlış pozitif bölümüne sahip olmamalıdır olduğunda bir utterance bir hedefi veya varlık eşleştiğini gösterir. False negatif olması gereken durumlarda bir utterance bir hedefi veya varlık eşleşmedi gösterir. 

## <a name="fixing-batch-errors"></a>Toplu iş hataları çözme
Hatalar varsa toplu testinde, ya da daha fazla utterances bir hedefi ekleyin ve/veya HALUK hedefleri arasında Ayrımcılığı olun yardımcı olmak için varlığı ile daha fazla utterances etiket. Utterances eklendi ve bunları ve hala get toplu sınama tahmin hataları etiketli olsa eklemeyi göz önünde bir [tümcecik listesi](luis-concept-feature.md) daha hızlı bilgi HALUK yardımcı olmak için etki alanına özgü sözlük özelliğiyle. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [toplu test](luis-how-to-batch-test.md)