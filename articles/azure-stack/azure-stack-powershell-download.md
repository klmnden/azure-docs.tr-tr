---
title: "Azure yığın araçları Github'dan indirdiğinizde | Microsoft Docs"
description: "Azure yığın ile çalışmak için gerekli olan araçlar yükleyeceğinizi öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: E4DF77FA-F468-42B5-B44F-F10ED8049171
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: e730df58c54f7804d77b052a699d827bb82e5204
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="download-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan yükleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

**AzureStack Araçları** yönetme ve dağıtma kaynakları Azure yığınına için PowerShell modülleri barındıran bir GitHub depo. VPN bağlantısı kurmak planlıyorsanız, bu PowerShell modülleri Azure yığın Geliştirme Seti ya da Windows tabanlı bir dış istemci indirebilirsiniz. Bu araçları edinmek için GitHub deposunu kopyalayın veya karşıdan **AzureStack Araçları** aşağıdaki komut dosyası çalıştırarak klasörü:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
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
| [Azure yığın işlem yönetimi](azure-stack-add-vm-image.md) | Bu modül eklemek veya Azure yığın marketten VM görüntüsünü kaldırmak için kullanın. | Bulut operatörleri |
| [Azure yığın altyapı Yönetimi](https://github.com/Azure/AzureStack-Tools/blob/master/Infrastructure/README.md) | Azure yığın altyapı VM'ler, uyarılar, güncelleştirmeler ve benzeri yönetmek için bu modülü kullanın. |  Bulut operatörleri|
| [Azure yığını için kaynak yöneticisi İlkesi](user/azure-stack-policy-module.md) | Azure yığın olarak aynı sürüm oluşturma ve hizmet kullanılabilirliği olan bir Azure aboneliği veya bir Azure kaynak grubu yapılandırmak için bu modülü kullanın. | Bulut operatörleri ve kullanıcılar |
| [Azure ile kaydedin](azure-stack-register.md) | Geliştirme Seti örneğinizi Azure ile kaydetmek için bu modülü kullanın. Kaydolduktan sonra Azure Market öğesi indirmek ve bunları Azure yığınında kullanın. | Bulut operatörleri |
| [Azure yığın dağıtım](azure-stack-run-powershell-script.md) | Dağıtma ve Azure yığın sanal sabit disk (VHD) görüntüsü kullanarak yeniden dağıtmak için Azure yığın ana bilgisayarı hazırlamak için bu modülü kullanın. | Bulut operatörleri|
| [Azure yığınına bağlanma](azure-stack-connect-powershell.md) | PowerShell aracılığıyla Azure yığın örneğine bağlanmak için ve Azure yığınına VPN bağlantısı yapılandırmak için bu modülü kullanın. | Bulut operatörleri ve kullanıcılar |
| [Azure yığın Hizmet Yönetimi](azure-stack-create-offer.md) | İşlem, Azure depolama, ağ ve anahtar kasası Hizmetleri sınırsız kotaları varsayılan Kiracı teklif oluşturmak için bu modülü kullanın.   | Bulut operatörleri|
| [Şablon Doğrulayıcı](user/azure-stack-validate-templates.md) | Varolan veya yeni bir şablon Azure yığınına dağıtılabilir olmadığını doğrulamak için bu modülü kullanın. | Bulut operatörleri ve kullanıcılar|


## <a name="next-steps"></a>Sonraki adımlar
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)   
* [Azure yığın Geliştirme Seti için bir VPN üzerinden Bağlan](azure-stack-connect-azure-stack.md)  
