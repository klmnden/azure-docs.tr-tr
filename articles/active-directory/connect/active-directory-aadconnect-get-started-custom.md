---
title: "Azure AD Connect: Özel yükleme | Microsoft Belgeleri"
description: "Bu belgede Azure AD Connect için özel yükleme seçenekleri ayrıntılı olarak açıklanmaktadır. Active Directory&quot;yi Azure AD Connect aracılığı ile yüklemek için bu yönergeleri kullanın."
services: active-directory
keywords: "Azure AD Connect nedir, Active Directory yükleme, Azure AD için gerekli bileşenler"
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 6d42fb79-d9cf-48da-8445-f482c4c536af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/30/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 06f81b11205085357ba4ba4e2f0d2e1e4c0e940a
ms.lasthandoff: 04/07/2017


---
# <a name="custom-installation-of-azure-ad-connect"></a>Azure AD Connect özel yüklemesi
Yükleme için daha fazla seçenek istediğinizde Azure AD Connect **Özel ayarları** kullanılır. Birden fazla ormanınız varsa veya hızlı yükleme kapsamında yer almayan isteğe bağlı özellikleri yapılandırmak istiyorsanız kullanılır. [**Hızlı yükleme**](active-directory-aadconnect-get-started-express.md) seçeneğinin dağıtımınız veya topolojiniz için uygun olmadığı tüm durumlarda kullanılır.

