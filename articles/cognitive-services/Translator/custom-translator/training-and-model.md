---
title: Eğitim ve modeli nedir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Belirli bir dil çifti için çeviri sağlayan sistem modelidir. Başarılı bir eğitim sonucunu bir modeldir. Bir modeli eğitimindeki üç birbirini dışlayan veri kümeleri eğitim veri kümesi, veri kümesi ayarlama ve veri kümesi test gereklidir.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-pawal
ms.openlocfilehash: 530e87a84899b9659acd19f90c7c30ad3da3e7ba
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382302"
---
# <a name="what-are-trainings-and-models"></a>Gelişmeleri yakından izleyin ve modelleri nelerdir?

Belirli bir dil çifti için çeviri sağlayan sistem modelidir.
Başarılı bir eğitim sonucunu bir modeldir. Bir modeli eğitimindeki üç birbirini dışlayan veri kümeleri gereklidir: eğitim veri kümesi, veri kümesi ayarlama ve sınama veri kümesi. Sözlük verileri de sağlanabilir.

Eğitim verileri, bir eğitim sıraya alırken sağlanır, yalnızca özel Translator otomatik olarak ayarlama ve test derleyin. Bu, eğitim verilerinizden 5.000 cümleler hariç ve 2.500 ayarlama bir araya getirmek için her ve test kümelerini kullanın.

## <a name="training-dataset-for-custom-translator"></a>Özel Translator için eğitim veri kümesi

Eğitim kümesi dahil belgeleri, modelinizi oluşturmak için özel Translator tarafından temel olarak kullanılır. Eğitim yürütme sırasında bu belgelerde bulunan cümleler hizalı (eşleştirilmiş veya). Eğitim belgeleri kümenizi oluşturma içinde liberties alabilir. Ekleyebileceğiniz olduğuna inanıyorsanız belgelerdir tangential ilgi bir modeli. Yeniden etkileri görmek için başka bir dışlama [BLEU (iki dilli değerlendirme Understudy) puanı](what-is-bleu-score.md). Ayar kümesi ve test kümesi sabit tutmak sürece, oluşumunu Eğitim kümesi ile denemeler yapın çekinmeyin. Bu yaklaşım, çeviri sisteminizi kalitesini değiştirmek için etkili bir yoludur.

Bir projede birden çok eğitimleri çalıştırın ve karşılaştırma [BLEU puanları](what-is-bleu-score.md) tüm eğitim çalıştırmaları arasında. Karşılaştırma için birden çok eğitimleri çalıştırırken, aynı ayarlama sağlamak / test verileri her zaman belirtilir. Ayrıca ayrıca sonuçları el ile İnceleme emin ["Test"](how-to-view-system-test-results.md) sekmesi.

## <a name="tuning-dataset-for-custom-translator"></a>Veri kümesi için özel Translator ayarlama

Bu kümede yer paralel belgeler özel Translator tarafından en iyi sonuçlar için çeviri sistemi ayarlamak için kullanılır.

Ayar kümesi eğitim sırasında tüm parametreleri ve ağırlıklarını çeviri sistemin en uygun değerlere ayarlamak için kullanılır. Ayarlamayı kümesini dikkatle seçin: ayar kümesi gelecekte çevirmek için istediğinize belgelerin içeriğini olması gerekir. Ayar kümesi üretilen çevirilerin kalitesini üzerinde önemli bir etkisi vardır. Ayarlama örnekleri ayarlama kümesinde sağlamak için en yakın çevirileri sağlamak çeviri sistemi sağlar. Küme ayarlama olarak 2500'den fazla cümleler gerekmez. En uygun çeviri kalitesi için en iyi temsil seçimi tümcelerin seçerek ayarlama kümesi el ile seçmek için önerilir.

Çevrilecek beklediğiniz gelecekteki cümleleri anlamlı ve temsili bir uzunluğu cümleler ayarlama kümenizi oluştururken seçin. Ayrıca, sözcükleri ve gelecekteki çevirilerinizi beklediğiniz yaklaşık dağıtımında çevirmek için istediğinize tümcecikleri cümleler seçmeniz gerekir. Bu cümleleri dönüm Göster ve önemli, fazla karmaşık olmadan bir ifade uzunluğu sağlamak için yeterli bağlam içerdiğinden uygulamada, bir cümle uzunluğu 8 için 18 yaşında sözcük En iyi sonuçlar üretir.

Prose ayarlama kümesinde kullanmak için cümleleri türünü iyi bir açıklaması verilmektedir: Gerçek fluent cümleleri. Tablo hücrelerini, değil edersiniz, nesnelerin interneti, yalnızca noktalama listelerin veya değil bir cümle - normal dil numaraları.

Ayarlama Veri kümenizi el ile seçerseniz, eğitim ve test verilerinizi olarak aynı cümle hiçbirini olmamalıdır. Ayar kümesi çevirilerin kalitesini üzerinde önemli bir etkisi vardır - cümleleri dikkatle seçin.

Neyi seçeceğinizi ayarlama kümeniz için emin değilseniz, yalnızca Eğitim kümesi seçin ve özel Translator'ın sizin için ayarlama kümenizi seçin olanak tanır. Özel ayar kümesi otomatik olarak seçmesine Translator izin verdiğinizde, iki dilli eğitim belgelerden cümleleri rastgele bir alt kümesi kullanın ve bu cümleleri eğitim malzemesi kendisi hariç.

## <a name="testing-dataset-for-custom-translator"></a>Özel Translator için sınama veri kümesi

Sınama kümesine dahil paralel belgeleri BLEU (iki dilli değerlendirme Understudy) puanını hesaplamak için kullanılır. Bu puanı çeviri sisteminizi kalitesini gösterir. Bu puanı gerçekten size sunulan bu eğitimi kaynaklanan çeviri sistemi tarafından gerçekleştirilen çevirileri sınama veri kümesi başvurusu cümleleri ne kadar yakın eşleşen bildirir.

BLEU puanı, otomatik çeviri ve başvuru çevirisi arasında delta ölçüsüdür. Kendi aralıklarını 0-100. Bir puan 0 başvuru değil tek bir sözcük çeviriyi görüntülendiğini belirtir. Bir otomatik çeviri başvuru tam olarak eşleşen bir puan 100 gösterir: aynı kelimenin tam aynı konumdadır. Aldığınız puan test kümesinin tüm cümleleri BLEU puanı ortalamadır.

Sınama kümesi, hedef dil cümleleri çiftindeki karşılık gelen kaynak dili tümcelerin en çok istenen çevirileri nerede paralel belgeleri içermelidir. Ayar kümesi oluşturmak için kullanılan aynı kriteri kullanmak isteyebilirsiniz. Ancak, test kümesi çeviri sistem kalitesini üzerinde hiçbir etkisi yoktur. BLEU puanı, için ve başka hiçbir şey için özel olarak oluşturmak için kullanılır.

Test kümesi olarak 2.500 cümleler gerekmez. Sistem sınama kümesine otomatik olarak seçmesine izin verdiğinizde, bu, iki dilli eğitim belgelerden cümleleri rastgele bir alt kümesi kullanın ve bu cümleleri eğitim malzemesi kendisini dışarıda.

Test kümesi özel çevirileri görüntüleyebilir ve bunları model içindeki test sekmesine giderek test kümenizdeki, sağlanan çevirileri karşılaştırın.
