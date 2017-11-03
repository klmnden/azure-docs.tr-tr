---
title: "Terraform kullanın ve HCL kullanılarak ayarlanan bir Azure VM ölçek oluşturun."
description: "Kullanım Terraform yapılandırmak için ve bir Azure sanal makine ölçek bir sanal ağ ile tam ayarlayın ve yönetilen sürüm diskler bağlı."
keywords: "terraform, devops, ölçeklendirme ayarlayın, sanal makine, ağ, depolama, modüller"
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/04/2017
ms.topic: article
ms.openlocfilehash: 7a4e21d547b3d2b2399f9f68b1babd9f82a421b7
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="use-terraform-to-plan-and-create-a-networked-azure-vm-scale-set-with-managed-storage"></a>Planlama ve yönetilen Depolama'yı ile ağa bağlı bir Azure VM ölçek oluşturmak için Terraform kullanın

Bu makalede, kullandığınız [Terraform](https://www.terraform.io/) oluşturmak ve dağıtmak için bir [Azure sanal makinesi scaleset](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) kullanarak yönetilen disklerle [Hashicorp yapılandırma dil](https://www.terraform.io/docs/configuration/syntax.html) (HCL).  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Terraform dağıtımını ayarlama
> * Değişkenleri ve çıkışları Terraform dağıtım için kullanın 
> * Oluşturma ve ağ altyapısı dağıtma
> * Oluşturma ve bir sanal makine ölçek kümesini dağıtmak ve ağa bağlayın
> * Oluşturma ve sanal makineleri SSH aracılığıyla bağlanmak için bir jumpbox dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

- [Terraform yükleme ve Azure erişimi yapılandırma](/azure/virtual-machines/linux/terraform-install-configure)
- [Bir SSH anahtarı çifti oluşturma](/azure/virtual-machines/linux/mac-create-ssh-keys) zaten yoksa.

## <a name="create-the-file-structure"></a>Dosya yapısı oluşturun

Üç yeni dosyalar aşağıdaki adlarla boş bir dizin oluşturun:

- `variables.tf`Bu dosya şablonda kullanılan değişkenlerin değerleri tutar.
- `output.tf`Bu dosya dağıtımdan sonra görüntülenen ayarlar açıklanır.
- `vmss.tf`Bu kodu sanal makine ölçek için altyapı ayarlayın.

## <a name="create-the-variables-and-output-definitions"></a>Değişkenleri oluşturun ve tanımları çıkış

Bu adımda, Terraform tarafından oluşturulan kaynakları özelleştirme değişkenleri tanımlayın.

Düzen `variables.tf` dosya, aşağıdaki kodu kopyalayın ve ardından değişiklikleri kaydedin.

```tf 
variable "location" {
  description = "The location where resources will be created"
  default     = "West US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources will be created"
  default     = "myResourceGroup"
}
```

Düzen `output.tf` dosya ve sanal makineler için tam etki alanı adı göstermek için aşağıdaki kodu kopyalayın. 

```hcl 
output "vmss_public_ip" {
    value = "${azurerm_public_ip.vmss.fqdn}"
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Ağ altyapısı bir şablon oluştur

Bu adımda aşağıdaki ağ altyapısında yeni bir Azure kaynak grubu oluşturun: 

  - 10.0.0.0/16 adres alanına sahip bir VNET 
  - Bir alt ağ ile 10.0.2.0/24 adres alanı
  - İki ortak IP adresi. Bir sanal makine ölçek kümesi yük dengeleyici, diğer SSH jumpbox bağlanmak için kullanılan tarafından kullanılır.


Düzenle ve aşağıdaki kodu kopyalayın `vmss.tf` dosyası: 

```tf
resource "azurerm_resource_group" "vmss" {
  name     = "${var.resource_group_name}"
  location = "${var.location}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_network" "vmss" {
  name                = "vmss-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_subnet" "vmss" {
  name                 = "vmss-subnet"
  resource_group_name  = "${azurerm_resource_group.vmss.name}"
  virtual_network_name = "${azurerm_virtual_network.vmss.name}"
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "vmss" {
  name                         = "vmss-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

``` 

> [!NOTE]
> Kimliklerini gelecekte kolaylaştırmak Azure'da dağıtılan kaynakları etiketlemek için iyi bir fikirdir.

## <a name="create-the-network-infrastructure"></a>Ağ altyapısı oluşturma

Oluşturduğunuz dizinde aşağıdaki komutu çalıştırarak Terraform ortamını başlatma `.tf` dosyaları:

```bash
terraform init 
```

Ardından Azure altyapısında dağıtmak için aşağıdaki komutu çalıştırın.

```bash
terraform apply
```

Genel IP adresi FQDN'sini yapılandırmanıza karşılık geldiğinden emin olun: ![genel IP adresi için VMSS terraform FQDN](./media/tf-create-vmss-step4-fqdn.png)


Kaynak grubu aşağıdaki kaynaklara sahip olmalıdır: ![VMSS terraform ağ kaynakları](./media/tf-create-vmss-step4-rg.png)


## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi eklemek için altyapı Düzenle

Bu adımda, aşağıdaki kaynaklara şablonuna ekleyin:

- Bir Azure yük dengeleyici ve uygulama hizmet ve daha önce yapılandırılmış bir genel IP adresi eklemek için kurallar.
- Azure arka uç adres havuzu ve loadbalancer atayın 
- Uygulama tarafından kullanılan ve yük dengeleyici üzerinde yapılandırılmış bir sistem durumu araştırma bağlantı noktası 
- Daha önce dağıttığınız vnet üzerinde çalışan yük dengeleyicinin arkasındaki bir defada bir sanal makine ölçek kümesi
- [Nginx](http://nginx.org/) bir özel betik uzantısı kullanılarak sanal makine ölçek düğümlerinde.

Sonuna aşağıdaki kodu ekleyin `vmss.tf` dosya.

```tf
resource "azurerm_lb" "vmss" {
  name                = "vmss-lb"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  frontend_ip_configuration {
    name                 = "PublicIPAddress"
    public_ip_address_id = "${azurerm_public_ip.vmss.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_lb_backend_address_pool" "bpepool" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "BackEndAddressPool"
}

resource "azurerm_lb_probe" "vmss" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "ssh-running-probe"
  port                = "${var.application_port}"
}

resource "azurerm_lb_rule" "lbnatrule" {
    resource_group_name            = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id                = "${azurerm_lb.vmss.id}"
    name                           = "http"
    protocol                       = "Tcp"
    frontend_port                  = "${var.application_port}"
    backend_port                   = "${var.application_port}"
    backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
    frontend_ip_configuration_name = "PublicIPAddress"
    probe_id                       = "${azurerm_lb_probe.vmss.id}"
}

resource "azurerm_virtual_machine_scale_set" "vmss" {
  name                = "vmscaleset"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  upgrade_policy_mode = "Manual"

  sku {
    name     = "Standard_DS1_v2"
    tier     = "Standard"
    capacity = 2
  }

  storage_profile_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_profile_os_disk {
    name              = ""
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_profile_data_disk {
    lun          = 0
    caching        = "ReadWrite"
    create_option  = "Empty"
    disk_size_gb   = 10
  }

  os_profile {
    computer_name_prefix = "vmlab"
    admin_username       = "azureuser"
    admin_password       = "Passwword1234"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  network_profile {
    name    = "terraformnetworkprofile"
    primary = true

    ip_configuration {
      name                                   = "IPConfiguration"
      subnet_id                              = "${azurerm_subnet.vmss.id}"
      load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
    }
  }

  extension { 
    name = "vmssextension"
    publisher = "Microsoft.OSTCExtensions"
    type = "CustomScriptForLinux"
    type_handler_version = "1.2"
    settings = <<SETTINGS
    {
        "commandToExecute": "sudo apt-get -y install nginx"
    }
    SETTINGS
  }

  tags {
    environment = "codelab"
  }
}
```

Aşağıdaki kodu ekleyerek bir dağıtımı özelleştirmek `variables.tf`:

```tf 
variable "application_port" {
    description = "The port that you want to expose to the external load balancer"
    default     = 80
}

variable "admin_password" {
    description = "Default password for admin"
    default = "Passwwoord11223344"
}
``` 


## <a name="deploy-the-virtual-machine-scale-set-in-azure"></a>Sanal makineyi ölçeği Azure'da Ayarla dağıtma

Sanal makine ölçek kümesi dağıtımı görselleştirmek için aşağıdaki komutu çalıştırın:

```bash
terraform plan
```

Komutunun çıktısını aşağıdaki gibi görünmelidir.
![Terraform vmss plan Ekle](./media/tf-create-vmss-step6-plan884d3aefd9708a711bc09a66e85eb149c23a3ccff959655ec00418168b2bd481.png)

Ardından Azure ek kaynakları dağıtabilirsiniz: 

```bash
terraform apply 
```

Kaynak grubu içeriğini gibi görünmelidir:

![Terraform vm scaleset kaynak grubu](./media/tf-create-vmss-step6-apply.png)

Bir tarayıcı açın ve komut tarafından döndürülen FQDN bağlanın. 

## <a name="add-an-ssh-jumpbox-to-the-existing-network"></a>Mevcut bir ağ için bir SSH jumpbox Ekle 

Bu adımda, aşağıdaki kaynaklara yapılandırın:
- Bir ağ arabirimi sanal makine ölçek kümesi aynı alt ağda bağlı.
- Bir sanal makine bu ağ arabirimi ile bağlı. Bu 'jumpbox' Uzaktan erişilebilir. Bağlantı kurulduktan sonra herhangi bir ölçek kümesindeki sanal makinelerin SSH kullanabilirsiniz.



Sonuna aşağıdaki kodu ekleyin `vmss.tf` dosyası:

```hcl 
resource "azurerm_public_ip" "jumpbox" {
  name                         = "jumpbox-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}-ssh"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_network_interface" "jumpbox" {
  name                = "jumpbox-nic"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  ip_configuration {
    name                          = "IPConfiguration"
    subnet_id                     = "${azurerm_subnet.vmss.id}"
    private_ip_address_allocation = "dynamic"
    public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_machine" "jumpbox" {
  name                  = "jumpbox"
  location              = "${var.location}"
  resource_group_name   = "${azurerm_resource_group.vmss.name}"
  network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
  vm_size               = "Standard_DS1_v2"

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "jumpbox-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  os_profile {
    computer_name  = "jumpbox"
    admin_username = "azureuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  tags {
    environment = "codelab"
  }
}
```

Düzenleme `outputs.tf` ve dağıtım tamamlandığında, ana bilgisayar jumpbox adını görüntülemek için aşağıdaki kodu ekleyin:

```
output "jumpbox_public_ip" {
    value = "${azurerm_public_ip.jumpbox.fqdn}"
}
```

## <a name="deploy-the-jumpbox"></a>Jumpbox dağıtma

Jumpbox dağıtın.

```bash
terraform apply 
```

Dağıtım tamamlandıktan sonra kaynak grubu içeriği şuna benzer:

![Terraform vm scaleset kaynak grubu](./media/tf-create-create-vmss-step8.png)

> [!NOTE]
> Bir parola ile oturum açma jumpbox üzerinde devre dışı bırakılır ve, dağıttığınız sanal makine ölçek kümesi. Sanal makineleri erişmek için SSH ile oturum açın.

## <a name="clean-up-the-environment"></a>Ortamını Temizle

Aşağıdaki komutlar, bu öğreticide oluşturduğunuz kaynakları silin:

```bash
terraform destroy
```

Tür `yes` kaynakları silinmek üzere onaylamanız istendiğinde. Yok etme işleminin tamamlanması birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide, bir sanal makineyi ölçeği Terraform kullanarak Azure'da Ayarla dağıtıldı. Şunları öğrendiniz:

> [!div class="checklist"]
> * Terraform dağıtım başlatılamadı
> * Değişkenleri ve çıkışları Terraform dağıtım için kullanın 
> * Oluşturma ve bir ağ altyapısı dağıtma
> * Oluşturma ve bir sanal makine ölçek kümesini dağıtmak ve mevcut bir ortama ekleme
> * Oluşturma ve sanal makineleri SSH aracılığıyla bağlanmak için bir jumpbox dağıtma 
