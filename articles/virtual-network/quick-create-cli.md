---
title: "Azure CLI - sanal ağ oluşturma | Microsoft Docs"
description: "Hızlı bir şekilde Azure CLI kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ türlerde özel olarak birbirleri ile iletişim kurmak için Azure kaynaklarını sağlar."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ff27d557f221be61a7384f6aaf6e57cef5cebb81
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Azure CLI kullanarak bir sanal ağ oluşturma

Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki sanal makine özel ağ iletişimi aralarında sınamak için sanal ağda dağıtın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Tüm Azure kaynakları bir Azure konumu (veya bölge) içinde oluşturulur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ ile oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create) komutu. Aşağıdaki örnek adlı bir varsayılan sanal ağ oluşturur *myVirtualNetwork* bir alt ağ ile *varsayılan*. Azure sanal ağı bir konum belirlenmezse olduğundan, kaynak grubu ile aynı konumda oluşturur.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

Sanal ağ oluşturulduktan sonra döndürülen bilgilerin bir bölümünü aşağıdaki gibidir.

```azurecli
"newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/16"
```

Tüm sanal ağları atanmış bir veya daha fazla adres öneklerini vardır. Sanal ağ oluştururken bir adres öneki belirtilmedi olduğundan, Azure 10.0.0.0/16 adres alanı, varsayılan olarak tanımlı. Adres alanı CIDR gösteriminde belirtilir. Adres alanı 10.0.0.0/16 10.0.0.0-10.0.255.254 kapsar.

Bilgileri başka bir kısmını döndürülen **addressPrefix** , *10.0.0.0/24* için *varsayılan* komutunda belirtilen alt ağ. Bir sanal ağ sıfır veya daha fazla alt ağlar içeriyor. Komutu oluşturulan adlı tek bir alt ağ *varsayılan*, ancak için alt ağ adresi önek belirtilmedi. Bir sanal ağ veya alt ağ için bir adres öneki belirtilmediğinde, Azure 10.0.0.0/24 varsayılan olarak ilk alt ağ için adres öneki tanımlar. Sonuç olarak, alt ağ 10.0.0.0-10.0.0.254 kapsayan ancak Azure ilk dört adresler (0-3), her alt ağda son adresi ayırdığından yalnızca 10.0.0.4-10.0.0.254 kullanılabilir.

## <a name="test-network-communication"></a>Test ağ iletişimi

Bir sanal ağ çeşitli özel olarak birbirleri ile iletişim kurmak için Azure kaynaklarını sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir. Sonraki adımda aralarında özel iletişim doğrulamak için iki sanal makineye sanal ağ oluşturun.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

[az vm create](/cli/azure/vm#az_vm_create) komutuyla bir sanal makine oluşturun. Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVm1*. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` Seçeneği bir sonraki adıma devam etmek için bu sanal makine arka planda oluşturur.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

Azure sanal makine otomatik olarak oluşturduğu *varsayılan* alt *myVirtualNetwork* sanal ağ, kaynak grubu ve sanal ağ sanal ağı var olduğundan veya alt ağ komutunda belirtilmedi. Azure DHCP otomatik olarak atanmış 10.0.0.4 sanal makine oluşturma sırasında listedeki ilk kullanılabilir adres olduğundan *varsayılan* alt ağ. Bir sanal makine oluşturulur konumu sanal ağ içinde aynı konumda olmalıdır. Bu makalede olmasına rağmen sanal makinenin sanal ağ ile aynı kaynak grubunda olması gerekli değildir.

İkinci bir sanal makine oluşturun. Varsayılan olarak, Azure da bu sanal makine oluşturur *varsayılan* alt ağ.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Sanal makine oluşturmak için birkaç dakika sürer. Sanal makine oluşturulduktan sonra Azure CLI çıktı aşağıdaki örneğe benzer şekilde döndürür: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

Örnekte, gördüğünüz **privateIpAddress** olan *10.0.0.5*. Otomatik olarak atanan azure DHCP *10.0.0.5* sanal makineye sonraki kullanılabilir adresi olduğundan *varsayılan* alt ağ. Not edin **Publicıpaddress**. Bu adres sanal makine bir sonraki adımda Internet'ten erişmek için kullanılır. Genel IP adresi alanından sanal ağ veya alt ağ adres öneklerini içinde atanmadı. Gelen atanmış ortak IP adresleri bir [her bir Azure bölgesine atanan adresler havuzuna](https://www.microsoft.com/download/details.aspx?id=41653). Hangi ortak IP adresi bir sanal makineye atanan Azure bilir olsa da, bir sanal makinede çalışan işletim sistemi atanmış tüm ortak IP adresinin hiçbir şeyin sahiptir.

### <a name="connect-to-a-virtual-machine"></a>Bir sanal makineye bağlanma

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVm2* sanal makine. Değiştir `<publicIpAddress>` sanal makinenizin ortak IP adresine sahip. Yukarıdaki örnekte, IP adresidir *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

### <a name="validate-communication"></a>İletişim doğrula

İle iletişim onaylamak için aşağıdaki komutu kullanın *myVm1* gelen *myVm2*:

```bash
ping myVm1 -c 4
```

Kaynağından gelen dört yanıt aldığınız *10.0.0.4*. İle iletişim kurabilir *myVm1* gelen *myVm2*, her iki sanal makine gelen atanan özel IP adresleri olduğundan *varsayılan* alt ağ. Azure DNS ad çözümlemesi otomatik olarak bir sanal ağ içindeki tüm ana bilgisayarlar için sağladığı için ana bilgisayar adı ile ping işlemi yapmak kullanabilirsiniz.

İnternet giden iletişim onaylamak için aşağıdaki komutu kullanın:

```bash
ping bing.com -c 4
```

Aratıp dört yanıt alırsınız. Varsayılan olarak, herhangi bir sanal makine bir sanal ağdaki internet giden iletişim kurabilir.

SSH oturumu VM'nize çıkın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için komutu:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ bir alt ağ ile dağıtılabilir. Birden çok alt ağ ile özel bir sanal ağ oluşturmayı öğrenmek için özel bir sanal ağ oluşturmak için öğretici devam edin.

> [!div class="nextstepaction"]
> [Özel bir sanal ağ oluşturma](virtual-networks-create-vnet-arm-cli.md)
