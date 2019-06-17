---
title: Otomatik ML denemeleri oluşturma
titleSuffix: Azure Machine Learning service
description: Otomatik machine learning, sizin için bir algoritma seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz seçenekleri öğrenin.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: df05bd984667283b0ccc143ba14fff6b35d69144
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753169"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Python'da otomatik ML denemeleri yapılandırın

Bu kılavuzda, otomatik, makine öğrenimi denemelerini ile çeşitli yapılandırma ayarlarını tanımlamayı öğrenin [Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). Otomatik machine learning bir algoritmaya ve hiperparametreleri sizin için seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz birkaç seçenek vardır.

Otomatik bir makine öğrenimi denemelerini örnekleri görüntülemek için bkz: [Öğreticisi: Otomatik machine learning ile bir sınıflandırma modeli eğitme](tutorial-auto-train-models.md) veya [eğitme modelleri bulutta otomatik machine learning ile](how-to-auto-train-remote.md).

Otomatik machine learning'de kullanılabilen yapılandırma seçenekleri:

* Deneme türünüzü seçin: Sınıflandırma, regresyon veya zaman serilerini tahmin etme
* Veri kaynağı, biçimleri ve verileri getirme
* Hedef işlem seçin: yerel veya uzak
* Otomatik machine learning deneme ayarları
* Bir otomatik makine öğrenimi denemesi çalıştırın
* Model ölçümleri keşfedin
* Kaydolun ve model dağıtma

Herhangi bir kod deneyimi tercih ediyorsanız, ayrıca [otomatik, makine öğrenimi denemeleri Azure portalında oluşturma](how-to-create-portal-experiments.md).

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçin

Denemenizi başlamadan önce çözümü machine learning sorun türünü belirlemeniz gerekir. Otomatik machine learning, Sınıflandırma, regresyon ve tahmin görev türlerini destekler.

Otomatik machine learning, otomasyon ve ayarlama işlemi sırasında aşağıdaki algoritmalarını destekler. Bir kullanıcı olarak, algoritma belirtmek gerek yoktur. DNN algoritmaları eğitim sırasında kullanılabilir, ancak otomatik ML DNN modelleri oluşturmaz.

