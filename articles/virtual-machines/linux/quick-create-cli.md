---
title: Hızlı Başlangıç - Azure CLI ile Linux sanal makinesi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure CLI’yi kullanarak Linux sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 87a36e027515319c4bdfeaa559f55fd6e5a1c75b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958535"
---
# <a name="quickstart-create-a-linux-virtual-machine-with-the-azure-cli"></a>Hızlı Başlangıç: Azure CLI ile Linux sanal makinesi oluşturma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure CLI kullanarak Azure’da Ubuntu çalıştıran bir Linux sanal makinesinin (VM) nasıl dağıtılacağı gösterilir. VM’ye SSH oluşturup NGINX web sunucusunu yükleyerek VM’nizin çalıştığını görebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#az_vm_create) komutuyla bir sanal makine oluşturun.

Aşağıdaki örnek, *myVM* adlı bir VM oluşturur, *azureuser* adlı bir kullanıcı hesabı ekler ve varsayılan anahtar konumunda (*~/.ssh*) mevcut değilse SSH anahtarlarını oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnekte VM oluşturma işleminin başarılı olduğu gösterilmektedir.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

VM’nizdeki çıktıda `publicIpAddress` değerini not alın. Sonraki adımlarda bu adres, VM’ye erişmek için kullanılır.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Azure üzerinde bir Linux VM oluşturduğunuzda varsayılan olarak, yalnızca SSH bağlantıları açılır. NGINX web sunucusuyla kullanacağınız TCP bağlantı noktası 80’i açmak için [az vm open-port](/cli/azure/vm#az_vm_open_port) komutunu kullanın:

```azurecli-interactive
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Normal şekilde VM’nize SSH’leyin. **publicIpAddress** değerini, VM’nizdeki bir önceki çıktıda belirtildiği şekilde sanal makinenizin genel IP adresiyle değiştirin:

```bash
ssh azureuser@publicIpAddress
```

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için NGINX web sunucusunu yükleyin. Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için SSH oturumunuzdan aşağıdaki komutları çalıştırın:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

İşiniz bittiğinde SSH oturumundan çıkın (`exit`).

## <a name="view-the-web-server-in-action"></a>Web sunucusunu iş başında görün

NGINX yüklendiğine ve VM’nizde İnternet üzerinden 80 numaralı bağlantı noktası açık olduğuna göre, varsayılan NGINX karşılama sayfasını görüntülemek için, seçtiğiniz bir web tarayıcısını kullanabilirsiniz. VM’nizin önceki bir adımda edinilen genel IP adresini kullanın. Aşağıdaki örnekte varsayılan NGINX web sitesi gösterilir:

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz. SSH oturumundan sanal makinenize çıkış yaptığınızdan emin olun, ardından kaynakları aşağıda belirtildiği gibi silin:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, basit bir sanal makine dağıttınız, web trafiği için bir ağ bağlantı noktası açtınız ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.


> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
