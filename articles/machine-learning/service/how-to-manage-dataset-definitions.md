---
title: Sürüm ve veri kümesi tanımlarını azureml veri kümeleri ile yönetme
titleSuffix: Azure Machine Learning service
description: Veri kümesi tanımlarını güncelleştir ve tanımları yaşam döngüsünü yönetme hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: 4476bdd902f054683fa544dc44b548689f3a1881
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241874"
---
# <a name="version-and-manage-your-dataset-definitions"></a>Sürüm ve veri kümesi tanımları yönetme

Güncelleştirme ve Azure Machine Learning veri kümeleri (Önizleme) ile veri kümesi tanımları yönetme hakkında bilgi edinin.

Veri kümesi, başvuru veri kaynağınızı ve gerçekleştirilen dönüştürme adımlarını içerir, veri kümeleri hazırlamak için kullanılan tarifleri tanımlardır. Bir veri kümesi çok tanımları olabilir ve her tanımı kendi yaşam döngüsüne sahiptir.

Aşağıdaki senaryoyu veri kümesi tanımlarını kullanarak, bir örnek verilmiştir:

1. Bir machine learning, veri kümesinin geçerli tanımı kullanan işlem hattı oluşturun.
1. Temel alınan veri değişikliklerini; Yeni özellikler eklenmiştir.
1. Yeni öznitelikler işlemek için ek dönüştürmeler ekler tanımınızı yeni bir sürümünü oluşturun.

Bu örnekte, özgün tanım sürümü kullanmak mevcut işlem hattı devam eder. Yeni tanım sürümü modeli veya bir işlem hattı oluşturma gibi yeni senaryolarda kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure aboneliği ve veri kümesi tanımları ömrünü yönetmek için veri kümenizin kaydetmeniz için bir çalışma alanı olması gerekir.

Bu belgedeki örneklerde kullanılan örnek dosyası şu adresten edinilebilir [ https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv ](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv).

## <a name="create-and-update-definitions"></a>Oluştur ve tanımları güncelleştir

İlk oluşturur ve bir veri kümesi ile çalışma alanınızı kaydedin.

```python
from azureml.core import Workspace, Datastore, Dataset

# change the configuration for workspace and Datastore
subscription_id = 'your subscription id'
resource_group = 'your resource group name'
workspace_name = 'your workspace name'
datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace(subscription_id, resource_group, workspace_name)

# get a Datastore that has already been created in the workspace
dstore = Datastore.get(workspace, datastore_name)

# create an in-memory Dataset from Datastore
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# register the Dataset with the workspace. if a Dataset with the same name already exists, retrieve it by turning `exist_ok = True`
dataset_name = 'crime dataset'
dataset = dataset.register(workspace = workspace, 
                 name = dataset_name, 
                 description = 'crime dataset for demo', 
                 tags = {'year':'2019', 'month':'Apr'},
                 exist_ok = True)
```

İlk veri kümesi tanımı (`version_id` = 1) veri kümesi oluşturduğunuzda oluşturulur. Veri kümesi tanımınızı her güncelleştirdiğinizde yeni version_id en son tanımlarla atanır.

