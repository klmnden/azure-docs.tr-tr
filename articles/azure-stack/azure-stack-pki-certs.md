---
title: Azure yığın Azure yığın ortak anahtar altyapısı sertifika gereksinimleri tümleşik sistemleri | Microsoft Docs
description: Azure tümleşik yığını sistemler için Azure yığın PKI sertifikası dağıtım gereksinimleri açıklanır.
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
ms.date: 06/06/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: faf85c34c527dd72889f0fcb5021925b79481163
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823858"
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
- Azure yığın altyapınızı sertifikada yayımlanan sertifika yetkilisinin sertifika iptal listesi (CRL) konumuna ağ erişimi olması gerekir. Bu CRL http uç noktası olması gerekir
- Sertifikaları döndürme, sertifikalar ya da dağıtım veya herhangi bir ortak sertifika yetkilisi yukarıda verilen sertifikaları imzalamak için kullanılan aynı iç sertifika yetkilisi tarafından verilen olmalıdır
- Otomatik olarak imzalanan sertifikaların kullanımını desteklenmez
- Sertifika konu alternatif adı (SAN) alanındaki tüm ad alanlarını kapsayan tek bir joker sertifika olabilir. Alternatif olarak, uç noktaları için gibi joker karakterler kullanarak tek tek sertifikaları kullanabilirsiniz **acs** ve bulundukları yerde gerekli anahtar kasası. 
- Sertifika imza algoritması güçlü olmalıdır SHA1, olamaz. 
- Ortak ve özel anahtarlar Azure yığın yükleme için gerekli olan sertifika biçimi PFX, olması gerekir. 
- Sertifika pfx dosyaları bir değer "Dijital imza" ve "KeyEncipherment", "Anahtar kullanımı" alanında olması gerekir.
- Sertifika pfx dosyaları değerleri "Sunucu kimlik doğrulaması (1.3.6.1.5.5.7.3.1)" ve "İstemci kimlik doğrulaması (1.3.6.1.5.5.7.3.2)" "Gelişmiş anahtar kullanımı" alanında olması gerekir.
- Sertifikanın "verilen:" alan aynı olmamalıdır, "tarafından verilen:" alanı.
- Tüm sertifika pfx dosyalarını parolaların aynı dağıtım zamanında olmalıdır
- Sertifika pfx parolası karmaşık bir parola olması gerekir.
- Konu adları ve tüm sertifikaların konu alternatif adlarını dağıtımları başarısız önlemek için bu makalede açıklanan belirtimleri eşleştiğinden emin olun.

> [!NOTE]
> Kendi kendine imzalandı sertifikalar desteklenmez.

> [!NOTE]
> Bir sertifika güven zinciri Is ara sertifika yetkililerini varlığını desteklenir. 

## <a name="mandatory-certificates"></a>Zorunlu sertifikaları
Her iki Azure AD için gerekli olan Azure yığın ortak uç nokta PKI sertifikaları bu bölümdeki tabloda açıklanmaktadır ve AD FS Azure yığın dağıtımları. Sertifika gereksinimleri alanı yanı sıra tarafından kullanılan ad alanları gruplandırılır ve sertifikaları, her ad alanı için gereklidir. Aşağıdaki tabloda çözüm sağlayıcınızda ortak uç nokta başına farklı sertifikaları kopyalar klasörü de açıklanmıştır. 

Her Azure yığın ortak altyapısı uç noktası için uygun DNS adları olan sertifikaları gereklidir. Her uç noktanın DNS adı biçiminde ifade edilir:  *&lt;öneki >.&lt; bölge >. &lt;fqdn >*. 

Dağıtımınız, [Bölge] ve [externalfqdn] değerleri bölge ve Azure yığın sisteminiz için seçtiğiniz dış etki alanı adları eşleşmelidir. Bölge adı ise bir örnek olarak *Redmond* ve dış etki alanı adı *contoso.com*, DNS adlarını biçimi olurdu *&lt;öneki >. redmond.contoso.com*. *&lt;Öneki >* değerleri sertifika tarafından güvenliği sağlanan uç nokta açıklamak için Microsoft tarafından predesignated. Ayrıca,  *&lt;öneki >* dış altyapı uç noktaları değerler belirli uç noktası kullanan Azure yığın hizmet bağlıdır. 

> [!note]  
> Tüm ad alanlarını tüm dizinlere kopyalanan konusu ve konu alternatif adı (SAN) alanları kapsayan bir tek joker sertifikası olarak veya tek sertifikaların her uç nokta karşılık gelen dizine kopyaladı sağlanan sertifika olabilir. Unutmayın, her iki seçenek için uç noktaları gibi joker karakterli sertifikalar kullanmanızı gerektirir **acs** ve bulundukları yerde gerekli anahtar kasası. 

| Dağıtım klasörü | Gerekli sertifika konusu ve konu alternatif adları (SAN) | Kapsam (her bölge) | Alt etki alanı ad alanı |
|-------------------------------|------------------------------------------------------------------|----------------------------------|-----------------------------|
| Ortak portalı | Portal. &lt;bölge >. &lt;fqdn > | Portallar | &lt;region>.&lt;fqdn> |
| Yönetim Portalı | adminportal. &lt;bölge >. &lt;fqdn > | Portallar | &lt;region>.&lt;fqdn> |
| Azure Resource Manager genel | yönetimi. &lt;bölge >. &lt;fqdn > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| Azure Kaynak Yöneticisi'ni yönetici | adminmanagement. &lt;bölge >. &lt;fqdn > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| ACSBlob | *.blob.&lt;region>.&lt;fqdn><br>(Joker SSL sertifikası) | Blob Depolama | BLOB. &lt;bölge >. &lt;fqdn > |
| ACSTable | * .table. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) | Tablo Depolama | Tablo. &lt;bölge >. &lt;fqdn > |
| ACSQueue | * .queue. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) | Kuyruk Depolama | sıra. &lt;bölge >. &lt;fqdn > |
| KeyVault | * .vault. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) | Key Vault | Kasa. &lt;bölge >. &lt;fqdn > |
| KeyVaultInternal | *.adminvault. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) |  İç Keyvault |  adminvault. &lt;bölge >. &lt;fqdn > |

