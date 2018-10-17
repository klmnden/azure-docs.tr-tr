---
title: Azure Hızlı Başlangıç - Derin öğrenme eğitimi - Azure CLI | Microsoft Docs
description: Hızlı Başlangıç - Azure CLI kullanarak Batch AI ile tek bir GPU üzerinde bir TensorFlow derin öğrenme sinir ağını eğitmeyi hızlı bir şekilde öğrenin
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
ms.openlocfilehash: 99d864a5d519ce56a559bea4db7fe89a113e47b9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157931"
---
# <a name="quickstart-train-a-deep-learning-model-with-batch-ai"></a>Hızlı Başlangıç: Batch AI ile bir derin öğrenme modeli eğitme

Bu hızlı başlangıçta, Batch AI tarafından yönetilen GPU özellikli bir sanal makine üzerinde örnek bir derin öğrenme modelini eğitme gösterilmektedir. Batch AI, veri bilimcilerinin ve yapay zeka araştırmacılarının Azure sanal makine kümelerindeki yapay zeka ve makine öğrenimi modellerini ölçeğe uygun olarak eğitmesini sağlayan bir yönetilen hizmettir. 

Bu örnekte, el yazısı rakamlardan oluşan [MNIST veritabanı](http://yann.lecun.com/exdb/mnist/) üzerinde örnek bir [TensorFlow](https://www.tensorflow.org/) sinir ağı eğitmek üzere Batch AI ayarlamak için Azure CLI’yi kullanırsınız. Bu hızlı başlangıcı tamamladıktan sonra, bir yapay zeka veya makine öğrenimi modelini eğitmek için Batch AI kullanmayla ilgili temel kavramları anlayacak ve daha büyük ölçekte farklı modelleri eğitmeyi denemek için hazır olacaksınız.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.38 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). 

Bu hızlı başlangıçta, komutları Cloud Shell veya yerel bilgisayarınızdaki bir Bash kabuğunda çalıştırdığınız varsayılır. [Azure CLI ile Batch AI kümesi oluşturmaya](quickstart-create-cluster-cli.md) yönelik hızlı başlangıcı zaten tamamladıysanız, kaynak grubu ve Batch AI kümesi oluşturmayı içeren ilk iki adımı atlayın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`az group create` komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus2* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Doğu ABD 2 konumunu veya Batch AI hizmetinin kullanılabilir olduğu başka bir konumu seçtiğinizden emin olun. 

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

Batch AI kümesi oluşturmak için, `az batchai cluster create` komutunu kullanın. Aşağıdaki örnekte, şu özellikler ile tek düğümlü bir küme oluşturulmuştur:

* Bir NVIDIA Tesla K80 GPU’ya sahip NC6 VM boyutunu kullanır. Azure, farklı NVIDIA GPU'ları ile çeşitli VM boyutları sunar.
* Kapsayıcı tabanlı uygulamaları barındırmak için tasarlanmış varsayılan bir Ubuntu Server görüntüsü çalıştırır. Bu dağıtımda eğitim çoğu iş yükünü çalıştırabilirsiniz. 
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

Küme oluşturmanın erken aşamalarında, çıkış aşağıdakine benzer ve kümenin `resizing` olduğunu gösterir:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```

Küme durumu değiştirilirken eğitim betiğini karşıya yüklemek ve eğitim işini oluşturmak için aşağıdaki adımları uygulayın. Durum `steady` olduğunda ve tek düğüm `Idle` olduğunda küme eğitim işini çalıştırmak için hazırdır.

## <a name="upload-training-script"></a>Eğitim betiğini karşıya yükleme

Eğitim betiğiniz ve eğitim çıkışınızı depolamak üzere bir depolama hesabı oluşturmak için `az storage account create` komutunu kullanın.

```azurecli-interactive
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus2 \
    --sku Standard_LRS
```

`az storage share create` komutunu kullanarak hesapta `myshare` adlı bir Azure dosya paylaşımı oluşturun:

```azurecli-interactive
az storage share create \
    --name myshare \
    --account-name mystorageaccount
```

Azure dosya paylaşımında dizin oluşturmak için `az storage directory create` komutunu kullanın. Eğitim betiği için `scripts` dizinin ve eğitim çıkışı için `logs` öğesini oluşturun:

```azurecli-interactive
# Create /scripts directory in file share
az storage directory create \
    --name scripts \
    --share-name myshare \
    --account-name mystorageaccount

# Create /logs directory in file share 
az storage directory create \
    --name logs \
    --share-name myshare \
    --account-name mystorageaccount
```

Bash kabuğunuzda, yerel bir çalışma dizini oluşturun ve TensorFlow [convolutional.py](https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py) örneğini indirin. Python betiği, 0’dan 9’a kadar 60.000 el yazısı sayıyı içeren MNIST görüntü kümesinde kıvrımlı bir sinir ağını eğitir. Daha sonra modeli bir dizi test örneğiyle test eder.

```bash
wget https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py
```

`az storage file upload` komutunu kullanarak betiği paylaşımda `scripts` dizinine yükleyin.

```azurecli-interactive
az storage file upload \
    --share-name myshare \
    --path scripts \
    --source convolutional.py \
    --account-name mystorageaccount
```

## <a name="submit-training-job"></a>Eğitim işi gönderme

İlk olarak, `az batchai experiment create` komutunu kullanarak çalışma alanınızda bir Batch AI *denemesi* oluşturun. Deneme, ilgili Batch AI işleri için kullanılan mantıksal kapsayıcıdır.

```azurecli-interactive
az batchai experiment create \
    --name myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Çalışma dizininizde aşağıdaki içeriğe sahip `job.json` adlı bir eğitim iş yapılandırma dosyası oluşturun. Eğitim işini gönderdiğinizde bu yapılandırma dosyasını geçirirsiniz.

Bu `job.json` dosyası Python betik dosyasını bulup GPU düğümündeki bir TensorFlow kapsayıcısında çalıştırmak için ayarları içerir. Ayrıca, işin çıkış dosyalarının Azure depolamada kaydedileceği konumu belirtir. `<AZURE_BATCHAI_STORAGE_ACCOUNT>` depolama hesabı adının iş gönderme sırasında belirtileceğini gösterir.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 1,
        "tensorFlowSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/scripts/convolutional.py"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/logs",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Düğümde işi göndermek için, `job.json` yapılandırma dosyasını ve depolama hesabınızın adını geçirerek `az batchai job create` komutunu kullanın:

```azurecli-interactive
az batchai job create \
    --name myjob \
    --cluster mycluster \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --config-file job.json \
    --storage-account-name mystorageaccount
```

Komut iş özelliklerini döndürür ve tamamlanması birkaç dakika sürer. Bu işin ilerleyişini izlemek için, `stdout-wk-0.txt` dosyasının düğümdeki standart çıkış dizininden akışını yapmak üzere `az batchai job file stream` komutunu kullanın. Eğitim betiği, iş çalıştırılmaya başladıktan sonra bu dosyayı oluşturur.  

```azurecli-interactive
az batchai job file stream \
    --job myjob \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --file-name stdout-wk-0.txt
```

Örnek çıktı:

```
File found with URL "https://mystorageaccount.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/myResourceGroup/workspaces/myworkspace/experiments/myexperiment/jobs/myjob/<JOB_ID>/stdouterr/stdout-wk-0.txt?sv=2016-05-31&sr=f&sig=Kih9baozMao8Ugos%2FVG%2BcsVsSeY1O%2FTocCNvLQhwtx4%3D&se=2018-06-20T22%3A07%3A30Z&sp=rl". Start streaming
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Step 0 (epoch 0.00), 14.9 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 9.7 ms
Minibatch loss: 3.240, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.7%
Step 200 (epoch 0.23), 8.3 ms
Minibatch loss: 3.335, learning rate: 0.010000
Minibatch error: 7.8%
Validation error: 4.5%
Step 300 (epoch 0.35), 8.3 ms
Minibatch loss: 3.157, learning rate: 0.010000
Minibatch error: 3.1%
...
Step 8500 (epoch 9.89), 8.3 ms
Minibatch loss: 1.605, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.9%
Test error: 0.8%
```

İş tamamlandığında akış durdurulur. Örnek betik 10’un üzerinde *dönem* eğitir veya eğitim veri kümesinden geçer. Bu örnekte, 10 dönemden sonra, eğitilen model yalnızca %0,8 oranında test hatası ile çalışır.

## <a name="get-job-output"></a>İş çıkışı alma

Batch AI, her işin çıkışı için depolama hesabında benzersiz bir klasör yapısı oluşturur. JOB_OUTPUT_PATH ortam değişkenini bu yol ile ayarlayın. Daha sonra, `az storage file list` komutunu kullanarak depolama alanındaki çıkış dosyalarını listeleyin:

```azurecli-interactive
export JOB_OUTPUT_PATH=$(az batchai job show --name myjob --experiment myexperiment --workspace myworkspace --resource-group myResourceGroup --query jobOutputDirectoryPathSegment --output tsv)

az storage file list \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr \
    --output table
```

Çıkış şuna benzer olacaktır:

```
Name               Content Length  Type    Last Modified
---------------  ----------------  ------  ---------------
execution.log               14866  file
stderr-wk-0.txt              1527  file
stdout-wk-0.txt             11027  file
```

Bir veya daha fazla dosyayı yerel çalışma dizininize indirmek için `az storage file download` komutunu kullanın. Örnek:

```azurecli-interactive
az storage file download \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr/stdout-wk-0.txt
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Batch AI öğreticileri ve örnekleri ile devam etmek istiyorsanız, bu hızlı başlangıçta oluşturulan Batch AI çalışma alanı, kümesi ve depolama hesabını kullanın. 

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
    --resource-group myResourceGroup
```

Artık gerekli değilse, `az group delete` komutunu kullanarak Batch AI ve depolama kaynakları için kaynak grubunu kaldırabilirsiniz. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure CLI kullanarak tek GPU’lu bir VM üzerinde örnek bir TensorFlow derin öğrenme modelini eğitmek için Batch AI kullanmayı öğrendiniz. Daha geniş bir GPU kümesinde model eğitimini dağıtmak için, Batch AI öğreticisi ile devam edin.

> [!div class="nextstepaction"]
> [Dağıtılmış eğitim öğreticisi](./tutorial-horovod-tensorflow.md)