---
title: Model sonuçlarını yorumlama
titleSuffix: Azure Machine Learning Studio
description: Bir algoritma kullanarak ve score model çıktıları görselleştirme için en iyi parametresinde nasıl seçeceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 11/29/2017
ms.openlocfilehash: c46f22fb5c906aaffa48f39a0c643ca2a48573f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60867338"
---
# <a name="interpret-model-results-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da model sonuçlarını yorumlama
Bu konuda, görselleştirin ve Azure Machine Learning Studio'da tahmin sonuçlarını yorumlama açıklanmaktadır. Bir modeli eğitilir ve Öngörüler, ("modeli puanlanmış") üzerinde yapılan sonra anlamak ve yorumlamak tahmin sonuç gerekir.



Makine öğrenimi modellerini Azure Machine Learning Studio'da dört ana türü vardır:

* Sınıflandırma
* Kümeleme
* Regresyon
* Öneren sistemleri

Bu modellerin üzerinde tahmin için kullanılan modüller şunlardır:

* [Modeli Puanlama] [ score-model] sınıflandırma ve regresyon için Modülü
* [Kümeye atamak] [ assign-to-clusters] kümeleme Modülü
* [Matchbox öneren puan] [ score-matchbox-recommender] öneri sistemleri

Bu belgede, bu modüllerin her biri için tahmin sonuçlarını yorumlama açıklanmaktadır. Bu modüller genel bakış için bkz. [algoritmalarınızı Azure Machine Learning Studio'da iyileştirmek için parametreleri seçme](algorithm-parameters-optimize.md).

Bu konuda, tahmin yorumu ancak değil model değerlendirme yöneliktir. Modelinizi değerlendirilecek hakkında daha fazla bilgi için bkz. [Azure Machine Learning Studio'da model performansını değerlendirme nasıl](evaluate-model-performance.md).

Azure Machine Learning Studio'da yeni kullanmaya başladıysanız ve başlamak için görmek için basit bir deneme oluşturma yardıma ihtiyacınız varsa [Azure Machine Learning Studio'da basit bir deneme oluşturma](create-experiment.md) Azure Machine Learning Studio'da.

## <a name="classification"></a>Sınıflandırma
İki alt kategorileri sınıflandırma sorunları vardır:

* Yalnızca iki sınıf (iki sınıflı veya ikili sınıflandırma) ile ilgili sorunlar
* İkiden fazla sınıfları (çok sınıflı sınıflandırma) ile ilgili sorunlar

Azure Machine Learning Studio için sınıflandırma türlerinin her biriyle dağıtılacak farklı modülleri olsa da, kendi tahmin sonuçları yorumlayarak destek sağlama yöntemleri benzerdir.

### <a name="two-class-classification"></a>İki sınıflı sınıflandırma
**Örnek deneme**

Iris çiçek sınıflandırma iki sınıflı sınıflandırma problemi örneğidir. Iris çiçek kendi özelliklere göre sınıflandırmak için bir görevdir. Azure Machine Learning Studio'da sağlanan kullanarak Iris veri kümesini popüler alt kümesidir [Iris veri kümesini](https://en.wikipedia.org/wiki/Iris_flower_data_set) iki türler (sınıflar 0 ve 1) çiçek yalnızca örneklerini içeren. Her çiçek (sepal uzunluğu, sepal genişliği, petal uzunluğu ve petal genişliği) için dört özellikler mevcuttur.

![Iris deneme ekran görüntüsü](./media/interpret-model-results/1.png)

Şekil 1. Iris iki sınıflı sınıflandırma sorunu deneme

Bir deney Şekil 1'de gösterildiği gibi bu sorunu çözmek için yapılmıştır. İki sınıflı artırmalı karar ağacı modeli eğitilir ve puanlanmış. Tahmin sonuçları görselleştirebilirsiniz artık [Score Model] [ score-model] modülünün çıkış bağlantı noktasına tıklayarak [Score Model] [ score-model] modülü ve ardından **Görselleştir**.

![Score model Modülü](./media/interpret-model-results/1_1.png)

Şekil 2'de gösterildiği gibi bu Puanlama sonuçlarını getirir.

![Iris iki sınıflı sınıflandırma deneme sonuçları](./media/interpret-model-results/2.png)

Şekil 2. Bir model Puanlama sonucunda iki sınıflı sınıflandırma görselleştirin

**Sonuç yorumu**

Sonuçlar tablosunda altı sütunları vardır. Sol dört sütun dört özellikleridir. Tahmin sonuçlarını Puanlanmış etiketler ve Puanlanmış olasılıklar, şu iki sütun var. Puanlanmış olasılıklar sütunu olasılığını gösterir çiçek (sınıf 1) pozitif sınıfına ait. Örneğin, ilk sütun (0.028571) gösterir ilk çiçek sınıfı 1'e ait 0.028571 olasılık sayısıdır. Puanlanmış etiketler sütunu her çiçek için tahmin edilen sınıf gösterir. Bu, Puanlanmış olasılıklar sütunu temel alır. Çiçek puanlanmış olasılığını 0,5 büyük ise, sınıf 1 olarak tahmin edildiğinde. Aksi takdirde, sınıf 0 olarak tahmin edildiğinde.

**Web hizmet yayımlama**

Tahmin sonuçlarını anladım ve ses materyali sonra deneme, çeşitli uygulamaları dağıtmak ve yeni bir süsen Çiçeği sınıfı tahminleri almak üzere çağrı bir web hizmeti olarak yayımlanabilir. Puanlama bir denemenin bir eğitim denemesini değiştirmek ve bir web hizmeti olarak yayımlama bilgi edinmek için [Tutorial 3: Kredi riski model dağıtma](tutorial-part3-credit-risk-deploy.md). Bu yordam ile bir Puanlama deney, Şekil 3'te gösterildiği gibi sağlar.

![Deneme Puanlama ekran görüntüsü](./media/interpret-model-results/3.png)

Şekil 3. Iris iki sınıflı sınıflandırma sorunu denemeyi Puanlama

Artık giriş ve çıkış web hizmeti için ayarlamanız gerekir. Sağ giriş bağlantı noktasına giriştir [Score Model][score-model], süsen Çiçeği olduğu giriş özellikleri. Çıkış seçimi olup, tahmin edilen sınıf (puanlanmış etiketi), puanlanmış olasılık ya da her ikisini bağlıdır. Bu örnekte, her ikisini de ilginizi varsayılır. İstenen çıkış sütunları seçmek için kullanın. bir [veri kümesindeki sütunları seçme] [ select-columns] modülü. Tıklayın [veri kümesindeki sütunları seçme][select-columns], tıklayın **Sütun seçiciyi Başlat**seçip **Puanlanmış etiketler** ve **Scored Olasılıklar**. Çıkış bağlantı noktasına ayarlandıktan sonra [veri kümesindeki sütunları seçme] [ select-columns] ve yeniden çalıştırma, tıklayarak denemeyi Puanlama web hizmeti olarak yayımlamaya hazır olmalısınız **WEB hizmeti yayımlama** . Son deneme Şekil 4 gibi görünüyor.

![Iris iki sınıflı sınıflandırma deneme](./media/interpret-model-results/4.png)

Şekil 4. Iris iki sınıflı sınıflandırma sorununun son Puanlama deneme

Web hizmetini çalıştırma ve test örneğinin bazı özellik değerlerini girin. sonra iki sayının sonucunu döndürür. Puanlanmış etiket ilk sayıdır ve ikinci puanlanmış bir olasılıktır. Bu çiçek sınıf 1 olarak ile 0.9655 olasılığını tahmin edildiğinde.

![Test yorumlama puan modeli](./media/interpret-model-results/4_1.png)

![Test sonuçlarını Puanlama](./media/interpret-model-results/5.png)

Şekil 5. Web hizmeti sonucu Iris iki sınıflı sınıflandırma

### <a name="multi-class-classification"></a>Çok sınıflı sınıflandırma
**Örnek deneme**

Bu deneyde bir harf tanıma görev sınıflı sınıflandırma örneği olarak gerçekleştirin. Sınıflandırıcı elle yazılmış görüntülerden ayıkladığınız bazı el ile yazılmış öznitelik değerlerine göre belirli bir harfi (sınıf) tahmin etmek çalışır.

![Harf tanıma örneği](./media/interpret-model-results/5_1.png)

Eğitim verileri, elle yazılmış harf görüntülerden ayıkladığınız 16 özellikler mevcuttur. 26 harf 26 göreceğiniz sınıfların oluşturur. Şekil 6 test veri kümesinde kudu ile aynı özelliklere harf tanıma için bir çok sınıflı sınıflandırma modeli eğitmek ve tahmin bir denemeyi gösterir.

![Harf tanıma sınıflı sınıflandırma deneme](./media/interpret-model-results/6.png)

Şekil 6. Harf tanıma sınıflı sınıflandırma sorunu deneme

Sonuçlardan görselleştirme [Score Model] [ score-model] modülünün çıkış bağlantı noktasına tıklayarak [Score Model] [ score-model] modülü ve ardından **Görselleştir**, içerik Şekil 7'de gösterildiği gibi görmeniz gerekir.

![Score model sonuçları](./media/interpret-model-results/7.png)

Şekil 7. Bir çok sınıflı sınıflandırma içinde Puanlama modeli sonuçlarını Görselleştirme

**Sonuç yorumu**

Sol 16 sütundan sınama kümesi özellik değerlerini temsil eder. Puanlanmış olasılıklar "XX" sınıf için yalnızca gibi adları olan sütunları Puanlanmış olasılıklar sütunu iki sınıflı durumda ister. Olasılık gösterdikleri karşılık gelen bir giriş denk gelen belirli bir sınıfa dönüştürür. Örneğin, ilk giriş için bir "A", "B" ve benzeri olduğunu 0.000451 olasılık olduğunu 0.003571 olasılık yoktur. Son sütun (Puanlanmış etiketler) Puanlanmış etiketler iki sınıflı durumda aynıdır. En büyük puanlanmış olasılık sınıfıyla ilgili girişi tahmin edilen sınıf seçilir. Örneğin, bir "F" (0.916995) olacak şekilde en büyük olasılıkla olduğundan ilk girişi için "F" puanlanmış etiketi olur.

**Web hizmet yayımlama**

Puanlanmış etiket Ayrıca, her giriş ve puanlanmış etiket olasılığını da alabilirsiniz. Temel mantığı tüm puanlanmış olasılıklar arasındaki en büyük olasılık bulmaktır. Bunu yapmak için kullanmanız gerekir [R betiği yürütme] [ execute-r-script] modülü. Şekil 8'de R kodunu görüntülenir ve deneme sonucunu Şekil 9'da gösterilir.

![R kod örneği](./media/interpret-model-results/8.png)

Şekil 8. Puanlanmış etiketler ve etiketlerin ilişkili olasılıklar ayıklanması için R kodu

![Deneme sonucu](./media/interpret-model-results/9.png)

Şekil 9. Harf tanıma sınıflı sınıflandırma problemi, son Puanlama deneme

Sonra yayımlama ve web hizmetini çalıştırmak ve döndürülen sonuç aşağıdaki gibi görünür Şekil 10 bazı giriş özellik değerleri girin. Bu elle yazılmış harf ayıklanan 16 özelliklerini 0.9715 olasılık ile "T" Tahmin edildiğinde.

![Testi yorumlama puanı Modülü](./media/interpret-model-results/9_1.png)

![Test sonucu](./media/interpret-model-results/10.png)

Şekil 10. Web hizmeti sonucu sınıflı sınıflandırma

## <a name="regression"></a>Regresyon
Regresyon sorunları sınıflandırma sorunlarında farklıdır. Ayrık sınıflar, sınıf bir süsen Çiçeği ait olduğu gibi tahmin etmek bir sınıflandırma sorun çalıştığınız. Ancak, regresyon problemi aşağıdaki örnekte görebileceğiniz gibi bir araba fiyatını gibi sürekli bir değişkene tahmin etmeye çalıştığınız.

**Örnek deneme**

Otomobil fiyat tahmini için regresyon, örnek olarak kullanın. Oluştur, yakıt türü, gövde türü ve sürücü tekerleği dahil olmak üzere kendi özelliklerini temel alarak bir araba fiyatını tahmin etmek çalışıyorsunuz. Denemeyi Şekil 11'de gösterilir.

![Otomobil fiyat regresyon denemesini](./media/interpret-model-results/11.png)

Şekil 11. Otomobil fiyat regresyon sorun deneme

Görselleştirme [Score Model] [ score-model] modülü, sonuç Şekil 12 gibi görünüyor.

![Otomobil fiyat tahmini sorun için Puanlama sonuçları](./media/interpret-model-results/12.png)

Şekil 12. Otomobil fiyat tahmini sorun için Puanlama sonuç

**Sonuç yorumu**

Puanlanmış etiketler, sonuç sütunuyla bu Puanlama sonuç olur. Tahmin edilen her bir araba fiyatını sayılardır.

**Web hizmet yayımlama**

Regresyon denemesini bir web hizmeti olarak yayımlayabilir ve otomobil fiyat tahmini için iki sınıflı sınıflandırma kullanım örneği olduğu gibi aynı şekilde çağırın.

![Otomobil fiyat regresyon problemi için deneme Puanlama](./media/interpret-model-results/13.png)

Şekil 13. Otomobil fiyat regresyon sorununun deneme Puanlama

Web servis çalışıyor, döndürülen sonuç Şekil 14 gibi görünüyor. Bu araç için tahmin edilen fiyat $15,085.52 olur.

![Testi yorumlama Puanlama Modülü](./media/interpret-model-results/13_1.png)

![Puanlama modülü sonuçları](./media/interpret-model-results/14.png)

Şekil 14. Web hizmeti bir otomobil fiyat regresyon problemi sonucunu

## <a name="clustering"></a>Kümeleme
**Örnek deneme**

Şimdi kullanarak Iris veri kümesini, küme bir deneme oluşturmak için yeniden kullanın. Burada yalnızca özelliklere sahiptir ve kümeleme için kullanılan çıkış veri kümesi sınıfı etiketleri filtreleyebilirsiniz. İçinde bu Iris kullanım örneği, iki iki sınıflara satmanın küme anlamına gelir eğitim işlemi sırasında olmasını küme sayısını belirtin. Şekil 15'te deneme gösterilmektedir.

![Iris kümeleme sorun denemeler yapın](./media/interpret-model-results/15.png)

Şekil 15. Iris kümeleme sorun denemeler yapın

Eğitim veri kümesi çığır truth etiketleri kendisi tarafından yok, kümeleme sınıflandırmasından farklıdır. Grupları eğitim veri kümesi örnekleri, ayrı kümeler halinde kümeleme. Eğitim işlemi sırasında modeli, özellikler arasındaki farkları öğrenerek girişleri etiketler. Bundan sonra eğitilen model daha ileride girişleri sınıflandırmak için kullanılabilir. İçinde bir kümeleme sorun ilgileniriz sonucun iki bölümü vardır. İlk bölümü, eğitim veri kümesi etiketleme ve ikinci, eğitilen modeli ile yeni bir veri kümesi sınıflandırma.

Sonuç ilk bölümünü sol çıkış bağlantı noktasına tıklayarak görselleştirilebilir [kümeleme modeli eğitme] [ train-clustering-model] tıklayıp **Görselleştir**. Görsel, Şekil 16'gösterilir.

![Sonuç kümesi](./media/interpret-model-results/16.png)

Şekil 16. Sonuç bir eğitim veri kümesi için küme Görselleştirme

Yeni girişlerle eğitilen kümeleme modeli, kümeleme ikinci bölümü sonucunu şekil 17'de gösterilir.

![Sonuç kümesi görselleştirin](./media/interpret-model-results/17.png)

Şekil 17. Sonuç yeni bir veri kümesinde Küme Görselleştirme

**Sonuç yorumu**

İki bölümü sonuçlarını farklı deneme aşamalarını kökü olsa da, aynı görünür ve aynı şekilde yorumlanır. İlk dört sütun özellikleridir. Son sütun, atama, tahmin sonucudur. Aynı numara atanan girişleri, bunlar benzerliğe şekilde (varsayılan Euclidean uzaklık ölçümü bu denemeyi kullanır) aynı kümede olması tahmin. 2 küme sayısı belirttiğinden atamaları girişleri 0 veya 1 olarak etiketlenmiştir.

**Web hizmet yayımlama**

Kümeleme denemenin bir web hizmeti yayımlama ve iki sınıflı sınıflandırma aynı şekilde kullanım örneği Öngörüler kümeleme için çağırın.

![Iris kümeleme sorun için deneme Puanlama](./media/interpret-model-results/18.png)

Şekil 18. Iris kümeleme sorununun deneme Puanlama

Sonra web hizmetini çalıştırma döndürülen sonuç şekil 19 gibi görünüyor. Bu çiçek 0 kümede tahmin edildiğinde.

![Test Puanlama modülü yorumlama](./media/interpret-model-results/18_1.png)

![Puanlama modülü sonuç](./media/interpret-model-results/19.png)

Şekil 19. Web hizmeti sonucu Iris iki sınıflı sınıflandırma

## <a name="recommender-system"></a>Öneren sistem
**Örnek deneme**

Öneren sistemleri için örnek olarak Restoran öneri sorun kullanabilirsiniz: müşterilerin kendi derecelendirme geçmişi temel alarak restoranlar önerilir. Giriş verileri üç bölümden oluşur:

* Müşterilerin restoran derecelendirmeleri
* Müşteri özellik verileri
* Restoran özelliği verileri

Biz ile yapabilir birkaç şey vardır [eğitme Matchbox öneren] [ train-matchbox-recommender] Azure Machine Learning Studio'da Modülü:

* Verilen kullanıcı ve öğe dereceleri tahmin edin
* Belirli bir kullanıcıya öğeleri önerin
* Belirli bir kullanıcıyla ilişkili kullanıcı Bul
* Belirli bir öğe için ilgili öğeleri bulma

Dört seçeneklerinde seçerek yapmasını istediğinizi seçebilirsiniz **öneren tahmin tür** menüsü. Burada, tüm dört senaryolar üzerinden yol.

![Matchbox öneren](./media/interpret-model-results/19_1.png)

Tipik bir Azure Machine Learning Studio denemesine öneren sistemi için Şekil 20 gibi görünüyor. Bu öneren sistem modüllerini kullanma hakkında daha fazla bilgi için bkz: [eğitme matchbox öneren] [ train-matchbox-recommender] ve [puanı matchbox öneren] [ score-matchbox-recommender].

![Öneren sistem deneme](./media/interpret-model-results/20.png)

Şekil 20. Öneren sistem deneme

**Sonuç yorumu**

**Verilen kullanıcı ve öğe dereceleri tahmin edin**

Seçerek **derecelendirme tahmin** altında **öneren tahmin tür**, belirli kullanıcı ve öğe derecesini tahmin etmek için öneren sistem seçmenizi istiyoruz. Bir görselleştirme [puanı Matchbox öneren] [ score-matchbox-recommender] çıktı şekil 21 gibi görünür.

![Sonucu tahmin derecelendirme öneren sisteminin--Puanlama](./media/interpret-model-results/21.png)

Şekil 21. Puan sonucu tahmin derecelendirme öneren sistem--görselleştirin

İlk iki sütun, girdi verilerini tarafından sağlanan kullanıcı öğesi çiftleridir. Üçüncü sütunda, bir kullanıcı belirli bir öğe için tahmin edilen derecesi ' dir. Örneğin, ilk satırın müşteri U1048 oranı restorana 135026 2 olarak tahmin edildiğinde.

**Belirli bir kullanıcıya öğeleri önerin**

Seçerek **öğesi öneri** altında **öneren tahmin tür**, belirli bir kullanıcıya önermek öneren sistem isteyen. Bu senaryoda seçmek için son parametre *öğe seçimi önerilen*. Seçeneğini **gelen derecelendirilmiş öğeleri (model değerlendirme)** öncelikle eğitim işlemi sırasında model değerlendirme içindir. Bu tahmin aşaması için Seçtiğimiz **gelen tüm öğeleri**. Bir görselleştirme [puanı Matchbox öneren] [ score-matchbox-recommender] çıktı Şekil 22 gibi görünür.

![Öneren sistem--öğesi öneri puanı sonucu](./media/interpret-model-results/22.png)

Şekil 22. Öneren sisteminin--öğesi öneri puanı sonucun görselleştirilmesi

İlk altı sütunların giriş verileri sağlanan belirli kullanıcı kimlikleri için önermek temsil eder. Diğer beş sütun, azalan ilgi düzeyi, kullanıcı için önerilen öğeler temsil eder. Örneğin, ilk satırın ardından 135018, 134975, 135021 ve 132862 134986, müşteri U1048 için önerilen en Restoran olur.

**Belirli bir kullanıcıyla ilişkili kullanıcı Bul**

Seçerek **ilgili kullanıcıları** altında **öneren tahmin tür**, öneren sistemin belirli bir kullanıcı için ilgili kullanıcıları bulmak isteyen. Benzer tercihleri olan kullanıcıları ilgili kullanıcılardır. Bu senaryoda seçmek için son parametre *ilgili kullanıcı seçimine*. Seçenek **öğesinden kullanıcılar olduğunu derecelendirilmiş öğeleri (model değerlendirme)** öncelikle eğitim işlemi sırasında model değerlendirme içindir. Seçin **tüm kullanıcıların** bu tahmin aşama için. Bir görselleştirme [puanı Matchbox öneren] [ score-matchbox-recommender] çıktı şekil 23'gibi görünür.

![Öneren sistem--ilgili kullanıcılar sonucunun Puanlama](./media/interpret-model-results/23.png)

Şekil 23. Öneren sisteminin--ilgili kullanıcıları Puanlama sonuçlarını Görselleştirme

İlk altı sütunların, belirtilen kullanıcı kimlikleri ilgili kullanıcıları bulmak için gereken girdi verilerini tarafından sağlanan gösterir. Diğer beş sütun ilgi azalan sırada kullanıcının tahmin edilen ilgili kullanıcıları depolayın. Örneğin, ilk satırın ardından U1066, U1044, U1017 ve U1072 U1051, müşteri U1048 en ilgili müşteri olur.

**Belirli bir öğe için ilgili öğeleri bulma**

Seçerek **ilgili öğeleri** altında **öneren tahmin tür**, belirli bir öğe için ilgili öğeleri bulmak için öneren sistem seçmenizi istiyoruz. İlgili öğeleri aynı kullanıcı tarafından beğenilen en olası öğeler görüntülenir. Bu senaryoda seçmek için son parametre *ilgili öğe seçimi*. Seçeneğini **gelen derecelendirilmiş öğeleri (model değerlendirme)** öncelikle eğitim işlemi sırasında model değerlendirme içindir. Burada **gelen tüm öğeleri** bu tahmin aşama için. Bir görselleştirme [puanı Matchbox öneren] [ score-matchbox-recommender] çıktı şekil 24 gibi görünür.

![Puan sonucunu öneren sistem--ilgili öğeler](./media/interpret-model-results/24.png)

Şekil 24. İlgili öğeler öneren sisteminin--Puanlama sonuçlarını Görselleştirme

İlk altı sütunların verilen öğe giriş verileri tarafından sağlanan gibi ilgili öğeleri bulmak için gerekli kimlikleri temsil eder. Diğer beş sütun öğesinin tahmin edilen ilgili öğeler bakımından ilgi azalan sırada depolayın. Örneğin, ilk satırın en uygun öğesinin 135026 135074, ardından 135035, 132875, 135055 ve 134992 öğesidir.

**Web hizmet yayımlama**

Bu denemeleri Öngörüler almak için web Hizmetleri olarak yayımlama işlemi her dört senaryo için benzerdir. Burada biz İkinci senaryoda (belirli bir kullanıcı için önerilen öğeler) örnek olarak alın. Diğer üç ile aynı yordamı izleyebilirsiniz.

Eğitilen öneren sistem eğitilen model olarak kaydetme ve girdi verilerini tek bir kullanıcı kimliği sütunu için istenen şekilde filtreleme, denemeyi şekil 25 olduğu gibi denetime ve bir web hizmeti olarak yayımlayın.

![Deneme Restoran öneri sorunun Puanlama](./media/interpret-model-results/25.png)

Şekil 25. Deneme Restoran öneri sorunun Puanlama

Web servis çalışıyor, döndürülen sonuç şekil 26 gibi görünüyor. Kullanıcının U1048 beş önerilen restoranlar 134986, 135018, 134975, 135021 ve 132862 ' dir.

![Öneren sistem hizmeti örneği](./media/interpret-model-results/25_1.png)

![Örnek deneme sonuçları](./media/interpret-model-results/26.png)

Şekil 26. Web hizmeti sonucu Restoran öneri sorun

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