### <a name="for-azure-stack-environment-on-pre-1803-versions"></a>Öncesi 1803 sürümlerinde Azure yığın ortamı için

|Dağıtım klasörü|Gerekli sertifika konusu ve konu alternatif adları (SAN)|Kapsam (her bölge)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|Ortak portalı|Portal.  *&lt;bölge >.&lt; FQDN >*|Portallar|*&lt;bölge >. &lt;fqdn >*|
|Yönetim Portalı|adminportal.  *&lt;bölge >.&lt; FQDN >*|Portallar|*&lt;bölge >. &lt;fqdn >*|
|Azure Resource Manager genel|yönetimi.  *&lt;bölge >.&lt; FQDN >*|Azure Resource Manager|*&lt;bölge >. &lt;fqdn >*|
|Azure Kaynak Yöneticisi'ni yönetici|adminmanagement.  *&lt;bölge >.&lt; FQDN >*|Azure Resource Manager|*&lt;bölge >. &lt;fqdn >*|
|ACS<sup>1</sup>|Konu alternatif adlarını içeren bir çoklu alt etki alanı joker sertifikası:<br>&#42;.BLOB.  *&lt;bölge >.&lt; FQDN >*<br>&#42;.Queue.  *&lt;bölge >.&lt; FQDN >*<br>&#42;.Table.  *&lt;bölge >.&lt; FQDN >*|Depolama|BLOB.  *&lt;bölge >.&lt; FQDN >*<br>Tablo.  *&lt;bölge >.&lt; FQDN >*<br>sıra.  *&lt;bölge >.&lt; FQDN >*|
|KeyVault|&#42;.Vault.  *&lt;bölge >.&lt; FQDN >*<br>(Joker SSL sertifikası)|Key Vault|Kasa.  *&lt;bölge >.&lt; FQDN >*|
|KeyVaultInternal|&#42;.adminvault.  *&lt;bölge >.&lt; FQDN >*<br>(Joker SSL sertifikası)|İç Keyvault|adminvault.  *&lt;bölge >.&lt; FQDN >*|
|
<sup>1</sup> ACS sertifikası üç joker SAN'ları üzerinde tek bir sertifika gerektirir. Tek bir sertifika üzerinde birden fazla joker karakter SANs tüm ortak sertifika yetkilisi tarafından desteklenmiyor olabilir. 

Azure yığını Azure AD dağıtım modunu kullanarak dağıtırsanız, yalnızca önceki tabloda listelenen sertifikaları istemeniz gerekir. Ancak, Azure AD FS dağıtım modunu kullanarak yığın dağıtırsanız, aşağıdaki tabloda açıklanan sertifikaları da isteğinde gerekir:

|Dağıtım klasörü|Gerekli sertifika konusu ve konu alternatif adları (SAN)|Kapsam (her bölge)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|ADFS|ADFS.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası)|ADFS|*&lt;bölge >. &lt;fqdn >*|
|Graf|Grafik.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası)|Graf|*&lt;bölge >. &lt;fqdn >*|
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
|SQL, MySQL|SQL ve MySQL|&#42;.dbadapter.  *&lt;bölge >.&lt; FQDN >*<br>(Joker SSL sertifikası)|dbadapter.*&lt;region>.&lt;fqdn>*|
|App Service|Web trafiği varsayılan SSL sertifikası|&#42;.appservice.  *&lt;bölge >.&lt; FQDN >*<br>&#42;. scm.appservice.  *&lt;bölge >.&lt; FQDN >*<br>&#42;. sso.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(Birden çok etki alanı joker SSL sertifikası<sup>1</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|
|App Service|API|api.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|
|App Service|FTP|FTP.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|
|App Service|SSO|SSO.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|uygulama hizmeti.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|

<sup>1</sup> birden fazla joker ilgili alternatif adlarına sahip bir sertifika gerektirir. Tek bir sertifika üzerinde birden fazla joker karakter SANs tüm ortak sertifika yetkilisi tarafından desteklenmiyor olabilir 

<sup>2</sup> A &#42;.appservice. *&lt;bölge >. &lt;fqdn >* joker sertifika yerine bu üç sertifikalar kullanılamıyor (api.appservice. *&lt;bölge >. &lt;fqdn >*, ftp.appservice. *&lt;bölge >. &lt;fqdn >* ve sso.appservice. *&lt;bölge >. &lt;fqdn >*. Uygulama hizmeti açıkça Bu uç noktalar için ayrı sertifikaların kullanımını gerektirir. 

## <a name="learn-more"></a>Daha fazla bilgi edinin
Bilgi edinmek için nasıl [Azure yığın dağıtımı için PKI sertifikalarını oluşturmak](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Sonraki adımlar
[Kimlik Tümleştirme](azure-stack-integrate-identity.md)

