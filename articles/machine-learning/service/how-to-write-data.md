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
ms.openlocfilehash: da5823473b7f69fa0a6c65d5ea7319bfd2e92720
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946774"
---
# <a name="write-data-with-the-azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning veri hazırlığı SDK'sı ile veri yazma
Verileri bir veri akışı herhangi bir noktada dışarıda yazabilirsiniz. Bu yazma elde edilen veri akışı adımları olarak eklenir ve veri akışı her zaman yeniden çalıştırılır. Kaç tane adımlar bir işlem hattında vardır yazma hiçbir sınırlama olduğundan, sorun giderme için Ara sonuçlar kullanıma yazmak için veya başka bir işlem hattı tarafından işlenmek üzere kolay bir işlemdir. Veri akışı verilerin tam bir çekme çalıştırma yazma her adımın sonuçları verdiğine dikkat edin önemlidir. Örneğin, üç yazma adımları ile bir veri akışını okuma ve üç kez kümesindeki her kayıt işlemi.

## <a name="writing-to-files"></a>Dosyalara yazma
İle [Azure Machine Learning veri hazırlığı SDK'sı](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py), müşterilerimizin desteklenen konumlardan (yerel dosya sistemi, Azure Blob Depolama ve Azure Data Lake depolama) birini dosyalarında veri yazabilirsiniz. Veriler, paralel yazma izin vermek için birden çok bölüm dosyaya yazılır. Yazma tamamlandıktan sonra başarı adlı bir sentinel dosyası da oluşturulur. Bu bir ara yazma tamamlamak tüm işlem hattı için beklemek zorunda kalmadan tamamlandığında belirlemenize yardımcı olur.

Bir veri akışı, Spark, çalıştırırken, boş bir klasöre yazmanız gerekir. Varolan bir klasöre yazma çalıştırma denemesi başarısız olur. Emin olun, hedef klasör boş veya her çalıştırma için farklı bir hedef konum kullanın veya yazma başarısız olur.

Azure Machine Learning veri hazırlığı SDK'sı, aşağıdaki dosya biçimlerini destekler:
-   Ayrılmış dosyalar (CSV, TSV, vb.)
-   Parquet dosyalarını

Bu örnekte, bir veri akışına veri yükleyerek başlayın. Biz bu farklı biçimlerin verilerle yeniden kullanır.

```

import azureml.dataprep as dprep
t = dprep.smart_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(10)
```   

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

## <a name="delimited-files"></a>Ayrılmış dosyalar
Aşağıdaki satırı bir yazma adım ile yeni bir veri akışı oluşturur, ancak gerçek yazma değil henüz oluştu. Veri akış çalıştırmaları, yazma yürütülür.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))
```
Şimdi yazma işlemi çalışan veri akışı çalıştırın.
```
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
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

Yazılan verileri nedeniyle doğru şekilde ayrıştırıldı olmayan sayılar sayısal sütunlardaki çeşitli hatalar içeriyor. CSV'ye yazılırken bu null değerler varsayılan olarak "ERROR" dizesi ile değiştirilir. Parametre, yazma bir parçası olarak çağırın ve null değerleri temsil etmek için kullanılacak bir dize belirtin ekleyebilirsiniz.

```
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(10)
```
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
## <a name="parquet-files"></a>Parquet dosyalarını

 Benzer şekilde `write_to_csv` işlevi yukarıdaki `write_to_parquet` yazma veri akış çalıştırmaları, yürütülecek Parquet adım ile yeni bir veri akışı döndürür.

```
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```
 Artık yazma işlemini yürütür veri akışı çalıştırıyoruz.

```
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(10)
```
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
