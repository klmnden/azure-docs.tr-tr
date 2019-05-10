---
title: Oluşturma ve veri kümeleri ile çalışma alanınızı kaydetme
titleSuffix: Azure Machine Learning service
description: Çeşitli kaynaklardan veri kümeleri oluşturma ve veri kümeleri ile çalışma alanınızı kaydetme hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/19
ms.openlocfilehash: 938f13524b22f34f4becc936885d1611cb854df1
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510501"
---
# <a name="create-and-register-azure-machine-learning-datasets-preview"></a>Oluşturma ve Azure Machine Learning veri kümeleri (Önizleme) kaydetme

Bu makalede, Azure Machine Learning iş akışları oluşturmak ve veri kümeleri ve bunları yeniden kullanım için yerel ve uzak denemeler arasında nasıl kaydetmek için öğrenin.

Azure Machine Learning veri kümeleri (Önizleme) erişip ile çalışmanızı kolaylaştırır. Veri kümeleri, veri modeli eğitimi ve işlem hattı oluşturma gibi çeşitli senaryolarda yönetin. Kullanarak [Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), çalışabilir popüler biçimlerde verilerle, temel alınan depolama alanına erişmek, keşfedin ve verileri hazırlama, farklı bir veri kümesi tanımları yaşam döngüsünü yönetme ve kullanılan veri kümeleri arasında karşılaştırma Eğitim ve üretim.

## <a name="prerequisites"></a>Önkoşullar

Oluşturma ve veri kümeleri kaydetmek için ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

* Azure Machine için Python SDK'sı Learning. Yüklemek veya SDK'sının en son sürüme güncelleştirmek için bkz: [yükleme veya SDK'sı güncelleştirme](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

## <a name="create-datasets-from-local-files"></a>Yerel veri kümeleri oluşturun

Yükleme dosyaları yerel makinenizden ile dosya veya klasör yolunu belirterek [ `auto_read_files()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) yönteminden `Dataset` sınıfı.  Bu yöntem, dosya türü belirtmenizi gerektiren veya bağımsız değişkenlerini ayrıştırma aşağıdaki adımları gerçekleştirir:

* Çıkarımını yapma ve sınırlayıcı ayarlama.
* Dosyanın en üstüne boş kayıtları atlıyor.
* Çıkarımını yapma ve üst bilgi satırı ayarlama.
* Çıkarımını yapma ve sütun veri türlerini dönüştürme.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Alternatif olarak, ayrıştırma dosyanızın açıkça denetlemek için dosya özel işlevleri kullanın. Veri kümeleri SDK'sı şu anda destekler [ayrılmış](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-delimited-files-path--separator------header--promoteheadersbehavior-allfileshavesameheaders--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-norows--0---comment-none--include-path-false--archive-options-none-), [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-), [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-), [ikili](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-), ve [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-) dosya biçimleri.

## <a name="create-datasets-from-azure-datastores"></a>Azure veri depoları veri kümeleri oluşturma

Bir Azure veri deposundan veri kümeleri oluşturmak için emin olun:

* Katkıda bulunan veya kayıtlı Azure veri deposuna erişim sahibi olduğunuz doğrulayın.

* İçeri aktarma [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) ve [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition) ve `Dataset` SDK paketleri.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 `get()` Yöntemi çalışma alanında var olan bir veri deposunu alır.

```
dstore = Datastore.get(workspace, datastore_name)
```

Kullanım `from_delimited_files()` sınırlı dosyalarda okuma ve kayıtlı olmayan bir veri kümesini oluşturmak için yöntem.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|Yurt içi|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum|
|--|--|---|---|---|---|----|------|-------|------|-----|---|----|----|-----|-----|------|----|-----|----|----|-----
|0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|YANILTICI YÖNTEMİ|FİNANSAL KİMLİK HIRSIZLIĞINDAN 300 ABD DOLARI ÜZERİNDEN|DİĞER|FALSE|FALSE|...|9|50|11|1183356|1831503|2016|5/11/2016 15:48|41.69283384|-87.60431945|(41.692833841, -87.60431945)|
1|10516598|HZ258664|4/15/2016 17:00|082XX S MARSHFIELD AVE|890|HIRSIZLIĞI| ÖĞESİNDEN YAPI|İKAMET|FALSE|FALSE|...|21|71|6|1166776|1850053|2016|5/12/2016 15:48|41.74410697|-87.66449429|(41.744106973, -87.664494285)
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO AVE|1154|YANILTICI YÖNTEMİ|FİNANSAL KİMLİK HIRSIZLIĞINDAN 300 ABD DOLARI VE ALTINDA|İKAMET|FALSE|FALSE|...|19|74|11|||2016|5/12/2016 15:50
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE KAYDET|1120|YANILTICI YÖNTEMİ|KARŞI KORUMA|İKAMET|FALSE|FALSE|...|9|49|10|||2016|5/13/2016 15:51
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE KAYDET|890|HIRSIZLIĞI|ÖĞESİNDEN YAPI|OKUL, OLUŞTURMA, GENEL|FALSE|FALSE|...|40|13|6|||2016|5/25/2016 15:59|

## <a name="register-your-datasets-with-workspace"></a>Veri kümelerinizi çalışma alanı ile kaydolun

Kullanım [ `register()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) veri kümeleri paylaşmak için çalışma alanınıza kaydolup kuruluşunuzda ve çeşitli denemeler arasında yeniden yöntemi.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Varsayılan parametre ayarı `register()` olduğu `exist_ok = False`. Bu ayarı değiştirmeden aynı ada sahip bir veri kümesi kaydettirmeye çalışırsanız, bir hata oluşur.

`register()` Yöntem zaten kayıtlı veri kümesi parametre ayarıyla döndürür `exist_ok = True`.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = True
                           )
```

Kullanım `list()` kayıtlı veri kümeleri çalışma alanınızdaki tüm görmek için.

```Python
Dataset.list(workspace_name)
```

Yukarıdaki kod, aşağıdaki sonuçları:

```Python
[Dataset(Name: dataset_crime,
         Workspace: workspace_name)]
```

## <a name="access-datasets-in-workspace"></a>Çalışma alanındaki veri kümelerine erişme

Kayıtlı veri kümeleri yerel olarak, uzaktan ve Azure Machine Learning işlem gibi işlem kümeleri üzerinde erişilebilir ve kullanılabilir. Kayıtlı Veri kümenizi yeniden denemeler arasında ve işlem ortamları için kayıtlı veri kümesi ve çalışma alanı tarafından adını almak için aşağıdaki kodu kullanın.

```Python
workspace = Workspace.from_config()

dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Sonraki adımlar

* [Keşfedin ve veri kümeleri hazırlama](how-to-explore-prepare-data.md).
* [Veri kümesi tanımları yaşam döngüsünü yönetme](how-to-manage-dataset-definitions.md).
* Veri kümelerini kullanan bir örnek için bkz: [örnek not defterleri](https://aka.ms/dataset-tutorial).
