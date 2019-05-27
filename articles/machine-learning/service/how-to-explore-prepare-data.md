---
title: Keşfedin ve dönüştürme (veri kümesi sınıfı)
titleSuffix: Azure Machine Learning service
description: Özet istatistikleri kullanarak verileri araştırmak ve verileri temizleme, Dönüşüm ve özellik Mühendisliği ile verileri hazırlama
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/23/2019
ms.openlocfilehash: e692b0dc1089804b1d68b79c1a6f438f30554602
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146290"
---
# <a name="explore-and-prepare-data-with-the-dataset-class-preview"></a>Keşfedin ve veri kümesi sınıfı (Önizleme) ile verileri hazırlama

Keşfedin ve azureml veri kümeleri pakette ile verileri hazırlama hakkında bilgi edinin [Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). [Veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) sınıfı (Önizleme) keşfedin ve işlevleri gibi sağlayarak verilerinizi hazırlamanız olanak tanır: örnekleme, Özet istatistikleri ve akıllı dönüşümler. Dönüştürme adımı kaydedilir [veri kümesi tanımları](how-to-manage-dataset-definitions.md) farklı şemalar birden çok daha büyük dosyaları yüksek düzeyde ölçeklenebilir bir şekilde işleme yeteneğine sahip.

> [!Important]
> Bazı veri kümesi sınıfları (Önizleme) bağımlılıklara sahip [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paket (GA). Dönüştürme işlevleri doğrudan GA'ed ile gerçekleştirilebilir ancak [Data Prep işlevleri](how-to-transform-data.md), yeni bir çözüm oluşturuyorsanız bu makalede açıklanan veri kümesi paket sarmalayıcıları öneririz. Azure Machine Learning veri kümeleri (Önizleme), yalnızca, verilerinizi dönüştürmenizi izin ver aynı zamanda [veri anlık görüntü](how-to-create-dataset-snapshots.md) ve depolamak [tutulan veri kümesi tanımları](how-to-manage-dataset-definitions.md). Veri kümeleri, yapay ZEKA çözümlerini veri kümelerini yönetmek için genişletilmiş işlevselliği sunan veri hazırlığı SDK ' nın sonraki sürümüdür.

## <a name="prerequisites"></a>Önkoşullar

Keşfedin ve verilerinizi hazırlamanız için gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

* Python için Azure Machine Learning SDK'sını (sürüm 1.0.21 veya üzeri), azureml veri kümeleri paketi içerir. Yüklemek veya SDK'sının en son sürüme güncelleştirmek için bkz: [yükleme veya SDK'sı güncelleştirme](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* Azure Machine Learning veri hazırlığı SDK. Yüklemek veya en son sürüme güncelleştirmek için bkz: [yükleme veya güncelleştirme Data Prep SDK'sını](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py#install).

* Birlikte örnekleri izlemek için örnek dosyalarını indirme: [crime.csv](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) ve [city.json](https://dprepdata.blob.core.windows.net/dataset-sample-files/city.json).

## <a name="sampling"></a>Örnekleme

Bir örnek veri mimarisi ve içeriğin ilk anlamak için verilerinizi alın. Şu anda [ `sample()` ](https://docs.microsoft.com//python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#sample-sample-strategy--arguments-) Dataset sınıfı yönteminden üst N, basit rastgele ve Stratified örnekleme stratejileri destekler.

```Python
from azureml.core.dataset import Dataset
import random

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')

# create seed for Simple Random and Stratified sampling
seed = random.randint(0, 4294967295)
```

### <a name="top-n-sample"></a>İlk N örneği

Üst N örnekleme için veri kümesinin ilk n kayıt, örnek olarak verilmiştir. Bu yalnızca veya verilerinizdeki alanları ne olduğunu görmek için hangi veri kayıtları görünümü fikir almak çalışıyorsanız yararlı olur.

```Python
top_n_sample_dataset = dataset.sample('top_n', {'n': 5})
top_n_sample_dataset.to_pandas_dataframe()
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|...|
-|--|-----------|----|-----|----|------------|---
0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|YANILTICI YÖNTEMİ|...
1|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD AVE|890|HIRSIZLIĞI|...
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO AVE|1154|YANILTICI YÖNTEMİ|...
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE KAYDET|1120|YANILTICI YÖNTEMİ|...
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE KAYDET|890|HIRSIZLIĞI|...

### <a name="simple-random-sample"></a>Basit rastgele örnek

Basit rastgele örnekleme veri doldurma her üyesi eşit bir örnek bir parçası olarak seçilmesini olasılığı vardır. İçinde `simple_random` örnek stratejisi, Veri kümenizi kayıtlardan belirtilen olasılığına bağlı olarak seçilir ve değiştirilmiş bir veri kümesi döndürür. Çekirdek parametresi isteğe bağlıdır.

```Python
simple_random_sample_dataset = dataset.sample('simple_random', {'probability':0.3, 'seed': seed})
simple_random_sample_dataset.to_pandas_dataframe()
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD AVE|890|HIRSIZLIĞI|...
1|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO AVE|1154|YANILTICI YÖNTEMİ|...
2|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE KAYDET|890|HIRSIZLIĞI|...

### <a name="stratified-sample"></a>Stratified örnek

Stratified örnekleri belirli gruplara popülasyonun bir örnek gösterilir emin olun. İçinde `stratified` örnek stratejisi popülasyonu strata veya benzerlikler üzerinde bağlı alt gruplar, ayrılmıştır ve kayıtları gelen her strata belirttiği strata ağırlıklara göre rastgele seçilir `fractions` parametresi.

Aşağıdaki örnekte, biz belirtilen sütunlara göre her bir kayıt grubunda ve söz konusu kaydı strata X ağırlık bilgilere dayalı dahil `fractions`. Bir strata belirtilmedi veya kayıt birleştirilemez, örnek için varsayılan ağırlık 0'dır.

```Python
# take 50% of records with `Primary Type` as `THEFT` and 20% of records with `Primary Type` as `DECEPTIVE PRACTICE` into sample Dataset
fractions = {}
fractions[('THEFT',)] = 0.5
fractions[('DECEPTIVE PRACTICE',)] = 0.2

sample_dataset = dataset.sample('stratified', {'columns': ['Primary Type'], 'fractions': fractions, 'seed': seed})

sample_dataset.to_pandas_dataframe()
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD AVE|890|HIRSIZLIĞI|...
1|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE KAYDET|890|HIRSIZLIĞI|...
2|10535059|HZ278872|4/15/2016 4:30|004XX S KILBOURN KAYDET|810|HIRSIZLIĞI|...

## <a name="explore-with-summary-statistics"></a>İle Özet istatistiklerini keşfedin

 Eksik değerleri, anormallikleri veya hata sayılarını ile [ `get_profile()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) yöntemi. Bu işlev profili alır ve Özet istatistiklerini sırayla yardımcı olur, verilerinizin uygulamak için gerekli veri hazırlığı işlemleri belirleyin.

```Python
dataset.get_profile()
```

||Tür|Min|Maks|Sayı|Eksik sayısı|Sayısı eksik|Eksik yüzde|Hata sayısı|Boş sayısı|% 0,1 Quantile|%1 Quantile|%5 Quantile|% 25 Quantile|% 50 Quantile|% 75 Quantile|% 95 Quantile|% 99 Quantile|% 99,9 Quantile|Ortalama|Standart Sapma|Varyans|Komutunu|Basıklık
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
Kimlik|FieldType.INTEGER|1.04986e + 07|1.05351e + 07|10.0|0.0|10.0|0.0|0.0|0.0|1.04986e + 07|1.04992e + 07|1.04986e + 07|1.05166e + 07|1.05209e + 07|1.05259e + 07|1.05351e + 07|1.05351e + 07|1.05351e + 07|1.05195e + 07|12302.7|1.51358e + 08|-0.495701|-1.02814
Büyük/küçük harf numarası|FieldType.STRING|HZ239907|HZ278872|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Date|FieldType.DATE|2016-04-04 23:56:00+00:00|2016-04-15 17:00:00+00:00|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Engelle|FieldType.STRING|004XX S KILBOURN KAYDET|113XX S PRAIRIE KAYDET|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
IUCR|FieldType.INTEGER|810|1154|10.0|0.0|10.0|0.0|0.0|0.0|810|850|810|890|1136|1153|1154|1154|1154|1058.5|137.285|18847.2|-0.785501|-1.3543
Birincil tür|FieldType.STRING|YANILTICI YÖNTEMİ|HIRSIZLIĞI|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Açıklama|FieldType.STRING|SAHTE DENETİMİ|500 ABD DOLARI|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Konum açıklaması|FieldType.STRING||OKUL, OLUŞTURMA, GENEL|10.0|0.0|10.0|0.0|0.0|1.0||||||||||||||
Arrest|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Yurt içi|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Sinyali|FieldType.INTEGER|531|2433|10.0|0.0|10.0|0.0|0.0|0.0|531|531|531|614|1318.5|1911|2433|2433|2433|1371.1|692.094|478994|0.105418|-1.60684
Bölge|FieldType.INTEGER|5|24|10.0|0.0|10.0|0.0|0.0|0.0|5|5|5|6|13|19|24|24|24|13.5|6.94822|48.2778|0.0930109|-1.62325
İleri Git|FieldType.INTEGER|1|48|10.0|0.0|10.0|0.0|0.0|0.0|1|5|1|9|22.5|40|48|48|48|24.5|16.2635|264.5|0.173723|-1.51271
Topluluk alan|FieldType.INTEGER|4|77|10.0|0.0|10.0|0.0|0.0|0.0|4|8.5|4|24|37.5|71|77|77|77|41.2|26.6366|709.511|0.112157|-1.73379
FBI kod|FieldType.INTEGER|6|11|10.0|0.0|10.0|0.0|0.0|0.0|6|6|6|6|11|11|11|11|11|9.4|2.36643|5.6|-0.702685|-1.59582
X Koordinatı|FieldType.INTEGER|1.16309e + 06|1.18336e + 06|10.0|7.0|3.0|0.7|0.0|0.0|1.16309e + 06|1.16309e + 06|1.16309e + 06|1.16401e + 06|1.16678e + 06|1.17921e + 06|1.18336e + 06|1.18336e + 06|1.18336e + 06|1.17108e + 06|10793.5|1.165e + 08|0.335126|-2.33333
Y Koordinatı|FieldType.INTEGER|1.8315e + 06|1.908e + 06|10.0|7.0|3.0|0.7|0.0|0.0|1.8315e + 06|1.8315e + 06|1.8315e + 06|1.83614e + 06|1.85005e + 06|1.89352e + 06|1.908e + 06|1.908e + 06|1.908e + 06|1.86319e + 06|39905.2|1.59243e + 09|0.293465|-2.33333
Yıl|FieldType.INTEGER|2016|2016|10.0|0.0|10.0|0.0|0.0|0.0|2016|2016|2016|2016|2016|2016|2016|2016|2016|2016|0|0|NaN|NaN
Güncelleştirme tarihi|FieldType.DATE|2016-05-11 15:48:00+00:00|2016-05-27 15:45:00+00:00|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Enlem|FieldType.DECIMAL|41.6928|41.9032|10.0|7.0|3.0|0.7|0.0|0.0|41.6928|41.6928|41.6928|41.7057|41.7441|41.8634|41.9032|41.9032|41.9032|41.78|0.109695|0.012033|0.292478|-2.33333
Boylam|FieldType.DECIMAL|-87.6764|-87.6043|10.0|7.0|3.0|0.7|0.0|0.0|-87.6764|-87.6764|-87.6764|-87.6734|-87.6645|-87.6194|-87.6043|-87.6043|-87.6043|-87.6484|0.0386264|0.001492|0.344429|-2.33333
Location|FieldType.STRING||(41.903206037, -87.676361925)|10.0|0.0|10.0|0.0|0.0|7.0||||||||||||||

## <a name="impute-missing-values"></a>Eksik değerleri impute

Veri kümeleri, null değerler, NaN'ın ve içerik içeren değerleri eksik değerler olarak kabul edilir. Bu makine öğrenimi Modellerinizi performansı etkiler veya geçersiz bir sonuca yol açabilir. İmputation eksik değerleri ele almak için ortak bir yaklaşım ise ve yüksek yüzdesi eksik olduğunda kullanışlıdır değeri yalnızca silinemiyor kaydeder.

Önceki bölümde oluşturulan veri kümesi profilinden olduğunu görebiliriz `Latitude` ve `Longitude` sütunlarına sahip eksik değerleri yüksek yüzdesi. Bu örnekte, ortalamasını hesaplamak ve bu iki sütun için eksik değerleri impute.

İlk olarak, en son tanım ile veri kümesinin alın [ `get_definition()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-definition-version-id-none-) ve verileri ile Karşılaştır [ `keep_columns()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#keep-columns-columns--multicolumnselection-----azureml-dataprep-api-dataflow-dataflow), biz yalnızca adresine istediğimiz sütunları görüntüleme.

```Python
from azureml.core.dataset import Dataset
import azureml.dataprep as dprep

# get the latest definition of Dataset
ds_def = dataset.get_definition()

# keep useful columns for this example
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
ds_def.head(3)
```

||Kimlik|Arrest| Enlem|Boylam|
-|---------|-----|---------|----------|
|0|10498554|False|41.692834|-87.604319|
|1|10516598|False| 41.744107 |-87.664494|
|2|10519196|False| NaN|NaN|

Ardından, kontrol `MEAN` enlem kullanarak sütun değeri [ `summarize()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#summarize-summary-columns--typing-union-typing-list-azureml-dataprep-api-dataflow-summarycolumnsvalue---nonetype----none--group-by-columns--typing-union-typing-list-str---nonetype----none--join-back--bool---false--join-back-columns-prefix--typing-union-str--nonetype----none-----azureml-dataprep-api-dataflow-dataflow) işlevi. Bu işlev dizi sütunların kabul `group_by_columns` toplama düzeyini belirtmek için parametre. `summary_columns` Parametreyi kabul eden `SummaryColumnsValue` işlevin yeni hesaplanan alan adı geçerli sütun adını belirtir ve `SummaryFunction` gerçekleştirilecek.

```Python
lat_mean = ds_def.summarize(group_by_columns = ['Arrest'],
                            summary_columns = [dprep.SummaryColumnsValue(column_id = 'Latitude',
                                                                         summary_column_name = 'Latitude_MEAN',
                                                                         summary_function = dprep.SummaryFunction.MEAN)])
lat_mean = lat_mean.filter(lat_mean['Arrest'] == False)
lat_mean.head(1)
```

||Arrest|Latitude_MEAN|
--|-----|--------|
|0|False|41.780049|

Biz impute için değerleri doğruladıktan sonra kullanmak [ `ImputeMissingValuesBuilder` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py) ya da bir hesaplanmış sütunları imputes sabit bir ifade öğrenmek `MIN`, `MAX`, `MEAN` değeri veya `CUSTOM` değeri. Zaman `group_by_columns` belirtilirse, eksik değerler imputed grubuyla tarafından `MIN`, `MAX`, ve `MEAN` grubuna göre hesaplanır.

[ `ImputeColumnArguments` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputecolumnarguments?view=azure-dataprep-pyfunction) Bir column_id dizesini kabul eder ve bir `ReplaceValueFunction` impute türünü belirtin. Eksik boylam değerini, dış bilgilere dayanan-87 impute.

```Python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id = 'Latitude',
                                          impute_function = dprep.ReplaceValueFunction.MEAN)

# impute with custom value -87
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=-87)
```

İmpute adımları zincirleme yapılabilir birlikte içine bir `ImputeMissingValuesBuilder` kullanarak nesne [ `Builders` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py) sınıfı işlevi [ `impute_missing_values()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py#impute-missing-values-impute-columns--typing-list-azureml-dataprep-api-builders-imputecolumnarguments----none--group-by-columns--typing-union-typing-list-str---nonetype----none-----azureml-dataprep-api-builders-imputemissingvaluesbuilder). `impute_columns` Parametreyi kabul eden bir dizi `ImputeColumnArguments` nesneleri.

```Python
# get instance of ImputeMissingValuesBuilder
impute_builder = ds_def.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
```

Çağrı [ `learn()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#learn------none) impute adımları depolamak için işlev ve bunları kullanarak bir veri akışı nesne geçerli [ `to_dataflow()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#to-dataflow------azureml-dataprep-api-dataflow-dataflow).

```Python
impute_builder.learn()
ds_def = impute_builder.to_dataflow()
ds_def.head(3)
```

Çıktı aşağıdaki tabloda gösterildiği gibi eksik enlem ile imputed `MEAN` değerini `Arrest==False` grubu ve eksik boylam ile-87 imputed.

||Kimlik|Arrest|Enlem|Boylam
-|---------|-----|---------|----------
0|10498554|False|41.692834|-87.604319
1|10516598|False|41.744107|-87.664494
2|10519196|False|41.780049|-87.000000

Veri kümesi tanımı, güncelleştirme [ `update_definition()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#update-definition-definition--definition-update-message-) gerçekleştirilen dönüştürme adımı korumak için.

```Python
dataset = dataset.update_definition(ds_def, 'Impute Missing')
dataset.head(3)
```

||Kimlik|Arrest|Enlem|Boylam
-|---------|-----|---------|----------
0|10498554|False|41.692834|-87.604319
1|10516598|False|41.744107|-87.664494
2|10519196|False|41.780049|-87.000000

## <a name="create-assertion-rules"></a>Onaylama kuralları oluşturma

Sık sık veri ile temizlerken çalışıyoruz ve verileri hazırlama yalnızca üretim için ihtiyacımız olan toplam veriler alt kümesidir. Sonuç olarak, bazı bizim temizleme işleminin bir parçası vermiyoruz varsayımların false olarak duyuyor olabilir. Örneğin, sürekli olarak güncelleştirilen bir veri kümesi, başlangıçta yalnızca numaraları belirli bir aralıkta bulunan bir sütun geniş bir sonraki yürütmeleri değerleri içerebilir. Bu hatalar genellikle bozuk işlem hatları veya bozuk veri neden.

İşlem hattı yürütme sırasında hangi değerlendirilir veri onaylar oluşturma veri kümeleri desteği. Bu bir onayları bize bizim varsayımlar veri çubuğunda doğru devam ettiğini doğrulayın ve belirtilmediğinde, buna göre hatalarını işlemek için.

Örneğin, kısıtlamak istediğiniz `Latitude` ve `Longitude` kümenize belirli Sayısal aralıklar, değerler [ `assert_value()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#assert-value-columns--multicolumnselection--expression--azureml-dataprep-api-expressions-expression--policy--azureml-dataprep-api-engineapi-typedefinitions-assertpolicy----assertpolicy-errorvalue--1---error-code--str----assertionfailed------azureml-dataprep-api-dataflow-dataflow) yöntemi, bu, her zaman çalışması sağlar.

```Python
from azureml.dataprep import value
from azureml.core.dataset import Dataset

# get the latest definition of the Dataset
ds_def = dataset.get_definition()

# set assertion rules for `Latitude` and `Longitude` columns
ds_def = ds_def.assert_value('Latitude', (value <= 90) & (value >= -90), error_code='InvalidLatitude')
ds_def = ds_def.assert_value('Longitude', (value <= 180) & (value >= -87), error_code='InvalidLongitude')

ds_def.get_profile()
```

||Tür|Min|Maks|Sayı|Eksik sayısı|Sayısı eksik|Eksik yüzde|Hata sayısı|Boş sayısı|% 0,1 Quantile|%1 Quantile|%5 Quantile|% 25 Quantile|% 50 Quantile|% 75 Quantile|% 95 Quantile|% 99 Quantile|% 99,9 Quantile|Ortalama|Standart Sapma|Varyans|Komutunu|Basıklık
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
Kimlik|FieldType.INTEGER|1.04986e + 07|1.05351e + 07|10.0|0.0|10.0|0.0|0.0|0.0|1.04986e + 07|1.04992e + 07|1.04986e + 07|1.05166e + 07|1.05209e + 07|1.05259e + 07|1.05351e + 07|1.05351e + 07|1.05351e + 07|1.05195e + 07|12302.7|1.51358e + 08|-0.495701|-1.02814
Arrest|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Enlem|FieldType.DECIMAL|41.6928|41.9032|10.0|0.0|10.0|0.0|0.0|0.0|41.6928|41.7185|41.6928|41.78|41.78|41.78|41.9032|41.9032|41.9032|41.78|0.0517107|0.002674|0.837593|1,05
Boylam|FieldType.INTEGER|-87|-87|10.0|0.0|10.0|0.0|3.0|0.0|-87|-87|-87|-87|-87|-87|-87|-87|-87|-87|0|0|NaN|NaN

Profilden görürsünüz `Error Count` için `Longitude` 3 sütundur. Aşağıdaki kod, veri kümesini filtreler, hatayı alır ve hangi değerin onaylama başarısız olmasına neden olan görür. Buradan, kodunuzu ayarlamak ve verilerinizi uygun şekilde temizler.

```Python
from azureml.dataprep import col

ds_error = ds_def.filter(col('Longitude').is_error())
error = ds_error.head(10)['Longitude'][0]

print(error.originalValue)
```

    -87.60431945

## <a name="derive-columns-by-example"></a>Sütunları örneğe göre türetme

Veri kümeleri için daha gelişmiş araçlara istenen sonuçları örneklerini kullanarak sütunları türetme olanağını biridir. Bu, hedeflenen dönüştürmeleri elde etmek için kod oluşturabilmek SDK'sı bir örnek vermek sağlar.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')
dataset.head(3)
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|...|
-|---------|-----|---------|----|---
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|...
1|10516598|HZ258664|2016-04-15 17:00:00|082XX S MARSHFIELD AVE|...
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO AVE|...

2016 ' tarih ve saat biçimini dönüştürmek ihtiyacınız olmadığını düşünelim-04-04 10 PM - 12: 00 '. İçinde [ `derive_column_by_example()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#derive-column-by-example-source-columns--sourcecolumns--new-column-name--str--example-data--exampledata-----azureml-dataprep-api-dataflow-dataflow) bağımsız değişkeni, istenen çıkışınızda örnekleri sağlamak `example_data` parametre şu biçimde: *(özgün çıkış, istenen çıkış)*.

Aşağıdaki kod iki istenen çıkış örnekleri sağlar ("2016-04-04 23:56:00", "2016-04-04 10 PM-12 AM") ve ("2016-04-15 17:00:00", "2016-04-15 4 PM - 18: 00")

```Python
ds_def = dataset.get_definition()
ds_def = ds_def.derive_column_by_example(
        source_columns = "Date",
        new_column_name = "Date_Time_Range",
        example_data = [("2016-04-04 23:56:00", "2016-04-04 10PM-12AM"), ("2016-04-15 17:00:00", "2016-04-15 4PM-6PM")]
    )
ds_def.keep_columns(['ID','Date','Date_Time_Range']).head(3)
```

Aşağıdaki tabloda, yeni bir sütun kayıtları belirtilen biçimde Date_Time_Range içerdiğine dikkat edin.

||Kimlik|Date|Date_Time_Range
-|--------|-----|----
0|10498554|2016-04-04 23:56:00|2016-04-04 10 PM-12 AM'DEN
1|10516598|2016-04-15 17:00:00|2016-04-15 4 PM - 18: 00
2|10519196|2016-04-15 10:00:00|2016-04-15 10: 00 - 12 PM

```Python
# update Dataset definition to keep the transformation steps performed.
dataset = dataset.update_definition(ds_def, 'Derive Date_Time_Range')
```

## <a name="fuzzy-groupings"></a>Belirsiz gruplandırmaları

Farklı kaynaklardan gelen verileri toplamanıza, yazım denetimi, büyük/küçük harf ya da aynı varlık kısaltmalar farklılığı karşılaşabilirsiniz. Otomatik olarak standart hale getirmek ve bu çeşitleri SDK'ın belirsiz gruplandırma veya metin kümeleme işlevselliğiyle mutabık kılma.

Örneğin, sütun `inspections.business.city` şehir adı "İstanbul" birçok formları içerir.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local json file
dataset = Dataset.auto_read_files('./data/city.json')
dataset.head(5)
```

||inspections.Business.business_id|inspections.business_name|inspections.Business.address|inspections.Business.City|...|
-|-----|-------------------------|------------|--|---
0|16162|Hızlı N Ezee Hindistan Foods|3861 24th St|SF|...
1|67565|Tay dili, Kıng Cafe Noodles|1541 TARAVAL St|SAN FRANCISCO|...
2|67565|Tay dili, Kıng Cafe Noodles|1541 TARAVAL St|SAN FRANCISCO|...
3|68701|Grindz|832 Clement St|SF|...
4|69186|Premier yemek & olayları, Inc.|1255 22nd St|S.F.|...

Kullanalım [ `fuzzy_group_column()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#fuzzy-group-column-source-column--str--new-column-name--str--similarity-threshold--float---0-8--similarity-score-column-name--str---none-----azureml-dataprep-api-dataflow-dataflow) "San Francisco" otomatik olarak algılanan kurallı biçimi içeren bir sütun eklemek için yöntemi. Bağımsız değişkenler `source_column` ve `new_column_name` gereklidir. Ayrıca, için seçeneğiniz de vardır:

* Yeni bir sütun oluşturun `similarity_score_column_name`, her bir özgün ve kurallı değer çiftinin arasında benzerlik puanı gösterir.

* Sağlayan bir `similarity_threshold`, birlikte gruplanması değerleri için en az benzerlik puanın olduğu.

```Python
# get the latest Dataset definition
ds_def = dataset.get_definition()
ds_def = ds_def.fuzzy_group_column(source_column='inspections.business.city',
                                       new_column_name='city_grouped',
                                       similarity_threshold=0.8,
                                       similarity_score_column_name='similarity_score')
ds_def.head(5)
```

||inspections.Business.business_id|inspections.business_name|inspections.Business.address|inspections.Business.City|city_grouped|similarity_score|...|
-|-----|-------------------------|------------|--|---|---|---
0|16162|Hızlı N Ezee Hindistan Foods|3861 24th St|SF|San Francisco|0.814806|...
1|67565|Tay dili, Kıng Cafe Noodles|1541 TARAVAL St|SAN FRANCISCO|San Francisco|1.000000|...
2|67565|Tay dili, Kıng Cafe Noodles|1541 TARAVAL St|SAN FRANCISCO|San Francisco|1.000000|...
3|68701|Grindz|832 Clement St|SF|San Francisco|0.814806|...
4|69186|Premier yemek & olayları, Inc.|1255 22nd St|S.F.|San Francisco|0.814806|...

Elde edilen veri kümesi tanımında, aynı dize, "San Francisco", "San Francisco" verileri temsil eden tüm farklı çeşitlemeleri normalleştirilmiş.

Bu en yeni veri kümesi tanımıyla belirsiz gruplandırma adımla Kaydet `update_definition()`.

```Python
dataset = dataset.update_definition(ds_def, 'fuzzy grouping')
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümesi tanımları yaşam döngüsünü yönetme](how-to-manage-dataset-definitions.md).

* Otomatik makine öğrenimi bkz [öğretici](tutorial-auto-train-models.md) bir regresyon modeli örnek.

* Bkz: SDK [genel bakış](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) tasarım desenleri ve kullanım örnekleri için.

* Veri kümelerini kullanan bir örnek için bkz: [örnek not defterleri](https://aka.ms/dataset-tutorial).
