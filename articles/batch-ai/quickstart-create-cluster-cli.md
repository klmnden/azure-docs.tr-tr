---
title: Azure Hızlı Başlangıç - Batch AI kümesi oluşturma - Azure CLI | Microsoft Docs
description: Hızlı Başlangıç - Makine öğrenimi ve yapay zeka modellerini eğitmek için Batch AI kümesi oluşturma - Azure CLI
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
ms.devlang: CLI
ms.topic: quickstart
ms.date: 09/03/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 1ea12c9a544704ea91b85ae944e611e6769b5592
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407142"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak Batch yapay ZEKA eğitim işleri için küme oluşturma

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Bu hızlı başlangıçta, yapay zeka ve makine öğrenimi modellerini eğitmek üzere kullanabileceğiniz bir Batch AI kümesi oluşturmak için Azure CLI’yi nasıl kullanabileceğiniz gösterilmektedir. Batch AI, veri bilimcilerinin ve yapay zeka araştırmacılarının Azure sanal makine kümelerindeki yapay zeka ve makine öğrenimi modellerini ölçeğe uygun olarak eğitmesini sağlayan bir yönetilen hizmettir.

Kümede başlangıçta bir GPU düğümü vardır. Bu hızlı başlangıcı tamamladıktan sonra, ölçeğini artırıp modellerinizi eğitmek için kullanabileceğiniz bir kümeniz olacak. Batch AI, [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) araçları veya [Visual Studio Tools for AI](https://github.com/Microsoft/vs-tools-for-ai) kullanarak kümeye eğitim işleri gönderin.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.38 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). 

Bu hızlı başlangıçta, komutları Cloud Shell veya yerel bilgisayarınızdaki bir Bash kabuğunda çalıştırdığınız varsayılır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`az group create` komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus2* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Doğu ABD 2 gibi Batch AI hizmetinin kullanılabilir olduğu bir konum seçtiğinizden emin olun.

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-batch-ai-cluster"></a>Batch AI kümesi oluşturma

İlk olarak, bir Batch AI *çalışma alanı* oluşturmak için `az batchai workspace create` komutunu kullanın. Batch AI kümeleriniz ve diğer kaynaklarınızı düzenlemek için bir çalışma alanınızın olması gerekir.

```azurecli-interactive
az batchai workspace create \
    --workspace myworkspace \
    --resource-group myResourceGroup 
```

Batch AI kümesi oluşturmak için, `az batchai cluster create` komutunu kullanın. Aşağıdaki örnekte, şu özellikler ile bir küme oluşturulmuştur:

* Bir NVIDIA Tesla K80 GPU’ya sahip NC6 VM boyutunda tek bir düğüm içerir. 
* Çoğu eğitim iş yükü için kullanabileceğiniz kapsayıcı tabanlı uygulamaları barındırmak için tasarlanmış varsayılan bir Ubuntu Server görüntüsü çalıştırır. 
* *myusername* adlı bir kullanıcı hesabı ekler ve yerel ortamınızdaki varsayılan anahtar konumunda (*~/.ssh*) zaten yoksa SSH anahtarları oluşturur. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --vm-size Standard_NC6 \
    --target 1 \
    --user-name myusername \
    --generate-ssh-keys
```

Komut çıkışı, küme özelliklerini gösterir. Düğümü oluşturup başlatmak birkaç dakika sürer. Kümenin durumunu görmek için `az batchai cluster show` komutunu çalıştırın. 

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

Küme oluşturmanın erken aşamalarında, çıkış aşağıdakine benzer ve kümeyi `resizing` durumunda gösterir:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```
Durum `steady` olduğunda ve tek düğüm `Idle` olduğunda küme kullanmak için hazırdır.

## <a name="list-cluster-nodes"></a>Küme düğümlerini listeleme 

Uygulama yüklemek veya bakım yapmak için küme düğümlerine (bu durumda, tek bir düğüme) bağlanmanız gerekiyorsa, `az batchai cluster node list` komutunu çalıştırarak bağlantı bilgilerini alın:


```azurecli-interactive
az batchai cluster node list \
    --cluster mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup 
 ```

JSON çıkışı şuna benzer:

```JSON
[
  {
    "ipAddress": "40.68.254.143",
    "nodeId": "tvm-1816144089_1-20180626t233430z",
    "port": 50000.0
  }
]
```
Düğüme bir SSH bağlantısı kurmak için bu bilgileri kullanın. Örneğin, aşağıdaki komutta IP adresini düğümünüzdeki doğru IP adresiyle değiştirin:

```bash
ssh myusername@40.68.254.143 -p 50000
``` 
Devam etmek için SSH oturumundan çıkın.

## <a name="resize-the-cluster"></a>Kümeyi yeniden boyutlandırma

Kümenizi eğitim işini çalıştırmak için kullandığınızda, daha fazla işlem kaynağına ihtiyacınız olabilir. Örneğin, dağıtılmış bir eğitim işi için boyutu 2 düğüme büyütmek için, [batch ai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize) komutunu çalıştırın:

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 2
```

Kümenin yeniden boyutlandırılması birkaç dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Batch AI öğreticileri ve örnekleri ile devam etmek istiyorsanız, bu hızlı başlangıçta oluşturulan Batch AI çalışma alanını kullanın. 

Düğümler çalışırken Batch AI kümesi için ücret ödersiniz. Çalıştırılacak iş olmadığında küme yapılandırmasını tutmak istiyorsanız, kümeyi 0 düğüm olarak yeniden boyutlandırın. 

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 0
```

Daha sonra, işlerinizi çalıştırmak için 1 veya daha fazla düğüm olarak yeniden boyutlandırın. Bir kümeye artık ihtiyacınız olmadığında, `az batchai cluster delete` komutu ile silebilirsiniz:

```azurecli-interactive
az batchai cluster delete \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
```

Artık gerekli değilse, `az group delete` komutunu kullanarak Batch AI kaynakları için kaynak grubunu kaldırabilirsiniz. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI’yi kullanarak bir Batch AI kümesi oluşturmayı öğrendiniz. Model eğitmek üzere Batch AI kümesi kullanma hakkında daha fazla bilgi edinmek için, ayrıntılı bir öğrenme modelini eğitmeye yönelik hızlı başlangıç adımlarına gidin.

> [!div class="nextstepaction"]
> [Ayrıntılı bir öğrenme modeli eğitme](./quickstart-tensorflow-training-cli.md)
