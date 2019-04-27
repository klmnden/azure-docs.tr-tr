---
title: "Dönüşümleri: Python SDK'sı veri hazırlama"
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning veri hazırlığı SDK'sı ile verileri temizleme ve dönüştürme hakkında bilgi edinin. Sütun ekleme, istenmeyen satırları veya sütunları filtrelemek ve eksik değerleri impute dönüştürme yöntemleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d2bd271557ae0deefeb12a2dc7343c46fbd35363
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817551"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning veri hazırlığı SDK'sı ile verileri dönüştürün

Bu makalede, Azure Machine Learning veri hazırlığı SDK'sını kullanarak verileri dönüştürme farklı yöntemleri öğrenin. SDK'sı eksik değerleri impute sütunlar eklemek ve istenmeyen satırları veya sütunları filtrelemek basit hale getirmek işlevleri sunar. SDK için başvuru belgeleri görmek için bkz: [genel bakış](https://aka.ms/data-prep-sdk).

Bu nasıl yapılır örnekler için aşağıdaki görevleri gösterir:

- Bir ifade kullanarak sütun ekleme
- [Eksik değerleri impute](#impute-missing-values)
- [Sütunu örneğe göre türetme](#derive-column-by-example)
- [Filtreleme](#filtering)
- [Özel bir Python dönüşümler](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Bir ifade kullanarak sütun ekleme

Azure Machine Learning veri hazırlığı SDK içerir `substring` mevcut sütunlar arasında bir değer hesaplamak için kullanın ve ardından yeni bir sütun değeri put ifadeler. Bu örnekte, veri yükleme ve sütunları, giriş verilerini eklemeyi deneyin.

```python
import azureml.dataprep as dprep

# loading data
dflow = dprep.read_csv(path=r'data\crime0-10.csv')
dflow.head(3)
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|Yurt içi|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|07/05/2015 11:50:00 PM|050XX N NEWLAND KAYDET|0820|HIRSIZLIĞI|500 $ VE ALTINDA|SOKAK|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 12:42:46 PM|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|07/05/2015 11:30:00 PM|011XX W MORSE KAYDET|0460|PİL|BASİT|SOKAK|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|07/05/2015 11:20:00 PM|121XX S ÖN KAYDET|0486|PİL|BASİT YURTİÇİ PİL|SOKAK|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|


Kullanım `substring(start, length)` önek çalışması sayı sütunu çıkarmak ve yeni bir sütunda, dize koymak için ifade `Case Category`. Geçirme `substring_expression` değişkenini `expression` parametresi ifadesi her kayıtta yürüten yeni bir hesaplanmış sütun oluşturur.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||Kimlik|Büyük/küçük harf numarası|Durum kategorisi|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|Yurt içi|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|FİSİ|07/05/2015 11:50:00 PM|050XX N NEWLAND KAYDET|0820|HIRSIZLIĞI|500 $ VE ALTINDA|SOKAK|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 12:42:46 PM|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|FİSİ|07/05/2015 11:30:00 PM|011XX W MORSE KAYDET|0460|PİL|BASİT|SOKAK|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|FİSİ|07/05/2015 11:20:00 PM|121XX S ÖN KAYDET|0486|PİL|BASİT YURTİÇİ PİL|SOKAK|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|



Kullanım `substring(start)` ifade yalnızca sayı çalışması sayı sütunu ayıklayın ve yeni bir sütun oluşturun. Kullanarak bir sayısal veri türü dönüştürme `to_number()` işlev ve dize sütun adı bir parametre olarak geçiriyoruz.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
```

## <a name="impute-missing-values"></a>Eksik değerleri impute

SDK'sı eksik değerleri belirtilen sütunlardaki impute. Bu örnekte, enlem ve boylam değerleri yükleme ve giriş verilerini eksik değerleri impute deneyin.

```python
import azureml.dataprep as dprep

# loading input data
dflow = dprep.read_csv(r'data\crime0-10.csv')
dflow = dflow.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
dflow = dflow.to_number(['Latitude', 'Longitude'])
dflow.head(3)
```

||Kimlik|Arrest|Enlem|Boylam|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|

Üçüncü kayıt enlem ve boylam değerleri eksik. Bu eksik değerleri impute için kullanmanız [ `ImputeMissingValuesBuilder` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py) sabit bir ifade öğrenin. Ya da bir hesaplanmış sütunları impute `MIN`, `MAX`, `MEAN` değeri veya `CUSTOM` değeri. Zaman `group_by_columns` belirtilirse, eksik değerler imputed grubuyla tarafından `MIN`, `MAX`, ve `MEAN` grubuna göre hesaplanır.

Denetleme `MEAN` enlem kullanarak sütun değeri [ `summarize()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#summarize-summary-columns--typing-union-typing-list-azureml-dataprep-api-dataflow-summarycolumnsvalue---nonetype----none--group-by-columns--typing-union-typing-list-str---nonetype----none--join-back--bool---false--join-back-columns-prefix--typing-union-str--nonetype----none-----azureml-dataprep-api-dataflow-dataflow) işlevi. Bu işlev dizi sütunların kabul `group_by_columns` toplama düzeyini belirtmek için parametre. `summary_columns` Parametreyi kabul eden bir `SummaryColumnsValue` çağırın. Bu işlev çağrısı geçerli sütun adı olan yeni hesaplanan alan adı belirtir ve `SummaryFunction` gerçekleştirilecek.

```python
dflow_mean = dflow.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
dflow_mean = dflow_mean.filter(dprep.col('Arrest') == 'false')
dflow_mean.head(1)
```

||Arrest|Latitude_MEAN|
|-----|-----|----|
|0|false|41.878961|

`MEAN` Latitudes değerini doğru görünüyor, kullanın `ImputeColumnArguments` bunu impute için işlevi. Bu işlev kabul eden bir `column_id` dize ve `ReplaceValueFunction` impute türü belirtmek için. Eksik boylam değerini, dış bilgilere dayanan 42 impute.

İmpute adımları zincirleme yapılabilir birlikte içine bir `ImputeMissingValuesBuilder` oluşturucu işlevi kullanarak nesne `impute_missing_values()`. `impute_columns` Parametreyi kabul eden bir dizi `ImputeColumnArguments` nesneleri. Çağrı `learn()` impute adımları depolamak için işlev ve kullanarak bir veri akışı nesnesi daha sonra uygulamanızı `to_dataflow()`.

```python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id='Latitude',
                                          impute_function=dprep.ReplaceValueFunction.MEAN)
# impute with custom value 42
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=42)
# get instance of ImputeMissingValuesBuilder
impute_builder = dflow.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])

impute_builder.learn()
dflow_imputed = impute_builder.to_dataflow()
dflow_imputed.head(3)
```

||Kimlik|Arrest|Enlem|Boylam|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|

Sonuçta yukarıda gösterildiği gibi eksik enlem ile imputed `MEAN` değerini `Arrest=='false'` grubu. Eksik boylam ile 42 imputed.

```python
imputed_longitude = dflow_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Sütunu örneğe göre türetme

Azure Machine Learning veri hazırlığı SDK'sı daha gelişmiş araçlar istenen sonuçları örneklerini kullanarak sütunları türetme olanağını biridir. Bu, hedeflenen dönüşümü elde etmek için kod oluşturabilmek SDK'sı bir örnek vermek sağlar.

```python
import azureml.dataprep as dprep
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dflow.head(4)
```

||DATE|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00|FM-12|22|50|10|
|2|1/1/2015 1:54|FM-15|22|50|10|
|3|1/1/2015 2:54|FM-15|22|50|11|

Tarih ve saat olduğu bir biçimde bu dosyayı bir veri kümesiyle katılmak gerektiğini varsayar ' 10 Mart 2018'den | 02: 00 - 04: 00 '.

```python
builder = dflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=dflow.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview(count=5) 
```

||DATE|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|1 Ocak 2015 12: 00 - 02: 00|
|1|1/1/2015 1:00|1 Ocak 2015 12: 00 - 02: 00|
|2|1/1/2015 1:54|1 Ocak 2015 12: 00 - 02: 00|
|3|1/1/2015 2:54|1 Ocak 2015 02: 00 - 04: 00|
|4|1/1/2015 3:54|1 Ocak 2015 02: 00 - 04: 00|

Yukarıdaki kod, ilk türetilmiş sütun için bir oluşturucu oluşturur. Kaynak sütunlar dikkate alınması gereken bir dizi sağlayın (`DATE`), yeni bir sütun eklemek için bir ad. İlk örnek olarak ikinci satırında (dizin 1) geçirin ve türetilmiş sütunu için beklenen bir değer verin.

Son olarak, çağrı `builder.preview(skip=30, count=5)` kaynak sütunun yanındaki türetilmiş sütuna görebilirsiniz. Biçimi doğru görünüyor, ancak yalnızca aynı tarih için "1 Ocak 2015" değerleri görebilirsiniz.

Şimdi, istediğiniz satır sayısını geçirin `skip` satırları görmek için üst birazcık daha indiğimde.

> [!NOTE]
> Preview() işlevi satırları atlar, ancak çıktı dizini yeniden sayı değil. Aşağıdaki örnekte, ' % s'dizini 0 tabloda veri akışı 30 dizinde karşılık gelir.

```python
builder.preview(skip=30, count=5)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|1 Ocak 2015 10 PM - 12: 00|
|1|1/1/2015 23:54|1 Ocak 2015 10 PM - 12: 00|
|2|1/1/2015 23:59|1 Ocak 2015 10 PM - 12: 00|
|3|1/2/2015 0:54|1 Şubat 2015 12: 00 - 02: 00|
|4|1/2/2015 1:00|1 Şubat 2015 12: 00 - 02: 00|

Burada oluşturulan program ile ilgili bir sorun görürsünüz. Yalnızca yukarıda sağlanan bir örneğe bağlı olarak, bu durumda istediklerinizi değil olduğu "Gün/ay/yıl" olarak bir tarihi ayrıştırmak türet program seçtiniz. Bu sorunu düzeltmek için belirli bir kayıt dizinini hedef ve kullanarak başka bir örnek sağlar `add_example()` işlevini `builder` değişkeni.

```python
builder.add_example(source_data=dflow.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=5)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|1 Ocak 2015 10 PM - 12: 00|
|1|1/1/2015 23:54|1 Ocak 2015 10 PM - 12: 00|
|2|1/1/2015 23:59|1 Ocak 2015 10 PM - 12: 00|
|3|1/2/2015 0:54|2 Ocak 2015 12: 00 - 02: 00|
|4|1/2/2015 1:00|2 Ocak 2015 12: 00 - 02: 00|

Satırları doğru bir şekilde işlemek artık ' 1/2/2015' olarak '2 Ocak 2015', ancak ötesinde bakarsanız dizin türetilmiş sütun 76, sonunda değerlere hiçbir şey türetilmiş sütununa sahip görürsünüz.

```python
builder.preview(skip=75, count=5)
```


||DATE|date_timerange|
|-----|-----|-----|
|0|1/3/2015 7:00|3 Ocak 2015 6 AM - 8: 00|
|1|1/3/2015 7:54|3 Ocak 2015 6 AM - 8: 00|
|2|1/29/2015 6:54|None|
|3|1/29/2015 7:00|None|
|4|1/29/2015 7:54|None|

```python
builder.add_example(source_data=dflow.iloc[77], example_value='Jan 29, 2015 6AM-8AM')
builder.preview(skip=75, count=5)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/3/2015 7:00|3 Ocak 2015 6 AM - 8: 00|
|1|1/3/2015 7:54|3 Ocak 2015 6 AM - 8: 00|
|2|1/29/2015 6:54|29 Ocak 2015 6 AM - 8: 00|
|3|1/29/2015 7:00|29 Ocak 2015 6 AM - 8: 00|
|4|1/29/2015 7:54|29 Ocak 2015 6 AM - 8: 00|

 Geçerli örnek türetme arama listesini görmek için `list_examples()` Oluşturucu nesne üzerinde.

```python
examples = builder.list_examples()
```

| |DATE|Örnek|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|1 Ocak 2015 12: 00 - 02: 00|-1|
|1|1/2/2015 0:54|2 Ocak 2015 12: 00 - 02: 00|-2|
|2|29/1/2015 20:54|29 Ocak 2015'te 8-10 PM|-3|


Yanlış örnek silmek istiyorsanız bazı durumlarda ya da geçirebilirsiniz `example_row` pandas DataFrame, gelen veya `example_id` değeri. Örneğin, çalıştırdığınız `builder.delete_example(example_id=-1)`, ilk dönüştürme örnek siler.


Çağrı `to_dataflow()` eklenen istenen türetilmiş sütunlar bir veri akışı döndürür Oluşturucusu '.

```python
dflow = builder.to_dataflow()
df = dflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filtering

SDK'sı yöntemleri içerir [ `drop_columns()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#drop-columns-columns--multicolumnselection-----azureml-dataprep-api-dataflow-dataflow) ve [ `filter()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py) izin verecek şekilde satırları veya sütunları filtreleyin.

### <a name="initial-setup"></a>Başlangıç kurulumu

```python
import azureml.dataprep as dprep
from datetime import datetime
dflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|N|1|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|N|5|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|.00|0|0|3.25|

### <a name="filtering-columns"></a>Sütunları filtreleme

Sütunları filtrelemek, kullanın `drop_columns()`. Bu yöntem bırakmak sütun listesini alır veya daha karmaşık bir bağımsız değişken olarak adlandırılan [ `ColumnSelector` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.columnselector?view=azure-dataprep-py).

#### <a name="filtering-columns-with-list-of-strings"></a>Sütunları içeren bir dize listesi filtreleme

Bu örnekte, `drop_columns()` dizeleri listesini alır. Her bir dizenin bırakmak istediğiniz sütunu tam olarak eşleşmelidir.

```python
dflow = dflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|0|0|0|0|1|.00|0|0|21,25|

#### <a name="filtering-columns-with-regex"></a>Regex sütunlarla filtreleme

Alternatif olarak, `ColumnSelector` ifade normal bir ifadeyle eşleşen sütunları kaldırın. Bu örnekte, bir ifadeyle eşleşen tüm sütunlar bırak `Column*|.*longitude|.*latitude`.

```python
dflow = dflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|1|.00|0|0|21,25|

## <a name="filtering-rows"></a>Filtre satırları

Satırları filtrele, kullanın `filter()`. Bu yöntem, Azure Machine Learning veri hazırlığı SDK'sı ifade bir bağımsız değişken olarak alır ve yeni bir veri akışı ile ifade True olarak değerlendirilen satırları döndürür. İfadeleri, ifade oluşturucular kullanılarak oluşturulur (`col`, `f_not`, `f_and`, `f_or`) ve normal işleçleri (>, <>, =, < =, ==,! =).

### <a name="filtering-rows-with-simple-expressions"></a>Basit ifadelerle satırları filtreleme

İfade Oluşturucu kullanın `col`, bir dize bağımsız değişkeni bir sütun adı belirtin `col('column_name')`. Bu ifade aşağıdaki standart işleçlerden ile birlikte kullanma >, <>, =, < =, ==,! gibi bir ifade oluşturmak için = `col('Tip_amount') > 0`. Son olarak, yerleşik ifadesine geçirmek `filter()` işlevi.

Bu örnekte, `dflow.filter(col('Tip_amount') > 0)` sütunları içeren yeni bir veri akışı döndüren değerini `Tip_amount` 0'dan büyük.

> [!NOTE] 
> `Tip_amount` ilk karşı diğer sayısal değerleri karşılaştırma bir ifade oluşturmak sağlıyor sayısal, dönüştürülür.

```python
dflow = dflow.to_number(['Tip_amount'])
dflow = dflow.filter(dprep.col('Tip_amount') > 0)
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|.00|0.08|0|4.58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24|1|.00|0.30|0|3.8|

### <a name="filtering-rows-with-complex-expressions"></a>Karmaşık ifadeleri ile satırları filtreleme

Karmaşık ifadeleri kullanarak filtre uygulamak için ifade oluşturuculara sahip bir veya daha fazla basit ifadeler birleştirmek `f_not`, `f_and`, veya `f_or`.

Bu örnekte, `dflow.filter()` sütunları içeren yeni bir veri akışı döndürür burada `'Passenger_count'` 5'ten az olan ve `'Tolls_amount'` 0'dan büyük.

```python
dflow = dflow.to_number(['Passenger_count', 'Tolls_amount'])
dflow = dflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08 12:16:00|2013-08-08 12:16:00|1.0|.00|2.25|5.00|19.75|
|1|2013-08-12 14:43:53|2013-08-12 15:04:50|1.0|5,28|6.46|5.33|32.29|

İç içe geçmiş bir ifade oluşturmak için birden fazla ifade oluşturucu birleştirme satırları filtrele da mümkündür.

> [!NOTE]
> `lpep_pickup_datetime` ve `Lpep_dropoff_datetime` kurmamızı diğer datetime değerleri karşı karşılaştırma bir ifade oluşturmak tarih saat önce dönüştürülür.

```python
dflow = dflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dflow = dflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dflow = dflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dflow.head(2)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013 08 13 06:11:06 + 00:00|2013 08 13 06:30:28 + 00:00|1.0|9,57|7.47|5.33|44.80|
|1|2013-08-23 12:28:20 + 00:00|2013-08-23 12:50:28 + 00:00|2.0|8.22|8.08|5.33|40.41|

## <a name="custom-python-transforms"></a>Özel bir Python dönüşümler

Her zaman olmayacaktır senaryoları dönüştürme yapmak için en kolay seçenek kendi betik yazarken. SDK, özel bir Python betikleri için kullanabileceğiniz üç uzantı noktaları sağlar.

- Yeni bir betik sütun
- Yeni betik filtresi
- Bölüm dönüştürme

Her uzantısı hem ölçek büyütme ve ölçek genişletme çalışma zamanı içinde desteklenir. Bu uzantı noktaları kullanmanın önemli bir avantajı, tüm verileri bir veri çerçevesi oluşturmak için çekme gerekmez ' dir. Kodu yalnızca çalışır, özel bir Python dönüştürme, uygun ölçekte, bölüm ve genellikle paralel ister.

### <a name="initial-data-preparation"></a>İlk veri hazırlama

Bazı verileri Azure Blobundan yükleyerek başlayın.

```python
import azureml.dataprep as dprep
col = dprep.col

dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
dflow.head(2)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|
|0|ALABAMA|1|101710|Hale ilçe|10171002158| |
|1|ALABAMA|1|101710|Hale ilçe|10171002162| |

Veri kümesinin trim ve sütunları kaldırma, değerleri değiştirerek ve türlerini dönüştürme gibi bazı temel dönüşümler yapın.

```python
dflow = dflow.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
dflow = dflow.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
dflow = dflow.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
dflow.head(2)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|
|0|ALABAMA|Hale ilçe|1.017100e + 10|None|
|1|ALABAMA|Hale ilçe|1.017100e + 10|None|

Null değerler aşağıdaki filtre kullanarak arayın.

```python
dflow.filter(col('MAM_MTH00numvalid_1011').is_null()).head(2)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|
|0|ALABAMA|Hale ilçe|1.017100e + 10|None|
|1|ALABAMA|Hale ilçe|1.017100e + 10|None|

### <a name="transform-partition"></a>Bölüm dönüştürme

Kullanım [ `transform_partition()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#transform-partition-script--str-----azureml-dataprep-api-dataflow-dataflow) tüm null değerleri 0 ile değiştirilecek. Bu kod, tek seferde tüm veri kümesi üzerinde bir bölümü tarafından çalıştırılır. Başka bir deyişle, bir büyük veri kümesini temel çalışma zamanı tarafından bölüm bölüm veri işlerken paralel olarak bu kod çalıştırabilir.

Python betiğini çağrılan bir işlev tanımlamalıdır `transform()` iki bağımsız değişkeni almayan `df` ve `index`. `df` Bağımsız değişken bölümü için veri içeren bir pandas dataframe olacaktır ve `index` değişkendir bölümünün benzersiz bir tanımlayıcı. Dönüştürme işlevi tam olarak geçirilen veri çerçevesi düzenleyebilirsiniz ancak bir dataframe döndürmelidir. Python betiğini içe aktaran tüm kitaplıkları, veri akışı çalıştırıldığı ortama mevcut olması gerekir.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(2)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|
|0|ALABAMA|Hale ilçe|1.017100e + 10|0.0|
|1|ALABAMA|Hale ilçe|1.017100e + 10|0.0|

### <a name="new-script-column"></a>Yeni bir betik sütun

Ülke adı ve durum adı olan yeni bir sütun oluşturmak için ve eyalet adı harf yapılacak bir Python betiği kullanabilirsiniz. Bunu yapmak için [ `new_script_column()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#new-script-column-new-column-name--str--insert-after--str--script--str-----azureml-dataprep-api-dataflow-dataflow) veri akışı üzerinde yöntemi.

Python betiğini çağrılan bir işlev tanımlamalıdır `newvalue()` tek bir bağımsız değişken almayan `row`. `row` Bağımsız değişkeni olan bir, dict (`key`: sütun adı `val`: geçerli değer) ve veri kümesindeki her satır için bu işleve geçirilir. Bu işlev, yeni bir sütun kullanılacak bir değer döndürmesi gerekir. Python betiğini içe aktaran tüm kitaplıkları, veri akışı çalıştırıldığı ortama mevcut olması gerekir.

```python
dflow = dflow.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
dflow.head(2)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|0.0|
|1|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|0.0|

### <a name="new-script-filter"></a>Yeni betik filtresi

Python kullanarak ifade derleme [ `new_script_filter()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#new-script-filter-script--str-----azureml-dataprep-api-dataflow-dataflow) 'Hale' olduğu yeni satırlar için veri kümesini filtrelemek için `county_state` sütun. Bir ifade döndürür `True` satır tutmak istiyorsanız ve `False` satır bırakmak.

```python
dflow = dflow.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
dflow.head(2)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|0.0|

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: SDK [genel bakış](https://aka.ms/data-prep-sdk) tasarım desenleri ve kullanım örnekleri
* Azure Machine Learning veri hazırlığı SDK'sı bkz [öğretici](tutorial-data-prep.md) çözmenin belirli bir senaryo örneği
