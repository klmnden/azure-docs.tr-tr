---
title: PaaS kaynaklarına - Azure CLI ağ erişimini kısıtlama | Microsoft Docs
description: Bu makalede, sınırlandırmak ve Azure CLI kullanarak sanal ağ hizmet uç noktaları ile Azure depolama ve Azure SQL veritabanı gibi Azure kaynaklarına ağ erişimini kısıtlama hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: ce4ff3fe2917d4dc34718fccc740223df0c52e8e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970949"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-cli"></a>Azure CLI kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimini kısıtlama

Sanal ağ hizmet uç noktaları bazı Azure hizmet uç noktalarına ağ erişimini bir sanal ağ alt ağı ile sınırlamanıza olanak tanır. Ayrıca, kaynaklara internet erişimini de kaldırabilirsiniz. Hizmet uç noktaları, sanal ağınızdan desteklenen Azure hizmetlerine doğrudan bağlantı sağlar, böylece Azure hizmetlerine erişmek için sanal ağınızın özel adres alanını kullanabilirsiniz. Hizmet uç noktaları aracılığıyla Azure kaynaklarına gönderilen trafik her zaman Microsoft Azure omurga ağı üzerinde kalır. Bu makalede şunları öğreneceksiniz:

* Alt ağ ile sanal ağ oluşturma
* Alt ağ ekleme ve hizmet uç noktasını etkinleştirme
* Azure kaynağı oluşturma ve yalnızca bir alt ağdan ağ erişimine izin verme
* Her alt ağa bir sanal makine (VM) dağıtma
* Bir alt ağdan kaynağa erişimi onaylama
* Bir alt ağdan ve internetten kaynağa erişimin reddedildiğini onaylama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynakları için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Bir alt ağ ile sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktasını girin 

