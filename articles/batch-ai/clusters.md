---
title: Azure Batch AI kümeleri ile çalışma | Microsoft Docs
description: Bir Batch AI kümesi yapılandırması seçin ve oluşturma ve yapay ZEKA küme yönetme
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 61294d8b6b84b03b1e0c8d79b4d2855452c7f0e6
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057288"
---
# <a name="work-with-batch-ai-clusters"></a>Batch AI kümeleri ile çalışma 

Bu makalede, Azure Batch AI kümeleri ile nasıl çalışılacağı açıklanmaktadır. Bu kümeler, olası yapılandırmalar ve örnekler türlerini kavramını sunar. Bu makalede bir küme oluşturma ve yönetme için örneklerin çoğu Azure CLI'yı kullanın. Ancak, kümeleriyle çalışmak için Azure portalı ve Azure Batch AI SDK'ları da dahil olmak üzere diğer araçları kullanabilirsiniz.

Bir Batch AI kümesi hizmetinde çeşitli kaynaklar biridir. Bkz: [Batch AI kaynak bakış](resource-concepts.md) kümeleri hizmetinde anlamak için.

## <a name="introduction-to-clusters"></a>Kümelerine giriş

Batch AI kümesinde, makine öğrenimi ve yapay ZEKA eğitim işleri çalıştırmak için işlem kaynaklarını içerir. Bir kümedeki tüm düğümlerin aynı VM boyutu ve işletim sistemi görüntüsü vardır. Batch AI kümeleri farklı ihtiyaçları için özelleştirilmiş oluşturmak için birçok seçenek sunar. Genellikle, bir projeyi tamamlamak için gereken power işleme her kategori için farklı bir kümeye ayarlarsınız. Talebe göre ve bütçe yukarı ve aşağı kümedeki düğüm sayısını ölçekleyebilirsiniz. Kümeleri sağlanabilir ve bir takım arasında paylaşılan ya da bireyler her kendi kümesi sağlayabilirsiniz. 

Her küme olarak adlandırılan bir Batch AI kaynak altında mevcut bir *çalışma*. Herhangi bir küme sağlamadan önce ayarladığınız bir Batch AI Çalışma olması gerekir. Örneğin, Azure CLI'yı kullanırsanız kullanın [az batchai çalışma alanı oluşturma](/cli/azure/batchai/workspace?view=azure-cli-latest#az-batchai-workspace-create) çalışma alanını ayarlama komutu. 

Bir küme oluşturduktan sonra işleri bir anda bir kuyruğa gönderebilirsiniz. Batch AI, ardından kaynak ayırma işlemi işleri küme üzerinde çalışan işler. 

## <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Bir küme planlarken, önce bilgi işlem gereksinimlerinizi belirleyin. Bir küme gereksinimlerinize uyarlayın için batch AI esnek yapılandırma seçenekleri sunar. Bir küme hazırlama sırasında dikkate alınması gereken majors seçenekleri şunlardır:

