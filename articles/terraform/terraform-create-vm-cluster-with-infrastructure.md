---
title: VM kümesinin Terraform ve HCL ile oluşturma
description: Azure yük dengeleyici ile Linux sanal makine kümesi oluşturmak için Terraform ve HashiCorp yapılandırma dili (HCL) kullanın
keywords: terraform, devops, sanal makine, ağ, modüller
author: tomarcher
manager: routlaw
ms.service: virtual-machines-linux
ms.custom: devops
ms.topic: article
ms.date: 11/13/2017
ms.author: tarcher
ms.openlocfilehash: 2435d694e6a1671a234d02f90860e5cafe98c2df
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
ms.locfileid: "24518809"
---
# <a name="create-a-vm-cluster-with-terraform-and-hcl"></a>VM kümesinin Terraform ve HCL ile oluşturma

Bu öğretici kullanılarak küçük bilgi işlem küme oluşturma gösterir [HashiCorp yapılandırma dil](https://www.terraform.io/docs/configuration/syntax.html) (HCL). İki Linux VM'ler için bir yük dengeleyici yapılandırması oluşturur bir [kullanılabilirlik kümesi](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)ve tüm gerekli ağ kaynaklarına.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure kimlik doğrulamasını kurma
> * Terraform yapılandırma dosyası oluşturma
> * Terraform başlatma
> * Bir Terraform yürütme planı oluşturma
> * Terraform yürütme planı Uygula

## <a name="1-set-up-azure-authentication"></a>1. Azure kimlik doğrulamasını kurma

> [!NOTE]
> Varsa, [Terraform ortam değişkenlerini kullanma](/azure/virtual-machines/linux/terraform-install-configure#set-environment-variables), ya da Bu öğretici Çalıştır [Azure bulut Kabuk](terraform-cloud-shell.md), bu bölüm atlayın.

Bu bölümde, bir Azure hizmet sorumlusu ve güvenlik sorumlusu kimlik bilgilerini içeren iki Terraform yapılandırma dosyaları oluşturur.

1. [Bir Azure AD hizmet sorumlusu ayarlamak](/azure/virtual-machines/linux/terraform-install-configure#set-up-terraform-access-to-azure) Terraform sağlama kaynaklara Azure'da etkinleştirmek için. Asıl oluşturulurken, abonelik kimliği, Kiracı, AppID ve parola değerlerini not edin.

2. Bir komut istemi açın.

3. Terraform dosyalarınızı depolamak boş bir dizin oluşturun.

4. Değişkenleri bildirimlerinizde tutan yeni bir dosya oluşturun. Bu dosya ile gibi herhangi bir şey adlandırabilirsiniz bir `.tf` uzantısı.

5. Değişken bildirimi dosyanıza aşağıdaki kodu kopyalayın:

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

6. Terraform değişkenleri için değerleri içeren yeni bir dosya oluşturun. Terraform değişken dosyanızın adı yaygındır `terraform.tfvars` Terraform adına sahip tüm dosyaları otomatik olarak yükler gibi `terraform.tfvars` (veya bir düzeni aşağıdaki `*.auto.tfvars`) varsa, geçerli dizin. 

7. Aşağıdaki kod, değişkenleri dosyasına kopyalayın. Yer tutucuları şu şekilde değiştirdiğinizden emin olun: için `subscription_id`, çalıştırıldığında, belirtilen Azure abonelik kimliği kullanmak `az account set`. İçin `tenant_id`, kullanın `tenant` döndürülen değer `az ad sp create-for-rbac`. İçin `client_id`, kullanın `appId` döndürülen değer `az ad sp create-for-rbac`. İçin `client_secret`, kullanın `password` döndürülen değer `az ad sp create-for-rbac`.

  ```tf
  subscription_id = "<azure-subscription-id>"
  tenant_id = "<tenant-returned-from-creating-a-service-principal>"
  client_id = "<appId-returned-from-creating-a-service-principal>"
  client_secret = "<password-returned-from-creating-a-service-principal>"
  ```

## <a name="2-create-a-terraform-configuration-file"></a>2. Terraform yapılandırma dosyası oluşturma

Bu bölümde, kaynak tanımlarında altyapınızı içeren bir dosya oluşturun.

1. Adlı yeni bir dosya oluşturun `main.tf`. 

2. Örnek kaynak tanımları yeni oluşturulan aşağıdaki kopyalama `main.tf` dosyası: 

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

## <a name="3-initialize-terraform"></a>3. Terraform başlatma 

[Terraform init komutu](https://www.terraform.io/docs/commands/init.html) Terraform yapılandırma dosyalarını - önceki bölümlerde ile oluşturulan dosyaları içeren bir dizin başlatmak için kullanılır. Her zaman çalışması gerektiğini `terraform init` yeni Terraform yapılandırması yazdıktan sonra komutu. 

> [!TIP]
> `terraform init` Komuttur ıdempotent, art arda aynı sonucu üretilirken çağrılabilir anlamına gelir. Bu nedenle, ortak bir ortamda çalışıyorsanız ve yapılandırma dosyalarını değiştirildiğini düşünüyorsanız, bu her zaman çağırmak için iyi bir fikirdir `terraform init` çalıştırma veya bir planı uygulamadan önce komutu.

Terraform başlatmak için aşağıdaki komutu çalıştırın:

  ```cmd
  terraform init
  ```

  ![Terraform başlatılıyor](media/terraform-create-vm-cluster-with-infrastructure/terraform-init.png)

## <a name="4-create-a-terraform-execution-plan"></a>4. Bir Terraform yürütme planı oluşturma

[Terraform planı komutu](https://www.terraform.io/docs/commands/plan.html) yürütme planı oluşturmak için kullanılır. Yürütme planı oluşturmak için Terraform tüm toplayan `.tf` geçerli dizindeki dosyaları. 

Burada yapılandırma değişebilir yürütme planı oluşturduğunuz zaman ve saat arasında ortak bir ortamda çalışıyorsanız yürütme planı uygulamak için kullanmanız gereken [terraform planı komutunun-çıkış parametresi](https://www.terraform.io/docs/commands/plan.html#out-path)yürütme planı bir dosyaya kaydetmek için. Bir tek kişi ortamında çalışıyorsanız, aksi takdirde atlayabilirsiniz `-out` parametresi.

Terraform değişkenleri dosyanızın adı olup olmadığını `terraform.tfvars` ve onu IU `*.auto.tfvars` desenini kullanarak dosya adı belirtmeniz gerekir [terraform planı komutunun - var dosya parametresi](https://www.terraform.io/docs/commands/plan.html#var-file-foo) çalışırken`terraform plan`komutu.

İşleme sırasında `terraform plan` komutu, Terraform bir yenileme gerçekleştirir ve hangi eylemleri Yapılandırma dosyalarınızda belirtilen istenen durumu elde etmek gerekli olduğunu belirler.

Yürütme planınız kaydetmek ihtiyacınız yoksa, aşağıdaki komutu çalıştırın:

  ```cmd
  terraform plan
  ```

Yürütme planınız kaydetmek gerekiyorsa, aşağıdaki komutu çalıştırın (değiştirme &lt;yolu > istenen çıkış yolu ile yer tutucu):

  ```cmd
  terraform plan -out=<path>
  ```

![Terraform yürütme planı oluşturma](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)

## <a name="5-apply-the-terraform-execution-plan"></a>5. Terraform yürütme planı Uygula

Bu öğreticinin son adımı kullanmaktır [terraform Uygula komutu](https://www.terraform.io/docs/commands/apply.html) tarafından oluşturulan eylemler kümesi uygulamak için `terraform plan` komutu.

En son yürütme planı uygulamak istiyorsanız, aşağıdaki komutu çalıştırın:

  ```cmd
  terraform apply
  ```

Daha önce kaydettiğiniz yürütme planı uygulamak istiyorsanız, aşağıdaki komutu çalıştırın (değiştirme &lt;yolu > kaydedilmiş yürütme planı içeren yolu ile yer tutucu):

  ```cmd
  terraform apply <path>
  ```

![Terraform yürütme planı uygulama](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Sonraki adımlar

- Listesine göz [Azure Terraform modülleri](https://registry.terraform.io/modules/Azure)
- Oluşturma bir [sanal makine ölçek kümesi ile Terraform](terraform-create-vm-scaleset-network-disks-hcl.md)