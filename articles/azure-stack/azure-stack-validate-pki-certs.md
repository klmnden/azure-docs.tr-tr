---
title: Azure tümleşik yığını systems dağıtımı Azure yığın ortak anahtar altyapısı sertifikalarını doğrulamak | Microsoft Docs
description: Azure tümleşik yığını sistemleri Azure yığın PKI sertifikalarını doğrulamak açıklar. Azure yığın sertifika Denetleyicisi aracı kullanmayı ele alır.
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
ms.date: 05/24/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: e381d2ed3c6a972d776dd31f311fcebe2e35823a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605619"
---
# <a name="validate-azure-stack-pki-certificates"></a>Azure yığın PKI sertifikaları doğrula

Bu makalede açıklanan Azure yığın hazırlık Denetleyicisi aracı kullanılabilir [PowerShell Galerisi'nden](https://aka.ms/AzsReadinessChecker). Aracı doğrulamak için kullanabileceğiniz [PKI sertifikaları oluşturulan](azure-stack-get-pki-certs.md) dağıtım öncesi uygundur. Sertifikaları test edin ve gerekirse sertifikaları yeniden için yeterli süre bırakarak doğrulamalıdır.

Hazırlık Denetleyicisi aracını aşağıdaki sertifika doğrulama gerçekleştirir:

- **PFX okuma**  
    Doğru parolayı geçerli PFX dosyası için denetler ve ortak bilgi parola ile korunmuyor durumunda sizi uyarır. 
- **İmza algoritması**  
    İmza algoritması SHA1 olmadığını denetler.
- **Özel anahtar**  
    Özel anahtarı mevcut olduğundan ve yerel makine özniteliğiyle dışarı denetler. 
- **Sertifika zinciri**  
    Denetimleri sertifika zinciri otomatik olarak imzalanan sertifikalar için bir denetimi dahil olmak üzere kalır.
- **DNS adları**  
    SAN her bitiş noktasıyla ilgili DNS adlarını içerdiğinden veya bir destekleniyorsa joker mevcut denetler.
- **Anahtar kullanımı**  
    Dijital imza ve anahtar şifreleme anahtar kullanımı içerir ve sunucu kimlik doğrulaması ve istemci kimlik doğrulaması Gelişmiş anahtar kullanımı içeren denetler.
- **Anahtar boyutu**  
    Anahtar boyutu 2048 veya daha büyük olup olmadığını denetler.
- **Zincir sırası**  
    Sipariş doğru olduğunu doğrulama diğer sertifikaları sırasını denetler.
- **Diğer sertifikaları**  
    Diğer Sertifika PFX içinde ilgili Yaprak sertifikası ve kendi zincirinin dışında paketlenmiş olun.
- **Profil yok**  
    Yeni bir kullanıcı PFX verileri sertifika bakım sırasında gMSA hesapları davranışını mimicking yüklenen, bir kullanıcı profili olmadan yükleyebilirsiniz denetler.

> [!IMPORTANT]  
> PKI sertifikasını bir PFX dosyası olduğunu ve parola hassas bilgileri olarak değerlendirilmelidir.

## <a name="prerequisites"></a>Önkoşullar

Sisteminizin bir Azure yığın dağıtımı için PKI sertifikaları doğrulamadan önce aşağıdaki önkoşulları karşılamalıdır:

- Microsoft Azure yığın hazırlık denetleyicisi
- SSL dışarı aşağıdaki sertifikaları [hazırlık yönergeleri](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 veya Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Çekirdek hizmetler sertifika doğrulama gerçekleştirme

Hazırlama ve dağıtım ve gizli döndürme Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın:

1. Yükleme **AzsReadinessChecker** aşağıdaki cmdlet'i çalıştırarak bir PowerShell isteminde (5.1 veya üstü):

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ````

2. Sertifika dizin yapısını oluşturun. Aşağıdaki örnekte değiştirebileceğiniz `<c:\certificates>` için tercih ettiğiniz yeni bir dizin yolu.

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ````
    
    > [!Note]  
    > Kimlik sisteminiz olarak AD FS kullanıyorsanız AD FS ve grafik gereklidir.
    
     - Önceki adımda oluşturduğunuz uygun dizinlerde sertifikalarınız yerleştirin. Örneğin:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx`

3. PowerShell penceresinde değerlerini değiştirmek **RegionName** ve **FQDN** Azure yığın ortamına uygun ve aşağıdaki komutu çalıştırın:

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD 

    ````

4. Çıkış ve tüm sertifikaları geçirmek tüm testleri denetleyin. Örneğin:

    ````PowerShell
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
    ````

### <a name="known-issues"></a>Bilinen sorunlar

**Belirti**: testleri atlanır

**Neden**: AzsReadinessChecker atlar belirli testleri bir bağımlılık karşılanır değil ise:

 - Sertifika zinciri başarısız olursa, diğer sertifikaları atlanır.

    ````PowerShell  
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
    ````

**Çözümleme**: her her sertifika için testleri kümesi altındaki ayrıntılar bölümünde aracın yönergeleri izleyin.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Platform Hizmet sertifika doğrulama gerçekleştirme

SQL/MySQL veya uygulama hizmetleri dağıtımlar planlanmış hazırlamak ve bir hizmet (PaaS) sertifikaları, platform Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın.

1.  Yükleme **AzsReadinessChecker** aşağıdaki cmdlet'i çalıştırarak bir PowerShell isteminde (5.1 veya üstü):

    ````PowerShell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ````

2.  Yolları ve doğrulama gerektiren her PaaS sertifika için parola içeren bir iç içe karma tablosu oluşturun. Çalıştırma PowerShell penceresinde:

    ```PowerShell
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Değerlerini değiştirmek **RegionName** ve **FQDN** doğrulamayı başlatmak için Azure yığın ortamınıza eşleşecek şekilde. Ardından çalıştırın:

    ```PowerShell
    Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Çıkış ve, tüm sertifikaları tüm testlerden geçtiğini denetleyin.

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

## <a name="using-validated-certificates"></a>Doğrulanmış sertifikaları kullanma

Sertifikalarınızı AzsReadinessChecker tarafından doğrulandıktan sonra Azure yığın dağıtımınızdaki veya Azure yığın gizli döndürme için kullanıma hazır. 

 - Böylece bunlar bunları belirtildiği gibi dağıtım ana bilgisayar üzerine kopyalayabilirsiniz dağıtımı için dağıtım mühendisinize sertifikalarınızı güvenli bir şekilde aktarım. [Azure yığın PKI gereksinimleri belgelerine](azure-stack-pki-certs.md).
 - Gizli dönüş izleyerek Azure yığın ortamı ortak altyapısı uç noktalar için eski sertifikalar güncelleştirmek için sertifikaları kullanabilirsiniz [Azure yığın gizli döndürme belgelerine](azure-stack-rotate-secrets.md).
 - PaaS hizmetler için SQL, MySQL ve uygulama hizmetleri kaynak sağlayıcıları Azure yığınında izleyerek yüklemek için sertifikaları kullanabilirsiniz [Azure yığın belgelerinde hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

[Veri merkezi kimlik tümleştirme](azure-stack-integrate-identity.md)