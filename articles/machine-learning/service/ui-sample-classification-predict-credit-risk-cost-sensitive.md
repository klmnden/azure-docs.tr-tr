---
title: 'Sınıflandırma: (Maliyet hassas) kredi riskini tahmin'
titleSuffix: Azure Machine Learning service
description: Bu görsel arabirim örnek deneyde maliyete duyarlı ikili sınıflandırma gerçekleştirmek için özelleştirilmiş bir Python betiğini nasıl yapılacağı açıklanır. Kredi riski kredi uygulamasında sağlanan bilgilere dayanarak tahmin eder.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: 433c258f86705f66e0163100407be7996d68bc6b
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65440967"
---
# <a name="sample-4---classification-predict-credit-risk-cost-sensitive"></a>4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin

Bu görsel arabirim örnek deneyde maliyete duyarlı ikili sınıflandırma gerçekleştirmek için özelleştirilmiş bir Python komut dosyası kullanmayı gösterir. Pozitif örnekleri misclassifying maliyeti negatif örnekleri misclassifying beş kez maliyeti.

Bu örnek, bilgilere dayanarak kredi riskini misclassification maliyetleri dikkate alarak, bir kredi uygulamasında sağlanan tahmin eder.

Bu deneyde Biz bu sorunu çözmek için modeller oluşturmak için iki farklı yaklaşım karşılaştırın:

- Özgün veri kümesi ile eğitim.
- Çoğaltılmış bir veri kümesiyle eğitim.

Her iki yaklaşım ile biz sonuçları Maliyet işleviyle hizalandığından emin olmak için çoğaltma ile test veri kümesini kullanarak modeli değerlendirin. Her iki yaklaşım ile iki sınıflandırıcı test ederiz: **İki sınıflı destekli vektör makinesi** ve **iki sınıflı Artırmalı karar ağacı**.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** düğmesi için örnek 4 deneme:

    ![Denemeyi açın](media/ui-sample-classification-predict-credit-risk-cost-sensitive/open-sample4.png)

## <a name="related-sample"></a>İlgili örnek

Bkz: [3 - sınıflandırma örneği: Kredi riski tahmini (Temel)](ui-sample-classification-predict-churn.md) Bu deney olarak aynı sorunu çözebilecek temel bir deneme için misclassification maliyetleri olmadan ayarlama.

## <a name="data"></a>Veriler

UC Irvine depodan Almanca kredi kartı veri kümesi kullanıyoruz. Bu veri kümesi 20 özellikleri ve 1 etiket 1.000 örnekleri içerir. Her örnek, bir kişiyi temsil eder. 20 özellikler, sayısal ve kategorik özelliklerini içerir. Bkz: [UCI Web sitesi](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) veri kümesi hakkında daha fazla bilgi. Kredi riski gösterir ve yalnızca iki olası değerler içeren etiket son sütundur: yüksek kredi riski = 2 ve düşük kredi riski = 1.

## <a name="experiment-summary"></a>Deneme özeti

1 düşük riskli Örneğin yüksek misclassifying maliyeti, yüksek riskli bir örnek olarak düşük misclassifying maliyeti 5'tir. Kullandığımız bir **Python betiği yürütme** maliyet bu misclassification için hesap modülü.

Denemeyi grafiği aşağıda verilmiştir:

