---
title: "Azure CLI 2.0 (Önizleme) kullanarak Linux VM oluşturma | Microsoft Azure"
description: "Azure CLI 2.0 (Önizleme) kullanarak Linux VM oluşturma."
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
ms.date: 09/26/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 2bd363e3c22f4cf4daf2e0fa352fd4a131d1675f
ms.openlocfilehash: 89db2c9f388b8a5496a306ba0a152ab57481ea50


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview"></a>Azure CLI 2.0 (Önizleme) kullanarak Linux VM oluşturma
Bu makalede Azure’da Azure CLI 2.0 (Önizleme) ile [az vm create](/cli/azure/vm#create) komutunu kullanarak bir Linux sanal makinesini (VM) hızlı bir şekilde nasıl dağıtacağınız gösterilmektedir. 

> [!NOTE] 
> Azure CLI 2.0 Önizleme, yeni nesil çok platformlu CLI uygulamasıdır. Deneyin ve düşüncelerinizi bize [GitHub proje sayfasından](https://github.com/Azure/azure-cli) iletin.
>
> Diğer belgelerimizde var olan Azure CLI sürümü anlatılmaktadır. CLI 2.0 Önizleme sürümünü değil de var olan Azure CLI uygulamasını kullanarak VM oluşturmak için bkz. [Azure CLI ile VM oluşturma](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

VM oluşturmak için aşağıdakilere ihtiyacını vardır: 

* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 2.0 (Önizleme)](https://github.com/Azure/azure-cli#installation) sürümünü yükleyin
* Azure hesabınızda oturum açın ([az login](/cli/azure/#login) yazın)

([Azure portalını](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanarak da hızlıca bir Linux VM dağıtabilirsiniz.)

Aşağıdaki örnekte, bir Debian VM'sinin nasıl dağıtılacağı ve Secure Shell (SSH) anahtarının nasıl ekleneceği gösterilmektedir (bağımsız değişkenleriniz farklı olabilir; farklı bir görüntü kullanmak istiyorsanız [arama yapabilirsiniz](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

İlk olarak dağıtılan tüm kaynakları içeren kaynak grubunuzu oluşturmak için [az resource group create](/cli/azure/resource/group#create) yazın:

```azurecli
az resource group create -n myResourceGroup -l westus
```

Çıktı aşağıdakine benzer olacaktır (dilerseniz farklı bir `--output` seçeneği belirleyebilirsiniz):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-your-vm-using-the-latest-debian-image"></a>En son Debian görüntüsünü kullanarak VM oluşturma

Artık VM’nizi ve ortamını oluşturabilirsiniz. `----public-ip-address-dns-name` değerinin yerine benzersiz bir değer yazmayı unutmayın. Aşağıda gösterilen değer başkası tarafından alınmış olabilir.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username ops \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name mydns \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Çıktı aşağıdakine benzer olacaktır. `publicIpAddress` veya `fqdn` değerini, VM’nize **ssh** bağlantısı kurarken kullanmak için not edin.


```json
{
  "fqdn": "mydns.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-05-07",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.112.217.29",
  "resourceGroup": "myResourceGroup"
}
```

Çıktıda listelenen genel IP adresini kullanarak VM'inizde oturum açın. Listelenen tam etki alanı adını (FQDN) da kullanabilirsiniz.

```bash
ssh ops@mydns.westus.cloudapp.azure.com
```

Seçtiğiniz dağıtıma bağlı olarak aşağıdakine benzer bir çıktıyla karşılaşırsınız:

```
The authenticity of host 'mydns.westus.cloudapp.azure.com (40.112.217.29)' can't be established.
RSA key fingerprint is SHA256:xbVC//lciRvKild64lvup2qIRimr/GB8C43j0tSHWnY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mydns.westus.cloudapp.azure.com,40.112.217.29' (RSA) to the list of known hosts.

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@mynewvm:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Sonraki adımlar
`az vm create` komutu, bir Bash kabuğunda oturum açıp çalışmaya başlamanız için VM dağıtmanın hızlı bir yoludur. Ancak `az vm create` kullanarak kapsamlı bir denetim gerçekleştiremez ve daha karmaşık bir ortam oluşturamazsınız.  Altyapınız için özelleştirilmiş bir Linux VM'si dağıtmak üzere şu makalelerden herhangi birine bakın:

* [Belirli bir dağıtımı oluşturmak için Azure Resource Manager şablonu kullanma](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI'si komutlarını doğrudan kullanarak bir Linux VM'si için kendi özel ortamınızı oluşturun](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da SSH Secure ile Güvenliği Sağlanmış Linux VM'si oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Docker ana bilgisayarı olarak hızlı şekilde bir Linux VM'si oluşturmak [için `docker-machine` Azure sürücüsünü farklı komutlarla kullanabilirsiniz.](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Java kullanıyorsanız [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine) yöntemini deneyebilirsiniz.




<!--HONumber=Nov16_HO4-->


