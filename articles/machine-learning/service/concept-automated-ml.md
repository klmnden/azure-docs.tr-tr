---
title: Otomatik ML algoritması seçme ve ayarlama
titleSuffix: Azure Machine Learning service
description: Nasıl Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçme ve ondan size zaman sağlamak için ölçütleri ve parametreleri kullanarak kaydetmek için bir model oluşturmak öğrenin modeliniz için en iyi algoritmayı seçin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 1dcdbbf0a2a71fa38b6eacd6a8d179cdad979937
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059298"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

Otomatik machine Learning, otomatik ML da bilinir, makine öğrenimi modeli geliştirme zaman alıcı ve yinelemeli görevleri otomatikleştirme işlemidir. Veri uzmanları, analistler ve geliştiricilerin tüm model kalitesi dayanıklılık geçtikten çalışırken ile yüksek ölçek, verimliliğini ve üretkenliğini ML modelleri oluşturup izin verir.

Kaynak Kullanımı Yoğun, önemli bir etki alanı bilgisini ve üretmek ve modelleri onlarca karşılaştırmak için zaman gerektiren geleneksel makine öğrenme modeli geliştirme. Eğitme ve modeli sizin için belirttiğiniz hedef ölçüm kullanarak ayarlamak için Azure Machine Learning istediğinizde otomatik ML uygulayın. Hizmet ardından özellik seçimleri, burada her yinelemenin bir eğitim puan modeliyle üretir ile eşleştirilmiş ML algoritmaları aracılığıyla yinelenir. Daha yüksek puanı, daha iyi modeli "verilerinizi uygun" kabul edilir.

Otomatik machine learning ile üretime hazır ML model harika kolaylık ve verimlilik elde süresini hızlandırın.

## <a name="when-to-use-automated-ml"></a>ML otomatik ne zaman kullanılır?

Otomatik ML, makine öğrenimi modeli geliştirme sürecine ıot'yi herkesin kullanımına sunan ve herhangi bir sorun için bir uçtan uca makine öğrenimi işlem hattı tanımlamak için kullanıcılar, veri bilimi uzmanlıklarını ne olursa olsun güçlendirir.

Veri uzmanları ve analistleri sektörlerden geliştiriciler için otomatik ML kullanabilirsiniz:

+ Kapsamlı bir programlama bilgisi olmadan makine öğrenimi çözümleri uygulayın
+ Zamandan ve kaynaklardan tasarruf
+ Veri bilimi en iyi yararlanın
+ Çevik sorun giderme sağlayın

## <a name="how-automated-ml-works"></a>Nasıl otomatik ML çalışır

Kullanarak **Azure Machine Learning hizmeti**, tasarım ve otomatik ML eğitim denemelerinizi ile aşağıdaki adımları çalıştırın:

1. **ML tanımlamaya** çözülecek: Sınıflandırma, tahmin veya regresyon

1. **Kaynak ve etiketli bir eğitim veri biçimi belirtmek**: Numpy diziler veya Pandas dataframe

1. **Model eğitimi için işlem hedefini yapılandırın**, gibi [yerel bilgisayar, Azure Machine Learning hesaplar, uzaktan sanal makineleri veya Azure Databricks](how-to-set-up-training-targets.md).  Otomatik eğitim hakkında bilgi edinin [uzak kaynak](how-to-auto-train-remote.md).

