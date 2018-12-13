---
title: 'Regresyon modeli öğretici: Verileri hazırlama'
titleSuffix: Azure Machine Learning service
description: Bu öğreticinin ilk bölümünde Azure ML SDK'sını kullanarak regresyon modelleme python'da verileri hazırlama öğreneceksiniz.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: cforbe
ms.author: cforbe
ms.reviewer: trbye
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d20ff1fabfb73c899153cf42bb6f2d7a8f233e21
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314695"
---
# <a name="tutorial-prepare-data-for-regression-modeling"></a>Öğretici: Regresyon model için verileri hazırlama

Bu öğreticide, Azure Machine Learning veri hazırlığı SDK'sını kullanarak regresyon modelleme için veri hazırlama konusunda bilgi edinin. Filtre ve iki farklı NYC taksi veri kümeleri birleştirmek için çeşitli dönüştürmeleri gerçekleştirebilirsiniz. Toplama saat dahil olmak üzere veri özellikleri bir model eğitip geleceği taksi seyahat maliyetini tahmin etmek için Bu öğretici kümesi son hedefi olduğu gün, hafta sayısı Yolcuların ve koordinatları. Bu öğreticide iki kısımlı öğretici serisinin bir parçasıdır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Paketleri içeri aktarmak ve Python ortamını ayarlama
> * İki veri kümesi farklı alan adları ile yükleme
> * Anomalileri kaldırılacağı verilerini temizlemek
> * Yeni özellikler oluşturmak için akıllı dönüşümler kullanarak verileri dönüştürme
> * Veri akışı nesnenizin bir regresyon modeli kullanmak için kaydetme

