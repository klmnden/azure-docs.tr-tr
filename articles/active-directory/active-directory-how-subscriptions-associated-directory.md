---
title: "Azure aboneliklerinin Azure Active Directory ile ilişkisi | Microsoft Belgeleri"
description: "Microsoft Azure&quot;da oturum açma ve Azure aboneliğinin Azure Active Directory ile ilişkisi gibi ilgili konular."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 73d58df13d3265312b371a02e12fbb9342fb3980


---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Azure aboneliklerinin Azure Active Directory ile ilişkisi
Bu makalede, Microsoft Azure'da oturum açma ve Azure aboneliğinin Azure Active Directory (Azure AD) ile ilişkisi gibi ilgili konulara yönelik bilgiler ele alınmaktadır.

## <a name="accounts-that-you-can-use-to-sign-in"></a>Oturum açmak için kullanabileceğiniz hesaplar
Oturum açmak için kullanabileceğiniz hesaplarla başlayabiliriz. İki tür mevcuttur: bir Microsoft hesabı (eski adıyla Microsoft Live ID) ve Azure AD'de depolanan bir hesap olarak bir iş veya okul hesabı.

| Microsoft hesabı | Azure AD hesabı |
| --- | --- |
| Microsoft tarafından çalıştırılan tüketici kimlik sistemi |Microsoft tarafından çalıştırılan işletme kimlik sistemi |
| Hotmail ve MSN gibi tüketiciye yönelik hizmetler için kimlik doğrulaması |Office 365 gibi işletmelere yönelik hizmetler için kimlik doğrulaması |
| Tüketiciler, e-posta kaydı gibi işlemlerle kendi Microsoft hesaplarını oluşturur |Şirketler ve kuruluşlar kendi iş veya okul hesaplarını oluşturur ve yönetir |
| Kimlikler Microsoft hesap sisteminde oluşturulur ve depolanır |Kimlikler Azure kullanılarak veya Office 365 gibi başka bir hizmet kullanılarak oluşturulur ve kuruluşa atanan Azure AD örneğinde depolanır |

Azure başlangıçta yalnızca Microsoft hesabı kullanıcıları tarafından erişime izin verse de artık *her iki* sistemin kullanıcıları tarafından erişime izin vermektedir. Bu, tüm Azure özelliklerinin kimlik doğrulaması için Azure AD'ye güvenmesi sağlanarak, Azure AD'nin kuruluş kullanıcılarının kimliğini doğrulaması sağlanarak ve Azure AD'nin tüketici kullanıcıların kimliğini doğrulamak için Microsoft hesabı tüketici kimliği sistemine güvendiği bir federasyon ilişkisi oluşturularak gerçekleştirilmiştir. Sonuç olarak Azure AD, "yerel" Azure AD hesaplarının yanı sıra, "konuk" Microsoft hesapları için kimlik doğrulaması yapabilmektedir.

Örneğin, burada, Microsoft hesabı olan bir kullanıcı klasik Azure portalında oturum açmaktadır.

> [!NOTE]
> msmith@hotmail.com, klasik Azure portalında oturum açmak için Azure aboneliğine sahip olmalıdır. Hesap bir Hizmet yöneticisi veya aboneliğin ortak yöneticisi olmalıdır.
> 
> 

![][1]

Bu Hotmail adresi bir tüketici hesabı olduğundan, oturum açma işleminin kimlik doğrulaması Microsoft hesabı tüketici kimlik sistemi tarafından gerçekleştirilir. Azure AD kimlik sistemi, Microsoft hesabı sistemi tarafından gerçekleştirilen kimlik doğrulamasına güvenir ve Azure hizmetlerine erişmek için bir belirteç verir.

## <a name="how-an-azure-subscription-is-related-to-azure-ad"></a>Azure aboneliğinin Azure AD ile ilişkisi
Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Ayarlar sekmesi altında aboneliğinizin hangi dizine güvendiğini görebilirsiniz. [Abonelik ayarlarını düzenleyerek](active-directory-understanding-resource-access.md) güvenilen dizini değiştirebilirsiniz.

