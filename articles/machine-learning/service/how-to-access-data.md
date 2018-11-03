---
title: Veri depoları, verilere erişmek için Azure Machine Learning'de kullanın
description: Eğitim sırasında veri erişim veri depolama için depoları kullanma
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 79e26d4d2cf5743abae6dc0f1fb58585e1b9b9db
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50977915"
---
# <a name="how-to-access-data-during-training"></a>Eğitim sırasında verilere erişmek nasıl
Erişim ve Azure Machine Learning iş akışlarında verilerinizle etkileşim kurmak için bir veri deposu kullanın.

Azure Machine Learning hizmetinde bir veri deposu üzerinde bir Özet, [Azure depolama](https://docs.microsoft.com/azure/storage/common/storage-introduction). Veri deposu ya da başvurabilirsiniz bir [Azure Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) kapsayıcı veya [Azure dosya paylaşımı](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) temel alınan depolama alanı olarak. 

## <a name="create-a-datastore"></a>Bir veri deposu oluşturma
Veri depoları kullanmak için önce ihtiyacınız bir [çalışma](concept-azure-machine-learning-architecture.md#workspace). Başlangıç ya da tarafından [yeni bir çalışma alanı oluşturma](quickstart-create-workspace-with-python.md) veya mevcut bir alınıyor:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Varsayılan veri deposunu kullanın
Oluşturun veya bir depolama hesabı yapılandırmak gerekmez.  Her bir çalışma alanı hemen kullanmaya başlayabilmeniz için varsayılan bir veri deposu sahiptir.

Çalışma alanınızın varsayılan veri deposu almak için:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Bir veri deposu kaydetme
Var olan Azure Depolama'iniz varsa, çalışma alanınızda bir veri deposu olarak kaydedebilirsiniz. Bir veri deposu olarak Azure Blob kapsayıcısı ya da Azure dosya paylaşımı kaydedebilirsiniz. Tüm kayıt yöntemleri bulunan `Datastore` sınıf ve form sahip `register_azure_*`.

#### <a name="azure-blob-container-datastore"></a>Azure Blob kapsayıcısı veri deposu
Bir Azure Blob kapsayıcısı veri deposu kaydetmek için:

```Python
ds = Datastore.register_azure_blob_container(workspace=ws, 
                                             datastore_name='your datastore name', 
                                             container_name='your azure blob container name',
                                             account_name='your storage account name', 
                                             account_key='your storage account key',
                                             create_if_not_exists=True)
```

#### <a name="azure-file-share-datastore"></a>Azure dosya paylaşımı veri deposu
Bir Azure dosya paylaşımı veri deposu kaydetmek için:

```Python
ds = Datastore.register_azure_file_share(workspace=ws, 
                                         datastore_name='your datastore name', 
                                         container_name='your file share name',
                                         account_name='your storage account name', 
                                         account_key='your storage account key',
                                         create_if_not_exists=True)
```

### <a name="get-an-existing-datastore"></a>Mevcut bir veri deposu alma
Kayıtlı bir veri deposu için ada göre sorgulamak için:
```Python
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Bir çalışma alanı için tüm veri depoları da edinebilirsiniz:
```Python
datastores = ws.datastores()
for name, ds in datastores.items(),
    print(name, ds.datastore_type)"
```

Kolaylık olması için kayıtlı veri depoları birini çalışma alanınız için varsayılan veri deposu olarak ayarlayın:
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
Uzak işlem hedefi üzerinde Python SDK'sı çalıştırın (örneğin, eğitim veya doğrulama verileri) bir eğitim sırasında bir veri deposu erişebilirsiniz. 

Veri deposu bağlı uzak işlem yapmak için desteklenen iki yolu vardır:
* **Bağlama**  
`ds.as_mount()`: Bu bağlama modu belirterek, veri deposu, bağlı uzak işlem bağlı. 
* **Yükleme/yükleme**  
    * `ds.as_download(path_on_compute='your path on compute')` veri deposundan veri uzak bir işlem tarafından belirtilen konuma indirir `path_on_compute`.
    * `ds.as_upload(path_on_compute='yourfilename'` verileri veri deposuna yükler.  Eğitim betiğinizi oluşturur varsayalım bir `foo.pkl` dosyasını geçerli çalışma dizini, uzak işlem. İle veri deposu için bu dosyayı karşıya `ds.as_upload(path_on_compute='./foo.pkl')` sonra betik dosyası oluşturur. Dosya, veri deposu köküne yüklenir.
    
Belirli bir klasörü veya kendi veri deposu dosyasına başvurmak için veri deposu'nın kullanın **`path`** işlevi. Örneğin, içeriğini indirmek için `./bar` kullanın, işlem hedef veri deposu gelen dizin `ds.path('./bar').as_download()`.

Tüm `ds` veya `ds.path` nesnesi bir ortam değişkeni adı biçimi için çözer `"$AZUREML_DATAREFERENCE_XXXX"` uzak işlem bağlama/yükleme yolunda değerini temsil eder. Uzak işlem veri deposu yolunda betik için yürütme yolu ile aynı olmayabilir.

Eğitim sırasında veri deposuna erişmek için eğitim betiğinizi komut satırı bağımsız değişken olarak geçirin `script_params`:

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
`as_mount()` yalnızca aynı zamanda doğrudan geçirebiliriz şekilde bir veri deposu için varsayılan moddur `ds` için `'--data_dir'` bağımsız değişken.

Veya veri depoları listesinde Estimator oluşturucuya `inputs` veya kopyalamak için buralardan, datastore(s) çıkarmaya parametresi:

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```
Yukarıdaki kod olur:
* veri deposu tüm içeriğini indirme `ds1` eğitim betiğinizi önce uzak işlem için `train.py` çalıştırılır
* indirme klasörü `'./foo'` veri deposu olarak `ds2` önce uzak işlem için `train.py` çalıştırılır
* dosyayı karşıya yüklemeyi `'./bar.pkl'` veri deposu kadar uzak işlemden `d3` betiğinizi çalıştırıldıktan sonra

## <a name="next-steps"></a>Sonraki adımlar
* [Bir model eğitip](how-to-train-ml-models.md)
