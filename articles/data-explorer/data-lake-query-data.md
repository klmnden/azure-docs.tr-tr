---
title: Azure Veri Gezgini'ni kullanarak Azure Data Lake veri sorgulama
description: Azure Veri Gezgini'ni kullanarak Azure Data Lake veri sorgulamayı öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d6a58d144482e17f7e4b615134115d1da46af6f0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453183"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer-preview"></a>Azure Data Lake, Azure Veri Gezgini (Önizleme) kullanarak verileri Sorgulama

Azure Data Lake Store, büyük veri analizi için yüksek oranda ölçeklenebilir ve ekonomik veri lake çözümüdür. İçgörülere daha hızlı bir şekilde ulaşabilmeniz için yüksek performanslı dosya sisteminin gücünü büyük ölçek ve tasarrufla sunar. Analiz iş yükleri için iyileştirilmiş olan Data Lake Storage Gen2, Azure Blob Depolama özelliklerini geliştirir.
 
Azure Veri Gezgini, Azure Blob Depolama ile tümleşir ve Azure Data Lake depolama hızlı sağlama Gen2, önbelleğe alınmış ve veri gölü'nde erişim dizini. Analiz ve veri gölü, Azure Veri Gezgini içine önceki alma olmadan'nde sorgu. Alınan ve uningested yerel lake veri üzerinde aynı anda sorgulayabilirsiniz.  

> [!TIP]
> Azure Veri Gezgini içinde veri alımı en iyi sorgu performansını gerektirir. Azure Data Lake depolama Gen2 verilerde sorgu önceki alma olmadan özelliği yalnızca geçmiş verileri veya nadiren sorgulanan veriler için kullanılmalıdır.
 
## <a name="optimize-query-performance-in-the-lake"></a>Gölü'nde sorgu performansını iyileştirme 

* Performans ve en iyi duruma getirilmiş sorgu süresini bölüm veri.
* (En iyi sıkıştırma gzip, en iyi performans için lz4) performansı için verileri sıkıştırın.
* Azure Veri Gezgini kümeniz ile aynı bölgede ile Azure Blob Depolama veya Azure Data Lake depolama Gen2'ı kullanın. 

## <a name="create-an-external-table"></a>Bir dış tablo oluşturma

