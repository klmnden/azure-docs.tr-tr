---
title: Bir Azure sanal makine ölçek oluşturmak için Terraform'u kullanın ayarlayın
description: Bağlı diskleri yapılandırmak için Terraform ve Azure sanal makine ölçek kümesi bir sanal ağ ile tam ve yönetilen sürüm kullanmayla ilgili öğretici
keywords: terraform, devops, sanal makine, Azure, Ölçek kümesi, ağ, depolama, modüller
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/04/2018
ms.topic: article
ms.openlocfilehash: 5922bad24c50a9d315aae42ce11a33801b9dbcaf
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971842"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek oluşturmak için Terraform'u kullanın ayarlayın

[Azure sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets) burada sanal makine örneği sayısını otomatik olarak artırabilir, veya içinde tanımlanmış bir zamanlamaya veya talebe yanıt olarak azaltmak dengeli sanal makineler oluşturmak ve bir grup özdeş, yönetmek izin yükleyin. 

Bu öğreticide, şunların nasıl kullanılacağını [Azure Cloud Shell](/azure/cloud-shell/overview) aşağıdaki görevleri gerçekleştirmek için:

> [!div class="checklist"]
> * Terraform dağıtımı ayarlama
> * Değişkenleri ve çıktılar Terraform dağıtım için kullanın. 
> * Oluşturun ve ağ altyapısı dağıtma
> * Oluşturma ve bir sanal makine ölçek kümesini dağıtmak ve ağ ekleme
> * Oluşturma ve SSH aracılığıyla Vm'lerine bağlanmak için bir Sıçrama kutusu dağıtma

