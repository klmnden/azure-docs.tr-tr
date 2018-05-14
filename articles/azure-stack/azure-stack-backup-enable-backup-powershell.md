---
title: PowerShell ile Azure yığınının yedeklemeyi etkinleştirme | Microsoft Docs
description: Böylece bir hata olduğunda Azure yığın geri Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştirin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/10/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 4faa6930c37f9d491a3efa4b34519dbb13761a9d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>PowerShell ile Azure yığınının yedeklemeyi etkinleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Windows PowerShell ile hizmet altyapı yedekleme etkinleştir şekilde düzenli yedeklemelerini alın:
 - İç kimlik hizmeti ve kök sertifikası
 - Kullanıcı planları, teklifler, abonelik
 - Keyvault gizli
 - Kullanıcı RBAC rolleri ve ilkeleri

Yedeklemeyi etkinleştirmek için yedekleme başlatın ve işleci yönetim uç noktası aracılığıyla yedekleme bilgi almak için PowerShell cmdlet'leri erişebilir.

## <a name="prepare-powershell-environment"></a>PowerShell ortamını hazırlayın

PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure yığını için PowerShell yükleme ](azure-stack-powershell-install.md).

## <a name="generate-a-new-encryption-key"></a>Yeni bir şifreleme anahtarı oluştur

Yükleme ve Azure yığını için yapılandırılmış PowerShell ve Azure yığın araçları.
 - Bkz: [Azure yığınında PowerShell ile başlamak ve çalıştırmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).
 - Bkz: [github'dan indirin Azure yığın araçları](azure-stack-powershell-download.md)

Windows PowerShell ile yükseltilmiş istemi açın ve aşağıdaki komutları çalıştırın:
   
   ```powershell
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
   ```
   
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
   Get-AzsBackupLocation | Select-Object -ExpandProperty externalStoreDefault | Select-Object -Property Path, UserName, Password | ConvertTo-Json
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