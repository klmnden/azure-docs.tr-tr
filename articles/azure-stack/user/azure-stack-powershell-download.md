---
title: "Azure yığın araçları Github'dan indirdiğinizde | Microsoft Docs"
description: "Azure yığın ile çalışmak için gereken araçları yükleyeceğinizi öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 28F360AD-789A-488D-965F-FC6E6CCF3329
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: mabrigg
ms.openlocfilehash: d4f8a8d73f8e2ea321cb6cc1deda2301033b249d
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="download-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan yükleyin

AzureStack araçları yönetmek ve kaynakları Azure yığınına dağıtmak için kullanabileceğiniz bir PowerShell modülleri barındıran GitHub deposuna olur. Karşıdan yükle ve VPN bağlantısı kurmak planlıyorsanız, bu PowerShell modülleri Azure yığın Geliştirme Seti ya da windows tabanlı bir dış istemci kullanın. Bu araçları elde etmek için GitHub deposunu kopyalayın veya AzureStack Araçlar klasörünü aşağıdaki komut dosyasını çalıştırarak yükleyebilirsiniz:

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
