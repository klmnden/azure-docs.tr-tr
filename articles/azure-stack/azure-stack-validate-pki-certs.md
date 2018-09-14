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
ms.date: 09/12/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: b041991d7be7ed6625ddddb569b276b6118ef782
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543988"
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
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal'
    
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

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD 
    ```

4. Çıktı ve tüm sertifikaları geçirmek tüm testleri denetleyin. Örneğin:

    ```PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Validation

    Starting Azure Stack Certificate Validation 1.1803.405.3
    Testing: ARM Public\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: ACSBlob\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location: 
    C:\AzsReadinessChecker\AzsReadinessReport.json
    AzsReadinessChecker Completed
    ```

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

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Completed
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
    Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Tüm sertifikaları çıktı ve, tüm sınamaları geçmesi denetleyin.

    ```PowerShell
    AzsReadinessChecker v1.1805.425.2 started
    Starting PaaS Certificate Validation
    
    Starting Azure Stack Certificate Validation 1.0 
    Testing: PaaSCerts\wildcard.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\api.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\wildcard.dbadapter.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\sso.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
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