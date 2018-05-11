---
title: Azure Machine Learning veri hazırlıklar ile Python genişletilebilirlik kullanın | Microsoft Docs
description: Bu belge genel bir bakış ve Python kodu veri hazırlığı işlevselliğini genişletmek için nasıl kullanılacağını ayrıntılı bazı örnekleri sağlar
services: machine-learning
author: euangMS
ms.author: euang
ms.service: machine-learning
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.openlocfilehash: 6363d39b2dfbd36ccebff6780e35caf58ca84dda
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="data-preparations-python-extensions"></a>Veriler hazırlıkları Python uzantıları
Yerleşik özellikleri arasında işlevselliği aralıklar doldurma bir yolu olarak Azure Machine Learning veriler hazırlıkları birçok düzeyde genişletilebilirlik içerir. Bu belgede, Python komut dosyası aracılığıyla genişletilebilirlik verilmiştir. 

## <a name="custom-code-steps"></a>Özel kod adımları 
Veriler hazırlıkları kullanıcılar kod burada yazabilirsiniz aşağıdaki özel adımlar vardır:

* Sütun ekleme
* Gelişmiş Filtre
* Veri akışı dönüştürme
* Bölüm dönüştürme

## <a name="code-block-types"></a>Kod bloğu türleri 
Bu adımların her biri için iki kod bloğunun türü destekliyoruz. İlk olarak, olarak yürütülen tam bir Python ifadesi destekliyoruz. İkinci olarak, bilinen bir imza ile belirli bir işlev sağladığınız kodda burada diyoruz bir Python modülü destekler.

Örneğin, aşağıdaki iki yolla başka bir sütuna günlük hesaplar yeni bir sütun ekleyebilirsiniz:

İfade 

```python    
    math.log(row["Score"])
```

Modül 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


Adlı bir işlev bulmak Sütun Ekle dönüştürme modülü modunda bekliyor `newvalue` satır değişkeni kabul eder ve sütununun değeri döndürür. Bu modül herhangi diğer işlevleri, içeri aktarmalar, vb. ile Python kodu miktarını içerebilir.

Her bir uzantı noktası Ayrıntılar aşağıdaki bölümlerde ele alınmıştır. 

## <a name="imports"></a>İçeri aktarmalar 
İfade blok türü kullanıyorsanız, yine de ekleyebilirsiniz **alma** kodunuzu deyimlerini. Bunların tümü kodunuzu üst satırlarında gruplandırılmalıdır.

Düzeltin 

```python
import math 
import numpy 
math.log(row["Score"])
```
 

Hata  

```python
import math  
math.log(row["Score"])  
import numpy
```
 
 
Modül blok türü kullanırsanız, kullanarak tüm normal Python kuralları takip edebilirsiniz **alma** deyimi. 

## <a name="default-imports"></a>Varsayılan içeri aktarmalar
Aşağıdaki içeri aktarmaları her zaman dahil ve kodunuzda kullanılabilir. Bunları yeniden içe aktarmanız gerekmez. 

```python
import math  
import numbers  
import datetime  
import re  
import pandas as pd  
import numpy as np  
import scipy as sp
```
  

## <a name="install-new-packages"></a>Yeni paket yüklemek için
Varsayılan olarak yüklü olmayan bir paket kullanmak için ilk veriler hazırlıkları kullanan ortamlara yüklemeniz gerekir. Bu yükleme yerel makinenizde ve çalıştırmak istediğiniz herhangi bir işlem hedefleri yapılmalıdır.

Bir işlem hedef paketlerinizi yüklemek için projenizi kökü altındaki aml_config klasöründe bulunan conda_dependencies.yml dosyasını değiştirmeniz gerekir.

### <a name="windows"></a>Windows 
Windows konumu bulmak için uygulamaya özgü yüklemeyi Python ve komut dosyaları dizinini bulun. Varsayılan konumu şudur:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts` 

Ardından aşağıdaki komutlardan birini çalıştırın: 

`conda install <libraryname>` 

or 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
Mac'te konumunu bulmak için uygulamaya özgü yüklemeyi Python ve komut dosyaları dizinini bulun. Varsayılan konumu şudur: 

`/Users/<user>/Library/Caches/AmlWorkbench/Python/bin` 

Ardından aşağıdaki komutlardan birini çalıştırın: 

`./conda install <libraryname>`

or 

`./pip install <libraryname>`

## <a name="use-custom-modules"></a>Özel modüller kullanın
Dönüştürme veri akışı içinde (komut), aşağıdaki Python kodu yazma

```python
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction1
df = ExtensionFunction1(df)
```

Kod bloğunun türü ekleme sütununda (komut) ayarlayın modülü = ve aşağıdaki Python kodu yazma

```python 
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction2

def newvalue(row):
    return ExtensionFunction2(row)
