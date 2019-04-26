---
title: 'Azure AD Connect: Kullanıcı oturumu açma | Microsoft Docs'
description: Azure AD Connect kullanıcı oturum açma için özel ayarlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb44c64540cc461bca4e305f7783f7c6b612591b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60296451"
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect kullanıcı oturum açma seçenekleri
Azure Active Directory (Azure AD) Connect, kullanıcılarınızın hem bulut hem de şirket içi kaynaklara aynı parolayı kullanarak oturum açmasını sağlar. Bu makalede, Azure AD'de oturum açmak için kullanmak istediğiniz kimlik seçmenize yardımcı olmak her bir kimlik modeli için temel kavramları açıklar.

Azure AD kimlik modeliyle zaten biliyor ve belirli bir yöntemi hakkında daha fazla bilgi edinmek istiyorsanız, uygun bağlantıya bakın:

* [Parola Karması eşitleme](#password-hash-synchronization) ile [sorunsuz çoklu oturum açma (SSO)](how-to-connect-sso.md)
* [Geçişli kimlik doğrulaması](how-to-connect-pta.md) ile [sorunsuz çoklu oturum açma (SSO)](how-to-connect-sso.md)
* [Federasyon SSO (ile Active Directory Federasyon Hizmetleri (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)
* [PingFederate ile Federasyon](#federation-with-pingfederate)

> [!NOTE] 
> Azure AD için Federasyon yapılandırarak, Azure AD kiracınız ile Federasyon etki alanları arasında güven olduğunu unutmamak önemlidir. Bu güven Federasyon etki alanı ile kullanıcılar Kiracı içinde Azure AD bulut kaynaklarına erişebilir.  
>

## <a name="choosing-the-user-sign-in-method-for-your-organization"></a>Kuruluşunuz için kullanıcı oturum açma yöntemi seçme
Azure AD Connect uygulama ilk karar, hangi kimlik doğrulama yöntemi, kullanıcılarınızın oturum açmak için kullanacağı seçmektir. Kuruluşunuzun güvenlik ve Gelişmiş gereksinimlerinizi karşılayan doğru yöntemi seçtiğinizden emin olmak önemlidir. Kimlik doğrulaması kritik, çünkü kullanıcının erişimi uygulamaları ve verileri bulutta kimlikleri doğrulanır. Doğru kimlik doğrulama yöntemini seçmek için zaman, mevcut altyapı, karmaşıklığı ve maliyeti, seçtiğiniz uygulama göz önünde bulundurmanız gerekir. Bu faktörlerin her kuruluş için farklıdır ve zaman içinde değişebilir.

Azure AD aşağıdaki kimlik doğrulama yöntemlerini destekler: 

* **Bulut kimlik doğrulaması** - bu Azure AD kimlik doğrulama yöntemi, kullanıcının oturum açma için kimlik doğrulama işlemi işleme'yi seçin. Bulut kimlik doğrulamasıyla iki seçenekler arasından seçim yapabilirsiniz: 
   * **Parola Karması eşitleme (PHS)** -parola karma eşitlemesi sağlayan aynı kullanıcı adı ve parola şirket içinde kullandıkları kullanıcılar Azure AD Connect yanı sıra ek altyapı dağıtmak zorunda kalmadan. 
   * **Geçişli kimlik doğrulaması (PTA)** -bu seçenek parola karması eşitleme için benzer, ancak güçlü güvenlik ve uyumluluk ilkeleri olan kuruluşlar şirket içi yazılım aracıları kullanarak bir basit parola doğrulama sağlar.
* **Federe kimlik doğrulaması** - AD FS veya bir üçüncü taraf Federasyon sistem kullanıcıyı doğrulamak için oturum açma gibi, bu Azure AD kimlik doğrulama yöntemi el kapalı bir güvenilen kimlik doğrulama sistemi için kimlik doğrulama işlemi seçin. 

Kullanıcı oturum açma için Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynakları etkinleştirmek istediğiniz çoğu kuruluş için varsayılan parola karması eşitleme seçeneğini öneririz.
 
Bir kimlik doğrulama yöntemi seçme hakkında ayrıntılı bilgi için bkz: [, Azure Active Directory karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin](../../security/azure-ad-choose-authn.md)

### <a name="password-hash-synchronization"></a>Parola karması eşitleme
Parola Karması eşitleme ile kullanıcı parola karmalarının Azure AD'ye şirket içi Active Directory'nizden eşitlenen. Parolaları değiştirilen veya şirket içi sıfırlama, yeni parola karmalarının Azure AD'ye eşitlenen böylece hemen kullanıcılarınızın her zaman aynı parolayı bulut kaynaklarına ve şirket içi kaynaklar için kullanabilirsiniz. Parolalar hiçbir zaman gönderilen Azure AD'ye veya Azure AD düz metin olarak depolanır. Self Servis parola sıfırlama Azure AD'de etkinleştirmek için parola karması eşitleme parola geri yazma ile birlikte kullanabilirsiniz.

Ayrıca, etkinleştirebilirsiniz [sorunsuz çoklu oturum açma](how-to-connect-sso.md) kullanıcılar kurumsal ağdaki etki alanına katılmış makinelerde için. Çoklu oturum açma ile etkin kullanıcılar yalnızca bulut kaynaklarına erişimi güvenli bir şekilde yardımcı olmak için bir kullanıcı adı girmeniz gerekir.

![Parola karması eşitleme](./media/plan-connect-user-signin/passwordhash.png)

Daha fazla bilgi için [parola karması eşitleme](how-to-connect-password-hash-synchronization.md) makalesi.

### <a name="pass-through-authentication"></a>Doğrudan kimlik doğrulama
Geçişli kimlik doğrulaması ile şirket içi Active Directory denetleyicisine karşı kullanıcının parolasını doğrulanır. Parola, herhangi bir biçimde Azure AD'de mevcut olması gerekmez. Bu şirket içi ilkeleri, saat, oturum açma kısıtlamaları gibi bulut kimlik doğrulaması sırasında değerlendirilecek hizmetleri sağlar.

Geçişli kimlik doğrulaması, şirket içi ortamındaki etki alanı ile birleşik Windows Server 2012 R2 makinesine basit bir aracı kullanır. Bu aracı, parola doğrulama isteklerini dinler. Bu, Internet'e açık olmasını gelen bağlantı noktalarının gerektirmez.

Ayrıca, kurumsal ağ üzerindeki etki alanına katılmış makinelerde kullanıcılar için çoklu oturum açmayı de etkinleştirebilirsiniz. Çoklu oturum açma ile etkin kullanıcılar yalnızca bulut kaynaklarına erişimi güvenli bir şekilde yardımcı olmak için bir kullanıcı adı girmeniz gerekir.
![Doğrudan kimlik doğrulama](./media/plan-connect-user-signin/pta.png)

Daha fazla bilgi için bkz.
- [Doğrudan kimlik doğrulama](how-to-connect-pta.md)
- [Çoklu oturum açma](how-to-connect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS ile yeni veya mevcut bir grubu kullanan Federasyon
Federasyon ile oturum açma, kullanıcılarınızın şirket içi parolalarını ile Azure AD tabanlı hizmetlere oturum açabilirsiniz. Bunlar şirket ağında olduğunuzda bunların bile parolalarını girmeniz gerekmez. AD FS ile Federasyon seçeneğini kullanarak Windows Server 2012 R2'de AD FS ile yeni veya mevcut bir grubu dağıtabilirsiniz. Mevcut bir grubu belirtmek isterseniz, Azure AD Connect, kullanıcılar oturum açabilir, grubu ve Azure AD arasında güven yapılandırır.

<center>

![Windows Server 2012 R2'de AD FS ile Federasyon](./media/plan-connect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS ile Federasyon dağıtma

Yeni bir grup dağıtıyorsanız ihtiyacınız vardır:

* Federasyon sunucusu için Windows Server 2012 R2 sunucusu.
* Web uygulama proxy'si için Windows Server 2012 R2 sunucusu.
* Bir SSL sertifikası ile bir .pfx dosyası hedeflenen Federasyon Hizmeti adı. Örneğin: fs.contoso.com.

Yeni grubu dağıtma ya da mevcut bir gruba kullanarak, gerekir:

* Federasyon sunucularınız üzerinde yerel yönetici kimlik bilgileri.
* Web uygulaması Proxy rolünü dağıtmak istediğiniz yerel yönetici kimlik bilgileri tüm çalışma grubu sunucuları (değil etki alanına katılmış).
* Sihirbaz, diğer makinelere bağlanmanız için çalıştırdığınız makine istediğiniz AD FS veya Web uygulaması Ara sunucusu Windows Uzaktan Yönetimi'ni kullanarak yüklemek.

Daha fazla bilgi için [AD FS ile SSO yapılandırma](how-to-connect-install-custom.md#configuring-federation-with-ad-fs).

### <a name="federation-with-pingfederate"></a>PingFederate ile federasyon
Federasyon ile oturum açma, kullanıcılarınızın şirket içi parolalarını ile Azure AD tabanlı hizmetlere oturum açabilirsiniz. Bunlar şirket ağında olduğunuzda bunların bile parolalarını girmeniz gerekmez.

Azure Active Directory ile kullanmak için Pingfederate'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure Active Directory ve Office 365 ile PingFederate tümleştirme](https://www.pingidentity.com/AzureADConnect)

PingFederate kullanarak Azure AD Connect ayarlama hakkında daha fazla bilgi için bkz. [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>AD FS'den veya üçüncü taraf çözümü önceki bir sürümünü kullanarak oturum açın
Zaten bulut oturum açma (örneğin, AD FS 2.0) AD FS'den veya üçüncü taraf Federasyon sağlayıcısı önceki bir sürümünü kullanarak yapılandırdıysanız, Azure AD Connect aracılığıyla kullanıcı oturum açma yapılandırması atlamayı seçebilirsiniz. Bu son eşitleme ve diğer özellikleri Azure AD Connect oturum açma için hala var olan çözümünüzü kullanırken yararlanmanıza olanak tanıyacak.

Daha fazla bilgi için [Azure AD'ye üçüncü taraf Federasyon Uyumluluğu Listesi](how-to-connect-fed-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Kullanıcı oturum açma ve kullanıcı asıl adı
### <a name="understanding-user-principal-name"></a>Kullanıcı asıl adı anlama
Active Directory'de varsayılan kullanıcı asıl adı (UPN) sonekini kullanıcı hesabının oluşturulduğu etki alanı DNS adıdır. Çoğu durumda, bu, Internet üzerinde Kurumsal etki alanı olarak kayıtlı etki alanı adıdır. Ancak, daha fazla UPN soneki, Active Directory etki alanları ve Güvenleri'ni kullanarak ekleyebilirsiniz.

Kullanıcının UPN'sini şu biçimdedir username@domain. Örneğin, "contoso.com" adlı bir Active Directory etki alanı için Gamze adlı bir kullanıcı UPN olabilir "john@contoso.com". UPN kullanıcının RFC 822 üzerinde temel alır. UPN ve e-posta aynı biçimde olmasına rağmen bir kullanıcının UPN değerini olabilir veya kullanıcının e-posta adresiyle aynı olmayabilir.

### <a name="user-principal-name-in-azure-ad"></a>Azure AD'de kullanıcı asıl adı
Azure AD Connect Sihirbazı, userPrincipalName özniteliği kullanır veya şirket içinden Azure AD'de kullanıcı asıl adı kullanılmak üzere özniteliğinde (özel bir yükleme) belirtmenize olanak tanır. Azure AD'de oturum açmak için kullanılan değer budur. UserPrincipalName özniteliğinin değeri Azure AD'de doğrulanmış bir etki alanına karşılık gelmesi gerekmez, ardından Azure AD ile bir varsayılan değiştirir. onmicrosoft.com değeri.

Her dizinde bir Azure Active Directory biçimi contoso.onmicrosoft.com, Azure veya diğer Microsoft Hizmetleri kullanmaya başlamanıza olanak tanıyan bir yerleşik etki alanı adıyla birlikte gönderilir. Geliştirin ve özel etki alanları kullanarak oturum açma deneyimini basitleştirir. Azure AD'de özel etki alanı adlarını ve bir etki alanını doğrulama hakkında daha fazla bilgi için bkz: [Azure Active Directory'ye özel etki alanı adınızı ekleme](../fundamentals/add-custom-domain.md).

## <a name="azure-ad-sign-in-configuration"></a>Azure AD oturum açma yapılandırması
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD oturum açma yapılandırması Azure AD Connect ile
Azure AD oturum açma deneyimini, Azure AD için Azure AD dizininde doğrulanmış özel etki alanlarından biri eşitlenmiş bir kullanıcının kullanıcı asıl adı soneki olup eşleşebilir bağlıdır. Azure AD oturum açma ayarlarını yapılandırırken azure AD Connect Yardım sağlar, böylece kullanıcı oturum açma deneyimini bulutta şirket içi deneyimine benzer.

Azure AD Connect, bunları Azure AD'de özel etki alanı ile eşleştirmeye çalışır ve etki alanları için tanımlanan UPN sonekleri listeler. Ardından, yapılması gereken uygun eylem yardımcı olur.
Azure AD oturum açma sayfasının, her sonek karşı karşılık gelen durumunu görüntüler ve şirket içi Active Directory için tanımlanan UPN sonekleri listeler. Durum değerleri aşağıdakilerden biri olabilir:

| Durum | Açıklama | Eylem gerekli |
|:--- |:--- |:--- |
| Doğrulandı |Azure AD Connect, Azure AD'de doğrulanmış etki alanı için eşleşen bir bulundu. Bu etki alanı için tüm kullanıcılar, şirket içi kimlik bilgilerini kullanarak oturum açabilirsiniz. |Herhangi bir eylemde bulunmanız gerekmez. |
| Doğrulanmadı |Azure AD Connect, Azure AD'de eşleşen özel etki alanı bulundu, ancak doğrulanmış değil. Bu etki alanı kullanıcılarının UPN soneki, varsayılan değiştirilecek. onmicrosoft.com sonekini etki alanını doğruladıysanız değil eşitlemeden sonra. | [Azure AD'de özel etki alanını doğrulayın.](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) |
| Ekli değil |Azure AD Connect için UPN soneki corresponded özel bir etki alanı bulunamadı. Bu etki alanı kullanıcılarının UPN soneki, varsayılan değiştirilecek. onmicrosoft.com sonekini etki alanı eklenmiş değildir ve Azure'da doğrulanır. | [Ekleme ve için UPN son ekine karşılık gelen bir özel etki alanını doğrulayın.](../fundamentals/add-custom-domain.md) |

Azure AD oturum açma sayfasında şirket içi Active Directory ile ilgili özel etki alanı için geçerli doğrulama durumunu ile Azure AD'de tanımlanan UPN sonekleri listeler. Özel bir yükleme artık kullanıcı asıl adı özniteliğini üzerinde seçebilirsiniz **Azure AD oturum açma** sayfası.

![Azure AD oturum açma sayfası](./media/plan-connect-user-signin/custom_azure_sign_in.png)

Özel etki alanlarının en son durumu Azure AD'den yeniden getirmek için yenile düğmesine tıklayabilirsiniz.

### <a name="selecting-the-attribute-for-the-user-principal-name-in-azure-ad"></a>Azure AD'de kullanıcı asıl adı özniteliğini seçme
UserPrincipalName özniteliği Azure AD'de oturum açtığında, kullanıcıların özniteliktir ve Office 365. Kullanıcılar eşitlenmeden önce Azure AD'de kullanılan etki alanları (UPN soneki olarak da bilinir) doğrulamanız gerekir.

UserPrincipalName varsayılan özniteliğinin tutulmasını öneririz. Bu öznitelik nonroutable ve doğrulanamıyor, ardından başka bir öznitelik (örneğin, email) oturum açma kimliğinin bulunduğu öznitelik seçmek mümkündür Bu alternatif kimliği da bilinir Alternatif kimlik öznitelik değeri, RFC 822 standart izlemeniz gerekir. Alternatif kimlik, hem parola SSO hem de Federasyon SSO oturum açma çözümü olarak kullanabilirsiniz.

> [!NOTE]
> Alternatif kimlik kullanarak tüm Office 365 iş yükü ile uyumlu değildir. Daha fazla bilgi için [alternatif oturum açma Kimliğini yapılandırma](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-the-azure-sign-in-experience"></a>Farklı bir özel etki alanı durumları ve etkilerini Azure oturum açma deneyimi
Azure AD dizininize özel etki alanı durumlarda arasındaki ilişkiyi anlamak çok önemlidir ve şirket içi olan UPN sonekleri tanımlanmalıdır. Farklı olası Azure oturum açma deneyimlerini Azure AD Connect kullanarak eşitleme tutunun olduğunda Bahsedelim.

Biz şirket içi dizin örneğin UPN--bir parçası olarak kullanılan UPN soneki contoso.com ile ilgili aşağıdaki bilgileri için varsayalım user@contoso.com.

###### <a name="express-settingspassword-hash-synchronization"></a>Hızlı ayarlar/parola karması eşitleme

| Durum | Azure oturum açma kullanıcı deneyimine etkisi |
|:---:|:--- |
| Ekli değil |Bu durumda, herhangi bir özel etki alanı contoso.com için Azure AD dizininde eklendi. UPN ile şirket içi son ekine sahip kullanıcılar @contoso.com şirket içi UPN Azure'da oturum açarken kullandığınız mümkün olmayacaktır. Bunlar bunun yerine varsayılan Azure AD dizini için son eki ekleyerek bunları Azure AD tarafından sağlanır yeni bir UPN kullanmanız gerekir. Örneğin, Azure AD directory azurecontoso.onmicrosoft.com kullanıcılara, sonra şirket içi kullanıcı eşitleme user@contoso.com bir UPN ile verilen user@azurecontoso.onmicrosoft.com. |
| Doğrulanmadı |Bu durumda, Azure AD dizininde eklenen bir özel etki alanı contoso.com sahibiz. Ancak, henüz doğrulanır. Etki alanı doğrulamadan kullanıcıları eşitleme ile devam edin, ardından kullanıcıları yeni UPN Azure AD tarafından olduğu gibi "Eklenmedi" senaryosunda atanır. |
| Doğrulandı |Bu durumda, zaten eklenmiş olan ve UPN soneki için Azure ad'deki doğrulanmış özel etki alanı contoso.com sahibiz. Kullanıcılar, şirket içi kullanıcı asıl adı, örneğin kullanmanız mümkün olacaktır user@contoso.com, Azure AD'ye eşitlenmiş sonra Azure'da oturum açın. |

###### <a name="ad-fs-federation"></a>AD FS federasyon
Varsayılan bir Federasyon oluşturulamıyor. onmicrosoft.com etki alanı adını Azure ad'deki veya doğrulanmamış bir özel etki alanını Azure AD'de. İle Federasyon oluşturmak için doğrulanmamış bir etki alanını seçerseniz Azure AD Connect Sihirbazı çalıştırırken, daha sonra Azure AD Connect, etki alanı için DNS'İNİZDE barındırıldığı oluşturulması gereken kayıtlarla ister. Daha fazla bilgi için [Federasyon için seçilen Azure AD etki alanını doğrulama](how-to-connect-install-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Kullanıcı oturum açma seçeneğini seçtiyseniz, **AD FS ile Federasyon**, Azure AD'de bir Federasyon oluşturmaya devam etmek için özel bir etki alanına sahip olmalıdır. Bizim tartışma için bu Azure AD dizininde eklenen bir özel etki alanı contoso.com olmalıdır anlamına gelir.

| Durum | Azure oturum açma deneyimini kullanıcı üzerinde etkisi |
|:---:|:--- |
| Ekli değil |Bu durumda, Azure AD Connect, Azure AD directory UPN soneki contoso.com için eşleşen özel etki alanı bulunamadı. Şirket içi UPN ile AD FS kullanarak oturum açmalarını gerekiyorsa, özel etki alanı contoso.com eklemeniz gerekir (gibi user@contoso.com). |
| Doğrulanmadı |Bu durumda, Azure AD Connect ile nasıl daha sonraki bir aşamada, etki alanını doğrulayabilmesi uygun ayrıntı ister. |
| Doğrulandı |Bu durumda, başka bir eylem olmadan yapılandırmasıyla devam gidebilirsiniz. |

## <a name="changing-the-user-sign-in-method"></a>Kullanıcı oturum açma yöntemini değiştirme
Azure AD Connect Sihirbazı ile ilk yapılandırmadan sonra Azure AD Connect içinde kullanılabilir olan görevler kullanarak, Federasyon, parola karması eşitleme veya doğrudan kimlik doğrulama kullanıcı oturum açma yöntemini değiştirebilirsiniz. Azure AD Connect Sihirbazı'nı yeniden çalıştırın ve gerçekleştirebileceğiniz görevler listesini görürsünüz. Seçin **değiştirme kullanıcı oturum açma** görevler listesinden.

![Kullanıcı oturumunu değiştir](./media/plan-connect-user-signin/changeusersignin.png)

Sonraki sayfada, Azure AD için kimlik bilgilerini sağlamanız istenir.

![Azure AD'ye Bağlanma](./media/plan-connect-user-signin/changeusersignin2.png)

Üzerinde **kullanıcı oturum açma** sayfasında, istenen kullanıcı oturum açma seçin.

![Azure AD'ye Bağlanma](./media/plan-connect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Parola Karması eşitleme için yalnızca geçici bir geçiş yapıyorsanız, ardından **kullanıcı hesaplarını dönüştürme** onay kutusu. Federasyona her bir kullanıcı denetimi seçeneğini değil dönüştürür ve birkaç saat sürebilir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
- Daha fazla bilgi edinin [Azure AD Connect tasarım kavramları](plan-connect-design-concepts.md).
