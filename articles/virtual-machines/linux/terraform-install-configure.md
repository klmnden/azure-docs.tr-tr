---
title: Terraform'u yükleme ve Azure ile kullanmak için yapılandırma | Microsoft Docs
description: Azure kaynakları oluşturmak için Terraform'u yükleme ve yapılandırma hakkında bilgi edinin
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
ms.date: 06/19/2018
ms.author: echuvyrov
ms.openlocfilehash: 71cf07b227a75e53119f2f35e79ccd7926b551e7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60418872"
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>VM'ler ve diğer altyapı Azure'a sağlamak için Terraform'u yükleme ve yapılandırma
 
Terraform tanımlamak, Önizleme ve bulut altyapısını kullanarak dağıtmak için kolay bir yol sağlar bir [basit bir şablon oluşturma dil](https://www.terraform.io/docs/configuration/syntax.html). Bu makalede, azure'daki kaynaklara sağlama Terraform kullanmak için gerekli adımları açıklar.

Terraform ile Azure kullanma hakkında daha fazla bilgi için ziyaret [Terraform Hub](/azure/terraform).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Varsayılan olarak yüklü Terraform [Cloud Shell](/azure/terraform/terraform-cloud-shell). Terraform yerel olarak yüklemeyi seçerseniz, sonraki adımı tamamlamak, aksi takdirde devam [Azure Terraform erişimi ayarlama](#set-up-terraform-access-to-azure).

## <a name="install-terraform"></a>Terraform'u yükleme

Terraform, yüklenecek [indirme](https://www.terraform.io/downloads.html) ayrı işletim sisteminize uygun paketi yükleme dizini. İndirme için genel bir yolu da tanımlamanız gerekir tek bir yürütülebilir dosya içerir. Linux ve Mac'de yolunu ayarlamak yönergeler için Git [bu Web sayfasını](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Windows üzerinde yolunu ayarlamak yönergeler için Git [bu Web sayfasını](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).

Yol yapılandırmanızı doğrulamak `terraform` komutu. Aşağıdaki örnek çıktıda gösterildiği gibi kullanılabilir Terraform seçeneklerin bir listesi gösterilir:

```bash
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Azure Terraform erişimi ayarlama

Terraform sağlama kaynakları Azure'a etkinleştirmek için oluşturun bir [Azure AD hizmet sorumlusu](/cli/azure/create-an-azure-service-principal-azure-cli). Hizmet sorumlusu, Terraform betiklerinizi Azure aboneliğinizdeki sağlamak için verir.

Birden çok Azure aboneliğiniz varsa, önce hesabınızla sorgu [az hesabı show](/cli/azure/account#az-account-show) abonelik listesi kimliği ve Kiracı kimlik değerlerini almak için:

```azurecli-interactive
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Seçili bir aboneliği kullanmak için abonelik için bu oturumla ayarlamak [az hesabı kümesi](/cli/azure/account#az-account-set). Ayarlama `SUBSCRIPTION_ID` döndürülen değerini tutacak ortam değişkeni `id` alanını kullanmak istediğiniz aboneliği:

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Şimdi, Terraform ile kullanım için bir hizmet sorumlusu oluşturabilirsiniz. Kullanım [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac), ayarlayıp *kapsam* aboneliğinize aşağıdaki gibi:

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

`appId`, `password`, `sp_name`, Ve `tenant` döndürülür. Not `appId` ve `password`.

## <a name="configure-terraform-environment-variables"></a>Terraform ortam değişkenlerini yapılandırma

Terraform, Azure AD hizmet sorumlusu adını kullanmak üzere yapılandırmak için ardından tarafından kullanılan aşağıdaki ortam değişkenlerini ayarlamak [Azure Terraform modüllerini](https://registry.terraform.io/modules/Azure). Ayrıca, bir Azure bulut Azure genel diğer ile çalışıyorsanız, ortamı ayarlayabilirsiniz.

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

Bu değişkenlerini ayarlamak için aşağıdaki örnek Kabuk betiği kullanabilirsiniz:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>Bir örnek betiği çalıştırma

Bir dosya oluşturun `test.tf` bir boş dizin ve aşağıdaki betiği yapıştırın.

```tf
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Dosyayı kaydedin ve ardından Terraform dağıtımı başlatın. Bu adım, bir Azure kaynak grubu oluşturmak için gerekli Azure modüllerini yükler.

```bash
terraform init
```

Çıktı aşağıdaki örneğe benzer:

```bash
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Terraform betiğiyle tamamlanması için eylemleri önizleyebilirsiniz `terraform plan`. Hazır olduğunuzda kaynak grubu oluşturmak Terraform'u planınız şu şekilde uygulanır:

```bash
terraform apply
```

Çıktı aşağıdaki örneğe benzer:

```bash
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Terraform yüklü veya Cloud Shell, Azure kimlik bilgilerini yapılandırmak ve Azure aboneliğinizde kaynaklarını oluşturmaya başlamak için kullanılır. Azure'da daha eksiksiz bir Terraform dağıtımı oluşturmak için şu makaleye bakın:

> [!div class="nextstepaction"]
> [Terraform ile Azure VM oluşturma](terraform-create-complete-vm.md)
