---
title: "Azure CLI 2.0 kullanarak bir Linux VM oluşturma | Microsoft Azure"
description: "Azure CLI 2.0 kullanarak bir Linux VM oluşturma."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
ms.assetid: 82005a05-053d-4f52-b0c2-9ae2e51f7a7e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/13/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 892e3c62a2ad4dc4fd0691874d46bb296e379524
ms.openlocfilehash: cc51b04c31c02aabf25c9efb1e9cd975077811a4
ms.lasthandoff: 02/27/2017


---

# <a name="create-a-linux-vm-using-the-azure-cli-20"></a>Azure CLI 2.0 kullanarak bir Linux VM oluşturma
Bu makalede, Azure'da hem yönetilen diskleri hem de yerel depolama hesabındaki diskleri kullanarak Azure CLI 2.0 ve [az vm create](/cli/azure/vm#create) komutları ile bir Linux sanal makinesini (VM) hızlı bir şekilde nasıl dağıtacağınız gösterilmektedir. Bu adımları [Azure CLI 1.0](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz.

VM oluşturmak için aşağıdakilere ihtiyacını vardır: 

* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0](/cli/azure/install-az-cli2) uygulaması
* Azure hesabınızda oturum açın ([az login](/cli/azure/#login) yazın)

([Azure portalını](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanarak da Linux VM dağıtabilirsiniz.)

Aşağıdaki örnekte bir Debian VM'nin dağıtımı ve Güvenli Kabuk (SSH) anahtarı kullanılarak bu VM’ye bağlanma işlemi gösterilmektedir. Bağımsız değişkenleriniz farklı olabilir; farklı bir görüntü istiyorsanız [bir tane arayabilirsiniz](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="using-managed-disks"></a>Yönetilen diskleri kullanma

Azure yönetilen diskleri kullanmak için onu destekleyen bir bölge kullanmanız gerekir. İlk olarak, dağıtılan tüm kaynakları içeren kaynak grubunuzu oluşturmak için [az group create](/cli/azure/group#create) yazın:

```azurecli
 az group create -n myResourceGroup -l westus
```

Çıktı aşağıdaki gibi görünür (farklı bir biçim görmek isterseniz farklı bir `--output` seçeneği belirtebilirsiniz):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```
### <a name="create-your-vm"></a>VM oluşturma 
Artık VM’nizi ve ortamını oluşturabilirsiniz. `--public-ip-address-dns-name` değerinin yerine benzersiz bir değer yazmayı unutmayın. Aşağıda gösterilen değer başkası tarafından alınmış olabilir.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Çıktı aşağıdakine benzer olacaktır. `publicIpAddress` veya `fqdn` değerini, VM’nize **ssh** bağlantısı kurarken kullanmak için not edin.


```json
{
  "fqdn": "manageddisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-E9-41",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.42.127.53",
  "resourceGroup": "myResourceGroup"
}
```

Genel IP adresini veya çıktıda listelenen tam etki alanı adını (FQDN) kullanarak sanal makinenizde oturum açın.

```bash
ssh ops@manageddisks.westus.cloudapp.azure.com
```

Seçtiğiniz dağıtıma bağlı olarak aşağıdakine benzer bir çıktıyla karşılaşırsınız:

```bash
The authenticity of host 'manageddisks.westus.cloudapp.azure.com (134.42.127.53)' can't be established.
RSA key fingerprint is c9:93:f5:21:9e:33:78:d0:15:5c:b2:1a:23:fa:85:ba.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'manageddisks.westus.cloudapp.azure.com' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jan 13 14:44:21 2017 from net-37-117-240-123.cust.vodafonedsl.it
ops@myVM:~$ 
```

Yönetilen diskleri kullanarak yeni sanal makinenizle yapabileceğiniz diğer işlemler için bkz. [Sonraki Adımlar](#next-steps).

## <a name="using-unmanaged-disks"></a>Yönetilmeyen diskleri kullanma 

Yönetilmeyen depolama diskleri kullanan sanal makinelerin yönetilmeyen depolama hesapları vardır. İlk olarak, dağıtılan tüm kaynakları içerecek kaynak grubunuzu oluşturmak için [az group create](/cli/azure/group#create) yazın:

```azurecli
az group create --name nativedisks --location westus
```

Çıktı aşağıdakine benzer olacaktır (dilerseniz farklı bir `--output` seçeneği belirleyebilirsiniz):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks",
  "location": "westus",
  "managedBy": null,
  "name": "nativedisks",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

### <a name="create-your-vm"></a>VM oluşturma 

Artık VM’nizi ve ortamını oluşturabilirsiniz. Sanal makineyi yönetilmeyen disklerle oluşturmak için `--use-unmanaged-disk` bayrağını kullanın. Yönetilmeyen bir depolama hesabı da oluşturulur. `--public-ip-address-dns-name` değerinin yerine benzersiz bir değer yazmayı unutmayın. Aşağıda gösterilen değer başkası tarafından alınmış olabilir.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name nativedisks \
--resource-group nativedisks \
--location westus \
--name myVM \
--use-unmanaged-disk
```

Çıktı aşağıdakine benzer olacaktır. `publicIpAddress` veya `fqdn` değerini, VM’nize **ssh** bağlantısı kurarken kullanmak için not edin.

```json
{
  "fqdn": "nativedisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-33-24-3C",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.91.91.195",
  "resourceGroup": "nativedisks"
}
```

Çıktıda listelenen genel IP adresini veya tam etki alanı adını (FQDN) kullanarak sanal makinenizde oturum açın.

```bash
ssh ops@nativedisks.westus.cloudapp.azure.com
```

Seçtiğiniz dağıtıma bağlı olarak aşağıdakine benzer bir çıktıyla karşılaşırsınız:

```
The authenticity of host 'nativedisks.westus.cloudapp.azure.com (13.91.93.195)' can't be established.
RSA key fingerprint is 3f:65:22:b9:07:c9:ef:7f:8c:1b:be:65:1e:86:94:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'nativedisks.westus.cloudapp.azure.com,13.91.93.195' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@myVM:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Sonraki adımlar
`az vm create` komutu, bir Bash kabuğunda oturum açıp çalışmaya başlamanız için VM dağıtmanın hızlı bir yoludur. Ancak `az vm create` kullanarak kapsamlı bir denetim gerçekleştiremez ve daha karmaşık bir ortam oluşturamazsınız.  Altyapınız için özelleştirilmiş bir Linux VM'si dağıtmak üzere şu makalelerden herhangi birine bakın:

* [Belirli bir dağıtımı oluşturmak için Azure Resource Manager şablonu kullanma](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI'si komutlarını doğrudan kullanarak bir Linux VM'si için kendi özel ortamınızı oluşturun](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da SSH Secure ile Güvenliği Sağlanmış Linux VM'si oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Docker ana bilgisayarı olarak hızlı şekilde bir Linux VM'si oluşturmak [için `docker-machine` Azure sürücüsünü farklı komutlarla kullanabilirsiniz.](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Java kullanıyorsanız [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine) yöntemini deneyebilirsiniz.


