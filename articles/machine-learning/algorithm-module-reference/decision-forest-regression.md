---
title: 'Karar ormanı regresyon: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Karar ağaçları'bir topluluğu üzerinde temel bir regresyon modeli oluşturun, Azure Machine Learning hizmetinde karar ormanı regresyon modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: d372adf75d46fdedb7a6f2b17e47822475d1f155
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65442368"
---
# <a name="decision-forest-regression-module"></a>Karar ormanı regresyon Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Karar ağaçları'bir topluluğu üzerinde temel bir regresyon modeli oluşturmak için bu modülü kullanın.

Model yapılandırdıktan sonra etiketlenmiş bir veri kümesini kullanarak model eğitme ve [modeli eğitme](./train-model.md) modülü.  Eğitilen modelin ardından tahminlerde bulunmak üzere kullanılabilir. 

## <a name="how-it-works"></a>Nasıl çalışır?

Karar ağaçları bir ikili ağaç veri yapısı bir yaprak düğüm (karar) ulaşılana kadar geçiş her örneği için bir dizi basit testler gerçekleştirin parametrik olmayan modelleridir.

Karar ağaçları, şu avantajlara sahip olursunuz:

- Bunlar, eğitim ve tahmin sırasında hesaplama ve bellek kullanımı etkili olurlar.

- Bunlar, doğrusal olmayan karar sınırlarını temsil edebilir.

- Bunlar, tümleşik özellik seçimi ve sınıflandırma gerçekleştirmek ve saklanacaktır gürültülü özellikleri esnektir.

Bu regresyon modeli karar ağaçları'bir topluluğu oluşur. Her ormanda bir regresyon karar Gauss dağıtım tahmin çıkarır. Bir Gauss dağıtım modelinde tüm ağaçları birleşik dağıtımı için en yakın bulmak için ağaçları topluluğu üzerinde bir toplama gerçekleştirilmez.

Bu algoritma ve uygulanması için teorik çerçevesi hakkında daha fazla bilgi için bu makaleye bakın: [Karar ormanları: Sınıflandırma, regresyon, yoğunluklu tahmini için birleştirilmiş bir çerçeve Manifold öğrenme ve yarı denetimli öğrenme](https://www.microsoft.com/en-us/research/publication/decision-forests-a-unified-framework-for-classification-regression-density-estimation-manifold-learning-and-semi-supervised-learning/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D158806#)

## <a name="how-to-configure-decision-forest-regression-model"></a>Nasıl karar ormanı regresyon modelini yapılandırmak için

1. Ekleme **karar ormanı regresyon** modülünü deneme. Modül arabirimi altında bulabilirsiniz **Machine Learning**, **modeli Başlat**, ve **regresyon**.

2. Modül özelliklerini açın ve **yöntemi örnekleme**, tek tek ağacı oluşturmak için kullanılan yöntemi seçin.  Aralarından seçim yapabileceğiniz **Bagging** veya **çoğaltmak**.

    - **Bagging**: Bagging da çağrılır *önyükleme toplayarak*. Her ormanda bir regresyon karar tahmin yoluyla bir Gauss dağıtım çıkarır. Toplama, ilk iki dakika dakika bireysel ağaçları tarafından döndürülen tüm Gaussians birleştirerek verilen Gaussians karışımını, eşleşen bir Gauss bulmaktır.

         Daha fazla bilgi için bkz. Wikipedia girişini [önyükleme toplayarak](https://wikipedia.org/wiki/Bootstrap_aggregating).

    - **Çoğaltma**: Çoğaltmasında her ağaç tam olarak aynı giriş verileri eğitildi. Belirlenmesi hangi bölünmüş koşulu her ağaç düğümü için kullanılan rastgele olarak kalır ve ağaçları farklı olacaktır.

         Eğitim işlemle ilgili daha fazla bilgi için **çoğaltmak** seçeneği için bkz: [görüntü işleme ve tıbbi görüntü analizi için karar ormanları. Criminisi ve J. Shotton. Springer 2013.](https://research.microsoft.com/projects/decisionforests/).

3. Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.

    - **Tek bir parametre**

      Model yapılandırmak istediğiniz nasıl biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz. Bu değerler tarafından deneme öğrenebilir veya kılavuz almış.



4. İçin **karar ağaçları sayısı**, topluluğu içinde oluşturmaya karar ağaçları toplam sayısını gösterir. Daha fazla karar ağaçları oluşturarak potansiyel olarak daha iyi kapsamı elde edebilirsiniz, ancak eğitim süresini artırır.

    > [!TIP]
    > Bu değer, eğitilen model görselleştirirken kullanılacak görüntülenen ağaçları sayısını da denetler. bakın veya tek bir ağaç yazdırma istiyorsanız, değer 1 olarak ayarlayabilirsiniz; Ancak, bu yalnızca bir ağacı olacağı anlamına gelir (ilk parametre kümesiyle ağacı) üretilen ve herhangi bir yinelemenin gerçekleştirilir.

5. İçin **karar ağaçları maksimum derinliğini**, herhangi bir karar ağacını maksimum derinliğini sınırlamak için bir sayı yazın. Ağaç derinliği artırma duyarlığı, overfitting ve artan eğitim süre at the risk of.

6. İçin **düğüm başına rastgele bölmelerini sayısı**, her düğüm ağacı oluşturulurken kullanılacak bölmelerini sayısını yazın. A *bölme* anlamına gelir (node) ağacının her düzeyinde özellikleri rastgele ayrılmıştır.

7. İçin **yaprak düğüm başına örnek sayısı alt sınırı**, terminal düğümlerinden (yaprak) içinde bir ağaç oluşturmak için gereken durumlarda en az sayısını belirtin.

     Bu değer artırarak yeni kurallar oluşturma için eşik artırın. Örneğin, varsayılan değer olan 1 ile tek bir çalışmasını oluşturulacak yeni bir kural neden olabilir. 5 değerine artırmak istiyorsanız, eğitim verilerini aynı koşulları karşılayan en az beş servis taleplerini içerir etmesi gerekir.


9. Etiketlenmiş bir veri kümesine bağlanmak, ikiden fazla sonuçlarını içeren bir tek etiketli sütunu seçin ve bağlanma [modeli eğitme](./train-model.md).

    - Ayarlarsanız **Oluştur trainer modu** seçeneğini **tek parametre**, kullanarak modeli eğitme [modeli eğitme](./train-model.md) modülü.

   

10. Denemeyi çalıştırın.

### <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Her yineleme üzerinde oluşturuldu ağaç görmek için eğitim modülün çıkışına sağ tıklatın ve seçin **Görselleştir**.

+ Her düğüm için kuralları görmek için her ağaç'a tıklayın ve Gruplama detaya gitme.

+ Eğitilen modelin anlık görüntüsünü kaydetmek için eğitim modülün çıkışına sağ tıklatın ve seçin **eğitilen modeli olarak Kaydet**. Model bu kopyasını deneme art arda gelen çalışır güncelleştirilmez. 

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 