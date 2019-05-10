---
title: Machine learning Azure veri Gezgini'nde özelliği
description: Azure veri Gezgini'nde kök neden analizi için kümeleme machine Learning'i kullanın.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: bc72cc21ab525ec82d9ce4b24e80ce82d92a5d21
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233502"
---
# <a name="machine-learning-capability-in-azure-data-explorer"></a>Machine learning Azure veri Gezgini'nde özelliği

Hizmet durumu, QoS veya hatalı çalışan cihazlar için yerleşik kullanarak anormal davranışları izlemek için kullanılan Azure Veri Gezgini, bir büyük veri analizi platformunda [anomali algılama ve tahmin](/azure/data-explorer/anomaly-detection) işlevleri. Anormal bir düzen algılandığında, kök neden analizi (RCA) azaltmak veya anomali çözümlemek için gerçekleştirilir.

Tanılama karmaşık ve etki alanı uzmanları tarafından yapılan ve uzun işlemidir. Getirme ve bir ek değişkeni, grafik, birden çok boyut değerlerinin dağıtımındaki değişiklikler aranıyor aynı zaman çerçevesi için farklı kaynaklardan ek verileri birleştirme işlemini içerir ve diğer teknikleri bağlı etki alanı bilgisini ve intuition. Bu tanılama senaryoları Azure veri Gezgini'nde ortak olduğundan, machine learning eklentileri tanılama aşamasında daha kolay hale getirmek ve RCA süresini kısaltmak kullanılabilir.

Azure veri Gezgini'nde, üç Machine Learning eklentileri vardır: [ `autocluster` ](/azure/kusto/query/autoclusterplugin), [ `basket` ](/azure/kusto/query/basketplugin), ve [ `diffpatterns` ](/azure/kusto/query/diffpatternsplugin). Tüm eklentiler kümeleme algoritmaları uygular. `autocluster` Ve `basket` eklentileri küme tek bir kayıt kümesi ve `diffpatterns` eklentisi kümeleri iki kayıt kümeleri arasındaki farklar.

## <a name="clustering-a-single-record-set"></a>Tek bir kayıt kümesi kümeleme

Sık karşılaşılan bir senaryodur, anormal davranış, yüksek sıcaklık cihaz okumalar, uzun süreli komutları ve kullanıcıların harcama üst sergiler zaman penceresi gibi belirli bir ölçütü olarak seçili bir veri kümesi içerir. Ortak desenler (segmentler) verileri bulmak için basit ve hızlı bir şekilde isteriz. Desenler, kayıtlarını aynı değerleri birden çok boyut (kategorik sütunlar) paylaşın. veri kümesinin bir alt kümesidir. Aşağıdaki sorgu oluşturur ve bir zaman serisi hizmeti özel durumların bir hafta içerisinde on dakikalık depo gösterir:

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m
| render timechart with(title="Service exceptions over a week, 10 minutes resolution")
```

![Hizmet özel durumları zaman grafiğini](media/machine-learning-clustering/service-exceptions-timechart.png)

Hizmet özel durum sayısı ile genel karşılık gelen hizmet trafiği. Pazartesi-Cuma günü iş için günlük deseni açıkça görebilirsiniz, hizmet bir artış ile özel durum Orta gün sayısı ve sayıları geceleri içinde bırakır. Düz düşük sayıları hafta sonu boyunca görünürdür. Özel durum ani kullanarak algılanamıyor [zaman serisi anomali algılama](/azure/data-explorer/anomaly-detection?#time-series-anomaly-detection) Azure veri Gezgini'nde.

Veri ikinci artış Salı kıyısında gerçekleşir. Aşağıdaki sorgu, bu depo daha iyi tanılamak için kullanılır. Bir bir sharp ani olup olmadığını doğrulamak için daha yüksek çözünürlükte (bir dakikalık depo sekiz saat) depo etrafında grafiği yeniden için bu sorguyu kullanın ve kenarlıkları görüntüleyebilirsiniz.

```kusto
let min_t=datetime(2016-08-23 11:00);
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to min_t+8h step 1m
| render timechart with(title="Zoom on the 2nd spike, 1 minute resolution")
```

![Depo zaman grafiğini üzerinde odaklanın](media/machine-learning-clustering/focus-spike-timechart.png)

Bir dar 15:02 iki dakikalık ani 15:00 görüyoruz. Aşağıdaki sorguda, özel durumlar bu iki dakikalık pencere sayısı:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| count
```

|Count |
|---------|
|972    |

