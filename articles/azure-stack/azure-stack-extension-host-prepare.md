---
title: Azure Stack için uzantısı konağı için hazırlama | Microsoft Docs
description: Gelecekteki bir Azure Stack güncelleştirme paketi otomatik olarak etkinleştirilir uzantısı ana bilgisayarı hazırlamak öğrenin.
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 11/02/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.openlocfilehash: 4376b9e89aeef32987f7a3bb29ca6815e941ba00
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960251"
---
# <a name="prepare-for-extension-host-for-azure-stack"></a>Azure Stack için uzantısı konağı için hazırlama

Azure Stack gerekli TCP/IP bağlantı noktası sayısını azaltarak uzantısı konağı güvenliğini sağlar. Bu makalede, Azure Stack 1808 güncelleştirmesinden sonra bir Azure Stack güncelleştirme paketi otomatik olarak etkinleştirilir uzantısı konağı hazırlama konumunda ele alınmaktadır.

## <a name="certificate-requirements"></a>Sertifika gereksinimleri

Her bir portal uzantısı için benzersiz bir ana bilgisayar girişleri güvence altına almak için iki yeni etki alanı ad alanı uzantısı konağı uygular. Yeni etki alanı ad alanları, güvenli iletişim sağlamak için iki ek joker kart sertifikaları gerektirir.

Yeni ad alanları ve ilişkili sertifikaları tabloda gösterilmiştir:

| Dağıtım klasörü | Gerekli bir sertifika konusu ve konu alternatif adları (SAN) | Kapsam (bölge başına) | Alt etki alanı ad alanı |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| Yönetici uzantısı konağı | *.adminhosting. \<bölge >. \<fqdn > (joker SSL sertifikaları) | Yönetici uzantısı konağı | adminhosting. \<bölge >. \<fqdn > |
| Genel uzantı konak | * .hosting. \<bölge >. \<fqdn > (joker SSL sertifikaları) | Genel uzantı konak | barındırma. \<bölge >. \<fqdn > |

Ayrıntılı sertifika gereksinimleri bulunabilir [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri](azure-stack-pki-certs.md) makalesi.

## <a name="create-certificate-signing-request"></a>Sertifika imzalama isteği oluşturma

Azure Stack hazırlık Denetleyicisi Aracı'nı sertifika imzalama isteği için iki yeni, gerekli SSL sertifikaları oluşturma olanağı sağlar. Bu makaledeki adımları [Azure Stack sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md).

> [!Note]  
> SSL sertifikalarınızı istediğiniz nasıl bağlı olarak bu adımı atlayabilirsiniz.

## <a name="validate-new-certificates"></a>Yeni sertifika doğrulama

1. Yükseltilmiş izin donanım yaşam döngüsü konağında veya Azure Stack yönetim iş istasyonu ile PowerShell'i açın.
2. Azure Stack hazırlık Denetleyicisi aracı yüklemek için aşağıdaki cmdlet'i çalıştırın.

    ```PowerShell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

3. Gerekli bir klasör yapısını oluşturmak için aşağıdaki betiği çalıştırın:

    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > Azure Active Directory Federasyon Hizmetleri ile (AD FS) dağıtırsanız, aşağıdaki dizinlerin eklenmelidir **$directories** betikteki: `ADFS`, `Graph`.

4. Sertifika denetimi başlatmak için aşağıdaki cmdlet'leri çalıştırın:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ```

5. Uygun dizinler sertifikalarınız yerleştirin.

6. Çıktı ve tüm sertifikaları geçirmek tüm testleri denetleyin.


## <a name="import-extension-host-certificates"></a>Uzantı ana bilgisayar sertifikaları içeri aktarma

Sonraki adımlar için Azure Stack ayrıcalıklı uç noktasına bağlanabilir bir bilgisayar kullanın. Yeni sertifika dosyaları için bu bilgisayara erişimi olduğundan emin olun.

