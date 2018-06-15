---
title: Azure yığın araçları Github'dan indirdiğinizde | Microsoft Docs
description: Azure yığın ile çalışmak için gereken araçları yükleyeceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 28F360AD-789A-488D-965F-FC6E6CCF3329
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: a5bc23ee6f986da80630371bafcd8ec80dde3577
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258887"
---
# <a name="download-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan yükleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

AzureStack araçları yönetmek ve kaynakları Azure yığınına dağıtmak için kullanabileceğiniz bir PowerShell modülleri barındıran GitHub deposuna olur.

## <a name="download-targets"></a>Hedef indirme

Karşıdan yükleme ve bu PowerShell modülleri Azure yığın Geliştirme Seti için kullanma ya da Windows tabanlı bir dış istemci kullanan bir VPN bağlantısı.

## <a name="how-to-get-the-tools"></a>Araçlar alma

Bu araçları almak için AzureStack araçları GitHub deposunu kopyalayın veya AzureStack Araçlar klasörünü aşağıdaki komut dosyasını çalıştırarak yükleyebilirsiniz:

```PowerShell
# Change directory to the root directory
cd \

# Download the tools archive
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

## <a name="functionalities-provided-by-the-modules"></a>Modüller tarafından sağlanan işlevler

AzureStack araçları depo aşağıdaki işlevler için Azure yığınına destek PowerShell modülleri içerir:

| İşlev | Açıklama | Bu modül kimin kullanabilir miyim? |
| --- | --- | --- |
| [Bulut özellikleri](azure-stack-validate-templates.md) | Bir Bulut Bulut özelliklerini almak için bu modülü kullanın. Örneğin, Azure yığını ve bu modülü kullanılarak Azure Bulutları için API sürümü, Azure Resource Manager kaynakları, VM uzantıları vb. gibi bulut özelliklerini alabilir. | Bulut Yöneticiler ve kullanıcılar. |
| [Azure yığını için kaynak yöneticisi İlkesi](azure-stack-policy-module.md) | Azure yığın olarak aynı sürüm oluşturma ve hizmet kullanılabilirliği olan bir Azure aboneliği veya bir Azure kaynak grubu yapılandırmak için bu modülü kullanın. | Bulut Yöneticiler ve kullanıcılar |
| [Azure yığınına bağlanma](azure-stack-connect-azure-stack.md) | PowerShell aracılığıyla Azure yığın örneğine bağlanmak için ve Azure yığınına VPN bağlantısı yapılandırmak için bu modülü kullanın. | Bulut Yöneticiler ve kullanıcılar |
| [Şablon Doğrulayıcı](azure-stack-validate-templates.md) | Varolan veya yeni bir şablon Azure yığınına dağıtılabilir olmadığını doğrulamak için bu modülü kullanın. | Bulut Yöneticiler ve kullanıcılar |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
* [Azure yığın Geliştirme Seti için bir VPN üzerinden Bağlan](azure-stack-connect-azure-stack.md)
