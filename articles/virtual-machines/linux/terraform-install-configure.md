---
title: VM'ler ve diğer Azure altyapısında sağlayacak Terraform yükleyip | Microsoft Docs
description: Yükleyin ve Azure kaynaklarını oluşturmak için Terraform yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/23/2017
ms.author: echuvyrov
ms.openlocfilehash: dada9c70eef2adb2704e276a5401509581e37538
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29399177"
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Yükleme ve Azure'da VM'ler ve diğer altyapıya sağlamak için Terraform yapılandırma
 
Terraform tanımlamak, Önizleme ve bulut altyapısını kullanarak dağıtmak için kolay bir yol sağlayan bir [basit şablon dili](https://www.terraform.io/docs/configuration/syntax.html). Bu makalede Azure sağlama kaynaklara Terraform kullanmak için gereken adımları açıklar. 

> [!TIP]
Azure ile Terraform kullanma hakkında daha fazla bilgi için ziyaret [Terraform Hub](/azure/terraform). Varsayılan olarak yüklü Terraform [bulut Kabuk](/azure/terraform/terraform-cloud-shell). Bulut Kabuğu'nu kullanarak, bu belge yükleme/kurulum bölümünü atlayabilirsiniz.

## <a name="install-terraform"></a>Terraform yükleyin

Terraform, yüklemek için [karşıdan](https://www.terraform.io/downloads.html) bir ayrı bir işletim sistemi için uygun paket yükleme dizini. Yükleme için genel bir yol da tanımlamanız gerekir bir tek bir yürütülebilir dosya içerir. Linux ve Mac'de yolunu ayarlama hakkında daha fazla yönerge için Git [bu Web sayfası](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Windows üzerinde yolunu ayarlama hakkında daha fazla yönerge için Git [bu Web sayfası](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). 

Yol yapılandırmanızı doğrulayın `terraform` komutu. Çıkış olarak kullanılabilir Terraform seçeneklerin bir listesini görmeniz gerekir:

```bash
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Azure Terraform erişimi ayarlama

Yapılandırma [bir Azure AD hizmet sorumlusu](/cli/azure/create-an-azure-service-principal-azure-cli) Terraform sağlama kaynaklara Azure'da etkinleştirmek için. Hizmet sorumlusu Azure aboneliğinizde sağlama kaynakları için kimlik bilgilerini kullanarak Terraform komut verir.

Azure AD uygulaması ve bir Azure AD hizmet sorumlusu oluşturmak için birkaç yolu vardır. Kolay ve hızlı şekilde bugün Azure CLI 2.0 kullanmaktır hangi [indirin ve Windows, Linux veya Mac yükleyin](/cli/azure/install-azure-cli).

Aşağıdaki komutu gönderdikten Azure aboneliğinizi yönetmek oturum açın:

   `az login`

Birden çok Azure aboneliğiniz varsa, bunların ayrıntıları tarafından döndürülen `az login` komutu. Ayarlama `SUBSCRIPTION_ID` döndürülen değeri tutmak için ortam değişkeni `id` kullanmak istediğiniz abonelikten alan. 

Bu oturum için kullanmak istediğiniz aboneliği ayarlayın.

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Abonelik kimliği ve Kiracı kimliği değerleri almak için hesap sorgu.

```azurecli-interactive
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Ardından, farklı kimlik bilgileri için Terraform oluşturun.

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

AppID, parola, sp_name ve Kiracı döndürülür. Uygulama kimliği ve parola not edin.

Kimlik bilgilerinizi test etmek için yeni bir Kabuğu'nu açın ve sp_name, şifresi ve Kiracı için döndürülen değerlerini kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

## <a name="configure-terraform-environment-variables"></a>Terraform ortam değişkenlerini yapılandırın

Kiracı kimliği, abonelik kimliği, istemci kimliği ve istemci hizmet asıl gizli Azure kaynakları oluşturulurken kullanılacak Terraform yapılandırın. Ortamında, bir Azure bulut Azure genel diğer ile çalışıyorsanız de ayarlayabilirsiniz. Tarafından otomatik olarak kullanılan aşağıdaki ortam değişkenlerini ayarlama [Azure Terraform modülleri](https://registry.terraform.io/modules/Azure).

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID
- ARM_ENVIRONMENT

Bu değişkenleri ayarlamak için bu örnek Kabuk betiği kullanabilirsiniz:

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

Bir dosya oluşturun `test.tf` boş dizin ve aşağıdaki komut dosyası yapıştırın. 

```tf
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Dosyayı kaydedin ve ardından çalıştırın `terraform init`. Bu komut, bir Azure kaynak grubu oluşturmak için gereken Azure modüllerini yükler. Aşağıdaki çıkış bakın:

```
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Komut dosyası ile Önizleme `terraform plan`ve ardından oluşturun `testResouceGroup` kaynak grubuyla `terraform apply`:

```
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

Terraform yükledikten ve Azure aboneliğinize altyapısı dağıtma başlayabilmeniz için Azure kimlik yapılandırılmış. Ardından bir boş Azure kaynak grubu oluşturarak yüklemenizi test.

> [!div class="nextstepaction"]
> [Bir Azure VM Terraform ile oluşturma](terraform-create-complete-vm.md)

