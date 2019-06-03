---
title: 'Regresyon modeli öğretici: Verileri hazırlama'
titleSuffix: Azure Machine Learning service
description: Bu öğreticinin ilk bölümünde Azure Machine Learning SDK'sını kullanarak modelleme regresyon için Python uygulamasında veri hazırlama konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sihhu
ms.author: MayMSFT
ms.reviewer: trbye
ms.date: 03/29/2019
ms.custom: seodec18
ms.openlocfilehash: dabb43cb2fe9b66d5d83d163b74d2f22354e33b8
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418036"
---
# <a name="tutorial-prepare-data-for-regression-modeling"></a>Öğretici: Regresyon model için verileri hazırlama

Bu öğreticide, regresyon kullanarak model için verileri hazırlama hakkında bilgi edinin [veri hazırlama paketinde](https://aka.ms/data-prep-sdk) gelen [Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). Filtreleme ve iki farklı NYC taksi veri kümeleri birleştirmek için çeşitli dönüştürmeler çalıştırın.

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin birinci bölümüdür**. Öğretici serisinin tamamladıktan sonra taksi seyahat maliyetini veri özellikleri bir model eğitip geleceği tahmin edebilirsiniz. Bu özellikler, toplama gün ve saat, Yolcuların Sayısı ve toplama konumunu içerir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bir Python ortamını ayarlama ve paketleri içeri aktarın.
> * İki veri kümesi farklı alan adları ile yükleyin.
> * Anomalileri kaldırmak için verileri temizler.
> * Yeni özellikler oluşturmak için akıllı dönüşümler'ı kullanarak verileri dönüştürme.
> * Veri akışı nesnenizin nda bir regresyon modeli kaydedin.

## <a name="prerequisites"></a>Önkoşullar

Atlamak [geliştirme ortamınızı ayarlama](#start) not defteri adımları okuyun veya not defterini alma ve Azure not defterleri veya kendi notebook sunucusu üzerinde çalıştırmak için aşağıdaki yönergeleri kullanın. İhtiyacınız olacak not defteri çalıştırmak için:

* Aşağıdakilerin yüklü olan bir Python 3.6 Not Defteri sunucusu:
    * `azureml-dataprep` Paketi Azure Machine Learning SDK'sı
* Öğretici not defteri

* Kullanım bir [çalışma alanınızdaki bulut not defteri sunucusu](#azure) 
* Kullanım [kendi not defteri sunucusu](#server)

### <a name="azure"></a>Çalışma alanınızda bir bulut not defteri sunucusu kullan

Kendi bulut tabanlı bir not defteri sunucusu ile çalışmaya başlama daha kolaydır. Python için Azure Machine Learning SDK'sı zaten yüklü ve bu bulut kaynağı oluşturduktan sonra sizin için yapılandırılır.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Not Defteri Web sayfası başlattıktan sonra Çalıştır **öğreticiler/regresyon-bölüm 1-verileri-prep.ipynb** dizüstü bilgisayar.

### <a name="server"></a>Kendi Jupyter notebook sunucusu kullanma

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.  Adımları tamamladıktan sonra Çalıştır **öğreticiler/regresyon-bölüm 1-verileri-prep.ipynb** dizüstü bilgisayar.

1. Tam yükleme adımları [Azure Machine Learning Python hızlı](setup-create-workspace.md#sdk) Miniconda ortamı oluşturun ve SDK'sını yükleyin.  Atlayabilirsiniz **çalışma alanı oluşturma** istediğiniz, ancak bunun için ihtiyacınız olacak bölümünde [2. bölüm](tutorial-auto-train-models.md) Bu öğretici serisinin.
1. `azureml-dataprep` Paket SDK'yı yüklediğinizde otomatik olarak yüklenir.
1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```

## <a name="start"></a>Geliştirme ortamınızı ayarlama

Geliştirme çalışmanızdaki tüm kurulum bir Python not defterinde gerçekleştirilebilir. Kurulumu, aşağıdaki eylemleri içerir:

* SDK yükle
* Python paketlerini içeri aktarma

### <a name="install-and-import-packages"></a>Yükleme ve paketleri içeri aktarma

Bunları henüz yoksa, gerekli paketleri yüklemek için aşağıdakileri kullanın.

```shell
pip install "azureml-dataprep[pandas]>=1.1.0,<1.2.0"
```

Paketi içeri aktarın.

```python
import azureml.dataprep as dprep
```

> [!IMPORTANT]
> En son sürümü yükleyin emin olun. Bu öğreticide, sürüm numarası 1.1.0 düşük ile çalışmaz

## <a name="load-data"></a>Veri yükleme

İki farklı NYC taksi veri kümesi, veri akışı nesneleri indirin. Veri kümeleri biraz farklı alanlara sahiptir. `auto_read_file()` Yöntemi giriş dosya türü otomatik olarak tanır.

```python
from IPython.display import display
dataset_root = "https://dprepdata.blob.core.windows.net/demo"

green_path = "/".join([dataset_root, "green-small/*"])
yellow_path = "/".join([dataset_root, "yellow-small/*"])

green_df_raw = dprep.read_csv(path=green_path, header=dprep.PromoteHeadersMode.GROUPED)
# auto_read_file automatically identifies and parses the file type, which is useful when you don't know the file type.
yellow_df_raw = dprep.auto_read_file(path=yellow_path)

display(green_df_raw.head(5))
display(yellow_df_raw.head(5))
```

A `Dataflow` nesne için bir dataframe benzer ve bir dizi gevşek değerlendirilir, sabit işlem verilerini temsil eder. İşlemleri farklı dönüştürmeyi çağırma ve kullanılabilen yöntemler filtreleme olarak eklenebilir. İşlem için eklemenin sonucunu bir `Dataflow` her zaman yeni olan `Dataflow` nesne.

## <a name="cleanse-data"></a>Verilerini temizlemek

Şimdi bazı değişkenler için tüm veri akışlarını uygulamak için kısayol dönüşümler ile doldurun. `drop_if_all_null` Değişkeni kayıtları silmek için kullanılan tüm alanların null olduğu. `useful_columns` Değişken her veri akışı tutulur sütun açıklamaları dizisi tutar.

```python
all_columns = dprep.ColumnSelector(term=".*", use_regex=True)
drop_if_all_null = [all_columns, dprep.ColumnRelationship(dprep.ColumnRelationship.ALL)]
useful_columns = [
    "cost", "distance", "dropoff_datetime", "dropoff_latitude", "dropoff_longitude",
    "passengers", "pickup_datetime", "pickup_latitude", "pickup_longitude", "store_forward", "vendor"
]
```

İlk sarı taksi verileri birleştirilebilir geçerli bir şeklin içine almak için yeşil taksi verilerle çalışın. Çağrı `replace_na()`, `drop_nulls()`, ve `keep_columns()` işlevleri kısayolunu kullanarak oluşturduğunuz değişkenleri dönüştürün. Ayrıca, veri çerçevesi adlarını eşleştirmek için tüm sütunları yeniden adlandırma `useful_columns` değişkeni.


```python
green_df = (green_df_raw
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
green_df.head(5)
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

Aynı dönüştürme adımı sarı taksi verileri üzerinde çalıştırın. Bu işlevler, null veri machine learning artıracağına veri kümesi kaldırıldığından emin olmak model doğruluğu.

```python
yellow_df = (yellow_df_raw
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
yellow_df.head(5)
```

Çağrı `append_rows()` sarı taksi verileri eklemek için yeşil taksi verileri işlevi. Yeni bir birleşik dataframe oluşturulur.

```python
combined_df = green_df.append_rows([yellow_df])
```

### <a name="convert-types-and-filter"></a>Türleri ve filtre Dönüştür

Verilerin nasıl dağıtılması görmek için alma ve teslim koordinatları Özet istatistikleri inceleyin. İlk olarak tanımlayan bir `TypeConverter` decimal türü için enlem ve boylam alanları değiştirmek için nesne. Ardından, arama `keep_columns()` çıkış yalnızca enlem ve boylam alanları kısıtlamak için işlev ve çağırma `get_profile()` işlevi. Bu işlev çağrıları kolaylaştıran değerlendirmek eksik lat/uzun alanlar veya kapsam dışı koordinatları göstermek için veri akışı sıkıştırılmış bir görünümünü oluşturun.


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
      <th>Type</th>
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



Özet istatistikleri çıktısı olup görmek koordinatları ve New York City (Bu belirlenir öznel analiz) olmayan koordinatları eksik. Koordinatları Şehir kenarlık dışında olan konumlar için filtreleyin. Sütun filtresi zinciri komutları içinde `filter()` çalışır ve her bir alan için minimum ve maksimum sınırları tanımlayın. Ardından çağırın `get_profile()` yeniden dönüştürme doğrulamak için işlevi.


```python
latlong_filtered_df = (combined_df
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
latlong_filtered_df.keep_columns(columns=[
    "pickup_longitude", "pickup_latitude",
    "dropoff_longitude", "dropoff_latitude"
]).get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
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

### <a name="split-and-rename-columns"></a>Bölme ve sütunları yeniden adlandırma

Veri profili bakın `store_forward` sütun. Bu alan, bir Boole bayrağı olduğu `Y` zaman taksi sunucuya bağlantı seyahat sonrasında yoktu ve dolayısıyla bellekte seyahat verilerini depolamak ve daha sonra sunucuya bağlıyken ileterek gerekiyordu.


```python
latlong_filtered_df.keep_columns(columns='store_forward').get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
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



Veri profili çıkış bildirimi `store_forward` sütun veri tutarsız ve eksik veya null değerler olduğunu gösterir. Kullanım `replace()` ve `fill_nulls()` işlevleri bu değerleri "N" dizesi ile değiştirin:


```python
replaced_stfor_vals_df = latlong_filtered_df.replace(columns="store_forward", find="0", replace_with="N").fill_nulls("store_forward", "N")
```

Yürütme `replace` işlevini `distance` alan. İşlev olarak yanlış etiketli uzaklık değerleri yeniden biçimlendirir `.00`, herhangi bir null değerlere sıfır ile doldurur. Dönüştürme `distance` sayısal biçimi alanı. Bu yanlış veri olası anormallikleri taxi cab verileri koleksiyonu sisteminde noktalarıdır.


```python
replaced_distance_vals_df = replaced_stfor_vals_df.replace(columns="distance", find=".00", replace_with=0).fill_nulls("distance", 0)
replaced_distance_vals_df = replaced_distance_vals_df.to_number(["distance"])
```

Toplama ve dropoff datetime değerleri kendi tarih ve saat sütunlarına bölün. Kullanım `split_column_by_example()` bölme yapmak için işlevi. Bu durumda, isteğe bağlı `example` parametresinin `split_column_by_example()` işlevi atlanmış. Bu nedenle, işlev, veriler temel alınarak bölmek nereye otomatik olarak belirler.


```python
time_split_df = (replaced_distance_vals_df
    .split_column_by_example(source_column="pickup_datetime")
    .split_column_by_example(source_column="dropoff_datetime"))
time_split_df.head(5)
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

Tarafından oluşturulan sütunları yeniden adlandırma `split_column_by_example()` işlevini anlamlı adlar kullanın.

```python
renamed_col_df = (time_split_df
    .rename_columns(column_pairs={
        "pickup_datetime_1": "pickup_date",
        "pickup_datetime_2": "pickup_time",
        "dropoff_datetime_1": "dropoff_date",
        "dropoff_datetime_2": "dropoff_time"
    }))
renamed_col_df.head(5)
```

Çağrı `get_profile()` tüm adımları temizleme tam Özet istatistikleri görmek için işlev.

```python
renamed_col_df.get_profile()
```

## <a name="transform-data"></a>Verileri dönüştürme

Toplama ve dropoff tarihe daha fazla gün hafta, gün ay ve ayın değerleri bölün. Gün haftanın değeri almak için kullanın `derive_column_by_example()` işlevi. İşlevi, giriş verilerinin ve tercih edilen çıkış tanımlayan örnek nesnelerin bir dizi parametresi alır. İşlevi, tercih edilen dönüşümünüzü otomatik olarak belirler. Toplama ve dropoff saat sütunları için saat saat, dakika ve saniye kullanarak bölme `split_column_by_example()` işlevi ile örnek parametre yok.

Yeni özellikler oluşturduktan sonra kullanma `drop_columns()` işlevi yeni oluşturulan özellikleri tercih edilen olarak özgün alanları silinecek. Geri kalan alanları anlamlı açıklamalar kullanmak için yeniden adlandırın.

Zamana bağlı yeni bir özellik oluşturmak için bu şekilde verileri dönüştürme machine learning artıracak model doğruluğu. Örneğin, haftanın günü, haftanın günü taksi taksi Fiyat arasında bir ilişki kurmak yardımcı olacak için yeni bir özellik oluşturulamadı, durum genellikle bu şekildedir daha yüksek talep nedeniyle haftanın belirli günlerinde pahalıdır.


```python
transformed_features_df = (renamed_col_df
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
    # The following two calls to split_column_by_example reference the column names generated from the previous two calls.
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

transformed_features_df.head(5)
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

Türetilen dönüştürmeleri alma ve dropoff tarih ve saat bileşenlerini üretilen veri gösterir doğru olduğuna dikkat edin. DROP `pickup_datetime` ve `dropoff_datetime` sütunlar artık olduğundan (saat, dakika ve saniyenin modeli eğitimi için daha kullanışlı olduğu gibi ayrıntılı zamanı özellikleri) gerekli.


```python
processed_df = transformed_features_df.drop_columns(columns=["pickup_datetime", "dropoff_datetime"])
```

Her bir alanın veri türü otomatik olarak denetlemek için tür çıkarımı işlevselliği kullanmak ve çıkarım sonuçları görüntüleyebilirsiniz.


```python
type_infer = processed_df.builders.set_column_types()
type_infer.learn()
type_infer
```

Sonuç çıktısı `type_infer` gibidir.

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

Çıkarım sonuçlar doğru verileri temel alan görünür. Tür dönüştürmeleri veri akışı için şimdi başvurun.


```python
type_converted_df = type_infer.to_dataflow()
type_converted_df.get_profile()
```

Veri akışı paketlemeden önce veri kümesinde son iki filtre çalıştırın. Yanlış yakalanan veri noktalarını ortadan kaldırmak için veri akışı kayıtlar filtre nerede hem `cost` ve `distance` değişken değerleri sıfırdan büyük. Bu adım, veri noktaları sıfır maliyet çünkü doğruluğu, öğrenme makine ya da devre dışı tahmin doğruluğunu throw uzaklık temsil ana aykırı önemli ölçüde artırır.

```python
final_df = type_converted_df.filter(dprep.col("distance") > 0)
final_df = final_df.filter(dprep.col("cost") > 0)
```

Artık bir machine learning modeli ile kullanılacak bir tamamen dönüştürülmüş ve hazırlanmış veri akışı nesne var. SDK'sı, aşağıdaki kodda gösterildiği gibi kullanılan nesne seri hale getirme işlevselliğini içerir.

```python
import os

file_path = os.path.join(os.getcwd(), "dflows.dprep")
final_df.save(file_path)
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticinin ikinci Kısım ile devam etmek için ihtiyacınız **dflows.dprep** geçerli dizin dosyası.

Bölümüne iki devam etmeyi planlamıyorsanız, silme **dflows.dprep** geçerli dizininizde dosya. Bu dosya, yürütme yerel olarak çalışıyor olsun veya silin [Azure not defterleri](https://notebooks.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticinin bir parçası olarak:

> [!div class="checklist"]
> * Geliştirme ortamınızı ayarlayın.
> * Yüklenen ve Temizlenen veri kümeleri.
> * Akıllı dönüşümler mantığınızı bir örneği temel alarak tahmin etmek için kullanılır.
> * Birleştirilmiş ve paketli veri kümeleri için makine öğrenimi eğitim.

Öğreticinin iki eğitim verileri kullanmaya hazırsınız:

> [!div class="nextstepaction"]
> [Öğretici (ikinci Kısım): Regresyon modeli eğitme](tutorial-auto-train-models.md)
