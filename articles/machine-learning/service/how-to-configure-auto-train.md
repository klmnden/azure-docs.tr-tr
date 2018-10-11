---
title: Otomatik makine öğrenimi deneme - Azure Machine Learning yapılandırın
description: Otomatik machine learning, sizin için bir algoritma seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz seçenekleri öğrenin.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 33126c094a55bc57edd49a54fbc4f5acd7401998
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079013"
---
# <a name="configure-your-automated-machine-learning-experiment"></a>Otomatik makine öğrenimi deneme yapılandırma

Otomatik machine learning (ML otomatik) bir algoritmaya ve hiperparametreleri sizin için seçer ve dağıtım için hazır bir model oluşturur. Model, daha fazla özelleştirilmiş de indirilebilir. Otomatik ML denemeleri yapılandırmak için kullanabileceğiniz birkaç seçenek vardır. Bu kılavuzda, çeşitli yapılandırma ayarlarını tanımlamanızı öğreneceksiniz.

Bir otomatik ML örneklerini görüntülemek için bkz: [Öğreticisi: otomatik machine learning ile bir sınıflandırma modeli eğitme](tutorial-auto-train-models.md) veya [eğitme modelleri bulutta otomatik machine learning ile](how-to-auto-train-remote.md).

Otomatik machine learning'de kullanılabilen yapılandırma seçenekleri:

* Örneğin, Sınıflandırma, regresyon, deneme türünü seçin 
* Veri kaynağı, biçimleri ve verileri getirme
* (Yerel veya uzak) işlem hedef seçin
* ML denemesi otomatikleştirilmiş
* Otomatik bir ML denemeyi çalıştırma
* Model ölçümleri keşfedin
* Kaydolun ve model dağıtma

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçin
Denemenizi başlamadan önce çözümü machine learning sorun türünü belirlemeniz gerekir. Denetimli öğrenmede iki kategoriye otomatik ML destekler: sınıflandırma ve regresyon. Otomasyon ve ayarlama işlemi sırasında otomatik olarak aşağıdaki algoritmalarını ML destekler. Bir kullanıcı olarak, algoritma belirtmek gerek yoktur.
Sınıflandırma | Regresyon
--|--
sklearn.linear_model. LogisticRegression | sklearn.linear_model. ElasticNet
sklearn.linear_model. SGDClassifier  | sklearn.ensemble.GradientBoostingRegressor
sklearn.naive_bayes. BernoulliNB | sklearn.tree.DecisionTreeRegressor
sklearn.naive_bayes. MultinomialNB | sklearn.neighbors.KNeighborsRegressor
sklearn.svm.SVC | sklearn.linear_model. LassoLars
sklearn.svm.LinearSVC | sklearn.linear_model. SGDRegressor
sklearn.calibration.CalibratedClassifierCV |    sklearn.ensemble.RandomForestRegressor
sklearn.neighbors.KNeighborsClassifier |    sklearn.ensemble.ExtraTreesRegressor
sklearn.tree.DecisionTreeClassifier |   lightgbm. LGBMRegressor
sklearn.ensemble.RandomForestClassifier |
sklearn.ensemble.ExtraTreesClassifier   |
sklearn.ensemble.GradientBoostingClassifier |
lightgbm. LGBMClassifier |


## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi
Otomatik ML yerel masaüstüne veya Azure Blob Depolama bulutta bulunan verileri destekler. Verilerin scikit okunacağı-desteklenen veri biçimlerinden öğrenin. Verileri X (1) Numpy diziler (özellikleri) ve y (hedef değişkeni veya olarak da bilinen etiket) veya 2) Pandas dataframe okuyabilirsiniz. 

Örnekler:

1.  Numpy diziler

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

