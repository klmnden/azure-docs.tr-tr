---
title: Azure'da eksiksiz bir Linux VM oluşturmak için Terraform'u kullanın | Microsoft Docs
description: Terraform oluşturmak ve azure'da eksiksiz bir Linux sanal makine ortamı yönetmek için kullanmayı öğrenin
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
ms.openlocfilehash: 406af03ab81b279be4f0d72dcca4ec8ec0e4ee55
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304644"
---
# <a name="create-a-complete-linux-virtual-machine-infrastructure-in-azure-with-terraform"></a>Azure'da Terraform ile eksiksiz bir Linux sanal makine altyapısı oluşturma

Terraform tanımlayın ve Azure'da eksiksiz altyapı dağıtımlar oluşturmanıza olanak sağlar. Daha okunabilir bir biçimde oluşturun ve Azure kaynakları tutarlı ve tekrarlanabilir bir şekilde yapılandırmak Terraform şablonları oluşturacaksınız. Bu makalede eksiksiz bir Linux ortamı ve destekleyen kaynaklar Terraform ile nasıl oluşturulacağı gösterilmektedir. Da bilgi edinebilirsiniz nasıl [yükleme ve yapılandırma Terraform](terraform-install-configure.md).


## <a name="create-azure-connection-and-resource-group"></a>Azure bağlantı ve kaynak grubu oluşturun

Terraform şablonunun her bölümü Bahsedelim. Tam sürümünü de gördüğünüz [Terraform şablon](#complete-terraform-script) kopyalayın ve yapıştırın.

`provider` Bölümü Azure bir sağlayıcıyı kullanacak şekilde Terraform söyler. Değerlerini almak için *subscription_id*, *client_id*, *client_secret*, ve *Kiracı*, bkz: [yükleyin ve Terraform'u yapılandırın](terraform-install-configure.md). 

> [!TIP]
> Değerler için ortam değişkenlerini oluşturmak veya kullanmakta olduğunuz [Azure Cloud Shell Bash deneyimini](/azure/cloud-shell/overview) , bu bölümde değişken bildirimlerini eklemeniz gerekmez.

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

    tags = {
        environment = "Terraform Demo"
    }
}
```

Ek bölümlere sahip kaynak grubunda başvuru *${azurerm_resource_group.myterraformgroup.name}* .

## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Aşağıdaki bölümde adlı bir sanal ağ oluşturur *myVnet* içinde *10.0.0.0/16* adres alanı:

```tf
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags = {
        environment = "Terraform Demo"
    }
}
```

Aşağıdaki bölümde adlı bir alt ağ oluşturur *mySubnet* içinde *myVnet* sanal ağ:

```tf
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraformgroup.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.2.0/24"
}
```


## <a name="create-public-ip-address"></a>Genel IP adresi oluşturma
Kaynakları Internet üzerinden erişmek için oluşturma ve sanal makinenizde bir genel IP adresi atayın. Aşağıdaki bölümde adlı bir genel IP adresi oluşturur *Mypublicıp*:

```tf
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    allocation_method            = "Dynamic"

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma
Ağ güvenlik grupları, sanal Makinenizin içine ve dışına ağ trafiği akışını denetler. Aşağıdaki bölümde adlı bir ağ güvenlik grubu oluşturur *Vm2* ve TCP bağlantı noktası 22 SSH trafiğine izin verecek bir kural tanımlar:

