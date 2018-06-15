---
title: Sertifika sorunlarını düzeltmek için Azure yığınına | Microsoft Docs
description: Gözden geçirin ve sertifika sorunları çözmek için Azure yığın hazırlık Denetleyicisi'ni kullanın.
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
ms.openlocfilehash: 0d2c4d848f861e4e07dbd0de4609344955ca26f7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937856"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Azure yığın PKI sertifikaları için ortak sorunları çözmek
Bu makaledeki bilgiler anlamanıza ve Azure yığın PKI sertifikaları için sık karşılaşılan sorunları gidermeye yardımcı olabilir. Azure yığın hazırlık Denetleyicisi aracını kullandığınızda sorunları bulabilir [Azure yığın PKI sertifikalarını doğrulamak](azure-stack-validate-pki-certs.md). Sertifikalar bir Azure yığın dağıtımına ve Azure yığın gizli döndürme PKI gereksinimlerini karşılamak ve sonuçları oturum açtığında emin olmak için aracı denetler bir [report.json dosya](azure-stack-validation-report.md).  

## <a name="read-pfx"></a>PFX okuma
**Uyarı** -parola yalnızca sertifikadaki özel bilgiler korur.  
**Düzeltme** -için isteğe bağlı ayar ile PFX dosyalarını dışarı öneririz **etkinleştirmek sertifika gizlilik**.  

