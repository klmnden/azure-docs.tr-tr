---
title: Otomatik-zaman serisi tahmin modeli eğitme
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti zaman serisi tahmin regresyon modeli kullanılarak otomatik olarak makine öğrenimi eğitmek için kullanmayı öğrenin.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 05/02/2019
ms.openlocfilehash: c7f4b6d8aa614a460772fb7af11f9b83dc3fc979
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800808"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Otomatik-zaman serisi tahmin modeli eğitme

Bu makalede, otomatik machine learning Azure Machine Learning hizmeti kullanarak zaman serisi tahmin regresyon modelini eğitme öğrenin. Bir tahmin modelli yapılandırma ayarını otomatik makine öğrenimini kullanarak bir standart regresyon modelini benzer, ancak zaman serisi verileri ile çalışmak için bazı yapılandırma seçenekleri ve ön işleme adımları yok. Aşağıdaki örnekler, nasıl göstermek için:

* Zaman serisi model için verileri hazırlama
* Belirli bir zaman serisi parametrelerinde yapılandırma bir [ `AutoMLConfig` ](/python/api/azureml-train-automl/azureml.train.automl.automlconfig) nesnesi
* Zaman serisi verileri ile Öngörüler çalıştırma

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GW]

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Çalışma alanı oluşturmak için bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md).
* Bu makalede, bir otomatik makine öğrenimi denemesi ayarı ile temel alışkın olduğunuz varsayılır. İzleyin [öğretici](tutorial-auto-train-models.md) veya [yapılır](how-to-configure-auto-train.md) temel otomatik makine öğrenme deneme tasarım desenleri görmek için.

## <a name="preparing-data"></a>Verileri hazırlama

En önemli fark tahmin regresyon görev türü ve regresyon arasında görev türü otomatik makine öğrenimi içinde bir özellik gösteren geçerli zaman serisi verilerinizi dahil. Normal zaman serisi iyi tanımlanmış ve tutarlı bir sıklık ve sürekli bir süre içinde her bir örnek noktada bir değerine sahiptir. Bir dosyanın şu anlık görüntüye göz önünde bulundurun `sample.csv`.

    day_datetime,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/4/2018,A,2300,36
    9/4/2018,B,550,36
    9/5/2018,A,2100,36
    9/5/2018,B,650,36
    9/6/2018,A,2400,36
    9/6/2018,B,700,36
    9/7/2018,A,2450,36
    9/7/2018,B,650,36

Bu veri kümesi bir günlük satış verilerini iki farklı mağazalarda A ve b ayrıca sahip bir şirket için basit bir örnektir, bir özellik için `week_of_year` haftalık mevsimsellik algılamaya modeli sağlayacaktır. Alan `day_datetime` temiz zaman serisi Günlük Sıklık ve alanını temsil eden `sales_quantity` Öngörüler çalıştırmak için hedef sütunudur. Bir Pandas dataframe verileri okumak ve ardından kullanmak `to_datetime` zaman serisi olduğundan emin olmak için işlevi bir `datetime` türü.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

Bu durumda zaman alana göre artan düzende zaten veriler sıralanır `day_datetime`. Ancak, deneme ayarlama ayarlarken, istediğiniz zaman sütunu geçerli zaman serisi oluşturmak için artan düzende sıralanır olun. 1000 kayıt verileri içerdiğini varsayalım ve belirleyici bir bölünmüş verileri eğitim oluşturma ve veri kümelerini test yapın. Ardından hedef alan ayırmak `sales_quantity` tahmin eğitme ve test oluşturmak için ayarlar.

```python
X_train = data.iloc[:950]
X_test = data.iloc[-50:]

y_train = X_train.pop("sales_quantity").values
y_test = X_test.pop("sales_quantity").values
```

