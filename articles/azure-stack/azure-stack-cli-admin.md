---
title: Azure Stack kullanıcıları için Azure CLI'yi etkinleştirme | Microsoft Docs
description: Platformlar arası komut satırı arabirimi (CLI) yönetmek ve Azure Stack'te kaynakları dağıtmak için kullanmayı öğrenin
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
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: c2827a4badd61aeb8de556795834dee39769e85e
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554512"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure Stack kullanıcıları için Azure CLI'yi etkinleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Böylece kendi geliştirme makinelerinde Azure CLI'yi kullanabilirsiniz, Azure Stack kullanıcıları için CA kök sertifikasını sağlayabilirsiniz. Kullanıcılarınızın CLI aracılığıyla kaynaklarını yönetmek için sertifika gerekir.

* **Azure Stack CA kök sertifikasını** kullanıcılar Azure Stack geliştirme Seti'ni dışında bir iş istasyonundan CLI kullanıyorsanız gereklidir.  

* **Sanal makine diğer uç nokta** "UbuntuLTS" veya "bir görüntü yayımcısı, teklif, SKU ve sürüm Vm'leri dağıtırken tek bir parametre başvuran Win2012Datacenter," gibi bir diğer ad sağlar.  

Aşağıdaki bölümlerde, bu değerleri almak nasıl açıklanmaktadır.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını dışarı aktarma

Azure Stack CA kök sertifikasını Geliştirme Seti ve Geliştirme Seti ortamında çalışan bir kiracı sanal makine bulabilirsiniz. PEM biçiminde Azure Stack kök sertifikasını dışarı aktarmak için Geliştirme Seti veya Kiracı sanal makine için oturum açın ve aşağıdaki betiği çalıştırın:

```powershell
$label = "<Your Azure Stack CA root certificate name>"
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

Azure Stack operatörlerinin sanal makine diğer dosyasını barındıran bir genel olarak erişilebilir uç ayarlamanız gerekir. Sanal makine diğer dosyasının bir görüntü için ortak bir ad sağlar bir JSON dosyasıdır. Bir Azure CLI parametresi olarak bir sanal makine dağıtıldığında bu adı daha sonra belirtilir.  

Bir diğer ad dosyasına bir giriş eklemeden önce emin olun, [görüntüleri Azure Market'te indirme](azure-stack-download-azure-marketplace-item.md), veya [kendi özel görüntünüzü yayımlanan](azure-stack-add-vm-image.md). Özel bir görüntü yayımlarsanız, yayımlama sırasında belirtilen yayımcı, teklif, SKU ve sürüm bilgileri not edin. Marketten bir görüntü varsa kullanarak bilgileri görüntüleyebilir ```Get-AzureVMImage``` cmdlet'i.  

A [örnek diğer ad dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) birçok ortak görüntü ile diğer adları kullanılabilir. Bu bir başlangıç noktası olarak kullanabilirsiniz. Bu dosyada burada CLI istemcilerinize erişebileceği bir boşluk barındırın. Bir dosyayı blob depolama hesabında barındırmak ve URL'yi Kullanıcılarınızla paylaşın yoludur:

1. İndirme [örnek dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) github'dan.
2. Azure Stack'te yeni bir depolama hesabı oluşturun. Tamamlandığında, yeni bir blob kapsayıcı oluşturun. "Genel" erişim ilkesi ayarlayın  
3. JSON dosyasını yeni kapsayıcısına yükleyin. Tamamlandığında, blobun URL'sini blob adı seçerek ve ardından URL'yi blob özelliklerini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure CLI ile dağıtma](azure-stack-deploy-template-command-line.md)
- [PowerShell ile bağlanma](azure-stack-connect-powershell.md)
- [Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
