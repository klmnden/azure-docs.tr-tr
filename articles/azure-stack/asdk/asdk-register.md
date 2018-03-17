---
title: Azure ile ASDK kaydedin | Microsoft Docs
description: "Azure yığın Market dağıtım ve kullanım raporlama etkinleştirmek için Azure ile kaydetmek açıklar."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 9e2dbc71f6424b87945e346a42c86d4cde7f740e
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin
Market öğesi Azure'dan karşıdan yüklemek ve ticari veri geri Microsoft'a raporlama ayarlamak için Azure ile Azure yığın Geliştirme Seti (ASDK) yüklemenizi kaydedebilirsiniz. Market dağıtım ve kullanım raporlama gibi önemli Azure yığın işlevselliğini sınamak için sağladığından kayıt önerilir. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz. Ancak, ASDK kullanıcılar bunlar rapor herhangi kullanım için ücretlendirilmezsiniz.


## <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin 
Azure ile ASDK kaydetmek için aşağıdaki adımları izleyin.

> [!NOTE]
> Tüm adımları ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. ASDK için Geliştirme Seti ana bilgisayar olmasıdır. 

Azure ile ASDK kaydetmek için aşağıdaki adımları kullanmadan önce Azure yığın PowerShell yüklü ve açıklandığı gibi Azure yığın araçları indirilen emin [dağıtım sonrası yapılandırmaya](asdk-post-deploy.md) makalesi. 

1. PowerShell konsolunu yönetici olarak açın.  

2. (Azure aboneliğinizi ve yerel ASDK yükleme için oturum açmak gerekir) ASDK yüklemenizi Azure ile kaydetmek için aşağıdaki PowerShell komutlarını çalıştırın:

    ```PowerShell
    # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
    Set-AzsRegistration `
        -CloudAdminCredential $CloudAdminCred `
        -PrivilegedEndpoint AzS-ERCS01 `
        -BillingModel Development

3. When the script completes, you should see this message: **Your environment is now registered and activated using the provided parameters.**

    ![](media/asdk-register/1.PNG) 

## Verify the registration was successful
Follow these steps to verify that the ASDK registration with Azure was successful.

1. Sign in to the [Azure Stack administration portal](https://adminportal.local.azurestack.external).

2. Click **Marketplace Management** > **Add from Azure**.

    ![](media/asdk-register/2.PNG) 

3. If you see a list of items available from Azure, your activation was successful.

    ![](media/asdk-register/3.PNG) 

## Next steps
[Add an Azure Stack marketplace item](asdk-marketplace-item.md)