> [!NOTE]
> Gelecekteki değerleri tahmini bir model eğitim, tüm hedeflenen Ufkunuzu için Öngörüler çalıştırırken eğitim kullanılan özellikler kullanılabilir emin olun. Örneğin, bir talep tahmin oluştururken, bir özellik için geçerli hisse senedi fiyatı da dahil olmak üzere yüksek düzeyde eğitim doğruluğunu artırabilir. Ancak, uzun bir horizon ile tahmini yapmak istiyorsanız, gelecekteki zaman serisi noktalarına karşılık gelen gelecekteki stok değerleri doğru şekilde tahmin mümkün olmayabilir ve model doğruluğundan düşebilir.

## <a name="configure-and-run-experiment"></a>Denemeyi çalıştırma ve yapılandırma

Otomatik makine öğrenimi, görevler tahmin için zaman serisi verileri ön işleme ve tahmini adımlarını kullanır. Aşağıdaki önceden işleme adımları yürütülecek:

* Zaman serisi Örnek sıklığı (örn: saatlik, günlük, haftalık) ve sürekli seri hale getirmek için zaman noktası yok için yeni kayıt oluşturun algılayın.
* (ORTANCA sütun değerleri kullanarak) özellik sütunları ve hedef (aracılığıyla İleri dolgu) eksik değerleri impute
* Sabit etkileri farklı serilerdeki etkinleştirmek için dilimi tabanlı özellikleri oluşturma
* Dönemsel desenleri ilginizi yardımcı olmak için zaman tabanlı özellikleri oluşturma
* Kategorik değişkenleri sayısal miktarlar için kodlama

`AutoMLConfig` Nesne için bir otomatik machine learning görevi gerekli veri ve ayarları tanımlar. Benzer şekilde, regresyon problemi, görev türü, eğitim verileri, yineleme sayısı ve çapraz doğrulama sayısı gibi standart eğitim parametrelerini tanımlayın. Görevler tahmin için denemeyi etkileyen ayarlanması gereken ek parametre yok. Aşağıdaki tabloda, her bir parametre ve kullanımını açıklar.

| param | Açıklama | Gerekli |
|-------|-------|-------|
|`time_column_name`|Zaman serisi oluşturmak ve sıklığının çıkarımını yapma için kullanılan giriş veri datetime sütunu belirtmek için kullanılır.|✓|
|`grain_column_names`|Giriş verilerinde serisine grupları tanımlama adları. Dilimi tanımlanmamışsa, veri kümesi bir zaman serisi olduğu varsayılır.||
|`max_horizon`|Maksimum zaman serisi sıklık birimi tahmin horizon istenen.|✓|
|`target_lags`|*n* İleri lag dönemlerine hedef model eğitiminin önce değer.||
|`target_rolling_window_size`|*n* geçmiş dönemlerini tahmin edilen değerler oluşturmak için kullanılacak < = Eğitim kümesi boyutu. Atlanırsa, *n* tam eğitim boyutu ayarlanır.||

Zaman serisi ayarları bir sözlük nesnesi olarak oluşturun. Ayarlama `time_column_name` için `day_datetime` alanındaki veri kümesi. Tanımlama `grain_column_names` emin olmak için parametre **iki ayrı zaman serisi grupları** ; depolama A ve b son olarak, bir veri kümesi için oluşturulan `max_horizon` tahmin için tüm test için 50'ye ayarlayın. 10 nokta ile tahmini bir pencere olarak `target_rolling_window_size`ve hedef değer 2 nokta ile gecikme `target_lags` parametresi.

```python
time_series_settings = {
    "time_column_name": "day_datetime",
    "grain_column_names": ["store"],
    "max_horizon": 50,
    "target_lags": 2,
    "target_rolling_window_size": 10
}
```

Artık bir standart oluşturma `AutoMLConfig` belirterek nesne `forecasting` görev türü ve denemeyi gönderme. Model tamamlandıktan sonra en iyi şekilde çalışma yineleme alın.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

