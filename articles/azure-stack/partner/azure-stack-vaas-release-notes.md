---
title: Hizmet olarak Azure Stack doğrulama sürüm notları | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulama sürüm notları.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 687e47eff000679f24088a59297a258a1469f3fd
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106282"
---
# <a name="release-notes-for-validation-as-a-service"></a>Hizmet olarak doğrulama için sürüm notları

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bu makalede, bir hizmet olarak Azure Stack doğrulama için sürüm notları bulunur.

## <a name="version-401"></a>Sürüm 4.0.1'in

8 Ekim 2018

- VaaS önkoşulları

    `Install-VaaSPrerequisites` artık bulut Yöneticisi kimlik bilgilerini gerektirir. Bu cmdlet en son sürümünü çalıştırıyorsanız bkz [aracısını indirme ve yükleme](azure-stack-vaas-local-agent.md#download-and-install-the-agent) önkoşulları yüklemek için yeniden düzenlenen komutlar için. Komutlar şunlardır:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

## <a name="version-400"></a>Sürüm 4.0.0

29 Ağustos 2018

- VaaS önkoşulları ve VHD güncelleştirir

    `Install-VaaSPrerequisites` Şimdi çözüm doğrulama sırasında bir soruna yönelik olarak bulut Yöneticisi kimlik bilgileri gerektirir. Adresindeki belgelere [aracısını indirme ve yükleme](azure-stack-vaas-local-agent.md#download-and-install-the-agent) şununla güncelleştirildi:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential "<cloudAdminDomain\username>", (ConvertTo-SecureString "<cloudAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region `
                              -CloudAdminCredentials $CloudAdminCreds
    ```
    > [!NOTE]
    > `$CloudAdminCreds` Gerekli komut dosyası tarafından Azure Stack için örneği olan doğrulanır. Bunlar VaaS Kiracı tarafından kullanılan Azure Active Directory kimlik bilgilerini değil.

- Yerel aracı güncelleştirme

    Yerel aracısının önceki sürümünü geçerli 4.0.0 ile uyumlu olmayan hizmet sürümü. Tüm kullanıcılar, kendi yerel aracılarını güncelleştirmeniz gerekir. Bkz: [aracısını indirme ve yükleme](azure-stack-vaas-local-agent.md#download-and-install-the-agent) en yeni aracı yükleme yönergeleri için.

- PowerShell Otomasyon güncelleştirme

    Değişiklikler yapıldı `LaunchVaaSTests` PowerShell betikleri, komut dosyası paketlerinin en son sürümünü gerektirir. Bkz: [Test geçiş iş akışı başlatma](azure-stack-vaas-automate-with-powershell.md#launch-the-test-pass-workflow) betik paketinin en son sürümü yükleme hakkında yönergeler için.

- Doğrulama Hizmeti portalı

  - Bildirimleri imzalama paket

    Çözüm doğrulama iş akışının bir parçası bir OEM özelleştirme paketi gönderildiğinde, yayımlanan belirtimini izlediğini emin olmak için paket biçimi doğrulanır. Paket uyumlu değil çalıştırma başarısız olur. Kiracı için kaydedilen Azure Active Directory kişinin e-posta adresine e-posta bildirimi gönderilir.

  - Etkileşimli test kategorisi

    **Etkileşimli** test kategorisi eklendi. Bu testler, iş ortaklarının Azure Stack senaryoları etkileşimli ve otomatik olmayan çalışma olanak tanır.

  - Etkileşimli özellik doğrulama

    Odaklanmış geri bildirim belirli özellikleri sağlama olanağı Test geçiş iş akışı içinde kullanıma sunuldu. `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` Test belirli güncelleştirmeler uygulanmış olan doğru olmadığını denetler ve ardından geri bildirim toplar.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)