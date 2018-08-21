---
title: PowerShell ile Azure Stack doğrulama otomatik hale getirin | Microsoft Docs
description: PowerShell ile Azure Stack doğrulama otomatik hale getirebilirsiniz.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1cb4b1a7cfd72ea302676244a53af58e77215aa9
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235453"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>PowerShell ile Azure Stack doğrulamayı otomatikleştirin 

Doğrulama (VaaS) hizmet olarak kullanarak testleri başlatma otomatikleştirme olanağı sağlar **LaunchVaaSTests.ps1** betiği.

Aşağıdaki iş akışı için PowerShell kullanabilirsiniz:

- Test geçiş iş akışı

Bu betik, bir iş akışının dört öğeleri kapsar:

- Önkoşulları yükler.
- Yükler ve yerel aracı başlatılıyor.
- Testleri, tümleştirme, işlev, güvenilirlik gibi bir kategori başlatır.
- Her test sonucu izleme geçirin ve rapor raporlar oluşturma dosya.

## <a name="launch-the-test-pass-workflow"></a>Test geçiş iş akışını Başlat

1. Yükseltilmiş bir PowerShell istemi açın.

2. Otomasyon betiği indirmek için aşağıdaki betiği çalıştırın:

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. Değerleriniz ile aşağıdaki betiği çalıştırın:

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parametreler**

    | Parametre | Açıklama |
    | --- | --- |
    | VaaSUserld | VaaS kullanıcı kimliğinizi | 
    | VaaSUserPassword | VaaS parolanızı. |
    | ServiceAdminUser | Azure Stack Hizmet Yöneticisi hesabınıza.  |
    | ServiceAdminPassword | Azure Stack hizmeti parolası.  |
    | TenantAdminUser | Birincil Kiracı Yöneticisi.  |
    | TenantAdminPassword | Birincil Kiracı için parola.  |
    | FunctionalCategory| Tümleştirme, işlev, veya. Güvenilirlik. Birden çok değer kullanıyorsanız, her değeri virgülle ayırın.  |

4. Test sonuçlarını gözden geçirin. Test sonuçlarını okuma hakkında daha fazla bilgi için bkz: [izleme testleri](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Sonraki adımlar

 - Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
 - Azure Stack'te PowerShell hakkında daha fazla bilgi için bkz: [Azure Stack Modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) başvuru.