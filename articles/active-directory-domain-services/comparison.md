---
title: Azure AD etki alanı Hizmetleri karar verme, kullanım örneği için doğru
description: Azure Active Directory Domain Services kendin YAP etki alanı denetleyicilerine karşılaştırma
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: 17bb8eb910990e0e9da65491f8dd7361398479a5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246472"
---
# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Azure AD etki alanı Hizmetleri karar verme, kullanım örneği için doğru

Azure AD Domain Services ile kimlik altyapısını azure'da koruma hakkında endişelenmenize gerek kalmadan Azure altyapı hizmetleri iş yüklerinizi dağıtabilirsiniz. Yönetilen bu hizmet, dağıtmak ve yönetmek, kendi tipik bir Windows Server Active Directory Dağıtım farklıdır. Hizmet kolayca dağıtılır ve otomatik sistem durumu izleme ve düzeltme sağlar. Biz, dağıtım senaryoları için destek eklemek için hizmet sürekli geliştirilmektedir.

Azure AD Domain Services'ı kullanmak karar vermek için aşağıdaki okuma malzemelerini öneririz:

* Listesine bakın [özellikleri, Azure AD Domain Services tarafından sunulan](active-directory-ds-features.md).
* Gözden geçirme ortak [dağıtım senaryoları için Azure AD Domain Services](scenarios.md).
* Son olarak, [yazılanları bir AD seçeneği Azure AD Domain Services arasındaki farklar](comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure'da kendin YAP AD etki alanına Azure AD Domain Services karşılaştırması

Aşağıdaki tabloda, Azure AD Domain Services'ı kullanarak azure'da kendi AD altyapınızı yönetme arasında bir karar vermenize yardımcı olur.

| **Özelliği** | **Azure AD etki alanı Hizmetleri** | **Azure vm'lerde 'Kendin Yap ' türü AD** |
| --- |:---:|:---:|
| [**Yönetilen hizmet**](comparison.md#managed-service) |**&#x2713;** |**&#x2715;** |
| [**Güvenli dağıtımlar**](comparison.md#secure-deployments) |**&#x2713;** |Güvenli dağıtım yöneticisinin gerekir. |
| [**DNS sunucusu**](comparison.md#dns-server) |**&#x2713;** (yönetilen hizmet) |**&#x2713;** |
| [**Etki alanı veya kuruluş yöneticisi ayrıcalıkları**](comparison.md#domain-or-enterprise-administrator-privileges) |**&#x2715;** |**&#x2713;** |
| [**Etki alanına katılma**](comparison.md#domain-join) |**&#x2713;** |**&#x2713;** |
| [**NTLM ve Kerberos kullanarak etki alanı kimlik doğrulaması**](comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&#x2713;** |**&#x2713;** |
| [**Kısıtlı Kerberos temsilcisi seçme**](comparison.md#kerberos-constrained-delegation)|Kaynak tabanlı|Kaynak tabanlı & hesabı tabanlı|
| [**Özel OU yapısı**](comparison.md#custom-ou-structure) |**&#x2713;** |**&#x2713;** |
| [**Şema uzantıları**](comparison.md#schema-extensions) |**&#x2715;** |**&#x2713;** |
| [**AD etki alanı/orman güvenleri**](comparison.md#ad-domain-or-forest-trusts) |**&#x2715;** |**&#x2713;** |
| [**LDAP okuyun**](comparison.md#ldap-read) |**&#x2713;** |**&#x2713;** |
| [**Güvenli LDAP (LDAPS)** ](comparison.md#secure-ldap) |**&#x2713;** |**&#x2713;** |
| [**LDAP yazma**](comparison.md#ldap-write) |**&#x2715;** |**&#x2713;** |
| [**Grup İlkesi**](comparison.md#group-policy) |**&#x2713;** |**&#x2713;** |
| [**Coğrafi olarak dağıtılmış dağıtımlarla**](comparison.md#geo-dispersed-deployments) |**&#x2715;** |**&#x2713;** |

#### <a name="managed-service"></a>Yönetilen hizmet

Azure AD Domain Services etki alanı, Microsoft tarafından yönetilir. Düzeltme eki uygulama, güncelleştirmeleri, izleme, yedeklemeleri ve etki alanınızı kullanılabilirliğini sağlama hakkında endişelenmeniz gerekmez. Bu yönetim görevleri bir hizmet olarak, yönetilen etki alanları için Microsoft Azure tarafından sunulur.

#### <a name="secure-deployments"></a>Güvenli dağıtımlar

Yönetilen etki alanı güvenli bir şekilde aşağı AD dağıtımlar için Microsoft Güvenlik önerileri göre kilitli. Bu öneriler, AD ürün ekibinin yıllardır mühendislik ve AD dağıtımlarını destekleyen kökü. Yazılanları dağıtımları için aşağı ve güvenli kilitlemek için belirli bir dağıtım, dağıtım adımlar gerekir.

#### <a name="dns-server"></a>DNS sunucusu

Azure AD Domain Services yönetilen etki alanı, yönetilen DNS hizmetleri içerir. 'AAD DC Administrators' grubunun üyeleri, yönetilen etki alanındaki DNS yönetebilirsiniz. Bu grubun üyeleri, yönetilen etki alanı için tam DNS yönetim ayrıcalıkları verilir. DNS Yönetimi 'DNS Yönetim Konsolu Uzak Sunucu Yönetim Araçları (RSAT) paketinde' kullanılarak gerçekleştirilebilir.
[Daha fazla bilgi](manage-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Etki alanı veya kuruluş yöneticisi ayrıcalıkları

Bu yükseltilmiş ayrıcalıkları, bir AAD DS yönetilen etki alanında sunulmaz. AAD-DS karşı bu yükseltilmiş ayrıcalıklar dağıtılamıyor gerektiren uygulamalar, etki alanları yönetilen. Yönetim ayrıcalıkları küçük bir alt kümesi, 'AAD DC Administrators' olarak adlandırılan temsilci ile yönetim grubunun üyeleri için kullanılabilir. Bu ayrıcalıkların DNS yapılandırmak için Grup İlkesi yapılandırmak, etki alanına katılmış makinelerde vb. yönetici ayrıcalıkları elde ayrıcalıklarını içerir.

#### <a name="domain-join"></a>Etki alanına katılma

Sanal makineleri nasıl, bilgisayarları bir AD etki alanına katmak için benzer yönetilen etki alanına katılmasını sağlayabilirsiniz.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>NTLM ve Kerberos kullanarak etki alanı kimlik doğrulaması

Azure AD Domain Services ile yönetilen etki alanı ile kimlik doğrulaması için şirket kimlik bilgilerinizi kullanabilirsiniz. Kimlik bilgileri Azure AD kiracınıza eşitlenmiş olarak tutulur. Eşitlenmiş kiracılar için Azure AD Connect, şirket kimlik bilgilerini yapılan değişiklikler Azure AD'ye eşitlenen sağlar. Yazılanları etki alanı kurulum ile bir AD etki alanını ayarlama gerekebilir güven ile şirket içi kullanıcıların Kurumsal kimlik bilgileriyle kimlik doğrulaması AD. Alternatif olarak, kullanıcı parolalarını Azure etki alanı denetleyicisi sanal makinelerinizi eşitlenmesini AD çoğaltma ayarlamanız gerekebilir.

#### <a name="kerberos-constrained-delegation"></a>Kısıtlı Kerberos temsilcisi seçme

Bir Active Directory Domain Services yönetilen etki alanında ' etki alanı Yöneticisi ' ayrıcalıklara sahip değilsiniz. Bu nedenle, hesap tabanlı (Geleneksel) kısıtlı Kerberos temsilcisi seçmeyi yapılandıramazsınız. Ancak, daha güvenli kaynak tabanlı Kısıtlı temsilci yapılandırabilirsiniz.
[Daha fazla bilgi](deploy-kcd.md)

#### <a name="custom-ou-structure"></a>Özel OU yapısı

'AAD DC Administrators' grubunun üyeleri, yönetilen etki alanı içinde özel OU'ları oluşturabilirsiniz. Özel Kuruluş birimlerini kullanıcılara OU üzerinde tam yönetimsel ayrıcalıklar verilir.
[Daha fazla bilgi](create-ou.md)

#### <a name="schema-extensions"></a>Şema uzantıları

Bir Azure AD Domain Services yönetilen etki alanının temel şemayı uzatamadığınızda. Bu nedenle, uzantıları AD şemasına (örneğin, yeni öznitelikler kullanıcı nesnesinin altında) kullanan uygulamalar olamaz yükseltilmiş ve AAD DS etki alanlarına kaydırılacağı uzaklık.

#### <a name="ad-domain-or-forest-trusts"></a>AD etki alanı veya orman güvenleri

Yönetilen etki alanı güven ilişkileri (gelen/giden) diğer etki alanlarıyla ayarlamak için yapılandırılamaz. Bu nedenle, kaynak orman dağıtım senaryoları, Azure AD Domain Services kullanamazsınız. Benzer şekilde, burada parolaları Azure AD'ye eşitlemek için tercih ettiğiniz dağıtımları, Azure AD Domain Services kullanamazsınız.

#### <a name="ldap-read"></a>LDAP okuma

Yönetilen etki alanında LDAP okuma iş yüklerini destekler. Bu nedenle, yönetilen etki alanında LDAP okuma işlemlerini gerçekleştiren uygulamalar dağıtabilirsiniz.

#### <a name="secure-ldap"></a>Güvenli LDAP

İnternet üzerinden de dahil olmak üzere, yönetilen etki alanı için güvenli LDAP erişimini sağlamak için Azure AD Domain Services yapılandırabilirsiniz.
[Daha fazla bilgi](configure-ldaps.md)

#### <a name="ldap-write"></a>LDAP yazma

Yönetilen etki alanı kullanıcı nesneleri için salt okunur. Bu nedenle, kullanıcı nesnesinin öznitelikleri karşı LDAP yazma işlemleri gerçekleştiren uygulamalar, yönetilen bir etki alanında çalışmaz. Ayrıca, kullanıcı parolalarını yönetilen etki alanı içinde değiştirilemez. Başka bir örnek grup üyelikleri veya izin verilmeyen yönetilen etki alanındaki grup öznitelikleri değiştirilmesini olacaktır. Kullanıcı öznitelikleri veya parolaları (PowerShell/Azure portalından) Azure AD'de yapılan değişiklikler ancak veya şirket içinde AD DS AAD yönetilen etki alanına eşitlenir.

#### <a name="group-policy"></a>Grup İlkesi

Bir yerleşik GPO'yu her "AADDC Computers" ve "AADDC Users" kapsayıcıları yoktur. Grup İlkesi yapılandırmak için bu yerleşik GPO özelleştirebilirsiniz. 'AAD DC Administrators' grubunun üyeleri ayrıca özel GPO'ları oluşturmak ve bunları (özel OU'ları dahil) mevcut OU'lara bağlayın.
[Daha fazla bilgi](manage-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Coğrafi olarak dağınık dağıtım

Azure AD Domain Services yönetilen etki alanlarını azure'da tek bir sanal ağda kullanılabilir. Etki alanı denetleyicileri dünya genelindeki birden çok Azure bölgesinde kullanılabilir olmasını gerektiren senaryolar için Azure Iaas sanal makinelerinde etki alanı denetleyicileri ayarlama, daha iyi alternatif olabilir.

## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Kendin Yap ' türü (DIY) AD dağıtım seçenekleri

Windows Server AD yüklemesi tarafından sunulan özelliklerden bazıları, gereken dağıtım kullanım örneklerinize olabilir. Bu gibi durumlarda yazılanları (DIY) aşağıdaki seçeneklerden birini göz önünde bulundurun:

* **Tek başına bulut etki alanı:** Tek başına bir 'bulut etki alanına etki alanı denetleyicisi olarak yapılandırılmış olan Azure sanal makineleri kullanarak' ayarlayabilirsiniz. Bu altyapı, şirket içi ile tümleştirilmezse AD ortam. Bu seçenek 'bulut kimlik bilgileri' farklı kümesi gerektiren sanal makineleri bulutta oturum açma/yönetmek için.
* **Kaynak orman dağıtımı:** Kaynak orman topoloji, bir etki alanına etki alanı denetleyicileri olarak yapılandırılmış Azure sanal makineleri kullanarak ayarlayabilirsiniz. Ardından, şirket içi ile bir AD güven ilişkisi yapılandırabilirsiniz AD ortam. Bu kaynak ormanına bulut etki alanına katılım bilgisayarlar (Azure Vm'leri) kullanabilirsiniz. Kullanıcı kimlik doğrulaması ya da şirket içi dizininize VPN/ExpressRoute bağlantı olur.
* **Şirket içi etki alanınızı Azure'a genişletmek:** Bir Azure sanal ağı şirket içi ağınıza bir VPN/ExpressRoute bağlantısı kullanarak bağlanabilir. Bu ayar, Azure Vm'leri, şirket içi için katılmasını sağlar AD. Çoğaltma VM'si olarak azure'da şirket içi etki alanınızın etki alanı denetleyicilerini yükseltmek başka bir alternatiftir. Ardından, şirket içi dizininize VPN/ExpressRoute bağlantısı üzerinden çoğaltmak için ayarlayabilirsiniz. Bu dağıtım modu, şirket içi etki alanınızı Azure'da etkili bir şekilde genişletir.

> [!NOTE]
> Kendin YAP seçeneği dağıtım kullanım örneklerinize için daha uygun olduğunu belirleyebilir. Göz önünde bulundurun [geri bildirim paylaşma](contact-us.md) özellikleri yardımcı olan anlamamıza yardımcı olmak için Azure AD Domain Services'ı gelecekte seçtiniz. Bu geri bildirim hizmetini kullanım örnekleri ve dağıtım gereksinimlerini daha iyi uyacak şekilde yardımcı olur.
>
>

Yayımladık [dağıtma Windows Server Active Directory için Azure sanal Makineler'de yönergeleri](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) kendin YAP yüklemeleri daha kolay hale getirilmesine yardımcı olacak.

## <a name="related-content"></a>İlgili İçerik

* [Özellikler - Azure AD etki alanı Hizmetleri](active-directory-ds-features.md)
* [Dağıtım senaryoları - Azure AD Domain Services](scenarios.md)
* [Azure sanal makineler üzerinde Windows Server Active Directory dağıtma ilkeleri](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100)
