---
title: Tam bir Linux VM oluşturmak için Terraform kullanın | Microsoft Docs
description: Terraform azure'da tam bir Linux sanal makine ortamı oluşturmak ve yönetmek için nasıl kullanılacağını öğrenin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 6b2dc2e8859efdcc57c45831381bc1870495ecf6
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32776032"
---
# <a name="create-a-complete-linux-virtual-machine-infrastructure-in-azure-with-terraform"></a>Azure Terraform ile tam bir Linux sanal makine alt yapı oluşturun

Terraform tanımlamak ve Azure'da eksiksiz altyapı dağıtımları oluşturmanıza olanak sağlar. Oluşturma ve tutarlı ve tekrarlanabilir bir şekilde Azure kaynaklarını yapılandırma Terraform şablonları okunabilir bir biçimde oluşturun. Bu makalede eksiksiz bir Linux ortamı ve Terraform kaynaklarla destekleme nasıl oluşturulacağı gösterilmektedir. Ayrıca öğrenebilirsiniz nasıl [yükleyin ve Terraform yapılandırma](terraform-install-configure.md).


## <a name="create-azure-connection-and-resource-group"></a>Azure bağlantı ve kaynak grubu oluştur

Her bir Terraform şablonu bölümü edelim. Tam sürümünü de görebilirsiniz [Terraform şablonu](#complete-terraform-script) kopyalayın ve yapıştırın.

`provider` Bölüm Azure bir sağlayıcı kullanacak şekilde Terraform söyler. Değerlerini almak için *ABONELİK_KİMLİĞİ*, *client_id*, *client_secret*, ve *tenant_id*, bkz: [yükleyin ve Terraform yapılandırma](terraform-install-configure.md). 

> [!TIP]
> Ortam değişkenleri için değerleri oluşturmak veya kullanmakta olduğunuz [Azure bulut Kabuk Bash deneyimi](/azure/cloud-shell/overview) , bu bölümde değişken bildirimleri dahil gerek yoktur.

```tf
provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

Aşağıdaki bölümde adlı bir kaynak grubu oluşturur `myResourceGroup` içinde `eastus` konumu:

```tf
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "eastus"

    tags {
        environment = "Terraform Demo"
    }
}
```

Kaynak grubu ile başvuru ek bölümlere *${azurerm_resource_group.myterraformgroup.name}*.

## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Aşağıdaki bölümde adlı bir sanal ağ oluşturur *myVnet* içinde *10.0.0.0/16* adres alanı:

```tf
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags {
        environment = "Terraform Demo"
    }
}
```
: Adlı bir alt ağı aşağıdaki bölümde oluşturur *mySubnet* içinde *myVnet* sanal ağ

```tf
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraformgroup.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.2.0/24"
}
```


## <a name="create-public-ip-address"></a>Ortak IP adresi oluştur
Internet'te kaynaklara erişmek için oluşturmak ve VM'nize genel bir IP adresi atayın. Aşağıdaki bölümde adlı ortak IP adresi oluşturur *myPublicIP*:

```tf
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturun
Ağ güvenlik grupları, VM ve ağ trafiği akışını denetler. Aşağıdaki bölümde adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup* ve TCP bağlantı noktası 22 SSH trafiğine izin verecek bir kural tanımlar:

