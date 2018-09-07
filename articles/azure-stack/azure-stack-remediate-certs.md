---
title: Azure Stack için sertifika sorunları düzeltmek | Microsoft Docs
description: Gözden geçirin ve sertifika sorunlarını düzeltmek için Azure Stack hazırlık Denetleyicisi'ni kullanın.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 6bc7839e7db0022beaa9b31c390655f31d1d52c0
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053474"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Azure Stack PKI sertifikaları için ortak bir sorunu düzeltmenizi
Bu makaledeki bilgiler, anlamanıza ve Azure Stack PKI sertifikaları için yaygın sorunları çözmenize yardımcı olabilir. Azure Stack hazırlık denetleyicisi aracına kullandığınızda sorunlarını bulabilir [Azure Stack PKI sertifikalarını doğrulamak](azure-stack-validate-pki-certs.md). Sertifikalar bir Azure Stack dağıtımı ve Azure Stack gizli döndürme PKI gereksinimlerini karşılamak ve sonuçları oturum açtığında emin olmak için Aracı'nı denetler. bir [report.json dosya](azure-stack-validation-report.md).  

## <a name="pfx-encryption"></a>PFX şifreleme
**Hata** -PFX şifreleme TripleDES SHA1 değil.   
**Düzeltme** -dışarı aktarma PFX dosyaları ile **TripleDES SHA1** şifreleme. Tüm Windows 10 sertifika yaslama dışarı aktarma veya dışarı aktarma-PFXCertificate kullanırken istemcileri için varsayılan değer budur. 

## <a name="read-pfx"></a>PFX okuyun
**Uyarı** -parola yalnızca özel bilgileri korur.  
**Düzeltme** -isteğe bağlı ayarı ile PFX dosyalarını dışarı öneririz **etkinleştirme sertifika gizlilik**.  

**Hata** -PFX dosyası geçersiz.  
**Düzeltme** -adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>İmza algoritması
**Hata** -imza algoritması olan SHA1.    
**Düzeltme** -Azure Stack sertifika isteği oluşturma sertifika imzalama isteği (CSR), imza algoritması SHA256 olan yeniden imzalama adımları kullanın. Ardından CSR sertifika yetkilisi sertifikası vermeniz için yeniden gönderin.

## <a name="private-key"></a>Özel anahtar
**Hata** -özel anahtar eksik veya yerel makine özniteliği içermiyor.  
**Düzeltme** - CSR oluşturulan bir bilgisayardan yeniden dağıtım için hazırlama Azure Stack PKI sertifikaları adımları kullanarak sertifikayı dışarı aktarın. Bu adımlar, yerel makine sertifika depolama alanından dışarı aktarma içerir.

## <a name="certificate-chain"></a>Sertifika zinciri
**Hata** -sertifika zinciri tam değil.  
**Düzeltme** -sertifikalar, eksiksiz bir sertifika zinciri içermelidir.  İçindeki adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md) ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.**

## <a name="dns-names"></a>DNS Adları
**Hata** -Azure Stack hizmet uç noktası adı ya da geçerli bir joker karakter eşleşmesi DNSNameList sertifikadaki içermiyor.  Joker karakter eşleşme yalnızca DNS adının en solundaki ad alanı için geçerli değildir. Örneğin, _*. region.domain.com_ yalnızca geçerlidir *portal.region.domain.com*değil _*. table.region.domain.com_.  
**Düzeltme** -Azure Stack uç noktaları desteklemek için Azure Stack sertifika imzalama isteği oluşturma doğru DNS adlarına sahip CSR yeniden oluşturmak adımları kullanın. Bir sertifika yetkilisi için CSR'yi yeniden gönderin ve ardından adımları [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md) CSR'yi oluşturulan makineden sertifikasını dışarı aktarmak için.  

