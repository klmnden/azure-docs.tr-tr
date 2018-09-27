---
title: Azure Machine Learning veri hazırlığı SDK - Python ile veri yazma
description: Azure Machine Learning veri hazırlığı SDK'sı ile veri yazma hakkında bilgi edinin. Herhangi bir noktada bir veri akışı (yerel dosya sistemi, Azure Blob Depolama ve Azure Data Lake depolama), desteklenen konumlardan birini dosyalarında ve veri dışarı yazabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 81344d388fbba0db034b8adb06adab6797ec2ce1
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166758"
---
# <a name="write-data-using-the-azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning veri hazırlığı SDK'sını kullanarak veri yazma
Verileri bir veri akışı herhangi bir noktada dışarıda yazabilirsiniz. Bu yazma elde edilen veri akışı adımları olarak eklenir ve veri akışı her zaman yeniden çalıştırılır. Veriler, paralel yazma izin vermek için birden çok bölüm dosyaya yazılır.

Kaç tane adımlar bir işlem hattında vardır yazma hiçbir sınırlama olduğundan, sorun giderme için veya diğer işlem hatları için Ara sonuçlar elde etmek için ek yazma adımları kolayca ekleyebilirsiniz. 

Bir yazma adım her çalıştırdığınızda veri akışı verilerin tam bir çekme gerçekleşir. Örneğin, üç yazma adımları ile bir veri akışını okuma ve üç kez kümesindeki her kayıt işlemi.

## <a name="supported-data-types-and-location"></a>Desteklenen veri türleri ve konumu

Aşağıdaki dosya biçimlerini desteklenir
-   Ayrılmış dosyalar (CSV, TSV, vb.)
-   Parquet dosyalarını

