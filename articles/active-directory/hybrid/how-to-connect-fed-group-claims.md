---
title: Azure Active Directory ile uygulamaları için Grup talepleri yapılandırma | Microsoft Docs
description: Azure AD ile kullanım için Grup talepleri yapılandırma konusunda bilgi.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.component: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: 19a8400a076825f17501fabdb3f38ea05915822e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138757"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Azure Active Directory (genel Önizleme) ile uygulamalar için Grup talepleri yapılandırma

Azure Active Directory, uygulamalarında kullanılmak belirteçlerinde bir kullanıcı grup üyeliği bilgileri sağlayabilir.  İki ana düzenleri desteklenir:

- Kendi Azure Active Directory nesne tanımlayıcısını (OID) özniteliği tarafından (genel kullanıma sunuldu) tanımlanan grupları
- SAMAccountName veya GrupSID öznitelikler için Active Directory (eşitlenmiş AD) gruplarını ve kullanıcıları (genel Önizleme) ile tanımlanan grupları

> [!IMPORTANT]
> Bu önizleme özelliği için Not Uyarılar vardır:
>
>- SAMAccountName ve güvenlik tanımlayıcısı (SID) özniteliklerin şirket içi ad'nizden eşitlenmiş kullanımı için destek, mevcut uygulamaları AD FS ve diğer kimlik sağlayıcılardan taşıma sağlamak için tasarlanmıştır. Bu talep yaymak gerekli öznitelikler Azure AD'de yönetilen grupları içermez.
>- Daha büyük kuruluşlarda, Azure Active Directory için bir belirteç ekler sınırını üyesi olduğu gruplara sayısını aşabilir. SAML belirteci yanı sıra, bir JWT için 200 için 150 grupları. Bu, öngörülemeyen sonuçlara neden olabilir. Bu olası bir sorun ise test etmenizi öneririz ve gerekirse, kadar bekleyen uygulama için ilgili gruplara talepleri sınırlamanıza izin vermek için geliştirmeler ekleriz.  
>- Yeni uygulama geliştirme veya onun için ve burada iç içe geçmiş Grup destek gerekli değildir uygulama yapılandırılabileceği durumlarda, uygulama içi yetkilendirme grupları yerine uygulama rolleri tabanlı öneririz.  Bu belirtece gitmesi gereken daha güvenlidir ve Uygulama Yapılandırması kullanıcı atamadan ayıran bilgi miktarını sınırlar.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-identity-providers"></a>AD FS ve diğer kimlik sağlayıcılardan geçiş uygulamalar için Grup talepleri

AD FS ile kimlik doğrulaması için yapılandırılan çoğu uygulama, grup üyeliği bilgileri Windows AD grup öznitelikleri biçiminde dayanır.   Bu öznitelikler tam-etki alanı adına göre veya Windows grup güvenlik tanımlayıcısını (GrupSID) grubu sAMAccountName bağlıdır.  AD FS TokenGroups işlevi uygulamanın AD FS ile Federasyon olduğunda, kullanıcının grup üyeliklerini almak için kullanır.

AD FS'den alacak bir uygulama belirteci eşleştirmek için Grup ve rol talep grubun Azure Active Directory nesne kimliği yerine tam etki alanı sAMAccountName içeren yayılan.

Grup talepleri için desteklenen biçimler:

