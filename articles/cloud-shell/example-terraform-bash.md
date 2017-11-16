---
title: "Azure bulut Kabuğu'nda Bash ile Terraform ile dağıtma | Microsoft Docs"
description: "Azure kaynaklarıyla Bash Terraform dağıtma"
services: Azure
documentationcenter: 
author: tomarcher
manager: routlaw
tags: azure-cloud-shell
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: tarcher
ms.openlocfilehash: c75b5d521dc3eacaf5c5921c35442b1afeb4fa13
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="terraform-and-bash-in-cloud-shell"></a>Terraform ve bulut Kabuk Bash'te
Bu makalede sahip bir kaynak grubu oluşturma ile anlatılmaktadır [Terraform AzureRM sağlayıcı](https://www.terraform.io/docs/providers/azurerm/index.html). 

[Hashicorp Terraform](https://www.terraform.io/) API'leri düzenlenemez, gözden takım üyeleri arasında paylaşılacak ve sürümü tutulan bildirim temelli yapılandırma dosyalarına kod oluşturur bir açık kaynak aracıdır. Microsoft AzureRM sağlayıcısı AzureRM API'leri aracılığıyla Azure kaynak yöneticisi tarafından desteklenen kaynakları ile etkileşim kurmak için kullanılır. 

## <a name="automatic-authentication"></a>Otomatik kimlik doğrulama
Varsayılan olarak, bulut Kabuk Bash'te Terraform yüklenir. Ayrıca, bulut Kabuk otomatik olarak Terraform Azure modülleri kaynaklarına dağıtmak için varsayılan Azure CLI 2.0 aboneliğinizi kimliğini doğrular.

Ayarlanmış varsayılan Azure CLI 2.0 abonelik Terraform kullanır. Varsayılan abonelikleri güncelleştirmek için çalıştırın:

```azurecli-interactive
az account set --subscription mySubscriptionName
```

## <a name="walkthrough"></a>Kılavuz
### <a name="launch-bash-in-cloud-shell"></a>Bulut Kabuğu'nda Bash'i başlatın
1. Tercih edilen konumunuz bulut kabuğundan başlatma
2. Tercih edilen aboneliğinizi ayarlandığını doğrulayın

```azurecli-interactive
az account show
```

### <a name="create-a-terraform-template"></a>Terraform şablonu oluşturma
Tercih edilen metin düzenleyicisiyle Main.tf adlı yeni bir Terraform şablonu oluşturun.

```
vim main.tf
```

Kopyalayıp aşağıdaki kodu bulut kabuğundan yapıştırın.

```
resource "azurerm_resource_group" "myterraformgroup" {
    name = "myRgName"
    location = "West US"
}
```

Dosyanızı kaydedin ve Metin Düzenleyicisi'nden çıkın.

### <a name="terraform-init"></a>Terraform başlatma
Begin çalıştırarak `terraform init`.

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

[Terraform init komutu](https://www.terraform.io/docs/commands/init.html) Terraform yapılandırma dosyalarını içeren bir çalışma dizini başlatmak için kullanılır. `terraform init` Yeni Terraform yapılandırması yazma veya mevcut bir sürüm denetiminden kopyalama sonra çalıştırılması gereken ilk komut bir komuttur. Birden çok kez bu komutu çalıştırmak güvenlidir.

### <a name="terraform-plan"></a>Terraform planı
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

[Terraform planı komutu](https://www.terraform.io/docs/commands/plan.html) yürütme planı oluşturmak için kullanılır. Terraform bir yenileme açıkça devre dışı bırakılmamışsa gerçekleştirir ve hangi eylemleri yapılandırma dosyalarında belirtilen istenen durumu elde etmek için gerekli olan belirler. Plan kullanılarak kaydedilebilir-çıkış ve terraform için sağlanan yalnızca önceden planlanmış eylemleri yürütülme emin olmak için geçerlidir.

### <a name="terraform-apply"></a>Terraform Uygula
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

[Terraform Uygula komutu](https://www.terraform.io/docs/commands/apply.html) istenen yapılandırma durumunu erişmek için gerekli değişiklikleri uygulamak için kullanılır.

### <a name="verify-deployment-with-azure-cli-20"></a>Azure CLI 2.0 ile dağıtımı doğrulama
Çalıştırma `az group show -n myRgName` kaynak sağlama başarılı doğrulanamadı.

```azcliinteractive
az group show -n myRgName
```

### <a name="clean-up-with-terraform-destroy"></a>Temizleme ile terraform yok
Temiz ile oluşturulan kaynak grubu [Terraform destroy komutu](https://www.terraform.io/docs/commands/destroy.html) Terraform oluşturulan altyapıyı temizlenemedi.

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

Bir Azure kaynağı Terraform aracılığıyla başarıyla oluşturdunuz. Bulut kabuğu hakkında bilgi almaya devam etmek için sonraki adımlar ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
[Terraform Azure sağlayıcısı hakkında bilgi edinin](https://www.terraform.io/docs/providers/azurerm/#)<br>
[Bulut Kabuk hızlı başlangıcı bash](quickstart.md)