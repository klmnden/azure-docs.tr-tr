---
title: "Machine Learning modeli performansını değerlendirmek | Microsoft Docs"
description: "Azure Machine Learning modeli performansını değerlendirmek açıklanmaktadır."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 5dc5348a-4488-4536-99eb-ff105be9b160
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bradsev;garye
ms.openlocfilehash: 48ce4584f7270d78b1d09b848bfdd305d03012b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Azure Machine Learning’de model performansını değerlendirme
Bu makalede, Azure Machine Learning Studio'da bir model performansını değerlendirmek gösterir ve bu görev için kullanılabilir ölçümleri kısa bir açıklamasını sağlar. Üç yaygın denetimli öğrenme senaryo sunulur: 

* regresyon
* ikili sınıflandırma 
* çok sınıflı sınıflandırma

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Bir model performansını değerlendirirken, veri bilimi işlem çekirdek aşamada biridir. Bunu nasıl başarılı bir veri kümesinin Puanlama (tahminleri) eğitilen modeli tarafından da olduğunu gösterir. 

Azure Machine Learning modeli değerlendirme iki modülleri öğrenme kendi ana makine aracılığıyla destekler: [Evaluate Model] [ evaluate-model] ve [çapraz doğrulama modeli][cross-validate-model]. Bu modülleri nasıl machine learning ve istatistikleri yaygın olarak kullanılan ölçümleri sayısı bakımından modelinizi gerçekleştirir görmenize olanak sağlar.

## <a name="evaluation-vs-cross-validation"></a>Değerlendirme vs. Çapraz doğrulama
Değerlendirme ve çapraz doğrulama, model performansını ölçmek için standart yöntemleridir. Her ikisi de inceleme veya bunlar başka modellerinin karşı karşılaştırmak değerlendirme ölçümleri oluşturur.

[Modeli değerlendirin] [ evaluate-model] girişi (veya 2 farklı modelleri performansını karşılaştırmak istediğiniz durumda 2) olarak puanlanmış veri kümesi bekliyor. Bu kullanarak modeli eğitmek gerektiği anlamına gelir [modeli eğitmek] [ train-model] bazı veri kümesi kullanma modülü ve yapma tahminleri [Score Model] [ score-model] sonuçları değerlendirmeden önce modülü. Değerlendirme temel puanlanmış etiketler/olasılıklar true etiketlerin yanı sıra, her biri çıktı tarafından [Score Model] [ score-model] modülü.

Alternatif olarak, çapraz doğrulama tren puanı değerlendirme işlemleri (10 Katlama) sayısı gerçekleştirmek için otomatik olarak giriş verileri farklı kümelerine kullanabilirsiniz. Burada bir test için ayrılmıştır, 10 parça ve eğitim için diğer 9 giriş verileri bölün. Bu işlem 10 kez yinelenir ve değerlendirme ölçümleri ortalaması alınır. Bu, bir model yeni veri kümeleri için ne kadar iyi generalize belirlemede yardımcı olur. [Çapraz doğrulama modeli] [ cross-validate-model] modülü eğitimsiz modeli ve bazı etiketli veri kümesini alır ve her bir ortalama sonuçları ek olarak 10 Katlama değerlendirme sonuçlarını çıkarır.

Aşağıdaki bölümlerde, biz oluşturacak basit regresyon ve sınıflandırma modelleri ve her ikisini de kullanarak kendi performansını değerlendirmek [Evaluate Model] [ evaluate-model] ve [çapraz doğrulama modeli] [ cross-validate-model] modüller.

## <a name="evaluating-a-regression-model"></a>Bir regresyon modeli değerlendirme
Biz boyutları, beygir gücü, altyapısı belirtimlerin vb. gibi bazı özellikleri kullanılarak bir otomobilin fiyatını tahmin etmek istediğinizi varsayalım. Bu tipik regresyon, sorunudur burada hedef değişkeni (*fiyat*) sürekli sayısal bir değerdir. Biz, belirli bir araba özellik değerlerine göre bu otomobil fiyatını tahmin etmek bir Basit doğrusal regresyon modeli uygun olamaz. Bu regresyon modeli, biz üzerinde eğitilmiş aynı veri Puanlama amacıyla kullanılabilir. Biz araba tahmin edilen fiyatları oluşturduktan sonra ne kadar tahminleri gerçek fiyatlar ortalama sapma adresindeki bakarak biz model performansını değerlendirebilirsiniz. Bunu göstermek için kullanırız *otomobil fiyat verileri (ham) dataset* bulunan **kaydedilen veri kümeleri** Azure Machine Learning Studio bölümünde.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıdaki modüller, Azure Machine Learning Studio'da çalışma alanınıza ekleyin:

* Otomobil fiyat verileri (ham)
* [Doğrusal regresyon][linear-regression]
* [Train Model][train-model]
* [Score Model][score-model]
* [Modeli değerlendirin][evaluate-model]

Aşağıda Şekil 1'de gösterildiği gibi bağlantı noktalarını bağlamak ve etiket sütunu olarak ayarlanmış [Train Model] [ train-model] modülüne *fiyat*.

![Bir regresyon modeli değerlendirme](./media/evaluate-model-performance/1.png)

Şekil 1 '. Bir regresyon modeli değerlendiriliyor.

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme sonuçlarını İnceleme
Denemeyi çalıştırdıktan sonra çıkış bağlantı noktasına tıklatabilirsiniz [Evaluate Model] [ evaluate-model] modülü ve select *Görselleştir* değerlendirme sonuçlarını görmek için. Değerlendirme ölçümleri regresyon modelleri için kullanılabilir olan: *ortalama mutlak hata*, *kök ortalama mutlak hata*, *göreli mutlak hata*, *göreli karesi alınmış hata*ve *Coefficient of Determination*.

"Error" Buraya terim tahmin edilen değer ile gerçek değer arasındaki farkı temsil eder. Mutlak değerini ya da bu fark kare genellikle hesaplanan boyunca tüm örneklerde, hata toplam büyüklüğünü yakalamak için tahmin edilen ve gerçek değer arasındaki farkı bazı durumlarda negatif olması gerektiği. Tahmine dayalı bir regresyon modeli kendi tahminlerin gerçek değerlerle gelen ortalama sapmasını bakımından performansını hata ölçümleri. Alt hata değerlerini tahminleri yaparken daha doğru bir modeldir anlamına gelir. Genel bir hata ölçümü modelin verilere mükemmel uyduğunu 0 anlamına gelir.

Katsayısı olarak da bilinen R olan kare, ayrıca ne kadar iyi veri modeli uygun ölçü standart bir yoludur. Model tarafından açıklanan değişim oranı olarak yorumlanabilir. Yüksek bir oranda bu durumda, daha iyi 1 burada bir mükemmel gösterir.

![Doğrusal regresyon değerlendirme ölçümleri](./media/evaluate-model-performance/2.png)

Şekil 2. Doğrusal regresyon değerlendirme ölçümleri.

### <a name="using-cross-validation"></a>Doğrulama kullanma
Daha önce belirtildiği gibi yinelenen eğitim, Puanlama ve kullanılarak otomatik olarak değerlendirmeleri gerçekleştirebilirsiniz [çapraz doğrulama modeli] [ cross-validate-model] modülü. Tüm yapmanız gereken bu durumda olan bir veri kümesi, bir eğitimsiz modeli ve [çapraz doğrulama modeli] [ cross-validate-model] Modülü (aşağıdaki şekilde bakın). Etiket sütun kümesine gerektiğini unutmayın *fiyat* içinde [çapraz doğrulama modeli] [ cross-validate-model] modülünün özellikleri.

![Çapraz-doğrularken bir regresyon modeli](./media/evaluate-model-performance/3.png)

Şekil 3 '. Çapraz-bir regresyon modeli doğrulanıyor.

Denemeyi çalıştırdıktan sonra değerlendirme sonuçlarını sağ çıkış bağlantı noktasına tıklayarak inceleyebilirsiniz [çapraz doğrulama modeli] [ cross-validate-model] modülü. Her bir yineleme (Katlama) ve her (Şekil 4) ölçümleri ortalama sonuçları için bu ölçümleri ayrıntılı görünümünü sağlayacaktır.

![Çapraz doğrulama sonuçlarını bir regresyon modeli](./media/evaluate-model-performance/4.png)

Şekil 4 '. Çapraz doğrulama sonuçlarını bir regresyon modeli.