- **Azure Active Directory grubu objectID** (tüm grupları için kullanılabilir)
- **SAMAccountName** (Active Directory'nizden eşitlenen gruplar için kullanılabilir)
- **NetbiosDomain\sAMAccountName** (Active Directory'nizden eşitlenen gruplar için kullanılabilir)
- **DNSDomainName\sAMAccountName** (Active Directory'nizden eşitlenen gruplar için kullanılabilir)
- **Şirket içi grup güvenlik tanımlayıcısını** (Active Directory'nizden eşitlenen gruplar için kullanılabilir)

> [!NOTE]
> sAMAccountName ve şirket içi Grup SID'si öznitelikleri yalnızca Active Directory'den eşitlenen grubu nesneler üzerinde kullanılabilir.   Bunlar, Azure Active Directory veya Office 365 oluşturulan gruplarında kullanılamaz.   Eşitlenmiş şirket içi grup öznitelikleri almak için Azure Active Directory içinde yapılandırılan uygulamalar yalnızca eşitlenmiş grupları alın.

## <a name="options-for-applications-to-consume-group-information"></a>Grup bilgilerini kullanmak uygulamaları için seçenekleri

Grup bilgilerini almak için uygulamalara bir yol için kimliği doğrulanmış kullanıcının grup üyeliğini almak için Graph grupları uç noktası çağırmaktır. Bu çağrı bir kullanıcı bir üyesi, çok sayıda grupları dahil olan ve uygulama, kullanıcının üyesi olduğu tüm grupları listeleme gerekiyor bile kullanılabilir olduğu tüm grupları sağlar.  Grup numaralandırma ardından simge boyutu sınırlamaları bağımsızdır.

Ancak, Azure Active Directory grubu bilgileri belirteçteki talepleri aldığı aracılığıyla kullanmak zaten var olan bir uygulamayı bekliyorsa uygulamanızın gereksinimlerine uyacak şekilde farklı talepler seçenekleri bir dizi yapılandırılabilir.  Aşağıdaki seçenekleri göz önünde bulundurun:

- Grup üyeliği, uygulama kimlik doğrulama amacıyla kullanırken, sabit ve Azure Active Directory'de benzersiz ve kullanılabilir tüm gruplara ilişkin Grup objectID kullanmak için tercih edilir.
- Şirket içi Grup sAMAccountName yetkilendirme için kullanıyorsanız, tam etki alanı adları kullanın.  sahip doğan durumlarda olasılığını daha az vardı ad çakışmasına. Kendi kendine SAMAccountName bir Active Directory etki alanı içinde benzersiz olmalıdır, ancak birden fazla Active Directory etki alanı bir Azure Active Directory kiracısı ile eşitlenirse aynı ada sahip birden fazla grubu için bir olasılık yoktur.
- Kullanmayı [uygulama rolleri](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) grup üyeliği ve uygulama arasında bir yöneltme katmanı sağlamak için.   Uygulama rolü clams belirtecindeki göre iç yetkilendirme kararları.
- Uygulama, Active Directory'den eşitlenen grup öznitelikleri almak için yapılandırılmışsa ve bir grubu özniteliklerle içermiyor Taleplerde dahil edilmeyecektir.
- İç içe geçmiş gruplar belirteçlerinde grup talepleri içerir.   Bir kullanıcı GroupB bir üyesidir ve GroupB GroupA üyesi ise, kullanıcı için Grup talepleri GroupA hem GroupB içerecektir. İç içe grupların yoğun kullanımı olan Kuruluş ve kullanıcılar çok sayıda grup üyeliklerini belirteç listelenen grubu simge boyutu büyür.   Azure Active Directory grupları için SAML onaylamalarını 150 ve 200 JWT belirteçleri çok fazla büyümesini önlemek için bir belirteç yayar sayısını sınırlar.  Bir kullanıcı grupları daha büyük bir dizi sınırdan üyesi ise, gruplara gönderilir ve grup bilgilerini almak için Graph uç noktası için bir bağlantı.

> Active Directory'nizden eşitlenen grup öznitelikleri kullanma önkoşulları:   Grupları Azure AD Connect kullanarak Active Directory'den eşitlenmesi gerekir.

Grup adları Active Directory grupları için yaymak için Azure Active Directory'yi yapılandırma için iki adımı vardır.

1. **Eşitleme grubu adları Active Directory'den** önce Azure Active Directory grup adlarını yayabilir veya şirket içi Grup SID grubu veya rol talep kuralları, gerekli öznitelikleri Active Directory'den eşitlenmesi gerekir.  Azure AD Connect sürümü 1.2.70 çalıştırılması gerekir ya da daha sonra.   Sürümünden 1.2.70 Azure AD Connect Active Directory grubu nesnelerden eşitler ancak varsayılan olarak gerekli grup adı özniteliklerini içermez.  Geçerli sürümüne yükseltmeniz gerekir.

2. **Azure Active Directory'de Grup talepleri belirteçlerinde dahil etmek için uygulama kaydı yapılandırma** grup talepleri olabilir ya da bir galeride veya galeri dışı SAML SSO uygulama için portalı kurumsal uygulamalar bölümündeki yapılandırılmış veya Uygulama bildirimi uygulama kayıtları bölümünde kullanma.  Uygulama bildirim görüyor "Altındaki grup öznitelikleri için Azure Active Directory Uygulama kaydı yapılandırma" Grup talepleri yapılandırmak için.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>SSO Yapılandırması'nı kullanarak SAML uygulamaları için Grup talepleri yapılandırma

Galeride veya galeri dışı SAML bir uygulama için Grup talepleri yapılandırmak için kurumsal uygulamalar, uygulama listesinde tıklayın ve çoklu oturum açma yapılandırmasını seçin.

"Grupları belirtecinde verilen" yanında bulunan düzenleme simgesini seçin

![Talep kullanıcı Arabirimi](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Hangi grupları seçmek için radyo düğmeleri belirteç eklenmelidir

![Talep kullanıcı Arabirimi](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Seçim | Açıklama |
|----------|-------------|
| **Tüm grupları** | Güvenlik grupları ve dağıtım yayan listeler.   Ayrıca kullanıcı bir 'wids' talebi yayılan atandığı Dizin rolleri ve kullanıcı rolleri talebi yayılan atandığı herhangi bir uygulama rolü neden olur. |
| **Güvenlik grupları** | Kullanıcı grupları talep bir üyesi olduğu güvenlik gruplarıyla yayar |
| **Dağıtım listeleri** | Kullanıcının üyesi olduğu dağıtım listeleri yayar |
| **Dizin rolleri** | Kullanıcının Dizin rolleri atanır (talep yayınlanmayacak grupları) 'wids' talebi olarak bunlar gönderilir |

Örneğin, kullanıcının üyesi olduğu tüm güvenlik gruplarını yaymak için güvenlik gruplarını seçin.

![Talep kullanıcı Arabirimi](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

Yaymak için grupları yerine Azure AD objectIDs Active Directory'den eşitlenen Active Directory öznitelikleri kullanarak gerekli biçime açılan listeden seçin.  Bu grup adlarını içeren dize değerleriyle talepleri nesne kimliği değiştirir.   Yalnızca Active Directory'nizden eşitlenen gruplar Taleplerde dahil edilir.

![Talep kullanıcı Arabirimi](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

### <a name="advanced-options"></a>Gelişmiş seçenekler

Gelişmiş seçenekleri altından ayarları tarafından grup talepleri gösterilen şekilde değiştirilebilir.

Grup talebi adını Özelleştir:  Seçtiyseniz, farklı talep türü için Grup talepleri belirtilebilir.   Talep türünü talep ad alanına ad alanı ve isteğe bağlı ad girin.

![Talep kullanıcı Arabirimi](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Bazı uygulamalar 'role' talebi görüntülenecek grup üyeliği bilgileri gerektirir. İsteğe bağlı olarak, 'bir rol talep grupları yayma' kutuyu işaretleyerek kullanıcı grupları rolleri olarak gönderebilir.

![Talep kullanıcı Arabirimi](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Rol olarak Grup verileri Yayımla seçeneğini kullandıysanız, yalnızca gruplar rol talebi görünür.  Kullanıcıya atanan tüm uygulama rolleri, rol talebi görünmez.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Grup öznitelikleri için Azure AD uygulaması kaydını yapılandırma

Grup talepleri de yapılandırılabilir [isteğe bağlı bir talep](../../active-directory/develop/active-directory-optional-claims.md) bölümünü [uygulama bildirimi](../../active-directory/develop/reference-app-manifest.md).

1. Portalda Azure Active Directory -> -> Uygulama kayıtları seçin -> uygulama bildirimi ->

2. Grup üyeliğini talep groupMembershipClaim değiştirerek etkinleştir

   Geçerli değerler şunlardır:

   - "Tüm"
   - "IDAP"
   - "DistributionList"
   - "DirectoryRole"

   Örneğin:

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   Grup içinde grup ObjectIDs derleyicisindeki varsayılan talep değeri.  Şirket içi grup öznitelikleri içeren veya rol talep türünü değiştirmek için talep değerini değiştirmek için OptionalClaims yapılandırma şu şekilde kullanın:

3. Grup adı yapılandırması isteğe bağlı talepleri ayarlayın.

   Belirteç grupları şirket içi AD grup öznitelikleri isteğe bağlı bir talep bölümünde hangi belirteç türü isteğe bağlı bir talep uygulanması gerektiğini belirtmek, istenen isteğe bağlı bir talep ve istenen herhangi bir ek özellik adını içerecek şekilde isterseniz.  Birden çok belirteç türleri listelenir:

   - OIDC kimlik belirteci için ilgili Idtoken
   - OAuth/OIDC erişim belirteci için accessToken
   - SAML belirteçleri Saml2Token.

   > [!NOTE]
   > Biçim belirteçleri Saml2Token türü SAML1.1 hem SAML2.0 için geçerlidir

   İlgili her belirteç türü için bildirim OptionalClaims bölümde kullanmak üzere gruplar talep değiştirin. OptionalClaims Şeması aşağıdaki gibidir:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | İsteğe bağlı bir talep şeması | Değer |
   |----------|-------------|
   | **Adı:** | "Grupları" olmalıdır |
   | **Kaynak:** | Kullanılmıyor. Atlayın veya null |
   | **gerekli:** | Kullanılmıyor. Atlarsanız veya false değerini belirtin |
   | **additionalProperties:** | Ek özellikler listesi.  Geçerli seçenekler: "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   AdditionalProperties içinde yalnızca bir "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" gereklidir.  Birden fazla varsa, ilk kullanılır ve diğerleri yoksayılır.

   Bazı uygulamalar, rol talep kullanıcı grup bilgilerini gerektirir.  Bir gruptan bir rol talep için talep için talep türünü değiştirmek için "emit_as_roles" için ek özellikleri ekleyin.  Grubu değerlerini, rol talebi yayılan.

   > [!NOTE]
   > "Emit_as_roles" kullanılırsa, kullanıcının atandığı herhangi bir uygulama rolü yapılandırılmış rol talebi görünmüyor

### <a name="examples"></a>Örnekler

Grup adları dnsDomainName\SAMAccountName biçimde OAuth erişim belirteçleri olarak grupları yayma

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

SAML ve OIDC kimlik belirteçlerini rol talebi olarak netbiosDomain\samAccountName biçiminde döndürülecek grubu adları göstermek için:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }
 ```

## <a name="next-steps"></a>Sonraki adımlar

[Hibrit kimlik nedir?](whatis-hybrid-identity.md)