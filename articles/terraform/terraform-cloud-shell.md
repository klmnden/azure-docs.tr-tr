---
title: Terraform ile Azure Cloud Shell'i birlikte kullanma
description: Kimlik doğrulaması ve şablon yapılandırma adımlarını basitleştirmek için Terraform ile Azure Cloud Shell'i birlikte kullanın.
services: terraform
ms.service: azure
keywords: terraform, devops, ölçek kümesi, sanal makine, ağ, depolama alanı, modüller
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 10/19/2017
ms.openlocfilehash: ab2fd0c7fa546201d6eb19f727053a9ac54fa854
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66169922"
---
# <a name="terraform-cloud-shell-development"></a>Terraform Cloud Shell geliştirme 

Terraform macOS Terminali veya Windows ya da Linux üzerinde Bash gibi bir Bash komut satırından verimli bir şekilde çalıştırılabilir. Terraform yapılandırmalarınızı [Azure Cloud Shell](/azure/cloud-shell/overview)'in Bash deneyiminde çalıştırma, geliştirme döngünüzü hızlandıracak benzersiz avantajlara sahiptir.

Kavramları kapsayan bu makale, Azure'a dağıtım yapan Terraform betikleri yazmanıza yardımcı olan Cloud Shell özelliklerini kapsamaktadır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Terraform, Cloud Shell'de yüklüdür ve kullanıma hazırdır. Terraform betikleri Cloud Shell oturumu açıldığında otomatik olarak Azure kimlik doğrulamasından geçer ve ek yapılandırmaya gerek kalmadan altyapının yönetilmesini sağlar. Otomatik kimlik doğrulaması sayesinde el ile Active Directory hizmet sorumlusu oluşturma ve Azure Terraform sağlayıcısı değişkenlerini yapılandırma ihtiyacı ortadan kalkar.


## <a name="using-modules-and-providers"></a>Modülleri ve Sağlayıcıları kullanma

Azure Terraform modüllerinin Azure aboneliğinizdeki kaynaklara erişmesi ve değişiklik yapması için kimlik bilgilerinin kullanılması gerekir. Cloud Shell'de çalışırken Azure Terraform modüllerini Cloud Shell ile kullanmak için betiklerinize aşağıdaki kodu ekleyin:

```tf
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell, `terraform` CLI komutlarından biri kullanıldığında `azurerm` sağlayıcısı için gerekli değerleri ortam değişkenleriyle iletir.

## <a name="other-cloud-shell-developer-tools"></a>Diğer Cloud Shell geliştirici araçları

Azure Depolama dosyaları ve kabuk durumları Cloud Shell oturumları arasında kalıcı olur. [Azure Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer)'ni kullanarak Cloud Shell'den yerel bilgisayarınızla dosya kopyalama ve yükleme işlemleri gerçekleştirebilirsiniz.

Cloud Shell'de bulunan Azure CLI, yapılandırma testi ve `terraform apply` veya `terraform destroy` işlemlerinin ardından kontrol gerçekleştirmek için kullanışlı bir araçtır.


## <a name="next-steps"></a>Sonraki adımlar

[Modül Kayıt Defterini kullanarak küçük bir VM kümesi oluşturma](terraform-create-vm-cluster-module.md)
[Özel HCL kullanarak küçük bir VM kümesi oluşturma](terraform-create-vm-cluster-with-infrastructure.md)
