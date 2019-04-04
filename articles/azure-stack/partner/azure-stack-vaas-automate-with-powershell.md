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
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 39ed9ee9dab7f2ec97d2fb6a0148db333648b227
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481476"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>PowerShell ile Azure Stack doğrulamayı otomatikleştirin

Doğrulama (VaaS) hizmet olarak kullanarak testleri başlatma otomatikleştirme olanağı sağlar **LaunchVaaSTests.ps1** betiği.

> [!NOTE]  
> Otomasyon, yalnızca Test geçiş iş akışı için kullanılabilir. Paket doğrulaması ve çözüm doğrulama iş akışları yalnızca VaaS portal aracılığıyla desteklenir.

Bu betik için kullanılabilir:

> [!div class="checklist"]
> * Ön koşulları yükleme
> * Yükleme ve yerel aracı başlatma
> * Testleri, bir kategori gibi başlatma *tümleştirme*, *işlevsel*, *güvenilirlik*
> * Rapor test sonuçları

## <a name="launch-the-test-pass-workflow"></a>Test geçiş iş akışını Başlat

1. Yükseltilmiş bir PowerShell istemi açın.

2. Otomasyon betiği indirmek için aşağıdaki betiği çalıştırın:

    ```powershell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Uygun parametre değerleriniz ile aşağıdaki betiği çalıştırın:

    ```powershell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parametreler**

    | Parametre | Açıklama |
    | --- | --- |
    | VaaSUserId | VaaS kullanıcı kimliğinizi |
    | VaaSUserPassword | VaaS parolanızı. |
    | VaaSAccountTenantId | VaaS kiracınız GUID. |
    | VaaSSolutionName | Test altında başarısız VaaS çözümün adına çalıştırılır. |
    | VaaSTestPassName | VaaS test adı oluşturmak için iş akışı geçirin. |
    | VaaSTestCategories | `Integration`, `Functional`, veya. `Reliability`. Birden çok değer kullanıyorsanız, her değeri virgülle ayırın.  |
    | ServiceAdminUserName | Azure Stack Hizmet Yöneticisi hesabınıza.  |
    | ServiceAdminPassword | Azure Stack hizmeti parolası.  |
    | TenantAdminUserName | Birincil Kiracı Yöneticisi.  |
    | TenantAdminPassword | Birincil Kiracı için parola.  |
    | CloudAdminUserName | Bulut yönetici kullanıcı adı.  |
    | CloudAdminPassword | Bulut yöneticisinin parolası.  |

4. Test sonuçlarını gözden geçirin. Diğer seçenekler için bkz. [İzleyici ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack'te PowerShell hakkında daha fazla bilgi için en yeni modülleri gözden geçirin.

> [!div class="nextstepaction"]
> [Azure Stack Modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0)
