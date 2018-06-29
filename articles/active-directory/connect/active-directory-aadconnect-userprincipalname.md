---
title: Azure AD UserPrincipalName doldurma
description: Aşağıdaki belge UserPrincipalName özniteliği nasıl doldurulur açıklar.
author: billmath
ms.component: hybrid
ms.author: billmath
ms.date: 06/26/2018
ms.topic: article
ms.workload: identity
ms.service: active-Directory
manager: mtillman
ms.openlocfilehash: 6b3fddcdf6ff9c35d5932b9b83da02f60f9e081e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064055"
---
# <a name="azure-ad-userprincipalname-population"></a>Azure AD UserPrincipalName doldurma

Bu makalede, Azure Active Directory (Azure AD) UserPrincipalName özniteliği nasıl doldurulur açıklanmaktadır.
UserPrincipalName özniteliğinin değeri, kullanıcı hesapları için Azure AD kullanıcı adıdır.

## <a name="upn-terminology"></a>UPN terminolojisi
Bu makalede aşağıdaki terimler kullanılır:

|Sözleşme Dönemi|Açıklama|
|-----|-----|
|İlk etki alanı|Varsayılan etki alanına (onmicrosoft.com) Azure AD Kiracısı. Örneğin, contoso.onmicrosoft.com.|
|Microsoft çevrimiçi e-posta adresi (MOERA) yönlendirme|Azure AD hesaplar Azure AD MailNickName özniteliği ve Azure AD ilk etki alanı olarak MOERA &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.|
|Şirket içi mailNickName özniteliği|Bir öznitelik değeri, Active Directory içinde bir kullanıcı bir Exchange kuruluşunda diğer adı temsil eder.|
|Şirket içi posta özniteliği|Bir kullanıcının e-posta adresini Active Directory'de değeri bir özniteliği temsil eder|
|Birincil SMTP adresi|Bir Exchange alıcı nesne birincil e-posta adresi. Örneğin, SMTP:user\@contoso.com.|
|Alternatif oturum açma kimliği|Oturum açma için kullanılan posta öznitelik gibi bir şirket içi özniteliği, UserPrincipalName dışında.|

