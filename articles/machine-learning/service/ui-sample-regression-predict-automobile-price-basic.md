---
title: 'Regresyon: Fiyat tahmini'
titleSuffix: Azure Machine Learning service
description: Bir machine learning tek satırlık bir kod yazmadan otomobilin fiyatını tahmin modeli oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 9dfa4b62f5cb79a5716f6f29651e85d0f8a3a409
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787852"
---
# <a name="sample-1---regression-predict-price"></a>Örnek 1 - regresyon: Fiyat tahmini

Bir machine learning tek satırlık bir görsel arabirim kullanarak kod yazmadan regresyon modeli oluşturmayı öğrenin.

Bu deneme tren bir **orman regresörü karar** bir araba tahmin etmek için fiyat üreticisi, modeli, beygir gücü ve boyutu gibi teknik özellikleri temel kullanıcının. "Ne kadar?" sorusunu deniyoruz nedeni Bu, regresyon problemi olarak adlandırılır. Ancak, bu deneme regresyon, Sınıflandırma, kümeleme ve benzeri oluşmasından herhangi bir türde machine learning sorun gidermek için aynı temel adımları uygulayabilirsiniz.

Bir eğitim machine learning modeli temel adımları şunlardır:

1. Verileri alma
1. Verileri önceden işleme
1. Modeli eğitme
1. Modeli değerlendirme

Biz üzerinde yoğun şekilde kullanacağınız denemeyi son, tamamlanan grafiği aşağıda verilmiştir. Benzer kararlar kendi kendinize yapabileceğiniz şekilde tüm modüller için stratejinin sağlarız.

![Denemeyi grafiği](media/ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** örnek 1 denemeyi düğmesi:

    ![Denemeyi açın](media/ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="get-the-data"></a>Verileri alma

Bu deneyde kullandığımız **otomobil fiyat verileri (ham)** UCI Machine Learning depodan olan veri kümesi. Veri kümesi, otomobil üreticisi, modeli, fiyat, araç özellikleri (silindir sayısı gibi) MPG ve sigorta risk puanı da dahil olmak üzere ilgili bilgiler içeren 26 sütunlar içeriyor. Bu deneyde amaç, araba fiyatını tahmin etmektir.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayrılma veya boyutlandırma içerir. Bu işlemleri ve diğer veri görevlerinde önceden işleme gerçekleştirmek için modülleri visual arabiriminde bulabilirsiniz **veri dönüştürme** sol bölmedeki grubu.

Kullandığımız **kümesindeki sütunları seçme** normalleştirilmiş birçok eksik değerleri olan kayıplar dışlanacak modülü. Ardından kullandığımız **eksik verileri temizleme** eksik değerler içeren satırları kaldırmak için. Bu, temiz bir eğitim veri kümesi oluşturmak için yardımcı olur.

![Veri ön işleme](./media/ui-sample-regression-predict-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme

Machine learning sorunları farklılık gösterir. Sınıflandırma, kümeleme, regresyon ve her biri farklı bir algoritma gerektirebilir öneren sistemleri, ortak makine öğrenimi görevlerini içerir. Tercih ettiğiniz algoritması, genellikle kullanım örneği gereksinimlerine bağlıdır. Bir algoritma seçin sonra daha doğru bir model eğitip parametrelerini ayarlamak gerekir. Ardından tüm modellerin doğruluğu anlaşılabilirliği ve verimliliği gibi ölçümleri temel değerlendirmesi gerekir.

Bu deneyde amaç, otomobil fiyatlarını tahmin etmek için olduğundan ve gerçek sayılar (Fiyat) etiketi sütun içerdiği için bir regresyon modeli iyi bir seçimdir. Birçok özellik (daha az en az 100) görece küçük olduğunu ve bu özellikleri seyrek olduğunu dikkate alarak, karar sınır doğrusal olması olasıdır. Kullanacağız **karar ormanı regresyon** bu deneme için.

Kullandığımız **verileri bölme** modülü rastgele giriş verileri özgün veri 70 oranında eğitim veri kümesi içerir ve özgün veriler % 30'luk test veri kümesini içeren bölme.

## <a name="test-evaluate-and-compare"></a>Test, değerlendirin ve karşılaştırma

 Veri kümesi bölünemiyor ve farklı veri kümelerini eğitme ve değerlendirme modelin daha fazla hedefi bulunmak üzere modelinizi test etme için kullanırız.

Modeli eğitilir sonra kullandığımız **Score Model** ve **Evaluate Model** tahmini sonuçlar oluşturur ve modelleri değerlendirmek için modüller.

**Modeli Puanlama** eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Sonuç denetlemek için çıkış bağlantı noktasına seçin **Score Model** seçip **Görselleştir**.

![Puan sonucu](./media/ui-sample-regression-predict-automobile-price-basic/score-result.png)

Biz sonra puanlar geçirin **Evaluate Model** değerlendirmesi oluşturmak için modül. Sonuç denetlemek için çıkış bağlantı noktasına seçin **Evaluate Model** seçip **Görselleştir**.

![Değerlendirme sonucu](./media/ui-sample-regression-predict-automobile-price-basic/evaluate-result.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [2 - regresyon. örnek: Otomobil fiyat tahmini için algoritmalar karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
- [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [5 - sınıflandırma. örnek: Dalgalanmasını tahmin](ui-sample-classification-predict-churn.md)