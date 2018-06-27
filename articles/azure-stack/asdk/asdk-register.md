---
title: Azure ile ASDK kaydedin | Microsoft Docs
description: Azure yığın Market dağıtım ve kullanım raporlama etkinleştirmek için Azure ile kaydetmek açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 08a300d0e2d1565428f282a2073d91b5dd08c060
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37017008"
---
# <a name="azure-stack-registration"></a>Azure yığın kaydı
Market öğesi Azure'dan karşıdan yüklemek ve ticari veri geri Microsoft'a raporlama ayarlamak için Azure ile Azure yığın Geliştirme Seti (ASDK) yüklemenizi kaydedebilirsiniz. Kayıt Market dağıtım dahil olmak üzere, tam Azure yığın işlevleri desteklemek için gereklidir. Market dağıtım ve kullanım raporlama gibi önemli Azure yığın işlevselliğini sınamak için sağladığından kayıt önerilir. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz. Ancak, ASDK kullanıcılar bunlar rapor herhangi kullanım için ücretlendirilmezsiniz.

ASDK kaydettirmezseniz görebileceğiniz bir **gerekli etkinleştirme** Azure yığın Geliştirme Seti kaydetmek için öneren bir uyarı bildirimi. Bu davranış beklenir.

## <a name="prerequisites"></a>Önkoşullar
Azure ile ASDK kaydetmek için bu yönergeleri kullanmadan önce Azure yığın PowerShell yüklü ve açıklandığı gibi Azure yığın araçları indirilen emin [dağıtım sonrası yapılandırmaya](asdk-post-deploy.md) makalesi.

Ayrıca, PowerShell dil modu ayarlanması gerekir **FullLanguageMode** ASDK Azure ile kaydetmek için kullanılan bilgisayarda. Geçerli dil modu tam, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell komutlarını çalıştırmak için ayarlandığını doğrulamak için:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Çıktıyı döndürür olun **FullLanguageMode**. Başka bir dil modu döndürülen kayıt başka bir bilgisayarda çalıştırılması gerekir ya da dil modu ayarlanması gereken **FullLanguageMode** devam etmeden önce.

## <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin
Azure ile ASDK kaydetmek için aşağıdaki adımları izleyin.

> [!NOTE]
> Tüm adımları ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. ASDK için Geliştirme Seti ana bilgisayar olmasıdır.

1. PowerShell konsolunu yönetici olarak açın.  

2. Azure ile ASDK yüklemenizi kaydetmek için aşağıdaki PowerShell komutlarını çalıştırın. Azure aboneliğinizi ve yerel ASDK yükleme için oturum açmak gerekir. Bir Azure aboneliğiniz yok mu henüz yapabilecekleriniz, [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.

  ```powershell
  # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
  Add-AzureRmAccount -EnvironmentName "AzureCloud"

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  #Register Azure Stack
  $AzureContext = Get-AzureRmContext
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01 `
      -BillingModel Development
  ```
3. Betik tamamlandığında, bu iletiyi görmeniz gerekir: **ortamınızı artık kayıtlı ve sağlanan parametreler kullanılarak etkinleştirildi.**

    ![](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Kaydın başarılı olduğunu doğrulayın
Azure ile ASDK kaydın başarılı olduğunu doğrulamak için aşağıdaki adımları izleyin.

1. Oturum [Azure yığın yönetim portalı](https://adminportal.local.azurestack.external).

2. Tıklatın **Market Yönetim** > **Azure'dan eklemek**.

    ![](media/asdk-register/2.PNG)

3. Azure'dan kullanılabilir öğeleri listesini görürseniz, etkinleştirme başarılı oldu.

    ![](media/asdk-register/3.PNG)

## <a name="next-steps"></a>Sonraki adımlar
[Bir Azure yığın Market öğesi ekleme](.\.\azure-stack-marketplace.md)
