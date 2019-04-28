---
title: Klasik Azure CLI kullanarak bir Linux VM oluşturma | Microsoft Docs
description: Klasik Azure CLI'yi kullanarak Azure'da bir Linux VM oluşturma
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: jeconnoc
editor: ''
ms.assetid: facb1115-2b4e-4ef3-9905-330e42beb686
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2016
ms.author: v-livech
ms.openlocfilehash: 569e90c7908ce435689a80f7917b20275703f537
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473748"
---
# <a name="create-a-linux-vm-using-the-azure-classic-cli"></a>Klasik Azure CLI kullanarak bir Linux VM oluşturma

Bu makalede, Azure komut satırı arabiriminde (CLI) `azure vm quick-create` komutunu kullanarak Azure'da bir Linux sanal makinesini (VM) hızlı bir şekilde nasıl dağıtacağınız gösterilmektedir. `quick-create` komutu, bir kavramı hızlıca prototipleştirmek veya sınamak için kullanabileceğiniz temel, güvenli bir altyapı içine VM dağıtır.

> [!NOTE]
> Azure CLI kullanarak VM oluşturmak için bkz: [Azure CLI ile VM oluşturma](../windows/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[Azure portalını](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanarak da hızlıca bir Linux VM'si dağıtabilirsiniz.

Makale bir [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="quick-commands"></a>Hızlı komutlar

Aşağıdaki örnekte, bir CoreOS VM'sinin nasıl dağıtılacağı ve Secure Shell (SSH) anahtarının nasıl ekleneceği gösterilmektedir. (Bağımsız değişkenleriniz farklı olabilir):

```azurecli
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Aşağıdaki kılavuzda bir UbuntuLTS sanal makinesinin dağıtımı, her adımın sonuçlarına ilişkin açıklamalarla birlikte adım adım gösterilmektedir.

## <a name="vm-quick-create-aliases"></a>VM hızlı oluşturma diğer adları

En yaygın işletim sistemi dağıtımlarıyla eşlenmiş Azure CLI'si diğer adlarını kullanmak, dağıtım seçmenin hızlı bir yoludur. Aşağıdaki tabloda diğer adlar listelenmektedir (Azure CLI'sinin 0.10 sürümünden itibaren). `quick-create` kullanan tüm dağıtımlar, daha hızlı sağlama ve daha yüksek performanslı disk erişimi sunan katı hal sürücüsü (SSD) tarafından desteklenen VM'leri varsayılan olarak kullanır. (Bu diğer adlar, Azure'da bulunan dağıtımların çok küçük bir bölümünü temsil eder. [PowerShell’de](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ya da [web üzerinde](https://azure.microsoft.com/marketplace/virtual-machines/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) görüntü arayarak Azure Marketi'nde daha fazla görüntü bulun veya [kendi özel görüntünüzü karşıya yükleyin](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)

| Diğer ad | Yayımcı | Sunduğu | SKU | Version |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |CentOS |7.2 |en son |
| CoreOS |CoreOS |CoreOS |Dengeli |en son |
| Debian |credativ |Debian |8 |en son |
| openSUSE |SUSE |openSUSE |13.2 |en son |
| RHEL |Red Hat |RHEL |7.2 |en son |
| UbuntuLTS |Canonical |Ubuntu Server |14.04.4-LTS |en son |

Aşağıdaki bölümlerde, Ubuntu 14.04.4 LTS Server'ı dağıtmak üzere **ImageURN** seçeneği (`-Q`) için `UbuntuLTS` diğer adı kullanılmaktadır.

Önceki `quick-create` örneğinde, SSH parolaları devre dışı bırakılarak karşıya yüklenecek SSH ortak anahtarını tanımlamak üzere yalnızca `-M` bayrağı çağrılmıştır, bu nedenle sizden aşağıdaki bağımsız değişkenler istenir:

* kaynak grup adı (ilk Azure kaynak grubunuz için genellikle herhangi bir dize olabilir)
* VM adı
* konum (`westus` veya `westeurope` iyi varsayılanlardır)
* linux (Azure'ın hangi işletim sistemini istediğinizi bilmesi için)
* kullanıcı adı

Daha fazla istemin gerekli olmaması için aşağıdaki örnekte tüm değerler belirtilmiştir. ssh-rsa biçiminde ortak anahtar dosyası olarak `~/.ssh/id_rsa.pub` dosyanız olduğu sürece bu dosyayı kullanabilirsiniz:

```azurecli
azure vm quick-create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --admin-username myAdminUser \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --image-urn UbuntuLTS
```

Çıktı aşağıdaki çıktı bloğu gibi görünmelidir:

```azurecli
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Yeni sanal makinede oturum açma
Çıktıda listelenen genel IP adresini kullanarak VM'inizde oturum açın. Listelenen tam etki alanı adını (FQDN) da kullanabilirsiniz:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Oturum açma işlemi aşağıdaki çıktı bloğuna benzer şekilde görünmelidir:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    https://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Sonraki adımlar
`azure vm quick-create` komutu, bir Bash kabuğunda oturum açıp çalışmaya başlamanız için VM dağıtmanın hızlı bir yoludur. Ancak `vm quick-create` kullanarak kapsamlı bir denetim gerçekleştiremez ve daha karmaşık bir ortam oluşturamazsınız.  Altyapınız için özelleştirilmiş bir Linux VM'si dağıtmak üzere şu makalelerden herhangi birine bakın:

* [Azure CLI'si komutlarını doğrudan kullanarak bir Linux VM'si için kendi özel ortamınızı oluşturun](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da SSH Secure ile Güvenliği Sağlanmış Linux VM'si oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Docker konağı olarak hızlı şekilde bir Linux VM'si oluşturmak için çeşitli komutlara sahip `docker-machine` Azure sürücüsünü](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) de kullanabilirsiniz.