## <a name="what-is-userprincipalname"></a>UserPrincipalName nedir?
UserPrincipalName olan bir Internet stil oturum açma adı Internet standardına göre bir kullanıcı için bir öznitelik [RFC 822](http://www.ietf.org/rfc/rfc0822.txt). 

### <a name="upn-format"></a>UPN biçimi
UPN UPN önekini (kullanıcı hesabı adı) ve bir UPN soneki (bir DNS etki alanı adı) oluşur. Önek kullanarak soneki ile birleştirilmiş "\@" simgesi. Örneğin, "birisi\@example.com". UPN directory ormanındaki tüm güvenlik sorumlusu nesneleri arasında benzersiz olması gerekir. 

## <a name="upn-in-azure-ad"></a>Azure AD'de UPN 
UPN oturum açma yapmalarına izin vermek için Azure AD tarafından kullanılır.  Bir kullanıcı kullanabilir, UPN bağlıdır etki alanı desteklemediğini doğrulandı.  Etki alanı doğrulandı, ardından söz konusu sonekine sahip bir kullanıcı Azure AD ile oturum açmak için izin verilir.  

Öznitelik, Azure AD Connect tarafından eşitlenir.  Yükleme sırasında doğrulanmış etki alanlarını ve olmaması olanları görüntüleyebilirsiniz.

   ![Doğrulanmamış etki alanları](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png) 

## <a name="alternate-login-id"></a>Alternatif oturum açma kimliği
Bazı ortamlarda, son kullanıcılar yalnızca kendi e-posta adresi ve bunların UPN duyarlı olabilir.  E-posta adresinin kullanımını şirket ilkesi veya bir şirket içi iş kolu satır uygulama bağımlılık nedeniyle olabilir.

Alternatif oturum açma kimliği, bir oturum açma deneyimini yapılandırmak burada kullanıcılar oturum-posta gibi kendi UPN dışında bir özniteliği olan sağlar.

Azure AD ile alternatif oturum açma kimliği etkinleştirmek için Azure AD Connect kullanırken hiçbir ek yapılandırma adımları gereklidir. Alternatif kimlik doğrudan sihirbazından yapılandırılabilir. Eşitleme bölümünde, kullanıcılarınızın Azure AD oturum açma yapılandırması bakın. Altında **kullanıcı asıl adı** açılan listesinde, öznitelik için alternatif oturum açma kimliği seçin.

![Doğrulanmamış etki alanları](./media/active-directory-aadconnect-userprincipalname/altloginid.png)  

Daha fazla bilgi için bkz: [yapılandırma alternatif oturum açma Kimliğini](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) ve [Azure AD oturum açma yapılandırması](active-directory-aadconnect-get-started-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Doğrulanmamış UPN soneki
Şirket içi UserPrincipalName özniteliği/alternatif oturum açma kimliği soneki ile Azure AD Kiracı doğrulanamazsa, Azure AD UserPrincipalName öznitelik değeri için MOERA ayarlanır. Azure AD hesaplar Azure AD MailNickName özniteliği ve Azure AD ilk etki alanı olarak MOERA &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.

## <a name="verified-upn-suffix"></a>Doğrulanmış UPN soneki
Şirket içi UserPrincipalName özniteliği/alternatif varsa Azure AD UserPrincipalName özniteliği değeri şirket içi UserPrincipalName özniteliği/alternatif oturum açma kimliği değeri ile aynı olacak sonra oturum açma kimliği soneki Azure AD Kiracısı ile doğrulanır.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Azure AD MailNickName öznitelik değeri hesaplama
Azure AD UserPrincipalName öznitelik değeri için MOERA ayarlanamıyor çünkü MOERA önektir, Azure AD MailNickName öznitelik değeri nasıl hesaplandığını anlamak önemlidir.

Bir kullanıcı nesnesi ilk kez bir Azure AD Kiracısı eşitlendiğinde, Azure AD verilen sırayla aşağıdaki öğeleri denetler ve mevcut birinciye MailNickName öznitelik değeri ayarlar:

- Şirket içi mailNickName özniteliği
- Birincil SMTP adresi öneki
- Şirket içi posta özniteliğinin öneki
- Şirket içi userPrincipalName özniteliği/alternatif oturum açma kimliği öneki
- İkincil smtp adresi öneki

Bir kullanıcı nesnesi güncelleştirmeler için Azure AD Kiracı eşitlenirken Azure AD MailNickName öznitelik değeri güncelleştirmeler olduğunda şirket içi mailNickName öznitelik değeri için bir güncelleştirme yalnızca.

>[!IMPORTANT]
>Şirket içi UserPrincipalName özniteliği/alternatif oturum açma kimliği değeri için bir güncelleştirme için Azure AD Kiracı yalnızca eşitlenmiş durumda azure AD UserPrincipalName özniteliği değeri yeniden hesaplar. 
>
>Ayrıca Azure AD UserPrincipalName özniteliği yeniden hesaplar her MOERA yeniden hesaplar. 

## <a name="upn-scenarios"></a>UPN senaryoları
Nasıl UPN temel alınarak belirli senaryoya hesaplanır örnek senaryolar verilmiştir.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Senaryo 1: Doğrulanmamış UPN soneki – ilk eşitleme

![Scenario1](media/active-directory-aadconnect-userprincipalname/example1.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: &lt;ayarlı değil&gt;
- proxyAddresses: {SMTP:us1@contoso.com}
- posta: us2@contoso.com
- userPrincipalName: us3@contoso.com'

İlk olarak Azure AD Kiracı Kullanıcı nesnesine eşitlendi
- Azure AD MailNickName özniteliği birincil SMTP adresi öneki ayarlayın.
- Kümesine MOERA &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.
- Azure AD UserPrincipalName özniteliği için MOERA ayarlayın.

Azure AD Kiracı Kullanıcı nesnesi:
- MailNickName: us1           
- UserPrincipalName: us1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Senaryo 2: Doğrulanmamış UPN soneki – kümesi mailNickName özniteliği şirket içi

![Scenario2](media/active-directory-aadconnect-userprincipalname/example2.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us1@contoso.com}
- posta: us2@contoso.com
- userPrincipalName: us3@contoso.com

Azure AD Kiracısı için şirket içi mailNickName öznitelikte güncelleştirme Eşitle
- Azure AD MailNickName özniteliği şirket içi mailNickName özniteliği ile güncelleştirin.
- Şirket içi userPrincipalName özniteliği için hiçbir güncelleştirme olduğundan, Azure AD UserPrincipalName özniteliği için herhangi bir değişiklik yoktur.

Azure AD Kiracı Kullanıcı nesnesi:
- MailNickName: us4
- UserPrincipalName: us1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Senaryo 3: Doğrulanmamış UPN soneki – güncelleştirme userPrincipalName özniteliği şirket içi

![Scenario3](media/active-directory-aadconnect-userprincipalname/example3.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us1@contoso.com}
- posta: us2@contoso.com
- userPrincipalName: us5@contoso.com

Azure AD Kiracısı için şirket içi userPrincipalName özniteliği Update'te Eşitle
- Şirket içi userPrincipalName özniteliği Update'te MOERA ve Azure AD UserPrincipalName özniteliği hesaplanması tetikler.
- Kümesine MOERA &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.
- Azure AD UserPrincipalName özniteliği için MOERA ayarlayın.

Azure AD Kiracı Kullanıcı nesnesi:
- MailNickName: us4
- UserPrincipalName: us4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Senaryo 4: Doğrulanmamış UPN soneki – güncelleştirme birincil SMTP adresi ve şirket içi posta özniteliği

![Scenario4](media/active-directory-aadconnect-userprincipalname/example4.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us6@contoso.com}
- posta: us7@contoso.com
- userPrincipalName: us5@contoso.com

Güncelleştirmeyi şirket içi posta özniteliğinin ve Azure AD Kiracısı için birincil SMTP adresini Eşitle
- Kullanıcı nesnesinin ilk eşitleme güncelleştirmeleri sonra şirket içi posta özniteliği ve birincil SMTP adresi Azure AD MailNickName veya UserPrincipalName özniteliği etkilemez.

Azure AD Kiracı Kullanıcı nesnesi:
- MailNickName: us4
- UserPrincipalName: us4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Senaryo 5: Doğrulanmış UPN soneki – güncelleştirme userPrincipalName özniteliği soneki şirket içi

![Scenario5](media/active-directory-aadconnect-userprincipalname/example5.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us6@contoso.com}
- posta: us7@contoso.com
- serPrincipalName: us5@verified.contoso.com

Azure AD Kiracısı için şirket içi userPrincipalName özniteliği Update'te Eşitle
- Şirket içi userPrincipalName özniteliği Tetikleyicileri hesaplanması Azure AD UserPrincipalName özniteliği üzerinde güncelleştirin.
- UPN soneki olarak şirket içi userPrincipalName özniteliği için kümesi Azure AD UserPrincipalName özniteliği, Azure AD Kiracı ile doğrulanır.

Azure AD Kiracı Kullanıcı nesnesi:
- MailNickName: us4     
- UserPrincipalName: us5@verified.contoso.com

## <a name="next-steps"></a>Sonraki Adımlar
- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
- [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md)
