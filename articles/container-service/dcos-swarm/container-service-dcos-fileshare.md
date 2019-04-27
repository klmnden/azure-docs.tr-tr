---
title: (KULLANIM DIŞI) Dosya Paylaşımı için Azure DC/OS kümesi
description: Azure Container Service’te dosya oluşturma ve bu dosyayı DC/OS kümesine bağlama
services: container-service
author: julienstroheker
manager: dcaro
ms.service: container-service
ms.topic: tutorial
ms.date: 06/07/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: e6651fc5988a1e1830807219cda02ab057db9a4f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60480393"
---
# <a name="deprecated-create-and-mount-a-file-share-to-a-dcos-cluster"></a>(KULLANIM DIŞI) Oluşturma ve bir DC/OS kümesi için bir dosya paylaşımını bağlama

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu öğreticide, Azure'da dosya paylaşımı oluşturma ve bunu DC/OS kümesinin her aracısına ve ana düğümüne bağlama işlemi ayrıntılarıyla açıklanır. Dosya paylaşımı ayarlamak, yapılandırma, erişim ve günlükler gibi daha birçok dosyanın kümeniz genelinde paylaşılmasını kolaylaştırır. Bu öğreticide aşağıdaki görevler tamamlanır:

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Dosya paylaşımı oluşturma
> * Paylaşımı DC/OS kümesine bağlama

Bu öğreticideki adımları tamamlamak için bir ACS DC/OS kümesi gerekir. Gerekirse, [bu betik örneği](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) sizin için bir tane oluşturabilir.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a>Microsoft Azure'da dosya paylaşımı oluşturma

Azure dosya paylaşımını ACS DC/OS kümesiyle kullamadan önce, depolama hesabı ve dosya paylaşımı oluşturulmalıdır. Depolamayı ve dosya paylaşımını oluşturmak için aşağıdaki betiği çalıştırın. Parametreleri ortamınızdaki değerlerle güncelleştirin.

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create the storage account with the parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-the-share-in-your-cluster"></a>Paylaşımı kümenize bağlama

Bundan sonra, dosya paylaşımının kümenizin içindeki her sanal makineye bağlanması gerekir. Bu görevi tamamlamak için cifs aracı/protokolü kullanılır. Bağlama işlemi kümenin her düğümünde el ile veya kümedeki her düğümde bir betik çalıştırılarak tamamlanabilir.

Bu örnekte, iki betik çalıştırılır: biri Azure dosya paylaşımını bağlamak için ve ikincisi de bu betiği DC/OS kümesinin her düğümünde çalıştırmak için.

İlk olarak, Azure depolama hesabı adı ve erişim anahtarı gerekir. Aşağıdaki komutları çalıştırarak bu bilgileri alın. Bu değerleri not alın çünkü sonraki bir adımda kullanılacaklar.

Depolama hesabı adı:

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group $DCOS_PERS_RESOURCE_GROUP --query "[?contains(name, '$DCOS_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCT
```

Depolama hesabı erişim anahtarı:

```azurecli-interactive
az storage account keys list --resource-group $DCOS_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

Sonra, DC/OS ana şablonunun FQDN'sini alın ve bir değişkende depolayın.

```azurecli-interactive
FQDN=$(az acs list --resource-group $DCOS_PERS_RESOURCE_GROUP --query "[0].masterProfile.fqdn" --output tsv)
```

Özel anahtarınızı ana düğüme kopyalayın. Bu anahtar, kümedeki tüm düğümlerle ssh bağlantısı oluşturmak için gerekir. Küme oluşturulurken varsayılan olmayan bir değeri kullanıldıysa kullanıcı adını güncelleştirin. 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

DC/OS tabanlı kümenizin ana şablonuyla (veya ilk ana şablonuyla) bir SSH bağlantısı oluşturun. Küme oluşturulurken varsayılan olmayan bir değeri kullanıldıysa kullanıcı adını güncelleştirin.

```azurecli-interactive
ssh azureuser@$FQDN
```

**cifsMount.sh** adlı bir dosya oluşturun ve şu içerikleri dosyaya kopyalayın. 

Bu betik Azure dosya paylaşımını bağlamak için kullanılır. `STORAGE_ACCT_NAME` ve `ACCESS_KEY` değişkenlerini daha önce toplanan bilgilerle güncelleştirin.

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
SHARE_NAME=dcosshare
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install the cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create the local folder that will contain our share
if [ ! -d "/mnt/share/$SHARE_NAME" ]; then sudo mkdir -p "/mnt/share/$SHARE_NAME" ; fi

# Mount the share under the previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/$SHARE_NAME /mnt/share/$SHARE_NAME -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
**getNodesRunScript.sh** adıyla ikinci bir dosya oluşturun ve aşağıdaki içerikleri dosyaya kopyalayın. 

Bu betik tüm küme düğümlerini bulur ve sonra da **cifsMount.sh** betiğini çalıştırarak her birine dosya paylaşımını bağlar.

```azurecli-interactive
#!/bin/bash

# Install jq used for the next command
sudo apt-get install jq -y

# Get the IP address of each node using the mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From the previous file created, run our script to mount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

Azure dosya paylaşımını kümenin tüm düğümlerine bağlamak için betiği çalıştırın.

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

Artık dosya paylaşımına kümenin her düğümünde `/mnt/share/dcosshare` konumundan erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki adımlar kullanılarak bir DC/OS kümesine bir Azure dosya paylaşımı sağlandı:

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Dosya paylaşımı oluşturma
> * Paylaşımı DC/OS kümesine bağlama

Azure Container Registry’yi Azure'da DC/OS ile tümleştirmeyi öğrenmek için sonraki öğreticiye ilerleyin.  

> [!div class="nextstepaction"]
> [Uygulamalarda yük dengeleme gerçekleştirme](container-service-dcos-acr.md)
