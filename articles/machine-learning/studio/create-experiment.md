---
title: 'Hızlı Başlangıç: Bir veri bilimi deneme oluşturma'
titleSuffix: Azure Machine Learning Studio
description: Bu makine öğrenimi hızlı bir kolayca veri bilimi deneyinde size kılavuzluk eder. Regresyon algoritması kullanarak bir arabanın fiyatını tahmin edeceğiz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seodec18
ms.date: 02/06/2019
ms.openlocfilehash: 0819c232412e1619f82a25476a8318d26c8087da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60753447"
---
# <a name="quickstart-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Hızlı Başlangıç: Azure Machine Learning Studio'da ilk veri bilimi denemenizi oluşturma

Bu hızlı başlangıçta, bir machine learning denemesi oluşturma [Azure Machine Learning Studio](what-is-ml-studio.md) marka ve teknik belirtimler gibi farklı değişkenleri dayalı bir araba fiyatını tahmin eder.

Machine learning, video serisi için yeni [yeni başlayanlar için veri bilimi](data-science-for-beginners-the-5-questions-data-science-answers.md) gündelik dil ve kavramlarla makine öğrenimine harika bir giriş niteliğindedir.

Bu hızlı başlangıçta, bir deneme için varsayılan iş akışı aşağıdaki gibidir:

1. **Bir model oluşturma**
    - [Veri alma]
    - [Verileri hazırlama]
    - [Özellikleri tanımlama]
1. **Modeli eğitme**
    - [Bir algoritması seçme ve uygulama]
1. **Modeli puanlama ve test etme**
    - [Yeni otomobil fiyatlarını tahmin etme]

[Veri alma]: #get-the-data
[Verileri hazırlama]: #prepare-the-data
[Özellikleri tanımlama]: #define-features
[Bir algoritması seçme ve uygulama]: #choose-and-apply-an-algorithm
[Yeni otomobil fiyatlarını tahmin etme]: #predict-new-automobile-prices

