---
title: Azure Cloud Shell'deki Bash hizmetinde gelen Terraform ile dağıtma | Microsoft Docs
description: Azure Cloud Shell'deki Bash hizmetinde gelen Terraform ile dağıtma
services: Azure
documentationcenter: ''
author: tomarchermsft
manager: routlaw
tags: azure-cloud-shell
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: tarcher
ms.openlocfilehash: a08a4e7df6cf0493ab1aa6aced1abf888a61072a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62119121"
---
# <a name="deploy-with-terraform-from-bash-in-azure-cloud-shell"></a>Azure Cloud Shell'deki Bash hizmetinde gelen Terraform ile dağıtma
Bu makalede, bir kaynak grubu oluşturma işlemini gösterir. [Terraform AzureRM sağlayıcısı](https://www.terraform.io/docs/providers/azurerm/index.html). 

[Hashicorp Terraform](https://www.terraform.io/) düzenlendi, gözden takım üyeleri arasında paylaşılan ve sürümü tutulan bildirim temelli yapılandırma dosyalarına API'leri kod oluşturur bir açık kaynak araçtır. Microsoft AzureRM sağlayıcısı, AzureRM API'ler aracılığıyla Azure Resource Manager tarafından desteklenen kaynaklarla etkileşim kurmak için kullanılır. 

## <a name="automatic-authentication"></a>Otomatik kimlik doğrulaması
Varsayılan olarak, Cloud Shell'deki Bash hizmetinde Terraform yüklenir. Ek olarak, Cloud Shell aracılığıyla Terraform Azure modüllerini kaynakları dağıtmak için varsayılan Azure CLI aboneliğinizi otomatik olarak doğrular.

Terraform ayarlanmış varsayılan Azure CLI abonelik kullanır. Varsayılan abonelikleri güncelleştirmek için şunu çalıştırın:

```azurecli-interactive
az account set --subscription mySubscriptionName
```

## <a name="walkthrough"></a>Kılavuz
### <a name="launch-bash-in-cloud-shell"></a>Cloud shell'de Bash'i başlatma
1. Cloud Shell, tercih edilen konumdan başlatın
2. Tercih edilen aboneliğinizi ayarlandığını doğrulayın

```azurecli-interactive
az account show
```

### <a name="create-a-terraform-template"></a>Terraform şablon oluşturma
Tercih edilen bir metin düzenleyiciyle Main.tf adlı yeni bir Terraform şablonu oluşturun.

```
vim main.tf
```

Kopyalama/aşağıdaki kod Cloud shell'e yapıştırın.

```
resource "azurerm_resource_group" "myterraformgroup" {
    name = "myRgName"
    location = "West US"
}
```

Dosyanızı kaydedin ve metin düzenleyicinize çıkın.

### <a name="terraform-init"></a>Terraform başlatma
Çalıştırarak başlayın `terraform init`.

```
justin@Azure:~$ terraform init

Initializing provider plugins...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.azurerm: version = "~> 0.2"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

[Terraform init komutunu](https://www.terraform.io/docs/commands/init.html) Terraform yapılandırma dosyalarını içeren bir çalışma dizini başlatmak için kullanılır. `terraform init` Yeni bir Terraform yapılandırmasını yazma ya da mevcut bir sürüm denetiminden kopyalama çalıştırılması gereken ilk komut bir komuttur. Bu komut birden çok kez çalıştırmak güvenlidir.

### <a name="terraform-plan"></a>Terraform plan
Terraform şablonla tarafından oluşturulması için gereken kaynakları Önizleme `terraform plan`.

```
justin@Azure:~$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.demo
      id:       <computed>
      location: "westus"
      name:     "myRGName"
      tags.%:   <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

[terraform plan komutu](https://www.terraform.io/docs/commands/plan.html), yürütme planı oluşturmak için kullanılır. Terraform açıkça devre dışı bırakılmamışsa, yenileme gerçekleştirir ve ardından hangi eylemleri yapılandırma dosyalarında belirttiğiniz istenen duruma gelmesi için gerekli olan belirler. Plan kullanılarak kaydedilebilir-çıkış ve ardından terraform için sağlanan yalnızca önceden planlanmış eylemleri yürütülür emin olmak için geçerlidir.

### <a name="terraform-apply"></a>Terraform apply
İle Azure kaynaklarını hazırlama `terraform apply`.

```
justin@Azure:~$ terraform apply
azurerm_resource_group.demo: Creating...
  location: "" => "westus"
  name:     "" => "myRGName"
  tags.%:   "" => "<computed>"
azurerm_resource_group.demo: Creation complete after 0s (ID: /subscriptions/mySubIDmysub/resourceGroups/myRGName)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

[Terraform Uygula komutu](https://www.terraform.io/docs/commands/apply.html) istenen durum yapılandırması ile erişmek için gerekli değişiklikleri uygulamak için kullanılır.

### <a name="verify-deployment-with-azure-cli"></a>Azure CLI ile dağıtımı doğrulama
Çalıştırma `az group show -n myRgName` kaynak sağlama başarılı olduğunu doğrulamak için.

```azcliinteractive
az group show -n myRgName
```

### <a name="clean-up-with-terraform-destroy"></a>Temizleme terraform ile yok.
Temiz kaynak grubu ile oluşturulan [Terraform destroy komutunu](https://www.terraform.io/docs/commands/destroy.html) Terraform oluşturulan altyapıyı temizlemek için.

```
justin@Azure:~$ terraform destroy
azurerm_resource_group.demo: Refreshing state... (ID: /subscriptions/mySubID/resourceGroups/myRGName)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - azurerm_resource_group.demo


Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

azurerm_resource_group.demo: Destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 10s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 20s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 30s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 40s elapsed)
azurerm_resource_group.demo: Destruction complete after 45s

Destroy complete! Resources: 1 destroyed.
```

Terraform ile bir Azure kaynağı başarıyla oluşturdunuz. Cloud Shell hakkında daha fazla bilgi için sonraki adımlar ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
[Terraform Azure sağlayıcısı hakkında bilgi edinin](https://www.terraform.io/docs/providers/azurerm/#)<br>
[Bash cloud Shell hızlı başlangıçta](quickstart.md)
