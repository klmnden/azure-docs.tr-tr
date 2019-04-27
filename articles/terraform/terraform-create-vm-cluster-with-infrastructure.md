---
title: Terraform ve HCL ile VM kümesi oluşturma
description: Terraform ve HashiCorp Yapılandırma Dili (HCL) ile Azure'da yük dengeleyiciye sahip bir Linux sanal makine kümesi oluşturma
services: terraform
ms.service: azure
keywords: terraform, devops, sanal makine, ağ, modüller
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 11/13/2017
ms.openlocfilehash: a0358859d6f806a94c529bae2eb6fa9d1ab82963
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884835"
---
# <a name="create-a-vm-cluster-with-terraform-and-hcl"></a>Terraform ve HCL ile VM kümesi oluşturma

Bu öğreticide [HashiCorp Yapılandırma Dili](https://www.terraform.io/docs/configuration/syntax.html) (HCL) ile küçük bir işlem kümesi oluşturma adımları gösterilmiştir. Yapılandırma yük dengeleyici, bir [kullanılabilirlik kümesinde](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) iki Linux VM ve gerekli tüm ağ kaynaklarını oluşturur.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure kimlik doğrulamasını ayarlama
> * Terraform yapılandırma dosyası oluşturma
> * Terraform'u başlatma
> * Terraform yürütme planı oluşturma
> * Terraform yürütme planını uygulama

## <a name="1-set-up-azure-authentication"></a>1. Azure kimlik doğrulamasını ayarlama

> [!NOTE]
> [Terraform ortam değişkenlerini kullanıyorsanız](/azure/virtual-machines/linux/terraform-install-configure) veya bu [Azure Cloud Shell](terraform-cloud-shell.md) öğreticisini çalıştırdıysanız bu bölümü atlayın.

Bu bölümde bir Azure hizmet sorumlusu ve hizmet sorumlusu kimlik bilgilerini içeren iki Terraform yapılandırma dosyası oluşturacaksınız.

1. Terraform'un Azure'da kaynak sağlaması için bir [Azure AD hizmet sorumlusu ayarlayın](/azure/virtual-machines/linux/terraform-install-configure#set-up-terraform-access-to-azure). Hizmet sorumlusunu oluştururken abonelik kimliği, kiracı, uygulama kimliği ve parola değerlerini not edin.

2. Bir komut istemi açın.

3. Terraform dosyalarınızı depolamak için boş bir dizin oluşturun.

4. Değişkenlerinizin bildirimleri için yeni bir dosya oluşturun. Bu dosyaya istediğiniz adı verebilirsiniz ancak sonuna `.tf` uzantısını eklemeniz gerekir.

5. Aşağıdaki kodu değişken bildirim dosyanıza kopyalayın:

   ```tf
   variable subscription_id {}
   variable tenant_id {}
   variable client_id {}
   variable client_secret {}
  
   provider "azurerm" {
      subscription_id = "${var.subscription_id}"
      tenant_id = "${var.tenant_id}"
      client_id = "${var.client_id}"
      client_secret = "${var.client_secret}"
   }
   ```

6. Terraform değişkenlerinin değerlerini için yeni bir dosya oluşturun. Terraform değişken dosyaları genellikle `terraform.tfvars` olarak adlandırılır. Bunun nedeni Terraform'un geçerli dizinde bulunan `terraform.tfvars` adlı (veya `*.auto.tfvars` düzenine sahip) tüm dosyaları otomatik olarak yüklemesidir. 

7. Aşağıdaki kodu değişken dosyanıza kopyalayın. Yer tutucuları gibi değiştirdiğinizden emin olun: İçin `subscription_id`, çalıştırıldığında, belirtilen Azure abonelik kimliği kullanan `az account set`. `tenant_id` için `az ad sp create-for-rbac` komutunun döndürdüğü `tenant` değerini kullanın. `client_id` için `az ad sp create-for-rbac` komutunun döndürdüğü `appId` değerini kullanın. `client_secret` için `az ad sp create-for-rbac` komutunun döndürdüğü `password` değerini kullanın.

   ```tf
   subscription_id = "<azure-subscription-id>"
   tenant_id = "<tenant-returned-from-creating-a-service-principal>"
   client_id = "<appId-returned-from-creating-a-service-principal>"
   client_secret = "<password-returned-from-creating-a-service-principal>"
   ```

## <a name="2-create-a-terraform-configuration-file"></a>2. Terraform yapılandırma dosyası oluşturma

Bu bölümde altyapınız için kaynak tanımlarını içeren dosyayı oluşturacaksınız.

1. `main.tf` adlı yeni bir dosya oluşturun. 

2. Aşağıdaki örnek kaynak tanımlarını yeni oluşturduğunuz `main.tf` dosyasına kopyalayın: 

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

## <a name="3-initialize-terraform"></a>3. Terraform'u başlatma 

[terraform init komutu](https://www.terraform.io/docs/commands/init.html), önceki bölümlerde oluşturduğunuz Terraform yapılandırma dosyalarını içeren bir dizin başlatmak için kullanılır. Yeni bir Terraform yapılandırması yazdıktan sonra `terraform init` komutunu çalıştırmanız önerilir. 

> [!TIP]
> `terraform init` komutu bir kere etkilidir ve tekrar tekrar çağrılarak aynı sonuç elde edilebilir. Bu nedenle bir işbirliğine dayalı bir ortamda çalışıyorsanız ve yapılandırma dosyalarınızın değiştirilmiş olabileceğini düşünüyorsanız planı yürütmeden veya uygulamadan önce `terraform init` komutunu çalıştırmanız yararlı olacaktır.

Terraform'u başlatmak için şu komutu çalıştırın:

  ```cmd
  terraform init
  ```

  ![Terraform'u başlatma](media/terraform-create-vm-cluster-with-infrastructure/terraform-init.png)

## <a name="4-create-a-terraform-execution-plan"></a>4. Terraform yürütme planı oluşturma

[terraform plan komutu](https://www.terraform.io/docs/commands/plan.html), yürütme planı oluşturmak için kullanılır. Terraform, yürütme planı oluşturmak için geçerli dizindeki tüm `.tf` dosyalarını toplar. 

İşbirliğine dayalı bir ortamda çalışıyorsanız ve yürütme planını oluşturduğunuz zamanla yürütme planını uyguladığınız zaman arasında yapılandırmanın değişmiş olma ihtimali varsa [terraform plan komutunun -out parametresini](https://www.terraform.io/docs/commands/plan.html#out-path) kullanarak yürütme planını dosyaya kaydetmeniz gerekir. Tek kişilik bir ortamda çalışıyorsanız `-out` parametresini kullanmanız gerekmez.

Terraform değişkenleri dosyanızın adı `terraform.tfvars` değilse ve `*.auto.tfvars` düzenini kullanmıyorsa `terraform plan` komutunu çalıştırırken [terraform plan komutunun -var-file parametresini](https://www.terraform.io/docs/commands/plan.html#var-file-foo) kullanarak dosya adını belirtmeniz gerekir.

Terraform, `terraform plan` komutunu işlerken yenileme gerçekleştirir ve yapılandırma dosyalarınızda belirtilen istenen duruma ulaşmak için gerekli olan eylemleri belirler.

Yürütme planınızı kaydetmenize gerek yoksa şu komutu çalıştırın:

  ```cmd
  terraform plan
  ```

Yürütme planınızı kaydetmeniz gerekiyorsa şu komutu çalıştırın (&lt;path> yer tutucusunun yerine istediğiniz çıkış yolunu yazın):

  ```cmd
  terraform plan -out=<path>
  ```

![Terraform yürütme planı oluşturma](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)

## <a name="5-apply-the-terraform-execution-plan"></a>5. Terraform yürütme planını uygulama

Bu öğreticinin son adımı [terraform apply komutunu](https://www.terraform.io/docs/commands/apply.html) kullanarak `terraform plan` komutuyla oluşturulan eylem kümesini uygulamaktır.

En güncel yürütme planını uygulamak istiyorsanız şu komutu çalıştırın:

  ```cmd
  terraform apply
  ```

Önceden kaydedilen bir yürütme planını uygulamak istiyorsanız şu komutu çalıştırın (&lt;path> yer tutucusunun yerine kaydedilen yürütme planınızın bulunduğu yolu yazın):

  ```cmd
  terraform apply <path>
  ```

![Terraform yürütme planını uygulama](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Terraform modülleri](https://registry.terraform.io/modules/Azure) listesine göz atma
- [Terraform ile sanal makine ölçek kümesi](terraform-create-vm-scaleset-network-disks-hcl.md) oluşturma
