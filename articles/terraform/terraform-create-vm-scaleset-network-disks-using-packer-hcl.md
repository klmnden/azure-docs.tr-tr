---
title: Bir Azure sanal makine ölçek oluşturmak için kullanım Terraform Packer özel görüntüden ayarlayın
description: Yapılandırmak için Terraform ve Packer (bir sanal ağ ve yönetilen bağlanmış diskler ile tamamlanan) tarafından oluşturulan özel bir görüntüden bir Azure sanal makine ölçek kümesi sürümü kullanın.
keywords: terraform, devops, ölçeklendirme ayarlayın, sanal makine, ağ, depolama, modüller, özel resimler, packer
author: VaijanathB
ms.author: tarcher
ms.date: 10/29/2017
ms.topic: article
ms.openlocfilehash: 284eae93de36986e41ba80f98f86495d8f34f57b
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
ms.locfileid: "23945859"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set-from-a-packer-custom-image"></a>Bir Azure sanal makine ölçek oluşturmak için kullanım Terraform Packer özel görüntüden ayarlayın

Bu öğreticide kullandığınız [Terraform](https://www.terraform.io/) oluşturmak ve dağıtmak için bir [Azure sanal makine ölçek kümesi](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) kullanılarak üretilen için özel bir görüntü oluşturulurken [Packer](https://www.packer.io/intro/index.html) yönetilen diskleri kullanarak [HashiCorp yapılandırma dil](https://www.terraform.io/docs/configuration/syntax.html) (HCL).  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Terraform dağıtımını ayarlama
> * Değişkenleri ve çıkışları Terraform dağıtım için kullanın 
> * Oluşturma ve bir ağ altyapısı dağıtma
> * Packer kullanarak özel bir sanal makine görüntüsü oluşturma
> * Oluşturma ve özel görüntü kullanarak bir sanal makine ölçek dağıtma
> * Oluşturma ve bir jumpbox dağıtma 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce
> * [Terraform yükleme ve Azure erişimi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure)
> * [Bir SSH anahtarı çifti oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) zaten yoksa,
> * [Packer yükleme](https://www.packer.io/docs/install/index.html) Packer yerel makinenizde zaten yoksa


## <a name="create-the-file-structure"></a>Dosya yapısı oluşturun

Üç yeni dosyalar aşağıdaki adlarla boş bir dizin oluşturun:

- ```variables.tf```Bu dosya şablonda kullanılan değişkenlerin değerleri tutar.
- ```output.tf```Bu dosya dağıtımdan sonra görüntüleme ayarlarını tanımlar.
- ```vmss.tf```Bu dosya, dağıttığınız altyapıyı kodunu içerir.

##  <a name="create-the-variables"></a>Değişkenleri oluşturun 

Bu adımda, Terraform tarafından oluşturulan kaynakları özelleştirme değişkenleri tanımlayın.

Düzen `variables.tf` dosya, aşağıdaki kodu kopyalayın ve ardından değişiklikleri kaydedin.

```tf 
variable "location" {
  description = "The location where resources are created"
  default     = "East US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources are created"
  default     = ""
}

```

> [!NOTE]
> Resource_group_name değişkeni varsayılan değerini ayarlama, kendi değeri belirleyin.

Dosyayı kaydedin.

Terraform şablonunuzu dağıttığınızda, uygulamaya erişmek için kullanılan tam etki alanı adını almak istiyorsunuz. Kullanım ```output``` kaynak türü Terraform ve get ```fqdn``` kaynak özelliği. 

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
  - İki ortak IP adresi. Bir sanal makine ölçek kümesi yük dengeleyici tarafından kullanılan; diğer SSH jumpbox bağlanmak için kullanılan

Ayrıca tüm kaynakları oluşturulduğu bir kaynak grubu gerekir. 

Düzenle ve aşağıdaki kodu kopyalayın ```vmss.tf``` dosyası: 

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
> Kimliklerini gelecekte kolaylaştırmak Azure'da dağıtılan kaynakları etiketleme önerilir.

## <a name="create-the-network-infrastructure"></a>Ağ altyapısı oluşturma

Oluşturduğunuz dizinde aşağıdaki komutu çalıştırarak Terraform ortamını başlatma `.tf` dosyaları:

```bash
terraform init 
```
 
Sağlayıcı eklentileri Terraform kayıt defterine indirin ```.terraform``` dizin komutu çalıştırdığınız klasörde.

Altyapısını Azure'a dağıtmak için aşağıdaki komutu çalıştırın.

```bash
terraform apply
```

Ortak IP adresinin tam etki alanı adı yapılandırmanıza karşılık geldiğinden emin olun.

![Sanal makine ölçek kümesi genel IP adresi için Terraform tam etki alanı adı](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step4-fqdn.png)

Kaynak grubu aşağıdaki kaynaklar içeriyor:

![Sanal makine ölçek kümesi Terraform ağ kaynakları](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step4-rg.png)


## <a name="create-an-azure-image-using-packer"></a>Packer kullanarak Azure görüntü oluşturma
Öğreticide özetlenen adımları kullanarak özel bir Linux görüntü oluşturma [Packer Linux sanal makine görüntülerini oluşturmak için nasıl kullanılacağını](https://docs.microsoft.com/azure/virtual-machines/linux/build-image-with-packer).
 
İle yüklenmiş NGINX bir deprovisioned Ubuntu görüntüsü oluşturmak için bu öğreticiyi izleyin.

![Packer görüntüsünü oluşturduktan sonra görüntüyü sahip](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/packerimagecreated.png)

> [!NOTE]
> Packer görüntüsündeki Bu öğreticinin amaçları için bir komut yükler nginx çalıştırılır. Kendi komut dosyası oluşturulurken de çalıştırabilirsiniz.

## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi eklemek için altyapı Düzenle

Bu adımda, aşağıdaki kaynaklara önceden dağıtılmış ağda oluşturun:
- Uygulama sunmak ve 4. adımda dağıtıldı genel IP adresi eklemek için azure yük dengeleyici
- Bir Azure yük dengeleyici ve uygulama hizmet ve daha önce yapılandırılmış bir genel IP adresi eklemek için kurallar.
- Azure arka uç adres havuzu ve yük dengeleyiciye atayın 
- Uygulama tarafından kullanılan ve yük dengeleyici üzerinde yapılandırılmış bir sistem durumu araştırma bağlantı noktası 
- Daha önce dağıttığınız vnet üzerinde çalışan yük dengeleyicinin arkasındaki bir defada bir sanal makine ölçek kümesi
- [Nginx](http://nginx.org/) özel görüntüden yüklü sanal makine ölçek düğümlerinde


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

data "azurerm_resource_group" "image" {
  name = "myResourceGroup"
}

data "azurerm_image" "image" {
  name                = "myPackerImage"
  resource_group_name = "${data.azurerm_resource_group.image.name}"
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
    id="${data.azurerm_image.image.id}"
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

Komutunun çıktısını aşağıdaki görüntü gibi görünür:

![Terraform ekleme planı sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step6-plan.png)

Ek kaynaklar Azure dağıtın: 

```bash
terraform apply 
```

Kaynak grubu içeriğini aşağıdaki görüntü gibi görünür:

![Kaynak grubu Terraform sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step6-apply.png)

Bir tarayıcı açın ve komut tarafından döndürülen tam etki alanı adı bağlanın. 


## <a name="add-a-jumpbox-to-the-existing-network"></a>Mevcut bir ağ için bir jumpbox ekleyin 

Bu isteğe bağlı adım bir jumpbox kullanarak sanal makine ölçek örneklerini SSH erişimini etkinleştirir.

Aşağıdaki kaynaklar, mevcut dağıtımınızı ekleyin:
- Sanal makine ölçek kümesi'den aynı alt ağa bağlı bir ağ arabirimi
- Bu ağ arabirimine sahip sanal makine

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

Düzen `outputs.tf` dağıtım tamamlandığında, ana bilgisayar adını jumpbox görüntüleyen aşağıdaki kodu eklemek için:

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

Dağıtım tamamlandıktan sonra kaynak grubu içeriğini aşağıdaki görüntü gibi görünür:

![Kaynak grubu Terraform sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-create-vmss-step8.png)

> [!NOTE]
> Bir parola ile oturum açma jumpbox üzerinde devre dışı bırakılır ve, dağıttığınız sanal makine ölçek kümesi. Sanal makineleri erişmek için SSH ile oturum açın.

## <a name="clean-up-the-environment"></a>Ortamını Temizle

Aşağıdaki komutlar, bu öğreticide oluşturduğunuz kaynakları silin:

```bash
terraform destroy
```

Tür `yes` kaynakları silinmek üzere onaylamanız istendiğinde. Yok etme işleminin tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal makine ölçek kümesini ve jumpbox Terraform kullanarak azure'da dağıtılabilir. Şunları öğrendiniz:

> [!div class="checklist"]
> * Terraform dağıtım başlatılamadı
> * Değişkenleri ve çıkışları Terraform dağıtım için kullanın 
> * Oluşturma ve bir ağ altyapısı dağıtma
> * Packer kullanarak özel bir sanal makine görüntüsü oluşturma
> * Oluşturma ve özel görüntü kullanarak bir sanal makine ölçek dağıtma
> * Oluşturma ve bir jumpbox dağıtma 