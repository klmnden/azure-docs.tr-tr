---
title: Azure ile ASDK kaydetme | Microsoft Docs
description: Azure Stack Market dağıtım ve kullanım raporlama etkinleştirmek için Azure ile kaydetmek açıklar.
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
ms.date: 07/25/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 2a447931ea850c4ccbe618270de5fbbc9b9eaea7
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366605"
---
# <a name="azure-stack-registration"></a>Azure Stack kaydı
Azure Market öğelerini indirme ve ticaret verileri Microsoft'a raporlamaya ayarlamak için Azure ile Azure Stack geliştirme Seti'ni (ASDK) yüklemenizi kaydedebilirsiniz. Kayıt, Pazar dağıtımı da dahil olmak üzere tam Azure Stack işlevleri desteklemek için gereklidir. Kayıt önerilir çünkü Market dağıtım ve kullanım raporlama gibi önemli Azure Stack işlevselliğini test etmek sağlar. Azure Stack kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz. Ancak ASDK kullanıcılar bunlar rapor tüm kullanımlar için ücretlendirilmezsiniz.

ASDK kaydedilmezse görebileceğiniz bir **etkinleştirme gerekli** , Azure Stack geliştirme Seti'ni kaydedilecek öneren uyarı bildirimi. Bu davranış beklenmektedir.

## <a name="prerequisites"></a>Önkoşullar
Azure Stack PowerShell yüklü ve açıklandığı gibi Azure Stack araçları indirilen olun, Azure ile ASDK kaydetmek için bu yönergeleri kullanmadan önce [dağıtım sonrası yapılandırma](asdk-post-deploy.md) makalesi.

Ayrıca, PowerShell dil modunu ayarlamanız gerekir **FullLanguageMode** ASDK Azure ile kaydetmek için kullanılan bilgisayarda. Geçerli dil modunu tam, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell komutlarını çalıştırmak için ayarlandığını doğrulamak için:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Çıktıyı döndürür olun **FullLanguageMode**. Diğer bir dil modu döndürülen kayıt başka bir bilgisayarda çalıştırılması gerekir ya da dil modunu ayarlanması gerekir **FullLanguageMode** devam etmeden önce.

## <a name="register-azure-stack-with-azure"></a>Azure Stack Azure ile kaydedin
Azure ile ASDK kaydetmek için aşağıdaki adımları izleyin.

> [!NOTE]
> Bu adımların tümünü ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. ASDK için Geliştirme Seti ana bilgisayar olmasıdır.

1. Yönetici olarak bir PowerShell konsolu açın.  

2. Azure ile ASDK yüklemenizi kaydetmek için aşağıdaki PowerShell komutlarını çalıştırın. Azure aboneliğinizi ve yerel ASDK yükleme için oturum açmak gerekir. Bir Azure aboneliğiniz yoksa henüz yapabilecekleriniz [buradan ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/?b=17.06). Azure aboneliğinize ücret ödemeden Azure Stack kaydetme artmasına neden olur.  

  ```PowerShell  
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
3. Betik tamamlandığında, bu iletiyi görmeniz gerekir: **ortamınızı şimdi kaydedilir ve sağlanan parametreleri kullanarak etkinleştirildi.**

    ![Ortamınızı artık kayıtlı](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Kaydın başarılı olduğunu doğrulayın
Azure ile ASDK kaydın başarılı olduğunu doğrulamak için aşağıdaki adımları izleyin.

1. Oturum [Azure Stack Yönetim Portalı](https://adminportal.local.azurestack.external).

2. Tıklayın **Market Yönetim** > **Azure'dan ekleme**.

    ![](media/asdk-register/2.PNG)

3. Azure'dan kullanılabilir öğeleri listesini görürseniz, etkinleştirme başarılı oldu.

    ![](media/asdk-register/3.PNG)

## <a name="next-steps"></a>Sonraki adımlar
[Bir Azure Stack Market öğesi Ekle](.\.\azure-stack-marketplace.md)