1. **Yapılandırma parametreleri öğrenme otomatik makine** farklı modelleri, Hiper parametre ayarları üzerinden kaç adet yineleme ön işleme/özellik kazandırma sayesinde Gelişmiş ve en iyi modeli belirlerken bakmak için hangi ölçümleri belirler.  Otomatik eğitim denemesini ayarlarını yapılandırabilirsiniz [Azure portalında](how-to-create-portal-experiments.md) veya [SDK'sı ile](how-to-configure-auto-train.md).

1. **Çalıştırma eğitim gönderin.**

  ![Otomatik makine öğrenimi](./media/how-to-automated-ml/automl-concept-diagram2.png)

Eğitim sırasında Azure Machine Learning hizmeti farklı algoritmaları ve parametrelerle deneyin paralel işlem hatları bir dizi oluşturur. Deneme içinde tanımlanan çıkış ölçütlerini İsabetleri sonra durdurulur.

Ayrıca, çalışma sırasında toplanan ölçümleri içeren günlüğe kaydedilen çalışma bilgileri inceleyebilirsiniz. Python serileştirilmiş nesne eğitim çalıştırmanın (`.pkl` dosyası) modeli ve veri ön işleme içerir.

Model oluşturmanın otomatik karşın, şunları da yapabilirsiniz. [nasıl önemli veya ilgili özellikleri olan](how-to-configure-auto-train.md#explain) oluşturulmuş modelleri.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="time-series-forecasting"></a>Zaman serisi tahmini
Tahminlerini oluşturmak, gelir, Envanter, satış ve müşteri isteğe bağlı olup olmadığını, herhangi bir iş bütünleyici özelliğidir. ML kullanır otomatik olarak bir dizi birleşik teknikleri ve yaklaşımları bir yüksek kaliteli zaman serisi tahmin önerilir. Zaman serisi denemeleri otomatik ML çok değişkenli regresyon problemi kabul edilir. Zaman serisi değerleri "diğer adaylarının birlikte üzerine regressor için ek boyutlar olacak özetlenmiş". 

Klasik zaman serisi yöntemlerinden farklı olarak, bu yaklaşım, birden çok bağlam değişkenleri ve bunların birbirleriyle eğitim sırasında doğal olarak ekleme avantajı vardır. Gerçek tahmin uygulamalarında Çoklu faktörlerle tahmin etkileyebilir. Örneğin, satışları tahmin, geçmiş eğilimleri, döviz kuru ve fiyat tüm etkileşimleri ortaklaşa satış sonucu sürücü. Başka bir avantajı, tahmin için tüm son yeniliklerini regresyon modelleri, hemen geçerli olur.

Ne kadar geleceğe hava durumu tahminini (tahmin horizon) genişletmelidir temel tahmin belirtiminin bir parçasıdır. Gerekli parametre ayarı `max_horizon` deneme (forecaster tahmin kullanıma eğitim verilerinizi, örn. aylık, haftalık zaman aralığını temel. kaç birim nokta tanımlar. 

Otomatik ML veri kümesi ve tahmin horizons içindeki tüm öğeler için tek, ancak genellikle dahili olarak dallandırılmış bir model öğrenir. Daha fazla veri modelini parametreleri tahmin etmek bu nedenle kullanılabilir ve Genelleştirme görünmeyen dizilerine mümkün hale gelir. 

Eğitim verileri ayıklanan özellikleri, kritik bir rol oynar. Otomatik ML standart önceden işleme adımları gerçekleştirir ve dönemsel etkileri yakalamak ve Tahmine dayalı doğruluğu en üst düzeye çıkarmak için ek zaman serisi özellikleri (örneğin yıl, ay, haftanın günü vb.) oluşturur. 

Senaryonuz için uygun, ML aksamalar oluşturmak için otomatik yönlendirebilir, (`target_lags`) veya veri toplama çalışırken penceresi (`target_rolling_window_size`), hedefin gelen (`y_value`) son değerleri. 

## <a name="preprocessing"></a>Ön işleme

Deneme her otomatik makine öğrenimi, verilerinizi varsayılan yöntemleri kullanılarak ve isteğe bağlı olarak Gelişmiş ön işleme önceden işlenmiş.

### <a name="automatic-preprocessing-standard"></a>Otomatik (standart) ön işleme

Deneme her otomatik makine öğrenimi, verilerinizi otomatik olarak ölçeği veya iyi gerçekleştirilip algoritmaları yardımcı olmak için normalleştirilmiş.  Her model için model eğitim sırasında ölçeklendirme veya normalleştirme aşağıdaki tekniklerden birini uygulanır.

|Ölçeklendirme&nbsp;&&nbsp;normalleştirme| Açıklama |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Ortalama kaldırarak ve birim varyansı için ölçeklendirme özellikleri standart hale getirin  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her bir özellik bu sütunun minimum ve maksimum tarafından ölçeklendirerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Her bir özellik tarafından en yüksek mutlak değeri ölçeklendirin |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Bu Scaler tarafından quantile menzil özellikleri |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Doğrusal boyut düzeyi azaltma için daha düşük bir boyutlu alanı proje için verilerin tekil değer ayrıştırma kullanma |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Bu dönüştürücü doğrusal boyut düzeyi azaltma kesilmiş tekil değer ayrıştırma (SVD) yoluyla gerçekleştirir. PCA aykırı bu tahmin veri tekil değer ayrıştırma bilgi işlem önce Merkezi değil. Bu scipy.sparse matrisleri ile verimli bir şekilde çalışabilir anlamına gelir. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Böylece kendi norm (l1 veya l2) bir eşittir en az bir sıfır olmayan bileşeni ile her örnek (diğer bir deyişle, her satır veri matris) bağımsız olarak diğer örnekleri yeniden Ölçeklendirildi |

### <a name="advanced-preprocessing-optional-featurization"></a>Ön işleme Gelişmiş: isteğe bağlı özellik kazandırma sayesinde

Ayrıca ek Gelişmiş ön işleme ve özellik kazandırma sayesinde değerleri imputation, kodlama ve dönüştürmeler eksik gibi kullanılabilir. [Hangi özellik kazandırma sayesinde dahildir hakkında daha fazla bilgi](how-to-create-portal-experiments.md#preprocess). Bu ayarı etkinleştirin:

+ Azure portalı: Seçme **ön işleme** onay kutusu **Gelişmiş ayarlar** [adımlarıyla](how-to-create-portal-experiments.md).

+ Python SDK: Belirtme `"preprocess": True` için [ `AutoMLConfig` sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

## <a name="ensemble-models"></a>Topluluğu modelleri

Otomatik machine learning ile kullanarak topluluğu modelleri eğitebilirsiniz [Caruana topluluğu seçimi algoritması ile sıralanmış topluluğu başlatma](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Topluluğu öğrenme, machine learning sonuçları ve öngörülebilir performans sınıfını tek modelleri kullanarak çok sayıda model tarayan tarafından artırır. Topluluğu yineleme çalıştırmanızın son yineleme görünür.

## <a name="training-metric-output"></a>Eğitim ölçüm çıkış

Her çalıştırma yineleme için eğitim doğruluğu ölçümleri görüntülemek için birden çok yolu vardır.

* Jupyter pencere öğesi kullanın.
* Kullanım `get_metrics()` herhangi işlevi `Run` nesne.
* Azure portalında denemenizi ölçümleri görüntüleyin.

### <a name="classification-metrics"></a>Sınıflandırma ölçümleri

Aşağıdaki ölçümler için bir sınıflandırma görevi çalıştırma her yinelemede kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
AUC_Macro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Her sınıf için AUC aritmetik ortalamasını makrodur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "makrosu" =|
AUC_Micro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Mikro genel doğru pozitif sonuçlar ve hatalı pozitif sonuçları her sınıftaki birleştirilerek hesaplanır| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "micro" =|
AUC_Weighted  | AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Ağırlıklı ortalamasını puanın ağırlıklı true örnekleri her sınıfta sayısına göre her sınıf için olan| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Ortalama "ağırlıklı" =
accuracy|Doğruluk true etiketlerin tam olarak eşleşen tahmin edilen etiketleri yüzdesi ' dir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Her sınıf ortalama duyarlılık puanı aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "makrosu" =|
average_precision_score_micro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Mikro doğru pozitif sonuçlar ve hatalı pozitif sonuçları en her kesme tarayan tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "micro" =|
average_precision_score_weighted|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Ağırlıklı olan doğru örneklerinin her sınıfta tarafından ağırlıklı, her sınıf için ortalama kesinlik puanı aritmetik ortalaması|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "ağırlıklı" =|
balanced_accuracy|Dengeli doğruluğu her sınıf için geri çağırma aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
f1_score_macro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "makrosu" =|
f1_score_micro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Mikro toplam doğru pozitif sonuçlar, yanlış negatif ve hatalı pozitif sonuç sayımı tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "micro" =|
f1_score_weighted|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı sınıfı sıklığı Ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "ağırlıklı" =|
log_loss|(ÇOKTERİMLİ) Lojistik regresyon ve bunu uzantıları sinir ağları, negatif log-olasılığını olasılıklara sınıflandırıcının Öngörüler verilen true etiketlerin tanımlandığı gibi kullanılan kaybı işlev budur. True ile tek bir örnek için yt etiket {0,1} ve o yt yp olasılık tahmini = 1, günlük kaybı olan - P oturum (yt&#124;yp) =-(yt log(yp) + (1 - yt) günlük (1 - yp))|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|Normalleştirilmiş makrosu geri çağırma rastgele performans bir puan, 0 ve 1'in bir puan mükemmel performans sahip olacak şekilde normalleştirilmiş makrosu geri çağırma olur. Bu norm_macro_recall tarafından sağlanır: (recall_score_macro - R) = /(1-R), R recall_score_macro (yani, R = 0,5 ikili sınıflandırma için) ve C sınıflı sınıflandırma sorunları R=(1/C) rastgele tahminler elde etmek için beklenen değeri olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" = ve ardından (recall_score_macro - R) /(1-R), R recall_score_macro (yani, R = 0,5 ikili sınıflandırma için) ve C sınıflı sınıflandırma sorunları R=(1/C) rastgele tahminler elde etmek için beklenen değeri olduğu|
precision_score_macro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Her sınıf için duyarlık aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "makrosu" =|
precision_score_micro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Mikro toplam doğru pozitif sonuçlar ve hatalı pozitif sonuç sayımı tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "micro" =|
precision_score_weighted|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Ağırlıklı ortalamasını duyarlık true her sınıf örneği sayısını tarafından ağırlıklı, her sınıf için olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "ağırlıklı" =|
recall_score_macro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Aritmetik ortalamasıdır her sınıf için geri çağırma makrosu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
recall_score_micro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Toplam doğru pozitif sonuçlar, yanlış negatif sayılarak Micro genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "micro" =|
recall_score_weighted|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Ağırlıklı ortalamasını true her sınıf örneği sayısını tarafından ağırlıklı, her sınıf için geri çağırma olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "ağırlıklı" =|
weighted_accuracy|Her örnek için verilen ağırlık o örneğin true sınıfında true örnekleri oranını eşit olduğu doğruluğu ağırlıklı doğruluğu olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|Hedef eşit oranda her öğe için söz konusu sınıfın bir vektör sample_weight olduğu|

### <a name="regression-and-forecasting-metrics"></a>Regresyon ve tahmin ölçümleri

Aşağıdaki ölçümler, gerileme veya tahmin görev çalıştırma her yinelemede kaydedilir.

|Ölçüm|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|
explained_variance|Anlatıldığı farkı, belirli bir veri kümesi çeşitlemesi için matematiksel bir model hesapları oranı ' dir. Sadece, varyans hataları varyansını özgün verilerin içinde azaltma yüzdesi değil. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2 belirleme veya karesi alınmış hataların ortalaması çıkaran bir temel modele kıyasla yüzde azalma katsayısıdır. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|Spearman bağıntı iki veri kümesi arasındaki ilişkinin monotonicity nonparametric ölçüsüdür. Pearson bağıntı, iki veri kümesini normalde dağıtılmış Spearman bağıntı varsaymaz. Diğer korelasyon katsayısını gibi bunu -1 ve + ile hiçbir bağıntısı olduğunu belirtmek için 0 ile 1 arasında değişiklik gösterir. -1 veya + 1 bağıntılar tam bir monoton ilişki kapsıyor. Pozitif bağıntılar x olarak arttıkça, bu nedenle y yaptığı kapsıyor. Negatif bağıntılar olarak arttıkça, x y azaltır kapsıyor.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|Mutlak hata tahmin ile hedef arasındaki farkı mutlak değeri beklenen değeri anlama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|Veri aralığı tarafından ayrılmış mean Absolute Error normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Veri aralığına göre Böl|
median_absolute_error|Ortalama mutlak hata tahmin ile hedef arasındaki tüm mutlak farkları ortalamasıdır. Aykırı değerleri için bu kayıp sağlamdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|Veri aralığı tarafından ayrılmış ortalama mutlak hata normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Veri aralığına göre Böl|
root_mean_squared_error|Kök ortalama karesi alınmış hata hedef ve tahmin arasındaki karesi alınmış Beklenen fark kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama karesi alınmış hata kök ortalama karesi alınmış hata veri aralığını tarafından ayrılmış olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Veri aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama karesi alınmış günlük hatadır Logaritmik beklenen karesi alınmış hata kare kökünü|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|Veri aralığına göre bölünmüş kök ortalama karesi alınmış günlük hata normalleştirilmiş kök ortalama karesi alınmış günlük hatadır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Veri aralığına göre Böl|


## <a name="use-with-onnx-in-c-apps"></a>İçinde ONNX kullanımıyla C# uygulamaları

Azure Machine Learning ile bir Python modeli oluşturun ve ONNX biçimine dönüştürülmesini sağlamak için otomatik ML kullanabilirsiniz. ONNX çalışma zamanını destekleyen C#otomatik olarak derlenen modelini kullanabilirsiniz. Bu nedenle, C# değiştirilemeyen için herhangi bir gereksinim ya da herhangi bir REST uç noktalarını tanıtan ağ gecikme olmadan uygulamalar. Bu akış örneği deneyin [bu Jupyter not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Microsoft Otomatik ML

Otomatik ML ayrıca gibi diğer Microsoft çözümleri kullanılabilir:

|Tümleştirmeler|Açıklama|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Otomatik model seçimi ve eğitim ML.NET ile Visual Studio ve Visual Studio Code kullanarak .NET uygulamaları (Önizleme) ML otomatik.|
|[HDIsnight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Ölçeği genişletme, otomatik ML eğitim işleri HDInsight içindeki Spark üzerinde paralel olarak kümeleri.|
|[PowerBI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Makine öğrenimi modellerini doğrudan Power BI (Önizleme) içinde çağırın.|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Yeni makine öğrenimi modellerini SQL Server 2019 büyük veri kümelerinde verileriniz üzerinde oluşturun.|

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik Machine Learning kullanarak modelleri oluşturmayı öğrenin:

+ İzleyin [Öğreticisi: Otomatik olarak bir Azure Machine Learning otomatik sınıflandırma modeli eğitme](tutorial-auto-train-models.md)

+ Otomatik eğitim denemesini için ayarları yapılandırın:
  + Azure portal arabiriminde [şu adımları kullanarak](how-to-create-portal-experiments.md).
  + Python SDK'sı ile [şu adımları kullanarak](how-to-configure-auto-train.md).

+ Zaman serisi verilerini kullanarak train otomatik öğrenin [şu adımları kullanarak](how-to-auto-train-forecast.md).

+ Denemenin [Jupyter not defteri örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
