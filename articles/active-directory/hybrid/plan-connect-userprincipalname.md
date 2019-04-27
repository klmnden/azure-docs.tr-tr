---
title: Azure AD UserPrincipalName popülasyonu
description: UserPrincipalName özniteliği nasıl doldurulur aşağıdaki belge açıklar.
author: billmath
ms.subservice: hybrid
ms.author: billmath
ms.date: 06/26/2018
ms.topic: conceptual
ms.workload: identity
ms.service: active-directory
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: c198b329f07c5c7459f25165b2dc0a3bfa032276
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60382132"
---
# <a name="azure-ad-userprincipalname-population"></a>Azure AD UserPrincipalName popülasyonu

Bu makalede, Azure Active Directory (Azure AD) UserPrincipalName özniteliği nasıl doldurulur açıklanır.
UserPrincipalName özniteliği değeri, kullanıcı hesapları için Azure AD kullanıcı adıdır.

## <a name="upn-terminology"></a>UPN terminolojisi
Bu makalede aşağıdaki terimler kullanılır:

|Sözleşme Dönemi|Açıklama|
|-----|-----|
|İlk etki alanı|Varsayılan etki alanı (onmicrosoft.com) Azure AD kiracısında. Örneğin, contoso.onmicrosoft.com.|
|Microsoft Online e-posta adresi (MOERA) yönlendirme|Azure AD, Azure AD MailNickName özniteliğine ve İlk Azure AD etki alanı olarak MOERA hesaplar &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.|
|Şirket içi mailNickName özniteliği|Bir öznitelik değeri, Active Directory içinde bir kullanıcı bir Exchange kuruluşunda diğer adı temsil eder.|
|Şirket içi posta özniteliği|Bir öznitelik değeri, Active Directory içinde bir kullanıcının e-posta adresini temsil eder.|
|Birincil SMTP adresi|Bir Exchange alıcı nesne birincil e-posta adresi. Örneğin, SMTP:user\@contoso.com.|
|Alternatif oturum açma kimliği|Posta özniteliği, oturum açmak için kullanılan gibi bir şirket içi özniteliği, UserPrincipalName dışında.|