[![Denemeyi grafiği](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="data-processing"></a>Veri işleme

Kullanarak başlatın **meta verileri Düzenleyicisi** daha anlamlı adlar ile varsayılan sütun adlarını değiştirmek için sütun adları eklemek için modülü alınan UCI sitesinde veri kümesi açıklamasından. Yeni sütun adlarının virgülle ayrılmış değerler olarak sağladığımız **yeni bir sütun** ad alanının **meta verileri Düzenleyicisi**.

Ardından, biz eğitim oluşturmak ve riskleri tahmin modeli geliştirmek için kullanılan ayarlar test edin. Biz özgün veri kümesini kullanarak aynı boyutta eğitim ve test kümesi bölme **verileri bölme** modülü. Eşit boyutta kümeleri oluşturmak için ayarladığımız **ilk çıkış veri kümesinde satır kesiri** 0,5 seçeneği.

### <a name="generate-the-new-dataset"></a>Yeni veri kümesi oluştur

Risk küçümsüyor maliyeti yüksek olduğundan, bu gibi misclassification maliyetini ayarlarız:

- Düşük riskli olarak bildireceğinizi yüksek riskli çalışmaları için: 5
- Yüksek riskli bildireceğinizi düşük riskli çalışmaları için: 1

Bu maliyet işlevi yansıtmak için size yeni bir veri kümesi oluşturur. Yeni veri kümesi, yüksek riskli her örnek beş kez çoğaltılır, ancak düşük riskli örnekleri sayısı değişmez. Eğitim ve test veri kümeleri çoğaltma aynı satırda hem kümelerinde engellemek için önce oturum verileri bölün.

Yüksek riskli veri çoğaltmak için Biz bu Python kodu içine koyun bir **Python betiği yürütme** Modülü:

```
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    df_label_1 = dataframe1[dataframe1.iloc[:, 20] == 1]
    df_label_2 = dataframe1[dataframe1.iloc[:, 20] == 2]

    result = df_label_1.append([df_label_2] * 5, ignore_index=True)
    return result,
```

**Python betiği yürütme** modülü çoğaltır eğitim ve test veri kümeleri.

### <a name="feature-engineering"></a>Özellik mühendisliği

**İki sınıflı destekli vektör makinesi** algoritması normalleştirilmiş veri gerektirir. Kullanacağız **Normalleştir veri** tüm sayısal özelliklerle aralıkları'leri normalleştirmek için modülü bir `tanh` dönüştürme. A `tanh` dönüştürme dönüştürür tüm sayısal özellik değerleri 0 ile 1 aralığında değerleri genel dağılımını korur.

**İki sınıflı destekli vektör makinesi** modülü dize özellikleri, bunları kategorik özellikleri ve ardından ikili özellikleri 0 veya 1 değerini dönüştürme işler. Bu nedenle bu özellikleri'leri normalleştirmek gerekmez.

## <a name="models"></a>Modeller

Şu iki sınıflandırıcı uyguladıklarından **iki sınıflı destekli vektör makinesi** (SVM) ve **iki sınıflı artırılmış karar ağacı**ve iki veri kümesi de, farklı olan dört model toplam oluşturduğumuz:

- SVM özgün verilerle eğitilir.
- Çoğaltılan verileri ile eğitilmiş SVM.
- Özgün verilerle artırmalı karar ağacı eğitilir.
- Çoğaltılan verileri ile eğitilmiş artırmalı karar ağacı.

Standart Deneysel iş akışı oluşturmak, eğitmek ve modelleri test etmek için kullanırız:

1. Kullanarak öğrenimi algoritmalarını başlatmak **iki sınıflı destekli vektör makinesi** ve **iki sınıflı artırılmış karar ağacı**.
1. Kullanım **modeli eğitme** algoritma verilere uygulamak ve gerçek model oluşturmak için.
3. Kullanım **Score Model** puanları test örnekleri kullanarak oluşturmak için.

Aşağıdaki diyagramda, özgün ve çoğaltılan eğitim kümeleri iki farklı SVM modeli eğitmek için kullanılan bu deneyde, bir bölümü gösterilmektedir. **Modeli eğitme** eğitim kümesine bağlı olduğu ve **Score Model** test kümesine bağlıdır.

![Deneme grafiğini](media/ui-sample-classification-predict-credit-risk-cost-sensitive/score-part.png)


Deneme değerlendirme aşamasında size her biri olan dört model doğruluğunu işlem. Bu deneme için kullandığımız **Evaluate Model** aynı misclassification sahip örnekler karşılaştırmak için maliyet.

**Evaluate Model** modülü kadar iki puanlanmış modelleri için performans ölçümlerini işlem. Bir örneğini kullanacağız **Evaluate Model** iki SVM modelleri ve başka bir örneğinin değerlendirilecek **Evaluate Model** iki artırılmış karar ağacı modeli değerlendirilecek.

Yinelenen test veri kümesi için giriş olarak kullanıldığını fark **Score Model**. Diğer bir deyişle, son doğruluğu puanları etiketler yanlış alma maliyetini içerir.

## <a name="combine-multiple-results"></a>Birden çok sonuçları birleştirme

**Evaluate Model** modül çeşitli ölçümleri içeren tek bir satır içeren bir tablo oluşturur. Tek bir doğruluk sonuç kümesini oluşturmak için önce kullandığımız **Add Rows** tek bir tabloda birleştirme sonuçları için. Ardından, aşağıdaki Python betiğini kullanıyoruz **Python betiği yürütme** modülü sonuç tablosunda her satır için eğitim yaklaşım ve model adını eklemek için:

```
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    new_cols = pd.DataFrame(
            columns=["Algorithm","Training"],
            data=[
                ["SVM", "weighted"],
                ["SVM", "unweighted"],
                ["Boosted Decision Tree","weighted"],
                ["Boosted Decision Tree","unweighted"]
            ])

    result = pd.concat([new_cols, dataframe1], axis=1)
    return result,
```


## <a name="results"></a>Sonuçlar

Deneme sonuçlarını görüntülemek için son Görselleştir çıktısını sağ tıklayabilirsiniz **kümesindeki sütunları seçme** modülü.

![Çıkışı Görselleştirme](media/ui-sample-classification-predict-credit-risk-cost-sensitive/result.png)

İlk sütun, machine learning modeli oluşturmak için kullanılan algoritma listeler.
İkinci sütunda eğitim kümesine türünü belirtir.
Üçüncü sütunda maliyete duyarlı doğruluk değeri içerir.

Bu sonuçlardan ile oluşturulmuş model en yüksek doğruluk sağladığı görebilirsiniz **iki sınıflı destekli vektör makinesi** ve eğitilen çoğaltılmış bir eğitim veri kümesi üzerinde.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [Örnek 1 - regresyon: Otomobilin fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [2 - regresyon. örnek: Otomobil fiyat tahmini için algoritmalar karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
- [5 - sınıflandırma. örnek: Dalgalanmasını tahmin](ui-sample-classification-predict-churn.md)
