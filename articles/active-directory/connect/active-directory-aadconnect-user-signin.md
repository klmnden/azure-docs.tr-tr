---
title: 'Azure AD Connect: Kullanıcı oturum açma | Microsoft Docs'
description: Azure AD Connect kullanıcı oturum açma için özel ayarlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 6a6e83ad73f561cd8aa4fc629fb9b48449af6d0a
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect kullanıcı oturum açma seçenekleri
Azure Active Directory (Azure AD) Bağlan aynı parolayı kullanarak Bulut ve şirket içi kaynaklara oturum açmalarını sağlar. Bu makalede, Azure AD ile oturum açmak için kullanmak istediğiniz kimlik seçmenize yardımcı olmak her bir kimlik modeli için temel kavramları açıklar.

Zaten Azure AD kimlik modeliyle tanıdık ve belirli bir yöntemi hakkında daha fazla bilgi edinmek istiyorsanız, uygun bağlantıyı bakın:

* [Parola karma eşitlemesi](#password-hash-synchronization) ile [sorunsuz çoklu oturum açma (SSO)](active-directory-aadconnect-sso.md)
* [Doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md) ile [sorunsuz çoklu oturum açma (SSO)](active-directory-aadconnect-sso.md)
* [Federasyon SSO (ile Active Directory Federasyon Hizmetleri (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)

> [!NOTE] 
> Azure AD için Federasyon yapılandırarak, Azure AD kiracınıza ile Federasyon, etki alanı arasında güven olduğunu unutmamak önemlidir. Bu güven Federasyon etki alanı ile kullanıcılar Kiracı içinde Azure AD bulut kaynaklarına erişebilir.  
>

## <a name="choosing-the-user-sign-in-method-for-your-organization"></a>Kuruluşunuz için kullanıcı oturum açma yöntemini seçme
Kullanıcı oturum açma Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklara etkinleştirmek istediğiniz çoğu kuruluş için varsayılan parola karma eşitlemesi seçeneği öneririz. Bazı kuruluşlar, ancak, bu seçeneği kullanmanız mümkün olmayan belirli bir nedeni de sahiptir. Bunlar ya da bir federe oturum açma seçeneği, AD FS veya doğrudan kimlik doğrulama gibi seçebilirsiniz. Doğru seçim yapmanıza yardımcı olmak için aşağıdaki tabloyu kullanın.

İstiyorum | SSO PHS| SSO PTA| AD FS |
 --- | --- | --- | --- |
Yeni kullanıcı, kişi ve grup hesapları şirket içi Active Directory'de bulut için otomatik olarak eşitle.|x|x|x|
My Kiracı Office 365 karma senaryolar için ayarlayın.|x|x|x|
Oturum açma ve şirket içi parolalarını kullanarak bulut hizmetlerine erişmek Kullanıcılarım etkinleştirin.|x|x|x|
Çoklu oturum açma şirket kimlik bilgilerini kullanarak uygular.|x|x|x|
Hiçbir parolaları bulutta depolandığından emin olun.||x*|x|
Şirket içi çok faktörlü kimlik doğrulaması çözümleri etkinleştirin.|||x|

* İle basit bir aracıyı.

### <a name="password-hash-synchronization"></a>Parola karma eşitlemesi
Parola karma eşitlemesi ile kullanıcı parolalarını karmalarını şirket içi Active Directory'den Azure AD ile eşitlenir. Yeni parola karmaları parolalar değiştirilir veya şirket içi sıfırlama, Azure AD ile eşitlenir böylece hemen kullanıcılarınızın her zaman aynı parolayı bulut kaynaklarına ve şirket içi kaynaklar için kullanabilirsiniz. Parolalar hiçbir zaman Azure AD ile gönderilen veya düz metin Azure AD'de depolanabilir. Self Servis parola sıfırlama Azure AD'de etkinleştirmek için parola karma eşitlemesi parola geri yazma ile birlikte kullanabilirsiniz.

Ayrıca, etkinleştirebilirsiniz [sorunsuz SSO](active-directory-aadconnect-sso.md) makinelerde şirket ağında olan etki alanına katılmış kullanıcılar için. Çoklu oturum açma ile etkin kullanıcılar yalnızca güvenli bir şekilde erişim bulut kaynakları yardımcı olmak için bir kullanıcı adı girmeniz gerekir.

![Parola karma eşitlemesi](./media/active-directory-aadconnect-user-signin/passwordhash.png)

Daha fazla bilgi için bkz: [parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-hash-synchronization.md) makalesi.

### <a name="pass-through-authentication"></a>Geçişli kimlik doğrulaması
Doğrudan kimlik doğrulama ile şirket içi Active Directory denetleyiciye karşı kullanıcının parolasını doğrulanır. Parola, herhangi bir biçimde Azure AD'de mevcut olması gerekmez. Bu oturum açma saatleri kısıtlaması gibi şirket içi ilkeleri, bulut kimlik doğrulaması sırasında değerlendirilecek hizmetleri sağlar.

Doğrudan kimlik doğrulaması, bir Windows Server 2012 R2 etki alanına katılmış makinede şirket içi ortamda basit bir aracı kullanır. Bu aracı parola doğrulama isteklerini dinler. Internet'e açık olmasını gelen bağlantı noktalarının gerektirmez.

Ek olarak, çoklu oturum açma kurumsal ağdaki etki alanına katılmış makinede kullanıcılar için de etkinleştirebilirsiniz. Çoklu oturum açma ile etkin kullanıcılar yalnızca güvenli bir şekilde erişim bulut kaynakları yardımcı olmak için bir kullanıcı adı girmeniz gerekir.
![Doğrudan kimlik doğrulama](./media/active-directory-aadconnect-user-signin/pta.png)

Daha fazla bilgi için bkz.
- [Doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md)
- [Çoklu oturum açma](active-directory-aadconnect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS ile yeni veya varolan bir grubu kullanan Federasyon
Federasyon ile oturum açma, kullanıcılarınızın şirket içi parolalarını ile Azure AD tabanlı hizmetler için oturum açabilir. Kurumsal ağda olup olmadıklarını olsa da, bunlar bile parolalarını girmeniz gerekmez. AD FS ile Federasyon seçeneğini kullanarak, Windows Server 2012 R2'de AD FS ile yeni veya varolan bir grubu dağıtabilirsiniz. Varolan bir grubu belirtmek seçerseniz, Azure AD Connect kullanıcılarınız oturum açarak grubu ve Azure AD arasında güven yapılandırır.

<center>![Windows Server 2012 R2 AD FS ile Federasyon](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS ile Federasyon dağıtma

Yeni bir grubu dağıtıyorsanız, gerekir:

* Federasyon sunucusu için Windows Server 2012 R2 sunucusu.
* Web uygulaması proxy'si için Windows Server 2012 R2 sunucusu.
* Hedeflenen Federasyon Hizmeti adı için bir .pfx dosyası bir SSL sertifikası ile. Örneğin: fs.contoso.com.

Yeni grubu dağıtma ya da varolan bir grubu kullanarak, gerekir:

* Federasyon sunucularınız üzerinde yerel yönetici kimlik bilgileri.
* Üzerinde Web Uygulaması Proxy'si rol dağıtmak istiyorsanız yerel yönetici kimlik bilgileri tüm çalışma grubu sunucuları (olmayan etki alanına katılmış).
* Sihirbazın diğer makinelere bağlanmak kullanabilmek için çalıştırdığınız makine istediğiniz AD FS veya Web uygulaması proxy'si Windows Uzaktan Yönetimi'ni kullanarak yüklemek.

Daha fazla bilgi için bkz: [AD FS yapılandırma SSO'su](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>AD FS veya bir üçüncü taraf çözümü önceki bir sürümünü kullanarak oturum açın
Zaten bulut oturum açma (örneğin, AD FS 2.0) AD FS veya üçüncü taraf Federasyon sağlayıcısı önceki bir sürümünü kullanarak yapılandırdıysanız, kullanıcı oturum açma yapılandırması Azure AD Connect aracılığı atlamak seçebilirsiniz. Bu, oturum açma için hala var olan çözümünüzü kullanırken son eşitleme ve Azure AD Connect diğer özelliklerini almaya olanak tanır.

Daha fazla bilgi için bkz: [Azure AD üçüncü taraf Federasyon Uyumluluğu Listesi](active-directory-aadconnect-federation-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Kullanıcı oturum açma ve kullanıcı asıl adı
### <a name="understanding-user-principal-name"></a>Anlama kullanıcı asıl adı
Active Directory'de varsayılan kullanıcı asıl adı (UPN) soneki, kullanıcı hesabının oluşturulduğu etki alanının DNS adıdır. Çoğu durumda, bu Internet üzerinde kuruluş etki alanı olarak kayıtlı etki alanı adıdır. Ancak, Active Directory etki alanları ve Güvenleri'ni kullanarak daha fazla UPN soneki ekleyebilirsiniz.

Kullanıcı UPN biçimdedir username@domain. Örneğin, "contoso.com" adlı bir Active Directory etki alanı için John adlı bir kullanıcı UPN olabilir "john@contoso.com". Kullanıcı UPN RFC 822 temel alır. UPN ve e-posta aynı biçimde paylaşmak karşın, bir kullanıcı için UPN değerini olabilir veya kullanıcının e-posta adresi ile aynı olmayabilir.

### <a name="user-principal-name-in-azure-ad"></a>Azure AD'de kullanıcı asıl adı
Azure AD Connect Sihirbazı userPrincipalName özniteliği kullanır veya şirket içi kullanıcı asıl adı olarak Azure AD içinde kullanılacak öznitelikte (özel bir yükleme) belirtmenize olanak sağlar. Azure AD ile oturum açmak için kullanılan değer budur. UserPrincipalName özniteliğinin değeri Azure AD'de doğrulanmış bir etki alanına karşılık gelmiyor, ardından Azure AD ile bir varsayılan değiştirir. onmicrosoft.com değeri.

Azure Active Directory'de her dizin biçimi contoso.onmicrosoft.com, Azure veya diğer Microsoft hizmetlerini kullanmaya başlamanıza olanak tanıyan bir yerleşik etki alanı adıyla birlikte gönderilir. Geliştirmek ve özel etki alanlarını kullanarak oturum açma deneyimini basitleştirin. Özel etki alanı adlarını Azure AD'de ve bir etki alanı doğrulama hakkında daha fazla bilgi için bkz: [özel etki alanı adınızı Azure Active Directory'ye ekleme](../add-custom-domain.md#add-the-custom-domain-name-to-your-directory).

## <a name="azure-ad-sign-in-configuration"></a>Azure AD oturum açma yapılandırması
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD oturum açma yapılandırması Azure AD Connect ile
Azure AD oturum açma deneyimi, Azure AD, bir Azure AD dizininde doğrulanır özel etki alanlarının eşitlenmiş bir kullanıcının kullanıcı asıl adı soneki olup eşleştirebilirsiniz bağlıdır. Azure AD oturum açma ayarları yapılandırması sırasında azure AD Connect bulut kullanıcı oturum açma deneyimi şirket içi deneyimine benzer Yardım sağlar.

Azure AD Connect, etki alanları için tanımlanır ve bunları Azure AD'de özel bir etki alanı ile eşleştirmeye çalışır UPN soneki listeler. Ardından yapılması uygun eylemi ile yardımcı olur.
Azure AD oturum açma sayfasında, şirket içi Active Directory için tanımlanır ve karşılık gelen durum her sonek karşı görüntüleyen UPN soneki listeler. Durum değerleri aşağıdakilerden biri olabilir:

| Durum | Açıklama | Eylem gerekli |
|:--- |:--- |:--- |
| Doğrulandı |Azure AD Connect, eşleşen bir etki alanı Azure AD'de doğrulanmış bulundu. Bu etki alanı için tüm kullanıcıların şirket içi kimlik bilgilerini kullanarak oturum açabilir. |Eylem gerekmiyor. |
| Doğrulanmadı |Azure AD Connect özel eşleşen bir etki alanı Azure AD içinde bulundu, ancak doğrulandı değil. Bu etki alanı kullanıcılarının UPN soneki varsayılan olarak değiştirilecek. onmicrosoft.com sonekini etki alanını doğruladıysanız değil, eşitlemeden sonra. | [Özel etki alanı Azure AD'de doğrulayın.](../add-custom-domain.md#verify-the-custom-domain-name-in-azure-ad) |
| Eklenmedi |Azure AD Connect UPN soneki corresponded özel bir etki alanı bulunamadı. Bu etki alanı kullanıcılarının UPN soneki varsayılan olarak değiştirilecek. onmicrosoft.com sonekini etki alanı eklediyseniz değilse ve Azure'da doğrulandı. | [Ekleyin ve UPN soneki karşılık gelen özel bir etki alanını doğrulayın.](../add-custom-domain.md) |

Azure AD oturum açma sayfasında, şirket içi Active Directory ve karşılık gelen özel etki alanı için Azure AD geçerli doğrulama durumunu ile tanımlanan UPN soneki listeler. Özel bir yükleme artık kullanıcı asıl adı özniteliği üzerinde seçebilirsiniz **Azure AD oturum açma** sayfası.

![Azure AD oturum açma sayfası](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Özel etki alanlarını en son durumunu Azure AD'den yeniden getirin için Yenile düğmesini tıklatabilirsiniz.

### <a name="selecting-the-attribute-for-the-user-principal-name-in-azure-ad"></a>Azure AD'de kullanıcı asıl adı özniteliğini seçme
Öznitelik userPrincipalName kullanıcılar Azure AD'de oturum zaman kullanan özniteliktir ve Office 365. Kullanıcılar eşitlenmeden önce Azure AD içinde kullanılan etki alanları (UPN soneki olarak da bilinir) doğrulamanız gerekir.

Varsayılan özniteliği userPrincipalName tutmanızı öneririz. Bu öznitelik nonroutable ise ve doğrulanamıyor, ardından başka bir öznitelik (örneğin, eposta) oturum açma kimliğinin bulunduğu öznitelik seçmek mümkündür Bu alternatif kimliği olarak da bilinir Alternatif kimlik öznitelik değeri RFC 822 standardına uygun olmalıdır. Alternatif kimlik, hem parola SSO hem de Federasyon SSO oturum açma çözümü olarak kullanabilirsiniz.

> [!NOTE]
> Alternatif kimlik kullanımı tüm Office 365 iş yükü ile uyumlu değil. Daha fazla bilgi için bkz: [alternatif oturum açma Kimliğini yapılandırma](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-the-azure-sign-in-experience"></a>Farklı bir özel etki alanı durumları ve Azure oturum açma deneyimi etkileri
Azure AD dizininizi özel etki alanı Devletleri'nde arasındaki ilişkiyi anlamak çok önemlidir ve şirket içi olan UPN soneki tanımlanmalıdır. Farklı olası Azure oturum açma deneyimlerini Azure AD Connect kullanarak eşitleme ayarlama zaman edelim.

Biz şirket içi dizinde örneğin UPN--bir parçası olarak kullanılan UPN soneki contoso.com ile ilgili aşağıdaki bilgileri için varsayalım user@contoso.com.

###### <a name="express-settingspassword-hash-synchronization"></a>Ayarları/parola karması eşitlemesi express
| Durum | Azure oturum açma kullanıcı deneyimi üzerinde etkisi |
|:---:|:--- |
| Eklenmedi |Bu durumda, hiçbir özel etki alanı contoso.com için Azure AD dizininde eklendi. UPN şirket içi sonekine sahip olan kullanıcıların @contoso.com Azure'a oturum açmak için kendi şirket içi UPN kullanmanız mümkün olmayacaktır. Bunlar bunun yerine varsayılan Azure AD dizini için sonek ekleyerek bunları Azure AD tarafından sağlanır yeni bir UPN kullanmanız gerekir. Örneğin, Azure AD directory azurecontoso.onmicrosoft.com kullanıcılara sonra şirket içi kullanıcı eşitleniyor user@contoso.com UPN verilen user@azurecontoso.onmicrosoft.com. |
| Doğrulanmadı |Bu durumda, Azure AD dizininde eklenen bir özel etki alanı contoso.com sahip. Ancak, henüz doğrulanır. Etki alanı doğrulamadan kullanıcılar eşitleniyor ile devam edin, ardından kullanıcıları yeni UPN Azure AD tarafından olduğu gibi "Eklenmedi" senaryoda atanır. |
| Doğrulandı |Bu durumda, daha önce eklenen ve UPN soneki için Azure AD'de doğrulanmış bir özel etki alanı contoso.com sahip. Kullanıcıların şirket içi kullanıcı asıl adına, örneğin kullanmanız mümkün olacaktır user@contoso.com, Azure AD'ye eşitlenen sonra Azure'da oturum açın. |

###### <a name="ad-fs-federation"></a>AD FS federasyon
Varsayılan bir Federasyon oluşturulamıyor. onmicrosoft.com etki alanı Azure AD'de veya doğrulanmamış özel etki alanı Azure AD'de. Federasyon ile oluşturmak için doğrulanmamış bir etki alanını seçerseniz Azure AD Connect Sihirbazı'nı çalıştırırken, ardından Azure AD Connect, DNS etki alanı için barındırıldığı oluşturulacak gerekli kayıtlarla ister. Daha fazla bilgi için bkz: [Federasyon için seçili Azure AD etki alanını doğrulayın](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Oturum açma kullanıcı seçeneğini belirlediyseniz **AD FS ile Federasyon**, sonra da Azure AD'de bir Federasyon oluşturmaya devam etmek için özel bir etki alanı olması gerekir. Bizim hakkında bilgi için bu Azure AD dizininde eklenen bir özel etki alanı contoso.com olmalıdır anlamına gelir.

| Durum | Azure oturum açma deneyimi kullanıcı etkisi |
|:---:|:--- |
| Eklenmedi |Bu durumda, Azure AD Connect, Azure AD dizini UPN soneki contoso.com için eşleşen bir özel etki alanı bulunamadı. Kendi şirket içi UPN ile AD FS kullanarak oturum açmalarını gereksinim duyarsanız, özel etki alanı contoso.com eklemeniz gerekir (gibi user@contoso.com). |
| Doğrulanmadı |Bu durumda, Azure AD Connect, daha sonraki bir aşamada etki alanınızın nasıl doğrulayabilirsiniz üzerinde uygun ayrıntılarla ister. |
| Doğrulandı |Bu durumda, başka bir eylem olmadan yapılandırmasıyla devam gidebilirsiniz. |

## <a name="changing-the-user-sign-in-method"></a>Kullanıcı oturum açma yöntemini değiştirme
Azure AD Connect Sihirbazı'nı kullanarak ilk yapılandırmadan sonra Azure AD Connect içinde kullanılabilir görevler kullanarak, Federasyon, parola karması eşitlemesi ya da doğrudan kimlik doğrulama oturum açma kullanıcı yöntemi değiştirebilirsiniz. Azure AD Connect Sihirbazı'nı yeniden çalıştırın ve gerçekleştirebileceğiniz görevlerin bir listesi görürsünüz. Seçin **değiştirme kullanıcı oturum açma** görevler listesinden.

![Kullanıcı oturumunu değiştir](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Sonraki sayfada Azure AD için kimlik bilgilerini sağlamanız istenir.

![Azure AD'ye Bağlanma](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Üzerinde **kullanıcı oturum açma** sayfasında, istenen kullanıcı oturum açma seçin.

![Azure AD'ye Bağlanma](./media/active-directory-aadconnect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Parola karma eşitlemesi için yalnızca bir geçici geçiş yapıyorsanız, ardından **kullanıcı hesapları dönüştürmez** onay kutusu. Federasyon için her bir kullanıcı denetleme seçeneği değil dönüştürür ve birkaç saat sürebilir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).
- Daha fazla bilgi edinmek [Azure AD Connect tasarım kavramları](active-directory-aadconnect-design-concepts.md).