## <a name="key-usage"></a>Anahtar kullanımı
**Hata** - anahtar kullanımı, dijital imzası eksik veya sunucu kimlik doğrulaması veya istemci kimlik doğrulaması, anahtar şifreleme orEnhanced anahtar kullanımı eksik.  
**Düzeltme** -kullanmak adımda [Azure Stack sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md) doğru anahtar kullanımı öznitelikleri olan CSR yeniden oluşturmak.  Sertifika yetkilisi için CSR'yi yeniden gönderin ve bir sertifika şablonu anahtar kullanımı istek üzerine değil onaylayın.

## <a name="key-size"></a>Anahtar Boyutu
**Hata** -anahtar boyutu 2048 daha küçük    
**Düzeltme** -kullanmak adımda [Azure Stack sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md) CSR'yi doğru anahtar uzunluğu (2048) ile yeniden oluşturun ve ardından sertifika yetkilisi için CSR'yi yeniden gönderin.

## <a name="chain-order"></a>Zincir sırası
**Hata** -sertifika zinciri sırası yanlış.  
**Düzeltme** -adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md) ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.** Yalnızca yaprak sertifikayı dışarı aktarma için seçildiğinden emin olun. 

## <a name="other-certificates"></a>Diğer sertifikaları
**Hata** -yaprak sertifika veya sertifika zinciri parçası olmayan bir sertifika PFX paketi içerir.  
**Düzeltme** -adımları kullanarak sertifikayı yeniden dışarı [dağıtımı için hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md)ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.** Yalnızca yaprak sertifikayı dışarı aktarma için seçildiğinden emin olun.

## <a name="fix-common-packaging-issues"></a>Paketleme yaygın sorunları çözme
AzsReadinessChecker içeri aktarabilir ve sonra bir PFX dosyası dahil olmak üzere, ortak paketleme sorunları gidermek için ver: 
 - *PFX şifreleme* TripleDES SHA1 değil
 - *Özel anahtar* yerel makine özniteliği eksik.
 - *Sertifika zinciri* eksik veya yanlış. (PFX paketi yoksa yerel makine sertifika zinciri içermelidir.) 
 - *Diğer sertifikaları*.
Ancak, yeni bir CSR ve bir sertifika yeniden gönderin gerekiyorsa AzsReadinessChecker yardımcı olamaz. 

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki Önkoşullar aracın çalıştığı bilgisayarda bulunması gerekir: 
 - Windows 10 veya Windows Server 2016, internet bağlantısı.
 - PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd'yi çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri.

   > `$PSVersionTable.PSVersion`
 - Yapılandırma [Azure Stack için PowerShell](azure-stack-powershell-install.md). 
 - En son sürümünü indirin [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

### <a name="import-and-export-an-existing-pfx-file"></a>Mevcut bir PFX dosyasını içeri ve dışarı
1. Önkoşulları karşılayan bir bilgisayarda, yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın  
   > `Install-Module Microsoft.AzureStack.ReadinessChecker- Force`

2. PowerShell isteminden PFX parolasını ayarlamak için aşağıdaki komutu çalıştırın. Değiştirin *PFXpassword* gerçek parolayla. 
   > `$password = Read-Host -Prompt PFXpassword -AsSecureString`

3. PowerShell komut isteminde, yeni bir PFX dosyasını dışarı aktarmak için aşağıdaki komutu çalıştırın.
   - İçin *- PfxPath*, birlikte çalıştığınız PFX dosyasının yolunu belirtin.  Aşağıdaki örnekte yoludur *.\certificates\ssl.pfx*.
   - İçin *- ExportPFXPath*, dışarı aktarma için PFX dosyasının adını ve konumunu belirtin.  Aşağıdaki örnekte yoludur *.\certificates\ssl_new.pfx*

   > `Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx`  

4. Araç tamamladıktan sonra başarı için çıktıyı gözden geçirin: ![sonuçları](./media/azure-stack-remediate-certs/remediate-results.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-rotate-secrets.md)