1. Sonraki adımlar için Azure Stack ayrıcalıklı uç noktasına bağlanabilir bir bilgisayar kullanın. Yeni sertifika dosyaları için bu bilgisayara erişmek emin olun.
2. Sonraki komut dosyası blokları yürütmek için PowerShell ISE'yi açın
3. Uç nokta barındırmak için sertifika alın. Betik ortamınızla eşleşecek şekilde ayarlayın.
4. Uç noktayı barındıran yönetim sertifikası alın.

    ```PowerShell  

    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```
5. Barındırma uç noktası için sertifika alın.
    ```PowerShell  
    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$HostingCertContent = [Byte[]](Get-Content c:\certificate\myhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```



### <a name="update-dns-configuration"></a>DNS yapılandırmasını güncelleştirme

> [!Note]  
> DNS tümleştirme için DNS bölgesini temsilci olarak seçme kullandıysanız, bu adım gerekli değildir.
Azure Stack uç noktalarını yayımlama için ayrı ayrı konak A kaydı yapılandırıldıysa, iki ek konak A kaydı oluşturmanız gerekir:

| IP | Ana Bilgisayar Adı | Tür |
|----|------------------------------|------|
| \<IP &GT; | Adminhosting. <Region>.<FQDN> | A |
| \<IP &GT; | Barındırma. <Region>.<FQDN> | A |

Ayrılmış IP'leri kullanan ayrıcalıklı uç noktasını cmdlet çalıştırılarak alınabilir **Get-AzureStackStampInformation**.

### <a name="ports-and-protocols"></a>Bağlantı noktaları ve protokoller

Makale [Azure Stack'i veri merkezi tümleştirmesi - uç noktalarını yayımlama](azure-stack-integrate-endpoints.md), gelen iletişim istekleri uzantısı konak dağıtımı önce Azure Stack yayımlamak için gerekli protokoller ve bağlantı noktalarını kapsar.

### <a name="publish-new-endpoints"></a>Yeni uç noktalarını yayımlama

Duvarınızda yayımlanması için gereken iki yeni uç nokta vardır. Ayrılmış IP'ler genel VIP havuzundan cmdlet kullanılarak alınabilir **Get-AzureStackStampInformation**.

> [!Note]  
> Uzantısı konağı etkinleştirmeden önce bu değişikliği yapın. Bu, Azure Stack portalı sürekli olarak erişebilmesini sağlar.

| Uç nokta (VIP) | Protokol | Bağlantı Noktaları |
|----------------|----------|-------|
| AdminHosting | HTTPS | 443 |
| Barındırma | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>Varolan Yayımlama Kuralları (Post etkinleştirme uzantısı konağının) güncelleştirme

> [!Note]  
> Azure Stack güncelleştirme paketi 1808 mu **değil** uzantısı konağı henüz etkinleştirin. Uzantı ana bilgisayar için gerekli sertifikaları içeri aktararak hazırlamak için sağlar. Uzantısı konağı 1808 güncelleştirmesinden sonra otomatik olarak bir Azure Stack güncelleştirme paketi etkinleştirilmeden önce herhangi bir bağlantı noktası kapatmayın.

Aşağıdaki mevcut uç nokta bağlantı noktası, mevcut güvenlik duvarı kurallarında kapatılmalıdır.

> [!Note]  
> Doğrulama başarılı olduktan sonra bu bağlantı noktalarını kapatmanız önerilir.

| Uç nokta (VIP) | Protokol | Bağlantı Noktaları |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| Portal (Yönetici) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015 |
| Portal (kullanıcı) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003 |
| Azure Resource Manager (Yönetici) | HTTPS | 30024 |
| Azure Resource Manager (kullanıcı) | HTTPS | 30024 |

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [güvenlik duvarı tümleştirmesi](azure-stack-firewall.md).
- Hakkında bilgi edinin [Azure Stack sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md)
