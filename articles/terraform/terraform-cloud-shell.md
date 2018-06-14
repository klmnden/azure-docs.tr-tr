---
title: Azure bulut kabuğunda Terraform kullanın
description: Terraform Azure bulut kabuğu ile kimlik doğrulaması ve şablon yapılandırmasını basitleştirmek için kullanın.
keywords: terraform, devops, ölçeklendirme ayarlayın, sanal makine, ağ, depolama, modüller
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/19/2017
ms.topic: article
ms.openlocfilehash: 5157066086f1bdfa580c1946942bda4505e48935
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29121534"
---
# <a name="terraform-cloud-shell-development"></a>Terraform bulut Kabuk geliştirme 

Terraform harika macOS Terminal veya Bash gibi bir Bash komut satırından Windows veya Linux üzerinde çalışır. Terraform çalıştıran biri Bash yapılandırmalarında deneyimi [Azure bulut Kabuk](/azure/cloud-shell/overview) , geliştirme döngüsü hızlandırmak için bazı benzersiz avantajları vardır.

Bu kavramları makalede bulut yardımcı özellikleri Azure'a dağıtma Terraform komut dosyaları yazmak Kabuk kapsar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgilerini yapılandırma

Terraform yüklenir ve bulut Kabuğu'nda hemen kullanılabilir. Terraform komut dosyaları, herhangi bir ek yapılandırma olmadan altyapısını yönetmek için bulut Kabuğu oturum açıldığında Azure ile kimlik doğrulaması. Otomatik kimlik doğrulama el ile bir Active Directory Hizmet sorumlusu oluşturma ve Azure Terraform sağlayıcı değişkenleri yapılandırmak için gereken atlar.


## <a name="using-modules-and-providers"></a>Modüller ve sağlayıcıları kullanma

Azure Terraform modülleri erişmek ve kaynakları Azure aboneliğinizde değişiklikler yapmak için kimlik bilgileri gerektirir. Bulut Kabuğu'nda çalışırken Azure Terraform modülleri bulut Kabuğu'nda kullanmak için komut dosyaları için aşağıdaki kodu ekleyin:

```tf
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Gerekli değerler bulut Kabuk geçirir `azurerm` sağlayıcısı üzerinden herhangi birini kullanırken, ortam değişkenleri `terraform` CLI komutları.

## <a name="other-cloud-shell-developer-tools"></a>Diğer bulut Kabuk geliştirici araçları

Dosyaları ve Kabuk durumları bulut Kabuk oturumlar arasında Azure Depolama'da kalıcı olmasını sağlar. Kullanım [Azure Storage Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer) kopyalayıp dosyaları bulut Kabuk yerel bilgisayarınızdan yüklemek için.

Azure CLI 2.0 bulut Kabuğu'nda kullanılabilir ve yapılandırmaları test ve sonra iş denetimi için harika bir araçtır bir `terraform apply` veya `terraform destroy` tamamlar.


## <a name="next-steps"></a>Sonraki adımlar

[Modül Kayıt Defteri'ni kullanarak küçük bir VM küme oluşturma](terraform-create-vm-cluster-module.md)
[özel HCL'yi kullanarak küçük bir VM küme oluşturma](terraform-create-vm-cluster-with-infrastructure.md)
