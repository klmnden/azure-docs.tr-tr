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
ms.date: 03/19/2019
ms.openlocfilehash: 32f96a28e027bfd0e65d934bb47bb98400af459d
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58360733"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Otomatik-zaman serisi tahmin modeli eğitme

Bu makalede, otomatik machine learning Azure Machine Learning hizmeti kullanarak zaman serisi tahmin regresyon modelini eğitme öğrenin. Bir tahmin modelli yapılandırma ayarını otomatik makine öğrenimini kullanarak bir standart regresyon modelini benzer, ancak zaman serisi verileri ile çalışmak için bazı yapılandırma seçenekleri ve ön işleme adımları yok. Aşağıdaki örnekler, nasıl göstermek için:

* Zaman serisi model için verileri hazırlama
* Belirli bir zaman serisi parametrelerinde yapılandırma bir [ `AutoMLConfig` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) nesnesi
* Zaman serisi verileri ile Öngörüler çalıştırma

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Çalışma alanı oluşturmak için bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md).
* Bu makalede, bir otomatik makine öğrenimi denemesi ayarı ile temel alışkın olduğunuz varsayılır. İzleyin [öğretici](tutorial-auto-train-models.md) veya [yapılır](how-to-configure-auto-train.md) temel otomatik makine öğrenme deneme tasarım desenleri görmek için.

## <a name="preparing-data"></a>Verileri hazırlama

En önemli fark tahmin regresyon görev türü ve regresyon arasında görev türü otomatik makine öğrenimi içinde bir özellik gösteren geçerli zaman serisi verilerinizi dahil. Normal zaman serisi iyi tanımlanmış ve tutarlı bir sıklık ve sürekli bir süre içinde her bir örnek noktada bir değerine sahiptir. Bir dosyanın şu anlık görüntüye göz önünde bulundurun `sample.csv`.

    week_starting,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/10/2018,A,2300,37
    9/10/2018,B,550,37
    9/17/2018,A,2100,38
    9/17/2018,B,650,38
    9/24/2018,A,2400,39
    9/24/2018,B,700,39
    10/1/2018,A,2450,40
    10/1/2018,B,650,40

Bu veri kümesine ilişkin haftalık satış verileri A ve b ayrıca olmak üzere iki farklı mağazalarda sahip bir şirket için basit bir örnektir, bir özellik için `week_of_year` haftalık mevsimsellik algılamaya modeli sağlayacaktır. Alan `week_starting` temiz zaman serisi haftalık bir sıklık ve alanını temsil eder. `sales_quantity` Öngörüler çalıştırmak için hedef sütunudur. Bir Pandas dataframe verileri okumak ve ardından kullanmak `to_datetime` zaman serisi olduğundan emin olmak için işlevi bir `datetime` türü.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["week_starting"] = pd.to_datetime(data["week_starting"])
```

Bu durumda zaman alana göre artan düzende zaten veriler sıralanır `week_starting`. Ancak, deneme ayarlama ayarlarken, istediğiniz zaman sütunu geçerli zaman serisi oluşturmak için artan düzende sıralanır olun. 1000 kayıt verileri içerdiğini varsayalım ve belirleyici bir bölünmüş verileri eğitim oluşturma ve veri kümelerini test yapın. Ardından hedef alan ayırmak `sales_quantity` tahmin eğitme ve test oluşturmak için ayarlar.

```python
X_train = data.iloc[:950]
X_test = data.iloc[-50:]

y_train = X_train.pop("sales_quantity").values
y_test = X_test.pop("sales_quantity").values
```

> [!NOTE]
> Gelecekteki değerleri tahmini bir model eğitim, tüm hedeflenen Ufkunuzu için Öngörüler çalıştırırken eğitim kullanılan özellikler kullanılabilir emin olun. Örneğin, bir talep tahmin oluştururken, bir özellik için geçerli hisse senedi fiyatı da dahil olmak üzere yüksek düzeyde eğitim doğruluğunu artırabilir. Ancak, uzun bir horizon ile tahmini yapmak istiyorsanız, gelecekteki zaman serisi noktalarına karşılık gelen gelecekteki stok değerleri doğru şekilde tahmin mümkün olmayabilir ve model doğruluğundan düşebilir.

## <a name="configure-experiment"></a>Deneme yapılandırma

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

Zaman serisi ayarları bir sözlük nesnesi olarak oluşturun. Ayarlama `time_column_name` için `week_starting` alanındaki veri kümesi. Tanımlama `grain_column_names` emin olmak için parametre **iki ayrı zaman serisi grupları** verilerimizi; depolama A ve b son olarak, bir kümesi için oluşturulan `max_horizon` tahmin için tüm test için 50'ye ayarlayın.

```python
time_series_settings = {
    "time_column_name": "week_starting",
    "grain_column_names": ["store"],
    "max_horizon": 50
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

## <a name="forecasting-with-best-model"></a>En iyi modeli ile tahmin

Sınama veri kümesi için değerleri tahmin etmek için en iyi modeli yineleme kullanın.

```python
y_predict = fitted_model.predict(X_test)
y_actual = y_test.flatten()
```

RMSE Hesapla (kök ortalama karesi alınmış hata) arasında `y_test` gerçek değerler ve tahmin edilen değerler `y_pred`.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Artık genel doğruluğu belirlenen, model bilinmeyen gelecekteki değerleri tahmin etmek için kullanılacak en gerçekçi bir sonraki adım. Yalnızca sınama kümesi ile aynı biçimde bir veri kümesi kaynağı `X_test` ancak gelecekteki bir tarih/saat ve sonuçta elde edilen tahmini her zaman serisi adım için tahmin edilen değerler kümesidir. Veri kümesindeki son zaman serisi kayıtları olan haftadan itibaren varsayar 12/31 Ocak 2018. Sonraki hafta için talebini tahmin etmek için (veya tahmin etmek gerek duyduğunuz kadar çok nokta < = `max_horizon`), tek bir oluşturma zaman serisi kayıt her mağaza için haftadan itibaren 07/01/2019.

    week_starting,store,week_of_year
    01/07/2019,A,2
    01/07/2019,A,2

Bir dataframe gelecekte bu verileri yüklemek ve sonra çalıştırmak için gerekli olan adımları yineleyin `best_run.predict(X_test)` gelecekteki değerleri tahmin etmek için.

> [!NOTE]
> Değerler kullanılamaz tahmin büyüktür dönem sayısı için `max_horizon`. Model ile mevcut Ufuk ötesinde gelecekteki değerleri tahmin etmek için daha büyük bir yatay yeniden eğitimli olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* İzleyin [öğretici](tutorial-auto-train-models.md) otomatik makine öğrenimi denemeleri oluşturmayı öğrenin.
* Görünüm [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) başvuru belgeleri.
