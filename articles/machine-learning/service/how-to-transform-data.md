---
title: "Dönüşümleri: Python SDK'sı veri hazırlama"
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning veri hazırlığı SDK'sı ile verileri temizleme ve dönüştürme hakkında bilgi edinin. Sütun ekleme, istenmeyen satırları veya sütunları filtrelemek ve eksik değerleri impute dönüştürme yöntemleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d32244cd49ebd42192b2388215f79a64cacb3caa
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186149"
---
# <a name="transform-data-with-the-azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning veri hazırlığı SDK'sı ile verileri dönüştürün

Bu makalede, farklı yöntemler kullanarak verileri yükleme bilgi [Azure Machine Learning veri hazırlığı SDK'sı](https://aka.ms/data-prep-sdk). İşlevler sütunlar eklemek istenmeyen satırları veya sütunları filtrelemek basit hale getirmek ve eksik değerleri impute SDK'sı sunar.

Şu anda aşağıdaki görevler için işlev vardır:

- [Bir ifade kullanarak sütun ekleme](#column)
- [Eksik değerleri impute](#impute-missing-values)
- [Sütunu örneğe göre türetme](#derive-column-by-example)
- [Filtreleme](#filtering)
- [Özel bir Python dönüşümler](#custom-python-transforms)

## <a name="add-column-using-an-expression"></a>Bir ifade kullanarak sütun ekleme

Azure Machine Learning veri hazırlığı SDK içerir `substring` mevcut sütunlar arasında bir değer hesaplamak için kullanın ve ardından yeni bir sütun değeri put ifadeler. Bu örnekte, veri yükleme ve sütunları, giriş verilerini eklemeyi deneyin.

```python
import azureml.dataprep as dprep

# loading data
dataflow = dprep.read_csv(path=r'data\crime0-10.csv')
dataflow.head(3)
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|Yurt içi|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
|0|10140490|HY329907|07/05/2015 11:50:00 PM|050XX N NEWLAND KAYDET|0820|HIRSIZLIĞI|500 $ VE ALTINDA|SOKAK|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|07/05/2015 11:30:00 PM|011XX W MORSE KAYDET|0460|PİL|BASİT|SOKAK|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|07/05/2015 11:20:00 PM|121XX S ÖN KAYDET|0486|PİL|BASİT YURTİÇİ PİL|SOKAK|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|


Kullanım `substring(start, length)` önek çalışması sayı sütunu çıkarmak ve yeni bir sütunda, dize koymak için ifade `Case Category`. Geçirme `substring_expression` değişkenini `expression` parametresi ifadesi her kayıtta yürüten yeni bir hesaplanmış sütun oluşturur.

```python
substring_expression = dprep.col('Case Number').substring(0, 2)
case_category = dataflow.add_column(new_column_name='Case Category',
                                    prior_column='Case Number',
                                    expression=substring_expression)
case_category.head(3)
```

||Kimlik|Büyük/küçük harf numarası|Durum kategorisi|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|FİSİ|07/05/2015 11:50:00 PM|050XX N NEWLAND KAYDET|0820|HIRSIZLIĞI|500 $ VE ALTINDA|SOKAK|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|FİSİ|07/05/2015 11:30:00 PM|011XX W MORSE KAYDET|0460|PİL|BASİT|SOKAK|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|FİSİ|07/05/2015 11:20:00 PM|121XX S ÖN KAYDET|0486|PİL|BASİT YURTİÇİ PİL|SOKAK|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|



Kullanım `substring(start)` ifade yalnızca sayı çalışması sayı sütunu ayıklayın ve yeni bir sütun oluşturun. Kullanarak bir sayısal veri türü dönüştürme `to_number()` işlev ve dize sütun adı bir parametre olarak geçiriyoruz.

```python
substring_expression2 = dprep.col('Case Number').substring(2)
case_id = dataflow.add_column(new_column_name='Case Id',
                              prior_column='Case Number',
                              expression=substring_expression2)
case_id = case_id.to_number('Case Id')
case_id.head(3)
```

||Kimlik|Büyük/küçük harf numarası|Durum kimliği|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum|
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|------|
|0|10140490|HY329907|329907.0|07/05/2015 11:50:00 PM|050XX N NEWLAND KAYDET|0820|HIRSIZLIĞI|500 $ VE ALTINDA|SOKAK|false|false|...|41|10|06|1129230|1933315|2015|07/12/2015 |12:42:46 PM|41.973309466|-87.800174996|(41.973309466,-87.800174996)|
|1|10139776|HY329265|329265.0|07/05/2015 11:30:00 PM|011XX W MORSE KAYDET|0460|PİL|BASİT|SOKAK|false|true|...|49|1|08B|1167370|1946271|2015|07/12/2015 12:42:46 PM|42.008124017|-87.65955018|(42.008124017,-87.65955018)|
|2|10140270|HY329253|329253.0|07/05/2015 11:20:00 PM|121XX S ÖN KAYDET|0486|PİL|BASİT YURTİÇİ PİL|SOKAK|false|true|...|9|53|08B|||2015|07/12/2015 12:42:46 PM|

## <a name="impute-missing-values"></a>Eksik değerleri impute

SDK'sı eksik değerleri belirtilen sütunlardaki impute. Bu örnekte, enlem ve boylam değerleri yükleme ve giriş verilerini eksik değerleri impute deneyin.

```python
import azureml.dataprep as dprep

# loading input data
df = dprep.read_csv(r'data\crime0-10.csv')
df = df.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
df = df.to_number(['Latitude', 'Longitude'])
df.head(5)
```

||Kimlik|Arrest|Enlem|Boylam|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|NaN|NaN|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Üçüncü kayıt enlem ve boylam değerleri eksik. Bu eksik değerleri impute için kullanmanız `ImputeMissingValuesBuilder` sabit bir ifade öğrenin. Ya da bir hesaplanmış sütunları impute `MIN`, `MAX`, `MEAN` değeri veya `CUSTOM` değeri. Zaman `group_by_columns` belirtilirse, eksik değerler imputed grubuyla tarafından `MIN`, `MAX`, ve `MEAN` grubuna göre hesaplanır.

Denetleme `MEAN` enlem kullanarak sütun değeri `summarize()` işlevi. Bu işlev dizi sütunların kabul `group_by_columns` toplama düzeyini belirtmek için parametre. `summary_columns` Parametreyi kabul eden bir `SummaryColumnsValue` çağırın. Bu işlev çağrısı geçerli sütun adı olan yeni hesaplanan alan adı belirtir ve `SummaryFunction` gerçekleştirilecek.

```python
df_mean = df.summarize(group_by_columns=['Arrest'],
                       summary_columns=[dprep.SummaryColumnsValue(column_id='Latitude',
                                                                 summary_column_name='Latitude_MEAN',
                                                                 summary_function=dprep.SummaryFunction.MEAN)])
df_mean = df_mean.filter(dprep.col('Arrest') == 'false')
df_mean.head(1)
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
impute_builder = df.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
# call learn() to learn a fixed program to impute missing values
impute_builder.learn()
# call to_dataflow() to get a data flow with impute step added
df_imputed = impute_builder.to_dataflow()

# check impute result
df_imputed.head(5)
```

||Kimlik|Arrest|Enlem|Boylam|
|-----|------|-----|------|-----|
|0|10140490|false|41.973309|-87.800175|
|1|10139776|false|42.008124|-87.659550|
|2|10140270|false|41.878961|42.000000|
|3|10139885|false|41.902152|-87.754883|
|4|10140379|false|41.885610|-87.657009|

Sonuçta yukarıda gösterildiği gibi eksik enlem ile imputed `MEAN` değerini `Arrest=='false'` grubu. Eksik boylam ile 42 imputed.

```python
imputed_longitude = df_imputed.to_pandas_dataframe()['Longitude'][2]
assert imputed_longitude == 42
```

## <a name="derive-column-by-example"></a>Sütunu örneğe göre türetme

Azure Machine Learning veri hazırlığı SDK'sı daha gelişmiş araçlar istenen sonuçları örneklerini kullanarak sütunları türetme olanağını biridir. Bu, hedeflenen dönüşümü elde etmek için kod oluşturabilmek SDK'sı bir örnek vermek sağlar.

```python
import azureml.dataprep as dprep
dataflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/BostonWeather.csv')
dataflow.head(10)
```

||DATE|REPORTTPYE|HOURLYDRYBULBTEMPF|HOURLYRelativeHumidity|HOURLYWindSpeed|
|----|----|----|----|----|----|
|0|1/1/2015 0:54|FM-15|22|50|10|
|1|1/1/2015 1:00|FM-12|22|50|10|
|2|1/1/2015 1:54|FM-15|22|50|10|
|3|1/1/2015 2:54|FM-15|22|50|11|
|4|1/1/2015 3:54|FM-15|24|46|13|
|5|1/1/2015 4:00|FM-12|24|46|13|
|6|1/1/2015 4:54|FM-15|22|52|15|
|7|1/1/2015 5:54|FM-15|23|48|17|
|8|1/1/2015 6:54|FM-15|23|50|14|
|9|1/1/2015 7:00|FM-12|23|50|14|

Tarih ve saat olduğu bir biçimde bu dosyayı bir veri kümesiyle katılmak gerektiğini varsayar ' 10 Mart 2018'den | 02: 00 - 04: 00 '.

```python
builder = dataflow.builders.derive_column_by_example(source_columns=['DATE'], new_column_name='date_timerange')
builder.add_example(source_data=df.iloc[1], example_value='Jan 1, 2015 12AM-2AM')
builder.preview() 
```

||DATE|date_timerange|
|----|----|----|
|0|1/1/2015 0:54|1 Ocak 2015 12: 00 - 02: 00|
|1|1/1/2015 1:00|1 Ocak 2015 12: 00 - 02: 00|
|2|1/1/2015 1:54|1 Ocak 2015 12: 00 - 02: 00|
|3|1/1/2015 2:54|1 Ocak 2015 02: 00 - 04: 00|
|4|1/1/2015 3:54|1 Ocak 2015 02: 00 - 04: 00|
|5|1/1/2015 4:00|1 Ocak 2015 04: 00 - 6'da|
|6|1/1/2015 4:54|1 Ocak 2015 04: 00 - 6'da|
|7|1/1/2015 5:54|1 Ocak 2015 04: 00 - 6'da|
|8|1/1/2015 6:54|1 Ocak 2015 6 AM - 8: 00|
|9|1/1/2015 7:00|1 Ocak 2015 6 AM - 8: 00|

Yukarıdaki kod, ilk türetilmiş sütun için bir oluşturucu oluşturur. Kaynak sütunlar dikkate alınması gereken bir dizi sağlayın (`DATE`), yeni bir sütun eklemek için bir ad. İlk örnek olarak ikinci satırında (dizin 1) geçirin ve türetilmiş sütunu için beklenen bir değer verin.

Son olarak, çağrı `builder.preview()` kaynak sütunun yanındaki türetilmiş sütuna görebilirsiniz. Biçimi doğru görünüyor, ancak yalnızca aynı tarih için "1 Ocak 2015" değerleri görebilirsiniz.

Şimdi, istediğiniz satır sayısını geçirin `skip` satırları görmek için üst birazcık daha indiğimde.

```
builder.preview(skip=30)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|1 Ocak 2015 10 PM - 12: 00|
|31|1/1/2015 23:54|1 Ocak 2015 10 PM - 12: 00|
|32|1/1/2015 23:59|1 Ocak 2015 10 PM - 12: 00|
|33|1/2/2015 0:54|1 Şubat 2015 12: 00 - 02: 00|
|34|1/2/2015 1:00|1 Şubat 2015 12: 00 - 02: 00|
|35|1/2/2015 1:54|1 Şubat 2015 12: 00 - 02: 00|
|36|1/2/2015 2:54|1 Şubat 2015 02: 00 - 04: 00|
|37|1/2/2015 3:54|1 Şubat 2015 02: 00 - 04: 00|
|38|1/2/2015 4:00|1 Şubat 2015'te 4-6'da|
|39|1/2/2015 4:54|1 Şubat 2015'te 4-6'da|

Burada oluşturulan program ile ilgili bir sorun görürsünüz. Yalnızca yukarıda sağlanan bir örneğe bağlı olarak, bu durumda istediklerinizi değil olduğu "Gün/ay/yıl" olarak bir tarihi ayrıştırmak türet program seçtiniz. Bu sorunu gidermek için başka bir örnek kullanarak sağlamak `add_example()` işlevini `builder` değişkeni.

```python
builder.add_example(source_data=preview_df.iloc[3], example_value='Jan 2, 2015 12AM-2AM')
builder.preview(skip=30, count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|30|1/1/2015 22:54|1 Ocak 2015 10 PM - 12: 00|
|31|1/1/2015 23:54|1 Ocak 2015 10 PM - 12: 00|
|32|1/1/2015 23:59|1 Ocak 2015 10 PM - 12: 00|
|33|1/2/2015 0:54|2 Ocak 2015 12: 00 - 02: 00|
|34|1/2/2015 1:00|2 Ocak 2015 12: 00 - 02: 00|
|35|1/2/2015 1:54|2 Ocak 2015 12: 00 - 02: 00|
|36|1/2/2015 2:54|2 Ocak 2015 02: 00 - 04: 00|
|37|1/2/2015 3:54|2 Ocak 2015 02: 00 - 04: 00|
|38|1/2/2015 4:00|2 Ocak 2015 04: 00 - 6'da|
|39|1/2/2015 4:54|2 Ocak 2015 04: 00 - 6'da|

Satırları doğru bir şekilde işlemek artık ' 1/2/2015' 'Oca ancak türetilmiş sütun boyunca başka baktığınızda, 2 2015' olarak gördüğünüz değerleri sonunda türetilmiş sütununda hiçbir şey vardır. Bu sorunu gidermek için başka bir örnek için satır 66 sağlamanız gerekir.

```python
builder.add_example(source_data=preview_df.iloc[66], example_value='Jan 29, 2015 8PM-10PM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|1 Ocak 2015 10 PM - 12: 00|
|1|1/1/2015 23:54|1 Ocak 2015 10 PM - 12: 00|
|2|1/1/2015 23:59|1 Ocak 2015 10 PM - 12: 00|
|3|1/2/2015 0:54|2 Ocak 2015 12: 00 - 02: 00|
|4|1/2/2015 1:00|2 Ocak 2015 12: 00 - 02: 00|
|5|1/2/2015 1:54|2 Ocak 2015 12: 00 - 02: 00|
|6|1/2/2015 2:54|2 Ocak 2015 02: 00 - 04: 00|
|7|1/2/2015 3:54|2 Ocak 2015 02: 00 - 04: 00|
|8|1/2/2015 4:00|2 Ocak 2015 04: 00 - 6'da|
|9|1/2/2015 4:54|2 Ocak 2015 04: 00 - 6'da|

Ayrı tarih ve saat ile ' |', başka bir örnek ekleyin. Preview sürümünden bir satırda geçirmek yerine bu kez, sütun adı'değerine sözlüğü oluşturmak `source_data` parametresi.

```python
builder.add_example(source_data={'DATE': '11/11/2015 0:54'}, example_value='Nov 11, 2015 | 12AM-2AM')
builder.preview(count=10)
```

||DATE|date_timerange|
|-----|-----|-----|
|0|1/1/2015 22:54|None|
|1|1/1/2015 23:54|None|
|2|1/1/2015 23:59|None|
|3|1/2/2015 0:54|None|
|4|1/2/2015 1:00|None|
|5|1/2/2015 1:54|None|
|6|1/2/2015 2:54|None|
|7|1/2/2015 3:54|None|
|8|1/2/2015 4:00|None|
|9|1/2/2015 4:54|None|

Bu açıkça artık türetilmiş bir sütunda herhangi bir değere sahip tek satır bizim sağladığımız örnek ile tam olarak eşleşen olanlardır gibi olumsuz etkileri vardı. Çağrı `list_examples()` geçerli örnek türetme bir listesini görmek için oluşturucu nesne üzerinde.

```python
examples = builder.list_examples()
```

| |DATE|Örnek|example_id|
| -------- | -------- | -------- | -------- |
|0|1/1/2015 1:00|1 Ocak 2015 12: 00 - 02: 00|-1|
|1|1/2/2015 0:54|2 Ocak 2015 12: 00 - 02: 00|-2|
|2|29/1/2015 20:54|29 Ocak 2015'te 8-10 PM|-3|
|3|11/11/2015 0:54|11 Kasım 2015 \| 12: 00 - 02: 00|-4|

Bu durumda, tutarsız örnekler sağlanmıştır. Bu sorunu düzeltmek için ilk üç örnekler doğru oluşturduklarıyla değiştirin (dahil olmak üzere ' |' tarih ve saat arasında).

Yanlış örnekler silerek tutarsız örnekleri düzeltme (ya da geçirerek `example_row` pandas DataFrame veya geçirerek `example_id` değeri) ve yeni ekleme örnekleri geri değiştirilebilir.

```python
builder.delete_example(example_id=-1)
builder.delete_example(example_row=examples.iloc[1])
builder.delete_example(example_row=examples.iloc[2])
builder.add_example(examples.iloc[0], 'Jan 1, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[1], 'Jan 2, 2015 | 12AM-2AM')
builder.add_example(examples.iloc[2], 'Jan 29, 2015 | 8PM-10PM')
builder.preview()
```

| | DATE | date_timerange |
| -------- | -------- | -------- |
| 0 | 1/1/2015 0:54 | 1 Ocak 2015 \| 12: 00 - 02: 00 |
| 1 | 1/1/2015 1:00 | 1 Ocak 2015 \| 12: 00 - 02: 00 |
| 2 | 1/1/2015 1:54 | 1 Ocak 2015 \| 12: 00 - 02: 00 |
| 3 | 1/1/2015 2:54 | 1 Ocak 2015 \| 02: 00 - 04: 00 |
| 4 | 1/1/2015 3:54 | 1 Ocak 2015 \| 02: 00 - 04: 00 |
| 5 | 1/1/2015 4:00 | 1 Ocak 2015 \| 04: 00 - 6'da|
| 6 | 1/1/2015 4:54 | 1 Ocak 2015 \| 04: 00 - 6'da|
| 7 | 1/1/2015 5:54 | 1 Ocak 2015 \| 04: 00 - 6'da|
| 8 | 1/1/2015 6:54 | 1 Ocak 2015 \| 6 AM - 8: 00|
| 9 | 1/1/2015 7:00 | 1 Ocak 2015 \| 6 AM - 8: 00|

Artık verileri doğru bakar ve çağırmanızı `to_dataflow()` Oluşturucusu'eklenen istenen türetilmiş sütunlar bir veri akışı döndürülür.

```python
dataflow = builder.to_dataflow()
df = dataflow.to_pandas_dataframe()
```

## <a name="filtering"></a>Filtreleme

SDK'sı yöntemleri içerir `Dataflow.drop_columns` ve `Dataflow.filter` izin verecek şekilde satırları veya sütunları filtreleyin.

### <a name="initial-setup"></a>Başlangıç kurulumu

```python
import azureml.dataprep as dprep
from datetime import datetime
dataflow = dprep.read_csv(path='https://dprepdata.blob.core.windows.net/demo/green-small/*')
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Store_and_fwd_flag|RateCodeID|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|N|1|0|0|0|0|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|N|1|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|N|5|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|N|1|0|0|0|0|1|.00|0|0|3.25|

### <a name="filtering-columns"></a>Sütunları filtreleme

Sütunları filtrelemek, kullanın `Dataflow.drop_columns`. Bu yöntem bırakmak sütun listesini alır veya daha karmaşık bir bağımsız değişken olarak adlandırılan `ColumnSelector`.

#### <a name="filtering-columns-with-list-of-strings"></a>Sütunları içeren bir dize listesi filtreleme

Bu örnekte, `drop_columns` dizeleri listesini alır. Her bir dizenin bırakmak istediğiniz sütunu tam olarak eşleşmelidir.

```python
dataflow = dataflow.drop_columns(['Store_and_fwd_flag', 'RateCodeID'])
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Pickup_longitude|Pickup_latitude|Dropoff_longitude|Dropoff_latitude|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|0|0|0|0|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|0|0|0|0|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|0|0|0|0|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|0|0|0|0|1|.00|0|0|3.25|

#### <a name="filtering-columns-with-regex"></a>Regex sütunlarla filtreleme

Alternatif olarak, `ColumnSelector` ifade normal bir ifadeyle eşleşen sütunları kaldırın. Bu örnekte, bir ifadeyle eşleşen tüm sütunlar bırak `Column*|.*longitude|.*latitude`.

```python
dataflow = dataflow.drop_columns(dprep.ColumnSelector('Column*|.*longitud|.*latitude', True, True))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|None|Yok.|Yok.|Yok.|Yok.|Yok.|None|
|1|2013-08-01 08:14:37|2013-08-01 09:09:06|1|.00|0|0|21,25|
|2|2013-08-01 09:13:00|2013-08-01 11:38:00|2|.00|0|0|75|
|3|2013-08-01 09:48:00|2013-08-01 09:49:00|1|.00|0|1|2.1|
|4|2013-08-01 10:38:35|2013-08-01 10:38:51|1|.00|0|0|3.25|

## <a name="filtering-rows"></a>Filtre satırları

Satırları filtrele, kullanın `DataFlow.filter`. Bu yöntem, Azure Machine Learning veri hazırlığı SDK'sı ifade bir bağımsız değişken olarak alır ve yeni bir veri akışı ile ifade True olarak değerlendirilen satırları döndürür. İfadeleri, ifade oluşturucular kullanılarak oluşturulur (`col`, `f_not`, `f_and`, `f_or`) ve normal işleçleri (>, <>, =, < =, ==,! =).

### <a name="filtering-rows-with-simple-expressions"></a>Basit ifadelerle satırları filtreleme

İfade Oluşturucu kullanın `col`, bir dize bağımsız değişkeni bir sütun adı belirtin `col('column_name')`. Bu ifade aşağıdaki standart işleçlerden ile birlikte kullanma >, <>, =, < =, ==,! gibi bir ifade oluşturmak için = `col('Tip_amount') > 0`. Son olarak, yerleşik ifadesine geçirmek `Dataflow.filter` işlevi.

Bu örnekte, `dataflow.filter(col('Tip_amount') > 0)` sütunları içeren yeni bir veri akışı döndüren değerini `Tip_amount` 0'dan büyük.

> [!NOTE] 
> `Tip_amount` ilk karşı diğer sayısal değerleri karşılaştırma bir ifade oluşturmak sağlıyor sayısal, dönüştürülür.

```python
dataflow = dataflow.to_number(['Tip_amount'])
dataflow = dataflow.filter(dprep.col('Tip_amount') > 0)
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-01 19:33:28|2013-08-01 19:35:21|5|.00|0.08|0|4.58|
|1|2013-08-05 13:16:38|2013-08-05 13:18:24|1|.00|0.30|0|3.8|
|2|2013-08-05 14:11:42|2013-08-05 14:12:47|1|.00|1,05|0|4.55|
|3|2013-08-05 14:15:56|2013-08-05 14:18:04|5|.00|2.22|0|5,72|
|4|2013-08-05 14:42:14|2013-08-05 14:42:38|1|.00|0.88|0|4.38|

### <a name="filtering-rows-with-complex-expressions"></a>Karmaşık ifadeleri ile satırları filtreleme

Karmaşık ifadeleri kullanarak filtre uygulamak için ifade oluşturuculara sahip bir veya daha fazla basit ifadeler birleştirmek `f_not`, `f_and`, veya `f_or`.

Bu örnekte, `Dataflow.filter` sütunları içeren yeni bir veri akışı döndürür burada `'Passenger_count'` 5'ten az olan ve `'Tolls_amount'` 0'dan büyük.

```python
dataflow = dataflow.to_number(['Passenger_count', 'Tolls_amount'])
dataflow = dataflow.filter(dprep.f_and(dprep.col('Passenger_count') < 5, dprep.col('Tolls_amount') > 0))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013-08-08 12:16:00|2013-08-08 12:16:00|1.0|.00|2.25|5.00|19.75|
|1|2013-08-12 14:43:53|2013-08-12 15:04:50|1.0|5,28|6.46|5.33|32.29|
|2|2013-08-12 19:48:12|2013-08-12 20:03:42|1.0|5.50|1.00|10.66|30.66|
|3|2013 08 13 06:11:06|2013 08 13 06:30:28|1.0|9,57|7.47|5.33|44,8|
|4|2013-08-16 20:33:50|2013-08-16 20:48:50|1.0|5.63|3.00|5.33|27.83|

İç içe geçmiş bir ifade oluşturmak için birden fazla ifade oluşturucu birleştirme satırları filtrele da mümkündür.

> [!NOTE]
> `lpep_pickup_datetime` ve `Lpep_dropoff_datetime` kurmamızı diğer datetime değerleri karşı karşılaştırma bir ifade oluşturmak tarih saat önce dönüştürülür.

```python
dataflow = dataflow.to_datetime(['lpep_pickup_datetime', 'Lpep_dropoff_datetime'], ['%Y-%m-%d %H:%M:%S'])
dataflow = dataflow.to_number(['Total_amount', 'Trip_distance'])
mid_2013 = datetime(2013,7,1)
dataflow = dataflow.filter(
    dprep.f_and(
        dprep.f_or(
            dprep.col('lpep_pickup_datetime') > mid_2013,
            dprep.col('Lpep_dropoff_datetime') > mid_2013),
        dprep.f_and(
            dprep.col('Total_amount') > 40,
            dprep.col('Trip_distance') < 10)))
dataflow.head(5)
```

||lpep_pickup_datetime|Lpep_dropoff_datetime|Passenger_count|Trip_distance|Tip_amount|Tolls_amount|Total_amount|
|-----|-----|-----|-----|-----|-----|-----|-----|
|0|2013 08 13 06:11:06 + 00:00|2013 08 13 06:30:28 + 00:00|1.0|9,57|7.47|5.33|44.80|
|1|2013-08-23 12:28:20 + 00:00|2013-08-23 12:50:28 + 00:00|2.0|8.22|8.08|5.33|40.41|
|2|2013-08-25 09:12:52 + 00:00|2013-08-25 09:34:34 + 00:00|1.0|8.80|8.33|5.33|41.66|
|3|2013-08-25 16:46:51 + 00:00|2013-08-25 17:13:55 + 00:00|2.0|9.66|7.37|5.33|44.20|
|4|2013-08-25 17:42:11 + 00:00|2013-08-25 18:02:57 + 00:00|1.0|9.60|6.87|5.33|41.20|

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

df = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv', skip_rows=1)
df.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|ALABAMA|1|101710|Hale ilçe|10171002158| |
|1|ALABAMA|1|101710|Hale ilçe|10171002162| |
|2|ALABAMA|1|101710|Hale ilçe|10171002156| |
|3|ALABAMA|1|101710|Hale ilçe|10171000588|2|
|4|ALABAMA|1|101710|Hale ilçe|10171000589| |

Veri kümesinin trim ve bazı temel dönüşümler yapın.

```python
df = df.keep_columns(['stnam', 'leanm10', 'ncessch', 'MAM_MTH00numvalid_1011'])
df = df.replace_na(columns=['leanm10', 'MAM_MTH00numvalid_1011'], custom_na_list='.')
df = df.to_number(['ncessch', 'MAM_MTH00numvalid_1011'])
df.head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale ilçe|1.017100e + 10|None|
|1|ALABAMA|Hale ilçe|1.017100e + 10|None|
|2|ALABAMA|Hale ilçe|1.017100e + 10|None|
|3|ALABAMA|Hale ilçe|1.017100e + 10|2|
|4|ALABAMA|Hale ilçe|1.017100e + 10|None|

Null değerler aşağıdaki filtre kullanarak arayın.

```python
df.filter(col('MAM_MTH00numvalid_1011').is_null()).head(5)
```

| |stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale ilçe|1.017100e + 10|None|
|1|ALABAMA|Hale ilçe|1.017100e + 10|None|
|2|ALABAMA|Hale ilçe|1.017100e + 10|None|
|3|ALABAMA|Hale ilçe|1.017100e + 10|None|
|4|ALABAMA|Hale ilçe|1.017100e + 10|None|

### <a name="transform-partition"></a>Bölüm dönüştürme

Tüm null değerleri 0 ile değiştirmek için bir pandas işlevini kullanın. Bu kod, tek seferde tüm veri kümesi üzerinde bir bölümü tarafından çalıştırılır. Başka bir deyişle, bir büyük veri kümesini temel çalışma zamanı tarafından bölüm bölüm veri işlerken paralel olarak bu kod çalıştırabilir.

Python betiğini çağrılan bir işlev tanımlamalıdır `transform()` iki bağımsız değişkeni almayan `df` ve `index`. `df` Bağımsız değişken bölümü için veri içeren bir pandas dataframe olacaktır ve `index` değişkendir bölümünün benzersiz bir tanımlayıcı. Dönüştürme işlevi tam olarak geçirilen veri çerçevesi düzenleyebilirsiniz ancak bir dataframe döndürmelidir. Python betiğini içe aktaran tüm kitaplıkları, veri akışı çalıştırıldığı ortama mevcut olması gerekir.

```python
df = df.transform_partition("""
def transform(df, index):
    df['MAM_MTH00numvalid_1011'].fillna(0,inplace=True)
    return df
""")
df.head(5)
```

||stnam|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale ilçe|1.017100e + 10|0.0|
|1|ALABAMA|Hale ilçe|1.017100e + 10|0.0|
|2|ALABAMA|Hale ilçe|1.017100e + 10|0.0|
|3|ALABAMA|Hale ilçe|1.017100e + 10|2.0|
|4|ALABAMA|Hale ilçe|1.017100e + 10|0.0|

### <a name="new-script-column"></a>Yeni bir betik sütun

Ülke adı ve durum adı olan yeni bir sütun oluşturmak için ve eyalet adı analizden yararlanmak için Python kodu kullanabilirsiniz. Bunu yapmak için `new_script_column()` veri akışı üzerinde yöntemi.

Python betiğini çağrılan bir işlev tanımlamalıdır `newvalue()` tek bir bağımsız değişken almayan `row`. `row` Bağımsız değişkeni olan bir, dict (`key`: sütun adı `val`: geçerli değer) ve veri kümesindeki her satır için bu işleve geçirilir. Bu işlev, yeni bir sütun kullanılacak bir değer döndürmesi gerekir. Python betiğini içe aktaran tüm kitaplıkları, veri akışı çalıştırıldığı ortama mevcut olması gerekir.

```python
df = df.new_script_column(new_column_name='county_state', insert_after='leanm10', script="""
def newvalue(row):
    return row['leanm10'] + ', ' + row['stnam'].title()
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|0.0|
|1|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|0.0|
|2|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|0.0|
|3|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|2.0|
|4|ALABAMA|Hale ilçe|Hale ilçe, Alabama|1.017100e + 10|0.0|

### <a name="new-script-filter"></a>Yeni betik filtresi

Veri kümesi yalnızca 'Hale' olduğu yeni satırlara filtrelemek için bir Python ifadesi Oluştur `county_state` sütun. Bir ifade döndürür `True` satır tutmak istiyorsanız ve `False` satır bırakmak.

```python
df = df.new_script_filter("""
def includerow(row):
    val = row['county_state']
    return 'Hale' not in val
""")
df.head(5)
```

||stnam|leanm10|county_state|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|
|0|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|1.0|
|1|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|0.0|
|2|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|0.0|
|3|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|0.0|
|4|ALABAMA|Jefferson ilçe|Jefferson ilçe, Alabama|1.019200e + 10|0.0|
