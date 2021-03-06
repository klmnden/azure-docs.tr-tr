---
title: 'Sınıflandırma: Değişim sıklığı, deneyde ve yukarı satış tahmin edin '
titleSuffix: Azure Machine Learning service
description: Bu görsel arabirim örnek deneyde, değişim sıklığı, müşteri ilişkileri yönetimi (CRM) için genel bir görev ikili dosya sınıflandırıcı tahminini gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 7d10d996febd0e31c9085bf5cb82324cce101c80
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606150"
---
# <a name="sample-5---classification-predict-churn-appetency-and-up-selling"></a>5 - sınıflandırma. örnek: Değişim sıklığı, deneyde ve yukarı satış tahmin edin 

Tek satırlık bir görsel arabirim kullanarak kod yazmadan karmaşık machine learning denemesi oluşturmayı öğrenin.

Bu deneyde, üç eğitir **iki sınıflı artırmalı karar ağacı** Müşteri İlişkileri Yönetimi (CRM) sistemleri için ortak görevler tahmin etmek için sınıflandırıcılar: değişim sıklığı, deneyde ve yukarı satış. Etiketleri ve veri değerleri birden çok veri kaynağında bölme ve müşteri bilgilerinin anonimleştirmek için karıştırılmış, ancak yine de görsel arabirim karıştırılmış değerleri kullanarak bir model eğitip ve veri kümeleri birleştirmek için kullanabiliriz.

"Hangisinin?" sorusunu deniyoruz nedeni Bu, bir sınıflandırma problemi olarak adlandırılır. Ancak, bu deneme regresyon, Sınıflandırma, kümeleme ve benzeri oluşmasından herhangi bir türde machine learning sorun gidermek için aynı adımları uygulayabilirsiniz.

Bu deneme için tamamlanan grafiği aşağıda verilmiştir:

![Deneme grafiğini](./media/ui-sample-classification-predict-churn/experiment-graph.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** örnek 5 deneme düğmesi.

    ![Denemeyi açın](media/ui-sample-classification-predict-churn/open-sample5.png)

## <a name="data"></a>Data

Bu deneme için kullandığımız KDD Kupası 2009 ' verilerdir. 50.000 satırları ve 230 özellik sütunları bir veri kümesine sahiptir. Değişim sıklığı, deneyde ve bu özellikleri kullanan müşteriler için yukarı satış tahmin etmek için bir görevdir. Veri ve görev hakkında daha fazla bilgi için bkz: [KDD Web sitesi](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="experiment-summary"></a>Deneme özeti

Bu görsel arabirim örnek deneyde, ikili dosya sınıflandırıcı tahmin değişim sıklığı, deneyde ve yukarı-satış, müşteri ilişkileri yönetimi (CRM) için ortak bir görevi gösterir.

İlk olarak, bazı basit veri işleme desteklemiyoruz.

- Ham veri kümesi eksik değerleri çok sayıda içerir. Kullandığımız **eksik verileri temizleme** değerleri 0 ile modülü eksik değiştirin.

    ![Veri kümesi Temizle](./media/ui-sample-classification-predict-churn/cleaned-dataset.png)

- Özellikleri ve karşılık gelen, deneyde, karmaşıklığı ve farklı veri kümelerini olan yukarı satış etiketleri. Kullandığımız **Sütun Ekle** modülü için özellik sütunları etiket sütunlar eklenecek. İlk sütun **Col1**, etiket sütundur. Sütunlara rest **Var1**, **Var2**ve benzeri özellik sütunlarıdır.

    ![Sütun veri kümesi Ekle](./media/ui-sample-classification-predict-churn/added-column1.png)

- Kullandığımız **verileri bölme** train veri kümesi bölünemiyor ve kümeleri test etmek için modül.

    Ardından artırılmış karar ağacı ikili dosya sınıflandırıcı varsayılan parametreleri tahmin modellerini oluşturmak için kullanıyoruz. Her görev, diğer bir deyişle, bir model yukarı satış, deneyde ve karmaşıklığı tahmin etmek için her bir model ekleriz.

## <a name="results"></a>Sonuçlar

Çıkışı görselleştirme **Evaluate Model** test kümesi üzerinde model performansını görmek için modülü. Yukarı satış görev için model rastgele bir modeli daha iyi yaptığı ROC eğrisi gösterir. Eğriyi (AUC) altında 0.857 alanıdır. Daha Eşikte 0,5, duyarlık 0,7 olduğundan, geri çağırma 0.463, ve F1 puanı 0.545.

![Sonuçları değerlendirin](./media/ui-sample-classification-predict-churn/evaluate-result.png)

 Taşıyabilir **eşiği** kaydırıcı ve ölçümleri değiştirmek için ikili sınıflandırma görevi bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [Örnek 1 - regresyon: Otomobilin fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [2 - regresyon. örnek: Otomobil fiyat tahmini için algoritmalar karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
- [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Örnek 6 - sınıflandırma: Uçuş gecikme tahmin edin](ui-sample-classification-predict-flight-delay.md)