---
title: Güncelleştirilmiş Azure Batch AI API'sine geçirin | Microsoft Docs
description: Azure Batch AI kod ve çalışma alanını kullanın ve kaynakları denemek için betiklerini nasıl güncelleştirileceği
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
ms.openlocfilehash: c5e4c1569464d2e204edf13fe7534d80780524e8
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294969"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Güncelleştirilmiş toplu AI API'sine geçirin

Toplu AI REST API sürümü 2018-05-01 ve ilgili toplu AI SDK'lar ve Araçlar, önemli değişiklikleri ve yeni özellikleri tanıtılmıştır.

Toplu AI API önceki bir sürümünü kullandıysanız, bu makalede kod ve yeni API'NİZLE çalışmak üzere komut dosyaları değiştirmek açıklanmaktadır. 

## <a name="whats-changing"></a>Ne değişiyor?

Müşteri geri bildirimine yanıt olarak, biz iş sayısı ve toplu AI kaynakları yönetmek daha kolay sınırları kaldırırsınız. İki yeni kaynaklar, yeni API tanıtır *çalışma* ve *denemeler*. Bir deneme altında ilgili eğitim işleri toplamak ve tüm ilişkili toplu AI kaynakları (kümeleri, dosya sunucuları, denemeler, işler) çalışma alanı altında düzenleyin.  

* **Çalışma alanı** -toplu AI kaynakların her türlü en üst düzey bir koleksiyon. Kümeleri ve dosya sunucuları çalışma alanında yer alır. Çalışma alanları genellikle ayrı iş farklı gruplar veya projeleri ait. Örneğin, bir geliştirme ve test çalışma olabilir. Büyük olasılıkla çalışma alanları abonelik başına yalnızca sınırlı sayıda gerekir. 

* **Deneme** -sorgulanan ve birlikte yönetilen ilgili işleri koleksiyonu. Örneğin, bir hyper-parametre ayarlama tarama bir parçası olarak gerçekleştirilen tüm işleri gruplandırmak için bir deneme kullanın. 

Aşağıdaki resimde bir örnek kaynak hiyerarşisini gösterir. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>İzleme ve varolan kaynakları yönetme
Zaten oluşturduğunuz toplu AI kümeleri, işleri veya dosya sunucuları, her kaynak grubunda toplu AI hizmeti adlı bir çalışma alanı oluşturur `migrated-<region>` (örneğin, `migrated-eastus`) ve adlı bir denemeyi `migrated`. Daha önce oluşturulan işleri, kümelere veya dosya sunucularına erişmek için geçirilen çalışma alanını kullanın ve denemeniz gerekir. 

### <a name="portal"></a>Portal 
Daha önce oluşturulan işleri, kümelere veya dosya sunucuları portalını kullanarak erişmek için öncelikle seçin `migrated-<region>` çalışma. Çalışma alanı seçtikten sonra yeniden boyutlandırma veya küme silme ve iş durumu ve çıkışları görüntüleme gibi işlemleri gerçekleştirir. 

### <a name="sdks"></a>SDK’lar 
İşleri, kümelere veya dosya sunucuları toplu AI SDK'ları daha önce oluşturduğunuz erişmek için çalışma alanı adı sağlayın ve name parametreleri deneyin. 

Python SDK'sı kullanıyorsanız, ilgili değişiklikleri aşağıdaki örneklerde gösterilmektedir. Değişiklikler diğer toplu AI SDK içinde benzerdir. 


#### <a name="get-old-cluster"></a>Eski kümeyi Al 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Eski kümeyi Sil 

```python
client.clusters.delete(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="get-old-file-server"></a>Eski dosya sunucusu 

```python
cluster = client.fileservers.get(resource_group_name, 'migrated-<region>', fileserver_name)
```

#### <a name="delete-old-file-server"></a>Eski dosya sunucusu Sil  

```python
client.fileservers.delete(resource_group_name, 'migrated-<region>', fileserver_name)
``` 


#### <a name="get-old-job"></a>Eski işini alın 

```python
cluster = client.jobs.get(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```

#### <a name="delete-old-job"></a>Eski işini sil

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>Azure CLI 
 
Daha önce oluşturduğunuz erişim işleri, kümelere veya dosya sunucuları için Azure CLI, kullanırken `-w` ve `-e` çalışma sağlayın ve adları denemek için parametreler. 


#### <a name="get-old-cluster"></a>Eski kümeyi Al

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Eski kümeyi Sil 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Eski dosya sunucusu

```azurecli
az batchai file-server show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Eski dosya sunucusu Sil 

```azurecli
az batchai file-server delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Eski işini alın

```azurecli
az batchai job show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Eski işini sil 

```azurecli
az batchai job delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Toplu AI kaynakları oluşturun 
 
Var olan toplu AI SDK'ları birini kullanıyorsanız, kod değişiklikleri yapmadan toplu AI kaynakları (işleri, kümelere veya dosya sunucuları) oluşturmak için kullanmaya devam edebilirsiniz. Ancak, yeni SDK'sına yükselttikten sonra aşağıdaki değişiklikleri yapmanız gerekir.
 
### <a name="create-workspace"></a>Çalışma alanı oluştur 
Senaryonuza bağlı olarak, portal veya CLI aracılığıyla el ile tek seferlik bir çalışma alanı oluşturabilirsiniz. CLI kullanıyorsanız, aşağıdaki komutu kullanarak bir çalışma alanı oluşturun: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Deneme oluşturma 


Senaryonuza bağlı olarak, portal veya CLI aracılığıyla el ile tek seferlik bir deneme oluşturabilirsiniz. CLI kullanıyorsanız, aşağıdaki komutu kullanarak bir deneme oluşturma: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Kümeler, dosya sunucuları ve işleri oluşturma
 
Portal işleri, kümelere veya dosya sunucuları oluşturmak için portal kullanıyorsanız, çalışma alanı adı sağlayın ve name parametreleri denemeler oluşturma deneyimi sırasında yol gösterecektir.

İşleri oluşturmak için Küme veya güncelleştirilmiş bir SDK aracılığıyla dosya sunucuları çalışma alanı adı parametresi sağlayın. Python SDK'sı kullanıyorsanız, ilgili değişiklikleri aşağıdaki örneklerde gösterilmektedir. Değiştir *çalışma_alanı_adı* ve *experiment_name* çalışma ve daha önce oluşturduğunuz deneme. Değişiklikler diğer toplu AI SDK içinde benzerdir. 


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

* Toplu AI API başvurusuna bakın: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [Node.js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai)ve [REST](/rest/api/batchai)