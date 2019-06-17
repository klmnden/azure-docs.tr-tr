---
title: Model performansını değerlendirme
titleSuffix: Azure Machine Learning Studio
description: Bu makalede, Azure Machine Learning Studio'da bir model performansını değerlendirme yapmayı gösteren ve bu görev için mevcut olan ölçümler için kısa bir açıklama sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 37ab56c377bc53a7300b51ffc709ea8d1b9d6f9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60750656"
---
# <a name="how-to-evaluate-model-performance-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da model performansını değerlendirme

Bu makalede, Azure Machine Learning Studio'da bir model performansını değerlendirme yapmayı gösteren ve bu görev için mevcut olan ölçümler için kısa bir açıklama sağlar. Denetimli öğrenmede senaryoları üç sunulur: 

* Regresyon
* İkili sınıflandırma 
* Sınıflı sınıflandırma



Model performansını değerlendirme veri bilimi işlemi temel aşamaları biridir. Bunu nasıl başarılı bir veri kümesinin Puanlama (tahminler) eğitilen bir modeli tarafından da olduğunu gösterir. 

Azure Machine Learning Studio iki ana makine öğrenme modülleri aracılığıyla model değerlendirme destekler: [Modeli değerlendirme] [ evaluate-model] ve [çapraz doğrulama, Model][cross-validate-model]. Bu modüller, makine öğrenimi ve istatistikleri yaygın olarak kullanılan ölçümleri sayısı bakımından modelinizi nasıl performans gösterdiğini görmenize olanak sağlar.

## <a name="evaluation-vs-cross-validation"></a>Değerlendirme vs. Çapraz doğrulama
Değerlendirme ve çapraz doğrulama, model performansını ölçmek için standart yolları açıklanmıştır. Her ikisi de inceleme veya bu modeller karşı karşılaştırma değerlendirme ölçümleri oluşturur.

[Modeli değerlendirme] [ evaluate-model] puanlanmış bir veri kümesi (veya iki farklı model performansını karşılaştırmak istediğiniz durumda 2) girdi olarak bekler. Bunu kullanarak modeli eğitmek gerektiği anlamına gelir [modeli eğitme] [ train-model] bazı veri kümesini kullanarak modülü ve yapma Öngörüler [Score Model] [ score-model]sonuçları değerlendirmeden önce modülü. Değerlendirme, puanlanmış etiketler/olasılıklar true etiketlerin yanı sıra dayanır, her biri çıktı tarafından [Score Model] [ score-model] modülü.

Alternatif olarak, çapraz doğrulama birkaç train puanı değerlendirme işlemleri (10 hatları) gerçekleştirmek için otomatik olarak giriş verisi farklı alt kümelerinde üzerinde kullanabilirsiniz. Burada bir test için ayrılmıştır, 10 parça ve eğitim için diğer 9 giriş verileri bölün. Bu işlem 10 kez yinelenir ve değerlendirme ölçümleri ortalaması alınır. Bu, ne kadar iyi bir model yeni veri kümelerine generalize belirlemede yardımcı olur. [Çapraz doğrulama, Model] [ cross-validate-model] modülü deneyimsiz bir model ve bazı etiketli veri kümesini alır ve ortalama sonuçları ek olarak 10 kat sayısı değerlendirme sonuçları çıkarır.

Aşağıdaki bölümlerde, oluşturacak basit regresyon ve sınıflandırma modelleri ve ikisi de bunların performansı değerlendirmek [Evaluate Model] [ evaluate-model] ve [çapraz doğrulama modeli ] [ cross-validate-model] modüller.

