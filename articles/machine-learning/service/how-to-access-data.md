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
ms.openlocfilehash: 615ab592c040eedf7d31e3a3036f558ea6c09740
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990816"
---
# <a name="how-to-access-data-during-training"></a>Eğitim sırasında verilere erişmek nasıl
Azure Machine Learning Hizmetleri, bir veri deposu bir üzerinden soyutlamadır [Azure depolama](https://docs.microsoft.com/azure/storage/common/storage-introduction). Veri deposu ya da başvurabilirsiniz bir [Azure Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) kapsayıcı veya [Azure dosya paylaşımı](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) temel alınan depolama alanı olarak. Veri depoları kolayca erişebilir ve Azure Machine Learning iş akışlarınızı Python SDK'sı veya CLI aracılığıyla sırasında veri depolama ile etkileşim sağlar.

## <a name="create-a-datastore"></a>Bir veri deposu oluşturma
Veri depoları kullanmak için önce gerekir bir [çalışma](concept-azure-machine-learning-architecture.md#workspace). Yeni bir çalışma alanı oluşturun veya mevcut bir alabilirsiniz:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Varsayılan veri deposunu kullanın
Her bir çalışma alanı oluşturma ve kendi depolama hesaplarını yapılandırma işlemlerini kaydedileceği, hemen kullanmaya başlamak bir varsayılan veri deposu sahiptir.

Çalışma alanınızın varsayılan veri deposu almak için:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Bir veri deposu kaydetme
Var olan Azure depolama zaten varsa, çalışma alanınızda bir veri deposu olarak kaydedebilirsiniz. Azure Machine Learning, Azure dosya paylaşımını veya bir Azure Blob kapsayıcısı, bir veri deposu olarak kaydetmek için işlevsellik sağlar. Tüm kayıt yöntemleri bulunan `Datastore` sınıf ve form sahip `register_azure_*`.

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

Çalışma alanınız için varsayılan veri deposu olarak ayarlanmış bir, kayıtlı veri depoları istiyorsanız, kolaylık sağlaması için şu şekilde bunu yapabilirsiniz:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Karşıya yükleme ve verileri indirme
### <a name="upload"></a>Karşıya Yükle
Python SDK'sını kullanarak bir veri deposu için bir dizin veya tek tek dosyaları karşıya yükleyebilirsiniz.

Bir veri deposuna bir dizine yüklenecek `ds`:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` Dosya Paylaşımı (veya blob kapsayıcısı) karşıya yüklemek için konumu belirtir. Varsayılan `None`, bu durumda veriler karşıya kök. `overwrite=True` mevcut verileri geçersiz kılacaktır `target_path`.

Veri deposu aracılığıyla veri deposu'nın alternatif olarak tek tek dosyaların listesini yükleyebilirsiniz `upload_files()` yöntemi.

### <a name="download"></a>İndirme
Benzer şekilde, bir veri deposundan yerel dosya sisteminize verilerini indirebilir.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```
`target_path` Verileri yüklemek için yerel bir dizin konumdur. Dosya Paylaşımı (veya blob kapsayıcısı) indirmek için klasöre bir yol belirtmek için bu yolun sağlamak `prefix`. Varsa `prefix` olduğu `None`, dosya paylaşımı (veya blob kapsayıcısı) tüm içeriği karşıdan.

## <a name="access-datastores-for-training"></a>Eğitim için erişim veri depoları
Bir veri deposu (örneğin eğitimi veya doğrulama veriler için) bir uzak işlem hedef Python SDK'sı çalıştırmak eğitim sırasında erişebilirsiniz. 

Veri deposu bağlı uzak işlem yapmak için desteklenen iki yolu vardır:
* **Bağlama**  
`ds.as_mount()`: Bu bağlama modu belirterek, veri deposu, bağlı uzak işlem bağlı. 
* **Yükleme/yükleme**  
    * `ds.as_download(path_on_compute='your path on compute')`: Bu indirme modu ile verileri, veri deposundan uzak bir işlem tarafından belirtilen konuma indirilir `path_on_compute`.
    * Buna karşılık, bir veri deposuna gerçekleştirdiğinizden eğitim öğesinden üretilen veri yükleyebilirsiniz. Örneğin, bir eğitim betiği oluşturur, bir `foo.pkl` dosya uzak işlem, geçerli çalışma dizininde, onun için belirttiğiniz komut dosyası çalıştırıldıktan sonra veri deposuna karşıya: `ds.as_upload(path_on_compute='./foo.pkl')`. Bu dosya, veri deposu kök dizinine yükler.
    
Belirli bir klasöre veya kendi veri deposu dosyasına başvurmak istiyorsanız, veri deposu'nın kullanabileceğiniz **`path`** işlevi. Örneğin, veri deposu göreli yolu içeren bir dizine sahipse `./bar`ve yalnızca bu klasörünün içeriğini işlem hedefine indirmek istiyorsanız, bunu şu şekilde yapabilirsiniz: `ds.path('./bar').as_download()`

Tüm `ds` veya `ds.path` nesnesi bir ortam değişkeni adı biçimi için çözer `"$AZUREML_DATAREFERENCE_XXXX"` uzak işlem bağlama/yükleme yolunda değerini temsil eder. Uzak işlem veri deposu yolunda betik için yürütme yolu ile aynı olmayabilir.

Eğitim sırasında veri deposuna erişmek için bu eğitim betiğinizi komut satırı bağımsız değişkeni olarak geçirebilirsiniz `script_params`:

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

Alternatif olarak, veri depoları için listesine geçirebilirsiniz `inputs` veya kopyalamak için buralardan, datastore(s) çıkarmaya Estimator oluşturucunun parametresi:

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
