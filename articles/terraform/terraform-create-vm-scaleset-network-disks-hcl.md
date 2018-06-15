---
title: Bir Azure sanal makine ölçek oluşturmak için kullanım Terraform ayarlayın
description: Diskleri yapılandırmak için Terraform ve bir Azure sanal makine ölçek bir sanal ağ ile tam ayarlayın ve yönetilen sürüm kullanma hakkında öğretici bağlı
keywords: terraform, devops, sanal makine, Azure, ölçeklendirme kümesi, ağ, depolama, modüller
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/04/2018
ms.topic: article
ms.openlocfilehash: b7cd9ad90198ead7c68d838547232429dbd1289f
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757329"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale"></a>Bir Azure sanal makine ölçek oluşturmak için Terraform kullanın

[Azure sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets) oluşturmak ve aynı grup yönetmek izin burada sanal makine örnekleri sayısının otomatik olarak artırabilir, veya isteğe bağlı veya tanımlı bir zamanlamayı yanıt olarak azaltmak dengeli sanal makinelerin yük. 

Bu öğreticide, nasıl kullanacağınızı öğrenin [Azure bulut Kabuk](/azure/cloud-shell/overview) aşağıdaki görevleri gerçekleştirmek için:

> [!div class="checklist"]
> * Bir Terraform dağıtım ayarlayın.
> * Değişkenleri ve çıkışları Terraform dağıtım için kullanın 
> * Oluşturma ve ağ altyapısı dağıtma
> * Oluşturma ve bir sanal makine ölçek kümesini dağıtmak ve ağa bağlayın
> * Oluşturma ve sanal makineleri SSH aracılığıyla bağlanmak için bir jumpbox dağıtma