**Hata** -PFX dosyası geçersiz.  
**Düzeltme** -adımları kullanarak sertifikayı yeniden vermeniz [dağıtım için hazırlama Azure yığın PKI sertifikaları](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>İmza algoritması
**Hata** -imza algoritması olan SHA1.    
**Düzeltme** -Azure yığın sertifikalar sertifika imzalama isteği (CSR), imza algoritması SHA256 ile yeniden oluşturmak için istek nesil imzalama adımları kullanın. Sonra sertifikayı yeniden vermeniz sertifika yetkilisine CSR yeniden gönderin.

## <a name="private-key"></a>Özel anahtar
**Hata** -özel anahtarı eksik veya yerel makine özniteliğini içermiyor.  
**Düzeltme** - CSR oluşturulan bilgisayardan adımları dağıtım için hazırlama Azure yığın PKI sertifikalarını kullanarak sertifikayı yeniden verin. Bu adımlar yerel makine sertifika deposundan dışarı aktarma içerir.

## <a name="certificate-chain"></a>Sertifika zinciri
**Hata** -sertifika zinciri tam değil.  
**Düzeltme** -sertifikalar, eksiksiz bir sertifika zinciri içermelidir.  ' Ndaki adımları kullanarak sertifikayı yeniden vermeniz [dağıtım için hazırlama Azure yığın PKI sertifikaları](azure-stack-prepare-pki-certs.md) ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.**

## <a name="dns-names"></a>DNS Adları
**Hata** -Azure yığın Hizmeti uç nokta adı ya da geçerli bir joker karakter eşleştirme sertifikadaki DNSNameList içermiyor.  Joker karakter eşleşmeleri yalnızca DNS adının en solundaki ad alanı için geçerlidir. Örneğin, _*. region.domain.com_ yalnızca için geçerlidir *portal.region.domain.com*değil _*. table.region.domain.com_.  
**Düzeltme** -Azure yığın uç noktaları desteklemek için Azure yığın sertifikaları doğru DNS adları olan CSR yeniden oluşturmak için istek nesil imzalama adımları kullanın. Bir sertifika yetkilisine CSR yeniden gönderin ve ardından adımları [dağıtım için hazırlama Azure yığın PKI sertifikaları](azure-stack-prepare-pki-certs.md) CSR oluşturulan makineden sertifika vermek için.  

## <a name="key-usage"></a>Anahtar kullanımı
**Hata** - anahtar kullanımı, dijital imza eksik veya anahtar şifreleme, orEnhanced anahtar kullanımı, sunucu kimlik doğrulaması veya istemci kimlik doğrulaması eksik.  
**Düzeltme** -içindeki adımları kullanın [Azure yığın sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md) doğru anahtar kullanımı özniteliklerle CSR yeniden oluşturmak için.  Sertifika yetkilisine CSR yeniden gönderin ve bir sertifika şablonu istekte anahtar kullanımı üzerine değildir onaylayın.

## <a name="key-size"></a>Anahtar Boyutu
**Hata** -anahtar boyutu 2048'den küçük    
**Düzeltme** -içindeki adımları kullanın [Azure yığın sertifika imzalama isteği oluşturma](azure-stack-get-pki-certs.md) CSR doğru anahtar uzunluğu (2048) ile yeniden oluşturmak ve sertifika yetkilisine CSR yeniden gönderin.

## <a name="chain-order"></a>Zincir sırası
**Hata** -sertifika zinciri yanlış sırasıdır.  
**Düzeltme** -adımları kullanarak sertifikayı yeniden vermeniz [dağıtım için hazırlama Azure yığın PKI sertifikaları](azure-stack-prepare-pki-certs.md) ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.** Yalnızca Yaprak sertifikası dışarı aktarma için seçili olduğundan emin olun. 

## <a name="other-certificates"></a>Diğer sertifikaları
**Hata** -PFX paketi yaprak sertifika veya sertifika zinciri parçası olmayan sertifikaları içerir.  
**Düzeltme** -adımları kullanarak sertifikayı yeniden vermeniz [dağıtım için hazırlama Azure yığın PKI sertifikaları](azure-stack-prepare-pki-certs.md)ve seçeneğini **mümkünse sertifika yolundaki tüm sertifikaları dahil et.** Yalnızca Yaprak sertifikası dışarı aktarma için seçili olduğundan emin olun.

## <a name="fix-common-packaging-issues"></a>Ortak paketleme ilgili sorunları giderme
AzsReadinessChecker alabilir ve ortak paketleme sorunlarını gidermeye yönelik bir PFX dosyasına dışarı aktar: 
 - *Özel anahtar* yerel makine özniteliği eksik.
 - *Sertifika zinciri* eksik ya da yanlış. (PFX paketi yoksa yerel makine sertifika zinciri içermelidir.) 
 - *Diğer sertifikaları*.
Ancak, bir sertifika reddetmesini ve yeni bir CSR gerekiyorsa AzsReadinessChecker yardımcı olamaz. 

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulları aracın çalıştığı bilgisayarda karşılanması gerekir: 
 - Windows 10 veya Windows Server 2016 Internet bağlantısına sahip.
 - PowerShell 5.1 veya üstü. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri.

   > `$PSVersionTable.PSVersion`
 - Yapılandırma [Azure yığını için PowerShell](azure-stack-powershell-install.md). 
 - En son sürümünü indirme [Microsoft Azure yığın hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

### <a name="import-and-export-an-existing-pfx-file"></a>İçeri ve dışarı aktarma varolan bir PFX dosyası
1. Önkoşulları karşılayan bir bilgisayar, bir yönetici PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın  
   > `Install-Module Microsoft.AzureStack.ReadinessChecker- Force`

2. PFX parolası olarak ayarlamak için aşağıdaki PowerShell isteminde çalıştırın. Değiştir *PFXpassword* gerçek parola ile. 
   > `$password = Read-Host -Prompt PFXpassword -AsSecureString`

3. Yeni bir PFX dosyası dışarı aktarmak için aşağıdaki PowerShell isteminde çalıştırın.
   - İçin *- PfxPath*, çalıştığınız PFX dosyasının yolunu belirtin.  Aşağıdaki örnekte yoludur *.\certificates\ssl.pfx*.
   - İçin *- ExportPFXPath*, dışa aktarma için PFX dosyasının adını ve konumunu belirtin.  Aşağıdaki örnekte yoludur *.\certificates\ssl_new.pfx*

   > `Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx`  

4. Aracı tamamlandıktan sonra başarı için çıktıyı inceleyin: ![sonuçları](./media/azure-stack-remediate-certs/remediate-results.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın güvenliği hakkında daha fazla bilgi edinin](azure-stack-rotate-secrets.md)