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
ms.date: 04/11/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: cd917165804314f6ee4ee006e3f29263d8d4b4c5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
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
    SAN her bitiş noktasıyla ilgili DNS adlarını içeriyor veya bir destekleniyorsa joker mevcut denetler.
- **Anahtar kullanımı**  
    Dijital imza ve anahtar şifreleme anahtar kullanımı içerir ve sunucu kimlik doğrulaması ve istemci kimlik doğrulaması Gelişmiş anahtar kullanımı içeren denetler.
- **Anahtar boyutu**  
    Anahtar boyutu 2048 veya daha büyük olup olmadığını denetler.
- **Zincir sırası**  
    Sipariş doğru olduğunu doğrulama diğer sertifikaları sırasını denetler.
- **Diğer sertifikaları**  
    Diğer Sertifika PFX içinde ilgili Yaprak sertifikası ve kendi zincirinin dışında paketlenmiş olun.

> [!IMPORTANT]  
> PKI sertifikasını bir PFX dosyası olduğunu ve parola hassas bilgileri olarak değerlendirilmelidir.

## <a name="prerequisites"></a>Önkoşullar

Sisteminizin bir Azure yığın dağıtımı için PKI sertifikaları doğrulamadan önce aşağıdaki önkoşulları karşılamalıdır:

- Microsoft Azure yığın hazırlık denetleyicisi
- SSL dışarı aşağıdaki sertifikaları [hazırlık yönergeleri](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 veya Windows Server 2016

## <a name="perform-certificate-validation"></a>Sertifika doğrulaması gerçekleştirme

Hazırlama ve Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın:

1. AzsReadinessChecker bir PowerShell isteminde (5.1 veya üstü), aşağıdaki cmdlet'i çalıştırarak yükleyin:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker 
    ````

2. Sertifika dizin yapısını oluşturun. Aşağıdaki örnekte değiştirebileceğiniz `<c:\certificates>` için tercih ettiğiniz yeni bir dizin yolu.

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal' 

    $destination = 'c:\certificates' 

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
    ````

 - Önceki adımda oluşturduğunuz uygun dizinlerde sertifikalarınız yerleştirin. Örneğin:  
    - c:\certificates\ACSBlob\CustomerCertificate.pfx 
    - c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx 
    - c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx 
    - ve dahası... 

3. Çalıştırma PowerShell penceresinde:

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ````

4. Tüm sertifikaları testlerini geçtiğini doğrulamak için çıktıyı inceleyin. Örneğin:

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
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessReport.json
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

## <a name="using-validated-certificates"></a>Doğrulanmış sertifikaları kullanma

Sertifikalarınızı AzsReadinessChecker tarafından doğrulandıktan sonra Azure yığın dağıtımınızdaki veya Azure yığın gizli döndürme için kullanıma hazır. 

 - Böylece bunlar bunları belirtildiği gibi dağıtım ana bilgisayar üzerine kopyalayabilirsiniz dağıtımı için dağıtım mühendisinize sertifikalarınızı güvenli bir şekilde aktarım. [Azure yığın PKI gereksinimleri belgelerine](azure-stack-pki-certs.md).
 - Gizli dönüş izleyerek Azure yığın ortamı ortak altyapısı uç noktalar için eski sertifikalar güncelleştirmek için sertifikaları kullanabilirsiniz [Azure yığın gizli döndürme belgelerine](azure-stack-rotate-secrets.md).

## <a name="next-steps"></a>Sonraki adımlar

[Veri merkezi kimlik tümleştirme](azure-stack-integrate-identity.md)