## <a name="evaluating-a-binary-classification-model"></a>Bir ikili sınıflandırma modeli değerlendirme
Örneğin yalnızca iki olası sonucunu bir ikili sınıflandırma senaryosunda, hedef değişkeni vardır: {0, 1} veya {yanlış, doğru}, {negatif, pozitif}. Bir veri kümesi yetişkin çalışanların bazı verilmiştir varsayın demografik ve İstihdam değişkenleri ve geliri düzeyi, bir ikili değişken değerleri tahmin etmek istenir {"< 50 K =", "> 50K"}. Diğer bir deyişle, negatif sınıfı küçük veya eşittir 50 K yıllık olun çalışanlar ve diğer tüm çalışanlar pozitif sınıfı temsil eder. Regresyon senaryo olduğu gibi biz bir modeli eğitmek, bazı veriler Puanlama ve sonuçları değerlendirin. Burada temel fark, Azure Machine Learning hesaplar ölçümleri ve çıkışları seçimdir. Gelir düzeyi tahmin senaryo göstermek için kullanacağız [yetişkin](http://archive.ics.uci.edu/ml/datasets/Adult) bir Azure Machine Learning deneme oluşturup iki sınıflı Lojistik regresyon modeli, yaygın olarak kullanılan bir ikili sınıflandırıcı performansını değerlendirmek için veri kümesi.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıdaki modüller, Azure Machine Learning Studio'da çalışma alanınıza ekleyin:

* Yetişkin Census gelir ikili sınıflandırma veri kümesi
* [İki sınıflı Lojistik regresyon][two-class-logistic-regression]
* [Train Model][train-model]
* [Score Model][score-model]
* [Modeli değerlendirin][evaluate-model]

Aşağıda Şekil 5'te gösterildiği gibi bağlantı noktalarını bağlamak ve etiket sütunu olarak ayarlanmış [Train Model] [ train-model] modülüne *gelir*.

![Bir ikili sınıflandırma modeli değerlendirme](./media/evaluate-model-performance/5.png)

Şekil 5. Bir ikili sınıflandırma modeli değerlendiriliyor.

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme sonuçlarını İnceleme
Denemeyi çalıştırdıktan sonra çıkış bağlantı noktasına tıklatabilirsiniz [Evaluate Model] [ evaluate-model] modülü ve select *Görselleştir* (Şekil 7) değerlendirme sonuçlarını görmek için. İkili sınıflandırma modelleri için kullanılabilir değerlendirme ölçümler şunlardır: *doğruluğu*, *duyarlık*, *geri çağırma*, *F1 puan*, ve *AUC*. Buna ek olarak, modül true pozitif sonuç, false negatif, hatalı pozitif sonuç ve doğru negatif sayısını gösteren bir karışıklığı matris çıkarır yanı *ROC*, *duyarlık/geri çağırma*, ve *Yükselt* Eğriler.

Doğruluk yalnızca doğru sınıflandırılmış örnekleri oranını ' dir. Bu genellikle, bir sınıflandırıcı değerlendirirken bakmak ilk ölçümüdür. Ancak, test verileri olduğunda (burada örnekleri çoğu sınıflardan birine ait) dengesini ya da daha fazla ilgilendiğiniz performansına sınıfları birini, doğruluk gerçekten sınıflandırıcı verimliliğini yakalamaz. Gelir level sınıflandırma senaryosunda, burada örneklerinin % 99 yıl başına 50 K eşit veya daha az kazanma kişilerin temsil eder bazı veriler üzerinde test varsayalım. Sınıf tahmin ederek çalışmasını 0.99 doğruluğu elde mümkündür "< 50 K =" tüm örnekleri için. Sınıflandırıcı bu durumda genel iyi bir iş yaparak görünüyor, ancak gerçekte, herhangi bir high-income kişiler (% 1) doğru sınıflandırmak başarısız oluyor.

Bu nedenle, değerlendirme daha belirli yönlerini yakalama ek ölçümler işlem yararlıdır. Bu tür ölçümleri ayrıntıları geçmeden önce ikili sınıflandırma değerlendirme karışıklığı matris anlamak önemlidir. Eğitim kümesi sınıfı etiketler adlandırdığımız genellikle yalnızca 2 olası değerler olarak olumlu veya olumsuz alabilir. Bir sınıflandırıcı doğru şekilde tahmin pozitif ve negatif örnekleri true pozitif sonuç (TP) ve doğru negatif (TN) adlandırılır. Benzer şekilde, yanlış sınıflandırılmış örnekleri hatalı pozitif sonuç (FP) ve false negatif (FN) adı verilir. Karışıklığı matris yalnızca bu kategorilerin her biri 4 altında kalan örneklerinin sayısını gösteren bir tablo değil. Azure Machine Learning pozitif sınıfı kümesindeki iki sınıfın olduğu otomatik olarak karar verir. Sınıf etiketleri Boole veya tamsayılar varsa, 'true' veya '1' etiketli örnekleri pozitif sınıfı atanır. Etiketleri dizeleri varsa, ilk düzeyi ikinci düzey pozitif sınıfı iken negatif sınıfı olarak seçilir ve geliri dataset durumda olduğu gibi etiketleri alfabetik olarak sıralanır.

![İkili sınıflandırma karışıklığı Matrisi](./media/evaluate-model-performance/6a.png)

Şekil 6. İkili sınıflandırma karışıklığı matrisi.

Gelir sınıflandırma sorunu geri dönerseniz, biz kullanılan sınıflandırıcı performansını iyileştirmemize yardımcı birkaç değerlendirme soruları anlamak sorulacak istersiniz. Çok doğal bir soru: ' Kime getirisi olması için model tahmin kişiler dışında > 50 kaç doğru şekilde sınıflandırıldığını K (TP + FP) (TP)?' Bu soru bakarak yanıtlanması **duyarlık** doğru olarak sınıflandırıldığını pozitif sonuç oranı olan modelinin: TP/(TP+FP). Başka bir ortak Soru "dışında tüm yüksek gelir çalışanlarla getirisi > 50 k (TP + FN) kaç sınıflandırıcı doğru sınıflandırmak (TP)". Gerçekte budur **geri çağırma**, ya da doğru pozitif oranı: sınıflandırıcı TP/(TP+FN). Duyarlık geri çekme arasındaki belirgin bir denge olduğunu fark edebilirsiniz. Örneğin, görece dengeli bir veri kümesi verildiğinde, çoğunlukla pozitif örnekleri tahmin bir sınıflandırıcı sahip olabilir yüksek bir geri çağırma, ancak yerine düşük duyarlılık birçok negatif örneklerinin gibi çok sayıda hatalı pozitif sonuç elde edilen misclassified. Bu iki ölçümleri nasıl farklılık, bir çizim görmek için değerlendirme sonucu çıkış sayfa ' DUYARLIK/geri ÇAĞIRMA' eğrisindeki tıklatabilirsiniz (Şekil 7 parçası sol üst).

![İkili sınıflandırma değerlendirme sonuçları](./media/evaluate-model-performance/7.png)

Şekil 7. İkili sınıflandırma değerlendirme sonuçları.

Başka bir sık kullanılan ölçümüdür ilgili **F1 puan**, hem duyarlık hem de geri çağırma dikkate alır. 2 bu ölçümleri harmonik olduğundan ve bu nedenle hesaplanır: F1 = 2 (x duyarlık geri çekme) / (duyarlık + geri çağırma). F1 puanı, tek bir sayı hesaplanmasında özetlemek için en iyi yolu olmakla birlikte, her zaman hem duyarlık hem de birlikte daha iyi bir sınıflandırıcı nasıl davranacağını anlamak için geri çağırma sırasında aramak için iyi bir uygulamadır.

Ayrıca, bir yanlış pozitif hızını karşılaştırması true pozitif oranı inceleyebilirsiniz **alıcı işletim karakteristiğini (ROC)** eğri ve karşılık gelen **alanı altında eğri (AUC)** değeri. Yakınsa bu eğri için sol üst köşesinde, daha iyi sınıflandırıcı 's performans (Bu en üst düzeye çıkarma true pozitif oranı yanlış pozitif hızı en aza indirerek). Çizim, rastgele tahmin yakın olan tahminlerde eğilimindedir sınıflandırıcı sonucundan çapraz yakın olan Eğriler.

### <a name="using-cross-validation"></a>Doğrulama kullanma
Regresyon örnekteki biz art arda eğitmek, puan ve farklı veri alt kümesi otomatik olarak değerlendirmek için çapraz doğrulama gerçekleştirebilirsiniz. Benzer şekilde, biz kullanabilirsiniz [çapraz doğrulama modeli] [ cross-validate-model] modülü, bir eğitimsiz Lojistik regresyon modeli ve bir veri kümesi. Etiket sütununda ayarlamak *gelir* içinde [çapraz doğrulama modeli] [ cross-validate-model] modülünün özellikleri. Denemeyi çalıştırmak ve sağ tıklayarak bağlantı noktası çıktı sonra [çapraz doğrulama modeli] [ cross-validate-model] modülü, ikili sınıflandırma ölçüm değerleri her Katlama ayrıca ortalama ve her standart sapmasını görebiliriz. 

![Çapraz-doğrulama ikili sınıflandırma modeli](./media/evaluate-model-performance/8.png)

Şekil 8'de. Çapraz-ikili sınıflandırma Model doğrulama.

![Bir ikili sınıflandırıcı çapraz doğrulama sonuçlarını](./media/evaluate-model-performance/9.png)

Şekil 9. Bir ikili sınıflandırıcı çapraz doğrulama sonuçları.

## <a name="evaluating-a-multiclass-classification-model"></a>Bir çok sınıflı sınıflandırma modeli değerlendirme
Bu deneme popüler kullanacağız [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") 3 farklı türleri (sınıflar) iris tesis örneklerini içeren veri kümesi. 4 özellik değerlerini (sepal uzunluğu/genişlik ve petal uzunluğu/genişlik) her örneği vardır. Önceki denemeleri, eğitilmiş ve aynı veri kümeleri kullanarak modelleri test. Burada, kullanacağız [bölünmüş veri] [ split] 2 veri alt kümesi oluşturun, ilk, eğitim ve puanlama ve ikinci değerlendirmek için modülü. Iris dataset üzerinde genel kullanıma [UCI Machine Learning deposu](http://archive.ics.uci.edu/ml/index.html)ve kullanarak indirilebilir bir [veri içeri aktarma] [ import-data] modülü.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıdaki modüller, Azure Machine Learning Studio'da çalışma alanınıza ekleyin:

* [Veri alma][import-data]
* [Veya çoklu sınıflar karar orman][multiclass-decision-forest]
* [Veri bölme][split]
* [Train Model][train-model]
* [Score Model][score-model]
* [Modeli değerlendirin][evaluate-model]

Bağlantı aşağıda Şekil 10'da gösterildiği gibi bağlayın.

Etiket sütun dizini ayarlamak [Train Model] [ train-model] modülü 5. Veri kümesi başlık satırı yok ancak bir sınıf etiketleri beşinci sütununda olduğunu biliyoruz.

Tıklayın [veri içeri aktarma] [ import-data] modülü ve kümesi *veri kaynağı* özelliğine *Web URL'si HTTP üzerinden*ve *URL* http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data için.

Eğitim için kullanılacak örnek kesir ayarlamak [bölünmüş veri] [ split] Modülü (örneğin 0,7).

![Çok sınıflı sınıflandırıcı değerlendirme](./media/evaluate-model-performance/10.png)

Şekil 10. Çok sınıflı sınıflandırıcı değerlendirme

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme sonuçlarını İnceleme
Denemeyi çalıştırmak ve çıkış bağlantı noktasına tıklayın [Evaluate Model][evaluate-model]. Değerlendirme sonuçları karışıklığı matrisi biçiminde bu durumda sunulur. Matris gerçek tüm 3 sınıfları için tahmin edilen örnekleri ve gösterir.

![Çok sınıflı sınıflandırma değerlendirme sonuçları](./media/evaluate-model-performance/11.png)

Şekil 11. Çok sınıflı sınıflandırma değerlendirme sonuçları.

### <a name="using-cross-validation"></a>Doğrulama kullanma
Daha önce belirtildiği gibi yinelenen eğitim, Puanlama ve kullanılarak otomatik olarak değerlendirmeleri gerçekleştirebilirsiniz [çapraz doğrulama modeli] [ cross-validate-model] modülü. Bir veri kümesi, bir eğitimsiz modeli gerekir ve bir [çapraz doğrulama modeli] [ cross-validate-model] Modülü (aşağıdaki şekilde bakın). Etiket sütunu yeniden ayarlamanızı [çapraz doğrulama modeli] [ cross-validate-model] Modülü (5 Bu durumda sütun dizini). Denemeyi çalıştırmak ve sağ tıklayarak bağlantı noktası çıktı sonra [çapraz doğrulama modeli][cross-validate-model], ortalama ve standart sapma yanı sıra her Katlama ölçüm değerlerini inceleyebilirsiniz. Burada gösterilen ölçümleri ikili sınıflandırma durumda ele alınan olanlara benzerdir. Ancak, genel olumlu veya olumsuz sınıfı yok olduğundan çok sınıflı sınıflandırmasında doğru pozitif/negatif ve yanlış pozitif/negatif bir sınıf başına temelinde sayım tarafından yapıldığını unutmayın. Örneğin, duyarlılık veya geri çağırma 'Iris-setosa' sınıfının hesaplanırken, bu pozitif sınıf ve negatif olarak diğerlerinin tümü olduğu varsayılır.

![Çapraz-doğrulama çok sınıflı sınıflandırma modeli](./media/evaluate-model-performance/12.png)

Şekil 12. Çapraz-çok sınıflı sınıflandırma Model doğrulama.

![Çapraz doğrulama sonuçlarını çok sınıflı sınıflandırma modeli](./media/evaluate-model-performance/13.png)

Şekil 13. Çapraz doğrulama sonuçlarını çok sınıflı sınıflandırma modeli.

<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/

