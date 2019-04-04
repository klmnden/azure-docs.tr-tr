---
title: Azure Stack tümleşik sistemleri dağıtımı için Azure Stack ortak anahtar altyapısı sertifikaları doğrulamak | Microsoft Docs
description: Azure Stack tümleşik sistemleri Azure Stack PKI sertifikalarını doğrulamak açıklar. Azure Stack sertifika Denetleyicisi aracını kullanmayı ele alır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 06cb29d6d04fb314f9eefa63d7a2b628a2af846b
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481129"
---
# <a name="validate-azure-stack-pki-certificates"></a>Azure Stack PKI sertifikalarını doğrulama

Bu makalede açıklanan Azure Stack hazırlık Denetleyicisi aracı kullanılabilir [PowerShell Galerisi'ndeki](https://aka.ms/AzsReadinessChecker). Aracı doğrulamak için kullanabileceğiniz [oluşturulan PKI sertifikalarını](azure-stack-get-pki-certs.md) dağıtım öncesi için uygundur. Sertifikalar gerekiyorsa sertifikaları yeniden gönderin ve test için yeterli zaman bırakarak doğrulayın.

Hazır olma denetimi aracı, aşağıdaki sertifika doğrulama gerçekleştirir:

- **PFX okuyun**  
    Doğru parolayı geçerli PFX dosyası için denetler ve olup ortak bilgilerini parola ile korunmuyor. 
- **İmza algoritması**  
    İmza algoritması SHA1 olmadığını denetler.
- **Özel anahtar**  
    Özel anahtarı yok ve yerel makine özniteliğiyle verilir denetimleri. 
- **Sertifika zinciri**  
    Denetimleri sertifika zinciri, otomatik olarak imzalanan sertifikaları için bir onay dahil olduğu.
- **DNS adları**  
    SAN her uç nokta için ilgili DNS adlarını içeren veya destekleniyorsa bir joker karakter varsa denetler.
- **Anahtar kullanımı**  
    Anahtar kullanımı bir dijital imza ve anahtar şifreleme içerir ve sunucu kimlik doğrulaması ve istemci kimlik doğrulaması Gelişmiş anahtar kullanımı içeren denetler.
- **Anahtar boyutu**  
    Anahtar boyutu 2048 ya da daha büyük olup olmadığını denetler.
- **Zincir sırası**  
    Sipariş doğru olduğunu doğrulama sertifikaları sırasını denetler.
- **Diğer sertifikaları**  
    Diğer Sertifika PFX içinde ilgili yaprak sertifikayı ve kendi zinciri dışında paketlenmiş emin olun.
- **Profil yok**  
    Yeni bir kullanıcı PFX verilerinin sertifika bakım sırasında gMSA hesabı davranışını yakından taklit eden bir kullanıcı profili yüklendi, olmadan yükleyebilir denetler.

> [!IMPORTANT]  
> PKI sertifikasını bir PFX dosyası ve parola gizli bilgi değerlendirilmelidir.

## <a name="prerequisites"></a>Önkoşullar

Sisteminizde Azure Stack dağıtımı için PKI sertifikaları doğrulamadan önce aşağıdaki önkoşulları karşılamalıdır:

- Microsoft Azure Stack hazırlık denetleyicisi
- SSL dışarı aşağıdaki sertifikaları [hazırlık yönergeleri](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 veya Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Çekirdek hizmetler sertifika doğrulama gerçekleştirme

Hazırlama ve dağıtım ve gizli döndürme Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın:

1. Yükleme **AzsReadinessChecker** aşağıdaki cmdlet'i çalıştırarak bir PowerShell isteminden (5.1 veya üstü):

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ```

2. Sertifika dizin yapısı oluşturun. Aşağıdaki örnekte, değiştirebileceğiniz `<c:\certificates>` için tercih ettiğiniz yeni bir dizin yolu.
    ```powershell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'Admin Extension Host', 'Admin Portal', 'api_appservice', 'ARM Admin', 'ARM Public', 'ftp_appservice', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal', 'sso_appservice', 'wildcard_dbadapter', 'wildcard_sso_appservice'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```
    
    > [!Note]  
    > AD FS ve graf kimlik sisteminizde AD FS kullanıyorsanız gereklidir. Örneğin:
    >
    > ```powershell  
    > $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'ADFS', 'Admin Extension Host', 'Admin Portal', 'api_appservice', 'ARM Admin', 'ARM Public', 'ftp_appservice', 'Graph', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal', 'sso_appservice', 'wildcard_dbadapter', 'wildcard_sso_appservice'
    > ```
    
     - Önceki adımda oluşturduğunuz uygun dizinleri sertifikalarınız yerleştirin. Örneğin:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\ARM Admin\CustomerCertificate.pfx`

3. PowerShell penceresinde değerlerini değiştirmek **RegionName** ve **FQDN** Azure Stack ortamına uygun ve aşağıdaki komutu çalıştırın:

    ```powershell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Invoke-AzsCertificateValidation -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD  
    ```

4. Çıktı ve tüm sertifikaları geçirmek tüm testleri denetleyin. Örneğin:

```powershell
Invoke-AzsCertificateValidation v1.1809.1005.1 started.
Testing: ARM Public\ssl.pfx
Thumbprint: 7F6B27****************************E9C35A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Admin Extension Host\ssl.pfx
Thumbprint: A631A5****************************35390A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Public Extension Host\ssl.pfx
Thumbprint: 4DBEB2****************************C5E7E6
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsCertificateValidation Completed
```

### <a name="known-issues"></a>Bilinen sorunlar

**Belirti**: Testler atlandı

**Neden**: Bir bağımlılık uyulmazsa AzsReadinessChecker belirli testleri atlar:

 - Sertifika zinciri başarısız olursa, diğer sertifikalar atlanır.

    ```powershell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
    Invoke-AzsCertificateValidation Completed
    ```

**Çözüm**: Her her sertifika için test kümesini altındaki ayrıntılar bölümünde Aracı'nın yönergeleri izleyin.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Platform bir hizmet sertifika doğrulama gerçekleştirme

SQL/MySQL veya uygulama hizmetleri dağıtımları planlı hazırlayıp hizmet (PaaS) sertifikaları, platform için Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın.

1.  Yükleme **AzsReadinessChecker** aşağıdaki cmdlet'i çalıştırarak bir PowerShell isteminden (5.1 veya üstü):

    ```powershell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ```

2.  Yollar ve doğrulama gerektiren her bir PaaS sertifikanın parolasını içeren iç içe geçmiş bir karma tablosu oluşturun. Çalıştırma PowerShell penceresinde:

    ```powershell  
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Değerlerini değiştirmek **RegionName** ve **FQDN** doğrulamayı başlatmak için Azure Stack ortamınıza uyum sağlaması için. Ardından şunu çalıştırın:

    ```powershell  
    Invoke-AzsCertificateValidation -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Tüm sertifikaları çıktı ve, tüm sınamaları geçmesi denetleyin.

    ```powershell
    Invoke-AzsCertificateValidation v1.0 started.
    Thumbprint: 95A50B****************************FA6DDA
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: EBB011****************************59BE9A
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 76AEBA****************************C1265E
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 8D6CCD****************************DB6AE9
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
    ```

## <a name="certificates"></a>Sertifikalar

| Dizin | Sertifika |
| ---    | ----        |
| acsBlob | wildcard_blob_\<bölge > _\<externalFQDN > |
| ACSQueue  |  wildcard_queue_\<region>_\<externalFQDN> |
| ACSTable  |  wildcard_table_\<bölge > _\<externalFQDN > |
| Yönetici uzantısı konağı  |  wildcard_adminhosting_\<bölge > _\<externalFQDN > |
| Yönetici portalı  |  adminportal_\<bölge > _\<externalFQDN > |
| ARM yönetici  |  adminmanagement_\<bölge > _\<externalFQDN > |
| ARM genel  |  management_\<bölge > _\<externalFQDN > |
| KeyVault  |  wildcard_vault_\<region>_\<externalFQDN> |
| KeyVaultInternal  |  wildcard_adminvault_\<bölge > _\<externalFQDN > |
| Genel uzantı konak  |  wildcard_hosting_\<bölge > _\<externalFQDN > |
| Genel kullanıma açık portala  |  portal_\<bölge > _\<externalFQDN > |

## <a name="using-validated-certificates"></a>Doğrulanmış bir sertifika kullanma

Sertifikalarınızı AzsReadinessChecker tarafından doğrulandıktan sonra Azure Stack dağıtımınıza veya Azure Stack gizli döndürme için kullanıma hazır olursunuz. 

 - Böylece bunlar bunları belirtildiği gibi dağıtım konağı üzerine kopyalayabilirsiniz dağıtımı için dağıtım mühendisinize sertifikalarınızı güvenli bir şekilde aktarın. [Azure Stack PKI gereksinimleri belgeleri](azure-stack-pki-certs.md).
 - Gizli dönüş izleyerek Azure Stack ortamınızın genel altyapı uç noktalar için eski sertifikaları güncelleştirmek için sertifikaları kullanabilirsiniz [Azure Stack gizli döndürme belgeleri](azure-stack-rotate-secrets.md).
 - PaaS Hizmetleri için takip ederek Azure Stack'te SQL, MySQL ve App Services kaynak sağlayıcılarını yüklemek için sertifikaları kullanabilirsiniz [Azure Stack belgeleri hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

[Veri merkezi kimlik tümleştirme](azure-stack-integrate-identity.md)