```tf
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

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-network-interface-card"></a>Sanal ağ arabirim kartı oluşturun
Bir sanal ağ arabirim kartı (NIC), belirtilen sanal ağ, genel IP adresi ve ağ güvenlik grubu, sanal Makinenizin bağlanır. Aşağıdaki bölümde Terraform şablonunda adlı bir sanal NIC oluşturur *Mynıc* oluşturduğunuz sanal ağ kaynaklarına bağlı:

```tf
resource "azurerm_network_interface" "myterraformnic" {
    name                = "myNIC"
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    network_security_group_id = "${azurerm_network_security_group.myterraformnsg.id}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-storage-account-for-diagnostics"></a>Tanılama için depolama hesabı oluşturma
Bir sanal makine için önyükleme tanılaması depolamak için bir depolama hesabı gerekir. Bu önyükleme Tanılaması, sanal makinenizin durumunu izlemek ve sorunlarını gidermenize yardımcı olabilir. Yalnızca önyükleme tanılama verilerini depolamak için oluşturduğunuz depolama hesabı var. Her Depolama hesabı adları benzersiz olmalıdır, aşağıdaki bölümde bazı rastgele metin oluşturur:

```tf
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraformgroup.name}"
    }
    
    byte_length = 8
}
```

Artık bir depolama hesabı oluşturabilirsiniz. Aşağıdaki bölümde, önceki adımda oluşturulan rastgele metin temelinde ada sahip bir depolama hesabı oluşturur:

```tf
resource "azurerm_storage_account" "mystorageaccount" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    location            = "eastus"
    account_replication_type = "LRS"
    account_tier = "Standard"

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Son adım, bir VM oluşturun ve oluşturulan tüm kaynakları kullanmaktır. Aşağıdaki bölümde adlı bir VM oluşturur *myVM* ve adlı sanal NIC'yi ekler *Mynıc*. En son *Ubuntu 16.04 LTS* görüntüsü kullanılır ve bir kullanıcı adlı *azureuser* parola kimlik doğrulaması devre dışı oluşturulur.

 SSH anahtar veri sağlanır *ssh_keys* bölümü. İçinde geçerli bir ortak SSH anahtarının sağlamak *key_data* alan.

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

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="complete-terraform-script"></a>Terraform betiğin tamamı

Bu bölümler bir araya getirin ve Terraform çalışırken görün adlı bir dosya oluşturun. *terraform_azure.tf* ve aşağıdaki içeriği yapıştırın:

```tf
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

    tags = {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags = {
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
    allocation_method            = "Dynamic"

    tags = {
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

    tags = {
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
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags = {
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

    tags = {
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

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="build-and-deploy-the-infrastructure"></a>Altyapıyı ve derleme
Oluşturulan Terraform şablonunuzla birlikte Terraform başlatmak ilk adımdır. Bu adım, Terraform, Azure, şablonunuzda oluşturmak için tüm önkoşulları sahip olmasını sağlar.

```bash
terraform init
```

Sonraki adım, gözden geçirin ve şablon doğrulama Terraform olmasını sağlamaktır. Bu adım, istenen kaynaklara Terraform tarafından kaydedilmiş durum bilgilerini karşılaştırır ve ardından planlı yürütme çıkarır. Kaynakları *değil* Azure'da oluşturuldu.

```bash
terraform plan
```

Önceki komutu yürüttükten sonra aşağıdaki ekrana benzer bir şey görmeniz gerekir:

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

Her şeyin doğru bakar ve Azure altyapısı oluşturmak hazır olursunuz, Terraform şablonda uygulayın:

```bash
terraform apply
```

Terraform tamamlandıktan sonra VM altyapınızı hazırdır. İle sanal makinenizin genel IP adresini [az vm show](/cli/azure/vm):

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
```

Ardından sanal Makinenize SSH yapabilirsiniz:

```bash
ssh azureuser@<publicIps>
```

## <a name="next-steps"></a>Sonraki adımlar
Terraform kullanarak Azure'da temel altyapı oluşturdunuz. Daha karmaşık senaryolarda, kullanım yük dengeleyicileri ve sanal makine ölçek örnekler de dahil olmak üzere kümeleri bakın çok sayıda [Azure Terraform örnekler](https://github.com/hashicorp/terraform/tree/master/examples). Desteklenen Azure sağlayıcılar güncel bir listesi için bkz. [Terraform belgeleri](https://www.terraform.io/docs/providers/azurerm/index.html).
