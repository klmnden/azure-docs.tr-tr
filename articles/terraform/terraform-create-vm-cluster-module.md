---
title: "Azure üzerinde bir VM küme oluşturmak için Terraform modülleri kullanın"
description: "Azure'da Windows sanal makine küme oluşturmak için Terraform modülleri kullanmayı öğrenin"
keywords: "terraform, devops, sanal makine, ağ, modüller"
author: rloutlaw
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure
ms.date: 10/19/2017
ms.custom: devops
ms.author: routlaw
ms.openlocfilehash: e33aef252413eeb243b03543f171d5f1e2385b48
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-vm-cluster-with-terraform-using-the-module-registry"></a>Modül Kayıt Defteri'ni kullanarak Terraform ile VM küme oluşturma

Bu makalede küçük bir VM küme ile Terraform oluşturmada size anlatılmaktadır [Azure işlem Modülü](https://registry.terraform.io/modules/Azure/compute/azurerm/1.0.2). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]
> * Azure ile kimlik doğrulaması ayarlama
> * Terraform şablonu oluşturma
> * Plan değişikliklerle Görselleştirme
> * VM küme oluşturmak için yapılandırmasını Uygula

Terraform hakkında daha fazla bilgi için bkz: [Terraform belgelerine](https://www.terraform.io/docs/index.html).

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
module mycompute {
    source = "Azure/compute/azurerm"
    resource_group_name = "myResourceGroup"
    location = "East US 2"
    admin_password = "ComplxP@assw0rd!"
    vm_os_simple = "WindowsServer"
    remote_port = "3389"
    nb_instances = 2
    public_ip_dns = ["unique_dns_name"]
    vnet_subnet_id = "${module.network.vnet_subnets[0]}"
}

module "network" {
    source = "Azure/network/azurerm"
    location = "East US 2"
    resource_group_name = "myResourceGroup"
}

output "vm_public_name" {
    value = "${module.mycompute.public_ip_dns_name}"
}

output "vm_public_ip" {
    value = "${module.mycompute.public_ip_address}"
}

output "vm_private_ips" {
    value = "${module.mycompute.network_interface_private_ip}"
}
```

Çalıştırma `terraform init` yapılandırma dizininizde. En az 0.10.6 Terraform sürümünü kullanarak aşağıdaki çıkış şunları gösterir:

![Terraform başlatma](media/terraformInitWithModules.png)

## <a name="visualize-the-changes-with-plan"></a>Plan değişikliklerle Görselleştirme

Çalıştırma `terraform plan` şablonu tarafından oluşturulan sanal makine altyapısı önizlemek için.

![Terraform Plan](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)


## <a name="create-the-virtual-machines-with-apply"></a>Sanal makineler Uygula ile oluşturma

Çalıştırma `terraform apply` Azure Vm'lerinde sağlamak için.

![Terraform Uygula](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Sonraki adımlar

- Listesine göz [Azure Terraform modülleri](https://registry.terraform.io/modules/Azure)
- Oluşturma bir [sanal makine ölçek kümesi ile Terraform](terraform-create-vm-scaleset-network-disks-hcl.md)