Kullanarak [Azure Machine Learning veri hazırlığı python SDK'sı](https://aka.ms/data-prep-sdk), veri yazabilirsiniz:
+ bir yerel dosya sistemi
+ Azure Blob Depolama
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Spark konuları
Bir veri akışı, Spark, çalıştırırken, boş bir klasöre yazmanız gerekir. Bir hata var olan bir klasörü sonuçlanır bir yazma çalıştırılmaya başlanıyor. Emin olun, hedef klasör boş veya her çalıştırma için farklı bir hedef konum kullanın veya yazma başarısız olur.

## <a name="monitoring-write-operations"></a>Yazma işlemleri izleme
Bir yazma işlemi tamamlandıktan sonra kolaylık olması için başarı adlı bir sentinel dosyası oluşturulur. Kendi durum tamamlamak tüm işlem hattı için beklemek zorunda kalmadan bir ara yazma tamamlandıktan sonra belirlemenize yardımcı olur.

## <a name="example-write-code"></a>Örnek kod yazma

Bu örnekte, bir veri akışına veri yükleyerek başlayın. Biz bu farklı biçimlerin verilerle yeniden kullanır.

```python
import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```   

Örnek çıktı:
|   |  Column1 |    Column2 | Sütun3 | Sütun4  |Sütun5   | Sütun6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   None|       HAYIR|     HAYIR  |   ENRS    |NaN    |   NaN |   NaN|    
|   1|      10003.0 |   99999.0 |   None|       HAYIR|     HAYIR  |   ENSO|       NaN|        NaN |NaN|   
|   2|  10010.0|    99999.0|    None|   HAYIR| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    None|   HAYIR| HAYIR| |   NaN|    NaN|    NaN|
|4| 10014.0|    99999.0|    None|   HAYIR| HAYIR| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    None|   HAYIR| HAYIR| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   None|   HAYIR| HAYIR|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    None|   HAYIR| HAYIR| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    None|   HAYIR| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    None|   HAYIR| SV|     |77000.0|   15500.0|    120.0|

### <a name="delimited-file-example"></a>Ayrılmış dosyası örneği

Bu bölümde, bir örnek kullanarak görebilirsiniz `write_to_csv` ile sınırlandırılmış bir dosyanın yazmak için işlevi.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Örnek çıktı:
|   |  Column1 |    Column2 | Sütun3 | Sütun4  |Sütun5   | Sütun6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   HATA |       HAYIR|     HAYIR  |   ENRS    |HATA    |   HATA |   HATA|    
|   1|      10003.0 |   99999.0 |   HATA |       HAYIR|     HAYIR  |   ENSO|       HATA|        HATA |HATA|   
|   2|  10010.0|    99999.0|    HATA |   HAYIR| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    HATA |   HAYIR| HAYIR| |   HATA|    HATA|    HATA|
|4| 10014.0|    99999.0|    HATA |   HAYIR| HAYIR| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    HATA |   HAYIR| HAYIR| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   HATA |   HAYIR| HAYIR|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    HATA |   HAYIR| HAYIR| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    HATA |   HAYIR| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    HATA |   HAYIR| SV|     |77000.0|   15500.0|    120.0|

Yukarıdaki çıktıda çeşitli hatalar nedeniyle doğru şekilde ayrıştırıldı olmayan sayılar sayısal sütunları görünen görebilirsiniz. CSV'ye yazılırken bu null değerler varsayılan olarak "ERROR" dizesi ile değiştirilir. 

Parametre, yazma bir parçası olarak çağırın ve null değerleri temsil etmek için kullanılacak bir dize belirtin ekleyebilirsiniz. Örneğin:

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```

Yukarıdaki kod, bu çıktıyı üretir:
|   |  Column1 |    Column2 | Sütun3 | Sütun4  |Sütun5   | Sütun6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   BadData |       HAYIR|     HAYIR  |   ENRS    |BadData    |   BadData |   BadData|    
|   1|      10003.0 |   99999.0 |   BadData |       HAYIR|     HAYIR  |   ENSO|       BadData|        BadData |BadData|   
|   2|  10010.0|    99999.0|    BadData |   HAYIR| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    BadData |   HAYIR| HAYIR| |   BadData|    BadData|    BadData|
|4| 10014.0|    99999.0|    BadData |   HAYIR| HAYIR| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    BadData |   HAYIR| HAYIR| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   BadData |   HAYIR| HAYIR|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    BadData |   HAYIR| HAYIR| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    BadData |   HAYIR| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    BadData |   HAYIR| SV|     |77000.0|   15500.0|    120.0|


### <a name="parquet-file-example"></a>Parquet dosyası örneği

Benzer şekilde `write_to_csv`, `write_to_parquet` işlevi bir veri akış çalıştırmaları çalıştırılan Parquet adım yazma ile yeni bir veri akışı döndürür.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Ardından, yazma işlemi başlatmak için veri akışı çalıştırabilirsiniz.

```
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```

Yukarıdaki kod, bu çıktıyı üretir:
|   |  Column1 |    Column2 | Sütun3 | Sütun4  |Sütun5   | Sütun6 | Column7 | Column8 | Column9 |
| -------- |  -------- | -------- | -------- |  -------- |  -------- |  -------- |  -------- |  -------- |  -------- |
| 0 |   10000.0 |   99999.0 |   MiscreantData |       HAYIR|     HAYIR  |   ENRS    |MiscreantData    |   MiscreantData |   MiscreantData|    
|   1|      10003.0 |   99999.0 |   MiscreantData |       HAYIR|     HAYIR  |   ENSO|       MiscreantData|        MiscreantData |MiscreantData|   
|   2|  10010.0|    99999.0|    MiscreantData |   HAYIR| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0|    99999.0|    MiscreantData |   HAYIR| HAYIR| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0|    99999.0|    MiscreantData |   HAYIR| HAYIR| ENSO|   59783.0|    5350.0| 500.0|
|5| 10015.0|    99999.0|    MiscreantData |   HAYIR| HAYIR| ENBL|   61383.0|    5867.0| 3270.0|
|6| 10016.0 |99999.0|   MiscreantData |   HAYIR| HAYIR|     |64850.0|   11233.0|    140.0|
|7| 10017.0|    99999.0|    MiscreantData |   HAYIR| HAYIR| ENFR|   59933.0|    2417.0| 480.0|
|8| 10020.0|    99999.0|    MiscreantData |   HAYIR| SV|     |80050.0|   16250.0|    80,0|
|9| 10030.0|    99999.0|    MiscreantData |   HAYIR| SV|     |77000.0|   15500.0|    120.0|
