---
title: "Azure yığın Azure yığın ortak anahtar altyapısı sertifika gereksinimleri tümleşik sistemleri | Microsoft Docs"
description: "Azure tümleşik yığını sistemler için Azure yığın PKI sertifikası dağıtım gereksinimleri açıklanır."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: ppacent
ms.openlocfilehash: 75a8f521135757ceb99cb0086f331c35827e4800
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure yığın ortak anahtar altyapısı sertifika gereksinimleri
Küçük bir Azure yığın Hizmetleri ve büyük olasılıkla Kiracı VM'ler kümesine atanmış dışarıdan erişilebilir ortak IP adresleri kullanan bir ortak altyapı ağı Azure yığınına sahiptir. Bu Azure yığın ortak altyapısı uç noktalar için uygun DNS adları ile birlikte PKI sertifikalarını Azure yığın dağıtımı sırasında gereklidir. Bu makalede, hakkında bilgi sağlar:

- Hangi sertifikaların Azure yığın dağıtmak için gerekli
- Bu belirtimler eşleşen sertifikalar alma işlemi
- Hazırlama, doğrulama ve dağıtımı sırasında bu sertifikaları kullanma
> [!NOTE]
> Dağıtım sırasında sertifikaları (Azure karşı AD veya AD FS) dağıtıyorsanız kimlik sağlayıcısı eşleşen dağıtım klasörünü kopyalamanız gerekir. Tüm uç noktaları için tek bir sertifika kullanıyorsanız, aşağıdaki tabloda özetlendiği gibi her dağıtım klasörüne bu sertifika dosyasını kopyalamanız gerekir. Klasör yapısı dağıtım sanal makinede önceden oluşturulmuş ve şurada bulunabilir: C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Sertifika gereksinimleri
Aşağıdaki listede, Azure yığın dağıtmak için gerekli sertifika gereksinimleri açıklanmaktadır: 
- Bir iç sertifika yetkilisi veya bir ortak sertifika yetkilisi sertifikaları verilmesi gerekir. Bir ortak sertifika yetkilisi kullanılırsa, temel işletim sistemi görüntüsü Microsoft güvenilir kök yetkilisi programı bir parçası olarak eklenmelidir. Tam listesini burada bulabilirsiniz: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- Sertifika konu alternatif adı (SAN) alanındaki tüm ad alanlarını kapsayan tek bir joker sertifika olabilir. Alternatif olarak, depolama ve anahtar kasası gerekli olduğu gibi bitiş noktası için joker karakterler kullanarak tek tek sertifikaları kullanabilirsiniz. 
- Sertifika imza algoritması güçlü olmalıdır SHA1, olamaz. 
- Ortak ve özel anahtarlar Azure yığın yükleme için gerekli olan sertifika biçimi PFX, olması gerekir. 
- Sertifika pfx dosyaları bir değer "Dijital imza" ve "KeyEncipherment", "Anahtar kullanımı" alanında olması gerekir.
- Tüm sertifika pfx dosyalarını parolaların aynı dağıtım zamanında olmalıdır
- Konu adları ve tüm sertifikaların konu alternatif adlarını dağıtımları başarısız önlemek için bu makalede açıklanan belirtimleri eşleştiğinden emin olun.

> [!NOTE]
> Bir sertifika güven zinciri Is ara sertifika yetkililerini varlığını desteklenir. 

## <a name="mandatory-certificates"></a>Zorunlu sertifikaları
Her iki Azure AD için gerekli olan Azure yığın ortak uç nokta PKI sertifikaları bu bölümdeki tabloda açıklanmaktadır ve AD FS Azure yığın dağıtımları. Sertifika gereksinimleri alanı yanı sıra tarafından kullanılan ad alanları gruplandırılır ve sertifikaları, her ad alanı için gereklidir. Aşağıdaki tabloda çözüm sağlayıcınızda ortak uç nokta başına farklı sertifikaları kopyalar klasörü de açıklanmıştır. 

Her Azure yığın ortak altyapısı uç noktası için uygun DNS adları olan sertifikaları gereklidir. Her uç noktanın DNS adı biçiminde ifade edilir:  *&lt;öneki >.&lt; bölge >. &lt;fqdn >*. 

Dağıtımınız, [Bölge] ve [externalfqdn] değerleri bölge ve Azure yığın sisteminiz için seçtiğiniz dış etki alanı adları eşleşmelidir. Bölge adı ise bir örnek olarak *Redmond* ve dış etki alanı adı *contoso.com*, DNS adlarını biçimi olurdu  *&lt;öneki >. redmond.contoso.com* .  *&lt;Öneki >* değerleri sertifika tarafından güvenliği sağlanan uç nokta açıklamak için Microsoft tarafından predesignated. Ayrıca,  *&lt;öneki >* dış altyapı uç noktaları değerler belirli uç noktası kullanan Azure yığın hizmet bağlıdır. 