1. Kullanım `.create external table` Azure veri Gezgini'nde bir dış tablo oluşturma için komutu. Yeni bir dış tablo gibi komutları `.show`, `.drop`, ve `.alter` bölümünde belgelendirilen [dış tablo komutları](/azure/kusto/management/externaltables).

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```

    Bu sorgu, günlük bölümler oluşturur *container1/yyyy/MM/dd/all_exported_blobs.csv*. Performans artışı ile daha ayrıntılı bölümleme bekleniyor. Örneğin, sorgular Yukarıdakilerden biri gösterildiği gibi günlük bölümler ile dış tablolar üzerinden aylık bölümlenmiş tabloları ile bu sorgular daha iyi performans sahip olur.

    > [!NOTE]
    > Şu anda desteklenen depolama hesapları, Azure Blob Depolama veya Azure Data Lake depolama Gen2 ' dir. Şu anda desteklenen veri biçimlerini, csv, tsv ve txt vardır.

1. Dış tablo Web kullanıcı arabirimini'nın sol bölmesinde görünür

    ![Dış tablo Web kullanıcı Arabirimi](media/data-lake-query-data/external-tables-web-ui.png)
 
### <a name="external-table-permissions"></a>Dış tablo izinleri
 
* Veritabanı kullanıcısı, bir dış tablo oluşturabilirsiniz. Tablo Oluşturucusu otomatik olarak tablo yönetici olur.
* Küme, veritabanı veya tablo yönetici mevcut bir tabloyu düzenleyebilirsiniz.
* Herhangi bir veritabanı kullanıcısı veya okuyucu bir dış tablo sorgulayabilirsiniz.
 
## <a name="query-an-external-table"></a>Sorgu bir dış tablo
 
Dış bir tabloyu sorgulamak üzere kullanmak `external_table()` işlev ve işlev bağımsız değişken olarak tablo adı sağlayın. Sorgu geri kalanı standart Kusto sorgu dilidir.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> IntelliSense, dış tablo sorguları üzerinde şu anda desteklenmemektedir.

## <a name="query-external-and-ingested-data-together"></a>Dış ve alınan verileri birlikte sorgulama

Dış tablolar hem aynı sorgu içinde alınan veri tabloları sorgulama yapabilirsiniz. [ `join` ](/azure/kusto/query/joinoperator) Veya [ `union` ](/azure/kusto/query/unionoperator) ek verileri Azure Veri Gezgini, SQL Server veya diğer kaynaklar ile dış tablo. Kullanım bir [ `let( ) statement` ](/azure/kusto/query/letstatement) bir dış tablo başvurusu için Toplu özellik adı atamak için.

Aşağıdaki örnekte *ürünleri* alınan veri tablosu ve *ArchivedProducts* Azure Data Lake depolama Gen2 verileri içeren bir dış tablo:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Sorgu *TaxiRides* Yardım kümesindeki dış tablo

*TaxiRides* örnek veri kümesini, New York City taksi verilerini içerir [NYC taksi ve Limousine komisyon](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

### <a name="create-external-table-taxirides"></a>Create External table *TaxiRides* 

> [!NOTE]
> Bu bölümde gösterilmiştir oluşturmak için kullanılan sorgu *TaxiRides* external table içinde *yardımcı* kümesi. Bu tablo zaten oluşturulduğundan bu bölümü atlayarak gerçekleştirmek [sorgu *TaxiRides* dış tablo verilerini](#query-taxirides-external-table-data). 

1. Aşağıdaki sorgu, dış tablo oluşturmak için kullanılan *TaxiRides* Yardım kümesindeki. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
    h@'https://externalkustosamples.blob.core.windows.net/taxiridesbyday?st=2019-06-18T14%3A59%3A00Z&se=2029-06-19T14%3A59%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=yEaO%2BrzFHzAq7lvd4d9PeQ%2BTi3AWnho8Rn8hGU0X30M%3D'
    )
    ```
1. Sonuçta elde edilen tablo oluşturulduğu *yardımcı* küme:

    ![TaxiRides dış tablo](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Sorgu *TaxiRides* dış tablo verileri 

Oturum [ https://dataexplorer.azure.com/clusters/help/databases/Samples ](https://dataexplorer.azure.com/clusters/help/databases/Samples) Sorgulanacak *TaxiRides* dış tablo. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Sorgu *TaxiRides* bölümleme olmadan dış tablo

[Bu sorguyu çalıştırmak](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) dış tablosunda *TaxiRides* sürmeye haftanın her günü için tüm veri kümesinde göstermek için. 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Bu sorgu, haftanın en yoğun saatinde gösterir. Bu sorgu, veri bölümlenmiş olmadığından bu yana (en fazla birkaç dakika) sonuçları döndürmek için bir uzun zaman alabilir.

![bölümlenmemiş sorgu oluşturma](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Dış tablo bölümleme ile TaxiRides sorgulama 

[Bu sorguyu çalıştırmak](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) dış tablosunda *TaxiRides* taxi cab türleri (sarı veya yeşil), Ocak 2017'de kullanılan gösteriliyor. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

Bu sorgu, sorgu süresini ve performansı iyileştirir bölümleme, kullanır. Sorgu, bölümlenmiş sütun (pickup_datetime) filtreler ve birkaç saniye içinde sonuçlarını döndürür.

![bölümlenmiş bir sorgu oluşturma](media/data-lake-query-data/taxirides-with-partition.png)
  
Dış tablo üzerinde çalıştırmak için ek sorgular yazabilirsiniz *TaxiRides* ve veriler hakkında daha fazla bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Gezgini'ni kullanarak Azure Data Lake, verilerinizi sorgulayın. Öğrenme [sorguları yazma](write-queries.md) ve ek Öngörüler verilerinizden türetilir.