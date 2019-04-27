---
title: Veri depoları içindeki verilere erişen / blobları için eğitim
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile eğitim sırasında blob veri depolama alanına veri depoları kullanmayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 02/25/2019
ms.custom: seodec18
ms.openlocfilehash: 0ab01187b03b3d658b171029003667588382bd7f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60820287"
---
# <a name="access-data-from-your-datastores"></a>Verilere erişmek, veri depoları

 Azure Machine Learning hizmetinde veri depoları bilgi işlem depolamaya erişimi için konumdan bağımsız mekanizmaları, kaynak kodunda değişiklik yapılmasını gerektirmeden. Bir yolu olarak bir parametre alan bir eğitim kod yazma ya da bir veri deposuna doğrudan bir tahmin sağlamak olsun, Azure Machine Learning iş akışı veri deposu konumlarınıza erişilebilir olduğundan ve işlem bağlamınızın sunulan emin olun.

Bu nasıl yapılır örnekleri aşağıdaki görevleri gösterir:
* [Bir veri deposu seçin](#access)
* [Verileri alma](#get)
* [Karşıya yükleme ve verileri veri depoları için indirme](#up-and-down)
* [Eğitim sırasında erişim veri deposu](#train)

## <a name="prerequisites"></a>Önkoşullar

Veri depoları kullanmak için önce ihtiyacınız bir [çalışma](concept-azure-machine-learning-architecture.md#workspace).

Başlangıç ya da tarafından [yeni bir çalışma alanı oluşturma](setup-create-workspace.md#sdk) veya mevcut bir alınıyor:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="choose-a-datastore"></a>Bir veri deposu seçin

Varsayılan veri deposu kullanabilir veya kendi işleyicinizi getirin.

### <a name="use-the-default-datastore-in-your-workspace"></a>Varsayılan veri deposu çalışma alanınızda kullanma

 Her bir çalışma alanı, hemen kullanabileceğiniz bir kayıtlı, varsayılan veri deposu sahiptir.

Çalışma alanınızın varsayılan veri deposu almak için:

```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Çalışma alanı ile kendi veri deposu kaydetme

Var olan Azure Depolama'iniz varsa, çalışma alanınızda bir veri deposu olarak kaydedebilirsiniz.   Tüm kayıt yöntemleri bulunan [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıf ve form register_azure_ * sahip. 

Aşağıdaki örnekler, bir veri deposu olarak bir Azure Blob kapsayıcısı veya bir Azure dosya paylaşımı kaydetmenizi gösterir.

+ İçin bir **Azure Blob kapsayıcısı veri deposu**, kullanın [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ İçin bir **Azure dosya paylaşımı veri deposu**, kullanın [ `register_azure_file_share()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). Örneğin: 
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           file_share_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Veri depoları tanımlayın & Bul

Geçerli çalışma alanında kayıtlı belirtilen bir veri deposu almak için kullanın [ `get()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) :

```Python
#get named datastore from current workspace
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Belirli bir çalışma alanındaki tüm veri depoları, bir listesini almak için bu kodu kullanın:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Geçerli çalışma alanı için bir farklı varsayılan veri deposu tanımlamak için [ `set_default_datastore()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-):

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Karşıya yükleme ve verileri indirme
[ `upload()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) Ve [ `download()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) yöntemleri aşağıdaki örneklerde açıklandığı özgüdür ve için aynı şekilde çalışan [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) ve [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) sınıfları.

### <a name="upload"></a>Karşıya Yükle

 Python SDK'sını kullanarak bir veri deposu için bir dizin veya tek tek dosyaları karşıya yükleyin.

Bir veri deposuna bir dizine yüklenecek `ds`:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```

`target_path` Dosya Paylaşımı (veya blob kapsayıcısı) karşıya yüklemek için konumu belirtir. Varsayılan `None`, bu durumda veriler karşıya kök. `overwrite=True` mevcut verileri geçersiz kılacaktır `target_path`.

Veya veri deposu aracılığıyla veri deposu'nın tek tek dosyaların listesini karşıya `upload_files()` yöntemi.

### <a name="download"></a>İndirme
Benzer şekilde, verileri bir veri deposundan yerel dosya sisteminize indirin.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```

`target_path` Verileri yüklemek için yerel bir dizin konumdur. Dosya Paylaşımı (veya blob kapsayıcısı) indirmek için klasöre bir yol belirtmek için bu yolun sağlamak `prefix`. Varsa `prefix` olduğu `None`, dosya paylaşımı (veya blob kapsayıcısı) tüm içeriği karşıdan.

<a name="train"></a>
## <a name="access-datastores-during-training"></a>Eğitim sırasında erişim veri depoları

Veri deposu işlem hedefte kullanılabilir yaptıktan sonra yalnızca yolun eğitim komut dosyanızdaki bir parametre olarak geçirerek eğitim çalıştırmaları sırasında (örneğin, eğitim veya doğrulama verileri) erişebilir.

Aşağıdaki tabloda [ `DataReference` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) işlem hedef çalıştırma sırasında veri deposu kullanmayı açıklayan yöntemleri.

Way|Yöntem|Açıklama|
----|-----|--------
Bağlama| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| İşlem hedef veri deposuna bağlamak için kullanın.
İndirme|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Tarafından belirtilen konuma, veri deposu içeriğini indirin `path_on_compute`. <br> Eğitim çalıştırma bağlamı için bu yüklemeyi çalıştırmadan önce'olmuyor.
Karşıya Yükle|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Bir dosyayı tarafından belirtilen konumdan yüklemek için kullandığınız `path_on_compute` , veri deposu için. <br> Eğitim çalıştırma bağlamı için bu karşıya yükleme sonrasında, çalıştırma'olmuyor

 ```Python
import azureml.data
from azureml.data.data_reference import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Belirli bir klasöre ya da kendi veri deposu dosyasına başvuruyor ve işlem hedefte kullanılabilir hale getirmek için veri deposu'nın kullanın [ `path()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) işlevi.

```Python
#download the contents of the `./bar` directory in ds to the compute target
ds.path('./bar').as_download()
```

> [!NOTE]
> Tüm `ds` veya `ds.path` nesnesi bir ortam değişkeni adı biçimi için çözer `"$AZUREML_DATAREFERENCE_XXXX"` değeri hedef işlem üzerinde bağlama/indirme yolunu temsil eder. Veri deposu yolu hedef işlem üzerinde eğitim betiğin yürütme yolunu aynı olmayabilir.

### <a name="compute-context-and-datastore-type-matrix"></a>İşlem bağlamını ve veri deposu türü Matrisi

Matris farklı işlem bağlamını ve veri deposu senaryolar için kullanılabilir veri erişimi işlevleri görüntüler. Bu Matristeki "işlem hattı" terimi, veri depoları bir giriş veya çıkış yeteneği anlamına gelir [Azure Machine Learning işlem hatlarını](https://docs.microsoft.com/azure/machine-learning/service/concept-ml-pipelines).

||Yerel işlem|Azure Machine Learning işlem|Veri Aktarımı|Databricks|HDInsight|Azure Batch|Azure DataLake Analytics|Virtual Machines|
-|--|-----------|----------|---------|-----|--------------|---------|---------|
|AzureBlobDatastore|[`as_download()`] [`as_upload()`]|[`as_mount()`]<br> [`as_download()`] [`as_upload()`] <br> İşlem hattı|İşlem hattı|İşlem hattı|[`as_download()`] <br> [`as_upload()`]|İşlem hattı||[`as_download()`] <br> [`as_upload()`]|
|AzureFileDatastore|[`as_download()`] [`as_upload()`]|[`as_mount()`]<br> [`as_download()`] [`as_upload()`] İşlem hattı |||[`as_download()`] [`as_upload()`]|||[`as_download()`] [`as_upload()`]|
|AzureDataLakeDatastore|||İşlem hattı|İşlem hattı|||İşlem hattı||
|AzureDataLakeGen2Datastore|||İşlem hattı||||||
|AzureDataPostgresSqlDatastore|||İşlem hattı||||||
|AzureSqlDatabaseDataDatastore|||İşlem hattı||||||


> [!NOTE]
> Yüksek oranda yinelemeli, büyük veri süreçleri çalıştırarak daha hızlı kullanarak senaryolar olabilir. [`as_download()`] yerine [`as_mount()`]; Bu experimentally doğrulanabilir.

### <a name="examples"></a>Örnekler 

Aşağıdaki kod örnekleri özgü [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) eğitim sırasında veri deposuna erişmek için sınıf.

Bu kod oluşturur ve eğitim betiğini kullanarak bir tahmin `train.py`, tanımlanan parametreleri kullanarak belirtilen kaynak dizinden `script_params`, tüm belirtilen işlem hedefte.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

Veri depoları listesinde Estimator oluşturucuya geçirebilirsiniz `inputs` veya kopyalamak için buralardan, datastore(s) çıkarmaya parametresi. Bu kod örneği:
* Veri deposu tüm içeriğini indirir `ds1` eğitim betiğinizi önce işlem hedefine `train.py` çalıştırılır
* İndirmeler klasörü `'./foo'` veri deposu olarak `ds2` önce işlem hedefine `train.py` çalıştırılır
* Dosya yüklemeleri `'./bar.pkl'` işlem hedef veri deposu kadar `ds3` betiğinizi çalıştırıldıktan sonra

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

## <a name="next-steps"></a>Sonraki adımlar

* [Bir model eğitip](how-to-train-ml-models.md)

* [Model dağıtma](how-to-deploy-and-where.md)
