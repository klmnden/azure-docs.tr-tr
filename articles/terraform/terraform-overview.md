---
title: Terraform'u Azure ile birlikte kullanma
description: Terraform kullanarak sürümüne giriş ve Azure altyapı dağıtımı.
services: terraform
ms.service: azure
keywords: terraform, devops, genel bakış, planlama, uygulama, otomatikleştirme
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/31/2018
ms.openlocfilehash: 8b5ad3f060e2c8760014d7fc1990b971976388f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884452"
---
# <a name="terraform-with-azure"></a>Terraform ve Azure

[Hashicorp Terraform](https://www.terraform.io/), bulut altyapısı sağlamak ve yönetmek için kullanılan bir açık kaynak araçtır. Bu hizmet altyapıyı sanal makineler, depolama hesapları ve ağ arabirimleri gibi bulut kaynaklarının topolojisini anlatan yapılandırma dosyaları olarak kodlar. Terraform'un komut satırı arabirimi (CLI), yapılandırma dosyalarını Azure'a veya diğer desteklenen bulutlara dağıtmak ve sürüm oluşturmak için basit bir mekanizma sunar.

Bu makalede Azure altyapısını yönetmek için Terraform'u kullanmanın avantajları anlatılmaktadır.

## <a name="automate-infrastructure-management"></a>Altyapı yönetimini otomatikleştirin.

Terraform'un şablon tabanlı yapılandırma dosyaları Azure kaynaklarını yinelenebilir ve tahmin edilebilir bir şekilde tanımlamanızı, sağlamanızı ve yapılandırmanızı sağlar. Altyapıyı otomatikleştirmek birçok avantaja sahiptir:

- Altyapıyı dağıtma ve yönetme aşamalarındaki insan hatası ihtimalini azaltır.
- Birbirinin aynı geliştirme, test ve üretim ortamları oluşturmak için aynı şablonu birçok kez dağıtabilir.
- Geliştirme ve test ortamlarını istek üzerine oluşturarak geliştirme maliyetini azaltır.

## <a name="understand-infrastructure-changes-before-they-are-applied"></a>Altyapı değişikliklerini uygulanmadan önce anlama 

Kaynak topolojisi araçları daha karmaşık hale geldikçe altyapıda gerçekleştirilen değişikliklerin anlamının ve etkisinin anlaşılması zor olabilir.

Terraform, kullanıcıların altyapı değişikliklerini dağıtılmadan önce doğrulamasını ve önizlemesini sağlayan bir komut satırı arabirimi (CLI) sunar. Altyapı değişikliklerini güvenli ve üretken bir şekilde gerçekleştirmenin birçok avantajı vardır:
- Ekip üyeleri önerilen değişiklikleri ve etkisini daha hızlı anlayarak daha verimli bir şekilde çalışabilir.
- İstenmeden yapılan değişiklikler geliştirme sürecinin erken dönemlerinde fark edilebilir


## <a name="deploy-infrastructure-to-multiple-clouds"></a>Altyapı birden fazla buluta dağıtılabilir

Terraform, benzer yapıların Azure'a ve ek bulut sağlayıcılarına veya şirket içi veri merkezlerine dağıtıldığı çoklu bulut senaryoları için popüler bir araçtır. Bu araç geliştiricilerin aynı araçları ve yapılandırma dosyalarını kullanarak birden fazla bulut sunucusundaki altyapıyı yönetmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Terraform ve avantajlarına genel bir bakış elde ettiğinize göre, aşağıdaki önerilen adımlara geçebilirsiniz:

- [Terraform'u yükleyip Azure'u kullanacak şekilde yapılandırarak](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure) başlayın.
- [Terraform'u kullanarak bir Azure sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-create-complete-vm)
- [Terraform için Azure Resource Manager modülünü](https://www.terraform.io/docs/providers/azurerm/) keşfedin 