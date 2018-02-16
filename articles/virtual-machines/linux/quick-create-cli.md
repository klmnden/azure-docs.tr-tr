---
title: "Azure Hızlı Başlangıç - VM CLI oluşturma | Microsoft Docs"
description: "Azure CLI ile sanal makine oluşturmayı hızlı bir şekilde öğrenin."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/13/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 767b378331d994bb48a83726b1ebbfcb99649d12
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>Azure CLI ile Linux sanal makinesi oluşturma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Ubuntu sunucusu çalıştıran bir sanal makineyi Azure CLI kullanarak dağıtma işleminin ayrıntıları verilmektedir. Sunucu dağıtıldıktan sonra bir SSH bağlantısı oluşturulur ve bir NGINX web sunucusu yüklenir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#az_vm_create) komutuyla bir sanal makine oluşturun. 

Aşağıdaki örnekte *myVM* adlı bir VM oluşturulur ve varsayılan anahtar konumunda henüz yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

VM oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir. `publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

Varsayılan olarak, Azure’a dağıtılmış Linux sanal makinelerinde yalnızca SSH bağlantılarına izin verilir. Bu VM bir web sunucusu olacaksa, İnternet’ten 80 numaralı bağlantı noktasını açmanız gerekir. İstediğiniz bağlantı noktasını açmak için [az vm open-port](/cli/azure/vm#open-port) komutunu kullanın.  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>VM’ye SSH uygulama

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. **publicIpAddress** değerini, sanal makinenizin doğru genel IP adresi ile değiştirdiğinizden emin olun.  Yukarıdaki örneğimizde IP adresi *40.68.254.142* şeklindedir.

```bash 
ssh publicIpAddress
```

## <a name="install-nginx"></a>NGINX yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki komutları kullanın. 

```bash 
# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX yüklendiğine ve VM’nizde İnternet üzerinden 80 numaralı bağlantı noktası açık olduğuna göre, varsayılan NGINX karşılama sayfasını görüntülemek için, seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için yukarıda belgelediğiniz *publicIpAddress* değerini kullandığınızdan emin olun. 

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz. SSH oturumundan sanal makinenize çıkış yapın, ardından kaynakları aşağıda belirtildiği gibi silin:

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine ve bir ağ güvenlik grubu kuralı dağıtıp, bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.


> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