> [!NOTE]
> Bu makalede kullanılan yapılandırma dosyalarını olan Terraform en son sürümünü [harika Terraform deposu github'da](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terraform yükleme**: makalesindeki yönergeleri izleyin [Terraform ve Azure erişimi yapılandırma](/azure/virtual-machines/linux/terraform-install-configure)

- **Bir SSH anahtarı çifti oluşturma**: anahtar çifti, makalesindeki yönergeleri izleyin bir SSH zaten yoksa [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmaya](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturun

1. Gözat [Azure portal](http://portal.azure.com).

1. Açık [Azure bulut Kabuk](/azure/cloud-shell/overview). Bir ortam önceden seçmediyseniz, seçin **Bash** ortamınız olarak.

    ![Bulut Kabuk isteminde](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Değiştirme dizinleri `clouddrive` dizin.

    ```bash
    cd clouddrive
    ```

1. Adlı bir dizin oluşturun `vmss`.

    ```bash
    mkdir vmss
    ```

1. Dizinleri yeni dizine değiştirin:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>Değişkenleri tanımları oluşturma
Bu bölümde, Terraform tarafından oluşturulan kaynakları özelleştirme değişkenleri tanımlayın.

Azure bulut Kabuk içinde aşağıdaki adımları gerçekleştirin:

1. Adlı bir dosya oluşturun `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

  ```JSON
  variable "location" {
    description = "The location where resources will be created"
  }

  variable "tags" {
    description = "A map of the tags to use for the resources that are deployed"
    type        = "map"

    default = {
      environment = "codelab"
    }
  }

  variable "resource_group_name" {
    description = "The name of the resource group in which the resources will be created"
    default     = "myResourceGroup"
  }
  ```

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Çıktı tanımları oluşturma
Bu bölümde, çıktı dağıtımdan sonra tanımlayan dosyasını oluşturun.

Azure bulut Kabuk içinde aşağıdaki adımları gerçekleştirin:

1. Adlı bir dosya oluşturun `output.tf`.

    ```bash
    vi output.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Sanal makineler için tam etki alanı adı (FQDN) kullanıma sunmak için düzenleyicisine aşağıdaki kodu yapıştırın. :

  ```JSON
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
  ```

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Ağ altyapısı bir şablon oluştur
Bu bölümde, yeni bir Azure kaynak grubu aşağıdaki ağ altyapısında oluşturun: 

  - Bir sanal ağ (VNET) 10.0.0.0/16 adres alanına sahip 
  - Bir alt ağ ile 10.0.2.0/24 adres alanı
  - İki ortak IP adresi. Bir sanal makine ölçek kümesi yük dengeleyici, diğer SSH jumpbox bağlanmak için kullanılan tarafından kullanılır.

Azure bulut Kabuk içinde aşağıdaki adımları gerçekleştirin:

1. Adlı bir dosya oluşturun `vmss.tf` sanal makine ölçek açıklamak için altyapı ayarlayın.

    ```bash
    vi vmss.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Sanal makineler için tam etki alanı adı (FQDN) kullanıma sunmak için dosyanın sonuna aşağıdaki kodu yapıştırın. 

  ```JSON
  resource "azurerm_resource_group" "vmss" {
    name     = "${var.resource_group_name}"
    location = "${var.location}"
    tags     = "${var.tags}"
  }

  resource "random_string" "fqdn" {
    length  = 6
    special = false
    upper   = false
    number  = false
  }

  resource "azurerm_virtual_network" "vmss" {
    name                = "vmss-vnet"
    address_space       = ["10.0.0.0/16"]
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    tags                = "${var.tags}"
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
    domain_name_label            = "${random_string.fqdn.result}"
    tags                         = "${var.tags}"
  }
  ```

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

  ```bash
  :wq
  ```

## <a name="provision-the-network-infrastructure"></a>Ağ altyapısı sağlama
Yapılandırma dosyaları (.tf) oluşturulduğu dizininden Azure bulut Kabuğu'nu kullanarak, aşağıdaki adımları gerçekleştirin:

1. Terraform başlatır.

  ```bash
  terraform init 
  ```

1. Tanımlanan altyapısını Azure'a dağıtmak için aşağıdaki komutu çalıştırın.

  ```bash
  terraform apply
  ```

  Terraform sizden bir "Konum" değeri **konumu** değişkeni tanımlanmış `variables.tf`, ancak hiçbir zaman ayarlayın. "Batı Enter seçerek ve ardından ABD" gibi herhangi bir geçerli konumu - girebilirsiniz. (Herhangi bir değer parantezler alanları ile kullanın.)

1. Terraform tanımlandığı şekilde çıktı yazdıran `output.tf` dosya. Aşağıdaki ekran görüntüsünde gösterildiği gibi FQDN biçimini alır &lt;kimliği >.&lt; Konum >. cloudapp.azure.com. ID değeri hesaplanan bir değerdir ve konum Terraform çalıştırırken sağladığınız değerdir.

  ![Genel IP adresi için sanal makine ölçek kümesi tam etki alanı adı](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. Azure portal menüsünde seçin **kaynak grupları** ana menüden.

1. Üzerinde **kaynak grupları** sekmesine **myResourceGroup** Terraform tarafından oluşturulan kaynakları görüntülemek için.
  ![Sanal makine ölçek kümesi ağ kaynakları](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Ekle

Bu bölümde, aşağıdaki kaynaklara şablonuna eklemek nasıl öğrenin:

- Bir Azure yük dengeleyici ve uygulama hizmet ve bu makalenin önceki bölümlerinde yapılandırılmış ortak IP adresi eklemek için kuralları
- Bir Azure arka uç adresi havuzu ve yük dengeleyiciye atayın 
- Uygulama tarafından kullanılan ve yük dengeleyici üzerinde yapılandırılmış bir sistem durumu araştırma bağlantı noktası 
- Bu makalenin önceki bölümlerinde dağıtılan VNET üzerinde çalışan yük dengeleyicinin arkasındaki bir defada bir sanal makine ölçek kümesi
- [Nginx](http://nginx.org/) kullanarak sanal makine ölçek düğümlerde [bulut init](http://cloudinit.readthedocs.io/en/latest/).

Bulut Kabuğu'nda aşağıdaki adımları gerçekleştirin:

1. Açık `vmss.tf` yapılandırma dosyası.

  ```bash
  vi vmss.tf
  ```

1. Dosyanın sonuna gidin ve girin bir anahtar seçerek ekleme modu.

1. Dosyanın sonuna aşağıdaki kodu yapıştırın:

  ```JSON
  resource "azurerm_lb" "vmss" {
    name                = "vmss-lb"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    frontend_ip_configuration {
      name                 = "PublicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.vmss.id}"
    }

    tags = "${var.tags}"
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
      name              = "osdisk"
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
      admin_username       = "${var.admin_user}"
      admin_password       = "${var.admin_password}"
      custom_data          = "${file("web.conf")}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
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

    tags = "${var.tags}"
}
  ```

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Adlı bir dosya oluşturun `web.conf` ölçek kümesinin parçası olan sanal makineleri için bulut init yapılandırma olarak hizmet vermek için. 

    ```bash
    vi web.conf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

  ```JSON
  #cloud-config
  packages:
    - nginx
  ```

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Açık `variables.tf` yapılandırma dosyası.

  ```bash
  vi variables.tf
  ```

1. Dosyanın sonuna gidin ve girin bir anahtar seçerek ekleme modu.

1. Dağıtım, dosyanın sonuna aşağıdaki kodu yapıştırılarak Özelleştir:

  ```JSON
  variable "application_port" {
      description = "The port that you want to expose to the external load balancer"
      default     = 80
  }

  variable "admin_user" {
      description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
      default     = "azureuser"
  }

  variable "admin_password" {
      description = "Default password for admin account"
  }
  ``` 

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Sanal makine ölçek kümesi dağıtımı görselleştirmek için bir Terraform planı oluşturun. (Seçtiğiniz yanı sıra konumun kaynaklarınız için bir parola belirtmeniz gerekir.)

  ```bash
  terraform plan
  ```

  Komutunun çıktısını aşağıdaki ekran görüntüsüne benzer olmalıdır:

  ![Sanal makine ölçek kümesi oluşturmasına çıktı](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Azure'da yeni kaynaklar dağıtın.

  ```bash
  terraform apply 
  ```

  Komutunun çıktısını aşağıdaki ekran görüntüsüne benzer olmalıdır:

  ![Kaynak grubu Terraform sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Bir tarayıcı açın ve komut tarafından döndürülen FQDN bağlanın. 

  ![FQDN için tarama sonuçları](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Bir SSH jumpbox Ekle
Bir SSH *jumpbox* , "aracılığıyla ağ üzerindeki diğer sunuculara erişmek için atlama" tek bir sunucudur. Bu adımda, aşağıdaki kaynaklara yapılandırın:

- Sanal makine ölçek kümesi aynı alt ağda bir ağ arabirimi (veya jumpbox) bağlı.

- Bir sanal makine bu ağ arabirimi ile bağlı. Bu 'jumpbox' Uzaktan erişilebilir. Bağlantı kurulduktan sonra herhangi bir ölçek kümesindeki sanal makinelerin SSH kullanabilirsiniz.

1. Açık `vmss.tf` yapılandırma dosyası.

  ```bash
  vi vmss.tf
  ```

1. Dosyanın sonuna gidin ve girin bir anahtar seçerek ekleme modu.

1. Dosyanın sonuna aşağıdaki kodu yapıştırın:

  ```JSON
  resource "azurerm_public_ip" "jumpbox" {
    name                         = "jumpbox-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    public_ip_address_allocation = "static"
    domain_name_label            = "${random_string.fqdn.result}-ssh"
    tags                         = "${var.tags}"
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

    tags = "${var.tags}"
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
      admin_username = "${var.admin_user}"
      admin_password = "${var.admin_password}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags = "${var.tags}"
  }
  ```

1. Açık `output.tf` yapılandırma dosyası.

  ```bash
  vi output.tf
  ```

1. Dosyanın sonuna gidin ve girin bir anahtar seçerek ekleme modu.

1. Dağıtım tamamlandığında, ana bilgisayar jumpbox adını görüntülemek için dosyanın sonuna aşağıdaki kodu yapıştırın:

  ```
  output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
  }
  ```

1. Çıkış Esc tuşuna seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Jumpbox dağıtın.

  ```bash
  terraform apply 
  ```

Dağıtım tamamlandıktan sonra kaynak grubu içeriğini aşağıdaki ekran görüntüsünde gösterilen benzer:

![Kaynak grubu Terraform sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> Bir parolayla oturum yeteneği üzerinde jumpbox devre dışı bırakılır ve, dağıttığınız sanal makine ölçek kümesi. Sanal makineler erişmek için SSH ile oturum açın.

## <a name="environment-cleanup"></a>Ortam temizleme 

Bu öğreticide oluşturulan Terraform kaynaklarını silmek için bulut kabuğundan aşağıdaki komutu girin:

```bash
terraform destroy
```

Yok etme işleminin tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Terraform bir Azure sanal makine ölçek kümesi oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz. Azure üzerinde Terraform hakkında daha fazla bilgi edinmenize yardımcı olması için bazı ek kaynaklar aşağıda verilmiştir: 

 [Microsoft.com Terraform Hub](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcı belgeleri](http://aka.ms/terraform)  
 [Terraform Azure sağlayıcı kaynağı](http://aka.ms/tfgit)  
 [Terraform Azure modülleri](http://aka.ms/tfmodules)