2.  Pandas dataframe

    ```python
    Import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Uzak işlem üzerinde denemeyi çalıştırmak için veri alma

Denemenizi çalıştırmak için uzak bir işlem kullanıyorsanız, veri getirme, ayrı bir python betiği alınmalıdır `get_data()`. Bu betik, otomatik ML denemeyi çalıştırıldığı uzak işlem üzerinde çalıştırılır. `get_data` her yineleme için kablo üzerinden verileri getirme gereksinimini ortadan kaldırır. Olmadan `get_data`, uzak işlem üzerinde çalıştırdığınızda, deneme başarısız olur.

İşte bir örnek `get_data`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection 
import train_test_split 
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

`get_data` betik aşağıdaki döndürebilirsiniz:
Anahtar | Tür |    İle birbirini dışlayan | Açıklama
---|---|---|---
X | Pandas Dataframe veya Numpy dizisi | data_train, etiket, sütunları |  Tüm özellikleri ile eğitme
Y | Pandas Dataframe veya Numpy dizisi |   etiket   | Veri ile eğitmek için etiket. Sınıflandırma için tamsayı dizisi olmalıdır.
X_valid | Pandas Dataframe veya Numpy dizisi   | data_train, etiket | _İsteğe bağlı_ ile doğrulamak için tüm özellikleri. Belirtilmezse, X eğitimi arasında bölünmüş ve doğrulama
y_valid |   Pandas Dataframe veya Numpy dizisi | data_train, etiket | _İsteğe bağlı_ ile doğrulamak için verileri etiket. Belirtilmezse, y eğitimi arasında bölünmüş ve doğrulama
sample_weight | Pandas Dataframe veya Numpy dizisi |   data_train, etiket, sütunları| _İsteğe bağlı_ her örnek için bir ağırlık değeri. Veri noktaları için farklı ağırlıkları atamak istediğiniz zaman kullanın 
sample_weight_valid | Pandas Dataframe veya Numpy dizisi | data_train, etiket, sütunları |    _İsteğe bağlı_ her doğrulama örneği için bir ağırlık değeri. Belirtilmezse, sample_weight eğitimi arasında bölünmüş ve doğrulama
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Tüm veriler (Özellikler + etiketi) ile eğitme
etiket | dize  | X, y, X_valid, y_valid |  Hangi sütunun data_train etiketi temsil eder.
Sütunları | dize dizisi  ||  _İsteğe bağlı_ özelliklerini beyaz liste sütun
cv_splits_indices   | Tamsayı dizisi ||  _İsteğe bağlı_ dizinler için verileri çapraz doğrulama bölme listesi

## <a name="train-and-validation-data"></a>Veri eğitme ve doğrulama

Ayrı eğitme ve get_data() üzerinden veya doğrudan bir doğrulama belirtebilirsiniz `AutoMLConfig` yöntemi.

## <a name="cross-validation-split-options"></a>Çapraz doğrulama bölme seçeneği

### <a name="k-folds-cross-validation"></a>K hatları çapraz doğrulama

Kullanım `n_cross_validations` ayarı doğrulamaları çapraz sayısını belirtin. Eğitim veri kümesi, içine rastgele bölünür `n_cross_validations` eşit boyutta hatları. Her çapraz doğrulama sırasında yuvarlak bir kat sayısı üzerinde kalan hatları eğitilmiş modelin doğrulama için kullanılır. Bu işlem yinelenir `n_cross_validations` her Katlama doğrulama kümesi olarak bir kez kullanılana kadar yuvarlar. Son olarak, ortalama puanlar tamamında `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte Carlo çapraz doğrulama (yani) Yinelenen rastgele alt örnekleme)

Kullanma `validation_size` doğrulama ve kullanımı için kullanılması gereken eğitim veri kümesi yüzdesini belirtmek için `n_cross_validations` doğrulamaları çapraz sayısını belirtmek için. Her sırasında çapraz doğrulama round, bir alt boyutunun `validation_size` kalan veriler üzerine geliştirilen model doğrulama için rastgele seçilir. Son olarak, ortalama puanlar tamamında `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği.

### <a name="custom-validation-dataset"></a>Özel doğrulama veri kümesi

Rastgele bölünmüş kabul edilebilir değilse, özel doğrulama veri kümesini kullan (genellikle zaman serisi verileri veya imbalanced verileri). Bu, kendi doğrulama veri kümesi belirtebilirsiniz. Model doğrulama veri kümesi yerine rastgele veri kümesi belirtilen karşı değerlendirilir.

## <a name="compute-to-run-experiment"></a>Denemeyi çalıştırmak için işlem

Daha sonra modeli eğitimi burada belirleyin. Bir otomatik ML eğitim denemesini kendi ve yöneten bir işlem hedefine çalıştırır. 

Desteklenen işlem seçenekleri şunlardır:
1.  Yerel makinenizde yerel Masaüstü veya dizüstü – gibi genel olarak küçük veri kümesi olduğunda ve hala keşif aşamasında demektir.
2.  Buluttaki – uzak bir makine [Azure veri bilimi sanal makinesi](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) – Linux çalıştıran bir büyük veri kümeniz ve Azure bulutunda kullanılabilir büyük bir makineye ölçeklendirmek istiyorsunuz. 
3.  Azure Batch AI kümesi – A ölçeği ve ML otomatik yinelemeler paralel olarak çalıştırmak üzere ayarlayabildiği küme yönetilen. 


## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik ML denemenizi yapılandırmak için kullanabileceğiniz birkaç düğmelerini vardır. Bu parametreleri örnekleme tarafından ayarlanan bir `AutoMLConfig` nesne.

Bazı örnekler:

1.  Birincil Metrik yinelemeyle 50 yinelemeler ve 2 çapraz doğrulama hatları sonra sona erdirmek için denemeyi 12.000 saniyede en fazla süresine sahip olarak ağırlıklı AUC kullanarak sınıflandırma deneme.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        X=X, 
        y=y,
        n_cross_validations=2)
    ```