Azure AD Connect'i yüklemeye başlamadan önce [Azure AD Connect'i indirdiğinizden](http://go.microsoft.com/fwlink/?LinkId=615771) ve [Azure AD Connect: Donanım ve önkoşullar](active-directory-aadconnect-prerequisites.md) bölümündeki önkoşul adımlarını tamamladığınızdan emin olun. Ayrıca [Azure AD Connect hesapları ve izinleri](active-directory-aadconnect-accounts-permissions.md) bölümünde açıklandığı üzere, gerekli hesaplara sahip olduğunuzdan olduğundan emin olun .

Özel ayarlar, topolojinizle eşleşmiyorsa (örneğin, DirSync'i yükseltmek için) diğer senaryolara ilişkin [ilgili belgelere](#related-documentation) göz atın.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Azure AD Connect özel ayarlarını yükleme
### <a name="express-settings"></a>Hızlı Ayarlar
Özelleştirilmiş ayarlar yüklemeyi başlatmak için bu sayfada **Özelleştir**'e tıklayın.

### <a name="install-required-components"></a>Gerekli bileşenleri yükleme
Eşitleme hizmetlerini yüklerken isteğe bağlı yapılandırma bölümünü işaretlenmemiş olarak bırakabilirsiniz. Bu durumda, Azure AD Connect her şeyi otomatik olarak ayarlar. SQL Server 2012 Express LocalDB örneğini ayarlar, uygun gruplar oluşturur ve izinleri atar. Varsayılanları değiştirmek isterseniz var olan isteğe bağlı yapılandırma seçeneklerini anlamak üzere aşağıdaki tabloya bakabilirsiniz.

![Gerekli Bileşenler](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

| İsteğe Bağlı Yapılandırma | Açıklama |
| --- | --- |
| Mevcut bir SQL Server'ı kullanma |SQL Server adını ve örnek adını belirtebilirsiniz. Kullanmak istediğiniz bir veritabanı sunucusu zaten varsa bu seçeneği belirleyin. SQL Server'ınızda gözatma özelliği etkin değilse **Örnek Adı** alanına örnek adını girin, virgül ekleyin ve bağlantı noktası numarasını girin. |
| Mevcut bir hizmet hesabını kullanma |Varsayılan olarak Azure AD Connect, eşitleme hizmetleri tarafından kullanılmak üzere sanal bir hizmet hesabı kullanır. Kimlik doğrulaması gerektiren bir ara sunucu veya uzak bir SQL sunucusu kullanıyorsanız **yönetilen bir hizmet hesabı** kullanmanız veya etki alanında bir hizmet kullanıp parolayı biliyor olmanız gerekir. Bu gibi durumlarda kullanılacak olan hesabı girin. Hizmet hesabı için oturum açma seçeneğinin oluşturulabilmesi için, yüklemeyi çalıştıran kullanıcının SQL'de bir Sistem Yöneticisi olduğundan emin olun. Bkz. [Azure AD Connect hesapları ve izinleri](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) |
| Özel eşitleme grubu belirtme |Eşitleme hizmetleri yüklendiğinde Azure AD Connect varsayılan olarak sunucu için dört yerel grup oluşturur. Bunlar Yöneticiler grubu, İşleçler grubu, Gözatma grubu ve Parola Sıfırlama Grubudur. Kendi gruplarınızı burada belirtebilirsiniz. Gruplar sunucuda yerel olmalıdır ve etki alanında bulunamazlar. |

### <a name="user-sign-in"></a>Kullanıcı oturumu açma
Gerekli bileşenleri yükledikten sonra kullanıcı çoklu oturumu açma yönteminizi seçmeniz istenir. Aşağıdaki tabloda mevcut seçeneklerle ilgili kısa bir açıklama bulunmaktadır. Oturum açma yöntemleriyle ilgili tam açıklama için bkz. [Kullanıcı oturumu açma](active-directory-aadconnect-user-signin.md).

![Kullanıcı Oturumu açma](./media/active-directory-aadconnect-get-started-custom/usersignin2.png)

| Çoklu Oturum Açma Seçeneği | Açıklama |
| --- | --- |
| Parola Eşitleme |Kullanıcılar, Office 365 gibi Microsoft bulut hizmetlerinde kendi şirket içi ağlarında kullandıkları parolayla oturum açabilir. Kullanıcı parolaları, parola karması olarak Azure AD ile eşitlenir ve bulutta bir kimlik doğrulaması gerçekleşir. Daha fazla bilgi için bkz. [Parola eşitleme](active-directory-aadconnectsync-implement-password-synchronization.md). |
|Doğrudan kimlik doğrulama (Önizleme)|Kullanıcılar, Office 365 gibi Microsoft bulut hizmetlerinde kendi şirket içi ağlarında kullandıkları parolayla oturum açabilir.  Kullanıcının parolası, doğrulanmak üzere şirket içi Active Directory denetleyicisine geçirilir.
| AD FS ile Federasyon |Kullanıcılar, Office 365 gibi Microsoft bulut hizmetlerinde kendi şirket içi ağlarında kullandıkları parolayla oturum açabilir.  Kullanıcılar oturum açmak üzere kendi şirket içi AD FS örneklerine yönlendirilir ve şirket içi kimlik doğrulaması gerçekleşir. |
| Yapılandırmayın |Özellik yüklenmez ve yapılandırılmaz. Zaten 3. taraf bir federasyon sunucunuz varsa veya başka bir çözümden faydalanıyorsanız bu seçeneği belirleyin. |
|Çoklu Oturum Açmayı Etkinleştir|Bu seçenek hem parola eşitleme hem de Doğrudan kimlik doğrulaması ile kullanılabilir ve masaüstü kullanıcılarına kurumsal ağda çoklu oturum açma deneyimi sağlar.  Daha fazla bilgi için bkz. [Çoklu oturum açma](active-directory-aadconnect-sso.md). </br>AD FS zaten aynı düzeyde çoklu oturum açma olanağı sağladığından, AD FS müşterilerinin bu seçeneği kullanamayacağı unutulmamalıdır.</br>(Aynı zamanda PTA yayımlanmazsa)
|Oturum Açma Seçeneği|Bu seçenek parola eşitleme müşterileri tarafından kullanılabilir ve masaüstü kullanıcılarına kurumsal ağda çoklu oturum açma deneyimi sağlar.  </br>Daha fazla bilgi için bkz. [Çoklu oturum açma](active-directory-aadconnect-sso.md). </br>AD FS zaten aynı düzeyde çoklu oturum açma olanağı sağladığından, AD FS müşterilerinin bu seçeneği kullanamayacağı unutulmamalıdır.


### <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma
Azure AD'ye Bağlanma ekranında, genel yönetici hesabı ve parolasını girin. Önceki sayfada **AD FS ile Federasyon** seçeneğini belirlediyseniz etki alanında federasyon için etkinleştirmeyi düşündüğünüz bir hesap ile oturum açmayın. Azure AD dizininizle sunulan, varsayılan **onmicrosoft.com** etki alanındaki bir hesabı kullanmanız önerilir.

Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.  
![Kullanıcı Oturumu açma](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Genel yönetici hesabınızda MFA etkinse açılır oturum açma penceresine parolayı tekrar girmeniz ve MFA testini tamamlamanız gerekir. Test için bir doğrulama kodu sağlanır veya telefon görüşmesi yapılır.  
![MFA’da kullanıcı Oturumu açma](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Genel yönetici hesabında [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md) seçeneği de etkin olabilir.

Bir hatayla karşılaştıysanız ve bağlantı sorunlarınız varsa bkz. [Bağlantı sorunlarını giderme](active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Eşitleme bölümünde yer alan sayfalar

### <a name="connect-your-directories"></a>Dizinlerinizi bağlama
Azure AD Connect'in Active Directory Etki Alanı Hizmetinize bağlanabilmesi için yeterli izinlere sahip bir hesabın kimlik bilgilerine sahip olması gerekir. Etki alanı bölümünü NetBIOS veya FQDN biçiminde (ör. FABRIKAM\eşitlemekullanıcısı veya fabrikam.com\eşitlemekullanıcısı) girebilirsiniz. Yalnızca varsayılan okuma izinleri gerekli olduğundan, bu hesap normal bir kullanıcı hesabı olabilir. Ancak senaryonuza bağlı olarak daha fazla izin gerekebilir. Daha fazla bilgi için bkz. [Azure AD Connect Hesapları ve izinleri](active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Connect Dizini](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD oturum açma yapılandırması
Bu sayfa, Azure AD'de doğrulanmış olup şirket içi AD DS'de var olan UPN etki alanlarını gözden geçirmenize olanak sağlar. Ayrıca bu sayfa sayesinde userPrincipalName için kullanılacak özniteliği yapılandırabilirsiniz.

![Doğrulanmamış etki alanları](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
**Eklenmedi** ve **Doğrulanmadı** olarak işaretlenen tüm etki alanlarını gözden geçirin. Kullandığınız etki alanlarının Azure AD'de doğrulanmış olduğundan emin olun. Etki alanlarınızı doğruladıktan sonra Yenile simgesine tıklayın. Daha fazla bilgi için bkz. [etki alanı ekleme ve doğrulama](../active-directory-add-domain.md)

**UserPrincipalName** - userPrincipalName özniteliği, kullanıcıların Azure AD'de ve Office 365'te oturum açarken kullandıkları özniteliktir. Kullanıcılar eşitlenmeden önce, UPN soneki olarak da bilinen kullanılan etki alanlarının Azure AD'de doğrulanması gerekir. Microsoft, userPrincipalName varsayılan özniteliğinin tutulmasını önerir. Bu öznitelik yönlendirilemeyen bir öznitelikse ve doğrulanamazsa başka bir öznitelik seçebilirsiniz. Örneğin, oturum açma kimliğinin bulunduğu öznitelik olarak e-postayı seçin. userPrincipalName dışında başka bir özniteliğin kullanılmasına **Alternatif kimlik** adı verilir. Alternatif kimlik öznitelik değeri, RFC822 standardına uygun olmalıdır. Alternatif kimlik, hem parola eşitleme ile hem de federasyon ile kullanılabilir.

>[!NOTE]
> Doğrudan Kimlik Doğrulama’yı etkinleştirdiğinizde, sihirbazda devam edebilmeniz için en az bir doğrulanmış etki alanına sahip olmanız gerekir.

> [!WARNING]
> Alternatif kimlik kullanımı, hiçbir Office 365 iş yükü ile uyumlu değildir. Daha fazla bilgi için [Alternatif Oturum Açma Kimliğini Yapılandırma](https://technet.microsoft.com/library/dn659436.aspx) bölümüne bakın.
>
>

### <a name="domain-and-ou-filtering"></a>Etki alanı ve OU filtreleme
Varsayılan olarak tüm etki alanları ve OU'lar eşitlenir. Azure AD ile eşitlemek istemediğiniz etki alanları veya OU'lar varsa bu etki alanlarının veya OU'ların işaretini kaldırabilirsiniz.  
![DomainOU filtreleme](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png)  
Sihirbazın bu sayfası, etki alanı tabanlı ve OU tabanlı filtrelemeyi yapılandırmaya yöneliktir. Değişiklik yapmayı planlıyorsanız, yapmadan önce [etki alanı tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) ve [OU tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) konularını inceleyin. Bazı OU’lar işlevsellik açısından gereklidir ve bunların seçimi kaldırılmamalıdır.

OU tabanlı filtreleme kullanırsanız, daha sonra eklenen yeni OU'lar varsayılan olarak eşitlenir. Yeni OU'ların eşitlenmesini istemezseniz, sihirbazın [ou tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) yapılandırması tamamlandıktan sonra gerekli ayarları yapabilirsiniz.

[Grup tabanlı filtreleme](#sync-filtering-based-on-groups) kullanmayı planlıyorsanız, gruba sahip OU'nun dahil olduğundan ve OU filtreleme ile filtrelenmediğinden emin olun. OU filtreleme, grup tabanlı filtrelemeden önce değerlendirilir.

Güvenlik duvarı kısıtlamaları nedeniyle bazı etki alanlarına erişilemeyebilir. Bu etki alanları varsayılan olarak seçili değildir ve uyarı içerir.  
![Erişilemeyen etki alanları](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Bu uyarıyı görürseniz bu etki alanlarına gerçekten erişilemediğinden ve bir uyarının beklendiğinden emin olun.

### <a name="uniquely-identifying-your-users"></a>Kullanıcılarınızı benzersiz olarak tanımlama
Ormanlar arasında eşleştirme özelliği sayesinde, AD DS ormanlarındaki kullanıcıların Azure AD'de nasıl temsil edildiğini tanımlayabilirsiniz. Bir kullanıcı ya tüm ormanlarda yalnızca bir kez temsil edilebilir ya da etkin ve devre dışı hesapların birleşimine sahip olabilir. Ayrıca kullanıcı, bazı ormanlarda kişi olarak da temsil edilebilir.

![Benzersiz](./media/active-directory-aadconnect-get-started-custom/unique.png)

| Ayar | Açıklama |
| --- | --- |
| [Kullanıcılar tüm ormanlarda yalnızca bir kez temsil edilir](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Tüm kullanıcılar Azure AD'de bireysel nesne olarak oluşturulur. Nesneler meta veri deposunda birleştirilmez. |
| [Posta özniteliği](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Bu seçenek, posta özniteliğinin farklı ormanlarda aynı değere sahip olması halinde kullanıcıları ve kişileri birleştirir. Kişileriniz GALSync kullanılarak oluşturulduysa bu seçeneği kullanın. |
| [ObjectSID ve msExchangeMasterAccountSID/ msRTCSIP-OriginatorSid](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Bu seçenek, hesap ormanındaki etkin bir kullanıcıyla kaynak ormandaki devre dışı bırakılmış bir kullanıcıyı birleştirir. Bu yapılandırma, Exchange'de bağlı posta kutusu olarak bilinir. Yalnızca Lync'i kullanıyor olmanız ve kaynak ormanda Exchange olmaması halinde de bu seçeneği kullanabilirsiniz. |
| sAMAccountName ve MailNickName |Bu seçenek, kullanıcı için oturum açma kimliğinin bulunması beklenen öznitelikleri birleştirir. |
| Belirli bir öznitelik |Bu seçenek, kendi özniteliğinizi seçmenize olanak tanır. **Sınırlama:** Meta veri deposunda bulabileceğiniz bir özniteliği seçtiğinizden emin olun. Özel bir öznitelik (meta veri deposunda olmayan) seçerseniz sihirbaz tamamlanamaz. |

**Kaynak Bağlantısı** - sourceAnchor özniteliği, kullanıcı nesnesinin yaşam süresi boyunca sabit olan bir özniteliktir. Şirket içi kullanıcıyı Azure AD'deki kullanıcıya bağlayan birincil anahtardır. Öznitelik değiştirilemeyeceği için kullanmak üzere iyi bir öznitelik seçmeniz gerekir. ObjectGUID iyi bir seçenektir. Kullanıcı hesabı ormanlar/etki alanları arasında taşınmadığı sürece bu öznitelik değiştirilemez. Hesapları ormanlar arasında taşıdığınız çoklu orman ortamında başka bir öznitelik (örneğin, employeeID içeren bir öznitelik) kullanmanız gerekir. Bir kişi evlendiğinde değişecek olan veya atamaları değiştirecek olan öznitelikleri kullanmaktan kaçının. @-sign içeren nitelikleri kullanamazsınız. Bu nedenle e-posta ve userPrincipalName seçeneği kullanılamaz. Ayrıca öznitelikler büyük küçük harfe duyarlıdır. Bu nedenle bir nesneyi ormanlar arasında taşıdığınızda, büyük/küçük harfleri doğru yazdığınızdan emin olun. İkili öznitelikler base64 kodludur ancak diğer öznitelik türleri kodlanmamış durumda kalır. Federasyon senaryolarında ve bazı Azure AD arabirimlerinde, bu öznitelik immutableID özniteliği olarak da bilinir. Kaynak bağlantısı hakkında daha fazla bilgi için bkz. [tasarım kavramları](active-directory-aadconnect-design-concepts.md#sourceanchor).

### <a name="sync-filtering-based-on-groups"></a>Grup tabanlı eşitleme filtrelemesi
Grup filtreleme özelliği, pilot için yalnızca küçük bir nesne alt kümesini eşitlemenize olanak sağlar. Bu özelliği kullanmak için şirket içi Active Directory'nizde bu amaca uygun bir grup oluşturun. Ardından, doğrudan üye olarak Azure AD ile eşitlenecek kullanıcıları ve grupları ekleyin. Daha sonra, Azure AD'de mevcut olması gereken nesnelerin listesini korumak için bu gruba kullanıcı ekleyebilir ve gruptan kullanıcı çıkarabilirsiniz. Eşitlemek istediğiniz tüm nesneler grubun doğrudan üyesi olmalıdır. Tüm kullanıcılar, gruplar, kişiler ve bilgisayarlar/cihazlar doğrudan üye olmalıdır. İç içe geçmiş grup üyelikleri çözümlenmez. Bir grubu üye olarak eklediğinizde, yalnızca grubun kendisi eklenir; üyeleri eklenmez.

![Eşitleme Filtreleme](./media/active-directory-aadconnect-get-started-custom/filter2.png)

> [!WARNING]
> Bu özellik yalnızca pilot dağıtımı desteklemek üzere tasarlanmıştır. Bu özelliği tam gelişmiş üretim dağıtımında kullanmayın.
>
>

Tam gelişmiş üretim dağıtımında, tüm nesneleri eşitlenecek olan tek bir grubu kullanmak zordur. Bunun yerine, [Filtreleme yapılandırma](active-directory-aadconnectsync-configure-filtering.md) bölümünde belirtilen yöntemlerden birini kullanın.

### <a name="optional-features"></a>İsteğe Bağlı Özellikler
Bu ekran, belirli senaryolarınız için isteğe bağlı özellikler seçmenizi sağlar.

![İsteğe bağlı özellikler](./media/active-directory-aadconnect-get-started-custom/optional.png)

> [!WARNING]
> Şu anda DirSync veya Azure AD Eşitleme etkinse Azure AD Connect'te geri yazma özelliklerinden herhangi birini etkinleştirmeyin.
>
>

| İsteğe Bağlı Özellikler | Açıklama |
| --- | --- |
| Exchange Karma Dağıtımı |Exchange Karma Dağıtımı özelliği, Exchange posta kutularının hem şirket içinde hem de Office 365'te aynı anda var olmalarına olanak sağlar. Azure AD Connect, Azure AD'den belirli bir [öznitelikler](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) kümesini şirket içi dizininize geri eşitler. |
| Azure AD uygulaması ve öznitelik filtreleme |Azure AD uygulaması ve öznitelik filtreleme etkinleştirilerek, eşitlenen öznitelikler kümesi uyarlanabilir. Bu seçenek sihirbaza iki yapılandırma sayfası daha ekler. Daha fazla bilgi için bkz. [Azure AD uygulaması ve öznitelik filtreleme](#azure-ad-app-and-attribute-filtering). |
| Parola eşitleme |Oturum açma çözümü olarak federasyonu seçtiyseniz bu seçeneği etkinleştirebilirsiniz. Bu durumda parola eşitleme, bir yedekleme seçeneği olarak kullanılabilir. Ek bilgi için bkz. [Parola eşitleme](active-directory-aadconnectsync-implement-password-synchronization.md). </br></br>Doğrudan Kimlik Doğrulama’yı seçtiyseniz, eski istemcilere yönelik destek sağlanması ve bir yedek seçenek olarak kullanılması için bu seçenek otomatik olarak etkinleştirilir. Ek bilgi için bkz. [Parola eşitleme](active-directory-aadconnectsync-implement-password-synchronization.md).|
| Parola geri yazma |Parola geri yazma etkinleştirildiğinde Azure AD'de gerçekleşen parola değişiklikleri şirket içi dizininize geri yazılır. Daha fazla bilgi için bkz. [Parola yönetimine başlarken](../active-directory-passwords-getting-started.md). |
| Grup geri yazma |**Office 365 Grupları** özelliğini kullanıyorsanız bu gruplar şirket içi Active Directory'nizde de temsil edilir. Bu seçenek yalnızca şirket içi Active Directory'nizde Exchange varsa kullanılır. Daha fazla bilgi için bkz. [Grup geri yazma](active-directory-aadconnect-feature-preview.md#group-writeback). |
| Cihaz geri yazma |Koşullu erişim senaryoları için, Azure AD'deki cihaz nesnelerini şirket içi Active Directory'nize geri yazmanızı sağlar. Daha fazla bilgi için bkz. [Azure AD Connect'te cihaz geri yazma özelliğini etkinleştirme](active-directory-aadconnect-feature-device-writeback.md). |
| Dizin genişletme öznitelik eşitlemesi |Dizin genişletme öznitelik eşitlemesi etkinleştirildiğinde, belirtilen öznitelikler Azure AD ile eşitlenir. Daha fazla bilgi için bkz. [Dizin genişletmeleri](active-directory-aadconnectsync-feature-directory-extensions.md). |

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD uygulaması ve öznitelik filtreleme
Hangi özniteliklerin Azure AD ile eşitleneceğini sınırlamak istiyorsanız kullanmakta olduğunuz hizmetleri seçerek başlatın. Bu sayfada yapılandırma değişikliği yaparsanız yükleme sihirbazını yeniden çalıştırarak doğrudan yeni bir hizmet seçmeniz gerekir.

![İsteğe bağlı özellikler Uygulamalar](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Bu sayfa, önceki adımda seçtiğiniz hizmetlere bağlı olarak eşitlenen tüm öznitelikleri gösterir. Bu liste, eşitlenmekte olan tüm nesne türlerinin birleşimidir. Eşitlememeniz gereken bazı öznitelikler varsa bunların seçimlerini kaldırın.

![İsteğe bağlı özellikler Öznitelikler](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

> [!WARNING]
> Özniteliklerin kaldırılması, işlevselliği etkileyebilir. En iyi yöntemler ve öneriler için bkz. [eşitlenen öznitelikler](active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).
>
>

### <a name="directory-extension-attribute-sync"></a>Dizin Genişletme öznitelik eşitlemesi
Kuruluşunuz tarafından eklenen özel öznitelikler veya Active Directory'deki diğer özniteliklerle Azure AD'deki şemayı genişletebilirsiniz. Bu özelliği kullanmak için **İsteğe Bağlı Özellikler** sayfasındaki **Dizin Genişletme öznitelik eşitlemesi** öğesini seçin. Bu sayfada eşitlemek için daha fazla öznitelik seçebilirsiniz.

![Dizin genişletmeleri](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Daha fazla bilgi için bkz. [Dizin genişletmeleri](active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="enabling-single-sign-on-sso"></a>Çoklu oturum açmayı (SSO) etkinleştirme
Parola Eşitleme veya Doğrudan kimlik doğrulama ile birlikte kullanılmak üzere çoklu oturum açma özelliğinin yapılandırılması, Azure AD ile eşitlenen her ormanda bir kere tamamlamanız gereken basit bir işlemdir. Yapılandırma aşağıdaki iki adımdan oluşur:

1.    Şirket içi Active Directory'nizde gerekli bilgisayar hesabını oluşturma.
2.    İstemci makinelerin intranet bölgesini çoklu oturum açmayı destekleyecek şekilde yapılandırma.

#### <a name="create-the-computer-account-in-active-directory"></a>Active Directory'de bilgisayar hesabını oluşturma
Azure AD Connect'e bağlanan tüm ormanlarda bilgisayar hesabının oluşturulabilmesi için, her bir ormanda Etki Alanı Yöneticisi kimlik bilgilerini sağlamanız gerekir. Kimlik bilgileri yalnızca hesabı oluşturmak için kullanılır ve depolanmaz ya da başka bir işlem için kullanılmaz. Kimlik bilgisini Azure AD Connect sihirbazının **Çoklu oturum açmayı etkinleştirme** sayfasına eklemeniz gereklidir:

![Çoklu oturum açmayı etkinleştirme](./media/active-directory-aadconnect-get-started-custom/enablesso.png)

>[!NOTE]
>Belirli bir ormanda Çoklu oturum açma kullanmak istemiyorsanız o ormanı atlayabilirsiniz.

#### <a name="configure-the-intranet-zone-for-client-machines"></a>İstemci makineler için Intranet Bölgesini yapılandırma
İstemcinin intranet bölgesinde otomatik olarak oturum açabilmesini sağlamak için URL'lerin intranet bölgesinin bir parçası olduğundan emin olmanız gerekir. Bunun yapılması, etki alanına katılan bilgisayarın kurumsal ağa bağlandığında Azure AD'ye otomatik olarak bir Kerberos anahtarı göndermesini sağlar.
Grup İlkesi yönetim araçlarına sahip bir bilgisayarda.

1.    Grup İlkesi Yönetimi araçlarını açın
2.    Tüm kullanıcılara uygulanacak Grup ilkesini düzenleyin. Örneğin, Varsayılan Etki Alanı İlkesi.
3.    **User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** bölümüne gidin ve aşağıdaki şekilde gösterildiği gibi **Siteden Bölgeye Atama Listesi**'ni seçin.
4.    İlkeyi etkinleştirin ve iletişim kutusuna aşağıdaki iki öğeyi girin.

        Değer:`https://autologon.microsoftazuread-sso.com`  
        Veriler: 1  
        Değer:`https://aadg.windows.net.nsatc.net`  
        Veriler: 1

5.    Şunun gibi görünmelidir:  
![Intranet Bölgeleri](./media/active-directory-aadconnect-get-started-custom/sitezone.png)

6.    İki kez **Tamam**'a tıklayın.

## <a name="configuring-federation-with-ad-fs"></a>AD FS ile federasyonu yapılandırma
Azure AD Connect ile AD FS'yi yalnızca birkaç tıklama ile kolayca yapılandırabilirsiniz. Yapılandırma için aşağıdakiler gereklidir.

* Federasyon sunucusu için uzaktan yönetimi etkinleştirilmiş bir Windows Server 2012 R2 sunucusu
* Web Uygulaması Ara Sunucusu için uzaktan yönetimi etkinleştirilmiş bir Windows Server 2012 R2 sunucusu
* Kullanmayı düşündüğünüz federasyon hizmeti adı (örneğin, sts.contoso.com) için bir SSL sertifikası

### <a name="ad-fs-configuration-pre-requisites"></a>AD FS yapılandırması önkoşulları
Azure AD Connect'i kullanarak AD FS grubunuzu yapılandırmak için uzak sunucularda WinRM'nin etkinleştirildiğinden emin olun. Ayrıca, [Tablo 3 - Azure AD Connect ve Federasyon Sunucuları/WAP](active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-ad-fs-federation-serverswap) bölümünde listelenen bağlantı noktaları gereksinimlerini inceleyin.

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Yeni bir AD FS grubu oluşturma veya var olan bir AD FS grubunu kullanma
Var olan bir AD FS grubunu kullanabilir veya yeni bir AD FS grubu oluşturmayı seçebilirsiniz. Yeni bir grup oluşturmayı seçerseniz SSL sertifikası sağlamanız gerekir. SSL sertifikası bir parolayla korunuyorsa parolayı girmeniz istenir.

![AD FS Grubu](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Var olan bir AD FS grubunu kullanmayı seçerseniz doğrudan AD FS ile Azure AD arasındaki güven ilişkisini yapılandırma ekranına gidersiniz.

### <a name="specify-the-ad-fs-servers"></a>AD FS sunucularını belirtme
AD FS'yi yüklemek istediğiniz sunucuları girin. Kapasite planlama gereksinimlerinize göre bir veya daha fazla sunucu ekleyebilirsiniz. Bu yapılandırmayı gerçekleştirmeden önce tüm sunucuların Active Directory'ye katılmasını sağlayın. Microsoft, test ve pilot dağıtımlar için tek bir AD FS sunucusunun yüklenmesini önerir. Ardından, ölçeklendirme gereksinimlerinizi karşılamak için ilk yapılandırmadan sonra Azure AD Connect'i tekrar çalıştırarak daha fazla sunucu ekleyebilir ve dağıtabilirsiniz.

> [!NOTE]
> Bu yapılandırmayı geçekleştirmeden önce tüm sunucularınızın bir AD etki alanına katıldığından emin olun.
>
>

![AD FS Sunucuları](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Web Uygulaması Ara Sunucularını belirtme
Web Uygulaması ara sunucusu olarak kullanmak istediğiniz sunucuları girin. Web uygulaması ara sunucusu, çevre ağınızda (extranete yönelik) dağıtılır ve extranetten gelen kimlik doğrulama isteklerini destekler. Kapasite planlama gereksinimlerinize göre bir veya daha fazla sunucu ekleyebilirsiniz. Microsoft, test ve pilot dağıtımlar için tek bir Web uygulaması ara sunucusunun yüklenmesini önerir. Ardından, ölçeklendirme gereksinimlerinizi karşılamak için ilk yapılandırmadan sonra Azure AD Connect'i tekrar çalıştırarak daha fazla sunucu ekleyebilir ve dağıtabilirsiniz. İntranetten gelen kimlik doğrulamalarını gerçekleştirebilmek için, aynı sayıda ara sunucuya sahip olmanızı öneririz.

> [!NOTE]
> <li> Kullandığınız hesap AD FS sunucularında yerel yönetici değilse yönetici kimlik bilgileri girmeniz istenir.</li>
> <li> Bu adımı gerçekleştirmeden önce Azure AD Connect sunucusu ve Web Uygulaması Ara Sunucusu arasında HTTP/HTTPS bağlantısının olduğundan emin olun.</li>
> <li> Doğrulama isteklerinin akışına izin vermek için, Web Uygulaması Sunucusu ve AD FS sunucusu arasında HTTP/HTTPS bağlantısının olduğundan emin olun.</li>
>

![Web Uygulaması](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Web uygulama sunucusunun AD FS sunucusu ile güvenli bir bağlantı kurması için kimlik bilgileri girmeniz istenir. Bu kimlik bilgilerinin AD FS sunucusunda yerel bir yöneticiye ait olması gerekir.

![Ara sunucu](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>AD FS hizmetine ilişkin hizmet hesabını belirtme
AD FS hizmetinin kullanıcıların kimliklerini doğrulayabilmesi ve Active Directory'de kullanıcı bilgilerini arayabilmesi için bir etki alanı hizmet hesabı gerekir. AD FS hizmeti, iki hizmet hesabı türünü destekler:

* **Grup Tarafından Yönetilen Hizmet Hesabı** - Windows Server 2012'de Active Directory Etki Alanı Hizmetleri ile birlikte kullanıma sunuldu. Bu hesap türü, AD FS (hesap parolasının düzenli olarak güncelleştirilmesi gerekmeyen tek bir hesap) gibi hizmetler sağlar  AD FS sunucularınızın ait olduğu etki alanında Windows Server 2012 etki alanı denetleyicileriniz varsa bu seçeneği kullanın.
* **Etki Alanı Kullanıcı Hesabı** - Bu hesap türü için bir parola sağlamanız ve parola değiştiğinde veya süresi dolduğunda parolayı düzenli olarak güncelleştirmeniz gerekir. AD FS sunucularınızın ait olduğu etki alanında Windows Server 2012 etki alanı denetleyicileriniz yoksa bu seçeneği kullanın.

Grup Tarafından Yönetilen Hizmet Hesabı'nı seçtiyseniz ve bu özellik Active Directory'de hiç kullanılmadıysa Kuruluş Yöneticisi kimlik bilgilerini girmeniz istenir. Bu kimlik bilgileri, anahtar deposunu başlatmak ve Active Directory'de ilgili özelliği etkinleştirmek için kullanılır.

![AD FS Hizmet Hesabı](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Birleştirmek istediğiniz Azure AD etki alanını seçin
Bu yapılandırma, AD FS ile Azure AD arasındaki federasyon ilişkisini ayarlamak için kullanılır. AD FS'yi, Azure AD'ye güvenlik belirteçleri sağlamak üzere yapılandırır. Ayrıca Azure AD'yi bu belirli AD FS örneğinden gelen belirteçlere güvenecek şekilde yapılandırır. Bu sayfa, ilk yüklemede yalnızca bir etki alanını yapılandırmanıza izin verir. Daha sonra Azure AD Connect'i tekrar çalıştırarak daha fazla etki alanını yapılandırabilirsiniz.

![Azure AD Etki Alanı](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Federasyon için seçilen Azure AD etki alanını doğrulama
Birleştirilecek etki alanını seçtiğinizde Azure AD Connect, size doğrulanmamış bir etki alanını doğrulamak için gerekli olan bilgileri sağlar. Bu bilgileri nasıl kullanacağınız hakkında bilgi edinmek için bkz. [Etki alanı ekleme ve doğrulama](../active-directory-add-domain.md).

![Azure AD Etki Alanı](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

> [!NOTE]
> AD Connect, etki alanını yapılandırma aşamasında doğrulamaya çalışır. Gerekli DNS kayıtlarını eklemeden yapılandırmaya devam ederseniz sihirbaz, yapılandırmayı tamamlayamaz.
>
>

## <a name="configure-and-verify-pages"></a>Yapılandırma ve doğrulama sayfaları
Yapılandırma bu sayfada gerçekleşir.

> [!NOTE]
> Federasyonu yapılandırdıysanız ve yüklemeye devam etmek istiyorsanız [Federasyon sunucuları için ad çözümlemesi](active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers) yapılandırmasını tamamladığınızdan emin olun.
>
>

![Yapılandırma için hazır](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Hazırlama modu
Hazırlama modu ile paralel olarak yeni bir eşitleme sunucusu ayarlanabilir. Bir eşitleme sunucusunun buluttaki yalnızca bir dizine aktarım gerçekleştirmesi desteklenir. Ancak başka bir sunucudan öğe taşımak istiyorsanız (örneğin, çalışan bir DirSync'ten) Azure AD Connect'i hazırlama modunda etkinleştirebilirsiniz. Eşitleme altyapısı, etkinleştirildiğinde verileri normal olarak içeri aktarır ve eşitler ancak Azure AD'ye veya AD'ye aktarım gerçekleştirmez. Parola eşitleme ve parola geri yazma özellikleri hazırlama modunda devre dışı bırakılır.

![Hazırlama modu](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Hazırlama modundayken, eşitleme altyapısında gerekli değişiklikleri yapabilir ve dışarı aktarılacak öğeleri gözden geçirebilirsiniz. Yapılandırmayla ilgili bir sorun yoksa yükleme sihirbazını tekrar çalıştırın ve hazırlama modunu devre dışı bırakın. Veriler artık Azure AD'ye bu sunucudan aktarılır. Yalnızca bir sunucunun etkin şekilde dışarı aktarma işlemi gerçekleştirmesini sağlamak için, diğer sunucuyu devre dışı bıraktığınızdan emin olun.

Daha fazla bilgi için bkz. [Hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Federasyon yapılandırmanızı doğrulama
Doğrula düğmesine tıkladığınızda Azure AD Connect sizin için DNS ayarlarını doğrular.

![Tamamlama](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Doğrulama](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Ayrıca şu doğrulama adımlarını uygulayın:

* İntranet üzerinde etki alanına katılmış bir makinedeki tarayıcıdan oturum açabildiğinizi doğrulayın: https://myapps.microsoft.com adresine bağlanın ve oturum açtığınız hesabınız ile oturum açma işlemini doğrulayın. Yerleşik AD DS yönetici hesabı eşitlenmez ve doğrulama için kullanılamaz.
* Extranet üzerinde bir cihazdan oturum açabildiğinizi doğrulayın. Ana makineden veya bir mobil cihazdan https://myapps.microsoft.com adresine bağlanın ve kimlik bilgilerinizi girin.
* Zengin istemci oturumu açma işlemini doğrulayın. https://testconnectivity.microsoft.com adresine bağlanın, **Office 365** sekmesini ve ardından **Office 365 Çoklu Oturum Açma Testi** seçeneğini belirleyin seçin.

## <a name="next-steps"></a>Sonraki adımlar
Yükleme tamamlandıktan sonra Synchronization Service Manager'ı veya Synchronization Rule Editor'ı kullanmadan önce Windows oturumunuzu kapatıp tekrar açın.

Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](active-directory-aadconnect-whats-next.md).

Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](active-directory-aadconnectsync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.