Aşağıdaki sorguda, 20 özel durumlar dışında 972 örneği:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| take 20
```

| PreciseTimeStamp            | Bölge | ScaleUnit | Dağıtım kimliği                     | İzleme noktası | ServiceHost                          |
|-----------------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 2016-08-23 15:00:08.7302460 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:09.9496584 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 8d257da1-7a1c-44f5-9acd-f9e02ff507fd |
| 2016-08-23 15:00:10.5911748 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:12.2957912 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | f855fcef-ebfe-405d-aaf8-9c5e2e43d862 |
| 2016-08-23 15:00:18.5955357 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 9d390e07-417d-42eb-bebd-793965189a28 |
| 2016-08-23 15:00:20.7444854 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 6e54c1c8-42d3-4e4e-8b79-9bb076ca71f1 |
| 2016-08-23 15:00:23.8694999 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 36109      | 19422243-19b9-4d85-9ca6-bc961861d287 |
| 2016-08-23 15:00:26.4271786 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 36109      | 3271bae4-1c5b-4f73-98ef-cc117e9be914 |
| 2016-08-23 15:00:27.8958124 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 904498     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:32.9884969 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007007   | d5c7c825-9d46-4ab7-a0c1-8e2ac1d83ddb |
| 2016-08-23 15:00:34.5061623 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:37.4490273 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | f2ee8254-173c-477d-a1de-4902150ea50d |
| 2016-08-23 15:00:41.2431223 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 103200     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:47.2983975 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 423690590  | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:50.5932834 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 2a41b552-aa19-4987-8cdd-410a3af016ac |
| 2016-08-23 15:00:50.8259021 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 0d56b8e3-470d-4213-91da-97405f8d005e |
| 2016-08-23 15:00:53.2490731 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 36109      | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:57.0000946 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 64038      | cb55739e-4afe-46a3-970f-1b49d8ee7564 |
| 2016-08-23 15:00:58.2222707 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | 8215dcf6-2de0-42bd-9c90-181c70486c9c |
| 2016-08-23 15:00:59.9382620 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | 451e3c4c-0808-4566-a64d-84d85cf30978 |

### <a name="use-autocluster-for-single-record-set-clustering"></a>Autocluster() Kümelemesi ayarlamak için tek kaydı kullanın.

Olsa bile daha az binden bir özel durum, her sütunda birden çok değer gibi ortak bölümleri bulmak zor olabilir. Kullanabileceğiniz [ `autocluster()` ](/azure/kusto/query/autoclusterplugin) eklentisi anında küçük ortak kesimlerin listesini ayıklar ve ilgi çekici bulmak için aşağıdaki sorguda görüldüğü gibi depo iki dakika içinde kümeleri:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate autocluster()
```

| SegmentId | Count | Yüzde | Bölge | ScaleUnit | Dağıtım kimliği | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 94 | 9.67078189300411 | scus | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |
| 2 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 3 | 68 | 6.99588477366255 | scus | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |
| 4 | 55 | 5.65843621399177 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |

Yukarıdaki sonuçlarından en baskın kesim %65.74 toplam özel durum kayıtları içerir ve dört boyutları paylaşır görebilirsiniz. Sonraki segmenti yalnızca %9.67 kayıtları içeren çok daha az yaygındır ve üç boyut paylaşır. Diğer kesimlerle bile daha az yaygındır. 

Autocluster birden çok boyutta araştırma ve ilgi çekici segmentleri ayıklamak için özel bir algoritma kullanır. "İlginç", her bir kesim kayıt kümesi hem özellikler kümesi önemli kapsamını olduğu anlamına gelir. Bölümleri de ayrıldığını her biri diğerlerinden önemli ölçüde farklı olduğu anlamına gelen. Bir veya daha fazla bu segmentlerde RCA işlemi için uygun olmayabilir. Segment gözden geçirme ve değerlendirme en aza indirmek için yalnızca küçük listesini autocluster ayıklar.

### <a name="use-basket-for-single-record-set-clustering"></a>Basket() Kümelemesi ayarlamak için tek kaydı kullanın.

