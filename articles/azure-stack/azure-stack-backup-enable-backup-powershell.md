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
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 38ab7b80e2f03176c3bedfd98a2d0e20fc02592b
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56865901"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>PowerShell ile Azure Stack için yedeklemeyi etkinleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Windows PowerShell ile hizmet altyapı yedeklemeyi etkinleştir, bu nedenle düzenli yedeklemelerini alın:
 - İç kimlik hizmeti ve kök sertifikası
 - Kullanıcı planları, teklifleri ve abonelikleri
 - İşlem, depolama ve ağ kullanıcı kotaları
 - Kullanıcı Key vault gizli dizileri
 - Kullanıcı RBAC rolleri ve ilkeleri
 - Kullanıcı depolama hesapları

Yedeklemeyi etkinleştirmek için yedekleme başlatın ve işleci yönetim uç noktası aracılığıyla Yedekleme bilgileri almak için PowerShell cmdlet'lerini erişebilirsiniz.

## <a name="prepare-powershell-environment"></a>PowerShell ortamını hazırlama

PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md). Azure Stack'e oturum açmak için bkz: [işleci ortamı yapılandırmak ve Azure Stack için oturum açma](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Yedeklemeyi etkinleştirmek için yedek paylaşımı, kimlik bilgilerini ve şifreleme anahtarını sağlayın

Aynı PowerShell oturumunda, değişkenleri ortamınız için ekleyerek, aşağıdaki PowerShell betiğini düzenleyin. Altyapı yedekleme hizmetine yedekleme paylaşımı, kimlik bilgilerini ve şifreleme anahtarı sağlamak için güncelleştirilmiş betiği çalıştırın.

| Değişken        | Açıklama   |
|---              |---                                        |
| $username       | Tür **kullanıcıadı** etki alanını ve kullanıcı adı için yeterli erişimi olan paylaşılan sürücü konumunu dosyalarını okuma ve yazma için kullanma. Örneğin, `Contoso\backupshareuser`. |
| $password       | Tür **parola** kullanıcı. |
| $sharepath      | Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihazda barındırılan bir dosya paylaşımı yolu için bir Evrensel Adlandırma Kuralı (UNC) dize kullanmanız gerekir. Bir UNC dize paylaşılan dosyalarını veya cihazları gibi kaynakların konumunu belirtir. Yedekleme verilerini kullanılabilirliğini sağlamak için cihazı ayrı bir konumda olmalıdır. |
| $frequencyInHours | Saat cinsinden ne sıklıkta belirler yedekler oluşturulur. 12 varsayılan değerdir. Zamanlayıcı, en fazla 12 ve en az 4 destekler.|
| $retentionPeriodInDays | Gün cinsinden saklama süresi, kaç güne kadar yedek bir dış konuma göre korunur belirler. Varsayılan değer 7'dir. Zamanlayıcı, en fazla 14 ve en az 2 destekler. Yedekleri saklama süresinden daha eski bir dış konumdan otomatik olarak silinir.|
| $encryptioncertpath | Şifreleme sertifika yolu, dosya yolunu belirtir. Veri şifreleme için kullanılan ortak anahtarla CER dosyası. |
|     |     |

```powershell
    # Example username:
    $username = "domain\backupadmin"
 
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString

    # Create a self-signed certificate using New-SelfSignedCertificate, export the public key portion and save it locally.

    $cert = New-SelfSignedCertificate `
        -DnsName "www.contoso.com" `
        -CertStoreLocation "cert:\LocalMachine\My" 

    New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 

    #make sure to export the PFX format of the certificate with the public and private keys and then delete the certifcate from the local certificate store of the machine where you created the certificate
    
    Export-Certificate `
        -Cert $cert `
        -FilePath c:\certs\AzSIBCCert.cer 

    # Set the backup settings with the name, password, share, and CER certificate file.
    Set-AzsBackupConfiguration -BackupShare $sharepath -Username $username -Password $password -EncryptionCertPath "c:\temp\cert.cer"
```
   
##  <a name="confirm-backup-settings"></a>Yedekleme ayarlarını Onayla

Aynı PowerShell oturumunda aşağıdaki komutları çalıştırın:

   ```powershell
    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName
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

    Set-AzsBackupConfiguration -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays

    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

Sonuç, aşağıdaki örnek çıktı gibi görünmelidir:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

###<a name="azure-stack-powershell"></a>Azure Stack PowerShell 
Altyapı yedeklemeyi yapılandırmak için PowerShell cmdlet Set-AzsBackupConfiguration ' dir. Önceki sürümlerde, cmdlet kümesi AzsBackupShare oluştu. Bu cmdlet, bir sertifikanın belirtilmesi gerekir. Altyapı yedeklemesine bir şifreleme anahtarı ile yapılandırıldıysa, şifreleme anahtarını güncelleştiremezsiniz veya özelliğini görüntüleyin. Yönetici PowerShell 1.6 sürümünü kullanmanız gerekir. 

Altyapı yedekleme için 1901 güncelleştirmeden önce yapılandırıldıysa, şifreleme anahtarını görüntüleyin ve yönetici PowerShell 1.6 sürümünü kullanabilirsiniz. Sürüm 1.6 için şifreleme anahtarını güncelleştirmek için bir sertifika dosyası izin vermez.
Başvurmak [Azure Stack PowerShell yükleme](azure-stack-powershell-install.md) Modülü doğru sürümü yükleme hakkında daha fazla bilgi. 


## <a name="next-steps"></a>Sonraki adımlar

Yedekleme, see çalıştırmayı öğrenin [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md)

Yedekleme çalıştırdığını doğrulamak hakkında bilgi edinmek için bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md)
