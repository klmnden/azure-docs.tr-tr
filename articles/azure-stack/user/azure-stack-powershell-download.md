---
title: Azure Stack araçları Github'dan indirin. | Microsoft Docs
description: Azure Stack ile çalışması için gereken araçları indirme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.author: mabrigg
ms.openlocfilehash: 9c1e4abe50b06db58a4ca05a99e1ae4a531b2294
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187460"
---
# <a name="download-azure-stack-tools-from-github"></a>Github'dan Azure Stack araçları indirin

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

AzureStack Araçlar, yönetmek ve Azure Stack'e kaynakları dağıtmak için kullanabileceğiniz bir PowerShell modülleri barındıran bir GitHub deposu'dır.

## <a name="download-targets"></a>Hedefleri yükle

İndirin ve bu PowerShell modülleri için Azure Stack geliştirme Seti'ni kullanın veya Windows tabanlı bir dış istemci kullanan bir VPN bağlantısı.

## <a name="how-to-get-the-tools"></a>Araçları edinme

Bu araçları edinmek için AzureStack araçları GitHub deposunu kopyalayın veya AzureStack Araçlar klasörüne aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

```PowerShell
# Change directory to the root directory
cd \

# Download the tools archive
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory
cd AzureStack-Tools-master

```

## <a name="functionalities-provided-by-the-modules"></a>Modüller tarafından sağlanan işlevleri

AzureStack Araçlar deposu, aşağıdaki işlevleri desteklemek için Azure Stack PowerShell modüllerini içerir:

| İşlev | Açıklama | Kimler bu modül kullanabilir miyim? |
| --- | --- | --- |
| [Bulut özellikleri](https://github.com/Azure/AzureStack-Tools/tree/master/CloudCapabilities) | Bulut'u bulut özelliklerini almak için bu modülü kullanın. Örneğin, Azure Stack ve bu modülü kullanarak Azure bulut için API sürümü, Azure Resource Manager kaynaklarını, VM uzantılarını vb. gibi bulut özellikleri alabilirsiniz. | Bulut yöneticileri ve kullanıcılar. |
| [Azure Stack için Resource Manager İlkesi](azure-stack-policy-module.md) | Azure Stack olarak aynı sürüm oluşturma ve hizmet kullanılabilirliği olan bir Azure aboneliği veya bir Azure kaynak grubu yapılandırmak için bu modülü kullanın. | Bulut yöneticileri ve kullanıcılar |
| [Azure Stack'e bağlanma](azure-stack-connect-azure-stack.md) | Bu modülü, PowerShell aracılığıyla bir Azure Stack örneğine bağlamak ve Azure Stack için VPN bağlantısı yapılandırmak için kullanın. | Bulut yöneticileri ve kullanıcılar |
| [Şablon Doğrulayıcı](azure-stack-validate-templates.md) | Var olan veya yeni bir şablonu Azure Stack'e yerleştirilip yerleştirilemediğini doğrulamak için bu modülü kullanın. | Bulut yöneticileri ve kullanıcılar |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
- [Azure Stack Geliştirme Seti için bir VPN üzerinden bağlanan](azure-stack-connect-azure-stack.md)