```python
# get the Dataset from the workspace by name
dataset = workspace.datasets['dataset_name']

# get the latest Dataset definition
ds_def = dataset.get_definition()

# update the Dataset definition to select only the columns I'm interested in
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])

# with this update, the latest definition for dataset now has `version_id = 2`
dataset = dataset.update_definition(ds_def, 'select useful column')

dataset = workspace.datasets['dataset_name']
dataset.head(5)
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
      <th>Kimlik</th>
      <th>Arrest</th>
      <th>Enlem</th>
      <th>Boylam</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10498554</td>
      <td>FALSE</td>
      <td>41.69283384</td>
      <td>-87.60431945</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10516598</td>
      <td>FALSE</td>
      <td>41.74410697</td>
      <td>-87.66449429</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10519196</td>
      <td>FALSE</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>10519591</td>
      <td>FALSE</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>10534446</td>
      <td>FALSE</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

Bu tanım güncelleştirmesi önceki veri kümesi tanımlarını silme değil. Yine de, erişim ve önceki tanımları kullanır.

```python
# specify `version_id` to get a previous definition 
ds_def_old = dataset.get_definition(version_id = 1)
ds_def_old.head(5)
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
      <th>Kimlik</th>
      <th>Büyük/küçük harf numarası</th>
      <th>Tarih</th>
      <th>Engelle</th>
      <th>IUCR</th>
      <th>Birincil tür</th>
      <th>Açıklama</th>
      <th>Konum açıklaması</th>
      <th>Arrest</th>
      <th>Yurt içi</th>
      <th>...</th>
      <th>İleri Git</th>
      <th>Topluluk alan</th>
      <th>FBI kod</th>
      <th>X koordinatı</th>
      <th>Y koordinatı</th>
      <th>Yıl</th>
      <th>Güncelleştirme tarihi</th>
      <th>Enlem</th>
      <th>Boylam</th>
      <th>Konum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10498554</td>
      <td>HZ239907</td>
      <td>4/4/2016 23:56</td>
      <td>007XX E 111TH ST</td>
      <td>1153</td>
      <td>YANILTICI YÖNTEMİ</td>
      <td>FİNANSAL KİMLİK HIRSIZLIĞINDAN 300 ABD DOLARI ÜZERİNDEN</td>
      <td>DİĞER</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>9</td>
      <td>50</td>
      <td>11</td>
      <td>1183356</td>
      <td>1831503</td>
      <td>2016</td>
      <td>5/11/2016 15:48</td>
      <td>41.69283384</td>
      <td>-87.60431945</td>
      <td>(41.692833841, -87.60431945)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10516598</td>
      <td>HZ258664</td>
      <td>4/15/2016 17:00</td>
      <td>082XX S MARSHFIELD AVE</td>
      <td>890</td>
      <td>HIRSIZLIĞI</td>
      <td>ÖĞESİNDEN YAPI</td>
      <td>İKAMET</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>21</td>
      <td>71</td>
      <td>6</td>
      <td>1166776</td>
      <td>1850053</td>
      <td>2016</td>
      <td>5/12/2016 15:48</td>
      <td>41.74410697</td>
      <td>-87.66449429</td>
      <td>(41.744106973, -87.664494285)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10519196</td>
      <td>HZ261252</td>
      <td>4/15/2016 10:00</td>
      <td>104XX S SACRAMENTO AVE</td>
      <td>1154</td>
      <td>YANILTICI YÖNTEMİ</td>
      <td>FİNANSAL KİMLİK HIRSIZLIĞINDAN 300 ABD DOLARI VE ALTINDA</td>
      <td>İKAMET</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>19</td>
      <td>74</td>
      <td>11</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/12/2016 15:50</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>10519591</td>
      <td>HZ261534</td>
      <td>4/15/2016 9:00</td>
      <td>113XX S PRAIRIE KAYDET</td>
      <td>1120</td>
      <td>YANILTICI YÖNTEMİ</td>
      <td>KARŞI KORUMA</td>
      <td>İKAMET</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>9</td>
      <td>49</td>
      <td>10</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/13/2016 15:51</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>10534446</td>
      <td>HZ277630</td>
      <td>4/15/2016 10:00</td>
      <td>055XX N KEDZIE KAYDET</td>
      <td>890</td>
      <td>HIRSIZLIĞI</td>
      <td>ÖĞESİNDEN YAPI</td>
      <td>OKUL, OLUŞTURMA, GENEL</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>40</td>
      <td>13</td>
      <td>6</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/25/2016 15:59</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

Biz sonuçtan görebileceğiniz gibi veri kümesi tanımı ilk sürümü suç verilerden daha yeni güncelleştirme bağımsız olarak tüm sütunları hala tutar. Örneğin, varsa bir makine öğrenme modeli şu anda veri kümesini ilk sürümü kullanan ve kullanarak `Date` sütun olarak özelliklerden biri, veri kümesinden yalnızca Tutarensontanımgüncelleştirmeileherhangibirhatadeğilkarşılaşırsınız`ID`, `Arrest`, `Latitude`, `Longitude` suç veri sütunları.

