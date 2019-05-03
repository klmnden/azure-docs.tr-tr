---
title: 'Karar ormanı veya çoklu sınıflar: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir machine learning temel modeli oluşturmak için Azure Machine Learning hizmetinde veya çoklu sınıflar karar ormanı modülü kullanmayı öğrenirsiniz *karar ormanı* algoritması.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: d5ff1809886198f7f1e4f4b9c36f877ce61d512e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029438"
---
# <a name="multiclass-decision-forest-module"></a>Veya çoklu sınıflar karar ormanı Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir machine learning temel modeli oluşturmak için bu modülü kullanın *karar ormanı* algoritması. Karar ormanı hızlı bir şekilde bir dizi etiketli verilerden öğrenmeyi sırasında karar ağaçları oluşturan bir topluluğu modelidir.

## <a name="more-about-decision-forests"></a>Karar ormanları hakkında daha fazla bilgi

Karar ormanı yöntemi sınıflandırma için öğrenme bir topluluğu algoritmasıdır. Algoritma çalışan birden çok karar ağaçları oluşturarak ve ardından *oylama* en popüler çıkış sınıfı üzerinde. Oylama her hangi bir sınıflandırma karar ağacında etiketlerin sıklığını Normalleştirilmemiş histogram orman çıkaran bir toplama bir biçimidir. Toplama işlemi, bu Histogram toplar ve her etiket için "sayısal" almak için sonuca normalleştirir. Yüksek tahmin olmanızı ağaçları topluluğu son kararı büyük bir ağırlık sahip.

Karar ağaçları genel olarak değiştirilen dağıtımlar verilerle destekledikleri anlamına parametrik olmayan, modelleridir. Her ağacında bir yaprak düğüm (karar) ulaşılana kadar bir ağaç yapısı düzeyini artırmak her sınıf için bir dizi basit testler çalıştırılır.

Karar ağaçları, birçok avantaj vardır:

+ Bunlar, doğrusal olmayan karar sınırlarını temsil edebilir.
+ Bunlar, eğitim ve tahmin sırasında hesaplama ve bellek kullanımı etkili olurlar.
+ Tümleşik özellik seçimi ve sınıflandırma gerçekleştirirler.
+ Bunlar, saklanacaktır gürültülü özellikleri esnektir.

Karar ağaçları'bir topluluğu Azure Machine learning'de karar ormanı sınıflandırıcı oluşur. Genellikle, topluluğu modeller, daha iyi kapsamı ve tek karar ağaçları'den doğruluk sağlar. Daha fazla bilgi için [karar ağaçları](http://go.microsoft.com/fwlink/?LinkId=403677).

## <a name="how-to-configure-multiclass-decision-forest"></a>Yapılandırma veya çoklu sınıflar karar ormanı



1. Ekleme **veya çoklu sınıflar karar ormanı** denemenizde arabirimi modülü. Bu modül altında bulabilirsiniz **Machine Learning**, **modeli Başlat**, ve **sınıflandırma**.

2. Açmak için bir modüle çift tıklayın **özellikleri** bölmesi.

3. İçin **yöntemi örnekleme**, tek tek ağacı oluşturmak için kullanılan yöntemi seçin.  Bagging veya çoğaltma seçebilirsiniz.

    + **Bagging**: Bagging da çağrılır *önyükleme toplayarak*. Bu yöntemde her ağacı boyutu özgün bir veri kümeniz kadar rastgele değiştirme ile özgün veri kümesinden örnekleme tarafından oluşturulmuş olan yeni bir örneği şeylerde. Model çıktıları tarafından birleştirilir *oylama*, bir form toplama olduğu. Daha fazla bilgi için önyükleme toplamak için Wikipedia girişine bakın.

    + **Çoğaltma**: Çoğaltmasında her ağaç tam olarak aynı giriş verileri eğitildi. Belirlenmesi hangi bölünmüş koşulu her ağaç düğümü için kullanılan rastgele farklı ağaçları oluşturma kalır.

   

4. Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.

    + **Tek bir parametre**: Nasıl model yapılandırın ve bağımsız değişkenler olarak, bir dizi sağlamak istediğinizi biliyorsanız, bu seçeneği belirleyin.


5. **Karar ağaçları sayısı**: Topluluğu içinde oluşturulabilir karar ağaçları en fazla sayısını yazın. Daha fazla karar ağaçları oluşturarak potansiyel olarak daha iyi kapsamı elde edebilirsiniz, ancak eğitim süresini artırabilir.

    Bu değer, eğitilen model görselleştirirken kullanılacak sonuçlarında görüntülenen ağaçları sayısını da denetler. Bakın veya tek bir ağaç yazdırmak için değeri 1 olarak ayarlayabilirsiniz; Ancak, bu yalnızca bir ağacı olabilir anlamına gelir (ilk parametre kümesiyle ağacı) oluşturulur ve herhangi bir yinelemenin gerçekleştirilir.

6. **Karar ağaçları en büyük derinliği**: Herhangi bir karar ağacını maksimum derinliğini sınırlamak için bir sayı yazın. Ağaç derinliği artırma duyarlığı, overfitting ve artan eğitim süre at the risk of.

7. **Düğüm başına rastgele bölmelerini sayısı**: Her düğüm ağacı oluşturulurken kullanılacak bölmelerini sayısını yazın. A *bölme* anlamına gelir (node) ağacının her düzeyinde özellikleri rastgele ayrılmıştır.

8. **Örnek yaprak düğümü başına en düşük sayısı**: Terminal düğümlerinden (yaprak) içinde bir ağaç oluşturmak için gereken durumlarda en az sayısını belirtir. Bu değer artırarak yeni kurallar oluşturma için eşik artırın.

    Örneğin, varsayılan değer olan 1 ile tek bir çalışmasını oluşturulacak yeni bir kural neden olabilir. 5 değerine artırmak istiyorsanız, eğitim verilerini aynı koşulları karşılayan en az beş servis taleplerini içerir etmesi gerekir.



10. Etiketlenmiş bir veri kümesi ve eğitim modüllerinden birini bağlanın:

    + Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](./train-model.md) modülü.

11. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Çıktısı, her yineleme üzerinde oluşturuldu ağaç görmek için sağ [modeli eğitme](./train-model.md) modülü ve select **Görselleştir**.
+ Her düğüm için kuralları görmek için her ağaç gruplama detaya gitmek için tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 