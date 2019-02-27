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
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 410867ae034309db0bb013ae22f90e8489aa463e
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56886120"
---
# <a name="access-data-from-your-datastores"></a>Verilere erişmek, veri depoları
Bu makalede, erişim ve veri depoları aracılığıyla Azure Machine Learning iş akışlarında verilerinizle etkileşim kurmak için farklı yollarını öğrenin.

Bu nasıl yapılır örnekler için aşağıdaki görevleri gösterir: 
* Oluşturma ve bir veri deposuna erişme
* [Karşıya yükleme ve verileri veri depoları için indirme](#upload-and-download-data)
* [Eğitim için erişim veri deposu](#access-datastores-for-training)

<a name="access"></a>

## <a name="create-and-access-a-datastore"></a>Oluşturma ve bir veri deposuna erişme
Veri depoları kullanmak için önce ihtiyacınız bir [çalışma](concept-azure-machine-learning-architecture.md#workspace). Başlangıç ya da tarafından [yeni bir çalışma alanı oluşturma](quickstart-create-workspace-with-python.md) veya mevcut bir alınıyor:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Varsayılan veri deposunu kullanın
Oluşturun veya bir depolama hesabı yapılandırmak gerekmez.  Her bir çalışma alanı hemen kullanmaya başlayabilmeniz için varsayılan bir veri deposu sahiptir.

Çalışma alanınızın varsayılan veri deposu almak için:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Bir veri deposu kaydetme
Var olan Azure Depolama'iniz varsa, çalışma alanınızda bir veri deposu olarak kaydedebilirsiniz.  Tüm kayıt yöntemleri bulunan [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıf ve form register_azure_ * sahip. Aşağıdaki örnekler, bir veri deposu olarak bir Azure Blob kapsayıcısını ve Azure dosya paylaşımı kaydetmenizi gösterir.

#### <a name="azure-blob-container-datastore"></a>Azure Blob kapsayıcısı veri deposu
Bir Azure Blob kapsayıcısı veri deposu kaydetmek için kullanın [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-:)

```Python
ds = Datastore.register_azure_blob_container(workspace=ws, 
                                             datastore_name='your datastore name', 
                                             container_name='your azure blob container name',
                                             account_name='your storage account name', 
                                             account_key='your storage account key',
                                             create_if_not_exists=True)
```

#### <a name="azure-file-share-datastore"></a>Azure dosya paylaşımı veri deposu
Bir Azure dosya paylaşımı veri deposu kaydetmek için kullanın [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)

```Python
ds = Datastore.register_azure_file_share(workspace=ws, 
                                         datastore_name='your datastore name', 
                                         container_name='your file share name',
                                         account_name='your storage account name', 
                                         account_key='your storage account key',
                                         create_if_not_exists=True)
```

### <a name="get-an-existing-datastore"></a>Mevcut bir veri deposu alma
[ `get()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) Yöntemi sorgular adı tarafından zaten kayıtlı bir veri deposu için:

```Python
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Bir çalışma alanı için tüm veri depoları da edinebilirsiniz:

```Python
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Kolaylık sağlamak için [ `set_default_datastore()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) varsayılan veri deposu çalışma alanınız için seçtiğiniz hangi veri deposu için ayarlar:

```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Karşıya yükleme ve verileri indirme
### <a name="upload"></a>Karşıya Yükle
Python SDK'sını kullanarak bir veri deposu için bir dizin veya tek tek dosyaları karşıya yükleyin.

Bir veri deposuna bir dizine yüklenecek `ds`:

```Python
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

## <a name="access-datastores-for-training"></a>Eğitim için erişim veri depoları
Bir veri deposu (örneğin, eğitim veya doğrulama veri) üzerinde uzak işlem hedefi Python SDK'sını kullanarak üzerinden çalıştırın eğitim sırasında erişebileceğiniz [ `DataReference` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) sınıfı.

Veri deposu bağlı uzak işlem yapmak için desteklenen iki yolu vardır:
* **Bağlama**  

    * [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), bu bağlama modu belirterek, veri deposu, bağlı uzak işlem bağlı.

    ```Python
    import azureml.data
    from azureml.data import DataReference

    ds.as_mount()
    ```

* **Yükleme/yükleme**  
    * [`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), veri tarafından belirtilen konuma indirir `path_on_compute` , veri deposu için uzak işlem üzerinde.

    * [`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), veri tarafından belirtilen konumdan veri deposu, kök dizinine yükler. `path_on_compute`

    ```Python
    ds.as_download(path_on_compute='your path on compute')
    ds.as_upload(path_on_compute='yourfilename')
    ```   

Belirli bir klasörü veya kendi veri deposu dosyasına başvurmak için veri deposu'nın kullanın [ `path()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) işlevi.

```Python
#download the contents of the `./bar` directory from the datastore 
ds.path('./bar').as_download()
```
Tüm `ds` veya `ds.path` nesnesi bir ortam değişkeni adı biçimi için çözer `"$AZUREML_DATAREFERENCE_XXXX"` uzak işlem bağlama/yükleme yolunda değerini temsil eder. Uzak işlem veri deposu yolunda betik için yürütme yolu ile aynı olmayabilir.

Eğitim sırasında veri deposuna erişmek için eğitim betiğinizi komut satırı bağımsız değişken olarak geçirin `script_params` gelen [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) sınıfı:

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py')
```
`as_mount()` Ayrıca doğrudan geçirebiliriz şekilde bir veri deposu için varsayılan moddur `ds` için `'--data_dir'` bağımsız değişken.

Veya veri depoları listesinde Estimator oluşturucuya `inputs` veya kopyalamak için buralardan, datastore(s) çıkarmaya parametresi

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

Yukarıdaki kod örneğinde:

* Veri deposu tüm içeriğini indirir `ds1` eğitim betiğinizi önce uzak işlem için `train.py` çalıştırılır

* İndirmeler klasörü `'./foo'` veri deposu olarak `ds2` önce uzak işlem için `train.py` çalıştırılır

* Dosya yüklemeleri `'./bar.pkl'` veri deposu kadar uzak işlemden `d3` betiğinizi çalıştırıldıktan sonra

## <a name="next-steps"></a>Sonraki adımlar

* [Bir model eğitip](how-to-train-ml-models.md)
* [Model dağıtma](how-to-deploy-and-where.md)