## <a name="how-to-access-dataset-definitions"></a>Veri kümesi tanımları erişme

Tüm tanımlar listesini almak için kullanın `get_definitions()`. Bir tanımı belirli bir sürümünü almak için kullanın `get_definition()`. Aşağıdaki örnek, tüm tanımları listesi alınıyor ve ardından sürüm 1 alma gösterir:

```python
# list all definitions for the dataset
dataset.get_definitions()
# get version ID 1
dataset.get_definition(version_id=1)
```

Çıkışı `get_definitions()` aşağıdaki örneğe benzer:

```text
{'2': VersionID: 2, State: active, Created: 2019-04-19 16:43:52.439890+00:00, Modified: 2019-04-19 16:43:52.439890+00:00, Notes: select useful column,
 '1': VersionID: 1, State: active, Created: 2019-04-19 16:39:14.112046+00:00, Modified: 2019-04-19 16:44:12.912663+00:00, Notes: None}
```

Bir tanımı aldıktan sonra makine öğrenimi Modellerinizi ile veya bir makine öğrenimi işlem hattı kullanabilirsiniz.

## <a name="manage-lifecycle-of-dataset-definitions"></a>Veri kümesi tanımları yaşam döngüsünü yönetme

Her veri kümesi tanımı yaşam döngüsü bağımsız olarak yönetebilirsiniz. Yaşam döngüsünün üç aşama vardır: etkin, arşivlenen veya kullanım dışı.

### <a name="active"></a>Etkin

Yeni bir veri kümesi tanımı oluşturduğunuzda, varsayılan olarak etkindir. 

### <a name="deprecate"></a>Kullanımdan kaldırma

Veri kümesi tanımları kullanım artık önerilir ve yenisini kullanılabilir olduğunda kullanım dışı. Kullanım dışı bir veri kümesi tanımı machine learning işlem hatlarını kullanıldığında, bir uyarı iletisi döndürdü, ancak yürütme engellenmez.

Bir veri kümesi tanımı kullanım dışı bırakılıyor, veri kümesi Kimliğini ve veri kümesi tanımı sürüm Kimliğini değiştirme tanımı belirtin. Bu bilgiler depolanır ve uyarı iletisinde kullanım dışı bir veri kümesi tanımı kullanmayı denediğinizde kullanılır.

```python
# get the definition that you want to deprecate
ds_def = dataset.get_definition(version_id = 1)

# deprecate it by providing the information for the replacement definition, which is recommended to be used in machine learning scenarios
ds_def.deprecate(deprecate_by_dataset_id=dataset.id, deprecated_by_definition_version=2)
```

### <a name="archive"></a>Arşiv

Veri kümesi tanımları, herhangi bir nedenle (örneğin, temel alınan veriler artık kullanılabilir) kullanılacak tanımları görmemesi olduğunda saklanabilir. Arşivlenen bir veri kümesi tanımı machine learning işlem hatlarını kullanıldığında, yürütme hatası ile engellenir.

```python
# archive the definition with `version_id = 1`
ds_def = dataset.get_definition(version_id = 1)
ds_def.archive()
```

### <a name="reactivate"></a>Yeniden etkinleştirme

Kullanım dışı veya arşivlenmiş herhangi bir veri kümesi tanımı bir kolayca etkinleştirebilirsiniz.

```python
# reactivate Dataset definition with `version_id =1` which was archived in the previous step
ds_def = dataset.get_definition(version_id = 1)
ds_def.reactivate()
```

## <a name="next-steps"></a>Sonraki adımlar

Veri kümeleri ile çalışma hakkında daha fazla bilgi için bkz. [oluşturma ve Azure Machine Learning veri kümeleri kayıt](how-to-create-register-datasets.md).

Veri kümelerini kullanan bir örnek için bkz: [örnek not defterleri](https://aka.ms/dataset-tutorial).
