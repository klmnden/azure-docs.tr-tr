---
title: Azure Stack operatör olarak PowerShell ile bağlanma | Microsoft Docs
description: Azure Stack operatör olarak PowerShell ile bağlanma hakkında bilgi edinin
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
ms.date: 10/24/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: bbd20df7f002d6072110e3b94887bac24ce13cd2
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50087449"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>PowerShell ile Azure Stack operatör bağlanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack tekliflerini, planları, kotalar ve Uyarılar oluşturmak gibi kaynakları yönetmek için PowerShell kullanmak için yapılandırabilirsiniz. Bu konuda işleci ortam yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar'nden ya da çalıştırmak [Geliştirme Seti](.\asdk\asdk-connect.md#connect-with-rdp) ya da Eğer bir Windows tabanlı dış istemciden [ASDK VPN aracılığıyla bağlı](.\asdk\asdk-connect.md#connect-with-vpn). 

 - Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](azure-stack-powershell-install.md).  
 - İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>İşleç ortamı yapılandırmak ve Azure Stack için oturum açın

PowerShell ile Azure Stack operatörü ortam yapılandırın. Aşağıdaki komut dosyalarından birini çalıştırın: Azure AD'ye Kiracıadı, GraphAudience uç nokta ve ArmEndpoint değerleri kendi ortamınızdaki yapılandırmayı ile değiştirin.

````PowerShell  
    # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
    # To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Admin Resource Manager endpoint for your environment>"

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
````

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Artık her şeyi kendinizi, Kurulum, Azure Stack içindeki kaynakları oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. Adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın **MyResourceGroup**.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

 - [Şablonları Azure Stack için geliştirme](user/azure-stack-develop-templates.md)
 - [Şablonları PowerShell ile dağıtma](user/azure-stack-deploy-template-powershell.md)