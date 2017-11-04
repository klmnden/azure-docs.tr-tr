---
title: "Bir yük oluşturmak için kullanım Terraform modülleri dengeli Linux VM Azure üzerinde altyapısı"
description: "Azure yük dengeleyici ile Linux sanal makine küme oluşturmak için Terraform modülleri kullanmayı öğrenin"
keywords: "terraform, devops, sanal makine, ağ, modüller"
author: rloutlaw
manager: justhe
ms.service: virtual-machines-linux
ms.custom: devops
ms.topic: article
ms.date: 10/19/2017
ms.author: routlaw
ms.openlocfilehash: 86fee5446065ffa8b5e8186ba8d8f56aa3efc2ea
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="create-a-vm-cluster-with-terraform-using-custom-hcl"></a>Özel HCL'yi kullanarak Terraform ile VM küme oluşturma

Bu öğretici kullanılarak küçük bilgi işlem küme oluşturma gösterir [Hashicorp yapılandırma dil](https://www.terraform.io/docs/configuration/syntax.html) (HCL). İki Linux VM'ler için bir yük dengeleyici yapılandırması oluşturur bir [kullanılabilirlik kümesi](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)ve tüm gerekli ağ kaynaklarına.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure ile kimlik doğrulaması ayarlama
> * Terraform şablonu oluşturma
> * Plan değişikliklerle Görselleştirme
> * Kümeyi oluşturmak için yapılandırmasını Uygula

## <a name="set-up-authentication-with-azure"></a>Azure ile kimlik doğrulaması ayarlama

> [!TIP]
> Varsa, [Terraform ortam değişkenlerini kullanma](/azure/virtual-machines/linux/terraform-install-configure#set-environment-variables) veya Bu öğretici Çalıştır [Azure bulut Kabuk](/azure/cloud-shell/overview), bu adımı atlayın.

 Gözden geçirme [Terraform yükleyin ve Azure erişimi yapılandırma](/azure/virtual-machines/linux/terraform-install-configure) bir Azure hizmet sorumlusu oluşturmak için. Yeni bir dosya doldurmak için bu hizmet asıl adı kullanmasını `azureProviderAndCreds.tf` aşağıdaki kod ile boş bir dizinde:

```tf
variable subscription_id {}
variable tenant_id {}
variable client_id {}
variable client_secret {}

provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## <a name="create-the-template"></a>Şablonu oluşturma

Adlı yeni bir Terraform şablonu oluşturma `main.tf` aşağıdaki kod ile: 

```tf
resource "azurerm_resource_group" "test" {
  name     = "acctestrg"
  location = "West US 2"
}

resource "azurerm_virtual_network" "test" {
  name                = "acctvn"
  address_space       = ["10.0.0.0/16"]
  location            = "${azurerm_resource_group.test.location}"
  resource_group_name = "${azurerm_resource_group.test.name}"
}

resource "azurerm_subnet" "test" {
  name                 = "acctsub"
  resource_group_name  = "${azurerm_resource_group.test.name}"
  virtual_network_name = "${azurerm_virtual_network.test.name}"
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "test" {
  name                         = "publicIPForLB"
  location                     = "${azurerm_resource_group.test.location}"
  resource_group_name          = "${azurerm_resource_group.test.name}"
  public_ip_address_allocation = "static"
}

resource "azurerm_lb" "test" {
  name                = "loadBalancer"
  location            = "${azurerm_resource_group.test.location}"
  resource_group_name = "${azurerm_resource_group.test.name}"

  frontend_ip_configuration {
    name                 = "publicIPAddress"
    public_ip_address_id = "${azurerm_public_ip.test.id}"
  }
}

resource "azurerm_lb_backend_address_pool" "test" {
  resource_group_name = "${azurerm_resource_group.test.name}"
  loadbalancer_id     = "${azurerm_lb.test.id}"
  name                = "BackEndAddressPool"
}

resource "azurerm_network_interface" "test" {
  count               = 2
  name                = "acctni${count.index}"
  location            = "${azurerm_resource_group.test.location}"
  resource_group_name = "${azurerm_resource_group.test.name}"

  ip_configuration {
    name                          = "testConfiguration"
    subnet_id                     = "${azurerm_subnet.test.id}"
    private_ip_address_allocation = "dynamic"
    load_balancer_backend_address_pools_ids = ["${azurerm_lb_backend_address_pool.test.id}"]
  }
}

resource "azurerm_managed_disk" "test" {
  count                = 2
  name                 = "datadisk_existing_${count.index}"
  location             = "${azurerm_resource_group.test.location}"
  resource_group_name  = "${azurerm_resource_group.test.name}"
  storage_account_type = "Standard_LRS"
  create_option        = "Empty"
  disk_size_gb         = "1023"
}

resource "azurerm_availability_set" "avset" {
  name                         = "avset"
  location                     = "${azurerm_resource_group.test.location}"
  resource_group_name          = "${azurerm_resource_group.test.name}"
  platform_fault_domain_count  = 2
  platform_update_domain_count = 2
  managed                      = true
}

resource "azurerm_virtual_machine" "test" {
  count                 = 2
  name                  = "acctvm${count.index}"
  location              = "${azurerm_resource_group.test.location}"
  availability_set_id   = "${azurerm_availability_set.avset.id}"
  resource_group_name   = "${azurerm_resource_group.test.name}"
  network_interface_ids = ["${element(azurerm_network_interface.test.*.id, count.index)}"]
  vm_size               = "Standard_DS1_v2"

  # Uncomment this line to delete the OS disk automatically when deleting the VM
  # delete_os_disk_on_termination = true

  # Uncomment this line to delete the data disks automatically when deleting the VM
  # delete_data_disks_on_termination = true

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "myosdisk${count.index}"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  # Optional data disks
  storage_data_disk {
    name              = "datadisk_new_${count.index}"
    managed_disk_type = "Standard_LRS"
    create_option     = "Empty"
    lun               = 0
    disk_size_gb      = "1023"
  }

  storage_data_disk {
    name            = "${element(azurerm_managed_disk.test.*.name, count.index)}"
    managed_disk_id = "${element(azurerm_managed_disk.test.*.id, count.index)}"
    create_option   = "Attach"
    lun             = 1
    disk_size_gb    = "${element(azurerm_managed_disk.test.*.disk_size_gb, count.index)}"
  }

  os_profile {
    computer_name  = "hostname"
    admin_username = "testadmin"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }

  tags {
    environment = "staging"
  }
}
```

## <a name="visualize-the-changes-with-plan"></a>Plan değişikliklerle Görselleştirme

Çalıştırma `terraform plan` şablonu tarafından oluşturulan sanal makine altyapısı önizlemek için.

![Terraform planı](media/terraformPlanVmsWithModules.png)


## <a name="create-the-virtual-machines-with-apply"></a>Sanal makineler Uygula ile oluşturma

Çalıştırma `terraform apply` Azure VM kümede sağlayacak.

![Terraform Uygula](media/terraformApplyVmsWithModules.png)

## <a name="next-steps"></a>Sonraki adımlar

- Listesine göz [Azure Terraform modülleri](https://registry.terraform.io/modules/Azure)
- Oluşturma bir [sanal makine ölçek kümesi ile Terraform]()