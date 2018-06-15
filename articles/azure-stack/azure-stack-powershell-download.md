---
title: Azure yığın araçları Github'dan indirdiğinizde | Microsoft Docs
description: Azure yığın ile çalışmak için gerekli olan araçlar yükleyeceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: a116b7a048ff95ca601a65633cdc63f98fefee9d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075298"
---
# <a name="download-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan yükleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

**AzureStack Araçları** yönetme ve dağıtma kaynakları Azure yığınına için PowerShell modülleri barındıran bir GitHub depo. VPN bağlantısı kurmak planlıyorsanız, bu PowerShell modülleri Azure yığın Geliştirme Seti ya da Windows tabanlı bir dış istemci indirebilirsiniz. Bu araçları edinmek için GitHub deposunu kopyalayın veya karşıdan **AzureStack Araçları** aşağıdaki komut dosyası çalıştırarak klasörü:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>Modüller tarafından sağlanan işlevselliği

**AzureStack Araçları** depo aşağıdaki işlevler için Azure yığınına destek PowerShell modülleri içerir:  

| İşlev | Açıklama | Bu modül kimin kullanabilir miyim? |
| --- | --- | --- |
| [Bulut özellikleri](user/azure-stack-validate-templates.md) | Bir Bulut Bulut özelliklerini almak için bu modülü kullanın. Örneğin, bu modül kullanarak API sürümü ve Azure Resource Manager kaynakları gibi bulut özelliklerini alabilir. Ayrıca VM uzantıları Azure yığını ve Azure bulut için bu modül kullanarak alabilirsiniz. | Bulut operatörleri ve kullanıcılar |
| [Azure yığını için kaynak yöneticisi İlkesi](user/azure-stack-policy-module.md) | Azure yığın olarak aynı sürüm oluşturma ve hizmet kullanılabilirliği olan bir Azure aboneliği veya bir Azure kaynak grubu yapılandırmak için bu modülü kullanın. | Bulut operatörleri ve kullanıcılar |
| [Azure ile kaydedin](azure-stack-register.md) | Geliştirme Seti örneğinizi Azure ile kaydetmek için bu modülü kullanın. Kaydolduktan sonra Azure Market öğesi indirmek ve bunları Azure yığınında kullanın. | Bulut operatörleri |
| [Azure yığın dağıtım](azure-stack-run-powershell-script.md) | Dağıtma ve Azure yığın sanal sabit disk (VHD) görüntüsü kullanarak yeniden dağıtmak için Azure yığın ana bilgisayarı hazırlamak için bu modülü kullanın. | Bulut operatörleri|
| [Azure yığınına bağlanma](azure-stack-connect-powershell.md) | Azure yığınına VPN bağlantısı yapılandırmak için bu modülü kullanın. | Bulut operatörleri ve kullanıcılar |
| [Şablon Doğrulayıcı](user/azure-stack-validate-templates.md) | Varolan veya yeni bir şablon Azure yığınına dağıtılabilir olmadığını doğrulamak için bu modülü kullanın. | Bulut operatörleri ve kullanıcılar|


## <a name="next-steps"></a>Sonraki adımlar
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)   
* [Azure yığın Geliştirme Seti için bir VPN üzerinden Bağlan](azure-stack-connect-azure-stack.md)  