Ayrıca [ `basket()` ](/azure/kusto/query/basketplugin) aşağıdaki sorguda görüldüğü gibi Eklentisi:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate basket()
```

| SegmentId | Count | Yüzde | Bölge | ScaleUnit | Dağıtım kimliği | İzleme noktası | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 642 | 66.0493827160494 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |  |
| 2 | 324 | 33.3333333333333 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 0 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 3 | 315 | 32.4074074074074 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 16108 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 4 | 328 | 33.7448559670782 |  |  |  | 0 |  |
| 5 | 94 | 9.67078189300411 | scus | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |  |
| 6 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |  |
| 7 | 68 | 6.99588477366255 | scus | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |  |
| 8 | 167 | 17.1810699588477 | scus |  |  |  |  |
| 9 | 55 | 5.65843621399177 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |  |
| 10 | 92 | 9.46502057613169 |  |  |  | 10007007 |  |
| 11 | 90 | 9.25925925925926 |  |  |  | 10007006 |  |
| 12 | 57 | 5.8641975308642 |  |  |  |  | 00000000-0000-0000-0000-000000000000 |

Öğe araştırma ayarlayın ve eşiğin üstünde (varsayılan %5) kayıt kümesinin, kapsamı olan tüm parçaları ayıklar sepet Apriori algoritma uygular. Daha fazla segment (örneğin, parçaları 0,1 veya 2,3) için benzer olanları ile ayıklanan görebilirsiniz.

Her iki artırmasını güçlü ve kullanımı kolay, ancak bunlar (hiçbir etiketi ile) Denetimsiz bir şekilde kümesindeki tek bir kayıt kümesi Bunun nedeni, önemli kendi sınırlamasıdır. Bu nedenle ayıklanan desenleri, seçili kayıt kümesi (anormal kayıtlar) veya genel kayıt kümesi olup niteleyen anlaşılır değil.

## <a name="clustering-the-difference-between-two-records-sets"></a>İki kayıt kümeleri arasındaki farkı kümeleme

[ `diffpatterns()` ](/azure/kusto/query/diffpatternsplugin) Eklentisi yelken SORUMLULUĞUN `autocluster` ve `basket`. `Diffpatterns` iki kayıt kümelerini alır ve bunlar arasında farklı ana parçaları ayıklar. Bir grup genellikle anormal kayıt araştırılan kümesi içeren (bir analiz `autocluster` ve `basket`). Başka bir küme başvurusu kayıt kümesi (Temel) içerir. 

Aşağıdaki sorguda kullanıyoruz `diffpatterns` ilginç kümeleri taban çizgisi içinde küme farklıdır depo iki dakika içinde bulunacak. Taban çizgisi pencerenin 15:00 önce sekiz dakika olarak tanımlarız (depo başlatılır). Ayrıca belirli bir kaydı taban veya anormal bir kümeye ait olup olmadığını belirten bir ikili sütun (AB) tarafından genişletmek ihtiyacımız var. `Diffpatterns` anormal temel bayrağı (AB) karşı tarafından iki sınıf etiketleri burada oluşturulan bir denetimli öğrenme algoritması uygular.

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
let min_baseline_t=datetime(2016-08-23 14:50);
let max_baseline_t=datetime(2016-08-23 14:58); // Leave a gap between the baseline and the spike to avoid the transition zone.
let splitime=(max_baseline_t+min_peak_t)/2.0;
demo_clustering1
| where (PreciseTimeStamp between(min_baseline_t..max_baseline_t)) or
        (PreciseTimeStamp between(min_peak_t..max_peak_t))
| extend AB=iff(PreciseTimeStamp > splitime, 'Anomaly', 'Baseline')
| evaluate diffpatterns(AB, 'Anomaly', 'Baseline')
```

| SegmentId | CountA | CountB | PercentA | percentB | PercentDiffAB | Bölge | ScaleUnit | Dağıtım kimliği | İzleme noktası |
|-----------|--------|--------|----------|----------|---------------|--------|-----------|----------------------------------|------------|
| 0 | 639 | 21 | 65.74 | 1.7 | 64.04 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |
| 1 | 167 | 544 | 17.18 | 44.16 | 26.97 | scus |  |  |  |
| 2 | 92 | 356 | 9.47 | 28.9 | 19.43 |  |  |  | 10007007 |
| 3 | 90 | 336 | 9.26 | 27.27 | 18.01 |  |  |  | 10007006 |
| 4 | 82 | 318 | 8.44 | 25.81 | 17.38 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 5 | 55 | 252 | 5.66 | 20.45 | 14.8 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |
| 6 | 57 | 204 | 5.86 | 16.56 | 10.69 |  |  |  |  |

En baskın kesim tarafından ayıklanan aynı segmenttir `autocluster`, iki dakikalık anormal penceresinde kapsamını da %65.74 olur. Ancak sekiz dakika temel penceresinde kapsamını yalnızca %1.7. %64.04 farktır. Bu fark, anormal depoya ilişkilendirilmesini gibi görünüyor. Bu sorunlu segment sorguda görüldüğü gibi diğer kesimlerle karşı ait kayıtları içine ilk grafik ayırarak bu varsayımı doğrulayabilirsiniz:

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| extend seg = iff(Region == "eau" and ScaleUnit == "su7" and DeploymentId == "b5d1d4df547d4a04ac15885617edba57"
and ServiceHost == "e7f60c5d-4944-42b3-922a-92e98a8e7dec", "Problem", "Normal")
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m by seg
| render timechart
```

!['Diffpattern' segment zaman grafiğini doğrulanıyor](media/machine-learning-clustering/validating-diffpattern-timechart.png)

Salı öğleden sonra üzerinde depo bağlantısı kullanılarak bu belirli segment, özel durumlar nedeniyle olduğunu görmek bu grafik kurmamızı `diffpatterns` eklentisi.

## <a name="summary"></a>Özet

Azure Veri Gezgini Machine Learning artırmasını birçok senaryoda yardımcı olacaktır. `autocluster` Ve `basket` Denetimsiz öğrenme algoritmasını ve kullanımı kolay olan uygular. `Diffpatterns` denetimli öğrenme algoritmasını uygular ve ancak daha karmaşık, ayrıştırması parçaları için RCA ayıklanırken daha güçlüdür.

Bu eklenti, etkileşimli olarak geçici senaryolarında, neredeyse gerçek zamanlı izleme hizmetleri otomatik olarak kullanılır. Azure veri Gezgini'nde zaman serisi anomali algılama gerekli performansı standartları karşılamak üzere yüksek oranda iyileştirilmiş bir tanılama işlemi tarafından izlenir.
