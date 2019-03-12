---
title: Azure Stack için sertifika sorunları düzeltmek | Microsoft Docs
description: Gözden geçirin ve sertifika sorunlarını düzeltmek için Azure Stack hazırlık Denetleyicisi'ni kullanın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/19/2018
ms.openlocfilehash: 009eb56621f7cd395c3d2eefb29b9fa624af888b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57778204"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Azure Stack PKI sertifikaları için ortak bir sorunu düzeltmenizi

Bu makaledeki bilgiler, anlamanıza ve Azure Stack PKI sertifikaları için yaygın sorunları çözmenize yardımcı olabilir. Azure Stack hazırlık denetleyicisi aracına kullandığınızda sorunlarını bulabilir [Azure Stack PKI sertifikalarını doğrulamak](azure-stack-validate-pki-certs.md). Sertifikalar bir Azure Stack dağıtımı ve Azure Stack gizli döndürme PKI gereksinimlerini karşılamak ve sonuçları oturum açtığında emin olmak için Aracı'nı denetler. bir [report.json dosya](azure-stack-validation-report.md).  

## <a name="pfx-encryption"></a>PFX şifreleme

**Hata** -PFX şifreleme TripleDES SHA1 değil.

**Düzeltme** -dışarı aktarma PFX dosyaları ile **TripleDES SHA1** şifreleme. Bu Sertifika ek bileşenini veya kullanarak dışa aktarırken tüm Windows 10 istemcileri için varsayılan değerdir `Export-PFXCertificate`.

## <a name="read-pfx"></a>Read PFX

**Uyarı** -parola yalnızca özel bilgileri korur.  

**Düzeltme** -dışarı aktarma PFX dosyaları için isteğe bağlı ayar ile **etkinleştirme sertifika gizlilik**.  

**Hata** -PFX dosyası geçersiz.  

**Düzeltme** -adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>İmza algoritması

**Hata** -imza algoritması olan SHA1.

**Düzeltme** -Azure Stack sertifika isteği oluşturma sertifika imzalama isteği (CSR) SHA256 ' imza algoritması ile yeniden imzalama adımları kullanın. Ardından CSR sertifika yetkilisi sertifikası vermeniz için yeniden gönderin.

## <a name="private-key"></a>Özel anahtar

**Hata** -özel anahtar eksik veya yerel makine özniteliği içermiyor.  

**Düzeltme** - CSR oluşturulan bir bilgisayardan adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment). Bu adımlar, yerel makine sertifika depolama alanından dışarı aktarma içerir.

## <a name="certificate-chain"></a>Sertifika zinciri

**Hata** -sertifika zinciri tam değil.  

**Düzeltme** -sertifikalar, eksiksiz bir sertifika zinciri içermelidir. İçindeki adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.**

## <a name="dns-names"></a>DNS adları

**Hata** -Azure Stack hizmet uç noktası adı ya da geçerli bir joker karakter eşleşmesi sertifikadaki DNSNameList içermiyor. Joker karakter eşleşme yalnızca DNS adının en solundaki ad alanı için geçerli değildir. Örneğin, _*. region.domain.com_ yalnızca geçerlidir *portal.region.domain.com*değil _*. table.region.domain.com_.

**Düzeltme** -Azure Stack uç noktaları desteklemek için Azure Stack sertifika imzalama isteği oluşturma doğru DNS adlarına sahip CSR yeniden oluşturmak adımları kullanın. Bir sertifika yetkilisi için CSR'yi yeniden gönderin ve ardından adımları [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) CSR'yi oluşturulan makineden sertifikasını dışarı aktarmak için.  

## <a name="key-usage"></a>Anahtar kullanımı

**Hata** - dijital imza veya anahtar şifreleme anahtar kullanımı eksik veya sunucu kimlik doğrulaması veya istemci kimlik doğrulaması Gelişmiş anahtar kullanımı eksik.  

