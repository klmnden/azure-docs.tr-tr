---
title: 'Sınıflandırma: Uçuş rötarları tahmini'
titleSuffix: Azure Machine Learning service
description: Bu makalede bir machine learning modeli görsel sürükle ve bırak arabirimi ve özel R kodunu kullanarak uçuş gecikme tahmin etmek için derleme gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 07/02/2019
ms.openlocfilehash: 773e55fe4b5ca5acf27ba1765e5a16075f625187
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607643"
---
# <a name="sample-6---classification-predict-flight-delays-using-r"></a>Örnek 6 - sınıflandırma: R kullanarak uçuş gecikme tahmin edin

Bu deneyde, geçmiş uçuş ve hava durumu verileri 15 dakikadan fazla geciktirilecek tarifeli uçuş tahmin etmek için kullanır.

Bu sorun, iki sınıf--geciktirilmiş tahmin etmeye yönelik bir sınıflandırma problemi olarak veya zamanında yaklaşıldığında. Sınıflandırıcı, çok sayıda örneklerden geçmiş uçuş verileri kullanarak bu modeli oluşturmak için.

Son deneme grafiği aşağıda verilmiştir:

[![Denemeyi grafiği](media/ui-sample-classification-predict-flight-delay/experiment-graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** düğmesi için örnek 6 deneme:

    ![Denemeyi açın](media/ui-sample-classification-predict-flight-delay/open-sample6.png)

## <a name="get-the-data"></a>Verileri alma

Bu deneme kullandığı **uçuş gecikme verilerini** veri kümesi. Gereksinimlerimizim bir ABD TranStats veri koleksiyonunu bir parçasıdır Ulaştırma Bakanlığı. Veri kümesi, Ekim 2013'e Nisan uçuş gecikme bilgilerini içerir. Veriler için görsel arabirimle karşıya yüklemeden önce bunu şu şekilde ön işlemden geçtikten:

* 70'ten en yoğun saatinde havaalanları Kıta Amerika Birleşik Devletleri'nde içerecek şekilde filtrelenmiş'i tıklatın.
* İçin 15'ten fazla dakikaya Gecikmeli olarak relabeled uçuşlar, iptal edildi.
* Yolu saptırabilir uçuşlar filtre.
* 14 sütun seçilmedi.

Uçuş verileri desteklemek üzere **hava durumu Dataset** kullanılır. Hava Durumu verilerini saatlik land bulunduğunuz gözlemleri noaa'ya içeren ve aynı Nisan-Ekim 2013 dönemini kapsayan havaalanı hava durumu istasyonu gelen gözlemler temsil eder. Azure ML görsel arabirim olarak karşıya yüklemeden önce aşağıdaki gibi önceden işlendi:

* Hava durumu istasyonu kimlikleri karşılık gelen havaalanı kimlikleri eşlendi.
* Hava durumu istasyonu 70'ten en yoğun saatinde havaalanları ile ilişkili olmayan kaldırıldı.
* Tarih sütununu ayrı sütunlara bölme: Yıl, ay ve gün.
* 26 sütun seçilmedi.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Bir veri kümesi analiz edilmeden önce bazı ön işleme genellikle gerektirir.

![veri işlem](media/ui-sample-classification-predict-flight-delay/data-process.png)

### <a name="flight-data"></a>Uçuş verileri

Sütunları **taşıyıcı**, **OriginAirportID**, ve **DestAirportID** tamsayı olarak kaydedilir. Kullanın, ancak bunlar kategorik öznitelikleri hedeflenmiştir **meta verileri Düzenle** bunları için kategorik dönüştürmek için modülü.

![meta veri düzenleme](media/ui-sample-classification-predict-flight-delay/edit-metadata.png)

Ardından **Sütunları Seç** olası hedef leakers olan dataset sütunları dışlamak için veri kümesi modülünde: **DepDelay**, **DepDel15**, **ArrDelay**, **iptal**, **yıl**. 

Saatlik hava durumu kayıtları uçuş kayıtlarla katılmak için zamanlanmış Kalkış Saati birleştirme anahtarlarından birini kullanın. Birleştirme yapmak için CSRDepTime sütunu tarafından gerçekleştirilir en yakın saate yuvarlanır gerekir **R betiği yürütme** modülü. 

### <a name="weather-data"></a>Hava durumu verileri

Büyük kısmı eksik değerlere sahip sütun kullanarak dışlanır **proje sütunları** modülü. Bu sütunlar, tüm dize değerli sütunlar şunlardır: **ValueForWindCharacter**, **WetBulbFarenheit**, **WetBulbCelsius**, **PressureTendency**, **PressureChange**, **SeaLevelPressure**, ve **StationPressure**.

**Eksik verileri temizleme** modülü eksik veri içeren satırları kaldırmak için kalan sütunlara sonra uygulanır.

Hava durumu gözlem defa, en yakın tam saate yuvarlanır. Model uçuş zamanından önce yalnızca hava durumu kullandığından emin olun aksi yöne uçuşta süreleri ve hava durumu gözlem zamanları yuvarlanır. 

Hava durumu verileri yerel saatle bildirilir olduğundan, saat dilimi farklarını için zamanlanmış Kalkış Saati ve hava durumu gözlem saat saat dilimi sütunlarından çıkararak tüketici. Bu işlemler yapılır kullanarak **R betiği yürütme** modülü.

### <a name="joining-datasets"></a>Veri kümeleri birleştirme

Uçuş kayıtları, hava durumu veri kaynağı uçuş sırasında ile birleştirilir (**OriginAirportID**) kullanarak **veri birleştirme** modülü.

 ![Uçuş ve hava durumu, kaynak tarafından katılın](media/ui-sample-classification-predict-flight-delay/join-origin.png)


Uçuş kayıtları, uçuş hedefini kullanarak hava durumu verileri ile birleştirilir (**DestAirportID**).

 ![Uçuş ve hava durumu hedef tarafından katılın](media/ui-sample-classification-predict-flight-delay/join-destination.png)

### <a name="preparing-training-and-test-samples"></a>Eğitim ve Test Örnekleri hazırlama

**Verileri bölme** modülü Eylül kayıtları eğitim aracılığıyla Nisan verileri ayırır ve Ekim test için kaydeder.

 ![Bölme eğitim ve test verileri](media/ui-sample-classification-predict-flight-delay/split.png)

Yıl, ay ve saat dilimi sütunlar Sütun Seç modülünü kullanarak bir eğitim veri kümesinden kaldırılır.

## <a name="define-features"></a>Özellikleri tanımlama

Makine öğrenimi, ilgilendiğiniz bir şeyin tek tek ölçülebilir özellikleri özellikleridir. Güçlü bir özellikler kümesi bulmak, deneme ve etki alanı bilgisi gerektirir. Bazı özellikler, hedefi tahmin etmede diğerlerinden daha uygundur. Ayrıca, bazı özelliklerin diğer özelliklerle güçlü bir bağıntısı olabilir ve yeni bilgiler modele eklemeyeceğiz. Bu özellikler kaldırılabilir.

Bir model oluşturmak için sunulan tüm özellikleri kullanmak veya bir alt özellikler kümesini seçin.

## <a name="choose-and-apply-a-learning-algorithm"></a>Bir öğrenme algoritması seçme ve uygulama

Kullanarak bir model oluşturma **iki sınıflı Lojistik regresyon** modülü ve eğitim veri kümesi üzerinde eğitin. 

Sonucu **modeli eğitme** tahminde bulunmak amacıyla yeni örnekleri puanlamak için kullanılabilecek eğitilmiş bir sınıflandırma modeli modülüdür. Test puanlarını eğitilen modellerinden oluşturmak üzere kullanın. Ardından **Evaluate Model** analiz ve model kalitesi karşılaştırmak için modülü.

Denemeyi çalıştırdıktan sonra çıktısını görüntüleyebilirsiniz **Score Model** modülünün çıkış bağlantı noktasına tıklayıp seçerek **Görselleştir**. Çıktı, puanlanmış etiketler ve olasılıklar etiketlerinin içerir.

Son olarak sonuçların kalitesini test etmek için ekleyin **Evaluate Model** modülünü deneme tuvaline ve sol giriş bağlantı noktasına Score Model modülünün çıkışına bağlayın. Denemeyi çalıştırın ve çıktısını görüntülemek **Evaluate Model** modülü, çıkış bağlantı noktasına tıklayıp seçerek **Görselleştir**.

## <a name="evaluate"></a>Değerlendir
Lojistik regresyon modelini AUC ayarlama, test 0.631 sahiptir.

 ![değerlendir](media/ui-sample-classification-predict-flight-delay/evaluate.png)

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [Örnek 1 - regresyon: Otomobilin fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [2 - regresyon. örnek: Otomobil fiyat tahmini için algoritmalar karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
- [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [5 - sınıflandırma. örnek: Dalgalanmasını tahmin](ui-sample-classification-predict-churn.md)