```
Farklı bir yürütme bağlamı (yerel, Docker, Spark), mutlak yolu doğru yerde işaret eder. "Os.getcwd() + relativePath" bulmak için kullanmak isteyebilirsiniz.


## <a name="column-data"></a>Sütun verileri 
Sütun veri satırdan noktalı gösterim veya anahtar-değer gösterim kullanılarak erişilebilir. Boşluk veya özel karakterler içeren sütun adları noktalı gösterim kullanılarak erişilemez. `row` Değişkeni Python Uzantıları (modülü ve ifade) her iki modda her zaman tanımlanmalıdır. 

Örnekler 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="add-column"></a>Sütun ekleme 
### <a name="purpose"></a>Amaç
Sütun Ekle uzantı noktası yeni bir sütun hesaplamak için Python yazmanızı sağlar. Yazdığınız kodları tam satır erişebilir. Her satır için yeni bir sütun değer döndürmesi gerekir. 

### <a name="how-to-use"></a>Nasıl kullanılır
Sütun Ekle (komut) blok kullanarak bu uzantı noktası ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsünde, üzerinde de olarak **sütun** bağlam menüsü. 

### <a name="syntax"></a>Sözdizimi
İfade

```python
    math.log(row["Score"])
```

Modül 

```python
def newvalue(row):  
     return math.log(row["Score"])
```
 

## <a name="advanced-filter"></a>Gelişmiş Filtre
### <a name="purpose"></a>Amaç 
Gelişmiş Filtre uzantı noktası bir özel filtre yazmanızı sağlar. Tüm satırı erişimi ve kodunuzu True döndürmesi gerekir (satırı ekleyin) ya da False (dışlamak satır). 

### <a name="how-to-use"></a>Nasıl kullanılır
Gelişmiş Filtre (komut) blok kullanarak bu uzantı noktası ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsü. 

### <a name="syntax"></a>Sözdizimi

İfade

```python
    row["Score"] > 95
```

Modül  

```python
def includerow(row):  
    return row["Score"] > 95
```
 

## <a name="transform-dataflow"></a>Veri akışı dönüştürme
### <a name="purpose"></a>Amaç 
Veri akışı dönüştürme uzantı noktası tamamen veri akışı dönüştürme olanak sağlar. Tüm işleme satırları ve sütunları içeren bir Pandas dataframe erişebilirsiniz. Kodunuzu yeni verilerle Pandas dataframe döndürmesi gerekir. 

>[!NOTE]
>Python içinde belleğe yüklenmiş tüm verilerin ise bir Pandas dataframe bu uzantısı kullanılır. 
>
>Spark, tüm verilerin tek çalışan düğüme toplanır. Veriler çok büyükse, bir çalışan bellek yetersiz çalıştırabilirsiniz. Dikkatli kullanın.

### <a name="how-to-use"></a>Nasıl kullanılır 
Dönüştürme veri akışı (komut) blok kullanarak bu uzantı noktası ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsü. 
### <a name="syntax"></a>Sözdizimi 

İfade

```python
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Modül 

```python
def transform(df):  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()  
    return df
```
  

## <a name="transform-partition"></a>Bölüm dönüştürme  
### <a name="purpose"></a>Amaç 
Dönüştürme bölüm uzantı noktası bir bölüm veri akışının dönüştürme olanak sağlar. Tüm sütunları ve satırları Bu bölüm için içeren bir Pandas dataframe erişebilirsiniz. Kodunuzu yeni verilerle Pandas dataframe döndürmesi gerekir. 

>[!NOTE]
>Python içinde tek bir bölüm veya veri boyutuna bağlı olarak birden çok bölüm şunun. Spark, verilen çalışan düğümünde bir bölüm için veri tutan bir dataframe çalıştığınız. Her iki durumda da, tüm veri kümesinin erişimi varsayalım olamaz. 


### <a name="how-to-use"></a>Nasıl kullanılır
Dönüştürme bölüm (komut) blok kullanarak bu uzantı noktası ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsü. 

### <a name="syntax"></a>Sözdizimi 

İfade 

```python
    df['partition-id'] = index  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Modül 

```python
def transform(df, index):
    df['partition-id'] = index
    df['index-column'] = range(1, len(df) + 1)
    df = df.reset_index()
    return df
```


## <a name="datapreperror"></a>DataPrepError  
### <a name="error-values"></a>Hata değerleri  
Veriler hazırlıkları içinde hata değerlerini kavramı bulunmaktadır. 

Özel Python kodda hata değerlerini karşılaştığınız mümkündür. Adlı bir Python sınıfın örnekleri olan `DataPrepError`. Bu sınıf bir Python özel sarmalar ve birkaç özellikleri vardır. Özellikleri özgün değeri yanı sıra, özgün değeri işlenirken oluşan hata hakkında bilgi içerir. 


### <a name="datapreperror-class-definition"></a>DataPrepError sınıf tanımı
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
Veriler hazırlıkları Python Framework'te DataPrepError oluşturulmasını genellikle şu şekildedir: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Nasıl kullanılır 
Python önceki oluşturma yöntemini kullanarak DataPrepErrors dönüş değeri olarak oluşturmak için bir uzantı noktada çalıştığında mümkündür. Bir uzantı noktada veri işlendiğinde DataPrepErrors karşılaşılan çok daha yüksektir. Bu noktada, DataPrepError geçerli bir veri türü olarak işlemek özel Python kodu gerekir.

#### <a name="syntax"></a>Sözdizimi 
İfade 
```python 
    if (isinstance(row["Score"], DataPrepError)): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
```python 
    if (hasattr(row["Score"], "originalValue")): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
Modül 
```python 
def newvalue(row): 
    if (isinstance(row["Score"], DataPrepError)): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
``` 
```python 
def newvalue(row): 
    if (hasattr(row["Score"], "originalValue")): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
```  
