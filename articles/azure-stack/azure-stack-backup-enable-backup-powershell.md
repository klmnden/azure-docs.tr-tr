---
title: PowerShell ile Azure yığınının yedeklemeyi etkinleştirme | Microsoft Docs
description: Böylece bir hata olduğunda Azure yığın geri Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştirin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 7DFEFEBE-D6B7-4BE0-ADC1-1C01FB7E81A6
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: d21bb919686e318b1caf7267b3115dae20938884
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>PowerShell ile Azure yığınının yedeklemeyi etkinleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Böylece bir hata olduğunda Azure yığın geri Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştirin. Yedeklemeyi etkinleştirmek için yedekleme başlatın ve işleci yönetim uç noktası aracılığıyla yedekleme bilgi almak için PowerShell cmdlet'leri erişebilir.

## <a name="download-azure-stack-tools"></a>Azure yığın araçları yükleyin

Yükleme ve Azure yığını için yapılandırılmış PowerShell ve Azure yığın araçları. Bkz: [Azure yığınında PowerShell ile başlamak ve çalıştırmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

##  <a name="load-the-connect-and-infrastructure-modules"></a>Connect ve altyapı modülleri yükleme

Windows PowerShell ile yükseltilmiş istemi açın ve aşağıdaki komutları çalıştırın:

   ```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1
    
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
   ```

##  <a name="setup-rm-environment-and-log-into-the-operator-management-endpoint"></a>RM ortamı ve günlük işleci yönetim uç noktada Kurulumu

Aynı PowerShell oturumunda, aşağıdaki PowerShell komut dosyası değişkenleri ortamınız için ekleyerek düzenleyin. RM ortamını ayarlama ve işleci yönetim uç noktada oturum güncelleştirilmiş komut dosyasını çalıştırın.

| Değişken    | Açıklama |
|---          |---          |
| $TenantName | Azure Active Directory Kiracı adı. |
| İşleç hesap adı        | Azure yığın işleci hesap adınız. |
| Azure Kaynak Yöneticisi uç noktası | URL için Azure Resource Manager. |

   ```powershell
   # Specify Azure Active Directory tenant name
    $TenantName = "contoso.onmicrosoft.com"
    
    # Set the module repository and the execution policy
    Set-PSRepository `
      -Name "PSGallery" `
      -InstallationPolicy Trusted
    
    Set-ExecutionPolicy RemoteSigned `
      -force
    
    # Configure the Azure Stack operator’s PowerShell environment.
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint "https://adminmanagement.seattle.contoso.com"
    
    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience "https://graph.windows.net/"
    
    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName $TenantName `
      -EnvironmentName AzureStackAdmin
    
    # Sign-in to the operator's console.
    Connect-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID 
    
   ```
## <a name="generate-a-new-encryption-key"></a>Yeni bir şifreleme anahtarı oluştur

Aynı PowerShell oturumunda aşağıdaki komutları çalıştırın:

   ```powershell
   $encryptionkey = New-EncryptionKeyBase64
   ```

> [!Warning]  
> Anahtarı oluşturmak için AzureStack araçları kullanmanız gerekir.

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Yedeklemeyi etkinleştirmek için yedekleme paylaşımı, kimlik bilgileri ve şifreleme anahtarı sağlayın

Aynı PowerShell oturumunda, aşağıdaki PowerShell komut dosyası değişkenleri ortamınız için ekleyerek düzenleyin. Altyapı yedekleme hizmetine yedekleme paylaşımı, kimlik bilgileri ve şifreleme anahtarı sağlamak için güncelleştirilmiş komut dosyasını çalıştırın.

| Değişken        | Açıklama   |
|---              |---                                        |
| $username       | Tür **kullanıcıadı** kullanıcı adı ve etki alanı için paylaşılan sürücü konumunu kullanarak. Örneğin, `Contoso\administrator`. |
| $password       | Tür **parola** kullanıcı için. |
| $sharepath      | Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihaz üzerinde barındırılan bir dosya paylaşımına yol için bir Evrensel Adlandırma Kuralı (UNC) dize kullanmanız gerekir. Bir UNC dize paylaşılan dosyaları veya aygıt konumunu belirtir. Yedekleme verilerini kullanılabilirliğini sağlamak için aygıt ayrı bir konumda olmalıdır. |

   ```powershell
    $username = "domain\backupoadmin"
    $password = "password"
    $credential = New-Object System.Management.Automation.PSCredential($username, ($password| ConvertTo-SecureString -asPlainText -Force))  
    $location = Get-AzsLocation
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
    
    Set-AzSBackupShare -Location $location.Name -Path $sharepath -UserName $credential.UserName -Password $credential.GetNetworkCredential().password -EncryptionKey $encryptionkey
   ```
   
##  <a name="confirm-backup-settings"></a>Yedekleme ayarlarını Onayla

Aynı PowerShell oturumunda aşağıdaki komutları çalıştırın:

   ```powershell
   Get-AzsBackupLocation | Select-Object -Property Path, UserName, Password | ConvertTo-Json 
   ```

Sonuç aşağıdaki JSON çıktısını gibi görünmelidir:

   ```json
      {
    "ExternalStoreDefault":  {
        "Path":  "\\\\serverIP\\AzSBackupStore\\contoso.com\\seattle",
        "UserName":  "domain\backupoadmin",
        "Password":  null
       }
   } 
   ```

## <a name="next-steps"></a>Sonraki adımlar

 - Yedekleme, bkz: çalıştırmayı öğrenin [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).  
- Yedekleme çalıştığını doğrulamak bilgi [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md ).