```tf
resource "azurerm_network_security_group" "temyterraformpublicipnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-network-interface-card"></a>Sanal ağ arabirim kartı oluşturma
Bir sanal ağ arabirim kartı (NIC), VM verilen sanal ağ, genel IP adresi ve ağ güvenlik grubu bağlanır. Adlı bir sanal NIC Terraform şablon aşağıdaki bölümde oluşturur *myNIC* oluşturduğunuz sanal ağ kaynaklarına bağlı:

```tf
resource "azurerm_network_interface" "myterraformnic" {
    name                = "myNIC"
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-storage-account-for-diagnostics"></a>Diagnostics depolama hesabı oluşturma
Bir VM için önyükleme tanılaması depolamak için bir depolama hesabı gerekir. Bu önyükleme tanılaması sorunlarını gidermek ve VM durumunu izlemenize yardımcı olabilir. Yalnızca önyükleme tanılama verilerini depolamak için oluşturduğunuz depolama hesabı anlamına gelmektedir. Her Depolama hesabı adları benzersiz olmalıdır, aşağıdaki bölümde bazı rastgele metin oluşturur:

```tf
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraformgroup.name}"
    }
    
    byte_length = 8
}
```

Artık bir depolama hesabı oluşturabilirsiniz. Aşağıdaki bölümde önceki adımda oluşturulan rastgele metin göre ada sahip bir depolama hesabı oluşturur:

```tf
resource "azurerm_storage_account" "mystorageaccount" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    location            = "eastus"
    account_replication_type = "LRS"
    account_tier = "Standard"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Son adım, bir VM oluşturun ve oluşturulan tüm kaynakları kullanmaktır. Adlı bir VM'den aşağıdaki bölümde oluşturur *myVM* ve adlı bir sanal NIC ekler *myNIC*. En son *Ubuntu 16.04-LTS* görüntü kullanılır ve adlı bir kullanıcının *azureuser* parola kimlik doğrulaması devre dışı oluşturulur.

 SSH anahtar verileri sağlanır *ssh_keys* bölümü. Geçerli bir ortak SSH anahtar sağlamak *key_data* alan.

```tf
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
    resource_group_name   = "${azurerm_resource_group.myterraformgroup.name}"
    network_interface_ids = ["${azurerm_network_interface.myterraformnic.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Premium_LRS"
    }

    storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    os_profile {
        computer_name  = "myvm"
        admin_username = "azureuser"
    }

    os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
            path     = "/home/azureuser/.ssh/authorized_keys"
            key_data = "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
        }
    }

    boot_diagnostics {
        enabled     = "true"
        storage_uri = "${azurerm_storage_account.mystorageaccount.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="complete-terraform-script"></a>Tam Terraform komut dosyası

Bu bölümler bir araya getirme ve eylemde Terraform görmek için adlı bir dosya oluşturun *terraform_azure.tf* ve aşağıdaki içeriği yapıştırın:

```tf
variable "resourcename" {
  default = "myResourceGroup"
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}

# Create a resource group if it doesn’t exist
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "eastus"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# Create subnet
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraformgroup.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.1.0/24"
}

# Create public IPs
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# Create network interface
resource "azurerm_network_interface" "myterraformnic" {
    name                      = "myNIC"
    location                  = "eastus"
    resource_group_name       = "${azurerm_resource_group.myterraformgroup.name}"
    network_security_group_id = "${azurerm_network_security_group.myterraformnsg.id}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# Generate random text for a unique storage account name
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraformgroup.name}"
    }
    
    byte_length = 8
}

# Create storage account for boot diagnostics
resource "azurerm_storage_account" "mystorageaccount" {
    name                        = "diag${random_id.randomId.hex}"
    resource_group_name         = "${azurerm_resource_group.myterraformgroup.name}"
    location                    = "eastus"
    account_tier                = "Standard"
    account_replication_type    = "LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual machine
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
    resource_group_name   = "${azurerm_resource_group.myterraformgroup.name}"
    network_interface_ids = ["${azurerm_network_interface.myterraformnic.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Premium_LRS"
    }

    storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    os_profile {
        computer_name  = "myvm"
        admin_username = "azureuser"
    }

    os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
            path     = "/home/azureuser/.ssh/authorized_keys"
            key_data = "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
        }
    }

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.mystorageaccount.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="build-and-deploy-the-infrastructure"></a>Derleme ve altyapı dağıtma
Oluşturulan, Terraform şablonunuzla Terraform başlatma ilk adımdır. Bu adım Terraform şablonunuzu azure'da oluşturmak için tüm önkoşulların sahip olmasını sağlar.

```bash
terraform init
```

Sonraki adım gözden geçirin ve şablon doğrulamak Terraform olmalıdır. Bu adım, istenen kaynakları tarafından Terraform kaydedilmiş durum bilgilerini karşılaştırır ve planlı yürütme çıkarır. Kaynakları *değil* Azure içinde oluşturuldu.

```bash
terraform plan
```

Önceki komutu yürüttükten sonra aşağıdaki ekrana benzer şekilde görmeniz gerekir:

```bash
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


...

Note: You didn’t specify an “-out” parameter to save this plan, so when
“apply” is called, Terraform can’t guarantee this is what will execute.
  + azurerm_resource_group.myterraform
      <snip>
  + azurerm_virtual_network.myterraformnetwork
      <snip>
  + azurerm_network_interface.myterraformnic
      <snip>
  + azurerm_network_security_group.myterraformnsg
      <snip>
  + azurerm_public_ip.myterraformpublicip
      <snip>
  + azurerm_subnet.myterraformsubnet
      <snip>
  + azurerm_virtual_machine.myterraformvm
      <snip>
Plan: 7 to add, 0 to change, 0 to destroy.
```

Her şeyin doğru arar ve Azure altyapısı oluşturmak hazır, Terraform şablon Uygula:

```bash
terraform apply
```

Terraform işlemi tamamlandıktan sonra VM altyapınızı hazırdır. VM ile genel IP adresi elde [az vm Göster](/cli/azure/vm#az_vm_show):

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
```

Ardından, VM SSH yapabilirsiniz:

```bash
ssh azureuser@<publicIps>
```

## <a name="next-steps"></a>Sonraki adımlar
Terraform kullanarak Azure'da temel altyapı oluşturdunuz. Daha karmaşık senaryolar için kullanım yük dengeleyicileri ve sanal makine ölçekleme örnekler dahil olmak üzere kümeleri, bkz: çok sayıda [Terraform örnekler Azure için](https://github.com/hashicorp/terraform/tree/master/examples). Desteklenen Azure sağlayıcılar güncel bir listesi için bkz: [Terraform belgelerine](https://www.terraform.io/docs/providers/azurerm/index.html).