Aboneliğin bir dizinle arasındaki bu güven ilişkisi, bir aboneliğin daha çok abonelik alt kaynakları gibi olan, Azure'daki tüm diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili bu diğer kaynaklara erişim de durdurulur. Ancak dizin Azure içinde kalır, siz de başka bir aboneliği bu dizinle ilişkilendirebilir, dizin kullanıcılarını yönetmeye devam edebilirsiniz.

Benzer şekilde, aboneliğinizde gördüğünüz Azure AD uzantısı, klasik Azure portalındaki diğer uzantılar gibi çalışmaz. Klasik Azure portalındaki diğer uzantılar, Azure aboneliği kapsamlıdır. Azure AD uzantısında gördükleriniz, aboneliğe bağlı olarak değişiklik göstermez; bu yalnızca, oturum açan kullanıcıyı temel alarak dizinleri gösterir.

Tüm kullanıcılar kimliklerini doğrulayan tek giriş dizinine sahiptir ancak diğer dizinlerde konuk da olabilmektedir. Azure AD uzantısında, kullanıcı hesabınızın üye olduğu her dizini görürsünüz. Hesabınızın, üyesi olmadığı herhangi bir dizin görünmez. Bir dizin, (Azure AD, Microsoft hesap sistemi ile birleştirilmiş olduğundan) Azure AD içinde iş veya okul hesapları ya da Microsoft hesabı kullanıcıları için belirteçler sunabilir.

Bu diyagramda, Contoso'ya yönelik bir iş hesabı kullanarak kaydolan Michael Smith için bir abonelik gösterilmektedir.

![][2]

## <a name="how-to-manage-a-subscription-and-a-directory"></a>Aboneliği ve dizini yönetme
Bir Azure aboneliğine yönelik yönetim rolleri, Azure aboneliği ile bağlantılı kaynakları yönetir. Aboneliğinizi yönetmeye yönelik bu roller ve en iyi uygulamalar, [Azure Active Directory'de yönetici rolü atama](active-directory-assign-admin-roles.md) bölümünde ele alınmıştır.

Varsayılan olarak, kaydolduğunuzda size Hizmet Yöneticisi rolü atanır. Başkalarının aynı aboneliği kullanarak oturum açması ve hizmetlere erişmesi gerekiyorsa bu kişileri ortak yönetici olarak ekleyebilirsiniz. Hizmet Yöneticisi ve ortak yöneticiler, Microsoft hesapları veya Azure aboneliğinin ilişkili olduğu dizine ait iş ya da okul hesapları olabilir.

Azure AD, dizin ve kimlikle ilgili özelliklerin yönetilmesine ilişkin farklı bir yönetim rolleri dizisine sahiptir. Örneğin, bir dizinin genel yöneticisi dizine kullanıcı ve grup ekleyebilir veya kullanıcılar için çok faktörlü kimlik doğrulamasını gerekli kılabilir. Dizin oluşturan bir kullanıcı, genel yönetici rolüne atanır ve diğer kullanıcılara yönetici rolleri atayabilir.

Abonelik yöneticilerinde olduğu gibi, Azure AD yönetim rolleri, Microsoft hesapları veya iş veya okul hesapları olabilir. Azure AD yönetim rolleri aynı zamanda Office 365 ve Microsoft Intune gibi diğer hizmetler tarafından da kullanılır. Daha fazla bilgi için bkz. [Yönetici rolü atama](active-directory-assign-admin-roles.md).

Ancak burada önemli olan nokta, Azure aboneliği yöneticilerinin ve Azure AD dizini yöneticilerinin iki farklı kavram olmasıdır. Azure aboneliği yöneticileri, Azure'daki kaynakları yönetebilir ve (klasik Azure portalı bir Azure kaynağı olduğundan) klasik Azure portalında Active Directory uzantısını görüntüleyebilir. Dizin yöneticileri dizindeki özellikleri yönetebilir.

Bir kişi her iki rolde de olabilir ancak bu gerekli değildir. Bir kullanıcı, dizin genel yöneticisi rolüne atanabilir ancak Hizmet yöneticisi veya bir Azure aboneliğinin ortak yöneticisi olarak atanamaz. Bu kullanıcı, aboneliğin yöneticisi olmadan, klasik Azure portalında oturum açamaz. Ancak kullanıcı, Azure AD PowerShell veya Office 365 Yönetici Merkezi gibi diğer araçları kullanarak dizin yönetim görevleri gerçekleştirebilir.

## <a name="why-cant-i-manage-the-directory-with-my-current-user-account"></a>Neden dizini geçerli kullanıcı hesabımla yönetemiyorum?
Bazen bir kullanıcı, Azure aboneliğine kaydolmadan önce iş veya okul hesabını kullanarak klasik Azure portalında oturum açmayı deneyebilir. Bu durumda, kullanıcı bu hesap için abonelik olmadığını belirten bir ileti alır. İletide, ücretsiz deneme aboneliği başlatmaya yönelik bir bağlantı bulunur.

Ücretsiz deneme sürümüne kaydolduktan sonra kullanıcı, kuruluşa yönelik dizini klasik Azure portalında görür ancak dizin genel yöneticisi olmadığı için bu dizini yönetemez (kullanıcı ekleyemez veya herhangi bir mevcut kullanıcı özelliğini düzenleyemez). Abonelik; kullanıcının, klasik Azure portalını kullanmasına ve Azure Active Directory uzantısını görmesine olanak tanır ancak dizini yönetmek için genel yöneticiye ait ek izinler gereklidir.

## <a name="using-your-work-or-school-account-to-manage-an-azure-subscription-that-was-created-by-using-a-microsoft-account"></a>Bir Microsoft hesabı kullanılarak oluşturulmuş bir Azure aboneliğini yönetmek için iş veya okul hesabınızı kullanma
Azure'daki kaynakları yönetmek üzere, en iyi uygulama için [kuruluş olarak Azure'a kaydolmanız](sign-up-organization.md) ve bir iş veya okul hesabı kullanmanız gerekir. İş veya okul hesapları, bunları veren kuruluş tarafından merkezi olarak yönetilebildiği için tercih edilir; bunlar, Microsoft hesaplarından daha fazla özelliğe sahiptir ve kimlik doğrulamaları Azure AD tarafından doğrudan gerçekleştirilir. Aynı hesap; Office 365 veya Microsoft Intune gibi, işletmelere ve kuruluşlara sunulan diğer Microsoft Online Services olanaklarına erişim sağlar. Bu diğer özelliklerle kullandığınız bir hesabınız zaten varsa büyük olasılıkla Azure ile de aynı hesabı kullanmak istersiniz. Ayrıca, Azure aboneliğinizin güvenmesini isteyeceğiniz özellikleri destekleyen bir Active Directory örneğine de sahip olursunuz.