|Dağıtım klasörü|Gerekli sertifika konusu ve konu alternatif adları (SAN)|Kapsam (her bölge)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|Ortak portalı|Portal.  *&lt;bölge >.&lt; FQDN >*|Portallar|*&lt;region>.&lt;fqdn>*|
|Yönetim Portalı|Adminportal.  *&lt;bölge >.&lt; FQDN >*|Portallar|*&lt;region>.&lt;fqdn>*|
|Azure Resource Manager genel|yönetimi.  *&lt;bölge >.&lt; FQDN >*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|Azure Resource Manager Admin|adminmanagement.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|ACS<sup>1</sup>|Konu alternatif adlarını içeren bir çoklu alt etki alanı joker sertifikası:<br>&#42;.blob.*&lt;region>.&lt;fqdn>*<br>&#42;. sıra.  *&lt;bölge >.&lt; FQDN >*<br>&#42;. Tablo.  *&lt;bölge >.&lt; FQDN >*|Depolama|blob.*&lt;region>.&lt;fqdn>*<br>Tablo.  *&lt;bölge >.&lt; FQDN >*<br>sıra.  *&lt;bölge >.&lt; FQDN >*|
|KeyVault|&#42;. Kasa.  *&lt;bölge >.&lt; FQDN >*<br>(Joker SSL sertifikası)|Anahtar Kasası|Kasa.  *&lt;bölge >.&lt; FQDN >*|
|KeyVaultInternal|&#42;.adminvault.*&lt;region>.&lt;fqdn>*<br>(Joker SSL sertifikası)|İç Keyvault|adminvault.  *&lt;bölge >.&lt; FQDN >*|
|
<sup>1</sup> ACS sertifikası üç joker SAN'ları üzerinde tek bir sertifika gerektirir. Tek bir sertifika üzerinde birden fazla joker karakter SANs tüm ortak sertifika yetkilisi tarafından desteklenmiyor olabilir. 

Azure yığını Azure AD dağıtım modunu kullanarak dağıtırsanız, yalnızca önceki tabloda listelenen sertifikaları istemeniz gerekir. Ancak, Azure AD FS dağıtım modunu kullanarak yığın dağıtırsanız, aşağıdaki tabloda açıklanan sertifikaları da isteğinde gerekir:

|Dağıtım klasörü|Gerekli sertifika konusu ve konu alternatif adları (SAN)|Kapsam (her bölge)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|ADFS|ADFS.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası)|ADFS|*&lt;region>.&lt;fqdn>*|
|Graf|Grafik.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası)|Graf|*&lt;region>.&lt;fqdn>*|
|

> [!IMPORTANT]
> Bu bölümde listelenen tüm sertifikalar aynı parolaya sahip olmalıdır. 

## <a name="optional-paas-certificates"></a>İsteğe bağlı PaaS sertifikaları
Planlama Azure yığın sonra ek Azure yığın PaaS hizmetler (SQL, MySQL ve uygulama hizmeti) dağıtmak için dağıtılan ve yapılandırıldı, PaaS Hizmetleri uç noktalarına karşılamak için ek sertifika istemeniz gerekir. 

> [!IMPORTANT]
> Uygulama hizmeti, SQL ve MySQL kaynak sağlayıcıları için kullandığınız sertifikaları aynı kök yetkilisi olarak genel Azure yığın uç noktaları için kullanılanlarla olması gerekir. 

Aşağıdaki tabloda, SQL ve MySQL bağdaştırıcıları ve uygulama hizmeti için gereken sertifikaları ve uç noktaları açıklar. Bu sertifikalar Azure yığın dağıtım klasörüne kopyalamak gerekmez. Bunun yerine, ek kaynak sağlayıcıları yüklediğinizde, bu sertifikalar sağlar. 

|Kapsam (her bölge)|Sertifika|Gerekli sertifika konusu ve konu alternatif adları (SAN)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|SQL, MySQL|SQL ve MySQL|&#42;.dbadapter.*&lt;region>.&lt;fqdn>*<br>(Joker SSL sertifikası)|dbadapter.*&lt;region>.&lt;fqdn>*|
|App Service|Web trafiği varsayılan SSL sertifikası|&#42;.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*<br>(Birden çok etki alanı joker SSL sertifikası<sup>1</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|API|api.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL sertifikası<sup>2</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|FTP|ftp.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL sertifikası<sup>2</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|SSO|SSO.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|

<sup>1</sup> birden fazla joker ilgili alternatif adlarına sahip bir sertifika gerektirir. Tek bir sertifika üzerinde birden fazla joker karakter SANs tüm ortak sertifika yetkilisi tarafından desteklenmiyor olabilir 

<sup>2</sup> A &#42;.appservice.*&lt;region>.&lt;fqdn>* wild card certificate cannot be used in place of these three certificates (api.appservice.*&lt;region>.&lt;fqdn>*, ftp.appservice.*&lt;region>.&lt;fqdn>*, and sso.appservice.*&lt;region>.&lt;fqdn>*. Uygulama hizmeti açıkça Bu uç noktalar için ayrı sertifikaların kullanımını gerektirir. 

## <a name="learn-more"></a>Daha fazla bilgi edinin
Bilgi edinmek için nasıl [Azure yığın dağıtımı için PKI sertifikalarını oluşturmak](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Sonraki adımlar
[Kimlik Tümleştirme](azure-stack-integrate-identity.md)

