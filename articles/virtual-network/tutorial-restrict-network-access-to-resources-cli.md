---
title: PaaS kaynaklarına - Azure CLI ağ erişimi kısıtlama | Microsoft Docs
description: Bu makalede, sınırlayabilir ve Azure Storage ve Azure SQL veritabanı gibi Azure kaynakları için Azure CLI kullanarak sanal ağ hizmet uç noktaları ile ağ erişimini kısıtlayabilir öğrenin.
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
ms.openlocfilehash: f357861a7a44b249e06f091a8693b7f2d8dd5178
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-cli"></a>Azure CLI kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimi kısıtla

Sanal ağ hizmet uç noktaları bazı Azure hizmeti kaynaklar sanal ağ alt ağına ağ erişimini sınırlamak etkinleştirin. Internet kaynaklarına erişim de kaldırabilirsiniz. Hizmet uç noktaları desteklenen Azure Hizmetleri Azure hizmetlerine erişmek için sanal ağınızın özel adres alanı kullanmanıza olanak sağlayan, sanal ağ üzerinden doğrudan bağlantı sağlar. Hizmet uç noktaları üzerinden Azure kaynaklarına her zaman hedefleyen trafiğe, Microsoft Azure omurga ağı üzerinde kalır. Bu makalede, bilgi nasıl yapılır:

* Bir alt ağ ile bir sanal ağ oluşturma
* Bir alt ağ ekleyin ve bir hizmet uç noktası etkinleştirin
* Bir Azure kaynağı oluşturun ve ağ erişimi için yalnızca bir alt ağdan izin verin
* Her alt ağda bir sanal makine (VM) dağıtma
* Bir alt ağdan bir kaynağa erişimi onaylayın
* Bir alt ağ ve Internet bağlantısını bir kaynağa erişimi reddedildi onaylayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI Sürüm 2.0.28 çalıştırıyorsanız bu hızlı başlangıç yükleyip CLI yerel olarak kullanmak seçerseniz, gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Bir alt ağ ile birlikte bir sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktası etkinleştirme 

