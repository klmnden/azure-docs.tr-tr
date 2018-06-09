---
title: Azure CLI için Azure yığın kullanıcıları etkinleştir | Microsoft Docs
description: Platformlar arası komut satırı arabirimi (CLI) yönetmek ve kaynakları Azure yığında dağıtmak için nasıl kullanılacağını öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: mabrigg
ms.openlocfilehash: d0103d211608514848da7d789d32d37d8385f33f
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247865"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure CLI için Azure yığın kullanıcıları etkinleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure CLI kullanarak gerçekleştirebilirsiniz herhangi bir Azure yığın işleci özgü görevi değil. Ancak, kullanıcıların kaynaklara CLI aracılığıyla yönetebilmeniz için önce Azure yığın işleçleri bunları aşağıdakilerle sağlamanız gerekir:

* **Azure yığın CA kök sertifikasını** kullanıcılar Azure yığın Geliştirme Seti dışında bir iş istasyonundan CLI kullanıyorsanız gereklidir.  

* **Sanal makine diğer adlar uç noktası** "UbuntuLTS" veya "bir görüntü Yayımlayıcı, teklif, SKU ve sürümü VM'ler dağıtırken tek bir parametre başvuran Win2012Datacenter," gibi bir diğer ad sağlar.  

Aşağıdaki bölümlerde bu değerleri almak nasıl açıklanmaktadır.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Azure yığın CA kök sertifikasını dışarı aktarma

Azure yığın CA kök sertifikasını Geliştirme Seti ve Geliştirme Seti ortamında çalışan bir kiracı sanal makinede kullanılabilir. PEM biçimine Azure yığın kök sertifikayı dışarı aktarmak için Geliştirme Seti veya Kiracı sanal makine için oturum açın ve aşağıdaki komut dosyasını çalıştırın:

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer uç nokta ayarlamayı

Azure yığın işleçleri, sanal makine diğer dosyasını barındıran bir genel olarak erişilebilir uç nokta ayarlamayı ayarlamanız gerekir. Sanal makine diğer dosyası bir görüntü için ortak bir ad sağlayan bir JSON dosyasıdır. Bir Azure CLI parametresi olarak bir VM dağıtıldığında bu adı daha sonra belirtilir.  

Bir diğer ad dosyasına bir giriş eklemeden önce emin olun, [görüntüleri Azure Marketi'nden karşıdan](azure-stack-download-azure-marketplace-item.md), veya [kendi özel görüntünüzü yayımlanan](azure-stack-add-vm-image.md). Özel görüntü yayımlarsanız, yayımlama sırasında belirtilen yayımcı, teklif, SKU ve sürüm bilgileri not edin. Marketten bir görüntü varsa kullanarak bilgileri görüntüleyebilirsiniz ```Get-AzureVMImage``` cmdlet'i.  

A [örnek diğer dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) birçok ortak görüntüsüyle diğer adları kullanılabilir. Bir başlangıç noktası olarak kullanabilirsiniz. Bu dosyada CLI istemcileriniz, burada ulaşabileceği bir alanı ana bilgisayar. Bir blob storage hesabı dosyasında barındırmak ve URL, kullanıcılarla paylaşmak için yoludur:

1. Karşıdan [örnek dosya](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) github'dan.
2. Yeni bir depolama hesabı Azure yığınında oluşturun. Tamamlandığında, yeni blob kapsayıcı oluşturun. "Genel" için erişim ilkesi ayarlama  
3. JSON dosyasını yeni kapsayıcı karşıya yükleyin. Bu yapıldığında, blob adı seçtikten sonra URL blob özelliklerinden seçerek blob URL'si görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI ile şablonlarını dağıtma](azure-stack-deploy-template-command-line.md)

- [PowerShell ile bağlanma](azure-stack-connect-powershell.md)

- [Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
