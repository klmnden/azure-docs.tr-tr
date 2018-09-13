---
title: Azure Machine Learning veri hazırlıkları ile Python genişletilebilirlik kullanın | Microsoft Docs
description: Bu belgede bir genel bakış ve veri hazırlama genişletmek için Python kodu kullanmak ayrıntılı bazı örnekler sağlar.
services: machine-learning
author: euangMS
ms.author: euang
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.openlocfilehash: a713f5fcde31e0e25de080a65b71209011ef551d
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651129"
---
# <a name="data-preparations-python-extensions"></a>Veri hazırlıkları Python uzantıları
Yerleşik özellikler arasındaki işlev eksikliklerini doldurarak bir yolu olarak, Azure Machine Learning veri hazırlıkları farklı düzeylere genişletilebilirlik içerir. Bu belgede, biz Python betiği aracılığıyla genişletilebilirlik özetler. 

## <a name="custom-code-steps"></a>Özel kod adımları 
Veri hazırlıkları kullanıcılar kod burada yazabilirsiniz özel aşağıdakileri içerir:

* Sütun Ekle
* Gelişmiş Filtre
* Veri akışını dönüştürme
* Bölüm dönüştürme

## <a name="code-block-types"></a>Kod bloğu türü 
Bu adımların her biri için iki kod bloğu türü destekliyoruz. İlk olarak, olduğu gibi yürütülür çıplak bir Python ifade destekliyoruz. İkinci olarak, bilinen bir imza ile belirli bir işleve girdiğiniz kod burada diyoruz bir Python modülü destekliyoruz.

Örneğin, aşağıdaki iki şekilde başka bir sütuna günlüğü hesaplar yeni bir sütun ekleyebilirsiniz:

İfade 

```python    
    math.log(row["Score"])
```

Modül 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


Çağrılan işlev bulmak modül modu Sütun Ekle dönüşüm bekliyor `newvalue` satır değişkeni kabul eder ve sütun değeri döndürür. Bu modül, diğer işlevleri, içeri aktarımlar vb. ipucuyla Python kodunda herhangi bir miktarın dahil edebilirsiniz.

Her bir uzantı noktası ayrıntılarını aşağıdaki bölümlerde ele alınmıştır. 

## <a name="imports"></a>İçeri aktarmalar 
İfade blok türü kullanıyorsanız, eklemeye devam edebilirsiniz **alma** kodunuzda deyimleri. Bunların tümü, kodunuzun üst satırları gruplandırılmalıdır.

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
 
 
Modül blok türü kullanıyorsanız kullanarak tüm normal Python kurallarını takip edebilirsiniz **alma** deyimi. 

## <a name="default-imports"></a>Varsayılan içeri aktarmalara
Aşağıdaki içeri aktarmaları her zaman dahil edilen ve kodunuzun içinde kullanılabilir. Bunları yeniden içe aktarmanız gerekmez. 

```python
import math  
import numbers  
import datetime  
import re  
import pandas as pd  
import numpy as np  
import scipy as sp
```
  

## <a name="install-new-packages"></a>Yeni paket yükleme
Varsayılan olarak yüklü olmayan bir paket kullanılacak ilk veri hazırlıkları kullanan ortamlara yüklemeniz gerekir. Bu yükleme, hem yerel makinenizde hem de çalıştırmak istediğiniz herhangi bir işlem hedeflerde yapılması gerekir.

Paketlerinizi işlem hedefte yüklemek için projenizin kökü altındaki aml_config klasöründe bulunan conda_dependencies.yml dosyasını değiştirmeniz gerekir.

### <a name="windows"></a>Windows 
Windows konumu bulmak için uygulamaya özgü yüklemeyi Python ve betikleri dizinini bulun. Varsayılan konumu şudur:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts` 

Ardından aşağıdaki komutlardan birini çalıştırın: 

`conda install <libraryname>` 

or 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
Mac'te konumu bulmak için uygulamaya özgü yüklemeyi Python ve betikleri dizinini bulun. Varsayılan konumu şudur: 

`/Users/<user>/Library/Caches/AmlWorkbench/Python/bin` 

Ardından aşağıdaki komutlardan birini çalıştırın: 

`./conda install <libraryname>`

or 

`./pip install <libraryname>`

## <a name="use-custom-modules"></a>Özel modüller kullanın
Dönüşüm veri akışı (betik), aşağıdaki Python kodu yazma

```python
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction1
df = ExtensionFunction1(df)
```

Kod bloğu türü ekleme sütununda (betik) ayarlayın modülü = ve aşağıdaki Python kodu yazma

```python 
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction2

