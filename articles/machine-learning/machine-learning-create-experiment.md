<properties
    pageTitle="Machine Learning Studio'da basit bir deneme | Microsoft Azure"
    description="Bu makine öğrenimi öğreticisi kolay bir veri bilimi deneyinde size kılavuzluk etmektedir. Regresyon algoritması kullanarak bir arabanın fiyatını tahmin edeceğiz."
    keywords="deneme,doğrusal regresyon,makine öğrenimi algoritmaları,makine öğrenimi öğreticisi,tahmine dayalı modelleme teknikleri,veri bilimi deneyi"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# Makine öğrenimi öğreticisi: Azure Machine Learning Studio'da ilk veri bilimi denemenizi oluşturma

Bu makine öğrenimi öğreticisi kolay bir veri bilimi deneyinde size kılavuzluk etmektedir. Marka ve teknik belirtimler gibi farklı değişkenleri esas alarak otomobil fiyatını tahmin eden bir doğrusal regresyon modeli oluşturacağız. Bunu yapmak için, tahmine dayalı basit bir analiz denemesini geliştirmek ve yinelemek amacıyla Azure Machine Learning Studio'yu kullanacağız.

*Tahmine dayalı analiz* gelecekteki sonuçları tahmin etmek üzere mevcut verileri kullanan bir veri bilimi türüdür. Tahmine dayalı analizin çok basit bir örneği için Yeni Başlayanlar için Veri Bilimi video 4’ü izleyin: [Basit bir model ile yanıtı tahmin etme](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (çalışma zamanı: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Machine Learning Studio yardımı nasıl çalışır?

Machine Learning Studio tahmine dayalı modelleme teknikleriyle önceden programlanmış sürükle ve bırak modüllerini kullanarak bir deneme oluşturmayı kolaylaştırır. Denemenizi çalıştırmak ve bir yanıtı tahmin etmek için Machine Learning Studio’yu kullanarak *model oluşturun*, *modeli test edin* ve *modeli puanlayıp test edin*.

Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net)'e girin. Machine Learning Studio’da daha önce oturum açtıysanız **Burada oturum aç**’a tıklayın. Veya **Kaydol**’a tıklayıp ücretsiz ve ücretli seçenekler arasından seçim yapın.

Machine Learning Studio hakkında daha fazla genel bilgi için bkz. [Machine Learning Studio nedir?](machine-learning-what-is-ml-studio.md)

## Bir deneme oluşturmanın beş adımı

Bu makine öğrenimi öğreticisinde, modelinizi oluşturmak, eğitmek ve puanlamak amacıyla Machine Learning Studio'da bir deneme oluşturmak için beş temel adımı izleyeceksiniz.

- Bir model oluşturma
    - [1. Adım: Verileri alma]
    - [2. Adım: Verileri ön işleme]
    - [3. Adım: Özellikleri tanımlama]
- Modeli eğitme
    - [4. Adım: Bir öğrenme algoritması seçme ve uygulama]
- Modeli puanlama ve test etme
    - [5. Adım: Yeni otomobil fiyatlarını tahmin etme]

[1. Adım: Verileri alma]: #step-1-get-data
[2. Adım: Verileri ön işleme]: #step-2-preprocess-data
[3. Adım: Özellikleri tanımlama]: #step-3-define-features
[4. Adım: Bir öğrenme algoritması seçme ve uygulama]: #step-4-choose-and-apply-a-learning-algorithm
[5. Adım: Yeni otomobil fiyatlarını tahmin etme]: #step-5-predict-new-automobile-prices


## 1. Adım: Verileri alma

Machine Learning Studio'da seçebileceğiniz birçok örnek veri kümesi bulunur ve birçok kaynaktan verileri içeri aktarabilirsiniz. Bu örnekte, **Otomobil fiyat verileri (Ham)** adlı dahil edilmiş örnek veri kümesini kullanacağız.
Bu veri kümesi; marka, model, teknik belirtimler ve fiyat gibi bilgiler dahil olmak üzere birçok ayrı otomobil için giriş içerir.

1. Machine Learning Studio penceresinin alt kısmındaki **+NEW (+YENİ)** seçeneğine tıklayarak yeni bir deneme başlatın, **EXPERIMENT (DENEME)** seçeneğini belirletin ve ardından **Blank Experiment (Boş Deneme)** öğesini seçin. Tuvalin en üst kısmında varsayılan deneme adını seçin ve **Otomobil fiyat tahmini** gibi daha anlamlı bir şekilde yeniden adlandırın.

2. Deneme tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. **Otomobil fiyat verileri (Ham)** etiketli veri kümesini bulmak için bu paletin en üst kısmındaki Arama kutusuna **otomobil** yazın.

    ![Palet arama][screen1a]

3. Veri kümesini deneme tuvaline sürükleyin.

    ![Veri kümesi][screen1]

Bu verinin nasıl göründüğünü görmek için, otomobil veri kümesinin alt kısmındaki çıkış bağlantı noktasına tıklayın ve ardından **Görselleştir**'i seçin.

![Modül çıkışı bağlantı noktası][screen1c]

Veri kümesindeki değişkenler sütun olarak görünür ve her bir otomobil örneği de satır olarak görünür. En sağdaki sütun ("fiyat" başlıklı 26. sütun) tahmin etmeyi deneyeceğimiz hedef değişkendir.

![Veri kümesi görselleştirmesi][screen1b]

Sağ üst köşedeki "**x**" işaretine tıklayarak görselleştirme penceresini kapatın.

## 2. Adım: Verileri ön işleme

Genellikle bir veri kümesi analiz edilmeden önce biraz ön işleme gerekir. Çeşitli satırların sütunlarında bulunan eksik değerleri fark etmiş olabilirsiniz. Modelin verileri doğru şekilde analiz edebilmesi için bu eksik değerlerin temizlenmesi gerekir. Örneğimizde eksik değerleri olan satırları kaldıracağız. Ayrıca, **normalleştirilmiş kayıplar** sütununun büyük kısmı eksik değerlere sahiptir; bu nedenle bu sütunu modelin tamamen dışında bırakacağız.

> [AZURE.TIP] Giriş verilerinden eksik değerleri temizleme, çoğu modülü kullanmanın bir önkoşuludur.

Öncelikle **normalleştirilmiş kayıplar** sütununu kaldıracağız ve ardından eksik verileri olan satırları kaldıracağız.

1. [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülünü bulmak için modül paletinin en üst kısmındaki Arama kutusuna **sütun seçme** yazın, ardından bunu deneme tuvaline sürükleyin ve **Otomobil fiyat verileri (Ham)** veri kümesinin çıkış bağlantı noktasına bunu bağlayın. Bu modül, modele hangi veri sütunlarını dahil etmek veya dışarıda bırakmak istediğimizi seçmemizi sağlar.

2. [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülünü seçin ve **Properties (Özellikler)** bölmesinde **Launch column selector (Sütun seçiciyi başlat)** seçeneğine tıklayın.

    - Sol tarafta **Kurallar ile**’ye tıklayın
    - **Şununla Başla** altında **Tüm sütunlar**’a tıklayın. Bu, [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülünü tüm sütunlardan geçmeye yönlendirir (dışarıda bırakacaklarımız hariç).
    - Açılan menülerden **Hariç Tut** ve **sütun adlarını** seçerek metin kutusuna tıklayın. Sütun listesi görüntülenir. **Normalleştirilmiş kayıplar**'ı seçin; böylece metin kutusuna eklenir.
    - Sütun seçiciyi kapatmak için onay işareti (Tamam) düğmesine tıklayın.

    ![Sütun seçme][screen3]

    **Select Columns in Dataset (Veri Kümesinde Sütun Seçme)** için özellikler bölmesi, **normalleştirilmiş kayıplar** dışındaki tüm veri kümelerindeki tüm sütunlardan geçeceğini belirtir.

    ![Veri Kümesinde Sütun Seçme özellikleri][screen4]

    > [AZURE.TIP] Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. Bu durumda, [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülüne çift tıklayın ve "Normalleştirilmiş kayıpları dışarıda bırak" yorumunu yazın.

3. [Clean Missing Data (Eksik Verileri Temizleme)][clean-missing-data] modülünü deneme tuvaline sürükleyin ve bunu [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülüne bağlayın. **Özellikler** bölmesinde eksik değerler içeren satırları kaldırarak verileri temizlemek için **Temizleme modu**'nun altında **Tüm satırı kaldır**'ı seçin. Modüle çift tıklayın ve "Eksik değerli satırları kaldır" yorumunu yazın.

    ![Clean Missing Data (Eksik Verileri Temizleme) özellikleri][screen4a]

4. Deneme tuvalinin altında bulunan **RUN (ÇALIŞTIR)** düğmesine tıklayarak denemeyi çalıştırın.

Deneme bittiğinde, tüm modüllerin başarıyla tamamlandığını göstermek için yeşil bir onay işareti bulunur. Sağ üst köşede **Çalıştırma tamamlandı** durumunun olduğuna da dikkat edin.

![İlk denemenin çalıştırılması][screen5]

Denemede bu noktaya kadar yalnızca verileri temizledik. Temizlenen veri kümesini görüntülemek istiyorsanız [Clean Missing Data (Eksik Verileri Temizleme)][clean-missing-data] modülünün sol çıkış bağlantı noktasına ("Temizlenen veri kümeleri") tıklayın ve **Görselleştir**'i seçin. **Normalleştirilmiş kayıplar** sütununun artık dahil olmadığına ve eksik değerlerin yok olduğuna dikkat edin.

Artık veriler temizlendiğine göre, tahmine dayalı modelde hangi özellikleri kullanacağımızı belirtmeye hazırız.

## 3. Adım: Özellikleri tanımlama

Machine learning'de *özellikler*, ilgilendiğiniz bir şeyin tek tek ölçülebilir özellikleridir. Veri kümemizde her bir satır bir otomobili temsil eder ve her bir sütun da bu otomobilin bir özelliğidir.

Tahmine dayalı bir model oluşturmaya yönelik iyi bir özellikler kümesi bulmak için, deneme ve çözmek istediğiniz sorun hakkında bilgi gerekir. Bazı özellikler, hedefi tahmin etmede diğerlerinden daha uygundur. Ayrıca, bazı özelliklerin diğer özelliklerle güçlü bir bağıntısı vardır (şehir-mpg ile otoban-mpg karşılaştırması gibi); bu nedenle modele çok fazla yeni bilgi eklemezler ve bunlar kaldırılabilir.

Veri kümemizdeki bir alt özellikler kümesini kullanan bir model oluşturalım. Geri dönüp farklı özellikler seçebilir, denemeyi tekrar çalıştırabilir ve daha iyi sonuçlar elde edip etmeyeceğinizi görebilirsiniz. Ancak başlarken aşağıdaki özellikleri deneyelim:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Başka bir [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülünü deneme tuvaline sürükleyin ve bunu [Clean Missing Data (Eksik Verileri Temizleme)][clean-missing-data] modülünün sol çıkış bağlantı noktasına bağlayın. Modüle çift tıklayın ve "Tahmin için özellik seç" yazın.

2. **Properties (Özellikler)** bölmesindeki **Launch column selector (Sütun seçiciyi başlat)** seçeneğine tıklayın.

3. **Kurallar ile**’ye tıklayın.

4. **Şununla Başla** altında **Sütun yok**’a tıklayın, ardından filtre satırında **Dahil et**’i ve **sütun adlarını** seçin. Sütun adları listemizi girin. Bu, modülü yalnızca belirttiğimiz sütunlardan geçmeye yönlendirir.

    > [AZURE.TIP] Denemeyi çalıştırarak, verilerimizin sütun tanımlarının [Eksik Verileri Temizleme][clean-missing-data] modülü aracılığıyla veri kümesinden geçtiğinden emin olduk. Bu durum bağlandığınız diğer modüllerin de veri kümesinden bilgiler içereceği anlamına gelir.

5. Onay işareti (Tamam) düğmesine tıklayın.

![Sütun seçme][screen6]

Bu, sonraki adımlarda öğrenme algoritmasında kullanılacak veri kümesini oluşturur. Daha sonra geri dönüp farklı özellikler seçerek yeniden deneyebilirsiniz.

## 4. Adım: Bir öğrenme algoritması seçme ve uygulama

Artık veriler hazır olduğuna göre, tahmine dayalı bir model oluşturmak için eğitim ve test etme gerekir. Modeli eğitmek ve sonra fiyatları tahmin etmeye ne kadar yaklaştığını görmek üzere modeli test etmek için verilerimizi kullanacağız. Şu an için bir modeli neden eğitmemiz ve sonra test etmemiz gerektiğini düşünmeyin.

*Sınıflandırma* ve *regresyon*, denetimli iki makine öğrenimi tekniğidir. Sınıflandırma; renk gibi (kırmızı, mavi veya yeşil) tanımlanmış bir kategori kümesinden yanıt tahmin eder. Bir sayıyı tahmin etmek için regresyon kullanılır.

Bir sayı olan fiyatı tahmin etmek istediğimizden bir regresyon modeli kullanırız. Bu örnekte basit bir *doğrusal regresyon* modelini eğiteceğiz ve sonraki adımda bunu test edeceğiz.

1. Verilerimizi eğitim ve test etme kümelerine ayırarak bunları hem eğitim hem de test için kullanırız. [Split Data (Verileri Ayırma)][split] modülünü seçip deneme tuvaline sürükleyin ve bunu son [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülünün çıkışına bağlayın. **İlk çıkış veri kümesinde bulunan satırlar için kesir değerini** 0,75 olarak ayarlayın. Bu şekilde, modeli eğitmek için verilerin yüzde 75'ini kullanıp test etmek için yüzde 25'ini ayıracağız.

    > [AZURE.TIP] **Rastgele doldurma** parametresini değiştirerek eğitim ve test etme için farklı rastgele örnekler oluşturabilirsiniz. Bu parametre, sözde rastgele sayı üreticisinin doldurulmasını denetler.

2. Denemeyi çalıştırın. Böylece [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] ve [Split Data (Verileri Ayırma)][split] modüllerinin sütun tanımlarını sonra ekleyeceğimiz modüllere geçirmesine olanak sağlanır.  

3. Öğrenme algoritmasını seçmek için, tuvalin solundaki modül paletindeki **Machine Learning** kategorisini genişletin ve ardından **Modeli Başlat**'ı genişletin. Böylece makine öğrenimi algoritmalarını başlatmak için kullanılabilecek çeşitli modül kategorileri görüntülenir.

    Bu deneme için, **Regresyon** kategorisinin altında [Linear Regression (Doğrusal Regresyon)][linear-regression] modülünü (palet Arama kutusuna "doğrusal regresyon" yazarak da modülü bulabilirsiniz) seçin ve bunu deneme tuvaline sürükleyin.

4. [Train Model (Model Eğitme)][train-model] modülünü bulup deneme tuvaline sürükleyin. Sol giriş bağlantı noktasını [Linear Regression (Doğrusal Regresyon)][linear-regression] modülünün çıkışına bağlayın. Sağ giriş bağlantı noktasını [Split Data (Verileri Ayırma)][split] modülünün eğitim verileri çıkışına (sol bağlantı noktası) bağlayın.

5. [Train Model (Model Eğitme)][train-model] modülünü seçin, **Properties (Özellikler)** bölmesinde **Launch column selector (Sütun seçiciyi başlat)** seçeneğine tıklayın ve ardından **fiyat** sütununu seçin. Bu, modelimizin tahmin edeceği değerdir.

    !["Fiyat" sütununu seçme][screen7]

6. Denemeyi çalıştırın.

Sonuç, tahminde bulunmak amacıyla yeni örnekleri puanlamak için kullanılabilecek eğitilmiş bir regresyon modelidir.

![Makine öğrenme algoritmasını uygulama][screen8]

## 5. Adım: Yeni otomobil fiyatlarını tahmin etme

Verilerimizin yüzde 75'ini kullanarak modeli eğittiğimize göre, modelimizin ne kadar iyi işlediğini görmek için verilerimizin diğer yüzde 25'ini puanlama amacıyla kullanabiliriz.

1. [Score Model (Model Puanlama)][score-model] modülünü bulup deneme tuvaline sürükleyin ve sol giriş bağlantı noktasını [Train Model (Model Eğitme)][train-model] modülünün çıkışına bağlayın. Sağ giriş bağlantı noktasını [Split Data (Verileri Ayırma)][split] modülünün test etme verileri çıkışına (sağ bağlantı noktası) bağlayın.  

    ![Score Model (Model Puanlama) modülü][screen8a]

2. Denemeyi çalıştırmak ve [Score Model (Model Puanlama)][score-model] modülünden çıkışı görüntülemek için, çıkış bağlantı noktasına tıklayın ve ardından **Görselleştir**'i seçin. Çıkış, fiyat için tahmin edilen değerleri ve test verileri için bilinen değerleri gösterir.  

3. Son olarak, sonuçların kalitesini test etmek için [Evaluate Model (Model Değerlendirme)][evaluate-model] modülünü seçip deneme tuvaline sürükleyin ve sol giriş bağlantı noktasını [Score Model (Model Puanlama)][score-model] modülünün çıkışına bağlayın. ([Evaluate Model (Model Değerlendirme)][evaluate-model] modülü iki modeli karşılaştırmak için kullanılabildiğinden, iki giriş bağlantı noktası bulunur.)

4. Denemeyi çalıştırın.

[Evaluate Model (Model Değerlendirme)][evaluate-model] modülünden çıkışı görüntülemek için, çıkış bağlantı noktasına tıklayın ve ardından **Görselleştir**'i seçin. Modelimiz için aşağıdaki istatistikler gösterilir:

- **Mean Absolute Error (Ortalama Mutlak Hata)** (MAE): Mutlak hataların ortalaması (*hata*, tahmin edilen değer ile gerçek değer arasındaki farktır).
- **Root Mean Squared Error (Kök Ortalama Karesi Alınmış Hata)** (RMSE): Test veri kümesinde yapılan tahminlerin karesi alınmış hata ortalamasının kare kökü.
- **Relative Absolute Error (Göreli Mutlak Hata)**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak hataların mutlak farka göreli ortalaması.
- **Relative Squared Error (Göreli Karesi Alınmış Hata)**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki karesi alınmış hataların karesi alınmış farka göreli ortalaması.
- **Coefficient of Determination (Determinasyon Katsayısı)**: **R karesi alınmış değer** olarak da bilinen ve modelin verilere ne kadar iyi uyumlu olduğunu gösteren istatistik ölçümleridir.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük olan bir değer, tahminlerin gerçek değerlerle daha yakından eşleştiğini gösterir. **Coefficient of Determination (Determinasyon Katsayısı)** değeri bire (1.0) ne kadar yakınsa tahminler o kadar iyi olur.

![Değerlendirme sonuçları][screen9]

Son deneme şu şekilde görünecektir:

![Machine learning öğreticisi: Tahmine dayalı modelleme teknikleri kullanan doğrusal regresyon denemesini tamamlayın.][screen10]

## Sonraki adımlar

Artık ilk makine öğrenimi öğreticinizi tamamladığınıza ve denemenizi kurduğunuza göre, modeli iyileştirmeyi denemek için yineleyebilirsiniz. Örneğin, tahmininizde kullanmak istediğiniz özellikleri değiştirebilirsiniz. Veya [Linear Regression (Doğrusal Regresyon)][linear-regression] algoritmasının özelliklerini değiştirebilir veya tamamen farklı bir algoritma deneyebilirsiniz. Ayrıca, denemenize tek bir seferde birden çok makine öğrenimi algoritması ekleyebilir ve [Evaluate Model (Model Değerlendirme)][evaluate-model] modülünü kullanarak ikisini karşılaştırabilirsiniz.

> [AZURE.TIP] Denemenizin herhangi bir yinelemesini kopyalamak için deneme tuvalinin altındaki **SAVE AS (FARKLI KAYDET)** düğmesini kullanın. Tuvalin altındaki **VIEW RUN HISTORY (ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE)** seçeneğine tıklayarak denemenizin tüm yinelemelerini görebilirsiniz. Daha fazla ayrıntı için bkz. [Azure Machine Learning Studio'da deneme yinelemelerini yönetme][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

Modelinizden memnun kaldığınızda, yeni verileri kullanarak otomobil fiyatlarını tahmin etmek için kullanılacak bir web hizmeti olarak dağıtabilirsiniz. Daha fazla ayrıntı için bkz. [Bir Azure Machine Learning web hizmetini dağıtma][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Modeli oluşturma, eğitme, puanlama ve dağıtma için tahmine dayalı modelleme tekniklerinin daha kapsamlı ve ayrıntılı kılavuzu için bkz. [Azure Machine Learning kullanarak tahmine dayalı bir çözüm geliştirme][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/



<!--HONumber=sep16_HO2-->


