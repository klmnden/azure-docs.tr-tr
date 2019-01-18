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
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: 53d74e9979b9f82d7a76d21517f2fd62ac95787a
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54387975"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>PowerShell ile Azure Stack doğrulamayı otomatikleştirin

Doğrulama (VaaS) hizmet olarak kullanarak testleri başlatma otomatikleştirme olanağı sağlar **LaunchVaaSTests.ps1** betiği.

Aşağıdaki iş akışı için PowerShell kullanabilirsiniz:

- Test geçiş

Bu öğreticide, bir komut dosyasının nasıl oluşturulacağını öğrenin:

> [!div class="checklist"]
> * Önkoşulları yükler
> * Yükler ve yerel Aracısı'nı başlatır
> * Testleri, tümleştirme, işlev, güvenilirlik gibi bir kategori başlatır
> * Sonuçları test raporları

## <a name="launch-the-test-pass-workflow"></a>Test geçiş iş akışını Başlat

1. Yükseltilmiş bir PowerShell istemi açın.

2. Otomasyon betiği indirmek için aşağıdaki betiği çalıştırın:

    ```PowerShell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Uygun parametre değerleriniz ile aşağıdaki betiği çalıştırın:

    ```PowerShell
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

Azure Stack'te PowerShell hakkında daha fazla bilgi için en son modülleri gözden geçirin.

> [!div class="nextstepaction"]
> [Azure Stack Modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0)
