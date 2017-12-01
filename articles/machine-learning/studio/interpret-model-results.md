---
title: "Machine Learning modeli sonuçlarında yorumlama | Microsoft Docs"
description: "Bir algoritma kullanmak için en iyi parametre seçme ayarlayın ve score model görselleştirme çıkarır."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 6230e5ab-a5c0-4c21-a061-47675ba3342c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: d6563d411e9f159399f9863a5b572365dc2b05cc
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="interpret-model-results-in-azure-machine-learning"></a>Azure Machine Learning modeli sonuçlarında yorumlama
Bu konu, görselleştirin ve Azure Machine Learning Studio'da tahmin sonuçlarını yorumlanacağı açıklanmaktadır. Bir model eğitilmiş ve Öngörüler ("model skoru") en üstünde bitti sonra anlamak ve tahmin sonuç yorumlamak gerekir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Machine learning Azure Machine Learning modellerini dört ana tür vardır:

* Sınıflandırma
* Kümeleme
* regresyon
* Öneren sistemleri

Bu modeller üstünde tahmin için kullanılan modülleri şunlardır:

* [Modeli Puanlama] [ score-model] sınıflandırma ve regresyon için Modülü
* [Kümeye atamak] [ assign-to-clusters] kümeleme Modülü
* [Matchbox öneren puan] [ score-matchbox-recommender] önerisi sistemleri

Bu belgede tahmin sonuçlarını bu modüllerin her biri için yorumlama açıklanmaktadır. Bu modüller genel bakış için bkz: [, Azure Machine Learning algoritmaları en iyi duruma getirmek için parametreleri seçme](algorithm-parameters-optimize.md).

Bu konu, tahmin yorumlama ancak değil modeli değerlendirme giderir. Modelinizi değerlendirme hakkında daha fazla bilgi için bkz: [Azure Machine Learning modeli performansını değerlendirmek nasıl](evaluate-model-performance.md).

