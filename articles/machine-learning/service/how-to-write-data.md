---
title: "Yazma: Python SDK'sı veri hazırlama"
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning veri hazırlığı SDK'sı ile veri yazma hakkında bilgi edinin. Herhangi bir noktada bir veri akışı (yerel dosya sistemi, Azure Blob Depolama ve Azure Data Lake depolama), desteklenen konumlardan birini dosyalarında ve veri dışarı yazabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 0275d27a0a27d0279886f6f7fd15b14d312a44ea
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472000"
---
# <a name="write-and-configure-data--with-the-azure-machine-learning-data-prep-sdk"></a>Yazma ve verileri Azure Machine Learning veri hazırlığı SDK ile yapılandırma

Bu makalede, farklı yöntemler kullanarak veri yazmak için bilgi [Azure Machine Learning veri hazırlığı Python SDK'sı](https://aka.ms/data-prep-sdk) ve deneme için bu verilerin nasıl yapılandırılacağına ilişkin [PythoniçinAzureMachineLearningSDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).  Çıktı verilerini, bir veri akışı herhangi bir noktada yazılabilir. Yazma işlemleri, veri akış çalıştırmaları her zaman ortaya çıkan veri akışı için adımlar ve aşağıdaki adımları çalıştırın olarak eklenir. Veriler, paralel yazma izin vermek için birden çok bölüm dosyaya yazılır.

> [!Important]
> Yeni bir çözüm oluşturuyorsanız deneyin [Azure Machine Learning veri kümeleri](how-to-explore-prepare-data.md) (Önizleme) verileri, anlık görüntü verileri dönüştürme ve tutulan veri kümesi tanımlarını depolar. Veri kümeleri, veri hazırlığı SDK'sı, yapay ZEKA çözümlerini veri kümelerini yönetmek için genişletilmiş işlevselliği sunan sonraki sürümüdür.
> Kullanırsanız `azureml-dataprep` Bağlantılarınızdaki kullanmak yerine bir veri akışı oluşturmak için paket `azureml-datasets` bir veri kümesini oluşturmak için paket, anlık görüntüler veya tutulan veri kümeleri, daha sonra kullanmak üzere mümkün olmayacaktır.

Kaç tane adımlar bir işlem hattında vardır yazma hiçbir sınırlama olduğundan, sorun giderme için veya diğer işlem hatları için Ara sonuçlar elde etmek için ek yazma adımları kolayca ekleyebilirsiniz.

Bir yazma adım her çalıştırdığınızda veri akışı verilerin tam bir çekme gerçekleşir. Örneğin, üç yazma adımları ile bir veri akışını okuma ve üç kez veri kümesindeki her kayıt işlemi.

## <a name="supported-data-types-and-location"></a>Desteklenen veri türleri ve konumu

Aşağıdaki dosya biçimlerini desteklenir
-   Ayrılmış dosyalar (CSV, TSV, vb.)
-   Parquet dosyalarını

Azure Machine Learning veri hazırlığı Python SDK'sını kullanarak veri yazabilirsiniz:
+ bir yerel dosya sistemi
+ Azure Blob Depolama
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Spark konuları

Bir veri akışı, Spark, çalıştırırken, boş bir klasöre yazmanız gerekir. Bir hata var olan bir klasörü sonuçlanır bir yazma çalıştırılmaya başlanıyor. Emin olun, hedef klasör boş veya her çalıştırma için farklı bir hedef konum kullanın veya yazma başarısız olur.

## <a name="monitoring-write-operations"></a>Yazma işlemleri izleme

Bir yazma işlemi tamamlandıktan sonra kolaylık olması için başarı adlı bir sentinel dosyası oluşturulur. Kendi durum tamamlamak tüm işlem hattı için beklemek zorunda kalmadan bir ara yazma tamamlandıktan sonra belirlemenize yardımcı olur.

## <a name="example-write-code"></a>Örnek kod yazma

Bu örnekte, bir veri akışı kullanarak verileri yükleyerek Başlat `auto_read_file()`. Bu farklı biçimlerin verilerle yeniden.

```python
import azureml.dataprep as dprep
t = dprep.auto_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(5)
```

Örnek çıktı:

| | Column1 | Column2 | Sütun3 | Sütun4 | Sütun5 | Sütun6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | None | NO | NO | ENRS | NaN | NaN | NaN |   
|1| 10003.0 | 99999.0 | None | NO | NO | ENSO | NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | None | NO | JN | ENJA | 70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | None | NO | NO |      | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | None | NO | NO | ENSO | 59783.0 | 5350.0 |  500.0|

### <a name="delimited-file-example"></a>Ayrılmış dosyası örneği

Aşağıdaki kod [ `write_to_csv()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#write-to-csv-directory-path--destinationpath--separator--str--------na--str----na---error--str----error------azureml-dataprep-api-dataflow-dataflow) ayrılmış bir dosyaya veri yazmak için işlevi.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

Örnek çıktı:

| | Column1 | Column2 | Sütun3 | Sütun4 | Sütun5 | Sütun6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | HATA | NO | NO | ENRS | NaN    | NaN | NaN |   
|1| 10003.0 | 99999.0 | HATA | NO | NO | ENSO |    NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | HATA | NO | JN | ENJA |    70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | HATA | NO | NO |     | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | HATA | NO | NO | ENSO |    59783.0 | 5350.0 |  500.0|

Yukarıdaki çıktıda çeşitli hatalar nedeniyle doğru şekilde ayrıştırıldı olmayan sayılar sayısal sütunlarda görünür. CSV'ye yazılırken, null değerler varsayılan olarak "ERROR" dizesi ile değiştirilir.

Parametreleri, yazma bir parçası olarak çağırın ve null değerleri temsil etmek için kullanılacak bir dize belirtin ekleyin.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

Yukarıdaki kod, bu çıktıyı üretir:

| | Column1 | Column2 | Sütun3 | Sütun4 | Sütun5 | Sütun6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | BadData | NO | NO | ENRS | NaN  | NaN | NaN |   
|1| 10003.0 | 99999.0 | BadData | NO | NO | ENSO |  NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | BadData | NO | JN | ENJA |  70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | BadData | NO | NO |   | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | BadData | NO | NO | ENSO |  59783.0 | 5350.0 |  500.0|

### <a name="parquet-file-example"></a>Parquet dosyası örneği

Benzer şekilde `write_to_csv()`, [ `write_to_parquet()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#write-to-parquet-file-path--typing-union--destinationpath--nonetype----none--directory-path--typing-union--destinationpath--nonetype----none--single-file--bool---false--error--str----error---row-groups--int---0-----azureml-dataprep-api-dataflow-dataflow) işlevi bir veri akış çalıştırmaları çalıştırılan Parquet adım yazma ile yeni bir veri akışı döndürür.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Yazma işlemi başlatmak için veri akışı çalıştırın.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(5)
```

Yukarıdaki kod, bu çıktıyı üretir:

|   | Column1 | Column2 | Sütun3 | Sütun4 | Sütun5 | Sütun6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |-------- |
|0| 10000.0 | 99999.0 | MiscreantData | NO | NO | ENRS | MiscreantData | MiscreantData | MiscreantData |
|1| 10003.0 | 99999.0 | MiscreantData | NO | NO | ENSO | MiscreantData | MiscreantData | MiscreantData |   
|2| 10010.0 | 99999.0 | MiscreantData | NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0 | 99999.0 | MiscreantData | NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0 | 99999.0 | MiscreantData | NO| NO| ENSO|   59783.0|    5350.0| 500.0|

## <a name="configure-data-for-automated-machine-learning-training"></a>Otomatik machine learning eğitim verilerini Yapılandır

Yeni yazılmış veri dosyanıza geçirmek bir [ `AutoMLConfig` ](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#automlconfig) hazırlık otomatik makine öğrenimi eğitim için nesne. 

Aşağıdaki kod örneği, veri akışı için bir Pandas dataframe dönüştürün ve daha sonra eğitim ve test veri kümeleri için otomatik makine öğrenimi eğitim bölün gösterilmektedir.

```Python
from azureml.train.automl import AutoMLConfig
from sklearn.model_selection import train_test_split

dflow = dprep.auto_read_file(path="")
X_dflow = dflow.keep_columns([feature_1,feature_2, feature_3])
y_dflow = dflow.keep_columns("target")

X_df = X_dflow.to_pandas_dataframe()
y_df = y_dflow.to_pandas_dataframe()

X_train, X_test, y_train, y_test = train_test_split(X_df, y_df, test_size=0.2, random_state=223)

# flatten y_train to 1d array
y_train.values.flatten()

#configure 
automated_ml_config = AutoMLConfig(task = 'regression',
                               X = X_train.values,  
                   y = y_train.values.flatten(),
                   iterations = 30,
                       Primary_metric = "AUC_weighted",
                       n_cross_validation = 5
                       )

```

Önceki örnekte hiçbir ara veri hazırlama adımları ister gerekmiyorsa, veri akışı doğrudan geçirebilirsiniz `AutoMLConfig`.

```Python
automated_ml_config = AutoMLConfig(task = 'regression', 
                   X = X_dflow,   
                   y = y_dflow, 
                   iterations = 30, 
                   Primary_metric = "AUC_weighted",
                   n_cross_validation = 5
                   )
```

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: SDK [genel bakış](https://aka.ms/data-prep-sdk) tasarım desenleri ve kullanım örnekleri 
* Otomatik makine öğrenimi bkz [öğretici](tutorial-auto-train-models.md) bir regresyon modeli örnek