Hizmet uç noktaları destekleyen hizmetler için hizmet uç noktaları etkinleştirebilirsiniz. Hizmet uç noktası etkin hizmetler kullanılabilir bir Azure konumu görünümünde [az network vnet liste-endpoint-services](/cli/azure/network/vnet#az_network_vnet_list_endpoint_services). Aşağıdaki örnek, hizmet uç noktası etkin kullanılabilir hizmetlerin listesini döndürür *eastus* bölge. Diğer Azure Hizmetleri etkin hizmet bitiş noktası oldukça döndürülen hizmetlerin listesi zamanla büyüyecektir.

```azurecli-interactive
az network vnet list-endpoint-services \
  --location eastus \
  --out table
``` 

Ek bir alt ağ ile sanal ağ oluşturma [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Bu örnekte, bir hizmet uç noktası için *Microsoft.Storage* alt ağ için oluşturulur: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --service-endpoints Microsoft.Storage
```

## <a name="restrict-network-access-for-a-subnet"></a>Bir kaynak için ağ erişimini kısıtlama

Bir ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur *myNsgPrivate*.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Ağ güvenlik grubuyla ilişkilendirdiğiniz *özel* alt ağ ile [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). Aşağıdaki örnek ilişkilendirir *myNsgPrivate* ağ güvenlik grubunu *özel* alt ağı:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

Güvenlik kuralları ile oluşturma [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Aşağıdaki kural, Azure depolama hizmetine atanmış genel IP adreslerine giden erişim sağlar: 

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-Storage-All \
  --access Allow \
  --protocol "*" \
  --direction Outbound \
  --priority 100 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Storage" \
  --destination-port-range "*"

Each network security group contains several [default security rules](security-overview.md#default-security-rules). The rule that follows overrides a default security rule that allows outbound access to all public IP addresses. The `destination-address-prefix "Internet"` option denies outbound access to all public IP addresses. The previous rule overrides this rule, due to its higher priority, which allows access to the public IP addresses of Azure Storage.

az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Deny-Internet-All \
  --access Deny \
  --protocol "*" \
  --direction Outbound \
  --priority 110 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"

The following rule allows SSH traffic inbound to the subnet from anywhere. The rule overrides a default security rule that denies all inbound traffic from the internet. SSH is allowed to the subnet so that connectivity can be tested in a later step.

az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 120 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "VirtualNetwork" \
  --destination-port-range "22"
```

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimini kısıtlama

Hizmet uç noktaları için etkinleştirilmiş Azure hizmetleri aracılığıyla oluşturulan kaynaklara ağ erişimini kısıtlamak için gereken adımlar, hizmetler arasında farklılık gösterir. Bir hizmete yönelik belirli adımlar için ilgili hizmetin belgelerine bakın. Bu makalenin geri kalanında örnek olarak bir Azure depolama hesabı için ağ erişimini kısıtlamaya yönelik adımlar içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure depolama hesabı oluşturun [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). Değiştirin `<replace-with-your-unique-storage-account-name>` 3-24 karakter uzunluğunda, tüm Azure konumlarında benzersiz olan bir ada sahip yalnızca sayı ve küçük harfler kullanarak.

```azurecli-interactive
storageAcctName="<replace-with-your-unique-storage-account-name>"

az storage account create \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Depolama hesabı oluşturulduktan sonra depolama hesabı için bağlantı dizesi sahip bir değişken içine alma [az depolama hesabı bağlantı-dizesini-Göster](/cli/azure/storage/account#az_storage_account_show_connection_string). Bağlantı dizesini, daha sonraki bir adımda bir dosya paylaşımı oluşturmak için kullanılır.

```azurecli-interactive
saConnectionString=$(az storage account show-connection-string \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>Değişken içeriğini görüntüleyebilir ve değerini not edin **AccountKey** daha sonraki bir adımda kullanıldığından çıktıda döndürdü.

```azurecli-interactive
echo $saConnectionString
```

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında dosya paylaşımı oluşturma

İle depolama hesabında dosya paylaşımı oluşturma [az depolama alanı paylaşımı oluşturma](/cli/azure/storage/share#az_storage_share_create). Daha sonraki bir adımda, ağ erişimi onaylamak için bu dosya paylaşımı bağlanmıştır.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString > /dev/null
```

### <a name="deny-all-network-access-to-a-storage-account"></a>Tüm bir depolama hesabına ağ erişimini engelle

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen ağ bağlantılarını kabul eder. Seçili ağlar erişimi sınırlamak için varsayılan eylem için değiştirme *Reddet* ile [az depolama hesabını güncelleştirme](/cli/azure/storage/account#az_storage_account_update). Ağ erişimi engellendi sonra depolama hesabı herhangi bir ağdan erişilebilir değil.

```azurecli-interactive
az storage account update \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdan ağ erişimini etkinleştirme

Depolama hesabından için ağ erişimine izin ver *özel* alt ağ ile [az depolama hesabı ağ kuralı ekleyin](/cli/azure/storage/account/network-rule#az_storage_account_network_rule_add).

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName \
  --vnet-name myVirtualNetwork \
  --subnet Private
```
## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir depolama hesabına ağ erişimini test etmek için her alt ağa bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makineyi oluşturma

Bir VM oluşturma *genel* alt ağ ile [az vm oluşturma](/cli/azure/vm#az_vm_create). SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI'yı bilgiler aşağıdaki örneğe benzer gösterir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPublic",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Not **Publicıpaddress** döndürülen çıktı. Bu adres, bir sonraki adımda internet'ten sanal Makineye erişmek için kullanılır.

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makineyi oluşturma

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. Oluşturulduktan sonra Not **Publicıpaddress** döndürülen çıktı. Bu adres, bir sonraki adımda internet'ten sanal Makineye erişmek için kullanılır.

## <a name="confirm-access-to-storage-account"></a>Depolama hesabına erişimi onaylama

İçine SSH *myVmPrivate* VM. Değiştirin *<publicIpAddress>* genel IP adresi ile *myVmPrivate* VM.

```bash 
ssh <publicIpAddress>
```

Bir bağlama noktası için bir klasör oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Azure dosya paylaşımını oluşturduğunuz dizine bağlayın. Aşağıdaki komutu çalıştırmadan önce değiştirin `<storage-account-name>` hesap adıyla ve `<storage-account-key>` , alınan anahtarla [depolama hesabı oluşturma](#create-a-storage-account).

```bash
sudo mount --types cifs //<storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Aldığınız `user@myVmPrivate:~$` istemi. Azure dosya paylaşımı başarıyla takılı */mnt/MyAzureFileShare*.

VM'ye diğer herhangi bir genel IP adreslerine giden bağlantısının olmadığını doğrulayın:

```bash
ping bing.com -c 4
```

*Özel* alt ağ ile ilişkili ağ güvenlik grubu Azure Depolama hizmetine atanan adreslerden başka genel IP adreslerine giden erişime izin vermediği için bir yanıt almazsınız.

İçin SSH oturumundan çıkın *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabına erişimin reddedildiğini onaylama

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPublic* VM. Değiştirin `<publicIpAddress>` genel IP adresi ile *myVmPublic* VM: 

```bash 
ssh <publicIpAddress>
```

Bir bağlama noktası için bir dizin oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Azure dosya paylaşımını oluşturduğunuz dizine bağlama girişimi. Bu makalede, en son sürümünü Ubuntu dağıttığınız varsayılır. Ubuntu önceki sürümleri kullanıyorsanız bkz [Linux üzerinde bağlama](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dosya paylaşımlarına bağlama hakkında ek yönergeler için. Aşağıdaki komutu çalıştırmadan önce değiştirin `<storage-account-name>` hesap adıyla ve `<storage-account-key>` , alınan anahtarla [depolama hesabı oluşturma](#create-a-storage-account):

```bash
sudo mount --types cifs //storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Erişim reddedilir ve aldığınız bir `mount error(13): Permission denied` hata, çünkü *myVmPublic* VM içinden dağıtıldığı *genel* alt ağ. *Genel* alt ağında Azure Depolama için etkinleştirilmiş bir hizmet uç noktası bulunmaz ve depolama hesabı *Genel* alt ağından değil, yalnızca *Özel* alt ağından ağ erişimine izin verir.

İçin SSH oturumundan çıkın *myVmPublic* VM.

Depolama hesabınızda paylaşımlarını görüntülemek, bilgisayarınızdan denemek [az storage share liste](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Değiştirin `<account-name>` ve `<account-key>` depolama hesabı adı ve anahtarı ile [depolama hesabı oluşturma](#create-a-storage-account):

```azurecli-interactive
az storage share list \
  --account-name <account-name> \
  --account-key <account-key>
```

Erişim reddedilir ve aldığınız bir *bu isteği bu işlemi gerçekleştirmek için yetkili değil* hata, bilgisayarınızı olmadığı *özel* alt *MyVirtualNetwork* sanal ağ.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil](/cli/azure#az_group_delete) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ alt ağı için hizmet uç noktası etkin. Hizmet uç noktalarının birden fazla Azure hizmeti ile dağıtılmış kaynaklar için etkinleştirilebildiğini öğrendiniz. Bir Azure Depolama hesabı oluşturdunuz ve depolama hesabına ağ erişimini yalnızca bir sanal ağ alt ağındaki kaynaklarla sınırladınız. Hizmet uç noktaları hakkında daha fazla bilgi için bkz. [Hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md) ve [Alt ağları yönetme](virtual-network-manage-subnet.md).

Hesabınızda birden fazla sanal ağ varsa, her bir sanal ağın içindeki kaynakların birbiriyle iletişim kurabilmesi iki sanal ağı birbirine bağlamak isteyebilirsiniz. Bilgi edinmek için bkz. nasıl [sanal ağları birbirine bağlama](tutorial-connect-virtual-networks-cli.md).