---
title: 'Azure AD etki alanı Hizmetleri: Karşılaştırma Azure AD etki alanı Hizmetleri Dıy etki alanı denetleyicilerine | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri Dıy etki alanı denetleyicilerine karşılaştırma
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: maheshu
ms.openlocfilehash: 172477af5d5ae19cd0362cb1de0a8c66332cb0bd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34587843"
---
# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Azure AD etki alanı Hizmetleri karar verme, kullanım durumu için doğru
Azure AD etki alanı Hizmetleri ile iş yüklerinizi Azure altyapı Hizmetleri'nde, azure'da kimlik altyapısını sürdürme hakkında endişelenmeye gerek kalmadan dağıtabilirsiniz. Bu yönetilen hizmet dağıtmak ve yönetmek, kendi tipik bir Windows Server Active Directory Dağıtım farklıdır. Hizmetin dağıtılması kolaydır ve otomatik sistem durumu izleme ve düzeltme sunar. Biz ortak dağıtım senaryoları için destek eklenecek hizmetin sürekli olarak artmaktadır.

Azure AD etki alanı Hizmetleri kullanmaya karar vermek için aşağıdaki okuma malzemelerini öneririz:

* Listesine bakın [özelliklerini Azure AD etki alanı Hizmetleri tarafından sunulan](active-directory-ds-features.md).
* Gözden geçirme ortak [Azure AD etki alanı Hizmetleri için dağıtım senaryoları](active-directory-ds-scenarios.md).
* Son olarak, [yazılanları AD seçeneği Azure AD Etki Alanı Hizmetleri'ne karşılaştırmak](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure AD etki alanı Hizmetleri Azure Dıy AD etki alanına Karşılaştır
Aşağıdaki tabloda, Azure AD Etki Alanı Hizmetleri'ni kullanarak kendi Azure AD altyapısında yönetme arasında bir karar vermenize yardımcı olur.

| **Özellik** | **Azure AD etki alanı Hizmetleri** | **Azure VM'ler 'Yazılanları' AD** |
| --- |:---:|:---:|
| [**Yönetilen hizmet**](active-directory-ds-comparison.md#managed-service) |**&#x2713;** |**&#x2715;** |
| [**Güvenli dağıtımlar**](active-directory-ds-comparison.md#secure-deployments) |**&#x2713;** |Yönetici, dağıtımınızın güvenliğini gerekiyor. |
| [**DNS sunucusu**](active-directory-ds-comparison.md#dns-server) |**&#x2713;**(yönetilen hizmeti) |**&#x2713;** |
| [**Etki alanı veya kuruluş yönetici ayrıcalıkları**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**&#x2715;** |**&#x2713;** |
| [**Etki alanına katılma**](active-directory-ds-comparison.md#domain-join) |**&#x2713;** |**&#x2713;** |
| [**NTLM ve Kerberos kullanarak etki alanı kimlik doğrulaması**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&#x2713;** |**&#x2713;** |
| [**Kısıtlı Kerberos temsilcisi seçme**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|Kaynak tabanlı|Kaynak tabanlı & hesabı tabanlı|
| [**Özel OU yapısı**](active-directory-ds-comparison.md#custom-ou-structure) |**&#x2713;** |**&#x2713;** |
| [**Şema uzantıları**](active-directory-ds-comparison.md#schema-extensions) |**&#x2715;** |**&#x2713;** |
| [**AD etki alanı/orman güvenleri**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**&#x2715;** |**&#x2713;** |
| [**LDAP okuma**](active-directory-ds-comparison.md#ldap-read) |**&#x2713;** |**&#x2713;** |
| [**Güvenli LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&#x2713;** |**&#x2713;** |
| [**LDAP yazma**](active-directory-ds-comparison.md#ldap-write) |**&#x2715;** |**&#x2713;** |
| [**Grup İlkesi**](active-directory-ds-comparison.md#group-policy) |**&#x2713;** |**&#x2713;** |
| [**Coğrafi olarak dağıtılan dağıtımları**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**&#x2715;** |**&#x2713;** |

#### <a name="managed-service"></a>Yönetilen hizmet
Azure AD etki alanı Hizmetleri etki alanı, Microsoft tarafından yönetilir. Düzeltme eki uygulama, güncelleştirmelerinin, yedeklemeler, izleme ve etki alanınızı kullanılabilir olmasını sağlamaya hakkında endişelenmeniz gerekmez. Bu yönetim görevleri, Microsoft Azure tarafından yönetilen etki alanları için bir hizmet olarak sunulur.

#### <a name="secure-deployments"></a>Güvenli dağıtımlar
Yönetilen etki alanı güvenli bir şekilde Microsoft'un güvenlik önerilerini AD dağıtımlar için uygun şekilde kilitlenmiştir. Bu öneriler, mühendislik ve AD dağıtımları destekleyen deneyimi AD ürün ekibinin on yılları gövdesi. Yazılanları dağıtımları için dağıtımınızı aşağı ve güvenli kilitlemek için belirli dağıtım adımları gerçekleştirmeniz gerekir.

#### <a name="dns-server"></a>DNS sunucusu
Azure AD etki alanı Hizmetleri yönetilen etki alanı, yönetilen DNS hizmetleri içerir. 'AAD DC Yöneticiler' grubunun üyeleri yönetilen etki alanı DNS yönetebilirsiniz. Bu grubun üyeleri, yönetilen etki alanı için tam DNS yönetim ayrıcalıkları verilir. DNS Yönetim 'uzak sunucu Yönetim Araçları (RSAT) paketindeki DNS Yönetim Konsolu' kullanılarak gerçekleştirilebilir.
[Daha fazla bilgi](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Etki alanı veya kuruluş yöneticisi ayrıcalıkları
Bir AAD DS yönetilen etki alanında bu yükseltilmiş ayrıcalıklar önerilmez. Bu yükseltilmiş ayrıcalıklar AAD DS karşı dağıtılamıyor gerektiren bir uygulama etki alanları yönetilen. Yönetim ayrıcalıklarının daha küçük bir alt 'AAD DC Yöneticiler' olarak adlandırılan temsilci ile yönetim grubunun üyeleri için kullanılabilir. Bu ayrıcalıklar DNS yapılandırmak için Grup İlkesi yapılandırmak, yönetici ayrıcalıkları makinelerde etki alanına katılmış vb. elde ayrıcalıkları içerir.

#### <a name="domain-join"></a>Etki alanına katılma
Sanal makine nasıl, bilgisayarları bir AD etki alanına katmak için benzer yönetilen etki alanına katılamaz.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>NTLM ve Kerberos kullanarak etki alanı kimlik doğrulaması
Azure AD etki alanı Hizmetleri ile yönetilen etki alanı ile kimlik doğrulaması yapmak için şirket kimlik bilgilerinizi kullanabilirsiniz. Kimlik bilgileri Azure AD kiracınıza ile eşitlenmiş tutulur. Eşitlenmiş kiracılar için Azure AD Connect içi yapılan kimlik değişiklikleri Azure AD ile eşitlenir sağlar. Yazılanları etki alanı kurulumu ile bir AD etki alanını ayarlama gerekebilir, şirket içi güven kullanıcıların Kurumsal kimlik ile kimlik doğrulaması AD. Alternatif olarak, kullanıcı parolalarını Azure etki alanı denetleyicisi sanal makinelerinizi eşitlediğinizden emin olmak için AD çoğaltma ayarlamanız gerekebilir.

#### <a name="kerberos-constrained-delegation"></a>Kısıtlı Kerberos temsilcisi seçme
Bir Active Directory etki alanı Hizmetleri yönetilen etki alanında ' etki alanı Yönetici ' ayrıcalıklarına sahip değil. Bu nedenle, hesap tabanlı (Geleneksel) kısıtlı Kerberos temsilcisi yapılandıramazsınız. Ancak, daha güvenli kaynak tabanlı Kısıtlı temsilci yapılandırabilirsiniz.
[Daha fazla bilgi](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Özel OU yapısı
'AAD DC Yöneticiler' grubunun üyeleri yönetilen etki alanı içinde özel OU'lar oluşturabilirsiniz. Özel OU'lar Oluştur kullanıcılar OU üzerinde tam yönetim ayrıcalıklarına sahiptir.
[Daha fazla bilgi](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Şema uzantıları
Azure AD etki alanı Hizmetleri yönetilen etki alanı temel şeması genişletemezsiniz. Bu nedenle, uzantıları AD şemasına (örneğin, yeni öznitelikler kullanıcı nesnesi altında) kullanan uygulamalar olamaz kaldırılmış ve AAD DS etki alanlarına gölgeye.

#### <a name="ad-domain-or-forest-trusts"></a>AD etki alanı veya orman güvenleri
Yönetilen etki alanı güven ilişkileri (gelen/giden) diğer etki alanlarıyla ayarlamak için yapılandırılamaz. Bu nedenle, kaynak ormanı dağıtım senaryoları Azure AD Etki Alanı Hizmetleri'ni kullanamazsınız. Benzer şekilde, burada Azure ad parolalarını eşitlemek için tercih ettiğiniz dağıtımları Azure AD Etki Alanı Hizmetleri'ni kullanamazsınız.

#### <a name="ldap-read"></a>LDAP okuma
Yönetilen etki alanı iş yükleri okuma LDAP destekler. Bu nedenle, yönetilen etki alanına göre LDAP okuma işlemleri uygulamaları dağıtabilirsiniz.

#### <a name="secure-ldap"></a>Güvenli LDAP
Internet üzerinden de dahil olmak üzere, yönetilen etki alanınız güvenli LDAP erişim sağlamak için Azure AD etki alanı Hizmetleri yapılandırabilirsiniz.
[Daha fazla bilgi](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>LDAP yazma
Yönetilen etki alanı kullanıcı nesneleri için salt okunurdur. Bu nedenle, kullanıcı nesnesinin öznitelikleri karşı LDAP yazma işlemlerini gerçekleştiren uygulamalar yönetilen bir etki alanında çalışmaz. Ayrıca, kullanıcı parolalarını yönetilen etki alanı içinde değiştirilemez. Başka bir örnek grup üyeliklerini veya izin verilmez yönetilen etki alanında Grup öznitelikleri değiştirilmesine olabilir. Ancak, değişiklikleri kullanıcı öznitelikleri veya Azure AD (PowerShell/Azure portalı) yoluyla yapılan parolaları veya şirket içi AD DS AAD yönetilen etki alanı eşitlenir.

#### <a name="group-policy"></a>Grup ilkesi
Bir yerleşik GPO her "AADDC bilgisayarlar" ve "AADDC kullanıcıları" kapsayıcıları yoktur. Grup İlkesi yapılandırmak için bu yerleşik GPO'ları özelleştirebilirsiniz. 'AAD DC Yöneticiler' grubunun üyeleri, ayrıca özel GPO'ları oluşturmak ve bunları (özel OU'lar dahil) varolan OU'lara bağlayın.
[Daha fazla bilgi](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Coğrafi olarak dağınık dağıtımları
Azure AD etki alanı Hizmetleri yönetilen etki alanları, azure'da tek bir sanal ağda kullanılabilir. Etki alanı denetleyicileri dünya genelindeki birden çok Azure bölgelerinde kullanılabilir olmasını gerektiren senaryolar için Azure Iaas Vm'leri etki alanı denetleyicileri ayarlama daha iyi bir alternatif olabilir.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Yazılanları' (DIY) AD dağıtım seçenekleri
Windows Server AD yüklemesi tarafından sunulan özelliklerden bazıları ihtiyaç duyacağınız dağıtım kullanım örnekleri olabilir. Bu durumlarda, aşağıdaki yazılanları (DIY) seçeneklerden birini dikkate alın:

* **Tek başına bulut etki alanı:** tek başına ' etki alanı denetleyicileri olarak yapılandırılmış Azure sanal makineleri kullanarak bulut etki alanı' ayarlayabilirsiniz. Bu altyapı şirket içi ile tümleştirilmezse AD ortamı. Bu seçenek 'bulut kimlik bilgileri' farklı kümesi gerektiren oturum açma/VMs bulutta yönetmek için.
* **Kaynak orman dağıtımı:** kaynak orman topoloji, bir etki alanına etki alanı denetleyicileri olarak yapılandırılmış Azure sanal makineleri kullanarak ayarlayabilirsiniz. Ardından, şirket içi ile AD güven ilişkisi yapılandırabilirsiniz AD ortamı. Bu kaynak ormanına bulutta etki alanına katılma bilgisayarları (Azure VM) kullanabilirsiniz. Kullanıcı kimlik doğrulaması ya da şirket içi dizininize VPN/ExpressRoute bağlantı olur.
* **Şirket içi etki alanınız için Azure genişletmek:** bir Azure sanal ağı şirket içi ağınıza bir VPN/ExpressRoute bağlantısı kullanarak bağlanabilir. Bu ayar şirket içi için birleştirilecek Azure VM'ler sağlar AD. Başka bir Azure VM olarak şirket içi etki alanınızda kopya etki alanı denetleyicilerini yükseltmek için alternatiftir. Ardından, şirket içi dizininize VPN/ExpressRoute bağlantısı üzerinden çoğaltmak için ayarlayabilirsiniz. Bu dağıtım modu, şirket içi etki alanınız için Azure etkili bir şekilde genişletir.

> [!NOTE]
> Dıy seçeneği dağıtım kullanım örnekleri için daha uygun olan belirleyebilir. Göz önünde bulundurun [geri bildirim paylaşımı](active-directory-ds-contact-us.md) özellikleri yardımcı olacak anlamanıza yardımcı olmak için Azure AD etki alanı Hizmetleri gelecekte seçtiniz. Bu geri bildirim hizmeti kullanım örnekleri ve dağıtım gereksinimlerini daha iyi uyacak şekilde gelişmesi yardımcı olur.
>
>

Biz yayımlanan [dağıtma Windows Server Active Directory için Azure sanal makineler üzerinde yönergeleri](https://msdn.microsoft.com/library/azure/jj156090.aspx) Dıy yüklemeleri kolaylaştırılmasına yardımcı olmak için.

## <a name="related-content"></a>İlgili İçerik
* [Özellikler - Azure AD etki alanı Hizmetleri](active-directory-ds-features.md)
* [Dağıtım senaryoları - Azure AD etki alanı Hizmetleri](active-directory-ds-scenarios.md)
* [Windows Server Active Directory, Azure sanal makinelerde dağıtmak için yönergeler](https://msdn.microsoft.com/library/azure/jj156090.aspx)
