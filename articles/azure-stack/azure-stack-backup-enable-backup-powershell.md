---
title: PowerShell ile Azure Stack için yedeklemeyi etkinleştirme | Microsoft Docs
description: Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştirin; böylelikle bir hata varsa, Azure Stack geri yüklenebilir.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 8fe7f0ddd630cfca0242af6cc1d728bdef163352
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060918"
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
| $password       | Tür **parola** kullanıcı. |
| $sharepath      | Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihazda barındırılan bir dosya paylaşımı yolu için bir Evrensel Adlandırma Kuralı (UNC) dize kullanmanız gerekir. Bir UNC dize paylaşılan dosyalarını veya cihazları gibi kaynakların konumunu belirtir. Yedekleme verilerini kullanılabilirliğini sağlamak için cihazı ayrı bir konumda olmalıdır. |
| $frequencyInHours | Saat cinsinden ne sıklıkta belirler yedekler oluşturulur. 12 varsayılan değerdir. Zamanlayıcı, en fazla 12 ve en az 4 destekler.|
| $retentionPeriodInDays | Gün cinsinden saklama süresi, kaç güne kadar yedek bir dış konuma göre korunur belirler. Varsayılan değer 7'dir. Zamanlayıcı, en fazla 14 ve en az 2 destekler. Yedekleri saklama süresinden daha eski bir dış konumdan otomatik olarak silinir.|
|     |     |

   ```powershell
    # Example username:
    $username = "domain\backupadmin"
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
   
    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    
    # The encryption key is generated using the New-AzsEncryptionKeyBase64 cmdlet provided in Azure Stack PowerShell.
    # Make sure to store your encryption key in a secure location after it is generated.
    $Encryptionkey = New-AzsEncryptionKeyBase64
    $key = ConvertTo-SecureString -String ($Encryptionkey) -AsPlainText -Force

    Set-AzsBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Yedekleme ayarlarını Onayla

Aynı PowerShell oturumunda aşağıdaki komutları çalıştırın:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName
   ```

Sonuç, aşağıdaki örnek çıktı gibi görünmelidir:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
   ```

## <a name="update-backup-settings"></a>Yedekleme ayarlarını güncelleştirme
Aynı PowerShell oturumunda saklama süresi ve yedekleme sıklığı için varsayılan değerlerle güncelleştirebilirsiniz. 

   ```powershell
    #Set the backup frequency and retention period values.
    $frequencyInHours = 10
    $retentionPeriodInDays = 5

    Set-AzsBackupShare -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

Sonuç, aşağıdaki örnek çıktı gibi görünmelidir:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

## <a name="next-steps"></a>Sonraki adımlar

 - Yedekleme, see çalıştırmayı öğrenin [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md ).  
 - Yedekleme çalıştırdığını doğrulamak hakkında bilgi edinmek için bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md ).
