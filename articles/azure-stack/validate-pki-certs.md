---
title: Azure tümleşik yığını systems dağıtımı Azure yığın ortak anahtar altyapısı sertifikalarını doğrulamak | Microsoft Docs
description: Azure tümleşik yığını sistemleri Azure yığın PKI sertifikalarını doğrulamak açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: b38e3cc45d14645611c0cd804f2bfa66047810f0
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2018
---
# <a name="validate-azure-stack-pki-certificates"></a>Azure yığın PKI sertifikaları doğrula
Bu makalede açıklanan Azure yığın sertifika Denetleyicisi aracı doğrulamak için deploymentdata.json dosyasına dahil OEM tarafından sağlanan [PKI sertifikaları oluşturulan](azure-stack-get-pki-certs.md) dağıtım öncesi uygundur. Sertifikaları test edin ve gerekirse yeniden sertifikaları almak için yeterli süre doğrulanmalıdır. 

Sertifika Denetleyicisi aracını (Certchecker) aşağıdaki denetimleri gerçekleştirir:

- **PFX okuma**. Doğru parolayı geçerli PFX dosyası için denetler ve ortak bilgi parola ile korunmuyor durumunda sizi uyarır. 
- **İmza algoritması**. İmza algoritması SHA1 denetler 
- **Özel anahtar**. Özel anahtarı mevcut olduğundan ve yerel makine özniteliğiyle dışarı denetler. 
- **Sertifika zinciri**. Sertifika zinciri olduğu gibi için otomatik olarak imzalanan sertifikalar dahil olmak üzere denetler. 
- **DNS adları**. SAN her bitiş noktasıyla ilgili DNS adlarını içeriyor veya bir destekleniyorsa joker mevcut denetler. 
- **Anahtar kullanımı**. Anahtar kullanımı dijital imza ve anahtar şifreleme ve sunucu kimlik doğrulaması ve istemci kimlik doğrulaması Gelişmiş anahtar kullanımı içerir denetler. 
- **Anahtar boyutu**. Anahtar boyutu 2048 veya daha büyük denetler 
- **Zincir sipariş**. Doğru zinciri yapmadan diğer sertifikaları sırasını denetler. 
- **Diğer sertifikaları**. Diğer Sertifika PFX içinde ilgili Yaprak sertifikası ve kendi zincirinin dışında paketlenmiş olun. 
- **Profil yok**. Yeni bir kullanıcı PFX verileri yüklenmiş bir kullanıcı profili olmadan sertifika bakım sırasında gMSA hesapları davranışını mimicking yükleyebilirsiniz denetler.   

> [!IMPORTANT]
> PKI sertifika PFX dosyasını ve parola hassas bilgileri olarak değerlendirilmelidir.