İş veya okul hesapları aynı zamanda Microsoft hesabı dışındaki yöntemlerle de yönetilebilir. Örneğin, bir yönetici; bir iş veya okul hesabının parolasını sıfırlayabilir veya bu hesap için çok faktörlü kimlik doğrulaması isteyebilir.

Bazı durumlarda, kuruluşunuzdaki bir kullanıcının bir tüketici Microsoft hesabı için Azure aboneliği ile ilişkili kaynakları yönetebilmesini isteyebilirsiniz. Farklı hesapların, abonelikleri veya dizinleri yönetmesini sağlamak üzere geçiş yapma hakkında daha fazla bilgi için bkz. [Azure'da Office 365 aboneliğinize yönelik dizini yönetme](#manage-the-directory-for-your-office-365-subscription-in-azure).

## <a name="signing-in-when-you-used-your-work-email-for-your-microsoft-account"></a>Microsoft hesabınız için iş e-postanızı kullandığınızda oturum açma
Daha önce kullanıcı tanımlayıcısı olarak iş e-postanızla bir tüketici Microsoft hesabı oluşturduysanız Microsoft Azure Hesabı sisteminden veya Microsoft Hesabı sisteminden seçim yapmanızı isteyen bir sayfa görebilirsiniz.

![][3]

Biri Azure AD'de, diğeri ise tüketici Microsoft hesap sisteminde olmak üzere aynı ada sahip kullanıcı hesaplarınız mevcuttur. Kullanmak istediğiniz Azure aboneliğiyle ilişkili hesabı seçmeniz gerekir. Bu kullanıcı için aboneliğin mevcut olmadığını bildiren bir hata alırsanız büyük olasılıkla yanlış seçeneği seçmişsinizdir. Oturumu kapatın ve tekrar deneyin. Oturum açma işlemini engelleyebilen hatalar hakkında daha fazla bilgi için bkz. ["Hesabınızla ilişkili hiç abonelik bulamadık" hatalarını giderme](https://social.msdn.microsoft.com/Forums/en-US/f952f398-f700-41a1-8729-be49599dd7e2/troubleshooting-we-were-unable-to-find-any-subscriptions-associated-with-your-account-errors-in?forum=windowsazuremanagement).

## <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Azure'da Office 365 aboneliğinize yönelik dizini yönetme
Azure'a kaydolmadan önce Office 365'e kaydolduğunuzu varsayalım. Şimdi, Office 365 aboneliğine yönelik dizini klasik Azure portalında yönetmek istiyorsunuz. Azure'a kaydolup kaydolmamanıza bağlı olarak, bunu yapmanın iki yolu vardır.

### <a name="i-do-not-have-a-subscription-for-azure"></a>Azure aboneliğim yok
Bu durumda, Office 365'te oturum açmak için kullandığınız iş veya okul hesabını kullanarak [Azure'a kaydolmanız](sign-up-organization.md) yeterlidir. Office 365 hesabına ait ilgili bilgiler, Azure kayıt formunda önceden doldurulmuş olarak sağlanacaktır. Hesabınız, aboneliğin Hizmet Yöneticisi rolüne atanacaktır.  