Studio hesabı yoksa, Git [Studio giriş sayfası](https://studio.azureml.net) seçip **buradan kaydolun** ücretsiz bir hesap oluşturmak için. Bu Hızlı Başlangıç için ihtiyacınız olan tüm özellikleri ücretsiz olacaktır.

## <a name="get-the-data"></a>Verileri alma

Machine learning'de gereken ilk şey verilerdir.
Studio ile kullanabileceğiniz birçok örnek veri kümesi vardır ve birçok kaynaktan verileri içeri aktarabilirsiniz. Bu örnekte, **Otomobil fiyat verileri (Ham)** adlı çalışma alanınıza dahil edilmiş örnek veri kümesini kullanacağız.
Bu veri kümesi; marka, model, teknik belirtimler ve fiyat gibi bilgiler dahil olmak üzere birçok ayrı otomobil için giriş içerir.

> [!TIP]
> Aşağıdaki denemenin çalışan bir kopyasını [Azure AI Gallery](https://gallery.azure.ai)’de bulabilirsiniz. **[İlk veri bilimi denemeniz - Otomobil fiyat tahmini](https://gallery.azure.ai/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**’ne gidin ve denemenin bir kopyasını Machine Learning Studio çalışma alanınıza indirmek için **Studio’da Aç**’a tıklayın.

Veri kümesini denemenize aşağıdaki gibi aktarabilirsiniz.

1. Tıklayarak yeni bir deneme oluşturma **+ yeni** Machine Learning Studio penceresinin alt kısmındaki. Seçin **deneme** >  **boş deneme**.

1. Denemenize tuvalin üzerinde görebileceğiniz bir varsayılan ad verilir. Bu adı seçerek anlamlı bir adla değiştirin, örneğin, **Otomobil fiyat tahmini**. Adın benzersiz olması gerekmez.

    ![Denemeyi yeniden adlandırma](./media/create-experiment/rename-experiment.png)

1. Deneme tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. **Otomobil fiyat verileri (Ham)** etiketli veri kümesini bulmak için bu paletin en üst kısmındaki Arama kutusuna **otomobil** yazın. Bu veri kümesini deneme tuvaline sürükleyin.

    ![Otomobil veri kümesini bulun ve deneme tuvaline sürükleyin](./media/create-experiment/type-automobile.png)

Ardından bu verileri otomobil veri kümesini alt kısmındaki çıkış bağlantı noktasına tıklayın gibi göründüğünü görmek **Görselleştir**.

![Çıkış bağlantı noktasına tıklayın ve "Görselleştir" i seçin](./media/create-experiment/select-visualize.png)

> [!TIP]
> Veri kümeleri ve modülleri küçük dairelerle gösterilen giriş ve çıkış bağlantı noktalarına sahiptir. Giriş bağlantı noktaları yukarıda, çıkış bağlantı noktaları aşağıdadır.
Denemeniz üzerinden veri akışı oluşturmak için bir modülün çıkış bağlantı noktasını bir diğerinin giriş bağlantı noktasına bağlayacaksınız.
Herhangi bir zamanda bir veri kümesi veya modülün çıkış bağlantı noktasına tıklayarak, veri akışında bu noktadaki verileri görebilirsiniz.

Bu veri kümesi her satır bir otomobili temsil eder ve her Otomobille ilişkili değişkenler sütun olarak görünür. Biz, belirli bir otomobil için değişkenleri kullanarak en sağdaki sütun (başlıklı sütun 26 "price") fiyatı tahmin edeceğiz.

![Veri görselleştirme penceresinde otomobil verilerini görüntüleme](./media/create-experiment/visualize-auto-data.png)

Sağ üst köşedeki "**x**" işaretine tıklayarak görselleştirme penceresini kapatın.

## <a name="prepare-the-data"></a>Verileri hazırlama

Genellikle bir veri kümesi analiz edilmeden önce biraz ön işleme gerekir. Çeşitli satırların sütunlarında bulunan eksik değerleri fark etmiş olabilirsiniz. Modelin verileri doğru şekilde analiz edebilmesi için bu eksik değerlerin temizlenmesi gerekir. Eksik değerleri olan satırları kaldıracağız. Ayrıca, **normalleştirilmiş kayıplar** sütununun büyük kısmı eksik değerlere sahiptir; bu nedenle bu sütunu modelin tamamen dışında bırakacağız.

> [!TIP]
> Giriş verilerinden eksik değerleri temizleme, çoğu modülü kullanmanın bir önkoşuludur.

İlk olarak kaldıran bir modül ekleriz **normalized-losses** sütun tamamen. Ardından eksik veriler içeren satırları kaldıran başka bir modülü ekleyeceğiz.

1. Tür **sütunları seçme** bulmak için modül paletinin üst kısmındaki arama kutusuna [kümesindeki sütunları seçme] [ select-columns] modülü. Ardından bunu deneme tuvaline sürükleyin. Bu modül, modele hangi veri sütunlarını dahil etmek veya dışarıda bırakmak istediğimizi seçmemizi sağlar.

1. Çıkış bağlantı noktasına bağlanmak **otomobil fiyat verileri (ham)** veri kümesindeki sütunları seçme, giriş bağlantı noktasına kümesi.

    !["Kümesindeki sütunları seçme" modülünü deneme tuvaline ekleyin ve bağlayın](./media/create-experiment/type-select-columns.png)

1. [Select Columns in Dataset (Veri Kümesinde Sütun Seçme)][select-columns] modülüne tıklayın ve **Özellikler** bölmesinde **Sütun seçiciyi başlat** seçeneğine tıklayın.

   - Sol tarafta **Kurallar ile**’ye tıklayın
   - **Şununla Başla** altında **Tüm sütunlar**’a tıklayın. Bu kurallar doğrudan [kümesindeki sütunları seçme] [ select-columns] (dışarıda bu sütunlar hariç) tüm sütunlardan geçmeye.
   - Açılan menülerden **Hariç Tut** ve **sütun adlarını** seçerek metin kutusuna tıklayın. Sütun listesi görüntülenir. **Normalleştirilmiş kayıplar**’ı seçin; böylece metin kutusuna eklenir.
   - (Sağ alt köşede üzerinde) sütun seçiciyi kapatmak için onay işareti (Tamam) düğmesine tıklayın.

     ![Sütun seçiciyi başlatın ve "normalized-losses" sütununu hariç tutun](./media/create-experiment/launch-column-selector.png)

     Artık **Select Columns in Dataset (Veri Kümesinde Sütun Seçme)** için özellikler bölmesi, **normalleştirilmiş kayıplar** dışındaki tüm veri kümelerindeki tüm sütunlardan geçeceğini belirtir.

     ![Özellikler bölmesinde "normalized-losses" sütununun hariç tutulduğu gösterilir](./media/create-experiment/showing-excluded-column.png)

     > [!TIP] 
     > Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. Bu durumda, [Veri Kümesindeki Sütunları Seçme][select-columns] modülüne çift tıklayın ve "Normalleştirilmiş kayıpları dışarıda bırak" yorumunu yazın.

     ![Açıklama eklemek için bir modüle çift tıklayın](./media/create-experiment/add-comment.png)

1. [Eksik Verileri Temizleme][clean-missing-data] modülünü deneme tuvaline sürükleyin ve bunu [Veri Kümesindeki Sütunları Seçme][select-columns] modülüne bağlayın. **Özellikler** bölmesinde, **Temizleme modu** altında **Tüm satırı kaldır**’ı seçin. Bu seçenekleri doğrudan [eksik verileri temizleme] [ clean-missing-data] değer içeren satırları kaldırarak verileri temizlemesi için. Modüle çift tıklayın ve "Eksik değerli satırları kaldır" yorumunu yazın.

    !["" Eksik verileri temizleme"modülüne için tüm satırı Kaldır" için temizleme modunu ayarlayın](./media/create-experiment/set-remove-entire-row.png)

1. Sayfanın en altında yer alan **ÇALIŞTIR**'a tıklayarak denemeyi çalıştırın.

    Deneme çalıştırma bittiğinde, tüm modüllerin başarıyla tamamlandığını göstermek için yeşil bir onay işareti bulunur. Sağ üst köşede **Çalıştırma tamamlandı** durumunun olduğuna da dikkat edin.

    ![Çalıştırdıktan sonra deneme aşağıdakine benzer görünmelidir](./media/create-experiment/early-experiment-run.png)

> [!TIP]
> Denemeyi neden şimdi çalıştırdık? Deneme çalıştırılarak, verilerimizin sütun tanımları [Veri Kümesindeki Sütunları Seçme][select-columns] modülü ve [Eksik Verileri Temizleme][clean-missing-data] modülü aracılığıyla veri kümesinden geçer. Bu, [Eksik Verileri Temizleme][clean-missing-data] öğesine bağladığımız modüllerin de aynı bilgilere sahip olacağı anlamına gelir.

Artık verileri temizleme sahibiz. Temizlenen veri kümesini görüntülemek istiyorsanız [Eksik Verileri Temizleme][clean-missing-data] modülünün sol çıkış bağlantı noktasına tıklayın ve **Görselleştir**'i seçin. **Normalleştirilmiş kayıplar** sütununun artık dahil olmadığına ve eksik değerlerin yok olduğuna dikkat edin.

Artık veriler temizlendiğine göre, tahmine dayalı modelde hangi özellikleri kullanacağımızı belirtmeye hazırız.

## <a name="define-features"></a>Özellikleri tanımlama

Machine learning'de *özellikler*, ilgilendiğiniz bir şeyin tek tek ölçülebilir özellikleridir. Veri kümemizde her bir satır bir otomobili temsil eder ve her bir sütun da bu otomobilin bir özelliğidir.

Tahmine dayalı bir model oluşturmaya yönelik iyi bir özellikler kümesi bulmak için, deneme ve çözmek istediğiniz sorun hakkında bilgi gerekir. Bazı özellikler, hedefi tahmin etmede diğerlerinden daha uygundur. Bazı özelliklerin diğer özelliklerle güçlü bir bağıntısı vardır ve kaldırılabilir. Örneğin, city-mpg ve highway-mpg yakından ilişkilidir, bu nedenle tahmini önemli ölçüde etkilemeden birini tutabilir ve diğerini kaldırabiliriz.

Veri kümemizdeki bir alt özellikler kümesini kullanan bir model oluşturalım. Daha sonra geri dönüp farklı özellikler seçebilir, denemeyi tekrar çalıştırabilir ve daha iyi sonuçlar elde edip etmeyeceğinizi görebilirsiniz. Ancak başlarken aşağıdaki özellikleri deneyelim:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price

1. Deneme tuvaline başka bir [Veri Kümesindeki Sütunları Seçme][select-columns] modülü sürükleyin. [Eksik Verileri Temizleme][clean-missing-data] modülünün sol çıkış bağlantı noktasını [Veri Kümesindeki Sütunları Seçme][select-columns] modülünün girişine bağlayın.

    !["Kümesindeki sütunları seçme" modülünü "Eksik verileri temizleme" modülüne bağlayın](./media/create-experiment/connect-clean-to-select.png)

1. Modüle çift tıklayın ve "Tahmin için özellik seç" yazın.

1. **Properties (Özellikler)** bölmesindeki **Launch column selector (Sütun seçiciyi başlat)** seçeneğine tıklayın.

1. **Kurallar ile**’ye tıklayın.

1. **Şununla Başla** altında **Sütun yok**’a tıklayın. Filtre satırında, **Dahil et** ve **sütun adları** öğelerini seçin ve metin kutusunda sütun adları listemizi seçin. Bu filtre, özellikle belirttiklerimiz dışında hiçbir sütunu (özelliği) geçirmemesi için modülü yönlendirir.

1. Onay işareti (Tamam) düğmesine tıklayın.

    ![Tahmine dahil edilecek sütunları (özellikleri) seçin](./media/create-experiment/select-columns-to-include.png)

Bu modül, yalnızca bir sonraki adımda kullanacağız öğrenim algoritmasına geçirmek istediğimiz özellikleri içeren filtrelenmiş bir veri kümesi oluşturur. Daha sonra geri dönüp farklı özellikler seçerek yeniden deneyebilirsiniz.

## <a name="choose-and-apply-an-algorithm"></a>Bir algoritması seçme ve uygulama

Artık veriler hazır olduğuna göre, tahmine dayalı bir model oluşturmak için eğitim ve test etme gerekir. Modeli eğitmek ve sonra fiyatları tahmin etmeye ne kadar yaklaştığını görmek üzere modeli test etmek için verilerimizi kullanacağız.
<!-- For now, don't worry about *why* we need to train and then test a model.-->

*Sınıflandırma* ve *regresyon*, denetimli makine öğrenimi algoritmasının iki türüdür. Sınıflandırma; renk gibi (kırmızı, mavi veya yeşil) tanımlanmış bir kategori kümesinden yanıt tahmin eder. Bir sayıyı tahmin etmek için regresyon kullanılır.

Bir sayı olan fiyatı tahmin etmek istediğimizden bir regresyon algoritması kullanırız. Bu örnekte, kullanacağız bir *doğrusal regresyon* modeli.


Modele fiyatı da içeren bir veri kümesi vererek modeli eğitiriz. Model verileri tarar ve otomobilin özellikleri ile fiyatı arasındaki bağlantıları arar. Daha sonra modeli test edeceğiz. Bildiğimiz otomobiller için bir özellik kümesi vererek modelin bilinen fiyatı tahmin etmeye ne kadar yaklaştığını göreceğiz.

Verilerimizi modeli eğitmek ve verileri ayrı eğitim ve test kümelerine ayırarak modeli test etmek için kullanırız.

1. [Verileri Bölme][split] modülünü seçip deneme tuvaline sürükleyin ve bunu son [Veri Kümesindeki Sütunları Seçme][select-columns] modülüne bağlayın.

1. Seçmek için [Verileri Bölme][split] modülüne tıklayın. **İlk çıkış veri kümesinde satır kesiri**’ni bulun (tuvalin sağ tarafında **Özellikler** bölmesinde) ve 0,75 olarak ayarlayın. Bu şekilde, modeli eğitmek için verilerin yüzde 75'ini kullanıp test etmek için yüzde 25'ini ayıracağız.

    !["Verileri bölme" modülünün kesirini 0,75 olarak ayarlayın.](./media/create-experiment/set-split-data-percentage.png)

    > [!TIP]
    > **Rastgele doldurma** parametresini değiştirerek eğitim ve test etme için farklı rastgele örnekler oluşturabilirsiniz. Bu parametre, sözde rastgele sayı üreticisinin doldurulmasını denetler.

1. Denemeyi çalıştırın. Deneme çalıştırıldığında [Veri Kümesindeki Sütunları Seçme][select-columns] ve [Verileri Bölme][split] modüllerinin sütun tanımlarını sonra ekleyeceğimiz modüllere geçirmesine olanak sağlanır.  

1. Öğrenme algoritmasını seçmek için, tuvalin solundaki modül paletindeki **Machine Learning** kategorisini genişletin ve ardından **Modeli Başlat**'ı genişletin. Böylece makine öğrenimi algoritmalarını başlatmak için kullanılabilecek çeşitli modül kategorileri görüntülenir. Bu deneme için, **Regresyon** kategorisinin altında [Doğrusal Regresyon][linear-regression] modülünü seçin ve bunu deneme tuvaline sürükleyin. (Modülü bulmak için palet Arama kutusuna “doğrusal regresyon” da yazabilirsiniz.)

1. [Modeli Eğitme][train-model] modülünü bulup deneme tuvaline sürükleyin. [Doğrusal Regresyon][linear-regression] modülünün çıkışını [Modeli Eğitme][train-model] modülünün sol girişine bağlayın ve [Verileri Bölme][split] modülünün eğitim verileri çıkışını (sol bağlantı noktası) [Modeli Eğitme][train-model] modülünün sağ girişine bağlayın.

    !["Modeli eğitme" modülünü "Çizgisel regresyon" ve "Verileri bölme" modüllerine bağlayın](./media/create-experiment/connect-train-model.png)

1. [Modeli Eğitme][train-model] modülüne tıklayın, **Özellikler** bölmesinde **Sütun seçiciyi başlat** seçeneğine tıklayın ve ardından **fiyat** sütununu seçin. **Fiyat** modelimizi tahmin etmek için gittiği değerdir.

    Sütun seçicide **fiyat** sütununu **Kullanılabilir sütunlar** listesinden **Seçili sütunlar** listesine taşıyarak seçersiniz.

    !["Modeli eğitme" modülü için fiyat sütununu seçin](./media/create-experiment/select-price-column.png)

1. Denemeyi çalıştırın.

Şimdi, fiyat tahmininde bulunmak amacıyla yeni otomobil verilerini puanlamak için kullanılabilecek eğitilmiş bir regresyon modeli oluşturduk.

![Çalıştırdıktan sonra deneme şimdi aşağıdakine benzer görünmelidir](./media/create-experiment/second-experiment-run.png)

## <a name="predict-new-automobile-prices"></a>Yeni otomobil fiyatlarını tahmin etme

Verilerimizin yüzde 75'ini kullanarak modeli eğittiğimize göre, modelimizin ne kadar iyi işlediğini görmek için verilerimizin diğer yüzde 25'ini puanlama amacıyla kullanabiliriz.

1. [Model Puanlama][score-model] modülünü bulup deneme tuvaline sürükleyin. [Modeli Eğitme][train-model] modülünün çıkışını [Model Puanlama][score-model] modülünün sol giriş bağlantı noktasına bağlayın. [Verileri Bölme][split] modülünün test verileri çıkışını (sağ bağlantı noktası) [Model Puanlama][score-model] modülünün sağ giriş bağlantı noktasına bağlayın.

    !["Model Puanlama" modülünü "Modeli eğitme" ve "Verileri bölme" modüllerine bağlayın](./media/create-experiment/connect-score-model.png)

1. Denemeyi çalıştırın ve çıkışı görüntülemek [Score Model] [ score-model] modülünün çıkış bağlantı noktasına tıklayarak [Score Model] [ score-model] seçin **Görselleştirme**. Çıkış, fiyat için tahmin edilen değerleri ve test verileri için bilinen değerleri gösterir.  

    !["Model Puanlama" modülü çıkışı](./media/create-experiment/score-model-output.png)

1. Son olarak, sonuç kalitesini test edeceğiz. [Model Değerlendirme][evaluate-model] modülünü seçip deneme tuvaline sürükleyin ve [Model Puanlama][score-model] modülünün çıkışını [Model Değerlendirme][evaluate-model] modülünün sol giriş bağlantı noktasına bağlayın. Son deneme şuna benzer şekilde görünecektir:

    ![Son deneme](./media/create-experiment/complete-linear-regression-experiment.png)

1. Denemeyi çalıştırın.

[Model Değerlendirme][evaluate-model] modülünden çıkışı görüntülemek için, çıkış bağlantı noktasına tıklayın ve ardından **Görselleştir**'i seçin.

![Deneme için değerlendirme sonuçları](./media/create-experiment/evaluation-results.png)

Modelimiz için aşağıdaki istatistikler gösterilir:

- **Mean Absolute Error** (MAE): Mutlak hataların ortalaması (bir *hata* tahmin edilen değer ile gerçek değer arasındaki farktır).
- **Kök ortalama karesi alınmış hata** (RMSE): Test veri kümesinde yapılan tahminlerin karesi Ortalama kare kökü.
- **Göreli mutlak hata**: Mutlak hataların gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak farka göreli ortalaması.
- **Göreli karesi alınmış hata**: Ortalama gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki karesi alınmış fark göreli karesi.
- **Katsayısı**: Olarak da bilinen **R karesi alınmış değer**, bir model verileri ne kadar iyi uyumlu olduğunu gösteren istatistik ölçümleridir budur.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük olan bir değer, tahminlerin gerçek değerlerle daha yakından eşleştiğini gösterir. **Coefficient of Determination (Determinasyon Katsayısı)** değeri bire (1.0) ne kadar yakınsa tahminler o kadar iyi olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir örnek veri kullanarak basit bir deneme oluşturulur. Oluşturma ve daha ayrıntılı bir model dağıtma işlemini keşfetmek için Tahmine dayalı bir çözüm öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Studio'da öngörülebilir bir çözüm geliştirin](tutorial-part1-credit-risk.md)

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