## <a name="evaluating-a-regression-model"></a>Bir regresyon modeli değerlendirme
Boyutlar, beygir gücü, altyapısı özellikleri ve benzeri gibi bazı özellikleri kullanarak bir arabanın fiyatını tahmin etmek istiyoruz varsayılır. Bu, tipik bir regresyon problemi olduğunu nerede hedef değişkeni (*fiyat*) sürekli bir sayısal değerdir. Biz, belirli bir araba özellik değerlerine, otomobil fiyatını tahmin edebilen bir Basit doğrusal regresyon modeli uygun olamaz. Bu regresyon modeli, biz üzerinde geliştirilen aynı veri puanlamak için kullanılabilir. Biz bu arabalar tahmin edilen fiyatları aldıktan sonra biz ne kadar tahminlerin gerçek fiyatlarına ilişkin ortalama sapma adresindeki bakarak model performansını değerlendirebilirsiniz. Bunu açıklamak için kullanırız *otomobil fiyat verileri (ham) dataset* bulunan **kaydedilmiş veri kümeleri** bölümünde Azure Machine Learning Studio'da.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıdaki modüller, Azure Machine Learning Studio çalışma alanınıza ekleyin:

* Otomobil fiyat verileri (ham)
* [Doğrusal regresyon][linear-regression]
* [Modeli eğitme][train-model]
* [Model Puanlama][score-model]
* [Modeli değerlendirme][evaluate-model]

Aşağıda Şekil 1'de gösterildiği gibi bağlantı bağlayın ve etiket sütununda ayarlayın [modeli eğitme] [ train-model] modülüne *fiyat*.

![Bir regresyon modeli değerlendirme](./media/evaluate-model-performance/1.png)

Şekil 1. Bir regresyon modeli değerlendiriliyor.

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme sonuçlarını İnceleme
Denemeyi çalıştırdıktan sonra çıkış bağlantı noktasına tıklayabilirsiniz [Evaluate Model] [ evaluate-model] modülü ve select *Görselleştir* değerlendirme sonuçlarını görmek için. Regresyon modelleri için mevcut olan değerlendirme ölçümler şunlardır: *Mean Absolute Error*, *kök ortalama mutlak hata*, *göreli mutlak hata*, *göreli karesi alınmış hata*ve *katsayısı Belirleme*.

Terim "error" Buraya tahmin edilen değer true değeri arasındaki farkı temsil eder. Öngörülen ve gerçek değer arasındaki farkı, bazı durumlarda negatif olabileceğinden mutlak değeri veya bu farkı karesini genellikle tüm örneklerinde hata toplam büyüklüğünü yakalamak için hesaplanır. Hata ölçümleri, Tahmine dayalı bir regresyon modeli, Öngörüler true değerlerinin ortalaması sapmasını açısından performansını ölçme. Hata düşük değerler daha doğru tahminler yaparken modelidir anlamına gelir. Genel bir hata ölçümü sıfır model verileri mükemmel bir şekilde uyan anlamına gelir.

Katsayısı olarak da bilinen R olan kare, ayrıca nasıl veri modelinin en uygun ölçü standart bir yoludur. Model tarafından açıklanan değişim oranı olarak yorumlanabilir. Daha yüksek bir oran bu durumda, daha iyi 1 mükemmel bir uyum gösterir.

![Doğrusal regresyon değerlendirme ölçümleri](./media/evaluate-model-performance/2.png)

Şekil 2. Doğrusal regresyon değerlendirme ölçümleri.

### <a name="using-cross-validation"></a>Doğrulama kullanma
Daha önce bahsedildiği gibi yinelenen eğitim, Puanlama ve değerlendirmeleri kullanarak otomatik olarak gerçekleştirebilir [çapraz doğrulama, Model] [ cross-validate-model] modülü. Tüm yapmanız gereken bu durumda olan bir veri kümesi, bir deneyimsiz modeli ve [çapraz doğrulama, Model] [ cross-validate-model] Modülü (aşağıdaki şekilde bakın). Etiket sütunu ayarlanacak ihtiyacınız *fiyat* içinde [çapraz doğrulama, Model] [ cross-validate-model] modülün özellikleri.

![Çapraz-doğrularken bir regresyon modeli](./media/evaluate-model-performance/3.png)