2.  100 yinelemeden sonra sonuna bir regresyon deneme kümesinin bir örnek aşağıda verilmiştir, en çok uzun, her yineleme ile 600 saniye ile 5 çapraz doğrulama hatları.

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

Bu tabloda parametre ayarlarını denemenizi ve varsayılan değerleri için kullanılabilir.

Özellik |  Açıklama | Varsayılan Değer
--|--|--
`task`  |Machine learning sorun türünü belirtin. İzin verilen değerler: <li>sınıflandırma</li><li>Regresyon</li>    | None |
`primary_metric` |Modelinizi oluşturmak en iyi duruma getirmek istediğiniz ölçümü. Örneğin, doğruluk primary_metric belirtirseniz, ML otomatik en yüksek doğruluğa sahip modeli bulmak için görünür. Yalnızca deneme başına bir primary_metric belirtebilirsiniz. İzin verilen değerler: <br/>**Sınıflandırma**:<br/><li> accuracy  </li><li> AUC_weighted</li><li> precision_score_weighted </li><li> balanced_accuracy </li><li> average_precision_score_weighted </li><br/>**Regresyon**: <br/><li> normalized_mean_absolute_error </li><li> spearman_correlation </li><li> normalized_root_mean_squared_error </li><li> normalized_root_mean_squared_log_error</li><li> R2_score    </li> | Sınıflandırma: doğruluğu <br/>Regresyon için: spearman_correlation <br/> |
`exit_score` |  Hedef değer, primary_metric için ayarlayabilirsiniz. Bir model primary_metric hedef karşılayan bulunduğunda otomatik ML yineleme durdurur ve deneme sona erer. Bu değer (varsayılan) ayarlanmazsa, ML otomatik deneme yinelemelerini belirtilen yineleme sayısını çalışmaya devam eder. Bir çift değer alır. Hedef hiçbir zaman ulaşırsa, yinelemeler içinde belirtilen yineleme sayısını ulaşana kadar otomatik ML devam eder.|   None
`iterations` |En yüksek yineleme sayısı. Her yineleme, bir işlem hattı, sonuçları bir eğitim işini eşittir. İşlem hattı, verileri ön işleme ve modeli ' dir. Yüksek kaliteli model almak için 250 veya daha fazlasını kullanın | 100
`Concurrent_iterations`|    En fazla paralel olarak çalıştırılması için yineleme sayısı. Bu ayar, yalnızca uzak işlem için çalışır.|    1
`max_cores_per_iteration`   | Kaç tane çekirdeğim işlem hedef tek bir işlem hattını eğitmek için kullanılan gösterir. Birden çok çekirdek algoritması yararlanabilir, bu çok çekirdekli makine performansını artırır. Tüm çekirdekler üzerinde bir makineyi kullanmak için-1 ayarlayabilirsiniz.|  1
`max_time_sec` |    Belirli bir yinelemeye geçen süreyi (saniye) miktarını sınırlar. Bir yineleme belirtilen miktarı aşarsa, bu yineleme iptal. Aksi durumda ayarlama, yineleme işlemi tamamlanana kadar çalışmaya devam edecektir. |   None
`n_cross_validations`   |Çapraz doğrulama bölmelerinin sayısı| None
`validation_size`   |Tüm eğitim örnek bir yüzdesi olarak ayarlanmış doğrulama boyutu.|  None
`preprocess` | True/False <br/>Giriş ön işleme gerçekleştirmek için doğru etkinleştirir deneyin. Aşağıdaki ön işleme'nın bir alt kümesidir<li>Veriler eksik: veri-sayısal ortalama, çoğu occurance metinle birlikte eksik Imputes </li><li>Kategorik değerlere: veri türü sayısal ve benzersiz değerleri ise daha az yüzde 5'inden, sık erişimli bir kodlama içinde dönüştürür </li><li>Tam liste denetimi vb. [GitHub deposu](https://aka.ms/aml-notebooks)</li><br/>Not: veri seyrek ise kullanamazsınız önişle = true |  False | 
`blacklist_algos`   | Otomatik ML deneme çalışır birçok farklı algoritma vardır. ML deneme belirli algoritmaları dışlanacak otomatik yapılandırın. Algoritmalarından kümeniz için iyi çalışmaz farkında olması durumunda yararlıdır. Algoritmalar hariç kaydettiğinizde kaynaklar ve eğitim süresini hesaplayabilirsiniz.<br/>Sınıflandırma için izin verilen değerler<br/><li>Lojistik regresyon</li><li>SGD Sınıflandırıcısı</li><li>MultinomialNB</li><li>BernoulliNB</li><li>SVM</li><li>LinearSVM</li><li>kNN</li><li>DT</li><li>RF</li><li>Ek ağaçları</li><li>Gradyan artırma</li><li>lgbm_classifier</li><br/>Regresyon için izin verilen değerler<br/><li>Elastik net</li><li>Gradyan artırırken regresörü</li><li>DT regresörü</li><li>kNN regresörü</li><li>Lars Şekil</li><li>SGD regresörü</li><li>RF regresörü</li><li>Ek ağaçları regresörü</li>|   None
`verbosity` |En ayrıntılı ve kritik olan olan bilgileri günlüğe kaydetme düzeyini denetler en az.<br/>İzin verilen değerler şunlardır:<br/><li>logging.INFO</li><li>günlüğe kaydetme. UYARI</li><li>günlüğe kaydetme. HATA</li><li>günlüğe kaydetme. KRİTİK</li>  | logging.INFO</li> 
`X` | Tüm özellikleri ile eğitme |  None
`y` |   Veri ile eğitmek için etiket. Sınıflandırma için tamsayı dizisi olmalıdır.|  None
`X_valid`|_İsteğe bağlı_ ile doğrulamak için tüm özellikleri. Belirtilmezse, X eğitimi arasında bölünmüş ve doğrulama |   None
`y_valid`   |_İsteğe bağlı_ ile doğrulamak için verileri etiket. Belirtilmezse, y eğitimi arasında bölünmüş ve doğrulama    | None
`sample_weight` |   _İsteğe bağlı_ her örnek için bir ağırlık değeri. Veri noktaları için farklı ağırlıkları atamak istediğiniz zaman kullanın |   None
`sample_weight_valid`   |   _İsteğe bağlı_ her doğrulama örneği için bir ağırlık değeri. Belirtilmezse, sample_weight eğitimi arasında bölünmüş ve doğrulama   | None
`run_configuration` |   RunConfiguration nesnesi.  Uzaktan çalıştırmalar için kullanılır. |None
`data_script`  |    Get_data yöntemi içeren dosyanın yolu.  Uzaktan çalıştırmalar için gereklidir.   |None


## <a name="run-experiment"></a>Denemeyi çalıştırma

Ardından, biz bizim için bir model oluşturmak ve çalıştırmak için bir deneme başlatabilir. Geçirmek `AutoMLConfig` için `submit` modeli oluşturmak için yöntemi.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Bağımlılıklar, önce yeni bir DSVM'nin yüklenir.  Bu çıkış gösterilmeden önce en fazla 10 dakika sürebilir.
>Ayar `show_output` konsolunda gösterilen çıkış True ile sonuçlanır.


## <a name="explore-model-metrics"></a>Model ölçümleri keşfedin
Bir not defteri kullanıyorsanız, sonuçlarınızı bir pencere öğesi veya satır içi görüntüleyebilirsiniz. "İzlemek ve modellerin değerlendirmesi" için ayrıntılara bakın. (otomatik ML ilgili bilgilere AML içeriği sağlamak)

Aşağıdaki ölçümler her yinelemede kaydedilir
* AUC_macro
* AUC_micro
* AUC_weighted
* accuracy
* average_precision_score_macro
* average_precision_score_micro
* average_precision_score_weighted
* balanced_accuracy
* f1_score_macro
* f1_score_micro
* f1_score_weighted
* log_loss
* norm_macro_recall
* precision_score_macro
* precision_score_micro
* precision_score_weighted
* recall_score_macro
* recall_score_micro
* recall_score_weighted
* weighted_accuracy

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede model dağıtma](how-to-deploy-and-where.md).

Daha fazla bilgi edinin [otomatik ML ile bir sınıflandırma modeli eğitme](tutorial-auto-train-models.md) veya [otomatik ML kullanarak uzak bir kaynağa eğitme](how-to-auto-train-remote.md). 