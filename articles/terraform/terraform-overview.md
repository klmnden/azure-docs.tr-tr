---
title: Azure ile Terraform kullanma
description: "Terraform için vesion kullanmaya giriş ve Azure altyapısı dağıtın."
ms.service: virtual-machines-linux
keywords: "terraform, devops, genel bakış, planlama, uygulamak, otomatikleştirme"
author: binderjoe
ms.author: jbinder
ms.date: 10/19/2017
ms.topic: article
ms.openlocfilehash: 667752d8830cdac5e2338fd3ed7904917123be94
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="terraform-with-azure"></a>Azure ile Terraform

[Hashicorp Terraform](https://www.terraform.io/) sağlama ve bulut altyapısını yönetmek için bir açık kaynaklı bir araçtır. Bu, sanal makineler, depolama hesapları ve ağ arabirimleri gibi bulut kaynaklarını topoloji açıklayan yapılandırma dosyalarını altyapısında kod oluşturur. Terraform'ın komut satırı arabirimi (CLI) dağıtmak için basit bir mekanizma ve sürüm Azure veya desteklenen herhangi bir bulut yapılandırma dosyaları sağlar.

Bu makalede Azure altyapısını yönetmek için Terraform kullanmanın avantajları açıklanmıştır.

## <a name="automate-infrastructure-management"></a>Altyapı Yönetimi otomatikleştirin.

Terraform'ın şablon tabanlı yapılandırma dosyaları tanımlamak, sağlamak ve Azure kaynaklarını yinelenebilir ve tahmin edilebilir bir şekilde yapılandırmanıza olanak sağlar. Otomatik otomatikleştirme altyapı çeşitli avantajları vardır:

- Dağıtma ve altyapısını yönetme insan hataları olasılığını düşürür.
- Aynı şablonu birden çok kez aynı geliştirme, test ve üretim ortamları oluşturma dağıtır.
- İsteğe bağlı oluşturarak geliştirme ve test ortamları maliyetini azaltır.

## <a name="understand-infrastructure-changes-before-they-are-applied"></a>Altyapı değişiklikleri uygulanmadan önce anlama 

Bir kaynak olarak topoloji anlamı anlama karmaşık hale gelir ve altyapı değişikliklerin etkisini zor olabilir.

Terraform bir komut satırı dağıtılmadan önce önizleme altyapı değişiklikleri ve doğrulamak kullanıcı arabirimi (CLI) sağlar. Bir kasada altyapı değişiklikleri Önizleme, verimli bir şekilde çeşitli avantajları vardır:
- Takım üyeleri önerilen değişikliklerin ve etkilerini hızla anlayarak daha etkili bir şekilde işbirliği yapabilir.
- İstenmeyen değişiklikleri geliştirme sürecin başında Yakalanacak


## <a name="deploy-infrastructure-to-multiple-clouds"></a>Birden çok bulut altyapısı dağıtın

Terraform çok bulut senaryolarında, benzer altyapı Azure ve ek bulut sağlayıcılarının veya şirket içi veri merkezleri dağıtıldığı bir popüler aracı seçimdir. Birden çok bulut sağlayıcıları altyapısını yönetmek için aynı araçları ve yapılandırma dosyalarını kullanmak geliştiricilere sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Terraform ve onun avantajlarını genel bir bakış sahip olduğunuza göre önerilen sonraki adımlar şunlardır:

- İle çalışmaya başlama [Terraform yükleme ve Azure kullanacak şekilde yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure).
- [Terraform kullanarak bir Azure sanal makine oluşturun](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-create-complete-vm)
- Araştır [Terraform için Azure Resource Manager modülü](https://www.terraform.io/docs/providers/azurerm/) 