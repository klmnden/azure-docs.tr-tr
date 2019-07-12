---
title: 'Sınıflandırma: Kredi riski tahmini'
titleSuffix: Azure Machine Learning service
description: Tek satırlık bir görsel arabirim kullanarak kod yazmadan sınıflandırıcı öğrenme bir makine oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 6ec91ca83d7aa1bc5e6c290d35b573a60cc0ed19
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605765"
---
# <a name="sample-3---classification-predict-credit-risk"></a>3 - sınıflandırma. örnek: Kredi riski tahmini

Tek satırlık bir görsel arabirim kullanarak kod yazmadan sınıflandırıcı öğrenme bir makine oluşturmayı öğrenin. Bu örnek eğitir bir **iki sınıflı artırmalı karar ağacı** kredi tahmin etmek için iade uygulama bilgilerini kredi geçmişi yaş ve kredi kartı numarası gibi risk (yüksek veya düşük) bağlı.

"Hangisinin?" sorusunu deniyoruz nedeni Bu, bir sınıflandırma problemi olarak adlandırılır. Ancak, regresyon, Sınıflandırma, kümeleme ve benzeri oluşmasından herhangi bir türde machine learning sorun gidermek için aynı temel işlem uygulayabilirsiniz.

Bu deneme için tamamlanan grafiği aşağıda verilmiştir:

![Denemeyi grafiği](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** düğmesi için örnek 3 deneme:

    ![Denemeyi açın](media/ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>İlgili örnek

[4 - sınıflandırma. örnek: Kredi riski tahmini (maliyeti gizli)](ui-sample-classification-predict-credit-risk-cost-sensitive.md) Bu deney olarak aynı sorunu çözdü Gelişmiş bir deneme sunar. Nasıl gerçekleştirileceğini gösterir _maliyet hassas_ kullanarak sınıflandırma bir **Python betiği yürütme** modülü ve iki ikili sınıflandırma algoritmaların performansını karşılaştırın. Sınıflandırma denemeleri oluşturma hakkında daha fazla bilgi edinmek istiyorsanız buna bakın.

## <a name="data"></a>Data

UC Irvine depodan Almanca kredi kartı veri kümesi kullanıyoruz.
Veri kümesi 20 özellikleri ve 1 etiket 1.000 örnekleri içerir. Her örnek, bir kişiyi temsil eder. Özellikler, sayısal ve kategorik özelliklerini içerir. Bkz: [UCI Web sitesi](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) kategorik özelliklerinin anlamını için. Kredi riski gösterir ve yalnızca iki olası değerler içeren etiket son sütundur: yüksek kredi riski = 2 ve düşük kredi riski = 1.

## <a name="experiment-summary"></a>Deneme özeti

Biz, bir deneme oluşturmak için şu adımları izleyin:

1. Almanca kredi kartı UCI veri veri kümesi modülü deneme tuvale sürükleyin.
1. Ekleme bir **meta verileri Düzenle** her sütun için anlamlı adlar ekleyebiliriz. böylece modülü.
1. Ekleme bir **verileri bölme** modülünün eğitim ve test kümesi oluşturun. İlk çıkış veri kümesinde 0,7 için satırlar için kesir değerini ayarlayın. Bu ayar, verilerin %70 modülünün sol bağlantı noktası için çıktı ve doğru bağlantı noktasına rest olacağını belirtir. Eğitim ve test etmek için doğru olanı sol veri kümesi kullanıyoruz.
1. Ekleme bir **iki sınıflı artırılmış karar ağacı** modülü artırmalı karar ağacı Sınıflandırıcısı başlatılamadı.
1. Ekleme bir **modeli eğitme** modülü. Sınıflandırıcı önceki adımdaki sol giriş bağlantı noktasına bağlayın **modeli eğitme**. Eğitim kümesi Ekle (sol çıkış bağlantı noktasına **verileri bölme**) sağ giriş bağlantı noktasına **modeli eğitme**. **Modeli eğitme** sınıflandırıcı eğitme.
1. Ekleme bir **Score Model** modülü ve bağlama **modeli eğitme** ona modülü. Sınama kümesi eklersiniz (sağ bağlantı noktası **verileri bölme**) için **Score Model**. **Score Model** tahminler yapar. Tahminler ve pozitif sınıfı olasılıklar görmek için çıkış bağlantı noktasını seçebilirsiniz.
1. Ekleme bir **Evaluate Model** modülü ve puanlanmış veri kümesi sol giriş bağlantı noktasına bağlayın. Değerlendirme sonuçlarını görmek için çıkış bağlantı noktasına seçin **Evaluate Model** modülü ve select **Görselleştir**.

Eksiksiz bir deneme grafiğini şu şekildedir:

![Denemeyi grafiği](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="results"></a>Sonuçlar

![Sonuçları değerlendirin](media/ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

Değerlendirme sonuçları, modelin AUC 0.776 olduğunu görebilirsiniz. Daha Eşikte 0,5, duyarlık 0.621 olduğundan, geri çağırma 0.456, ve F1 puanı 0.526.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [Örnek 1 - regresyon: Otomobilin fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [2 - regresyon. örnek: Otomobil fiyat tahmini için algoritmalar karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [5 - sınıflandırma. örnek: Dalgalanmasını tahmin](ui-sample-classification-predict-churn.md)
- [Örnek 6 - sınıflandırma: Uçuş gecikme tahmin edin](ui-sample-classification-predict-flight-delay.md)