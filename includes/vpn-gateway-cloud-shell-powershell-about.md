---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/25/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b878d54f0f52768459dbfc810e47d294b9c8d996
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50097775"
---
Bu makalede PowerShell cmdlet'lerini kullanır. Cmdlet'leri çalıştırmak için Azure Cloud Shell, kullanabileceğiniz ücretsiz bir etkileşimli Kabuk. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. **Kopyala**’ya tıklayarak kodu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın. Cloud Shell’i başlatmanın birkaç yolu vardır:

|  |   |
|-----------------------------------------------|---|
| Kod bloğunun sağ üst köşesindeki **Deneyin**’e tıklayın. | ![Bu makaledeki Cloud Shell](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Cloud Shell’i tarayıcınızda açın. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Azure portalının sağ üst kısmındaki menüde yer alan **Cloud Shell** düğmesine tıklayın. | [![Portalda Cloud Shell](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

Azure Cloud Shell'i kullanmak istemiyorsanız, PowerShell'i yerel olarak bunun yerine yükleyebilirsiniz. PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yüklediğinizden emin olun. PowerShell cmdlet'leri sık sık güncelleştirilir ve genelde en yeni özelliklerin işlevselliğine sahip olmak için PowerShell cmdlet'lerinizi güncelleştirmeniz gerekir. PowerShell cmdlet’lerinizi güncelleştirmezseniz belirtilen değerler başarısız olabilir. Yerel olarak çalışan PowerShell sürümünü bulmak için 'Get-Module - ListAvailable AzureRM' cmdlet'ini kullanın. Yükseltmeniz gerekirse bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).