Python kullanarak veri hazırlayabilir [Azure Machine Learning veri hazırlığı SDK'sı](https://aka.ms/data-prep-sdk).

## <a name="get-the-notebook"></a>Not defterini alma

Kolaylık olması için, bu öğretici bir [Jupyter notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/regression-part1-data-prep.ipynb) olarak sağlanır. `regression-part1-data-prep.ipynb` notebook'unu Azure Notebooks üzerinde veya kendi Jupyter notebook sunucunuzda çalıştırın.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="import-packages"></a>Paketleri içeri aktarma

SDK'sı içeri aktararak başlayın.


```python
import azureml.dataprep as dprep
```

## <a name="load-data"></a>Veri yükleme

İki farklı NYC taksi veri kümesi, veri akışı nesneleri indirin.  Bu veri kümelerini biraz farklı alanları içerir. Yöntem `auto_read_file()` giriş dosya türü otomatik olarak tanır.


```python
dataset_root = "https://dprepdata.blob.core.windows.net/demo"

green_path = "/".join([dataset_root, "green-small/*"])
yellow_path = "/".join([dataset_root, "yellow-small/*"])

green_df = dprep.read_csv(path=green_path, header=dprep.PromoteHeadersMode.GROUPED)
# auto_read_file will automatically identify and parse the file type, and is useful if you don't know the file type
yellow_df = dprep.auto_read_file(path=yellow_path)

display(green_df.head(5))
display(yellow_df.head(5))
```

## <a name="cleanse-data"></a>Verilerini temizlemek

Artık tüm veri akışı için geçerli bir kısayol dönüşümler bazı değişkenlerle doldurun. Değişken `drop_if_all_null` kayıtları silmek için tüm alanların null olduğu kullanılacak. Değişken `useful_columns` her bir veri akışı korunur sütun açıklamaları dizisi içerir.

```python
all_columns = dprep.ColumnSelector(term=".*", use_regex=True)
drop_if_all_null = [all_columns, dprep.ColumnRelationship(dprep.ColumnRelationship.ALL)]
useful_columns = [
    "cost", "distance", "dropoff_datetime", "dropoff_latitude", "dropoff_longitude",
    "passengers", "pickup_datetime", "pickup_latitude", "pickup_longitude", "store_forward", "vendor"
]
```

İlk yeşil taksi verileri ile çalışma ve geçerli bir içine şekil get sarı taksi verileri ile birleştirilebilir. Geçici bir veri akışı oluşturma `tmp_df`. Çağrı `replace_na()`, `drop_nulls()`, ve `keep_columns()` işlevleri kısayolunu kullanarak oluşturduğunuz değişkenleri dönüştürün. Ayrıca, veri çerçevesi adlarını eşleştirmek için tüm sütunları yeniden adlandırma `useful_columns`.


```python
tmp_df = (green_df
    .replace_na(columns=all_columns)
    .drop_nulls(*drop_if_all_null)
    .rename_columns(column_pairs={
        "VendorID": "vendor",
        "lpep_pickup_datetime": "pickup_datetime",
        "Lpep_dropoff_datetime": "dropoff_datetime",
        "lpep_dropoff_datetime": "dropoff_datetime",
        "Store_and_fwd_flag": "store_forward",
        "store_and_fwd_flag": "store_forward",
        "Pickup_longitude": "pickup_longitude",
        "Pickup_latitude": "pickup_latitude",
        "Dropoff_longitude": "dropoff_longitude",
        "Dropoff_latitude": "dropoff_latitude",
        "Passenger_count": "passengers",
        "Fare_amount": "cost",
        "Trip_distance": "distance"
     })
    .keep_columns(columns=useful_columns))
tmp_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Satıcı</th>
      <th>pickup_datetime</th>
      <th>dropoff_datetime</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>Yolcuların</th>
      <th>uzaklık</th>
      <th>maliyet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 08:14:37</td>
      <td>2013-08-01 09:09:06</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>21,25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 09:13:00</td>
      <td>2013-08-01 11:38:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>.00</td>
      <td>74,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-01 09:48:00</td>
      <td>2013-08-01 09:49:00</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-01 10:38:35</td>
      <td>2013-08-01 10:38:51</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-01 11:51:45</td>
      <td>2013-08-01 12:03:52</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>.00</td>
      <td>8.5</td>
    </tr>
  </tbody>
</table>
</div>

Üzerine `green_df` gerçekleştirilen dönüşümler ile değişken `tmp_df` önceki adımda.

```python
green_df = tmp_df
```

Sarı taksi verileri dönüştürme adımları gerçekleştirin.


```python
tmp_df = (yellow_df
    .replace_na(columns=all_columns)
    .drop_nulls(*drop_if_all_null)
    .rename_columns(column_pairs={
        "vendor_name": "vendor",
        "VendorID": "vendor",
        "vendor_id": "vendor",
        "Trip_Pickup_DateTime": "pickup_datetime",
        "tpep_pickup_datetime": "pickup_datetime",
        "Trip_Dropoff_DateTime": "dropoff_datetime",
        "tpep_dropoff_datetime": "dropoff_datetime",
        "store_and_forward": "store_forward",
        "store_and_fwd_flag": "store_forward",
        "Start_Lon": "pickup_longitude",
        "Start_Lat": "pickup_latitude",
        "End_Lon": "dropoff_longitude",
        "End_Lat": "dropoff_latitude",
        "Passenger_Count": "passengers",
        "passenger_count": "passengers",
        "Fare_Amt": "cost",
        "fare_amount": "cost",
        "Trip_Distance": "distance",
        "trip_distance": "distance"
    })
    .keep_columns(columns=useful_columns))
tmp_df.head(5)
```

Yeniden üzerine `yellow_df` ile `tmp_df`ve sonra çağrı `append_rows()` işleticini yeşil taksi verileri yeni bir sarı taksi verileri eklemek için birleştirilmiş veri çerçevesi.


```python
yellow_df = tmp_df
combined_df = green_df.append_rows([yellow_df])
```

### <a name="convert-types-and-filter"></a>Türleri ve filtre Dönüştür 

Verilerin nasıl dağıtılması görmek için alma ve teslim koordinatları Özet istatistikleri inceleyin. İlk tanımlamak bir `TypeConverter` lat/uzun alanlar için ondalık türünü değiştirmek için nesne. Ardından, arama `keep_columns()` lat/uzun alanlara yalnızca çıktı kısıtlamak için işlev ve çağırma `get_profile()`.


```python
decimal_type = dprep.TypeConverter(data_type=dprep.FieldType.DECIMAL)
combined_df = combined_df.set_column_types(type_conversions={
    "pickup_longitude": decimal_type,
    "pickup_latitude": decimal_type,
    "dropoff_longitude": decimal_type,
    "dropoff_latitude": decimal_type
})
combined_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude", 
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tür</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 Quantile</th>
      <th>%1 Quantile</th>
      <th>%5 Quantile</th>
      <th>% 25 Quantile</th>
      <th>% 50 Quantile</th>
      <th>% 75 Quantile</th>
      <th>% 95 Quantile</th>
      <th>% 99 Quantile</th>
      <th>% 99,9 Quantile</th>
      <th>Standart Sapma</th>
      <th>Ortalama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115.179337</td>
      <td>0.000000</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-88.114046</td>
      <td>-73.961840</td>
      <td>-73.961964</td>
      <td>-73.947693</td>
      <td>-73.922097</td>
      <td>-73.846670</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>18.792672</td>
      <td>-68.833579</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0.000000</td>
      <td>40.919121</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>40.682889</td>
      <td>40.675541</td>
      <td>40.721075</td>
      <td>40.756159</td>
      <td>40.803909</td>
      <td>40.849406</td>
      <td>40.870681</td>
      <td>40.891244</td>
      <td>10.345967</td>
      <td>37.936742</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-115.179337</td>
      <td>0.000000</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-87.699611</td>
      <td>-73.984734</td>
      <td>-73.985777</td>
      <td>-73.956250</td>
      <td>-73.928948</td>
      <td>-73.866208</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>18.696526</td>
      <td>-68.896978</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>0.000000</td>
      <td>41.008934</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>7722.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>40.662763</td>
      <td>40.654851</td>
      <td>40.717821</td>
      <td>40.756534</td>
      <td>40.784688</td>
      <td>40.852437</td>
      <td>40.879289</td>
      <td>40.937291</td>
      <td>10.290780</td>
      <td>37.963774</td>
    </tr>
  </tbody>
</table>



Özet istatistikleri çıktısını eksik koordinatlar vardır ve koordinatları New York City olmayan bakın. Sütun filtresi komutlar zincirleme olarak Şehir kenarlık değil koordinatlarında filtrelemek `filter()` işlev ve her bir alan için minimum ve maksimum sınırları tanımlama. Ardından çağırın `get_profile()` yeniden dönüştürme doğrulamak için.


```python
tmp_df = (combined_df
    .drop_nulls(
        columns=["pickup_longitude", "pickup_latitude", "dropoff_longitude", "dropoff_latitude"],
        column_relationship=dprep.ColumnRelationship(dprep.ColumnRelationship.ANY)
    ) 
    .filter(dprep.f_and(
        dprep.col("pickup_longitude") <= -73.72,
        dprep.col("pickup_longitude") >= -74.09,
        dprep.col("pickup_latitude") <= 40.88,
        dprep.col("pickup_latitude") >= 40.53,
        dprep.col("dropoff_longitude") <= -73.72,
        dprep.col("dropoff_longitude") >= -74.09,
        dprep.col("dropoff_latitude") <= 40.88,
        dprep.col("dropoff_latitude") >= 40.53
    )))
tmp_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude", 
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tür</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 Quantile</th>
      <th>%1 Quantile</th>
      <th>%5 Quantile</th>
      <th>% 25 Quantile</th>
      <th>% 50 Quantile</th>
      <th>% 75 Quantile</th>
      <th>% 95 Quantile</th>
      <th>% 99 Quantile</th>
      <th>% 99,9 Quantile</th>
      <th>Standart Sapma</th>
      <th>Ortalama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.078156</td>
      <td>-73.736481</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.076314</td>
      <td>-73.962542</td>
      <td>-73.962893</td>
      <td>-73.948975</td>
      <td>-73.927856</td>
      <td>-73.866662</td>
      <td>-73.830438</td>
      <td>-73.823160</td>
      <td>-73.769750</td>
      <td>0.048711</td>
      <td>-73.913865</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.575485</td>
      <td>40.879852</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.632884</td>
      <td>40.713105</td>
      <td>40.711600</td>
      <td>40.721403</td>
      <td>40.758142</td>
      <td>40.805145</td>
      <td>40.848855</td>
      <td>40.867567</td>
      <td>40.877690</td>
      <td>0.048348</td>
      <td>40.765226</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.085747</td>
      <td>-73.720871</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.078828</td>
      <td>-73.985650</td>
      <td>-73.985813</td>
      <td>-73.959041</td>
      <td>-73.936681</td>
      <td>-73.884846</td>
      <td>-73.815507</td>
      <td>-73.776697</td>
      <td>-73.733471</td>
      <td>0.055961</td>
      <td>-73.920718</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.583530</td>
      <td>40.879734</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.597741</td>
      <td>40.695376</td>
      <td>40.695115</td>
      <td>40.727549</td>
      <td>40.758160</td>
      <td>40.788378</td>
      <td>40.850372</td>
      <td>40.867968</td>
      <td>40.878586</td>
      <td>0.050462</td>
      <td>40.759487</td>
    </tr>
  </tbody>
</table>



Üzerine `combined_df` yaptığınız dönüşümler ile `tmp_df`.


```python
combined_df = tmp_df
```

### <a name="split-and-rename-columns"></a>Bölme ve sütunları yeniden adlandırma

Veri profili bakın `store_forward` sütun.


```python
combined_df.keep_columns(columns='store_forward').get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tür</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>% 0,1 Quantile</th>
      <th>%1 Quantile</th>
      <th>%5 Quantile</th>
      <th>% 25 Quantile</th>
      <th>% 50 Quantile</th>
      <th>% 75 Quantile</th>
      <th>% 95 Quantile</th>
      <th>% 99 Quantile</th>
      <th>% 99,9 Quantile</th>
      <th>Standart Sapma</th>
      <th>Ortalama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>E</td>
      <td>7059.0</td>
      <td>99.0</td>
      <td>6960.0</td>
      <td>0.014025</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>



Veri profili çıktısından `store_forward`, verilerin tutarlı değil ve yok/null değerler görürsünüz. Bu değerleri kullanarak değiştirin `replace()` ve `fill_nulls()` çalışır ve her iki durumda da "N" dizesi olarak değiştirin.


```python
combined_df = combined_df.replace(columns="store_forward", find="0", replace_with="N").fill_nulls("store_forward", "N")
```

Başka bir yürütme `replace` , bu kez işlevini `distance` alan. Bu yanlış olarak etiketlenmiş uzaklık değerleri yeniden biçimlendirir `.00`, herhangi bir null değerlere sıfır ile doldurur. Dönüştürme `distance` sayısal biçimi alanı.


```python
combined_df = combined_df.replace(columns="distance", find=".00", replace_with=0).fill_nulls("distance", 0)
combined_df = combined_df.to_number(["distance"])
```

Çekme bölmek ve tarih/saat ilgili tarih ve saat sütunları devre dışı bırakın. Kullanım `split_column_by_example()` bölme işlemini gerçekleştirmek için. Bu durumda, isteğe bağlı `example` parametresinin `split_column_by_example()` atlanır. Bu nedenle işlevi veriler temel alınarak bölmek nereye otomatik olarak belirler.


```python
tmp_df = (combined_df
    .split_column_by_example(source_column="pickup_datetime")
    .split_column_by_example(source_column="dropoff_datetime"))
tmp_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Satıcı</th>
      <th>pickup_datetime</th>
      <th>pickup_datetime_1</th>
      <th>pickup_datetime_2</th>
      <th>dropoff_datetime</th>
      <th>dropoff_datetime_1</th>
      <th>dropoff_datetime_2</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>Yolcuların</th>
      <th>uzaklık</th>
      <th>maliyet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>2013-08-01 17:22:00</td>
      <td>2013-08-01</td>
      <td>17:22:00</td>
      <td>N</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>2013-08-01</td>
      <td>17:24:00</td>
      <td>2013-08-01 17:25:00</td>
      <td>2013-08-01</td>
      <td>17:25:00</td>
      <td>N</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>2013-08-06</td>
      <td>06:51:19</td>
      <td>2013-08-06 06:51:36</td>
      <td>2013-08-06</td>
      <td>06:51:36</td>
      <td>N</td>
      <td>-73.937721</td>
      <td>40.758404</td>
      <td>-73.937721</td>
      <td>40.758369</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>2013-08-06</td>
      <td>13:26:34</td>
      <td>2013-08-06 13:26:57</td>
      <td>2013-08-06</td>
      <td>13:26:57</td>
      <td>N</td>
      <td>-73.937691</td>
      <td>40.758419</td>
      <td>-73.937790</td>
      <td>40.758358</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>2013-08-06</td>
      <td>13:27:53</td>
      <td>2013-08-06 13:28:08</td>
      <td>2013-08-06</td>
      <td>13:28:08</td>
      <td>N</td>
      <td>-73.937805</td>
      <td>40.758396</td>
      <td>-73.937775</td>
      <td>40.758450</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>


Tarafından oluşturulan sütunları yeniden adlandırma `split_column_by_example()` içine anlamlı adlar.


```python
tmp_df_renamed = (tmp_df
    .rename_columns(column_pairs={
        "pickup_datetime_1": "pickup_date",
        "pickup_datetime_2": "pickup_time",
        "dropoff_datetime_1": "dropoff_date",
        "dropoff_datetime_2": "dropoff_time"
    }))
tmp_df_renamed.head(5)
```

Üzerine `combined_df` yürütülen dönüşümleri ve ardından bir çağrı ile `get_profile()` sonra tüm dönüştürmeler tam Özet istatistikleri görmek için.


```python
combined_df = tmp_df_renamed
combined_df.get_profile()
```

## <a name="transform-data"></a>Verileri dönüştürme

Daha fazla alma ve teslim tarihi haftanın günü, ayın ve ayın günü bölün. Haftanın günü almak için kullanın `derive_column_by_example()` işlevi. Bu işlev, girdi verilerini ve istenen çıkışı tanımlayan bir dizi örnek nesneleri bir parametre olarak alır. Ardından işlev, istedikleri dönüşümü otomatik olarak belirler. Saat içinde bölme, alma ve teslim saat sütunları için dakika ve kullanarak ikinci `split_column_by_example()` işlevi ile örnek parametre yok.

Bu yeni özellikler oluşturduktan sonra özgün alanlara kullanarak yeni oluşturulan özellik ile değiştiriliyor Sil `drop_columns()`. Kalan tüm alanlar için doğru açıklamaları yeniden adlandırın.


```python
tmp_df = (combined_df
    .derive_column_by_example(
        source_columns="pickup_date", 
        new_column_name="pickup_weekday", 
        example_data=[("2009-01-04", "Sunday"), ("2013-08-22", "Thursday")]
    )
    .derive_column_by_example(
        source_columns="dropoff_date",
        new_column_name="dropoff_weekday",
        example_data=[("2013-08-22", "Thursday"), ("2013-11-03", "Sunday")]
    )
          
    .split_column_by_example(source_column="pickup_time")
    .split_column_by_example(source_column="dropoff_time")
    # the following two split_column_by_example calls reference the generated column names from the above two calls
    .split_column_by_example(source_column="pickup_time_1")
    .split_column_by_example(source_column="dropoff_time_1")
    .drop_columns(columns=[
        "pickup_date", "pickup_time", "dropoff_date", "dropoff_time", 
        "pickup_date_1", "dropoff_date_1", "pickup_time_1", "dropoff_time_1"
    ])
          
    .rename_columns(column_pairs={
        "pickup_date_2": "pickup_month",
        "pickup_date_3": "pickup_monthday",
        "pickup_time_1_1": "pickup_hour",
        "pickup_time_1_2": "pickup_minute",
        "pickup_time_2": "pickup_second",
        "dropoff_date_2": "dropoff_month",
        "dropoff_date_3": "dropoff_monthday",
        "dropoff_time_1_1": "dropoff_hour",
        "dropoff_time_1_2": "dropoff_minute",
        "dropoff_time_2": "dropoff_second"
    }))

tmp_df.head(5)
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Satıcı</th>
      <th>pickup_datetime</th>
      <th>pickup_weekday</th>
      <th>pickup_hour</th>
      <th>pickup_minute</th>
      <th>pickup_second</th>
      <th>dropoff_datetime</th>
      <th>dropoff_weekday</th>
      <th>dropoff_hour</th>
      <th>dropoff_minute</th>
      <th>dropoff_second</th>
      <th>store_forward</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>Yolcuların</th>
      <th>uzaklık</th>
      <th>maliyet</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2013-08-01 17:22:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>22</td>
      <td>0</td>
      <td>2013-08-01 17:22:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>22</td>
      <td>0</td>
      <td>N</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>-73.937767</td>
      <td>40.758480</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2013-08-01 17:24:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>24</td>
      <td>0</td>
      <td>2013-08-01 17:25:00</td>
      <td>Perşembe</td>
      <td>17</td>
      <td>25</td>
      <td>0</td>
      <td>N</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>-73.937927</td>
      <td>40.757843</td>
      <td>1</td>
      <td>0.0</td>
      <td>2,5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2013-08-06 06:51:19</td>
      <td>Salı</td>
      <td>06</td>
      <td>51</td>
      <td>19</td>
      <td>2013-08-06 06:51:36</td>
      <td>Salı</td>
      <td>06</td>
      <td>51</td>
      <td>36</td>
      <td>N</td>
      <td>-73.937721</td>
      <td>40.758404</td>
      <td>-73.937721</td>
      <td>40.758369</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2013-08-06 13:26:34</td>
      <td>Salı</td>
      <td>13</td>
      <td>26</td>
      <td>34</td>
      <td>2013-08-06 13:26:57</td>
      <td>Salı</td>
      <td>13</td>
      <td>26</td>
      <td>57</td>
      <td>N</td>
      <td>-73.937691</td>
      <td>40.758419</td>
      <td>-73.937790</td>
      <td>40.758358</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2013-08-06 13:27:53</td>
      <td>Salı</td>
      <td>13</td>
      <td>27</td>
      <td>53</td>
      <td>2013-08-06 13:28:08</td>
      <td>Salı</td>
      <td>13</td>
      <td>28</td>
      <td>08</td>
      <td>N</td>
      <td>-73.937805</td>
      <td>40.758396</td>
      <td>-73.937775</td>
      <td>40.758450</td>
      <td>1</td>
      <td>0.0</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>

Yukarıdaki verilerden türetilen dönüştürmeleri üretilen alma ve teslim tarih ve saat bileşenlerini doğru olduğunu görürsünüz. DROP `pickup_datetime` ve `dropoff_datetime` sütunlar artık oldukları gibi gerekli.


```python
tmp_df = tmp_df.drop_columns(columns=["pickup_datetime", "dropoff_datetime"])
```

Her bir alanın veri türü otomatik olarak denetlemek için tür çıkarımı işlevselliği kullanmak ve çıkarım sonuçları görüntüleyebilirsiniz.


```python
type_infer = tmp_df.builders.set_column_types()
type_infer.learn()
type_infer
```

    Column types conversion candidates:
    'pickup_weekday': [FieldType.STRING],
    'pickup_hour': [FieldType.DECIMAL],
    'pickup_minute': [FieldType.DECIMAL],
    'pickup_second': [FieldType.DECIMAL],
    'dropoff_hour': [FieldType.DECIMAL],
    'dropoff_minute': [FieldType.DECIMAL],
    'dropoff_second': [FieldType.DECIMAL],
    'store_forward': [FieldType.STRING],
    'pickup_longitude': [FieldType.DECIMAL],
    'dropoff_longitude': [FieldType.DECIMAL],
    'passengers': [FieldType.DECIMAL],
    'distance': [FieldType.DECIMAL],
    'vendor': [FieldType.STRING],
    'dropoff_weekday': [FieldType.STRING],
    'pickup_latitude': [FieldType.DECIMAL],
    'dropoff_latitude': [FieldType.DECIMAL],
    'cost': [FieldType.DECIMAL]

Çıkarım sonuçları doğru konum verilerine dayalı veri akışı için tür dönüştürmeleri şimdi başvurun.


```python
tmp_df = type_infer.to_dataflow()
tmp_df.get_profile()
```

Veri akışı paketlemeden önce veri kümesinde son iki filtre uygulayın. Hatalı veri noktalarını ortadan kaldırmak için veri akışı kayıtlar filtre nerede hem `cost` ve `distance` sıfırdan büyükse.

```python
tmp_df = tmp_df.filter(dprep.col("distance") > 0)
tmp_df = tmp_df.filter(dprep.col("cost") > 0)
```

Bu noktada, machine learning modeli ile kullanmak için tamamen dönüştürülmüş ve hazırlanmış veri akışı nesnesi vardır. SDK'sı şu şekilde kullanılır nesne seri hale getirme işlevselliğini içerir.

```python
import os
file_path = os.path.join(os.getcwd(), "dflows.dprep")

dflow_prepared = tmp_df
package = dprep.Package([dflow_prepared])
package.save(file_path)
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dosya Sil `dflows.dprep` (yerel olarak veya Azure not defterleri çalıştırdığınız olup olmadığını) ile devam etmek istemiyorsanız, geçerli dizinde öğreticinin iki bölüm. İki kısımdan devam ederseniz, ihtiyacınız olacak `dflows.dprep` geçerli dizin dosyası.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticinin bir parçası olarak:

> [!div class="checklist"]
> * Geliştirme ortamınızı kurma
> * Yüklenen ve Temizlenen veri kümeleri
> * Akıllı dönüşümler mantığınızı bir örneği temel alarak tahmin etmek için kullanılan
> * Birleştirilmiş ve paketli veri kümeleri için machine learning eğitimi

Bu öğretici serisinin bir sonraki kısmına eğitim verileri kullanmaya hazırsınız:

> [!div class="nextstepaction"]
> [Öğretici #2: Regresyon modeli eğitme](tutorial-auto-train-models.md)