> [!NOTE]
> Oluşturmaya çalışırken bir kaynağı doğrulama yordam otomatik machine learning uygulayan şekilde çapraz doğrulama (MS) yordamı için zaman serisi verileri kurallı K Katlama çapraz doğrulama stratejisinin temel istatistiksel varsayımların ihlal edebilirsiniz Zaman serisi verileri çapraz doğrulama hatları. Bu yordamı kullanmak için belirtin `n_cross_validations` parametresinde `AutoMLConfig` nesne. Doğrulama ve kendi doğrulama ayarlar ile kullanımı devre dışı bırakabilir `X_valid` ve `y_valid` parametreleri.

### <a name="view-feature-engineering-summary"></a>Özellik Mühendisliği özetini görüntüle

Otomatik machine learning'de zaman serisi görev türleri için işlem mühendislik özellik ayrıntılarını görüntüleyebilirsiniz. Aşağıdaki kod, aşağıdaki öznitelikleri yanı sıra her bir ham özellik gösterir:

* İşlenmemiş özellik adı
* Bu ham özelliği biçimlendirilmiş mühendislik uygulanan özellikler
* Türü algılandı
* Özellik olup bırakıldı
* Raw özelliği için özellik dönüşümler listesi

```python
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
```

## <a name="forecasting-with-best-model"></a>En iyi modeli ile tahmin

Sınama veri kümesi için değerleri tahmin etmek için en iyi modeli yineleme kullanın.

```python
y_predict = fitted_model.predict(X_test)
y_actual = y_test.flatten()
```

Alternatif olarak, `forecast()` yerine işlev `predict()`, tahminler ne zaman başlaması gerektiğini, belirtimleri sağlayacaktır. Aşağıdaki örnekte, önce tüm değerleri değiştirin `y_pred` ile `NaN`. Kullanırken normalde olduğu gibi tahmin kaynağı eğitim verilerini sonunda bu durumda olacaktır `predict()`. Ancak, yalnızca ikinci yarısında yüklediyse `y_pred` ile `NaN`, işlev sayısal değerler ilk yarısında değiştirilmemiş ancak tahmin bırakacaksa `NaN` ikinci yarısında hiç değerleri. İşlev hizalanmış özellikleri ve tahmin edilen değerler döndürür.

Ayrıca `forecast_destination` parametresinde `forecast()` değerleri belirtilen bir tarih gönderinizi tahmin etmek için işlevi.

```python
y_query = y_test.copy().astype(np.float)
y_query.fill(np.nan)
y_fcst, X_trans = fitted_pipeline.forecast(X_test, y_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

RMSE Hesapla (kök ortalama karesi alınmış hata) arasında `y_test` gerçek değerler ve tahmin edilen değerler `y_pred`.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Artık genel doğruluğu belirlenen, model bilinmeyen gelecekteki değerleri tahmin etmek için kullanılacak en gerçekçi bir sonraki adım. Yalnızca sınama kümesi ile aynı biçimde bir veri kümesi kaynağı `X_test` ancak gelecekteki bir tarih/saat ve sonuçta elde edilen tahmini her zaman serisi adım için tahmin edilen değerler kümesidir. Veri kümesindeki son zaman serisi kayıtları 31/12/2018 için olan varsayılır. Sonraki gün için talebini tahmin etmek için (veya tahmin etmek gerek duyduğunuz kadar çok nokta < = `max_horizon`), tek bir oluşturma zaman serisi kayıt her mağaza için 01/01/2019 için.

    day_datetime,store,week_of_year
    01/01/2019,A,1
    01/01/2019,A,1

Bir dataframe gelecekte bu verileri yüklemek ve sonra çalıştırmak için gerekli olan adımları yineleyin `best_run.predict(X_test)` gelecekteki değerleri tahmin etmek için.

> [!NOTE]
> Değerler kullanılamaz tahmin büyüktür dönem sayısı için `max_horizon`. Model ile mevcut Ufuk ötesinde gelecekteki değerleri tahmin etmek için daha büyük bir yatay yeniden eğitimli olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* İzleyin [öğretici](tutorial-auto-train-models.md) otomatik makine öğrenimi denemeleri oluşturmayı öğrenin.
* Görünüm [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) başvuru belgeleri.
