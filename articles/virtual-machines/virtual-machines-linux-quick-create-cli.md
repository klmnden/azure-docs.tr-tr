<properties
   pageTitle="CLI kullanarak Azure’da bir Linux VM oluşturma | Microsoft Azure"
   description="CLI kullanarak Azure’da bir Linux VM oluşturun."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="05/03/2016"
   ms.author="v-livech"/>


# CLI kullanarak Azure’da bir Linux VM oluşturma.

Bu makalede, Azure’da Azure CLI `azure vm quick-create` komutu kullanarak hızlı bir şekilde Linux Sanal Makine dağıtma gösterilir. `quick-create` komutu, kendisini çevreleyen, bir kavramı çok hızlı bir şekilde prototip kullanabilmek veya test edebilmek için kullanabileceğiniz, temel altyapıyla bir VM dağıtır (bunu Linux bash kabuğun en hızlı yolu olarak düşünebilirsiniz).  Makale bir Azure hesabı ([ücretsiz bir deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/)) ve oturum açılmış (`azure login`) ve kaynak yöneticisi modunda (`azure config mode arm`) [Azure CLI](../xplat-cli-install.md) ve gerektirir.  Ayrıca bir [Azure Portal](virtual-machines-linux-quick-create-portal.md) kullanarak bir Linux VM dağıtabilirsiniz.

## Hızlı Komut Özeti

CoreOS VM dağıtmak ve SSH anahtarınızı eklemek için bir komut:

```bash
azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
```

## Linux VM’i dağıtma

Yukarıdaki aynı komutu kullanarak, aşağıda görmeyi beklemeniz gereken çıktıyla birlikte her istem gösterilmektedir, ancak RedHat Enteprise Linux 7.2 VM oluşturmak için RHEL diğer adını kulllanır.  

## ImageURN diğer adını kullanma

Azure CLI `quick-create` komutu en yaygın işletim sistemi dağıtımlarına eşlenmiş diğer adlara sahiptir. Aşağıdaki tabloda dağıtım diğer adları listelenmektedir (Azure CLI Sürüm 0.10 itibariyle).  `quick-create` kullanan tüm dağıtımlar, yüksek performanslı bir deneyim sunarak VM’ler yedekli SSD depolamayı varsayılan değer kılar.

| Diğer ad     | Yayımcı | Sunduğu        | SKU         | Sürüm |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | en son  |
| CoreOS    | CoreOS    | CoreOS       | Dengeli      | en son  |
| Debian    | credativ  | Debian       | 8           | en son  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | en son  |
| RHEL      | RedHat    | RHEL         | 7.2         | en son  |
| SLES      | SLES      | SLES         | 12 SP1      | en son  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | en son  |



**ImageURN** seçeneği (`-Q`) için, RedHat Enterprise Linux 7.2 VM’i dağıtmak için `RHEL` kullanacağız. (Bu 7 diğer ad Azure’da kullanılabilen işletim sistemlerinin küçük bir kısmını temsil eder; [bir görüntü arayarak](virtual-machines-linux-cli-ps-findimage.md) markette diğer görüntüleri bulun veya [kendi özel görüntünüzü de yükleyebilirsiniz](virtual-machines-linux-create-upload-generic.md).)

Aşağıdaki komut kılavuzunda, lütfen komut istemlerini kendi ortamınızdaki değerlerle değiştirin, biz “örnek” değerler kullanıyoruz.  