Azure Machine Learning için yenidir ve başlamak için bkz: basit bir deneme oluşturma konusunda Yardım ihtiyacınız varsa [Azure Machine Learning Studio'da basit bir deneme oluşturmak](create-experiment.md) Azure Machine Learning Studio'da.

## <a name="classification"></a>Sınıflandırma
Sınıflandırma sorunları iki alt kategorileri şunlardır:

* Yalnızca iki sınıf (iki sınıflı veya ikili sınıflandırma) sorunları
* İkiden fazla sınıfları (çok sınıfı sınıflandırma) sorunları

Azure Machine Learning için türlerinin her biri bu sınıflandırma ile mücadele etmek için farklı modülleri olsa da, tahmin sonuçlarını yorumlanması için yöntemler benzerdir.

### <a name="two-class-classification"></a>İki sınıflı sınıflandırma
**Örnek deneme**

İki sınıflı sınıflandırma sorunu iris çiçekler sınıflandırmasını örnektir. Kendi özelliklerini temel alarak iris çiçekler sınıflandırmak için bir görevdir. Azure Machine Learning ile sağlanan Iris veri kümesi popüler bir alt kümesidir [Iris veri kümesi](http://en.wikipedia.org/wiki/Iris_flower_data_set) iki sağındaki (0 ve 1 sınıflar) çiçek yalnızca örneklerini içeren. Her çiçek (sepal uzunluğu, sepal genişlik, petal uzunluğu ve petal genişliği) için dört özellikler vardır.

![İris deneme ekran görüntüsü](./media/interpret-model-results/1.png)

Şekil 1 '. Iris iki sınıflı sınıflandırma sorunu deneme

Bir deneme Şekil 1'de gösterildiği gibi bu sorunu çözmek için yapılmıştır. İki sınıflı artırılmış karar ağacı modeli eğitilmiş ve skoru. Tahmin sonuçlarını görselleştirme artık [Score Model] [ score-model] çıkış bağlantı noktasına tıklayarak Modülü [Score Model] [ score-model] modülü ve ardından **Görselleştir**.

![Score model Modülü](./media/interpret-model-results/1_1.png)

Şekil 2'de gösterildiği gibi bu Puanlama sonuçları getirir.

![İris iki sınıflı sınıflandırma deneme sonuçları](./media/interpret-model-results/2.png)

Şekil 2. İki sınıflı sınıflandırma score model sonucunda Görselleştirme

**Sonuç yorumlama**

Sonuçlar tablosunda altı sütun vardır. Sol dört sütun dört özellikleridir. Sağda iki sütun, skoru etiketleri ve skoru olasılıklar tahmin sonuçlarını vardır. Olasılık skoru olasılıklar sütunda görüntülenir çiçek pozitif sınıfı (sınıf 1) ait. Örneğin, ilk sütun (0.028571) gösterir ilk çiçek sınıfı 1'e ait 0.028571 olasılık sayısıdır. Skoru etiketleri sütunu her çiçek için tahmin edilen sınıfı gösterir. Bu skoru olasılıklar sütunu temel alır. Bir çiçek puanlanmış olasılığını 0,5 büyükse, sınıf 1 olarak tahmin. Aksi takdirde sınıfı 0 tahmin.

**Web hizmet yayımı**

Tahmin sonuçlarını anlamış ve ses nitelendirilmiştir sonra böylece çeşitli uygulamalar dağıtmak ve yeni bir iris çiçek üzerinde sınıf Öngörüler almak üzere çağrı denemeyi bir web hizmeti olarak yayımlanabilir. Eğitim denemenizi Puanlama denemeyi değiştirmek ve bir web hizmeti olarak yayımlamak öğrenmek için bkz: [Azure Machine Learning web hizmeti yayımlama](walkthrough-5-publish-web-service.md). Bu yordam ile Puanlama bir deneme, Şekil 3'te gösterildiği gibi sağlar.

![Deneme Puanlama işleminin ekran görüntüsü](./media/interpret-model-results/3.png)

Şekil 3 '. İris iki sınıflı sınıflandırma sorunu deneme Puanlama

Şimdi giriş ve çıkış web hizmeti için ayarlamanız gerekir. Sağ giriş bağlantı noktasını girdidir [Score Model][score-model], Iris çiçek olduğu giriş özellikleri. Çıktı Seçimi olup, tahmin edilen sınıfı (puanlanmış etiketi), puanlanmış olasılık veya her ikisi de ilginizi bağlıdır. Bu örnekte, her ikisinde de ilginizi varsayılır. İstenen çıkış sütunları seçmek için kullanın bir [veri kümesinde Sütun Seç] [ select-columns] modülü. ' I tıklatın [veri kümesinde Sütun Seç][select-columns], tıklatın **başlatma Sütun seçiciyi**seçip **skoru etiketleri** ve **skoru olasılıklar**. Çıkış bağlantı noktasına ayarlandıktan sonra [veri kümesinde Sütun Seç] [ select-columns] ve yeniden çalıştırmayı, tıklayarak bir web hizmeti olarak Puanlama deneme yayımlanacağını hazır olmalısınız **yayımlama WEB hizmeti**. Son deneme Şekil 4 gibi görünüyor.

![İris iki sınıflı sınıflandırma deneme](./media/interpret-model-results/4.png)

Şekil 4 '. Bir iris iki sınıflı sınıflandırma sorununun son Puanlama deneme

Web hizmetini çalıştırmak ve bir test örneğinin bazı özellik değerleri girin sonra iki sayının sonucunu döndürür. Puanlanmış etiket ilk sayıdır ve ikinci puanlanmış olasılıktır. Bu çiçek sınıfı 1 olarak 0.9655 olasılık ile tahmin.

![Test yorumlama score model](./media/interpret-model-results/4_1.png)

![Puanlama test sonuçları](./media/interpret-model-results/5.png)

Şekil 5. Web hizmeti sonucu iris iki sınıflı sınıflandırma

### <a name="multi-class-classification"></a>Birden çok sınıf sınıflandırma
**Örnek deneme**

Bu deneme içinde bir harf tanıma görev çok sınıflı sınıflandırma örneği olarak gerçekleştirin. Sınıflandırıcı, elle yazılmış görüntülerden ayıklanan bazı elle yazılmış öznitelik değerlerine göre belirli bir harfi (sınıfı) tahmin etmek çalışır.

![Harf tanıma örneği](./media/interpret-model-results/5_1.png)

Eğitim verileri içinde elle yazılmış harf görüntülerden ayıklanan 16 özellikler vardır. 26 harf bizim 26 sınıfları oluşturur. Şekil 6 harf tanıma için çok sınıflı sınıflandırma modeli eğitmek ve tahmin bir denemeyi sınama veri kümesi üzerinde aynı özelliğini gösterir.

![Harf tanıma çok sınıflı sınıflandırma deneme](./media/interpret-model-results/6.png)

Şekil 6. Harf tanıma çok sınıflı sınıflandırma sorunu deneme

Sonuçlarını görselleştirme [Score Model] [ score-model] çıkış bağlantı noktasına tıklayarak Modülü [Score Model] [ score-model] modülü ve ardından **Görselleştir**, içerik Şekil 7'de gösterildiği gibi görmeniz gerekir.

![Score model sonuçları](./media/interpret-model-results/7.png)

Şekil 7. Birden çok sınıf sınıflandırma içinde Puanlama modeli sonuçlarını Görselleştirme

**Sonuç yorumlama**

Sol 16 sütunlar sınama kümesi özellik değerlerini temsil eder. Sütunları skoru olasılıklar sınıf "XX" için yalnızca gibi adlara sahip iki sınıflı durumda skoru olasılıklar sütun ister. Olasılık Göster, belirli bir sınıfına karşılık gelen bir giriş döner. Örneğin, ilk giriş için bir "A", "B" ve benzeri olduğunu 0.000451 olasılık olduğunu 0.003571 olasılık yoktur. Son sütun (skoru etiketleri) skoru etiketleri aynı iki sınıflı durumda değil. En büyük puanlanmış olasılık sınıfıyla ilgili girişi tahmin edilen sınıf olarak seçilir. Örneğin, bir "F" (0.916995) olması için en büyük olasılık olduğundan ilk girişi için puanlanmış etiketi "F" olur.

**Web hizmet yayımı**

Ayrıca, her giriş ve puanlanmış etiket olasılığını için puanlanmış etiket elde edebilirsiniz. Temel mantığı puanlanmış olasılıklar arasında büyük olasılık bulmaktır. Bunu yapmak için kullanmanız gerekir [R betiği yürütün] [ execute-r-script] modülü. R kodu Şekil 8'de gösterilen ve deneme sonucunu Şekil 9'da gösterilir.

![R kod örneği](./media/interpret-model-results/8.png)

Şekil 8'de. R kodunu skoru etiketleri ve etiketlerin ilişkili olasılıklar ayıklanıyor

![Deneme sonucu](./media/interpret-model-results/9.png)

Şekil 9. Harf tanıma çok sınıflı sınıflandırma sorununun son Puanlama deneme

Sonra yayımlama ve web hizmetini çalıştırmak ve bazı giriş özellik değerleri, Şekil 10 döndürülen sonuç görülüyor girin. Bu elle yazılmış harf ayıklanan 16 özelliklerini 0.9715 olasılık ile "T" olarak tahmin.

![Sınama yorumlama puan Modülü](./media/interpret-model-results/9_1.png)

![Test sonucu](./media/interpret-model-results/10.png)

Şekil 10. Web hizmeti sonucu çok sınıflı sınıflandırma

## <a name="regression"></a>regresyon
Regresyon sorunları sınıflandırma sorunlarında farklıdır. Bir sınıflandırma sorunu sınıfı iris çiçek ait olduğu gibi ayrık sınıfları tahmin deniyorsunuz. Ancak regresyon sorun aşağıdaki örnekte görüldüğü gibi bir araba fiyatını gibi sürekli bir değişken tahmin etmeye çalıştığınız.

**Örnek deneme**

Otomobil fiyat tahmini regresyon için örnek olarak kullanın. Oluştur, yakıt türü, gövde türü ve sürücü tekerleği dahil olmak üzere kendi özelliklerini temel alarak otomobil fiyatını tahmin etmek çalışıyorsunuz. Denemeyi Şekil 11'de gösterilir.

![Otomobil fiyat regresyon denemesini](./media/interpret-model-results/11.png)

Şekil 11. Otomobil fiyat regresyon sorun deneme

Görselleştirme [Score Model] [ score-model] modülü, sonuç Şekil 12 gibi görünüyor.

![Otomobil fiyat tahmini soruna yönelik Puanlama sonuçları](./media/interpret-model-results/12.png)

Şekil 12. Otomobil fiyat tahmini soruna yönelik Puanlama sonucu

**Sonuç yorumlama**

Puanlanmış etiketler Puanlama bu sonucu sonuç sütununda olur. Tahmin edilen her araba fiyatını numaralarıdır.

**Web hizmet yayımı**

Bir web hizmeti içine regresyon denemesini yayımlayın ve bu otomobil fiyat tahmini için iki sınıflı sınıflandırma kullanım durumunda olduğu gibi aynı şekilde çağırın.

![Otomobil fiyat regresyon sorun için deneme Puanlama](./media/interpret-model-results/13.png)

Şekil 13. Bir otomobil fiyat regresyon sorununun deneme Puanlama

Web hizmeti çalıştıran, döndürülen sonuç Şekil 14 gibi görünüyor. Bu otomobil fiyatını tahmin edilen $15,085.52 ' dir.

![Sınama yorumlama Puanlama Modülü](./media/interpret-model-results/13_1.png)

![Puanlama modülü sonuçları](./media/interpret-model-results/14.png)

Şekil 14. Web hizmeti bir otomobil fiyat regresyon sorun sonucu

## <a name="clustering"></a>Kümeleme
**Örnek deneme**

Şimdi Iris veri kümesini yeniden kümeleme bir deneme oluşturmak için kullanın. Burada yalnızca özelliklere sahiptir ve kümeleme için kullanılan veri kümesi sınıfı etiketler çıkışı filtreleyebilirsiniz. Bu iris kullanım, iki iki sınıf halinde çiçekler küme anlamına gelir eğitim işlemi sırasında olması için küme sayısını belirtin. Denemeyi Şekil 15'te gösterilir.

![Iris kümeleme sorun denemeler](./media/interpret-model-results/15.png)

Şekil 15. Iris kümeleme sorun denemeler

Eğitim veri kümesinin başından başlayarak gerçekte etiketleri tek başına yok kümeleme sınıflandırma farklıdır. Grupları eğitim veri kümesi örnekleri, ayrı kümeler halinde kümeleme. Eğitim işlemi sırasında model özelliklerine arasındaki farklar öğrenerek girişleri etiketler. Bundan sonra eğitilen model daha ileride girişleri sınıflandırmak için kullanılabilir. Biz kümeleme sorun içinde ilgilendiğiniz sonucunun iki bölümü vardır. İlk bölümü eğitim veri kümesi etiketleme ve ikinci bir yeni veri kümesi ve eğitilen modele ile sınıflandırmak.

Sol çıkış bağlantı noktasına tıklayarak sonucu ilk bölümü canlandırılabilir [tren kümeleme modelinde] [ train-clustering-model] ve ardından **Görselleştir**. Görselleştirme Şekil 16'gösterilir.

![Kümeleme sonucu](./media/interpret-model-results/16.png)

Şekil 16. Sonuç eğitim veri kümesi için kümeleme Görselleştirme

Yeni girişlerle eğitilen kümeleme modeli, kümeleme ikinci bölümü, sonuç şekil 17'de gösterilir.

![Sonuç kümeleme Görselleştirme](./media/interpret-model-results/17.png)

Şekil 17. Yeni bir veri kümesi üzerinde sonuç kümeleme Görselleştirme

**Sonuç yorumlama**

Farklı deneme aşamaları iki bölümden sonuçlarını gövdesi karşın, aynı bakın ve aynı şekilde yorumlanır. İlk dört sütun özellikleridir. Son sütun atamaları tahmin sonucudur. Atanan aynı sayı girişleri, (Bu deneme varsayılan Euclidean uzaklığı ölçüm kullanır) şekilde benzerlikler paylaştıkları aynı küme olacak şekilde tahmin. 2 küme sayısı belirtilmediğinden atamaları girdileri 0 veya 1 etiketlenir.

**Web hizmet yayımı**

Bir web hizmeti içine kümeleme deneme yayımlama ve iki sınıflı sınıflandırma olduğu gibi kullanım örneği tahminleri kümeleme için çağırın.

![Deneme iris kümeleme sorunu Puanlama](./media/interpret-model-results/18.png)

Şekil 18. Deneme iris kümeleme sorununun Puanlama

Sonra web hizmetini çalıştırmak, döndürülen sonuç şekil 19 gibi görünüyor. Küme 0 olması için bu çiçek tahmin.

![Test Puanlama modülü yorumlama](./media/interpret-model-results/18_1.png)

![Puanlama modülü sonuç](./media/interpret-model-results/19.png)

Şekil 19. Web hizmeti sonucu iris iki sınıflı sınıflandırma

## <a name="recommender-system"></a>Öneren sistem
**Örnek deneme**

Öneren sistemleri için örnek olarak Restoran öneri sorun kullanabilirsiniz: müşteriler kendi derecelendirme geçmiş temelinde Restoran öneririz. Giriş verisi üç bölümden oluşur:

* Müşterilerden Restoran derecelendirme
* Müşteri özellik verileri
* Restoran özelliği verileri

Biz ile yapabilir birkaç şey vardır [tren Matchbox öneren] [ train-matchbox-recommender] Azure Machine Learning modülünde:

* Verilen kullanıcı ve madde derecelendirmesi tahmin etme
* Belirli bir kullanıcıya öğeleri önerilir
* Belirli bir kullanıcıyla ilişkili kullanıcı Bul
* Belirli bir öğesiyle ilgili öğeleri bulma

Dört seçeneklerinde seçerek yapmak istediğiniz seçebilirsiniz **öneren tahmin türü** menüsü. Burada tüm dört senaryolar üzerinden yol.

![Matchbox öneren](./media/interpret-model-results/19_1.png)

Tipik bir Azure Machine Learning deneme öneren sistemi Şekil 20 gibi görünüyor. Bu öneren sistem modüllerini kullanma hakkında daha fazla bilgi için bkz: [tren matchbox öneren] [ train-matchbox-recommender] ve [puan matchbox öneren][score-matchbox-recommender].

![Öneren sistem deneme](./media/interpret-model-results/20.png)

Şekil 20. Öneren sistem deneme

**Sonuç yorumlama**

**Verilen kullanıcı ve madde derecelendirmesi tahmin etme**

Seçerek **derecelendirme tahmin** altında **öneren tahmin türü**, öneren sistem verilen kullanıcı ve madde için derecelendirme tahmin etmek istiyoruz. Görsel olarak [puan Matchbox öneren] [ score-matchbox-recommender] çıkış şekil 21 gibi görünüyor.

![Tahmin derecelendirme öneren sistem--sonucunu puan](./media/interpret-model-results/21.png)

Şekil 21. Tahmin derecelendirme öneren sistem--puan sonucunu Görselleştirme

İlk iki sütunu giriş verisi tarafından sağlanan kullanıcı öğesi çiftleridir. Bir kullanıcı belirli bir öğe için tahmin edilen derecesi üçüncü sütundur. Örneğin, ilk satırda müşteri U1048 oranı Restoran 135026 2 olarak tahmin.

**Belirli bir kullanıcıya öğeleri önerilir**

Seçerek **öğesi öneri** altında **öneren tahmin türü**, öğeleri belirli bir kullanıcıya önermek için öneren sistem isteyen. Bu senaryoda seçmek için son parametre *öğe seçimi önerilen*. Seçenek **gelen derecelendirilmiş öğeleri (model değerlendirme)** eğitim işlemi sırasında model değerlendirme öncelikle içindir. Bu tahmin aşaması için seçeneğini belirledik **gelen tüm öğeleri**. Görsel olarak [puan Matchbox öneren] [ score-matchbox-recommender] çıkış Şekil 22 gibi görünüyor.

![Öneren sistem--öğesi öneri puan sonucu](./media/interpret-model-results/22.png)

Şekil 22. Öneren sistem--öğesi öneri puan sonucunu Görselleştirme

İlk altı sütunların giriş verisi tarafından sağlanan belirli kullanıcı için öğeleri önermek için kimlikleri temsil eder. Diğer beş sütun ilgi azalan düzende kullanıcıya önerilen öğeleri temsil eder. Örneğin, ilk satırda müşteri U1048 en önerilen Restoran 135018, 134975, 135021 ve 132862 tarafından izlenen 134986 ' dir.

**Belirli bir kullanıcıyla ilişkili kullanıcı Bul**

Seçerek **ilgili kullanıcıları** altında **öneren tahmin türü**, verili bir kullanıcı için ilgili kullanıcıları bulmak için öneren sistem isteyen. Benzer Tercihler kullanıcılar ilgili kullanıcılardır. Bu senaryoda seçmek için son parametre *ilgili kullanıcı seçimi*. Seçenek **gelen kullanıcılar, derecelendirilmiş öğeleri (model değerlendirme)** eğitim işlemi sırasında model değerlendirme öncelikle içindir. Seçin **tüm kullanıcıların** bu tahmin aşaması için. Görsel olarak [puan Matchbox öneren] [ score-matchbox-recommender] çıkış şekil 23 gibi görünüyor.

![Öneren sistem--ilgili kullanıcıları puan sonucu](./media/interpret-model-results/23.png)

Şekil 23. Öneren sisteminin--ilgili kullanıcıları Puanlama sonuçlarını Görselleştirme

İlk altı sütunların verilen kullanıcı kimlikleri ilgili kullanıcıları bulmak gerekli giriş verileri tarafından sağlanan gibi gösterir. Diğer beş sütun ilgi azalan sırada kullanıcının tahmin edilen ilgili kullanıcıları depolar. Örneğin, ilk satırda müşteri U1048 en ilgili müşteri U1066, U1044, U1017 ve U1072 tarafından izlenen U1051 ' dir.

**Belirli bir öğesiyle ilgili öğeleri bulma**

Seçerek **ilgili öğeler** altında **öneren tahmin türü**, belirli bir öğe için ilgili öğeleri bulmak için öneren sistem istiyoruz. İlgili öğeler aynı kullanıcı tarafından beğendiğinizi olasılıkla öğelerdir. Bu senaryoda seçmek için son parametre *ilgili öğe seçimi*. Seçenek **gelen derecelendirilmiş öğeleri (model değerlendirme)** eğitim işlemi sırasında model değerlendirme öncelikle içindir. Seçeneğini belirledik **gelen tüm öğeleri** bu tahmin aşaması için. Görsel olarak [puan Matchbox öneren] [ score-matchbox-recommender] çıkış şekil 24 gibi görünüyor.

![Puan sonuç öneren sisteminin--ilgili öğeler](./media/interpret-model-results/24.png)

Şekil 24. Öneren sisteminin--ilgili öğeler Puanlama sonuçlarını Görselleştirme

İlk altı sütunların verilen öğesi ilgili öğeleri bulmak için giriş verileri tarafından sağlanan gibi gerekli kimlikleri temsil eder. Diğer beş sütun uygunluk açısından azalan sırada tahmin edilen ilgili öğeler öğenin depolar. Örneğin, ilk satırda en uygun öğesinin 135026 135035, 132875, 135055 ve 134992 tarafından izlenen 135074 öğesidir.

**Web hizmet yayımı**

Bu denemeler Öngörüler almak için web Hizmetleri olarak yayımlama işlemi her dört senaryo için benzer. Burada size ikinci senaryo (belirli bir kullanıcının öğelerine önerilir) örnek olarak alın. Diğer üç ile aynı yordamı izleyebilirsiniz.

Eğitilmiş öneren sistem eğitilen model olarak kaydetme ve istendiği gibi tek bir kullanıcı kimliği sütunu için giriş verileri filtreleme, Şekil 25 olduğu gibi deneme bağlanacağını ve web hizmeti olarak yayımlayın.

![Deneme Restoran öneri sorununun Puanlama](./media/interpret-model-results/25.png)

Şekil 25. Deneme Restoran öneri sorununun Puanlama

Web hizmeti çalıştıran, döndürülen sonuç şekil 26 gibi görünüyor. Kullanıcı U1048 için beş önerilen Restoran 134986, 135018, 134975, 135021 ve 132862 ' dir.

![Öneren sistem hizmeti örneği](./media/interpret-model-results/25_1.png)

![Örnek deneme sonuçları](./media/interpret-model-results/26.png)

Şekil 26. Web hizmeti Restoran öneri sorun sonucu

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