def newvalue(row):
    return ExtensionFunction2(row)
```
Farklı yürütme bağlamları (yerel, Docker, Spark), doğru yere mutlak yolu noktası. "Os.getcwd() + relativePath" bulmak için kullanmak isteyebilirsiniz.


## <a name="column-data"></a>Sütun verileri 
Sütun verileri satırdan nokta gösterimi veya anahtar-değer gösterimi kullanılarak erişilebilir. Boşluk veya özel karakterler içeren sütun adları nokta gösterimi kullanılarak erişilemez. `row` Değişkeni Python Uzantıları (modülü ve ifade) her iki modda her zaman tanımlanmalıdır. 

Örnekler 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="add-column"></a>Sütun Ekle 
### <a name="purpose"></a>Amaç
Sütun Ekle uzantı noktası yeni bir sütun hesaplamak için Python yazmanızı sağlar. Yazdığınız kod tam satır erişebilir. Bu, her satır için yeni bir sütun değeri döndürmesi gerekir. 

### <a name="how-to-use"></a>Nasıl kullanılır
Sütun Ekle (betik) blok kullanarak bu uzantı noktası ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsünden de itibariyle **sütun** bağlam menüsü. 

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
Gelişmiş Filtre uzantı noktası özel filtre yazmanızı sağlar. Tüm satırı erişiminiz ve kodunuzu True döndürmelidir (satırı dahil) ya da False (satır hariç). 

### <a name="how-to-use"></a>Nasıl kullanılır
Gelişmiş Filtre (betik) blok kullanarak bu uzantı noktası ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsü. 

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
 

## <a name="transform-dataflow"></a>Veri akışını dönüştürme
### <a name="purpose"></a>Amaç 
Dönüşüm veri akışı uzantı noktası tamamen veri akışını dönüştürme sağlar. Tüm işlemekte satırları ve sütunları içeren bir Pandas dataframe erişebilirsiniz. Kodunuzu yeni verilerle bir Pandas dataframe döndürmesi gerekir. 

>[!NOTE]
>Python'da, tüm verilerin belleğe yüklenmesi olup bir Pandas dataframe içinde bu uzantısı kullanılır. 
>
>Spark, tüm verilerin bir tek bir çalışan düğümü toplanır. Veriler çok büyükse, bir çalışan bellek yetersiz çalışabilir. Dikkatli kullanın.

### <a name="how-to-use"></a>Nasıl kullanılır 
Bu uzantı noktası dönüşüm veri akışı (betik) blok kullanarak ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsü. 
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
Veri akışına ilişkin bir bölüm dönüştürme dönüştürme bölüm genişletme noktası sağlar. Tüm sütunları ve satırları için bu bölümü içeren bir Pandas dataframe erişebilirsiniz. Kodunuzu yeni verilerle bir Pandas dataframe döndürmesi gerekir. 

>[!NOTE]
>Python, tek bir bölüm veya verilerinizin boyutuna bağlı olarak birden çok bölüm çıkabilir. Spark, verilen çalışan düğümündeki bir bölüm için verileri tutan bir veri çerçevesi ile çalışıyoruz. Her iki durumda da, tüm veri kümesini erişiminiz varsayamazsınız. 


### <a name="how-to-use"></a>Nasıl kullanılır
Bu uzantı noktası dönüştürme bölüm (betik) blok kullanarak ekleyebilirsiniz. Üst düzey üzerinde kullanılabilir **dönüşümleri** menüsü. 

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
### <a name="error-values"></a>Hata değerlerini  
Veri hazırlıkları içinde hata değerlerini kavramı var. 

Özel bir Python kodunda hata değerlerini karşılaşmak mümkündür. Adlı bir Python sınıf örnekleridir `DataPrepError`. Bu sınıf, bir Python özel durumu sarmalar ve birkaç özellik vardır. Özellikleri özgün değerin yanı sıra, özgün değer işlenirken oluşan hata hakkında bilgi içerir. 


### <a name="datapreperror-class-definition"></a>DataPrepError sınıf tanımı
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
Veri hazırlıkları Python Framework'te bir DataPrepError oluşturulmasını genellikle şu şekilde görünür: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Nasıl kullanılır 
Python DataPrepErrors dönüş değerleri önceki oluşturma yöntemini kullanarak oluşturmak için bir uzantı noktada çalıştığında mümkündür. Bir uzantı noktada veri işlendiğinde DataPrepErrors karşılaşılan çok daha yüksektir. Bu noktada, bir DataPrepError geçerli bir veri türü olarak işlemek özel bir Python kodu gerekiyor.

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
