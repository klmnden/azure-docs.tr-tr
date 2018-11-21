---
title: Python – Azure Machine Learning veri hazırlığı SDK'sı ile verileri hazırlama
description: Python için Azure Machine Learning veri hazırlığı SDK çeşitli biçimlerden veri yükleme, daha kullanışlı olması için dönüştürmek ve Modellerinizi erişmek için bir konum verileri yazmak için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 11/20/2018
ms.openlocfilehash: 08510961616d2be8eac9b6a19063d5f0d613321f
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263307"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Azure Machine Learning ile model için verileri hazırlama
 
Bu makalede, Azure Machine Learning veri hazırlığı SDK'ın benzersiz özellikleri ve kullanım örnekleri hakkında bilgi edinin. Veri hazırlama, bir machine learning iş akışı, en önemli parçasıdır. Tutarsız veya önemli temizleme ve Dönüştürme Eğitim verisi olarak kullanılan kurulamıyor gerçek veriler genellikle ayrılır. Hatalar ve ham verilerdeki anomaliler düzeltme ve çözmeye çalıştığınız soruna ilgili yeni özellikler derlemeye doğruluğu artırır.

Python kullanarak veri hazırlayabilir [Azure Machine Learning veri hazırlığı SDK'sı](https://aka.ms/data-prep-sdk).

## <a name="azure-machine-learning-data-prep-sdk"></a>Azure Machine Learning veri hazırlığı SDK'sı

[Azure Machine Learning veri hazırlığı SDK'sı](https://aka.ms/data-prep-sdk) içeren bir Python kitaplığı:
+ Birçok ortak veri ön işleme araçları
+ Otomatik özellik Mühendisliği ve dönüştürmeler örneklerden türetilmiş

SDK gibi popüler kitaplıklara çekirdek işlevindeki benzer **Pandas** ve **PySpark**, henüz daha fazla esneklik sunar. Bellek kapasitesi kısıtlamaları performansı etkilemeden önce pandas genellikle daha küçük veri kümeleri üzerinde (< 2-5 GB) en yararlı olur. Buna karşılık, PySpark genellikle büyük veri uygulamaları için ancak daha yavaş küçük veri kümeleri ile çalışmayı yapan bir ek taşır.

Azure Machine Learning veri hazırlığı SDK'sı sunar:
- Practicality ve küçük veri kümeleriyle çalışırken kullanışlı

- Büyük veri uygulamaları modern ölçeklenebilirliği

- Her iki kullanım örnekleri aynı kodu ölçeklendirin olanağı

### <a name="install-the-sdk"></a>SDK yükle

Aşağıdaki komutu kullanarak Python ortamınızda SDK'sını yükleyin.

```shell
pip install azureml-dataprep
```

Paketini içeri aktarmak için aşağıdaki kodu kullanın.

```python
import azureml.dataprep as dprep
```

### <a name="examples-and-reference"></a>Örnekler ve başvuru

Bu SDK'sının işlevleri ve modülleri hakkında bilgi edinmek için [veri hazırlığı SDK başvuru belgeleri](https://aka.ms/data-prep-sdk).

Aşağıdaki örnekler SDK'ın benzersiz işlevlerinden bazıları vurgulayın dahil olmak üzere:
+ Otomatik dosya türü algılama
+ Otomatik özellik Mühendisliği
+ Özet istatistikleri

#### <a name="automatic-file-type-detection"></a>Otomatik dosya türü algılama

Kullanım `smart_read_file()` dosya türünü belirtmenize gerek kalmadan verilerinizi yüklemek için işlevi. Bu işlev, otomatik olarak algılar ve dosya türünü ayrıştırır.

```python
dataflow = dprep.smart_read_file(path="<your-file-path>")
```

#### <a name="automated-feature-engineering"></a>Otomatik özellik Mühendisliği

Bölme ve örnek ve özellik Mühendisliği otomatik hale getirmek için çıkarım sütun türetmek için SDK'yi kullanın. Veri akışı nesnenizin adlı bir alana sahip `datetime` değeriyle `2018-09-15 14:30:00`.

Otomatik olarak ayırmak için `datetime` alanında, aşağıdaki işlev çağrısı.

```python
new_dataflow = dataflow.split_column_by_example(source_column="datetime")
```

Örnek parametre tanımlamayarak işlevi otomatik olarak bölecek `datetime` iki yeni alanlar alanını `datetime_1` ve `datetime_2`. Sonuçta elde edilen değerler `2018-09-15` ve `14:30:00`sırasıyla. Bir örnek desen sağlamak da mümkündür ve SDK'sı tahmin edin ve hedeflenen dönüşümünüzü yürütün. Aynı kullanarak `datetime` nesne, aşağıdaki kod, yeni bir sütun oluşturur `datetime_weekday` sağlanan örneği temel alarak haftanın günü için.

```python
new_dataflow = dataflow.derive_column_by_example(
        source_columns="datetime", 
        new_column_name="datetime_weekday", 
        example_data=[("2009-01-04 10:12:00", "Sunday"), ("2013-08-22 17:00:00", "Thursday")]
    )
```

#### <a name="summary-statistics"></a>Özet istatistikleri

Bir kod satırı ile bir veri akışı için hızlı Özet istatistikleri oluşturabilirsiniz. Bu yöntem, verilerinizi ve nasıl dağıtıldığını anlamak için kullanışlı bir yol sunar.

```python
dataflow.get_profile()
```

Bu fonksiyonu çağıran bir veri akışı nesnesinde çıkış aşağıdaki tabloda gibi sonuçlanır.

![Özet istatistikleri çıkış](./media/concept-data-preparation/output-example.png)

## <a name="multiple-environment-compatibilities"></a>Birden çok ortam uyumluluğunu

SDK ayrıca sıralanabilir ve açılan veri akışı nesneleri sağlar *herhangi* Python ortamı. Burada açıldığında ortam, kayıtlı olduğu ortama farklı olabilir. Python ortamları ve Azure Machine Learning modellerini ile hızlı tümleştirme arasında kolayca aktarabilmek için bu işlevi sağlar.

Veri akışı nesnelerinizi kaydetmek için aşağıdaki kodu kullanın.

```python
package = dprep.Package([dataflow_1, dataflow_2])
package.save("<your-local-path>")
```

Paketiniz her türlü ortamda yeniden açın ve veri akışı nesnelerin bir listesini almak için aşağıdaki kodu kullanın.

```python
package = dprep.Package.open("<your-local-path>")
dataflow_list = package.dataflows
```

## <a name="data-preparation-pipeline"></a>Veri hazırlama işlem hattı

Ayrıntılı örnekler ve her hazırlama adımı kodunu görmek için aşağıdaki nasıl yapılır kılavuzları kullanın:

1. [Veri yükleme](how-to-load-data.md), çeşitli biçimlerde olabilir
2. [Dönüştürme](how-to-transform-data.md) içine daha kullanışlı bir yapısı
3. [Yazma](how-to-write-data.md) söz konusu veri modellerinize erişilebilir bir konuma

![Veri hazırlama işlemine](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme bir [örneğin Not Defteri](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/tutorials/getting-started/getting-started.ipynb) , Azure Machine Learning veri hazırlığı SDK'sını kullanarak veri hazırlama.

Azure Machine Learning veri hazırlığı SDK [başvuru belgeleri](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py).
