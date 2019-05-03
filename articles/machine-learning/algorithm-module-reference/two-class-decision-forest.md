---
title: 'İki sınıflı karar ormanı: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir machine learning modeli karar ormanları algoritmadan yola çıkılarak oluşturmak için Azure Machine Learning hizmetinde iki sınıflı karar ormanı modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f3e7cab6e33fa9373095441685f6ecb04f1d91a5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029378"
---
# <a name="two-class-decision-forest-module"></a>İki sınıflı karar ormanı Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir machine learning modeli karar ormanları algoritmadan yola çıkılarak oluşturmak için bu modülü kullanın.  

Karar ormanları hızlı, denetimli topluluğu modelleridir. Bir hedef en fazla iki sonuçlarını tahmin etmek istiyorsanız, bu modül iyi bir seçimdir. 

## <a name="understanding-decision-forests"></a>Karar ormanları anlama

Bu karar ormanı sınıflandırma görevleri için hedeflenen yöntemi öğrenme bir topluluğu algoritmasıdır. Topluluğu yöntemler, genel ilkeye dayalı, tek bir model üzerinde güvenmek yerine, daha iyi sonuçlar ve daha genel bir model birden çok ilgili modelleri oluşturarak ve bunları başka bir yolla birleştirme alabilirsiniz. Genellikle, topluluğu modeller, daha iyi kapsamı ve tek karar ağaçları'den doğruluk sağlar. 

Tek tek modeller oluşturun ve bunları bir topluluğu içinde birleştirmek için birçok yolu vardır. Bu belirli uygulama karar ormanı çalışan birden çok karar ağaçları oluşturarak ve ardından **oylama** en popüler çıkış sınıfı üzerinde. Oylama bir topluluğu modelinde sonuçlar oluşturma better-known yöntemleri biridir. 

+ Tüm veri kümesini kullanarak birçok ayrı sınıflandırma ağaçları oluşturulur ancak farklı (genellikle başlangıç noktaları rastgele). Bu, ayrı ayrı karar ağaçları rastgele veriler veya özellikler bölümü yalnızca kullanabilir rastgele orman yaklaşımından farklıdır.
+ Etiketlerin sıklığını Normalleştirilmemiş histogram karar ormanı ağacı her ağacında çıkarır. 
+ Toplama işlemi, bu Histogram toplar ve her etiket için "sayısal" almak için sonuca normalleştirir. 
+ Yüksek tahmin olmanızı ağaçları büyük ağırlık topluluğu son kararı sahip olur.

Karar ağaçları genel sınıflandırma görevleri için birçok avantaj vardır:
  
- Bunlar, doğrusal olmayan karar sınırları yakalayabilirsiniz.
- Eğitim ve verimli hesaplama ve bellek kullanımı gibi pek çok veri tahmin edin.
- Özellik Seçimi, eğitim ve sınıflandırma işlemlerinde tümleşiktir.  
- Ağaçları gürültülü veri ve birçok özellik barındırabilir.  
- Değiştirilen dağıtımlar verilerle başa çıkabilir anlamı parametrik olmayan modelleri değildirler. 

Ancak, basit karar ağaçları veri overfit ve ağaç Kümelemeler daha az genelleştirilebilir.