Komut istemlerini izleyin ve kendi adlarınızı girin

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q RHEL
```

Çıktı aşağıdaki çıktı bloğu gibi görünmelidir.

```bash
info:    Executing command vm quick-create
Resource group name: rhel-quick
Virtual machine name: rhel
Location name: westus
Operating system Type [Windows, Linux]: linux
User name: ops
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "rhel"
info:    Verifying the public key SSH file: /Users/ops/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli1630678171193501687
info:    Could not find the storage account "cli1630678171193501687", trying to create new one
+ Creating storage account "cli1630678171193501687" in "westus"
+ Looking up the storage account cli1630678171193501687
+ Looking up the NIC "rhel-westu-1630678171-nic"
info:    An nic with given name "rhel-westu-1630678171-nic" not found, creating a new one
+ Looking up the virtual network "rhel-westu-1630678171-vnet"
info:    Preparing to create new virtual network and subnet
+ Creating a new virtual network "rhel-westu-1630678171-vnet" [address prefix: "10.0.0.0/16"] with subnet "rhel-westu-1630678171-snet" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "rhel-westu-1630678171-vnet"
+ Looking up the subnet "rhel-westu-1630678171-snet" under the virtual network "rhel-westu-1630678171-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "rhel-westu-1630678171-pip"
info:    PublicIP with given name "rhel-westu-1630678171-pip" not found, creating a new one
+ Creating public ip "rhel-westu-1630678171-pip"
+ Looking up the public ip "rhel-westu-1630678171-pip"
+ Creating NIC "rhel-westu-1630678171-nic"
+ Looking up the NIC "rhel-westu-1630678171-nic"
+ Looking up the storage account clisto909893658rhel
+ Creating VM "rhel"
+ Looking up the VM "rhel"
+ Looking up the NIC "rhel-westu-1630678171-nic"
+ Looking up the public ip "rhel-westu-1630678171-pip"
data:    Id                              :/subscriptions/<guid>/resourceGroups/rhel-quick/providers/Microsoft.Compute/virtualMachines/rhel
data:    ProvisioningState               :Succeeded
data:    Name                            :rhel
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :RedHat
data:        Offer                       :RHEL
data:        Sku                         :7.2
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic5abbc145c0242c1-os-1462425492101
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli1630678171193501687.blob.core.windows.net/vhds/clic5abbc145c0242c1-os-1462425492101.vhd
data:
data:    OS Profile:
data:      Computer Name                 :rhel
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-32-0F-DD
data:          Provisioning State        :Succeeded
data:          Name                      :rhel-westu-1630678171-nic
data:          Location                  :westus
data:            Public IP address       :104.42.236.196
data:            FQDN                    :rhel-westu-1630678171-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto909893658rhel.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

Şimdi varsayılan SSH bağlantı noktası 22’de kendi VM’nizi ve yukarıdaki çıktıda listelenen tem uygun etki alanı adınızı (FQDN) SSH’leyebilirsiniz. (Ayrıca listelenen IP adresini de kullanabilirsiniz.)

```bash
ssh ops@rhel-westu-1630678171-pip.westus.cloudapp.azure.com
```
Oturum açma işlemi aşağıdaki gibi görünmelidir:

```bash
The authenticity of host 'rhel-westu-1630678171-pip.westus.cloudapp.azure.com (104.42.236.196)' can't be established.
RSA key fingerprint is 0e:81:c4:36:2d:eb:3c:5a:dc:7e:65:8a:3f:3e:b0:cb.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'rhel-westu-1630678171-pip.westus.cloudapp.azure.com,104.42.236.196' (RSA) to the list of known hosts.
[ops@rhel ~]$ ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .cache  .config  .ssh
```

## Sonraki Adımlar

`azure vm quick-create`, bir bash kabuğunda oturum açabileceğiniz ve çalışabileceğiniz şekilde, hızlı olarak bir VM dağıtmanın yoludur. `vm quick-create` kullanmak size karmaşık bir ortama ilişkin ek faydalar sağlamaz.  Altyapınız için özelleştirilmiş bir Linux VM dağıtmak için aşağıdaki makalelerden birini izleyebilirsiniz.

- [Belirli bir dağıtım oluşturmak için Azure Resource Manager şablonu kullanma](virtual-machines-linux-cli-deploy-templates.md)
- [Doğrudan Azure CLI komutları kullanarak bir Linux VM için kendi özel ortamınızı oluşturun](virtual-machines-linux-create-cli-complete.md).
- [Şablonları kullanarak Azure’da SSH Korumalı Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Bu makaleler, Azure altyapısının yanı sıra herhangi bir sayıda mülkiyete ait ve açık kaynak altyapı dağıtımı, yapılandırma ve düzenleme araçları oluşturmaya başlamanızı sağlar.



<!--HONumber=Aug16_HO1-->


