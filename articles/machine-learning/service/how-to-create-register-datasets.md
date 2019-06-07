---
title: Azureml veri kümeleri ile verilerine erişmek için veri kümeleri oluşturma
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
ms.date: 05/21/2019
ms.openlocfilehash: b4c22caae86e20b8379db2b7feffb1ca82001239
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753163"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Oluşturma ve veri kümeleri (Önizleme) Azure Machine learning'de erişme

Bu makalede, Azure Machine Learning veri kümeleri (Önizleme) oluşturma ve yerel ve uzak denemelerle verilere erişmek nasıl öğreneceksiniz.

Yönetilen veri kümeleri ile şunları yapabilirsiniz: 
* **Veri modeli eğitimi sırasında kolayca erişim** olmadan, temel alınan mağazalara yeniden bağlanıyor

* **Veri tutarlılığı & yeniden üretilebilirliğini olun** denemeler arasında aynı işaretçiyi kullanarak: dizüstü bilgisayarlar, otomatik ml, işlem hatları, görsel arabirim

* **Veri paylaşın ve işbirliği** diğer kullanıcılarla

* **Verileri keşfedin** & veri anlık görüntülerinin & sürümleri yaşam döngüsünü yönetme

* **Veri karşılaştırma** üretime eğitim


## <a name="prerequisites"></a>Önkoşullar

Oluşturma ve veri kümeleriyle çalışmak için ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* Bir [Azure Machine Learning hizmeti çalışma alanı](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* [Yüklü Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), azureml veri kümeleri paketi içerir.

> [!Note]
> Bazı veri kümesi sınıfları (Önizleme) bağımlılıklara sahip [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) paket (GA). Linux kullanıcıları için bu sınıflar üzerinde yalnızca aşağıdaki dağıtımlar desteklenir:  Red Hat Enterprise Linux, Ubuntu, Fedora ve CentOS.

## <a name="data-formats"></a>Veri biçimleri

Aşağıdaki veriler bir Azure Machine Learning veri kümesi oluşturabilirsiniz:
+ [ayrılmış](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none-)
+ [İkili](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)
+ [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Azure SQL Veritabanı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [Azure Data Lake genel. 1](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)

## <a name="create-datasets"></a>Veri kümeleri oluşturma 

Azureml veri kümeleri pakette, veri kümeleriyle etkileşim kurabileceğiniz [Azure Machine Learning Python SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ve özellikle [ `Dataset` sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py).

### <a name="create-from-local-files"></a>Yerel dosyaları oluşturma

Yükleme dosyaları yerel makinenizden ile dosya veya klasör yolunu belirterek [ `auto_read_files()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) yönteminden `Dataset` sınıfı.  Bu yöntem, dosya türü belirtmenizi gerektiren veya bağımsız değişkenlerini ayrıştırma aşağıdaki adımları gerçekleştirir:

* Çıkarımını yapma ve sınırlayıcı ayarlama.
* Dosyanın en üstüne boş kayıtları atlıyor.
* Çıkarımını yapma ve üst bilgi satırı ayarlama.
* Çıkarımını yapma ve sütun veri türlerini dönüştürme.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Alternatif olarak, ayrıştırma dosyanızın açıkça denetlemek için dosya özel işlevleri kullanın. 


### <a name="create-from-azure-datastores"></a>Azure veri depoları oluşturma

Bir Azure veri deposundan veri kümeleri oluşturmak için:

* Doğrulayın `contributor` veya `owner` kayıtlı Azure veri deposuna erişim.

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

## <a name="register-datasets"></a>DataSet kaydetme

Oluşturma işlemini tamamlamak için veri kümeleri ile çalışma kaydedin:

Kullanım [ `register()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) bunlar diğer kullanıcılarla paylaşılabilir ve çeşitli denemeler arasında yeniden şekilde veri kümeleri, çalışma alanına kaydetmek için yöntemi.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Varsa `exist_ok = False` (varsayılan), ve bir hata oluşursa, aynı ada sahip bir veri kümesi kaydolmaya çalışın. Kümesine `True` var olanın üzerine yaz için.

## <a name="access-data-in-datasets"></a>Erişim veri kümelerindeki verileri

Kayıtlı veri kümeleri yerel olarak, uzaktan ve Azure Machine Learning işlem gibi işlem kümeleri üzerinde erişilebilir ve kullanılabilir. Kayıtlı Veri kümenizi yeniden denemeler arasında ve işlem ortamları için kayıtlı veri kümesi ve çalışma alanı tarafından adını almak için aşağıdaki kodu kullanın.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
Dataset.list(workspace)

# Get dataset by name
dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Sonraki adımlar

* [Keşfedin ve veri kümeleri hazırlama](how-to-explore-prepare-data.md).
* [Veri kümesi tanımları yaşam döngüsünü yönetme](how-to-manage-dataset-definitions.md).
* Veri kümelerini kullanan bir örnek için bkz: [örnek not defterleri](https://aka.ms/dataset-tutorial).