Şekil 3. Çapraz-bir regresyon modeli doğrulanıyor.

Denemeyi çalıştırdıktan sonra değerlendirme sonuçlarını doğru çıkış bağlantı noktasına tıklayarak inceleyebilirsiniz [çapraz doğrulama, Model] [ cross-validate-model] modülü. Her yineleme (Katlama) ve ortalama sonuçları (Şekil 4) ölçümlerinin her biri için bu ölçümleri daha ayrıntılı bir görünümünü sağlayacaktır.

![Çapraz doğrulama sonuçlarını bir regresyon modeli](./media/evaluate-model-performance/4.png)

Şekil 4. Çapraz doğrulama sonuçlarını bir regresyon modeli.

## <a name="evaluating-a-binary-classification-model"></a>İkili sınıflandırma modelinde değerlendiriliyor
Örneğin yalnızca iki sonuçtan, ikili sınıflandırma senaryoda, hedef değişkeni sahiptir: {0, 1} veya {yanlış, doğru}, {negatif, pozitif}. Bazı yetişkinlere yönelik çalışan bir veri kümesi verilir varsayar demografik ve çalışma değişkenler ve değerlerle ikili bir değişken gelir düzeyinde tahmin istenir {"< 50 bin =", "> 50 K"}. Diğer bir deyişle, 50 bin yılda eşit veya daha az olmak çalışanları negatif sınıfı temsil eder ve tüm diğer çalışanlarla pozitif sınıfı temsil eder. Regresyon senaryo olduğu gibi biz bir model eğitip, bazı verileri puanlamak ve sonuçları değerlendirin. Burada temel fark, Azure Machine Learning Studio hesaplar ölçümleri ve çıktılar seçimdir. Gelir düzeyi tahmin senaryoyu göstermek üzere kullanacağız [yetişkinlere yönelik](https://archive.ics.uci.edu/ml/datasets/Adult) Studio deneme oluşturma ve iki sınıflı Lojistik regresyon modeli, yaygın olarak kullanılan bir ikili dosya sınıflandırıcı performansını değerlendirmek için veri kümesi.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıdaki modüller, Azure Machine Learning Studio çalışma alanınıza ekleyin:

* Yetişkin Görselleştirmenizdeki gelir ikili sınıflandırma veri kümesi
* [İki sınıflı Lojistik regresyon][two-class-logistic-regression]
* [Modeli eğitme][train-model]
* [Model Puanlama][score-model]
* [Modeli değerlendirme][evaluate-model]

Aşağıda Şekil 5'te gösterildiği gibi bağlantı bağlayın ve etiket sütununda ayarlayın [modeli eğitme] [ train-model] modülüne *gelir*.

![İkili sınıflandırma modelinde değerlendiriliyor](./media/evaluate-model-performance/5.png)

Şekil 5. İkili sınıflandırma modelinde değerlendiriliyor.

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme sonuçlarını İnceleme
Denemeyi çalıştırdıktan sonra çıkış bağlantı noktasına tıklayabilirsiniz [Evaluate Model] [ evaluate-model] modülü ve select *Görselleştir* (Şekil 7) değerlendirme sonuçlarını görmek için. İkili sınıflandırma modelleri için mevcut olan değerlendirme ölçümler şunlardır: *Doğruluk*, *duyarlık*, *geri çağırma*, *F1 puanı*, ve *AUC*. Ayrıca, modül doğru pozitif sonuçlar, yanlış negatif, hatalı pozitif sonuç ve gerçek negatif sayısını gösteren bir karışıklık matrisi çıkışları yanı *ROC*, *duyarlık/geri çağırma*, ve  *Lift* eğrileri.

Doğruluk oranı doğru sınıflandırılmış örnekleri yalnızca ' dir. Genellikle, bir sınıflandırıcı değerlendirirken gözden geçirmeniz ilk ölçüm olur. Ancak, test verileri olduğunda (burada örneklerin çoğu sınıflardan birine ait) dengesiz ya da daha fazla ilgilendiğiniz sınıfların ya da bir performans, kesinlik gerçekten sınıflandırıcı verimliliğini yakalamaz. Gelir düzeyi sınıflandırma senaryosunda, örneklerin % 99 yıl başına 50 bin eşit veya daha az kazanın kişilerin nerede temsil eden bazı verileri sınamakta olduğunuz varsayılır. Sınıfı hakkında tahminde bulunarak 0.99 doğruluk elde etmek mümkündür "< 50 bin =" tüm örnekleri için. Sınıflandırıcı bu durumda genel iyi bir iş çıkarıyor görünüyor, ancak gerçekte herhangi high-income kişilerin (% 1) sınıflandırmak başarısız doğru.

Bu nedenle, işlem daha belirli yönlerini değerlendirme yakalama ek ölçümler yararlıdır. Bu ölçümler ayrıntılarına geçmeden önce bir ikili sınıflandırma değerlendirme karışıklık matrisi anlamak önemlidir. Eğitim kümesi sınıfı etiketleri, genellikle diyoruz yalnızca 2 olası değerleri pozitif veya negatif olarak alabilir. Sınıflandırıcı doğru şekilde tahmin eden pozitif ve negatif örnekleri sırasıyla doğru pozitif sonuçlar (TP) ve true negatif (TN) verilir. Benzer şekilde, yanlış sınıflandırılmış örnekleri, hatalı pozitif sonuçları (dp) ve hatalı negatif (FN) adı verilir. Karışıklık matrisi yalnızca bu kategorilerden her biri 4 altında kalan örnek sayısını gösteren bir tablo var. Azure Machine Learning Studio, veri kümesinde iki sınıf pozitif sınıfı olduğu otomatik olarak karar verir. Ardından sınıf etiketleri Boole veya tamsayılar, 'true' veya '1' etiketli örnekleri pozitif sınıfı atanır. Etiketleri dizelerdir gelir dataset durumda olduğu gibi etiketlerin alfabetik olarak sıralanır ve ilk düzeyi, ikinci düzey pozitif bir sınıftır negatif sınıfı olarak seçilir.

![İkili sınıflandırma karışıklık Matrisi](./media/evaluate-model-performance/6a.png)

Şekil 6. İkili sınıflandırma karışıklık matrisi.

Gelir sınıflandırma sorunu için yukarıda bahsedilen, size yardımcı çeşitli değerlendirme sorularını kullanılan sınıflandırıcı performansını anlama sormak istersiniz. Çok doğal bir soru da şudur: ' Model kazanmaya olması için tahmin edilen kişilere dışında > 50 K (TP + FP) ne kadar doğru şekilde sınıflandırıldığını (TP)?' Bu soruyu bakarak yanıtlanması gereken **duyarlık** doğru olarak sınıflandırıldığını pozitif sonuç oranı olan modeli: TP/(TP+FP). Başka bir yaygın soru ise "dışında tüm yüksek gelir çalışanlarla kazanmaya > 50 k (TP + FN) kaç sınıflandırıcı doğru sınıflandırma (TP)". Bu, gerçekten **geri çağırma**, ya da gerçek pozitif sonuç oranı: Sınıflandırıcı tp/(tp+fn). Duyarlık ve geri çağırma arasında belirgin bir dengeyi olduğunu fark edebilirsiniz. Örneğin, oldukça dengeli bir veri kümesi göz önünde bulundurulduğunda, çoğunlukla olumlu örnekleri tahmin eden bir sınıflandırıcı haritamın yüksek geri çağırma işlemi, ancak çok düşük bir duyarlılık çoğu negatif gibi çok sayıda hatalı pozitif sonuç elde edilen misclassified. Bu iki ölçüm nasıl farklılık, bir çizim görmek için tıklayabilirsiniz **DUYARLIK/geri ÇAĞIRMA** eğri değerlendirme sonuç çıkış sayfasının (sol üst bölümü Şekil 7).

![İkili sınıflandırma değerlendirme sonuçları](./media/evaluate-model-performance/7.png)

Şekil 7. İkili sınıflandırma değerlendirme sonuçları.

Başka bir sık kullanılan ölçümü ilgili **F1 puanı**, duyarlık hem geri çağırma dikkate alır. Bu 2 ölçüm harmonik olduğundan ve bu şekilde hesaplanır: F1 = 2 (x duyarlık geri çekme) / (duyarlık + geri çağırma). F1 puanı tek bir sayı olarak değerlendirme özetlemek için iyi bir yoludur, ancak bu her zaman hem duyarlık ve birlikte daha iyi bir sınıflandırıcı nasıl davranacağını anlamak için geri çağırma aramak için iyi bir uygulamadır.

Ayrıca, bir doğru pozitif sonuç oranına hatalı pozitif sonuç oranı karşılaştırması inceleyebilirsiniz **alıcı çalıştırma özellikleri (ROC)** eğri ve karşılık gelen **alanı altında eğri (AUC)** değeri. Daha yakından bu eğri için sol üst köşesinde, daha iyi bir sınıflandırıcı ait performans (Bu en üst düzeye gerçek pozitif sonuç oranına hatalı pozitif sonuç oranı en aza indirerek). Rasgele tahmin yakın olan tahminlerde eğilimindedir sınıflandırıcılar sonuçtan çizimin alt kenar köşegeni yakın olan eğrileri.

### <a name="using-cross-validation"></a>Doğrulama kullanma
Regresyon örnekte olduğu gibi çapraz doğrulama, tekrar tekrar eğitme, Puanlama ve farklı veri kümelerine otomatik olarak değerlendirmek için gerçekleştirebiliriz. Benzer şekilde, kullanabiliriz [çapraz doğrulama, Model] [ cross-validate-model] modülü deneyimsiz Lojistik regresyon modeli ve bir veri kümesi. Etiket sütunu ayarlanmalıdır *gelir* içinde [çapraz doğrulama, Model] [ cross-validate-model] modülün özellikleri. Denemeyi çalıştırma ve sağ tarafta tıklayarak bağlantı noktası çıktı sonra [çapraz doğrulama, Model] [ cross-validate-model] modülü, ikili sınıflandırma ölçüm değerleri her Katlama ayrıca ortalamaya görebiliriz ve Her standart sapması. 

![Çapraz-doğrulama ikili sınıflandırma modeli](./media/evaluate-model-performance/8.png)

Şekil 8. Çapraz-ikili sınıflandırma modelinde doğrulanıyor.

![Çapraz doğrulama sonuçlarını bir ikili dosya sınıflandırıcı](./media/evaluate-model-performance/9.png)

Şekil 9. Bir ikili dosya sınıflandırıcı çapraz doğrulama sonuçları.

## <a name="evaluating-a-multiclass-classification-model"></a>Bir çok sınıflı sınıflandırma modeli değerlendirme
Bu deneyde popüler kullanacağız [Iris](https://archive.ics.uci.edu/ml/datasets/Iris "Iris") 3 farklı türleri (sınıflar) Iris tesis örneklerini içeren bir veri kümesi. 4 özellik değerleri (sepal uzunluğu/genişlik ve petal uzunluğu/genişlik) her örneği için vardır. Önceki denemeleri, eğitim ve aynı veri kümeleri aracılığıyla modelleri test. Burada, kullanacağız [verileri bölme] [ split] 2 veri kümelerine oluşturmak, ilk gününde eğitmek ve puanlamak ve ikinci değerlendirmek için modülü. Iris veri kümesini üzerinde genel kullanıma açık [UCI Machine Learning depo](https://archive.ics.uci.edu/ml/index.html)ve kullanarak indirilebilir bir [verileri içeri aktarma] [ import-data] modülü.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıdaki modüller, Azure Machine Learning Studio çalışma alanınıza ekleyin:

* [Verileri İçeri Aktar][import-data]
* [Veya çoklu sınıflar karar ormanı][multiclass-decision-forest]
* [Verileri bölme][split]
* [Modeli eğitme][train-model]
* [Model Puanlama][score-model]
* [Modeli değerlendirme][evaluate-model]

Şekil 10'da aşağıda gösterildiği gibi bağlantı noktalarına bağlayın.

Etiket sütun dizini ayarlamak [modeli eğitme] [ train-model] 5 modülü. Veri kümesi yok üst bilgi satırı içeriyor, ancak sınıf etiketler beşinci sütunda olduğunu biliyoruz.

Tıklayın [verileri içeri aktarma] [ import-data] modülü ve kümesi *veri kaynağı* özelliğini *Web URL'si HTTP üzerinden*ve *URL'si* için http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Örnekleri eğitimi için kullanılacak kesir ayarlamak [verileri bölme] [ split] Modülü (0,7 örneğin).

![Bir çok sınıflı sınıflandırıcı değerlendiriliyor](./media/evaluate-model-performance/10.png)

Şekil 10. Bir çok sınıflı sınıflandırıcı değerlendiriliyor

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme sonuçlarını İnceleme
Denemeyi çalıştırın ve çıkış bağlantı noktasına tıklayın [Evaluate Model][evaluate-model]. Değerlendirme sonuçları, bu durumda bir karışıklık matrisi biçiminde gösterilir. Matris gerçek 3 tüm sınıflar için tahmin edilen örnekler ve gösterir.

![Sınıflı sınıflandırma değerlendirme sonuçları](./media/evaluate-model-performance/11.png)

Şekil 11. Sınıflı sınıflandırma değerlendirme sonuçları.

### <a name="using-cross-validation"></a>Doğrulama kullanma
Daha önce bahsedildiği gibi yinelenen eğitim, Puanlama ve değerlendirmeleri kullanarak otomatik olarak gerçekleştirebilir [çapraz doğrulama, Model] [ cross-validate-model] modülü. Bir veri kümesi, bir deneyimsiz modeli gerekir ve bir [çapraz doğrulama, Model] [ cross-validate-model] Modülü (aşağıdaki şekilde bakın). Etiket sütunu yeniden ayarlamanızı [çapraz doğrulama, Model] [ cross-validate-model] Modülü (5 Bu durumda sütun dizini). Denemeyi çalıştırma ve sağ tıklayarak bağlantı noktası çıktı sonra [çapraz doğrulama, Model][cross-validate-model], ortalama ve standart sapma yanı sıra her Katlama ölçüm değerlerini inceleyebilirsiniz. Burada görüntülenen ölçümlerin ikili sınıflandırma durumda açıklanan ayarlara benzerdir. Ancak, hiçbir genel pozitif veya negatif bir sınıf olmadığından çok sınıflı Sınıflandırma, bilgi işlem gerçek pozitif sonuç negatif ve hatalı pozitif sonuç negatif bir sınıf başına temelinde sayılarak yapıldığını unutmayın. Örneğin, duyarlık veya geri çağırma 'Iris-setosa' sınıfının hesaplanırken, bu sınıf pozitif ve negatif olarak diğer tüm olduğu varsayılır.

![Çapraz-doğrularken bir çok sınıflı sınıflandırma modeli](./media/evaluate-model-performance/12.png)

Şekil 12. Çapraz-bir çok sınıflı sınıflandırma modeli doğrulanıyor.

![Çapraz doğrulama sonuçlarını bir çok sınıflı sınıflandırma modeli](./media/evaluate-model-performance/13.png)

Şekil 13. Çapraz doğrulama sonuçlarını bir çok sınıflı sınıflandırma modeli.

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