> [!NOTE]
> Bu makalede kullanılan yapılandırma dosyalar, Terraform en son sürümünü [Github deposunu harika Terraform](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terraform'u yükleme**: makaledeki yönergeleri izleyerek [Terraform ve Azure erişimi yapılandırma](/azure/virtual-machines/linux/terraform-install-configure)

- **SSH anahtar çifti oluşturma**: SSH anahtar çifti, makaledeki yönergeleri yoksa [oluşturmak ve azure'da Linux VM'ler için SSH ortak ve özel anahtar çifti kullanmak nasıl](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Dizin yapısı oluşturma

1. Gözat [Azure portalında](http://portal.azure.com).

1. Açık [Azure Cloud Shell'i](/azure/cloud-shell/overview). Daha önce bir ortam seçmediyseniz, seçin **Bash** ortamınızı olarak.

    ![Cloud Shell isteminde](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Dizinleri `clouddrive` dizin.

    ```bash
    cd clouddrive
    ```

1. Adlı bir dizin oluşturmak `vmss`.

    ```bash
    mkdir vmss
    ```

1. Dizinleri yeni dizine değiştirin:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>Değişkenleri tanımları dosya oluşturma
Bu bölümde, Terraform ile oluşturulan kaynakları özelleştirme değişkenleri tanımlayın.

Azure Cloud Shell içinde aşağıdaki adımları gerçekleştirin:

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

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Çıkış tanımları dosyası oluşturma
Bu bölümde, dağıtımdan sonra çıktı tanımlayan dosyası oluşturun.

Azure Cloud Shell içinde aşağıdaki adımları gerçekleştirin:

1. Adlı bir dosya oluşturun `output.tf`.

    ```bash
    vi output.tf
    ```

1. Ekleme modu seçerek t anahtarı girin.

1. Aşağıdaki kod, sanal makineler için tam etki alanı adı (FQDN) kullanıma sunmak için düzenleyicisine yapıştırın. :

  ```JSON
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
  ```

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Ağ altyapısını bir şablonda tanımlamak
Bu bölümde, aşağıdaki ağ altyapısında yeni bir Azure kaynak grubu oluşturun: 

  - 10.0.0.0/16 adres alanı ile bir sanal ağ (VNET) 
  - Bir alt ağ adres alanı 10.0.2.0/24 ile
  - İki ortak IP adresi. Bir sanal makine ölçek kümesi, yük dengeleyicinin SSH Sıçrama kutusuna bağlanmak için kullanılan diğer tarafından kullanılır.

Azure Cloud Shell içinde aşağıdaki adımları gerçekleştirin:

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

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

  ```bash
  :wq
  ```

## <a name="provision-the-network-infrastructure"></a>Ağ altyapısını sağlama
Yapılandırma dosyalarını (.tf) oluşturduğunuz dizininden Azure Cloud Shell'i kullanarak aşağıdaki adımları gerçekleştirin:

1. Terraform başlatın.

  ```bash
  terraform init 
  ```

1. Tanımlanan altyapısını Azure'a dağıtmak için aşağıdaki komutu çalıştırın.

  ```bash
  terraform apply
  ```

  Terraform sizden bir "Konum" değeri **konumu** değişken tanımlanmış `variables.tf`, ancak hiçbir zaman ayarlanır. "Batı Enter'ı seçerek ve ardından ABD" gibi herhangi bir geçerli konumu - girebilirsiniz. (Boşluk herhangi bir değer etrafında parantez kullanın).

1. Terraform tanımlandığı gibi çıktı yazdıran `output.tf` dosya. Aşağıdaki ekran görüntüsünde gösterildiği gibi FQDN biçimini alır &lt;kimliği >.&lt; Konum >. cloudapp.azure.com. Kimlik değerini hesaplanan bir değerdir ve Terraform çalıştırırken sağladığınız değerin konumdur.

  ![Sanal makine ölçek kümesi genel IP adresi için tam etki alanı adı](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. Azure portal menüsünde seçin **kaynak grupları** ana menüden.

1. Üzerinde **kaynak grupları** sekmesinde **myResourceGroup** Terraform ile oluşturulan kaynakları görüntülemek için.
  ![Ağ kaynaklarını sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Ekle

Bu bölümde, aşağıdaki kaynakları şablonuna ekleme öğreneceksiniz:

- Azure yük dengeleyici ve uygulama sunmak ve bu makalede daha önce yapılandırılmış bir genel IP adresini eklemek için kuralları
- Bir Azure arka uç adres havuzu ve yük dengeleyiciye bağlayabilir 
- Bir sistem durumu araştırma bağlantı noktası uygulama tarafından kullanılan ve yük dengeleyicide yapılandırılmış 
- Bu makalede daha önce dağıttığınız VNET üzerinde çalıştırılan yük dengeleyicinin arkasındaki oluşturmaması nedeniyle bir sanal makine ölçek kümesi
- [Ngınx](http://nginx.org/) kullanarak sanal makine ölçek düğümlerinde [cloud-init](http://cloudinit.readthedocs.io/en/latest/).

Cloud Shell'de aşağıdaki adımları gerçekleştirin:

1. Açık `vmss.tf` yapılandırma dosyası.

  ```bash
  vi vmss.tf
  ```

1. Dosyanın sonuna gidip girmek bir anahtar seçerek ekleme modu.

1. Dosyasının sonuna aşağıdaki kodu yapıştırın:

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

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Adlı bir dosya oluşturun `web.conf` ölçek kümesinin bir parçası olan sanal makineleri için cloud-init yapılandırma olarak görev yapacak. 

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

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Açık `variables.tf` yapılandırma dosyası.

  ```bash
  vi variables.tf
  ```

1. Dosyanın sonuna gidip girmek bir anahtar seçerek ekleme modu.

1. Dağıtım, dosyanın sonuna aşağıdaki kodu yapıştırarak Özelleştir:

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

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Sanal makine ölçek kümesi dağıtımı görselleştirmek için bir Terraform planı oluşturun. (Seçtiğiniz yanı sıra konumu kaynaklarınız için bir parola belirtmeniz gerekir.)

  ```bash
  terraform plan
  ```

  Komut çıktısı, aşağıdaki ekran görüntüsüne benzer olmalıdır:

  ![Sanal makine ölçek kümesi oluşturmasını çıkış](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Azure'da yeni kaynaklar dağıtın.

  ```bash
  terraform apply 
  ```

  Komut çıktısı, aşağıdaki ekran görüntüsüne benzer olmalıdır:

  ![Kaynak grubu Terraform sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Bir tarayıcı açın ve komutu tarafından döndürülen FQDN bağlanın. 

  ![FQDN için tarama sonuçları](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Bir SSH Sıçrama Kutusu Ekle
SSH *Sıçrama kutusu* "aracılığıyla ağ üzerindeki diğer sunuculara erişmek için hızlı" tek bir sunucudur. Bu adımda, aşağıdaki kaynakları yapılandırın:

- Sanal makine ölçek kümesi aynı alt ağda bir ağ arabirimi (veya Sıçrama kutusu) bağlı.

- Bu ağ arabirimi ile bir sanal makinenin bağlı. Bu 'Sıçrama' Uzaktan erişilebilir. Bağlantı kurulduktan sonra herhangi bir ölçek kümesindeki sanal makineler için SSH kullanabilirsiniz.

1. Açık `vmss.tf` yapılandırma dosyası.

  ```bash
  vi vmss.tf
  ```

1. Dosyanın sonuna gidip girmek bir anahtar seçerek ekleme modu.

1. Dosyasının sonuna aşağıdaki kodu yapıştırın:

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

1. Dosyanın sonuna gidip girmek bir anahtar seçerek ekleme modu.

1. Dağıtım tamamlandığında, ana bilgisayar adını Sıçrama kutusu görüntülemek için dosyanın sonuna aşağıdaki kodu yapıştırın:

  ```
  output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
  }
  ```

1. Çıkış, Esc tuşunu seçerek modu ekleyin.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Sıçrama kutusu dağıtın.

  ```bash
  terraform apply 
  ```

Dağıtım tamamlandıktan sonra kaynak grubunun içeriğini aşağıdaki ekran görüntüsünde gösterilen benzer:

![Kaynak grubu Terraform sanal makine ölçek kümesi](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> Sıçrama kutusu üzerinde bir parolayla oturum açmak için özelliği devre dışıdır ve dağıtmış olduğunuz sanal makine ölçek kümesi. Sanal makineler erişmek için SSH oturum açın.

## <a name="environment-cleanup"></a>Ortam temizleme 

Bu öğreticide oluşturulan Terraform kaynakları silmek için Cloud shell'e aşağıdaki komutu girin:

```bash
terraform destroy
```

Yok etme işleminin tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure sanal makine ölçek kümesi oluşturmak için Terraform'u kullanmayı öğrendiniz. Azure üzerinde Terraform hakkında daha fazla bilgi edinmenize yardımcı olacak bazı ek kaynaklar aşağıda verilmiştir: 

 [Microsoft.com Terraform Hub](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcısı belgeleri](http://aka.ms/terraform)  
 [Terraform Azure sağlayıcısı kaynağı](http://aka.ms/tfgit)  
 [Terraform Azure modülleri](http://aka.ms/tfmodules)