### <a name="i-do-have-a-subscription-for-azure-using-my-microsoft-account"></a>Azure için Microsoft hesabımı kullanan bir aboneliğe sahibim
Bir iş veya okul hesabı kullanarak Office 365 için kaydolup ardından bir Microsoft hesabı kullanarak Azure'a kaydolduysanız bu durumda iki dizininiz vardır: biri işinize veya okulunuza yönelik bir dizin, diğeri ise Azure'a kaydolduğunuzda oluşturulan bir Varsayılan dizin.

Her iki dizini de klasik Azure portalında yönetmek için bu adımları tamamlayın.

> [!NOTE]
> Bu adımlar, yalnızca bir kullanıcı bir Microsoft hesabıyla oturum açmış durumdayken tamamlanabilir. Kullanıcı bir iş veya okul hesabıyla oturum açmışsa bir iş veya okul hesabının kimlik doğrulaması yalnızca kendi ana dizini (iş veya okul hesabının depolandığı ve işin veya okulun sahibi olduğu dizin) tarafından gerçekleştirilebildiği için **Mevcut dizini kullan** seçeneği kullanılamaz.
> 
> 

1. Microsoft hesabınızı kullanarak klasik Azure portalında oturum açın.
2. **New (Yeni)** > **App services (Uygulama hizmetleri)** > **Active Directory** > **Directory (Dizin)** > **Custom Create (Özel Oluştur)** düğmesine tıklayın.
3. **Use existing directory (Var olan dizini kullan)** seçeneğine tıklayın ve **I am ready to be signed out now (Şimdi oturumumun kapatılması için hazırım)** seçeneğini işaretleyip eylemi tamamlamak için onay işaretine tıklayın.
4. İş veya okul dizini için genel yönetici haklarına sahip olan bir hesabı kullanarak klasik Azure portalında oturum açın.
5. **Use the Contoso directory with Azure? (Contoso dizini Azure ile kullanılsın mı?)** sorusu sorulduğunda, **Continue (Devam)** seçeneğine tıklayın.
6. **Sign out now (Şimdi oturumu kapat)** seçeneğine tıklayın.
7. Microsoft hesabınızı kullanarak klasik Azure portalında tekrar oturum açın. Her iki dizin de Active Directory uzantısında görünür.

## <a name="next-steps"></a>Sonraki Adımlar
* Bir Azure aboneliğine yönelik olarak yöneticileri değiştirme hakkında daha fazla bilgi için bkz. [Azure yönetici rollerini ekleme veya değiştirme](../billing-add-change-azure-subscription-administrator.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](active-directory-understanding-resource-access.md)
* Azure AD'de rol atama hakkında daha fazla bilgi için bkz. [Azure Active Directory'de yönetici rolü atama](active-directory-assign-admin-roles.md)
* [Azure’a kuruluş olarak kaydolma](sign-up-organization.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG



<!--HONumber=Nov16_HO2-->