* **VM boyutu** -seçim [VM boyutu](../virtual-machines/linux/sizes.md) içinde kullanılabilen bir [bölge desteklenen](https://azure.microsoft.com/global-infrastructure/services/) küme düğümleri için. Her kümenin yalnızca birini içerebilir, VM'nin boyutu birden çok VM görevlerinizi ihtiyacınız varsa her işlem gereksinim türü için ayrı bir küme sağlamanız gerekir. Machine learning'e ya da Gpu'lar yararlanan çerçeveleri ile geliştirilmiş yapay ZEKA modellerini eğitmek için bkz: [GPU için iyileştirilmiş sanal makine boyutlarını](../virtual-machines/linux/sizes-gpu.md) azure'da.
  
* **Öncelikli VM** -Batch AI, adanmış veya düşük öncelikli VM'ler için bir küme sunar. Özel VM'ler, kümedeki kullanmanız için ayrılmıştır. Düşük öncelikli seçeneği karşılığında Azure Vm'leri geri kazanır durumunda biterse işleri olasılığını önemli maliyet tasarrufları kullanılmayan Azure VM kapasitede ayırır. Düşük öncelikli VM üzerinde 24 saatten uzun süre çalışan işler de otomatik olarak ayırmalara. Bütçe önemliyse, düşük öncelikli VM'ler için kısa deneme işlemleriyle göz önünde bulundurun. Ardından, uzun işleri çalıştırma süresi olduğunda özel VM'ler için geçin.

* **Düğüm sayısı** -Batch AI, kümedeki düğüm sayısını el ile ve otomatik ölçeklendirme seçenekleri sunar. Böylece kendi işlem maliyetleri yönetebilir el ile seçeneğiyle, ne zaman bir kümenin ölçeğini artırma ve azaltma denetler. Otomatik ölçeklendirme seçeneği, işlem maliyetleri en aza indirmek için kullanmakta olduğunuz değil, kümenin her zaman downscales olmasını sağlar. 

  Sonra kümeyi el ile ölçeklendirme seçerseniz küme oluşturma sırasında ilk hedef tanımlayın. Batch AI başlangıçta ayırır düğüm sayısını hedefidir. Daha sonra düğüm sayısını el ile boyutlandırabilirsiniz.  

  Otomatik ölçeklendirme seçeneği tercih ederseniz, küme oluşturma sırasında maksimum ve minimum hedef düğüm sayısını tanımlayın. Hedef düğüm tarafından desteklenen en yüksek sayısı 0'dan değişebilir, [Batch AI çekirdek kotası](quota-limits.md). 0 hedef küme yapılandırmanızı herhangi bir işlem maliyetleri için ücretlendirilmeden korumanıza olanak sağlar. Ardından, kota sınırı desteklediği daha yüksek bir hedef istek, sağlama başarısız olur. 

* **VM görüntüsü** -Batch AI varsayılan olarak, kapsayıcı iş yüklerini destekleyen varsayılan Ubuntu Server görüntüsü ile küme Vm'leri sağlar. Azure Market'ten gibi başka bir önceden yapılandırılmış Linux görüntüsü seçebilirsiniz bir [veri bilimi sanal makinesi](../machine-learning/data-science-virtual-machine/overview.md). 

* **Depolama** -Batch AI, Azure CLI kullanarak bir küme oluştururken seçebileceğiniz bir otomatik depolama seçeneği sunar. Bu seçenek otomatik olarak bir Azure dosya paylaşımı ve Blob kapsayıcısı altında yeni bir depolama hesabı oluşturur. Bu depolama kaynaklarını, yürütme süresi, dosyaları yerel bir yoldan erişilmesine izin verme sırasında için kümedeki düğümlerin her birine bağlanır. Depolama hesabı adları, dosya paylaşım adı, Blob kapsayıcı adı ve bağlama yolları tümü de küme oluşturma sırasında ek parametreler kullanılarak özelleştirilebilir varsayılan değerlere sahip. 

  Hiçbir depolama seçenekleri tanımlanmışsa, işleri gönderirken tanımlama ve depolama kaynaklarını ayrı ayrı oluşturmanız gerekir. Bu seçenek kümenizi kuruluş düzeyinde yönetilir, ancak depolama alanınızı kullanıcı düzeyinde yönetilir yararlıdır. Azure Depolama'ya dosya yükleme ve yürütme sırasında erişim hakkında daha fazla bilgi için bkz. [Store Batch AI işi girdi ve çıktı ile Azure depolama](use-azure-storage.md).

* **Kullanıcı erişimini** - Batch AI olanak oluşturmak, genel ve özel SSH anahtar dosyaları bir küme oluştururken ya da tek tek düğümlere SSH kullanabilirsiniz, böylece kendi SSH anahtarları sağlayın. (Varsayılan olarak geçerli bir kullanıcı olarak ayarlanır) kullanıcı adı ve parola da tanımlayabilirsiniz. Bu kimlik bilgilerini, çeşitli ölçümleri görüntüleyin ya da daha fazla işlerinizi kavramak için yürütme sırasında düğümlerine erişmek için kullanılabilir.

* **Kurulum görevi** -Batch AI ayırma sırasında her bir düğümde yürütülmek üzere komut satırı bağımsız değişkenleri tanımlamanıza izin verir. Burada çıkış dosyası için oturum açmış olmanız yolunu da tanımlayabilirsiniz. Temel görüntü ötesinde ek sağlama adımlar varsa bu seçeneği kullanın.

* **Ek yapılandırma** -daha özel yapılandırmaları yapmanızı şart koşabileceği birkaç daha az yaygın senaryo vardır. Bu durumda, bir [küme yapılandırma dosyası](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#using-cluster-configuration-file) bir küme oluşturmak için Azure CLI komutuyla eklenebilir. 

## <a name="create-the-cluster"></a>Kümeyi oluşturma

Küme yapılandırma seçeneklerini karar verdikten sonra kümeyi oluşturmak için Azure CLI, Azure portalı ve Batch yapay ZEKA API'lerini kullanın. Örneğin, Azure CLI kullanarak bir küme oluşturmak için izleyebilirsiniz [az batchai küme oluşturma](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-create) gereken yapılandırmalar sunan tam komutu oluşturmak için belgeler. 

Aşağıdaki komut hükümler bir işler için standart_nc6 küme bir düğüm ve SSH erişimini ile en temel yapılandırma. Varsayılan olarak, bu küme en son varsayılan Ubuntu Server görüntüsü çalıştıran özel VM'ler içerir.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --target 1 \
    --generate-ssh-keys
```

Aşağıdaki örnek hükümler bir işler için standart_nc6 küme 0'dan 4 düğümlerine otomatik olarak ölçeklenen ve düşük öncelikli düğümler ve otomatik depolama hesabı kullanır. Düşük maliyet ve yönetimi kolay olan bir küme gerekiyorsa bu ayar iyi bir yapılandırmadır. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --vm-priority lowpriority \
    --max 4 \
    --min 0 \
    --use-auto-storage 
```

Aşağıdaki örnek bir veri bilimi sanal Makinesi'nin görüntüsünü içeren bir işler için standart_nc6 kümesi sağlar, özel depolama ve bağlama seçeneklerini, özel SSH kimlik bilgilerini yükleyen bir kurulum görevi *sıkıştırmasını* paketi ile bir küme ek kurulum yapılandırma dosyası. Bu yapılandırma, daha fazla özelleştirilmiş bir kümenin kendi gereksinimleriniz için bir örnektir.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --image UbuntuDSVM \ 
    --config-file cluster.json \
    --setup-task 'apt install unzip -y'
    --storage-account-name <STORAGE ACCOUNT NAME> \
    --nfs-name nfsmount \
    --afs-name afsmount \
    --bfs-name bfsmount \
    --user-name adminuser \
    --ssh-key id_rsa.pub \
    --password secretpassword 
```

## <a name="monitor-the-cluster"></a>Küme izleme

[Az batchai küme listesi](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-list), [az batchai kümenin Göster](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-show), ve [az batchai küme düğümü listesi](/cli/azure/batchai/cluster/node?view=azure-cli-latest#az-batchai-cluster-node-list) komutları, her küme için çeşitli bilgileri izlemek için kullanılabilir.

### <a name="list-all-clusters"></a>Tüm küme listesi

Aşağıdaki komut listesi Yanıtla kümelerinin bir çalışma alanında.

```azurecli-interactive
az batchai cluster list \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
```

### <a name="show-information-about-a-cluster"></a>Bir küme ile ilgili bilgileri gösterme

Aşağıdaki komut belirli bir küme hakkındaki tüm bilgiler bir tablo biçiminde gösterir.

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --output table
```

Kümenizi otomatik depolama seçeneğini kullanmayı sağlanması halinde, betikler ve eğitim işleri karşıya yüklerken kullanılacak depolama hesabı adı almak isteyebilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --query "nodeSetup.mountVolumes.azureFileShares[0].{storageAccountName:accountName}"
```

Çıktı aşağıdakine benzer olmalıdır.

```JSON
{
  "storageAccountName": "baixxxxxxxxx"
}
```

### <a name="list-cluster-nodes"></a>Liste küme düğümleri

Küme düğümlerine bağlanmak istiyorsanız, aşağıdaki komutu düğüm ve bağlantı bilgilerini listesini alır.  

```azurecli-interactive
az batchai cluster node list \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
 ```

Her düğüm için çıkış aşağıdakine benzer olacaktır:

```JSON
[
  {
    "ipAddress": "40.68.xxx.xxx",
    "nodeId": "tvm-xxxxxxxxxx-xxxxxxxx",
    "port": 50000.0
  }
]
```
Bu bilgiler, aşağıdakine benzer bir komut kullanarak bir düğümü için bir SSH bağlantısı yapmak için kullanabilirsiniz.

```bash
ssh myusername@40.68.xxx.xxx -p 50000
``` 

## <a name="submit-jobs-to-the-cluster"></a>İşleri kümeye gönderin

Küme sağlandıktan sonra düğümlerde çalıştırılacak işleri gönderebilirsiniz. Bkz: [az batchai iş](/cli/azure/batchai/job?view=azure-cli-latest) hazırlama, gönderin ve Azure CLI kullanarak işleri izlemek için farklı yollar için komut. 

## <a name="downscale-cluster-for-later-use"></a>Daha sonra kullanmak için downscale küme

İşlerinizi çalıştıran tamamladıktan sonra kümenizi aşağı ölçeklemenizi isteyeceksiniz. Her zaman, bunlar kaydetmek için kullanılmayan downscale kümeleri işlem maliyetleri önerilir. 0 düğümleri kümeye downscaling yeniden ölçeklemenizi gerektiğinde kümeleri gelecekte yeniden sağlamak zorunda kalmamanız çalışırken fatura ücretlerinizin durdurmanızı sağlar. Otomatik ölçeklendirme küme seçtiyseniz, küme için en düşük hedef aşağı ölçeklemenizi. El ile ölçeklendirme seçildiyse, aşağıdaki komutu kullanarak küme aşağı ölçeklemenizi.

```azurecli-interactive
az batchai cluster resize \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE> \
    --target 0
```

## <a name="delete-a-cluster"></a>Küme silme

Bir küme kullanarak tamamladığınızda kullanın [az batchai küme silme](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-delete) silmek için komutu.

```azurecli-interactive
az batchai cluster delete \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE>
```

Kaynak grubunuzun da otomatik olarak silinmesi, kaynak grubu altında sağlanan tüm kümelerin siler.

```azurecli-interactive
az group delete --name <RESOURCE GROUP>
```

## <a name="next-steps"></a>Sonraki adımlar

Bir Batch AI kümesi oluşturmaya daha fazla örnek için bkz: [portalı](quickstart-create-cluster-portal.md) veya [Azure CLI](quickstart-create-cluster-cli.md) hızlı, veya [Batch AI tarifleri](https://github.com/Azure/BatchAI/tree/master/recipes) GitHub üzerinde.