**Düzeltme** -kullanmak adımda [Azure Stack sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md) doğru anahtar kullanımı öznitelikleri olan CSR yeniden oluşturmak. Sertifika yetkilisi için CSR'yi yeniden gönderin ve bir sertifika şablonu anahtar kullanımı istek üzerine değil onaylayın.

## <a name="key-size"></a>Anahtar boyutu

**Hata** -anahtar boyutu 2048 daha küçük.

**Düzeltme** -kullanmak adımda [Azure Stack sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md) CSR'yi doğru anahtar uzunluğu (2048) ile yeniden oluşturun ve ardından sertifika yetkilisi için CSR'yi yeniden gönderin.

## <a name="chain-order"></a>Zincir sırası

**Hata** -sertifika zinciri sırası yanlış.  

**Düzeltme** -adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.** Yalnızca yaprak sertifikayı dışarı aktarma için seçildiğinden emin olun.

## <a name="other-certificates"></a>Diğer sertifikaları

**Hata** -yaprak sertifika veya sertifika zinciri parçası olmayan bir sertifika PFX paketi içerir.  

**Düzeltme** -adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment)ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.** Yalnızca yaprak sertifikayı dışarı aktarma için seçildiğinden emin olun.

## <a name="fix-common-packaging-issues"></a>Paketleme yaygın sorunları çözme

**AzsReadinessChecker** adlı bir yardımcı cmdlet'ini içeren `Repair-AzsPfxCertificate`, hangi içeri aktarabilir ve sonra dışarı aktarma bir PFX dosyası dahil olmak üzere, ortak paketleme sorunları düzeltmek için:

- *PFX şifreleme* TripleDES SHA1 değil.
- *Özel anahtar* yerel makine özniteliği eksik.
- *Sertifika zinciri* eksik veya yanlış. PFX paketi yoksa yerel makine sertifika zinciri içermelidir.
- *Diğer sertifikaları*

`Repair-AzsPfxCertificate` Yeni bir CSR ve bir sertifika yeniden gönderin gerekiyorsa yardımcı olamaz.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar, aracın çalıştığı bilgisayarda yerinde olmalıdır:

- Windows 10 veya Windows Server 2016, internet bağlantısı.
- PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell cmdlet'ini çalıştırın ve daha sonra gözden *ana* ve *küçük* sürümleri:

   ```powershell
   $PSVersionTable.PSVersion
   ```

- Yapılandırma [Azure Stack için PowerShell](azure-stack-powershell-install.md).
- En son sürümünü indirin [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

### <a name="import-and-export-an-existing-pfx-file"></a>Mevcut bir PFX dosyasını içeri ve dışarı

1. Önkoşulları karşılayan bir bilgisayarda, yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. PowerShell isteminden PFX parolasını ayarlamak için aşağıdaki cmdlet'i çalıştırın. Değiştirin `PFXpassword` gerçek parola ile:

   ```powershell
   $password = Read-Host -Prompt PFXpassword -AsSecureString
   ```

3. PowerShell komut isteminde, yeni bir PFX dosyasını dışarı aktarmak için aşağıdaki komutu çalıştırın:

   - İçin `-PfxPath`, birlikte çalıştığınız PFX dosyasının yolunu belirtin. Aşağıdaki örnekte yoludur `.\certificates\ssl.pfx`.
   - İçin `-ExportPFXPath`, dışarı aktarma için PFX dosyasının adını ve konumunu belirtin. Aşağıdaki örnekte yoludur `.\certificates\ssl_new.pfx`:

   ```powershell
   Repair-AzsPfxCertificate -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
   ```  

4. Araç tamamladıktan sonra başarı için çıktıyı gözden geçirin:

   ```powershell
   Repair-AzsPfxCertificate v1.1809.1005.1 started.
   Starting Azure Stack Certificate Import/Export
   Importing PFX .\certificates\ssl.pfx into Local Machine Store
   Exporting certificate to .\certificates\ssl_new.pfx
   Export complete. Removing certificate from the local machine store.
   Removal complete.
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Repair-AzsPfxCertificate Completed
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güvenliği hakkında daha fazla bilgi için buraya gidin](azure-stack-rotate-secrets.md).
