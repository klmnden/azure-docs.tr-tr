---
title: PowerShell ile Azure Stack için yedeklemeyi etkinleştirme | Microsoft Docs
description: Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştirin; böylelikle bir hata varsa, Azure Stack geri yüklenebilir.
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
ms.openlocfilehash: 4fb40904e59e78e416d4598472a6adeb498e49f4
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968893"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>PowerShell ile Azure Stack için yedeklemeyi etkinleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştir, bu nedenle düzenli yedeklemelerini alın:
 - İç kimlik hizmeti ve kök sertifikası
 - Kullanıcı planları, teklifleri ve abonelikleri
 - Keyvault gizli dizileri
 - Kullanıcı RBAC rolleri ve ilkeleri

Yedeklemeyi etkinleştirmek için yedekleme başlatın ve işleci yönetim uç noktası aracılığıyla Yedekleme bilgileri almak için PowerShell cmdlet'lerini erişebilirsiniz.

## <a name="prepare-powershell-environment"></a>PowerShell ortamını hazırlama

PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure Stack için PowerShell yükleme ](azure-stack-powershell-install.md). Azure Stack'e oturum açmak için bkz: [işleci ortamı yapılandırmak ve Azure Stack için oturum açma](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Yedeklemeyi etkinleştirmek için yedek paylaşımı, kimlik bilgilerini ve şifreleme anahtarını sağlayın

Aynı PowerShell oturumunda, değişkenleri ortamınız için ekleyerek, aşağıdaki PowerShell betiğini düzenleyin. Altyapı yedekleme hizmetine yedekleme paylaşımı, kimlik bilgilerini ve şifreleme anahtarı sağlamak için güncelleştirilmiş betiği çalıştırın.

| Değişken        | Açıklama   |
|---              |---                                        |
| $username       | Tür **kullanıcıadı** etki alanını ve kullanıcı adı için yeterli erişimi olan paylaşılan sürücü konumunu dosyalarını okuma ve yazma için kullanma. Örneğin, `Contoso\backupshareuser`. |
| $key            | Tür **şifreleme anahtarı** her yedeklemeyi şifrelemek için kullanılır. |
| $password       | Tür **parola** kullanıcı. |
| $sharepath      | Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihazda barındırılan bir dosya paylaşımı yolu için bir Evrensel Adlandırma Kuralı (UNC) dize kullanmanız gerekir. Bir UNC dize paylaşılan dosyalarını veya cihazları gibi kaynakların konumunu belirtir. Yedekleme verilerini kullanılabilirliğini sağlamak için cihazı ayrı bir konumda olmalıdır. |

   ```powershell
    $username = "domain\backupadmin"
   
    $Secure = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    $Encrypted = ConvertFrom-SecureString -SecureString $Secure
    $password = ConvertTo-SecureString -String $Encrypted
    
    $BackupEncryptionKeyBase64 = ""
    $tempEncryptionKeyString = ""
    foreach($i in 1..64) { $tempEncryptionKeyString += -join ((65..90) + (97..122) | Get-Random | % {[char]$_}) }
    $tempEncryptionKeyBytes = [System.Text.Encoding]::UTF8.GetBytes($tempEncryptionKeyString)
    $BackupEncryptionKeyBase64 = [System.Convert]::ToBase64String($tempEncryptionKeyBytes)
    $BackupEncryptionKeyBase64
    
    $Securekey = ConvertTo-SecureString -String $BackupEncryptionKeyBase64 -AsPlainText -Force
    $Encryptedkey = ConvertFrom-SecureString -SecureString $Securekey
    $key = ConvertTo-SecureString -String $Encryptedkey
    
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    Set-AzSBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Yedekleme ayarlarını Onayla

Aynı PowerShell oturumunda aşağıdaki komutları çalıştırın:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity
   ```

Sonuç aşağıdaki çıktı gibi görünmelidir:

   ```powershell
    Path                        : \\serverIP\AzSBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
   ```

## <a name="next-steps"></a>Sonraki adımlar

 - Yedekleme, see çalıştırmayı öğrenin [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md ).  
 - Yedekleme çalıştırdığını doğrulamak hakkında bilgi edinmek için bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md ).