Hizmet uç noktalarına hizmet uç noktaları destekleyen hizmetler için etkinleştirebilirsiniz. Bir Azure konumdaki Hizmeti uç noktası etkin hizmetleri kullanılabilir görüntülemek [az ağ vnet listesi-endpoint-services](/cli/azure/network/vnet#az_network_vnet_list_endpoint_services). Aşağıdaki örnek hizmet uç noktası etkin olmayan kullanılabilir hizmetlerin listesini döndürür *eastus* bölge. Daha fazla Azure Hizmetleri Hizmeti uç noktası etkin hale geldikçe döndürülen hizmetlerin listesini zamanla büyüyecektir.

```azurecli-interactive
az network vnet list-endpoint-services \
  --location eastus \
  --out table
``` 

Ek bir alt ağ ile sanal ağ oluşturmak [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Bu örnekte, bir hizmet uç noktası için *Microsoft.Storage* için alt oluşturulur: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --service-endpoints Microsoft.Storage
```

## <a name="restrict-network-access-for-a-subnet"></a>Bir alt ağ için ağ erişimi kısıtlama

Bir ağ güvenlik grubu oluşturma [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNsgPrivate*.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Ağ güvenlik grubuyla ilişkilendirdiğiniz *özel* alt ağ ile [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). Aşağıdaki örnek ilişkilendirir *myNsgPrivate* ağ güvenlik grubuna *özel* alt ağ:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

Güvenlik kuralları oluşturma [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Aşağıdaki kural Azure Storage hizmetine atanan genel IP adreslerine giden erişim sağlar: 

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

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimi kısıtlama

Hizmet uç noktaları için etkin Azure Hizmetleri aracılığıyla oluşturulan kaynaklarına ağ erişimi kısıtlamak için gerekli adımları hizmetleri arasında değişir. Her hizmet için belirli adımlar için tek tek Hizmetleri belgelerine bakın. Bu makalenin sonraki bölümlerinde, örnek bir Azure Storage hesabı ağ erişimini kısıtlamak için adımları içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure depolama hesabıyla oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). Değiştir `<replace-with-your-unique-storage-account-name>` 3-24 karakter uzunluğunda, tüm Azure konumları arasında benzersiz bir ad yalnızca sayılar ve küçük harfler kullanarak.

```azurecli-interactive
storageAcctName="<replace-with-your-unique-storage-account-name>"

az storage account create \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Depolama hesabı oluşturulduktan sonra sahip bir değişken içine depolama hesabı için bağlantı dizesini almak [az depolama hesabı Göster bağlantı dizesi](/cli/azure/storage/account#az_storage_account_show_connection_string). Bağlantı dizesi, bir sonraki adımda bir dosya paylaşımı oluşturmak için kullanılır.

```azurecli-interactive
saConnectionString=$(az storage account show-connection-string \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>Değişkeni içeriğini görüntüleyebilir ve değeri Not **AccountKey** bir sonraki adımda kullanıldığından çıktıda döndürdü.

```azurecli-interactive
echo $saConnectionString
```

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında bir dosya paylaşımı oluşturma

İle depolama hesabındaki bir dosya paylaşımı oluşturmak [az depolama paylaşımı oluşturmak](/cli/azure/storage/share#az_storage_share_create). Bir sonraki adımda, ağ erişimi onaylamak için bu dosya paylaşımını takılı.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString > /dev/null
```

### <a name="deny-all-network-access-to-a-storage-account"></a>Bir depolama hesabı için tüm ağ erişimini engelle

Varsayılan olarak, depolama hesapları herhangi bir ağ istemcilerinden gelen ağ bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için varsayılan eylem değiştirme *reddetme* ile [az depolama hesabı güncelleştirme](/cli/azure/storage/account#az_storage_account_update). Ağ erişim reddedildi sonra depolama hesabı herhangi bir ağdan erişilebilir değil.

```azurecli-interactive
az storage account update \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdaki ağ erişimini etkinleştir

Depolama hesabından ağ erişim izni *özel* alt ağ ile [az depolama hesabı ağ-kuralı ekleyin](/cli/azure/storage/account/network-rule#az_storage_account_network_rule_add).

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName \
  --vnet-name myVirtualNetwork \
  --subnet Private
```
## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Ağ erişimi bir depolama hesabına sınamak için her alt ağ için bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makine oluşturma

Bir VM oluşturma *ortak* alt ağ ile [az vm oluşturma](/cli/azure/vm#az_vm_create). SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

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

Not edin **Publicıpaddress** döndürülen çıkışı. Bu adres, sonraki adımda Internet'ten VM erişmek için kullanılır.

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makine oluşturma

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürer. Oluşturulduktan sonra not edin **Publicıpaddress** döndürülen çıkışı. Bu adres, sonraki adımda Internet'ten VM erişmek için kullanılır.

## <a name="confirm-access-to-storage-account"></a>Depolama hesabı erişim Onayla

İçine SSH *myVmPrivate* VM. Değiştir *<publicIpAddress>* genel IP adresi ile *myVmPrivate* VM.

```bash 
ssh <publicIpAddress>
```

Bir bağlama noktası için bir klasör oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Oluşturduğunuz dizin Azure dosya paylaşımına bağlayın. Aşağıdaki komutu çalıştırmadan önce yerine `<storage-account-name>` hesap adıyla ve `<storage-account-key>` , alınan anahtarla [depolama hesabı oluşturma](#create-a-storage-account).

```bash
sudo mount --types cifs //<storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Aldığınız `user@myVmPrivate:~$` istemi. Azure dosya paylaşımı için başarıyla bağlandığını */mnt/MyAzureFileShare*.

VM diğer herhangi bir genel IP adreslerine giden bağlantıya sahip onaylayın:

```bash
ping bing.com -c 4
```

Ağ güvenlik grubu ile ilişkili olduğundan hiç yanıt alırsınız *özel* alt ağ Azure Storage hizmetine atanan adresler dışında genel IP adreslerine giden erişim izin vermez.

SSH oturumu çıkmak *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabı için erişim reddedildi onaylayın

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPublic* VM. Değiştir `<publicIpAddress>` genel IP adresi ile *myVmPublic* VM: 

```bash 
ssh <publicIpAddress>
```

Bir bağlama noktası için bir dizin oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Oluşturduğunuz dizine Azure dosya paylaşımını bağlama girişimi. Bu makale, en son sürümünü Ubuntu dağıtılan varsayar. Ubuntu'nın önceki sürümlerini kullanıyorsanız, bkz: [bağlama Linux'ta](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dosya paylaşımları oluşturma hakkında ek yönergeler için. Aşağıdaki komutu çalıştırmadan önce yerine `<storage-account-name>` hesap adıyla ve `<storage-account-key>` , alınan anahtarla [depolama hesabı oluşturma](#create-a-storage-account):

```bash
sudo mount --types cifs //storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Erişim reddedildi ve aldığınız bir `mount error(13): Permission denied` hatası, çünkü *myVmPublic* VM içinde dağıtıldığı *ortak* alt ağ. *Ortak* alt Azure Storage için etkin bir hizmet uç noktası yok ve depolama hesabı yalnızca gelen ağ erişimi sağlayan *özel* alt değil *ortak*alt ağ.

SSH oturumu çıkmak *myVmPublic* VM.

Bilgisayarınızdan ile depolama hesabınızdaki paylaşımları görüntüleme girişiminde [az depolama paylaşımı listesi](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Değiştir `<account-name>` ve `<account-key>` anahtarından ve depolama hesabı adı ile [depolama hesabı oluşturma](#create-a-storage-account):

```azurecli-interactive
az storage share list \
  --account-name <account-name> \
  --account-key <account-key>
```

Erişim reddedildi ve aldığınız bir *bu isteği bu işlemi gerçekleştirmek için yetkili değil* hatası, bilgisayarınızın içinde olmadığından *özel* alt *MyVirtualNetwork* sanal ağ.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ alt ağı için bir hizmet uç noktası etkin. Hizmet uç noktaları birden çok Azure services ile dağıtılan kaynaklar için etkinleştirilebilir öğrendiniz. Bir Azure Storage hesabını ve sınırlı ağ erişimi yalnızca bir sanal ağ alt ağ içindeki kaynaklar için depolama hesabı oluşturuldu. Hizmet uç noktaları hakkında daha fazla bilgi için bkz: [hizmet uç noktaları genel bakış](virtual-network-service-endpoints-overview.md) ve [alt ağlarını yönetin](virtual-network-manage-subnet.md).

Hesabınızı birden çok sanal ağlarınız varsa, her sanal ağ içindeki kaynaklara birbirleri ile iletişim kurabilmesi iki sanal ağları birbirine bağlamak isteyebilirsiniz. Bilgi edinmek için bkz [sanal ağlara bağlanabilir](tutorial-connect-virtual-networks-cli.md).