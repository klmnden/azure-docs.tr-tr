---
title: "Azure DC/OS kümesi için dosya paylaşımı"
description: "Oluşturma ve Azure kapsayıcı hizmeti DC/OS kümesinde bir dosya paylaşımını bağlama"
services: container-service
author: julienstroheker
manager: dcaro
ms.service: container-service
ms.topic: tutorial
ms.date: 06/07/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: c1c318f4204efd24a2d9d3d83bb1cb71f5775bdb
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="create-and-mount-a-file-share-to-a-dcos-cluster"></a>Oluşturma ve DC/OS kümesi için bir dosya paylaşımını bağlama

Bu öğretici Azure'da bir dosya paylaşımı oluşturun ve her bir aracı ve DC/OS kümesi ana bağlamak nasıl ayrıntılarını verir. Bir dosya paylaşımı ayarlama, küme yapılandırması, erişim, günlükler ve daha fazla gibi içinde dosya paylaşmak üzere kolaylaştırır. Bu öğreticide aşağıdaki görevler tamamlanır:

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Dosya paylaşımı oluşturma
> * DC/OS kümesinde paylaşımını bağlama

Bu öğreticide adımları tamamlamak için bir ACS DC/OS kümesi gerekir. Gerekirse, [bu komut dosyası örneği](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) sizin için bir tane oluşturabilirsiniz.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a>Microsoft Azure üzerinde bir dosya paylaşımı oluşturma

Bir ACS DC/OS kümesi ile Azure dosya paylaşımının kullanmadan önce depolama hesabı ve dosya paylaşımı oluşturulması gerekir. Depolama ve dosya paylaşımı oluşturmak için aşağıdaki betiği çalıştırın. Parametreler, ortamınızdan thoes ile güncelleştirin.

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

## <a name="mount-the-share-in-your-cluster"></a>Kümenizdeki paylaşımını bağlama

Ardından, dosya paylaşımı, küme içindeki her sanal makineye bağlı gerekir. Bu görev CIFS aracı/protokolü kullanılarak tamamlanır. Bağlama işlemi, her bir düğümde kümenin veya kümedeki her düğümde karşı bir komut dosyası çalıştırarak el ile tamamlanabilir.

Bu örnekte, iki komut dosyaları çalıştırılır, bir Azure dosya paylaşımı ve DC/OS kümesi her düğümde bu komut dosyasını çalıştırmak için ikinci bir bağlama.

İlk olarak, Azure depolama hesabı adı ve erişim anahtarı gereklidir. Bu bilgileri almak için aşağıdaki komutları çalıştırın. Not her biri, bir sonraki adımda bu değerler kullanılır.

Depolama hesabı adı:

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group $DCOS_PERS_RESOURCE_GROUP --query "[?contains(name, '$DCOS_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCT
```

Depolama hesabının erişim anahtarı:

```azurecli-interactive
az storage account keys list --resource-group $DCOS_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

Ardından, DC/OS asıl FQDN'sini almak ve bir değişkende saklayın.

```azurecli-interactive
FQDN=$(az acs list --resource-group $DCOS_PERS_RESOURCE_GROUP --query "[0].masterProfile.fqdn" --output tsv)
```

Özel anahtarınızı ana düğüme kopyalayın. Bu anahtar oluşturmak için gereken bir ssh bağlantısı kümedeki tüm düğümlerle. Varsayılan olmayan bir değer kümesi oluştururken kullanılan kullanıcı adını güncelleştirin. 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

Bir SSH bağlantısı asıl (veya ilk ana) DC/OS tabanlı kümenizin ile oluşturun. Varsayılan olmayan bir değer kümesi oluştururken kullanılan kullanıcı adını güncelleştirin.

```azurecli-interactive
ssh azureuser@$FQDN
```

Adlı bir dosya oluşturun **cifsMount.sh**ve aşağıdaki içeriği buraya kopyalayın. 

Bu komut dosyası ve Azure dosya paylaşımı bağlamak için kullanılır. Güncelleştirme `STORAGE_ACCT_NAME` ve `ACCESS_KEY` bilgileri değişkenlerle toplanan daha önce.

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
Adlı ikinci bir dosya oluşturun **getNodesRunScript.sh** ve aşağıdaki içeriği dosyaya kopyalayın. 

Bu komut tüm küme düğümlerine bulur ve ardından çalıştırır **cifsMount.sh** her dosya paylaşımını bağlama için komut dosyası.

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

Azure dosya paylaşımı kümenin tüm düğümlerinde bağlamak için komut dosyasını çalıştırın.

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

Dosya Paylaşımı artık adresindeki erişilebilen `/mnt/share/dcosshare` kümedeki her düğümde.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure dosya paylaşımı adımları kullanarak bir DC/OS kümesi sunulmuştur:

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Dosya paylaşımı oluşturma
> * DC/OS kümesinde paylaşımını bağlama

Azure kapsayıcı kayıt defteri DC/OS Azure ile tümleştirme hakkında bilgi edinmek için sonraki öğretici ilerleyin.  

> [!div class="nextstepaction"]
> [Uygulamalarda yük dengeleme gerçekleştirme](container-service-dcos-acr.md)