## <a name="prerequisites"></a>Önkoşullar
Sisteminizi Azure yığın dağıtım için PKI sertifikaları doğrulamadan önce aşağıdaki gereksinimleri karşılamalıdır:
- CertChecker (içinde PartnerToolKit \utils\certchecker altında)
- SSL dışarı aşağıdaki sertifikaları [hazırlık yönergeleri](prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 veya Windows Server 2016

## <a name="perform-certificate-validation"></a>Sertifika doğrulaması gerçekleştirme
Hazırlama ve Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın: 

1. İçeriği Ayıkla <partnerToolkit>yeni bir dizin, örneğin, \utils\certchecker **c:\certchecker**.

2. Yönetici olarak PowerShell'i açın ve certchecker klasörüne dizini değiştirin:

  ```powershell
  cd c:\certchecker
  ```
 
3. Aşağıdaki PowerShell komutlarını çalıştırarak sertifikalar için bir dizin yapısını oluşturun:

  ```powershell 
  $directories = "ACS","ADFS","Admin Portal","ARM Admin","ARM Public","Graph","KeyVault","KeyVaultInternal","Public Portal" 
  $destination = '.\certs' 
  $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
  ```

  >  [!NOTE]
  >  Kimlik sağlayıcısı Azure yığın dağıtımı için Azure AD, kullanılmıyorsa **ADFS** ve **grafik** dizinleri. 

4. Örneğin önceki adımda oluşturduğunuz uygun dizinlerde sertifikalarınız koyun: 
  - c:\certchecker\Certs\ACS\CustomerCertificate.pfx,  
  - c:\certchecker\Certs\Admin Portal\CustomerCertificate.pfx  
  - c:\certchecker\Certs\ARM Admin\CustomerCertificate.pfx  
  - ve dahası... 

5. Kopya **deploymentdata.json** içine **c:\certchecker** dizini.

6. PowerShell penceresinde aşağıdaki komutları çalıştırın: 

  ```powershell
  $password = Read-Host -Prompt "Enter PFX Password" -AsSecureString 
  .\CertChecker.ps1 -CertificatePath .\Certs\ -pfxPassword $password -deploymentDataJSONPath .\DeploymentData.json  
  ```

7. Çıktı Tamam tüm sertifikaların ve kullanıma tüm öznitelikleri için içermelidir: 

  ```powershell
  Starting Azure Stack Certificate Validation 1.1802.221.1
  Testing: ADFS\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: KeyVaultInternal\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: ACS\ContosoSSL.pfx
  WARNING: Pre-1803 certificate structure. The folder structure for Azure Stack 1803 and above is: ACSBlob, ACSQueue, ACSTable instead of ACS folder. Refer to deployment documentation for further informat
  ion.
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Detailed log can be found C:\CertChecker\CertChecker.log 
  ```

### <a name="known-issues"></a>Bilinen sorunlar 
**Belirti**: Certchecker çıkar erken ve şu hatayı alıyorsunuz: 
> Başarısız

> Ayrıntı: Bu komut hata nedeniyle çalıştırılamaz: dizin adı geçersiz. 

**Neden**: certchecker.ps1 çalıştıran kısıtlayıcı klasörden, örneğin, c:\temp veya % temp % 

**Çözümleme**: yeni dizine, örneğin, C:\CertChecker certchecker aracı Taşı 


**Belirti**: Certchecker (örnektekiyle adım 7) öncesi 1803 kullanma hakkında bir uyarı verir:

> [!WARNING]
> Öncesi 1803 sertifika yapısı. Klasör yapısı için Azure yığın 1803 ve yukarıdaki: ACS klasörü yerine ACSBlob, ACSQueue, ACSTable. Daha fazla bilgi için dağıtım belgelerine bakın.

**Neden**: CertChecker 1803 önce bu dağıtımlar için doğru ise tek bir ACS klasör kullanımı algılandı. Azure yığın sürüm 1803 ve dağıtımları yukarıda ACSTable, ACSQueue, ACSBlob klasör yapısını değiştirir. Certchecker zaten olması bu işlevleri destekleyen şekilde güncelleştirildi.

**Çözümleme**: 1802 dağıtma, hiçbir eylem gerekli değildir. 1803 dağıtma ve ACS ACSTable, ACSQueue, ACSBlob ile değiştirin ve ACS sertifikaları bu klasörlerine kopyalayın.

**Belirti**: testleri atlanır

**Neden**: CertChecker atlar belirli testleri bir bağımlılık karşılanır değil ise:
- Sertifika zinciri başarısız olursa, diğer sertifikaları atlanır.
- Profil yok atlanır:
  - Bir güvenlik ilkesi geçici bir kullanıcı oluşturun ve bu kullanıcı olarak powershell çalıştırma yeteneğini kısıtlama yoktur.
  - Özel anahtar denetimi başarısız olur.

**Çözümleme**: Test kümesinin her sertifikanın Ayrıntılar bölümünde yer alan araçları yönergeleri izleyin.


## <a name="prepare-deployment-script-certificates"></a>Dağıtım betiği sertifikalar hazırlama 
Son adım olarak, hazır tüm sertifikaları dağıtımı konakta uygun dizinlerde yerleştirilmesi gerekir. Dağıtım konakta adlı bir klasör oluşturun. Sertifikaları ** ve Yerleştir, dışarı aktarılan sertifika dosyaları belirtilen karşılık gelen alt klasörlerdeki [zorunlu sertifikaları](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates) bölümü:

```
\Certificates
\ACS\ssl.pfx
\Admin Portal\ssl.pfx
\ARM Admin\ssl.pfx
\ARM Public\ssl.pfx
\KeyVault\ssl.pfx
\KeyVaultInternal\ssl.pfx
\Public Portal\ssl.pfx
\ADFS\ssl.pfx*
\Graph\ssl.pfx*
```

<sup>*</sup> Sertifikaları yıldızla işaretlenmiş * AD FS kimlik deposu olarak kullanıldığında, yalnızca gerekli.


## <a name="next-steps"></a>Sonraki adımlar
[Veri merkezi kimlik tümleştirme](azure-stack-integrate-identity.md)