Daha fazla bilgi için [karar ormanları](http://go.microsoft.com/fwlink/?LinkId=403677).  

## <a name="how-to-configure"></a>Yapılandırma
  
1.  Ekleme **iki sınıflı karar ormanı** modülü, Azure Machine learning'de ve açık bir denemenize **özellikleri** modülünün bölmesi. 

    Modül altında bulabilirsiniz **Machine Learning**. Genişletin **başlatmak**, ardından **sınıflandırma**.  
  
2.  İçin **yöntemi örnekleme**, tek tek ağacı oluşturmak için kullanılan yöntemi seçin.  Aralarından seçim yapabileceğiniz **Bagging** veya **çoğaltmak**.  
  
    -   **Bagging**: Bagging da çağrılır *önyükleme toplayarak*. Bu yöntemde her ağacı boyutu özgün bir veri kümeniz kadar rastgele değiştirme ile özgün veri kümesinden örnekleme tarafından oluşturulmuş olan yeni bir örneği şeylerde.  
  
         Model çıktıları tarafından birleştirilir *oylama*, bir form toplama olduğu. Her ormanda bir sınıflandırma karar etiketlerin bir Normalleştirilmemiş sıklığı histogram çıkarır. Bu çubuk grafikler toplamak ve her etiket için "sayısal" almak için'leri normalleştirmek için toplama var. Bu şekilde, yüksek tahmin olmanızı ağaçları topluluğu son kararı büyük bir ağırlık sahip olur.  
  
         Daha fazla bilgi için önyükleme toplamak için Wikipedia girişine bakın.  
  
    -   **Çoğaltma**: Çoğaltmasında her ağaç tam olarak aynı giriş verileri eğitildi. Belirlenmesi hangi bölünmüş koşulu her ağaç düğümü için kullanılan rastgele olarak kalır ve ağaçları farklı olacaktır.   
  
3.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.
  
4.  İçin **karar ağaçları sayısı**, topluluğu içinde oluşturulabilir karar ağaçları en fazla sayısını yazın. Daha fazla karar ağaçları oluşturarak potansiyel olarak daha iyi kapsamı elde edebilirsiniz, ancak eğitim süresini artırır.  
  
    > [!NOTE]
    >  Bu değer, eğitilen model görselleştirirken kullanılacak görüntülenen ağaçları sayısını da denetler. Bakın veya tek bir ağaç yazdırma istiyorsanız, değer 1 olarak ayarlayabilirsiniz. Ancak, yalnızca bir ağaç olabilir (ilk parametre kümesiyle ağacı) üretilen ve herhangi bir yinelemenin gerçekleştirilir.
  
5.  İçin **karar ağaçları maksimum derinliğini**, herhangi bir karar ağacını maksimum derinliğini sınırlamak için bir sayı yazın. Ağaç derinliği artırma duyarlığı, overfitting ve artan eğitim süre at the risk of.
  
6.  İçin **düğüm başına rastgele bölmelerini sayısı**, her düğüm ağacı oluşturulurken kullanılacak bölmelerini sayısını yazın. A *bölme* anlamına gelir (node) ağacının her düzeyinde özellikleri rastgele ayrılmıştır.
  
7.  İçin **yaprak düğüm başına örnek sayısı alt sınırı**, terminal düğümlerinden (yaprak) içinde bir ağaç oluşturmak için gereken durumlarda en az sayısını belirtin.
  
     Bu değer artırarak yeni kurallar oluşturma için eşik artırın. Örneğin, varsayılan değer olan 1 ile tek bir çalışmasını oluşturulacak yeni bir kural neden olabilir. 5 değerine artırmak istiyorsanız, eğitim verilerini aynı koşulları karşılayan en az beş servis taleplerini içerir etmesi gerekir.  
  
8.  Seçin **kategorik özellikleri için bilinmeyen değerlere izin** eğitimi veya doğrulama kümelerinde bilinmeyen değerler için bir grup oluşturmak için seçeneği. Model için bilinen değerleri daha az kesin olabilir, ancak daha iyi tahminler elde etmek için yeni (bilinmiyor) değerler sağlayabilirsiniz. 

     Bu seçeneğin işaretini kaldırırsanız, model eğitim verilerde bulunan değerleri kabul edebilir.
  
9. Etiketlenmiş bir veri kümesi ve bir ekleme [eğitim modülleri](module-reference.md):  
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](./train-model.md) modülü.  
  
    
## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Çıktısı, her yineleme üzerinde oluşturuldu ağaç görmek için sağ [modeli eğitme](./train-model.md) modülü ve select **Görselleştir**.
  
    Her ağaç gruplama detaya gitme ve her düğüm için kuralları görmek için tıklayın.

+ Model anlık görüntüsünü kaydetmek için sağ **eğitilen Model** çıktı ve seçin **Modeli Kaydet**. Denemeyi art arda gelen çalışır kaydedilmiş model güncelleştirilmez.

+ Modeli Puanlama için kullanmak için ekleyin **Score Model** modülünü deneme.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 