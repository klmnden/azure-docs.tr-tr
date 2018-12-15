---
title: Güncelleştirilmiş Azure Batch AI API'sine geçir | Microsoft Docs
description: Azure Batch AI kod ve betiklerin çalışma alanını kullanın ve kaynakları deneme güncelleştirme
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 75a9a5e9bafd62b320397c00ef6574b7536d9e09
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407789"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Güncelleştirilmiş Batch AI API'sine geçir

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Batch AI REST API sürümü 2018-05-01 ve ilgili Batch AI SDK'ları ve araçları, önemli değişiklikler ve yeni özellikler sunulmuştur.

Bu makalede, Batch AI API önceki bir sürümünü kullandıysanız, kodu ve betikleri, yeni API'NİZLE çalışmak üzere değiştirmek nasıl açıklar. 

## <a name="whats-changing"></a>Değişen nedir?

Müşteri geri bildirimine yanıt olarak, biz işleri ve Batch AI kaynakları yönetmek kolaylaştırarak sayısına yönelik sınırlar kaldırırsınız. İki yeni kaynaklar, yeni API tanıtır *çalışma* ve *deneme*. Deneme altında ilgili eğitim işleri toplayın ve tüm ilgili Batch AI kaynakları (kümeleri, dosya sunucuları, denemeleri, işleri) çalışma alanı altında düzenleyin.  

* **Çalışma alanı** -Batch AI kaynak tüm türlerin en üst düzey bir koleksiyon. Kümeler ve dosya sunucuları, bir çalışma alanında yer alır. Çalışma alanları genellikle ayrı iş farklı gruplara veya projelere ait. Örneğin, bir geliştirme ve test çalışma alanı olabilir. Büyük olasılıkla yalnızca sınırlı sayıda abonelik başına çalışma alanı gerekir. 

* **Deneme** -sorgulanan ve yönetilen birlikte ilgili işleri koleksiyonu. Örneğin, bir denemeyi hyper parametresi ayarlama gözden geçirme işleminin bir parçası olarak gerçekleştirilen tüm işleri kullanın. 

Aşağıdaki görüntüde bir örnek kaynak hiyerarşisini gösterir. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>İzleme ve var olan kaynakları yönetme
Zaten oluşturduğunuz Batch AI kümeleri, işleri veya dosya sunucuları, her kaynak grubunda bir Batch AI hizmeti adlı bir çalışma alanı oluşturur `migrated-<region>` (örneğin, `migrated-eastus`) ve adlı bir deneme `migrated`. Daha önce oluşturulan işler, kümeler veya dosya sunucuları erişmek için geçirilen çalışma alanını kullanın ve denemeniz gerekir. 

### <a name="portal"></a>Portal 
Portalı kullanarak daha önce oluşturulan işler, kümeler veya dosya sunucuları erişmek için öncelikle seçin `migrated-<region>` çalışma. Çalışma alanını seçtikten sonra yeniden boyutlandırma veya küme silme ve iş durumu ve çıkışlarını görüntülemek gibi işlemleri gerçekleştirin. 

### <a name="sdks"></a>SDK’lar 
İşleri erişmek için kümeler veya dosya sunucuları daha önce Batch AI SDK'ları oluşturulan çalışma alanı adı sağlayın ve name parametreleri denemeler yapın. 

Python SDK'sı kullanıyorsanız ilgili değişiklikler aşağıdaki örneklerde gösterilir. Değişiklikleri diğer Batch AI SDK'ları benzerdir. 


#### <a name="get-old-cluster"></a>Eski kümeyi Al 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Eski küme silme 

```python
client.clusters.delete(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="get-old-file-server"></a>Eski dosya sunucusu Al 

```python
cluster = client.fileservers.get(resource_group_name, 'migrated-<region>', fileserver_name)
```

#### <a name="delete-old-file-server"></a>Eski dosya sunucusunu silme  

```python
client.fileservers.delete(resource_group_name, 'migrated-<region>', fileserver_name)
``` 


#### <a name="get-old-job"></a>Eski iş alın 

```python
cluster = client.jobs.get(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```

#### <a name="delete-old-job"></a>Eski iş silme

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>Azure CLI 
 
Daha önce oluşturduğunuz erişim işleri, kümeler veya dosya sunucuları için Azure CLI'yı kullanırken kullanın `-w` ve `-e` çalışma sağlayın ve adları denemek için parametreleri. 


#### <a name="get-old-cluster"></a>Eski kümeyi Al

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Eski küme silme 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Eski dosya sunucusu Al

```azurecli
az batchai file-server show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Eski dosya sunucusunu silme 

```azurecli
az batchai file-server delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Eski iş alın

```azurecli
az batchai job show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Eski iş silme 

```azurecli
az batchai job delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Batch AI kaynak oluşturma 
 
Var olan Batch AI Sdk'lardan birini kullanıyorsanız, bu kod değişikliği yapmadan (işleri, kümeleri veya dosya sunucularının) Batch AI kaynak oluşturmak için kullanmaya devam edebilirsiniz. Ancak, yeni bir SDK yükselttikten sonra aşağıdaki değişiklikleri yapmanız gerekir.
 
### <a name="create-workspace"></a>Çalışma alanı oluşturma 
Senaryonuza bağlı olarak, portal veya CLI aracılığıyla el ile tek seferlik bir çalışma alanı oluşturabilirsiniz. CLI'yı kullanıyorsanız, aşağıdaki komutu kullanarak bir çalışma alanı oluşturun: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Deneme oluşturma 


Senaryonuza bağlı olarak, portal veya CLI aracılığıyla el ile tek seferlik bir deneme oluşturabilirsiniz. CLI'yı kullanıyorsanız, aşağıdaki komutu kullanarak bir deneme oluşturun: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Kümeler, dosya sunucuları ve işleri oluşturma
 
İşleri, kümeler veya dosya sunucuları oluşturmak için portalı kullanıyorsanız, portalı çalışma alanı adı sağlayın ve name parametreleri deneme oluşturma deneyimi sırasında yönlendirecektir.

İşleri, kümeler veya güncelleştirilmiş SDK'sı üzerinden dosya sunucuları oluşturmak için çalışma alanı adı parametresi sağlayın. Python SDK'sı kullanıyorsanız ilgili değişiklikler aşağıdaki örneklerde gösterilir. Değiştirin *çalışma_alanı_adı* ve *experiment_name* çalışma ve daha önce oluşturduğunuz deneme. Değişiklikleri diğer Batch AI SDK'ları benzerdir. 


#### <a name="create-cluster"></a>Küme oluşturma 

```python
_ = client.clusters.create(resource_group_name, workspace_name, cluster_name, cluster_create_parameters).result()
```

#### <a name="create-file-server"></a>Dosya sunucusu oluşturma 

```python
_ = client.fileservers.create(resource_group_name, workspace_name, fileserver_name, fileserver_create_parameters).result()
```

#### <a name="create-job"></a>İş oluştur 

```python
_ = client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name job_create_parameters).result()
```


## <a name="next-steps"></a>Sonraki adımlar

* Batch AI API Başvurusu bakın: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [Node.js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai), ve [REST](/rest/api/batchai)