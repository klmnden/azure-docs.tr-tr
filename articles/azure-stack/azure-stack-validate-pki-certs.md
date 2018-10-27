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
ms.date: 09/26/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 2a9de36322b0206d67ac7320012ac431e7b03454
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155737"
---
# <a name="validate-azure-stack-pki-certificates"></a>Azure Stack PKI sertifikalarını doğrulama

Bu makalede açıklanan Azure Stack hazırlık Denetleyicisi aracı kullanılabilir [PowerShell Galerisi'ndeki](https://aka.ms/AzsReadinessChecker). Aracı doğrulamak için kullanabileceğiniz [oluşturulan PKI sertifikalarını](azure-stack-get-pki-certs.md) dağıtım öncesi için uygundur. Sertifikalar gerekiyorsa sertifikaları yeniden gönderin ve test için yeterli zaman bırakarak doğrulamalıdır.

Hazır olma denetimi aracı, aşağıdaki sertifika doğrulama gerçekleştirir:

- **PFX okuyun**  
    Doğru parolayı geçerli PFX dosyası için denetler ve genel bilgileri parola ile korunmayan durumunda sizi uyarır. 
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

    ```PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ```

2. Sertifika dizin yapısı oluşturun. Aşağıdaki örnekte, değiştirebileceğiniz `<c:\certificates>` için tercih ettiğiniz yeni bir dizin yolu.
    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal','Admin Extension Host','Public Extension Host'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```
    
    > [!Note]  
    > AD FS ve graf kimlik sisteminizde AD FS kullanıyorsanız gereklidir.
    
     - Önceki adımda oluşturduğunuz uygun dizinleri sertifikalarınız yerleştirin. Örneğin:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx`

3. PowerShell penceresinde değerlerini değiştirmek **RegionName** ve **FQDN** Azure Stack ortamına uygun ve aşağıdaki komutu çalıştırın:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Invoke-AzsCertificateValidation -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD -extensionhostfeature 
    ```

4. Çıktı ve tüm sertifikaları geçirmek tüm testleri denetleyin. Örneğin:

````PowerShell
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
````

### <a name="known-issues"></a>Bilinen sorunlar

**Belirti**: testleri atlanır

**Neden**: bağımlılık uyulmazsa AzsReadinessChecker atlar belirli testleri:

 - Sertifika zinciri başarısız olursa, diğer sertifikalar atlanır.

    ```PowerShell  
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

**Çözüm**: her her sertifika için test kümesini altındaki ayrıntılar bölümünde Aracı'nın yönergeleri izleyin.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Platform bir hizmet sertifika doğrulama gerçekleştirme

SQL/MySQL veya uygulama hizmetleri dağıtımları planlı hazırlayıp hizmet (PaaS) sertifikaları, platform için Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın.

1.  Yükleme **AzsReadinessChecker** aşağıdaki cmdlet'i çalıştırarak bir PowerShell isteminden (5.1 veya üstü):

    ```PowerShell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ```

2.  Yollar ve doğrulama gerektiren her bir PaaS sertifikanın parolasını içeren iç içe geçmiş bir karma tablosu oluşturun. Çalıştırma PowerShell penceresinde:

    ```PowerShell  
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Değerlerini değiştirmek **RegionName** ve **FQDN** doğrulamayı başlatmak için Azure Stack ortamınıza uyum sağlaması için. Ardından şunu çalıştırın:

    ```PowerShell  
    Invoke-AzsCertificateValidation -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Tüm sertifikaları çıktı ve, tüm sınamaları geçmesi denetleyin.

    ```PowerShell
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

## <a name="using-validated-certificates"></a>Doğrulanmış bir sertifika kullanma

Sertifikalarınızı AzsReadinessChecker tarafından doğrulandıktan sonra Azure Stack dağıtımınıza veya Azure Stack gizli döndürme için kullanıma hazır olursunuz. 

 - Böylece bunlar bunları belirtildiği gibi dağıtım konağı üzerine kopyalayabilirsiniz dağıtımı için dağıtım mühendisinize sertifikalarınızı güvenli bir şekilde aktarın. [Azure Stack PKI gereksinimleri belgeleri](azure-stack-pki-certs.md).
 - Gizli dönüş izleyerek Azure Stack ortamınızın genel altyapı uç noktalar için eski sertifikaları güncelleştirmek için sertifikaları kullanabilirsiniz [Azure Stack gizli döndürme belgeleri](azure-stack-rotate-secrets.md).
 - PaaS Hizmetleri için takip ederek Azure Stack'te SQL, MySQL ve App Services kaynak sağlayıcılarını yüklemek için sertifikaları kullanabilirsiniz [Azure Stack belgeleri hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

[Veri merkezi kimlik tümleştirme](azure-stack-integrate-identity.md)