Sınıflandırma | Regresyon | Zaman serilerini tahmin etme
|-- |-- |--
[Lojistik regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Esnek Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Esnek Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Doğrusal SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Serbest Şekil](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Serbest Şekil](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Destek vektör sınıflandırma (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN Sınıflandırıcısı](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN Regresörü](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN Regresörü](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN doğrusal Sınıflandırıcısı](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Doğrusal Regresörü](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Doğrusal Regresörü](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|

Kullanım `task` parametresinde `AutoMLConfig` deneme türünüz belirtmek için oluşturucu.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi
Otomatik machine learning, yerel masaüstüne veya Azure Blob Depolama gibi bulutta bulunan verileri destekler. Verilerin scikit okunacağı-desteklenen veri biçimlerinden öğrenin. Verileri okuyabilirsiniz:
* Numpy diziler X (özellikleri) ve y (hedef değişkeni veya olarak da bilinen etiket)
* Pandas dataframe

Örnekler:

*   Numpy diziler

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data
    y_digits = digits.target
    ```

*   Pandas dataframe

    ```python
    import pandas as pd
    from sklearn.model_selection import train_test_split
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    y = df["Label"]
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Uzak işlem üzerinde denemeyi çalıştırmak için veri alma

Denemenizi çalıştırmak için uzak bir işlem kullanıyorsanız, veri getirme, ayrı bir python betiği alınmalıdır `get_data()`. Bu betik, otomatik makine öğrenimi denemesi çalıştırıldığı uzak işlem üzerinde çalıştırılır. `get_data` her yineleme için kablo üzerinden verileri getirme gereksinimini ortadan kaldırır. Olmadan `get_data`, uzak işlem üzerinde çalıştırdığınızda, deneme başarısız olur.

İşte bir örnek `get_data`:

```python
%%writefile $project_folder/get_data.py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
def get_data(): # Burning man 2016 data
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    le = LabelEncoder()
    le.fit(df["Label"].values)
    y = le.transform(df["Label"].values)
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    return { "X" : df, "y" : y }
```

İçinde `AutoMLConfig` nesne, belirttiğiniz `data_script` parametresi ve yolunu belirtin `get_data` aşağıdakine benzeyen bir komut dosyası:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

`get_data` betik döndürebilirsiniz:

Anahtar | Tür | İle birbirini dışlayan    | Açıklama
---|---|---|---
X | Pandas Dataframe veya Numpy dizisi | data_train, etiket, sütunları |  Tüm özellikleri ile eğitme
Y | Pandas Dataframe veya Numpy dizisi |   etiket   | Veri ile eğitmek için etiket. Sınıflandırma için tamsayı dizisi olmalıdır.
X_valid | Pandas Dataframe veya Numpy dizisi   | data_train, etiket | _İsteğe bağlı_ özellik doğrulama kümesi form verileri. Belirtilmezse, X eğitimi arasında bölünmüş ve doğrulama
y_valid |   Pandas Dataframe veya Numpy dizisi | data_train, etiket | _İsteğe bağlı_ ile doğrulamak için verileri etiket. Belirtilmezse, y eğitimi arasında bölünmüş ve doğrulama
sample_weight | Pandas Dataframe veya Numpy dizisi |   data_train, etiket, sütunları| _İsteğe bağlı_ her örnek için bir ağırlık değeri. Veri noktaları için farklı ağırlıkları atamak istediğiniz zaman kullanın
sample_weight_valid | Pandas Dataframe veya Numpy dizisi | data_train, etiket, sütunları |    _İsteğe bağlı_ her doğrulama örneği için bir ağırlık değeri. Belirtilmezse, sample_weight eğitimi arasında bölünmüş ve doğrulama
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Tüm veriler (Özellikler + etiketi) ile eğitme
etiket | dize  | X, y, X_valid, y_valid |  Hangi sütunun data_train etiketi temsil eder.
Sütunları | dize dizisi  ||  _İsteğe bağlı_ özelliklerini beyaz liste sütun
cv_splits_indices   | Tamsayı dizisi ||  _İsteğe bağlı_ dizinler için verileri çapraz doğrulama bölme listesi

## <a name="train-and-validation-data"></a>Veri eğitme ve doğrulama

Ayrı eğitme ve get_data() üzerinden veya doğrudan bir doğrulama belirtebilirsiniz `AutoMLConfig` yöntemi.

### <a name="k-folds-cross-validation"></a>K hatları çapraz doğrulama

Kullanım `n_cross_validations` ayarı doğrulamaları çapraz sayısını belirtin. Eğitim veri kümesi, içine rastgele bölünür `n_cross_validations` eşit boyutta hatları. Her çapraz doğrulama sırasında yuvarlak bir kat sayısı üzerinde kalan hatları eğitilmiş modelin doğrulama için kullanılır. Bu işlem yinelenir `n_cross_validations` her Katlama doğrulama kümesi olarak bir kez kullanılana kadar yuvarlar. Tüm ortalama puanları `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Monte Carlo çapraz doğrulama (rastgele yinelenen alt örnekleme)

Kullanma `validation_size` doğrulama ve kullanımı için kullanılması gereken eğitim veri kümesi yüzdesini belirtmek için `n_cross_validations` doğrulamaları çapraz sayısını belirtmek için. Her sırasında çapraz doğrulama round, bir alt boyutunun `validation_size` kalan veriler üzerine geliştirilen model doğrulama için rastgele seçilir. Son olarak, ortalama puanlar tamamında `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği. Monte Carlo zaman serisi tahmini için desteklenmiyor.

### <a name="custom-validation-dataset"></a>Özel doğrulama veri kümesi

Rastgele bölünmüş kabul edilebilir değilse, özel doğrulama veri kümesi, genellikle zaman serisi verilerini veya imbalanced verileri kullanın. Kendi doğrulama veri kümesi belirtebilirsiniz. Model doğrulama veri kümesi yerine rastgele veri kümesi belirtilen karşı değerlendirilir.

## <a name="compute-to-run-experiment"></a>Denemeyi çalıştırmak için işlem

Daha sonra modeli eğitimi burada belirleyin. Bir otomatik machine learning eğitim denemesini aşağıdaki işlem seçenekleri çalıştırabilirsiniz:
*   Yerel makinenizde yerel Masaüstü veya dizüstü – gibi genel olarak küçük veri kümesi olduğunda ve hala keşif aşamasında demektir.
*   Buluttaki – uzak bir makine [Azure Machine Learning işlem yönetilen](concept-compute-target.md#amlcompute) kümelerinde Azure sanal makineler, makine öğrenimi modellerini eğitmenize olanağı sağlayan yönetilen bir hizmettir.

Bkz: [GitHub site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) örneğin Not Defterleri ile yerel ve uzak hedef işlem.

*   Azure Databricks kümesiyle Azure aboneliğinizdeki. Burada - daha fazla ayrıntı bulabilirsiniz [otomatik ML için Kurulum Azure Databricks cluster](how-to-configure-environment.md#azure-databricks)

Bkz: [GitHub site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) örneğin Not Defterleri ile Azure Databricks.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik makine öğrenimi deneme yapılandırmak için kullanabileceğiniz birkaç seçenek vardır. Bu parametreleri örnekleme tarafından ayarlanan bir `AutoMLConfig` nesne. Bkz: [AutoMLConfig sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) parametrelerin tam bir listesi için.

Bazı örnekler:

1.  Birincil Metrik yinelemeyle 50 yinelemeler ve 2 çapraz doğrulama hatları sonra sona erdirmek için denemeyi 12.000 saniyede en fazla süresine sahip olarak ağırlıklı AUC kullanarak sınıflandırma deneme.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        X=X,
        y=y,
        n_cross_validations=2)
    ```
2.  100 yinelemeden sonra sonuna bir regresyon deneme kümesinin bir örnek aşağıda verilmiştir, en çok uzun, her yineleme ile 600 saniye ile 5 çapraz doğrulama hatları.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

Üç farklı `task` parametre değerleri uygulamak için algoritmalar listesini belirleyin.  Kullanım `whitelist` veya `blacklist` ek yinelemeler dahil etmek veya hariç tutmak için kullanılabilir algoritmalar ile değiştirmek için parametreleri. Desteklenen modellerin listesi bulunabilir [SupportedAlgorithms sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.constants.supportedalgorithms?view=azure-ml-py).

### <a name="primary-metric"></a>Birincil Metrik
Birincil metric; Yukarıdaki örneklerde gösterildiği gibi iyileştirme için modeli eğitimi sırasında kullanılacak ölçüm belirler. Birincil ölçüm seçebilirsiniz, seçtiğiniz görev türüne göre belirlenir. Ölçümlerin bir listesi aşağıda verilmiştir.

|Sınıflandırma | Regresyon | Zaman serilerini tahmin etme
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

### <a name="data-preprocessing--featurization"></a>Veri ön işleme & özellik kazandırma sayesinde

Deneme her otomatik makine öğrenimi, verilerinizi olduğu [otomatik olarak ölçeği ve normalleştirilmiş](concept-automated-ml.md#preprocess) iyi gerçekleştirilip algoritmaları yardımcı olmak için.  Ancak, ek ön işleme/eksik değerleri imputation, kodlama ve dönüşüm gibi özellik kazandırma sayesinde, etkinleştirebilirsiniz. [Hangi özellik kazandırma sayesinde dahildir hakkında daha fazla bilgi](how-to-create-portal-experiments.md#preprocess).

Bu özellik kazandırma sayesinde etkinleştirmek üzere belirtin `"preprocess": True` için [ `AutoMLConfig` sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

### <a name="time-series-forecasting"></a>Zaman serilerini tahmin etme
Zaman serisi tahmin görev türü için ek parametreler tanımlamak için vardır.
1. time_column_name - Bu, eğitim veri içeren tarih/saat serisinde sütunun adını tanımlayan gerekli bir parametredir.
1. max_horizon - Bu eğitim verilerin dönemselliği üzerinde temel kullanıma tahmin etmek istediğiniz süreyi tanımlar. Örneğin günlük zaman grains ile eğitim verileriniz varsa, ne kadar out modelini eğitmek için istediğiniz gün tanımlayın.
1. grain_column_names - Bu, tek tek zaman serisi verilerinde görülen eğitim verilerinizi içeren bir sütun adını tanımlar. Örneğin, mağaza tarafından belirli bir marka satışları tahmin etme özelliği, depolama ve marka sütunları dilimi sütunlar tanımlarsınız.

Bu örnek aşağıda kullanılan ayarları, Not Defteri örneği kullanılabilir [burada](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

```python
# Setting Store and Brand as grains for training.
grain_column_names = ['Store', 'Brand']
nseries = data.groupby(grain_column_names).ngroups

# View the number of time series data with defined grains
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'grain_column_names': grain_column_names,
    'drop_column_names': ['logQuantity'],
    'max_horizon': n_test_periods
}

automl_config = AutoMLConfig(task='forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

## <a name="run-experiment"></a>Denemeyi çalıştırma

Denemeyi çalıştırmak ve bir model oluşturmak için gönderin. Geçirmek `AutoMLConfig` için `submit` modeli oluşturmak için yöntemi.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Bağımlılıklar, önce yeni bir makineye yüklenir.  Bu çıkış gösterilmeden önce en fazla 10 dakika sürebilir.
>Ayarı `show_output` için `True` konsolunda gösterilen çıkış sonuçlanıyor.

### <a name="exit-criteria"></a>Sonlandırma kriteri
Burada birkaç seçenek denemenizi tamamlanması tanımlayabilirsiniz.
1. Hiçbir ölçüt herhangi tanımlamazsanız - denemeyi başka hiçbir ilerleme birincil ölçümünüzün yapılana kadar devam edecek parametreleri çıkın.
1. Sayı - yinelemeleri çalıştırılacak deneme için yineleme sayısını tanımlayın. İsteğe bağlı yapabilecekleriniz her yineleme başına dakika zaman sınırı tanımlamak için iteration_timeout_minutes ekleyin.
1. Saati - ne kadar süreyle dakikalar içinde bir deneme çalıştırma sırasında devam etmesi gerektiğini tanımlayabileceğiniz ayarlarınızdaki experiment_timeout_minutes kullanarak bir süre sonra çıkın.
1. Bir puan - kullanarak, birincil bir ölçüme göre bir puan limite ulaşıldıktan sonra deneme tamamlamak için seçebileceğiniz experiment_exit_score limite ulaşıldıktan sonra çıkın.

### <a name="explore-model-metrics"></a>Model ölçümleri keşfedin

Bir not defteri kullanıyorsanız, eğitim sonuçlarınızı bir pencere öğesi veya satır içi görüntüleyebilirsiniz. Bkz: [izlemek ve modellerin değerlendirmesi](how-to-track-experiments.md#view-run-details) daha fazla ayrıntı için.

## <a name="understand-automated-ml-models"></a>Otomatik ML modelleri anlama

Otomatik ML kullanılarak üretilen herhangi bir model, aşağıdaki adımları içerir:
+ Özellik Mühendisliği otomatik (durumunda ön işleme = True)
+ Ölçeklendirme normalleştirme ve hypermeter değerlerle algoritması

Biz bu bilgi otomatik ML fitted_model çıktısını alma saydam olun.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Otomatik özellik Mühendisliği

Ön işleme içeren listeye göz atın ve [özellik Mühendisliği otomatik](concept-automated-ml.md#preprocess) bu durumda olduğunda önişle = True.

Bu örneği göz önünde bulundurun:
+ 4 giriş özellikleri vardır: (Sayısal) B (sayısal), (sayısal) C, D (tarih/saat)
+ Sayısal özellik C tamamı benzersiz değerler içeren bir kimlik sütunu olduğundan atıldı
+ Sayısal özellik A ve B eksik değerleri olan ve bu nedenle göre ortalama imputed
+ Tarih/Saat D özellikleri tespit 11 farklı mühendislik uygulanan özellikleri içine özelliğidir

Bu 2 kullanmak daha iyi anlamak için ilk adımı, ekrana sığdırılmış model API'leri.  Bkz: [Bu örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

+ 1\. API: `get_engineered_feature_names()` mühendislik uygulanan özellik adlarının bir listesini döndürür.

  Kullanım:
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  Bu liste tüm mühendislik uygulanan özellik adlarını içerir.

  >[!Note]
  >'Timeseriestransformer' görev için kullanacağınız 'regresyon' veya 'sınıflandırma' görevinin ' datatransformer' = 'tahmin', başka kullanın.

+ 2\. API: `get_featurization_summary()` özellik kazandırma sayesinde Özet tüm giriş özellikleri döndürür.

  Kullanım:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >'Timeseriestransformer' görev için kullanacağınız 'regresyon' veya 'sınıflandırma' görevinin ' datatransformer' = 'tahmin', başka kullanın.

  Çıktı:
  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```

   Konumlar:

   |Çıktı|Tanım|
   |----|--------|
   |RawFeatureName|Sağlanan dataset adından giriş özelliği/sütun.|
   |TypeDetected|Giriş özellik algılanan veri türü.|
   |Bırakılan|Giriş özellik bırakılan veya kullanılan reddedildiğini gösterir.|
   |EngineeringFeatureCount|Otomatik özellik Mühendisliği dönüşümler oluşturulan özellikler sayısı.|
   |Dönüşümler|Mühendislik uygulanan özellikler oluşturmak için özellikleri girişi için uygulanan dönüşümler listesi.|

### <a name="scalingnormalization-and-algorithm-with-hypermeter-values"></a>Ölçeklendirme normalleştirme ve hypermeter değerlerle algoritması:

Bir işlem hattı ölçeklendirme normalleştirme ve algoritma/hiper parametre değerlerini anlamak için fitted_model.steps kullanın. [Ölçeklendirme normalleştirme hakkında daha fazla bilgi](concept-automated-ml.md#preprocess). Örnek çıktı aşağıdaki gibidir:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Daha fazla bilgi edinmek için gösterilen bu yardımcı işlevini kullanın. [Bu örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb).

```python
from pprint import pprint
def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0]+ ' - ')
        else:
            pprint(step[1].get_params())
            print()

print_model(fitted_model)
```

Belirli bir algoritma (Bu durumda RobustScalar ile LogisticRegression) kullanarak bir işlem hattı için bir örnek çıktı verilmiştir.

```
RobustScaler
{'copy': True,
'quantile_range': [10, 90],
'with_centering': True,
'with_scaling': True}

LogisticRegression
{'C': 0.18420699693267145,
'class_weight': 'balanced',
'dual': False,
'fit_intercept': True,
'intercept_scaling': 1,
'max_iter': 100,
'multi_class': 'multinomial',
'n_jobs': 1,
'penalty': 'l2',
'random_state': None,
'solver': 'newton-cg',
'tol': 0.0001,
'verbose': 0,
'warm_start': False}
```

<a name="explain"></a>

## <a name="explain-the-model-interpretability"></a>Model (interpretability) açıklayın

Otomatik machine learning özellik önem anlamanıza olanak sağlar.  Eğitim işlemi sırasında modeli için genel özellik önem alabilirsiniz.  Sınıflandırma senaryoları için sınıf düzeyi özelliği önem alabilirsiniz.  Doğrulama veri kümesi özelliği önem almak için (X_valid) sağlamanız gerekir.

Özellik önem oluşturmak için iki yolunuz vardır.

*   Bir deney tamamlandıktan sonra kullanabileceğiniz `explain_model` tüm yineleme yöntemi.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Tüm yinelemeler için özellik önem görüntülemek için ayarlanmış `model_explainability` bayrak `True` AutoMLConfig içinde.

    ```python
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train,
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Bunu yaptıktan sonra belirli bir yineleme için özellik önem alınacak retrieve_model_explanation yöntemi kullanabilirsiniz.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

Azure portalında çalışma alanınızdaki özellik önem grafiği görselleştirebilirsiniz. Grafik da Jupyter pencere öğesi içinde bir not defteri kullanırken gösterilir. Bilgi edinmek için grafikler hakkında daha fazla başvurmak [örnek Azure Machine Learning hizmet not defterlerini makalesi.](samples-notebooks.md)

```Python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![özellik önem grafiği](./media/how-to-configure-auto-train/feature-importance.png)

Nasıl model açıklamalar ve özellik önem diğer alanlardaki otomatik makine öğrenimi dışında SDK'sının etkin hale getirilebilir daha fazla bilgi için bkz: [kavramı](machine-learning-interpretability-explainability.md) interpretability makale.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede model dağıtma](how-to-deploy-and-where.md).

Daha fazla bilgi edinin [otomatik machine learning ile bir regresyon modeli eğitme](tutorial-auto-train-models.md) veya [uzak bir kaynağa machine learning kullanarak eğitme otomatik](how-to-auto-train-remote.md).