## <a name="what-is-userprincipalname"></a>UserPrincipalName nedir?
UserPrincipalName Internet standardına göre bir kullanıcı için bir Internet stili oturum açma adı bir öznitelik olduğunu [RFC 822](https://www.ietf.org/rfc/rfc0822.txt). 

### <a name="upn-format"></a>UPN biçimi
UPN (kullanıcı hesabı adı) UPN öneki ve UPN soneki (bir DNS etki alanı adı) oluşur. Önekini kullanarak soneki ile birleştirilmiş "\@" simgesi. Örneğin, "birisi\@example.com". UPN directory ormanındaki tüm güvenlik asıl nesneler arasında benzersiz olmalıdır. 

## <a name="upn-in-azure-ad"></a>Azure AD'de UPN 
UPN oturum açmasına izin vermek için Azure AD tarafından kullanılır.  Bir kullanıcı kullanabilir, UPN bağlıdır etki alanında olup olmadığını doğrulandı.  Etki alanını doğruladıysanız ardından söz konusu sonekine sahip bir kullanıcı Azure AD'de oturum izin.  

Öznitelik, Azure AD Connect tarafından eşitlenir.  Yükleme sırasında doğrulanmış etki alanları ve olmayan vm'lere görüntüleyebilirsiniz.

   ![Doğrulanmamış etki alanları](./media/plan-connect-userprincipalname/unverifieddomain.png) 

## <a name="alternate-login-id"></a>Alternatif oturum açma kimliği
Bazı ortamlarda, son kullanıcıların yalnızca e-posta adresi ve bunların UPN farkında olabilir.  E-posta adresi kullanımı, bir şirket ilkesi veya bir şirket içi iş kolu satır uygulama bağımlılığı nedeniyle olabilir.

Alternatif oturum açma kimliği nerede kullanıcıların UPN, e-posta gibi farklı bir öznitelik ile oturum bir oturum açma deneyimi yapılandırmanıza olanak sağlar.

Azure AD ile alternatif oturum açma kimliği'ni etkinleştirmek için Azure AD Connect kullanırken hiçbir ek yapılandırma adımları gereklidir. Alternatif kimlik doğrudan sihirbazından yapılandırılabilir. Eşitleme bölümünde, kullanıcılarınızın Azure AD oturum açma yapılandırması bakın. Altında **kullanıcı asıl adı** açılan listesinde, öznitelik için alternatif bir oturum açma kimliğini seçin

![Doğrulanmamış etki alanları](./media/plan-connect-userprincipalname/altloginid.png)  

Daha fazla bilgi için [yapılandırma alternatif oturum açma Kimliğini](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) ve [Azure AD oturum açma yapılandırması](how-to-connect-install-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Doğrulanmamış UPN soneki
Şirket içi UserPrincipalName özniteliği/alternatif oturum açma kimliği soneki ile Azure AD Kiracısı doğrulanamazsa, Azure AD UserPrincipalName özniteliği değeri için MOERA ayarlayın. Azure AD MOERA ilk Azure AD etki alanı olarak ve Azure AD MailNickName özniteliğine hesaplar &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.

## <a name="verified-upn-suffix"></a>Doğrulanmış UPN soneki
Şirket içi UserPrincipalName özniteliği/alternatif, şirket içi UserPrincipalName özniteliği/alternatif oturum açma kimliği değeri ile aynı Azure AD UserPrincipalName özniteliği değeri geçiyor sonra oturum açma kimliği soneki Azure AD Kiracınız ile doğrulanır.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Azure AD MailNickName özniteliğinin değeri hesaplama
Azure AD UserPrincipalName özniteliği değeri MOERA için ayarlanabilir, çünkü MOERA önekidir, Azure AD MailNickName öznitelik değeri nasıl hesaplandığını anlamak önemlidir.

Kullanıcı nesnesini Azure AD Kiracısı için ilk kez eşitlendiğinde, Azure AD aşağıdaki öğeleri verilen sırayla denetler ve mevcut birincisine MailNickName özniteliğinin değeri ayarlar:

- Şirket içi mailNickName özniteliği
- Birincil SMTP adresi öneki
- Şirket içi posta özniteliğinin öneki
- Şirket içi userPrincipalName özniteliği/alternatif oturum açma kimliği öneki
- İkincil smtp adresi öneki

Azure AD Kiracısına kullanıcı nesnesi güncelleştirmeleri eşitlenirken MailNickName özniteliğinin Azure AD'ye güncelleştirmeleri yalnızca şirket içi mailNickName özniteliğinin değeri için bir güncelleştirme var. durumda.

>[!IMPORTANT]
>Şirket içi UserPrincipalName özniteliği/alternatif oturum açma kimliği değeri için bir güncelleştirme için Azure AD Kiracısı yalnızca eşitlenmiş durumda azure AD UserPrincipalName özniteliği değeri yeniden hesaplar. 
>
>Ayrıca Azure AD UserPrincipalName özniteliği yeniden hesaplar her MOERA yeniden hesaplar. 

## <a name="upn-scenarios"></a>UPN senaryoları
Nasıl UPN göre belirli bir senaryoya göre hesaplanır örnek senaryolar verilmiştir.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Senaryo 1: UPN soneki olmayan doğrulandı – ilk eşitleme

![Scenario1](./media/plan-connect-userprincipalname/example1.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: &lt;ayarlı değil&gt;
- proxyAddresses: {SMTP:us1@contoso.com}
- posta: us2@contoso.com
- userPrincipalName: us3@contoso.com'

Azure AD kiracınıza kullanıcı nesnesinin ilk kez eşitlenir.
- Azure AD MailNickName özniteliğinin birincil SMTP adresi öneki ayarlayın.
- Kümesine MOERA &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.
- Azure AD UserPrincipalName özniteliği için MOERA ayarlayın.

Azure AD Kiracısına kullanıcı nesnesi:
- MailNickName      : us1           
- UserPrincipalName: us1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Senaryo 2: UPN soneki olmayan doğrulandı – kümesi mailNickName özniteliğinin şirket

![Scenario2](./media/plan-connect-userprincipalname/example2.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us1@contoso.com}
- posta: us2@contoso.com
- UserPrincipalName: us3@contoso.com

Azure AD Kiracısı için şirket içi mailNickName özniteliğine güncelleştirme Eşitle
- Azure AD MailNickName özniteliğine, şirket içi mailNickName özniteliğinin ile güncelleştirin.
- Şirket içi userPrincipalName özniteliği için güncelleştirme olduğundan, Azure AD UserPrincipalName özniteliği için değişiklik yoktur.

Azure AD Kiracısına kullanıcı nesnesi:
- MailNickName      : us4
- UserPrincipalName: us1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Senaryo 3: UPN soneki olmayan doğrulandı – güncelleştirme userPrincipalName özniteliği şirket

![Scenario3](./media/plan-connect-userprincipalname/example3.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us1@contoso.com}
- posta: us2@contoso.com
- UserPrincipalName: us5@contoso.com

Azure AD Kiracısı için şirket içi userPrincipalName özniteliği güncelleştirmesinde Eşitle
- Şirket içi userPrincipalName özniteliği güncelleştirmesinde MOERA ve Azure AD UserPrincipalName özniteliği hesaplanması tetikler.
- Kümesine MOERA &lt;MailNickName&gt;&#64;&lt;ilk etki alanı&gt;.
- Azure AD UserPrincipalName özniteliği için MOERA ayarlayın.

Azure AD Kiracısına kullanıcı nesnesi:
- MailNickName      : us4
- UserPrincipalName: us4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Senaryo 4: Olmayan doğrulandı UPN soneki – güncelleştirme birincil SMTP adresini ve şirket içi posta özniteliği

![Scenario4](./media/plan-connect-userprincipalname/example4.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us6@contoso.com}
- posta: us7@contoso.com
- UserPrincipalName: us5@contoso.com

Şirket içi posta özniteliği ve Azure AD Kiracısı için birincil SMTP adresi güncelleştirmesinde Eşitle
- İlk eşitleme kullanıcı nesnesinin güncelleştirmeleri sonra şirket içi posta özniteliği ve birincil SMTP adresini, Azure AD MailNickName veya UserPrincipalName özniteliği etkilemez.

Azure AD Kiracısına kullanıcı nesnesi:
- MailNickName      : us4
- UserPrincipalName: us4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Senaryo 5: UPN soneki doğrulanmış – güncelleştirme userPrincipalName özniteliği soneki şirket

![Scenario5](./media/plan-connect-userprincipalname/example5.png)

Şirket içi kullanıcı nesnesi:
- mailNickName: us4
- proxyAddresses: {SMTP:us6@contoso.com}
- posta: us7@contoso.com
- UserPrincipalName: us5@verified.contoso.com

Azure AD Kiracısı için şirket içi userPrincipalName özniteliği güncelleştirmesinde Eşitle
- Şirket içi userPrincipalName özniteliği Tetikleyicileri hesaplanması Azure AD UserPrincipalName özniteliği üzerinde güncelleştirin.
- Azure AD UserPrincipalName özniteliği, UPN soneki, Azure AD Kiracısı ile doğrulanmış şirket içi userPrincipalName özniteliği ayarlayın.

Azure AD Kiracısına kullanıcı nesnesi:
- MailNickName      : us4     
- UserPrincipalName: us5@verified.contoso.com

## <a name="next-steps"></a>Sonraki Adımlar
- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
