---
title: Azure Active Direcory kiracı dizini genel kullanımı | Microsoft Docs
description: Azure AD kiracısının ne olduğu ve Azure'ın, Azure Active Directory kullanılarak nasıl yönetileceği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.openlocfilehash: 9da9380f9fd492284a3e82be44051f150f7b3c53
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="manage-your-azure-ad-directory"></a>Azure AD dizininizi yönetme

## <a name="what-is-an-azure-ad-tenant"></a>Azure AD kiracısı nedir?
Azure Active Directory’de (Azure AD) kiracı, kuruluşunuzun Azure veya Office 365 gibi bir Microsoft bulut hizmetinde oturum açtığında aldığı ve sahip olduğu bir Azure AD dizininin adanmış örneğidir. Her Azure AD dizini, diğer Azure AD dizinlerinden farklı ve ayrıdır. Kurumsal ofis binasının yalnızca kuruluşunuza özel güvenilir bir varlık olması gibi, Azure AD dizini de yalnızca sizin kuruluşunuz tarafından kullanılmak üzere tasarlanan güvenilir bir varlıktır. Azure AD mimarisi, kullanıcı verilerini ve kimlik bilgilerini ayrı tutar, bu da bir Azure AD dizinindeki kullanıcıların ve yöneticilerin yanlışlıkla veya kötü amaçlı olarak başka bir dizindeki verilere erişemeyeceği anlamına gelir.

![Azure Active Directory'yi yönetme](./media/active-directory-administer/aad_portals.png)

## <a name="how-can-i-get-an-azure-ad-directory"></a>Azure AD dizinine nasıl sahip olabilirim?
Azure AD, aşağıdakiler dahil olmak üzere çoğu Microsoft bulut hizmetinin dışında çekirdek dizin ve kimlik yönetimi özellikleri sağlar:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Bu Microsoft bulut hizmetlerinden herhangi birine kaydolduğunuzda, bir Azure AD dizinine sahip olursunuz. Gerektikçe ek dizinler oluşturabilirsiniz. Örneğin, ilk dizininizi üretim dizini olarak tutup test veya hazırlama işlemleri için başka bir dizin oluşturabilirsiniz.

### <a name="using-the-azure-ad-directory-that-comes-with-a-new-azure-subscription"></a>Yeni Azure AD aboneliğiyle birlikte gelen Azure AD dizinini kullanma

Diğer Microsoft Hizmetleri için kaydolurken ilk hizmetiniz için kullandığınız yönetici hesabını kullanmanızı öneririz. Bir Microsoft hizmetine ilk kez kaydolurken sağladığınız bilgiler, kuruluşunuz için yeni Azure AD dizin örneğini oluşturmak için kullanılır. Diğer Microsoft hizmetlerine abone olduğunuzda oturum açma girişimlerinin kimliğini doğrulamak için bu dizini kullanırsanız, varsayılan dizininizde yapılandırdığınız mevcut kullanıcı hesaplarını, ilkeleri, ayarları veya şirket içi dizin tümleştirmesini kullanabilirler.

Örneğin, bir Microsoft Intune aboneliği için kaydolduysanız ve şirket içi Active Directory'nizi Azure AD diziniyle eşitlediyseniz, Office 365 gibi başka bir Microsoft hizmeti için kaydolabilir ve Microsoft Intune ile sahip olduğunuz aynı dizin tümleştirme avantajlarını kolayca elde edebilirsiniz.

Şirket içi dizininizi Azure AD ile tümleştirme hakkında daha fazla bilgi için bkz. [Azure AD Connect ile dizin tümleştirme](active-directory-aadconnect.md).

### <a name="associate-an-existing-azure-ad-directory-with-a-new-azure-subscription"></a>Mevcut bir Azure AD dizinini yeni bir Azure aboneliğiyle ilişkilendirme
Yeni bir Azure aboneliği ile mevcut bir Office 365 veya Microsoft Intune aboneliği için oturum açma kimlik doğrulaması bir dizini ilişkilendirebilirsiniz. Bu senaryo hakkında daha fazla bilgi için bkz. [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Microsoft bulut hizmetine bir kuruluş olarak kaydolarak Azure AD dizini oluşturma
Henüz bir Microsoft bulut hizmeti aboneliğiniz yoksa kaydolmak için aşağıdaki bağlantılardan birini kullanabilirsiniz. İlk hizmetiniz için kaydolduğunuzda otomatik olarak bir Azure AD dizini oluşturulur.

* [Microsoft Azure](https://account.azure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="how-to-change-the-default-directory-for-a-subscription"></a>Aboneliğin varsayılan dizinini değiştirme

1. Aboneliğin sahipliğini devretmek için, aboneliğin Hesap Yöneticisi olan bir hesapla [Azure Hesap Merkezi](https://account.azure.com/Subscriptions)’nde oturum açın.
2. Aboneliğin sahibi olmasını istediğiniz kullanıcının hedeflenen dizininde olduğundan emin olun.
3. **Aboneliği devret**'e tıklayın.
4. Alıcıyı belirtin. Alıcı otomatik olarak, kabul bağlantısı içeren bir e-posta alır.
5. Alıcı bağlantıya tıklar ve yönergeleri izler; bu arada ödeme bilgilerini de girer. Alıcı başarılı olduğunda, abonelik devredilir. 
6. Abonelik sahipliğini aktarma işlemi başarılı olursa, aboneliğin varsayılan dizini ilgili kullanıcının bulunduğu dizin olarak değiştirilir.

Daha fazla bilgi için bkz. [Azure aboneliği sahipliğini başka bir hesaba aktarma](../billing/billing-subscription-transfer.md)

### <a name="manage-the-default-directory-in-azure"></a>Azure’da varsayılan dizininizi yönetme
Azure için kaydolduğunuzda, aboneliğiniz ile bir varsayılan Azure AD dizini ilişkilendirilir. Azure AD kullanmanın maliyeti yoktur ve dizinleriniz ücretsiz kaynaklardır. Ayrı olarak lisanslanan ve şirkete özel oturum açma, self servis parola sıfırlama gibi ek işlevler sunan ücretli Azure AD hizmetleri vardır. Varsayılan *.onmicrosoft.com etki alanı yerine sahip olduğunuz bir DNS adını kullanarak özel bir etki alanı da oluşturabilirsiniz.

## <a name="how-can-i-manage-directory-data"></a>Dizin verilerini nasıl yönetebilirim?
Bir veya daha fazla Microsoft bulut hizmeti aboneliğini yönetmek için [Azure AD yönetim merkezini](https://aad.portal.azure.com), Microsoft Intune hesap portalını veya [Office 365 Yönetim Merkezi](https://portal.office.com/)'ni kullanarak kuruluşunuzun dizin verilerini yönetebilirsiniz. Azure AD'de depolanan verileri yönetmenize yardımcı olması için [Azure Active Directory PowerShell cmdlet’lerini](https://docs.microsoft.com/powershell/azure/active-directory) de kullanabilirsiniz.

Bu portallardan (veya cmdlet'lerden) birini kullanarak şunları yapabilirsiniz:

* Kullanıcı ve grup hesapları oluşturma ve yönetme
* Kuruluşunuzun abonelikleri için bulut hizmetlerini yönetme
* Azure AD kimlik ve kimlik doğrulama hizmetleri ile şirket içi tümleştirmeyi ayarlama

Azure AD yönetim merkezi, Office 365 Yönetici Merkezi, Microsoft Intune hesap portalı ve Azure AD cmdlet'lerinin tümü, okuma ve yazma işlemlerini kuruluş dizininizle ilişkilendirilen tek bir paylaşılan Azure AD örneğinde gerçekleştirir. Bu araçların her biri, dizin verilerinizdeki değişiklikleri alan bir ön uç arabirimi olarak görev yapar.
Bu hizmetlerde portallar veya cmdlet’lerden birini kullanarak kuruluşunuzun verileri değiştirdiğinizde, değişiklikler, oturum açtığınızda diğer portallarda da gösterilir. Bu veriler, abone olduğunuz Microsoft bulut hizmetleriyle paylaşılır.

Örneğin, Office 365 Yönetim Merkezi'ni kullanarak bir kullanıcının oturum açmasını engellerseniz aynı kullanıcının kuruluşunuzun şu anda abone olduğu diğer hizmetlerde de oturum açması engellenir. Aynı kullanıcı hesabını Microsoft Intune hesap portalında görüntülediğinizde kullanıcının engellendiğini görebilirsiniz.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Birden fazla dizini nasıl ekleyip yönetebilirim?
[Azure portalında bir Azure AD dizini ekleyebilirsiniz](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Bilgileri girip **Oluştur**’u seçin.

Her dizini tamamen bağımsız bir kaynak olarak yönetebilirsiniz: Her dizin eşdüzeyde, tam özellikli ve yönettiğiniz diğer dizinlerden mantıksal olarak bağımsızdır ve dizinler arasında üst-alt ilişkisi yoktur. Dizinler arasındaki bu bağımsızlığa kaynak bağımsızlığı, yönetim bağımsızlığı ve eşitleme bağımsızlığı dahildir.

* **Kaynak bağımsızlığı**. Bir dizinde kaynak oluşturur veya silerseniz dış kullanıcılar kısmen hariç olmak üzere bu durumun başka bir dizindeki kaynaklar üzerinde herhangi bir etkisi yoktur. Bir dizinde "contoso.com" özel etki alanını kullanırsanız başka bir dizinde kullanamazsınız.
* **Yönetim bağımsızlığı**.  "Contoso" dizininin yönetici olmayan bir kullanıcısı "Test" adlı bir test dizini oluşturursa:
  
  * ‘Test’ yöneticisi özel olarak yetki vermediği sürece, ‘Contoso’ dizininin yöneticilerinin ‘Test’ dizini için doğrudan yönetim ayrıcalıkları yoktur. "Contoso" yöneticileri, "Test" dizinini oluşturan kullanıcı hesabını denetleyebildiği için "Test" dizinine erişimi denetleyebilir.
    
  * Dizinde bir kullanıcının yönetici rolünü değiştirirseniz (ekler veya kaldırırsanız) bu değişiklik, kullanıcının başka bir dizinde sahip olduğu herhangi bir yönetici rolünü etkilemez.
* **Eşitleme bağımsızlığı**. Her Azure AD kiracısını bağımsız olarak tek örnek Azure AD Connect dizin eşitleme aracı verilerini almak için yapılandırabilirsiniz.

Diğer Azure kaynaklarının aksine dizinlerinizin bir Azure aboneliğinin alt kaynakları olmadığını unutmayın. Bu nedenle Azure aboneliğinizi iptal etseniz veya aboneliğinizin süresi dolsa bile Azure AD PowerShell'i, Azure Graph API'sini veya Office 365 Yönetim Merkezi gibi diğer arabirimleri kullanarak dizin verilerinize erişmeye devam edersiniz. Ayrıca başka bir aboneliği dizinle ilişkilendirebilirsiniz.

## <a name="how-to-prepare-to-delete-an-azure-ad-directory"></a>Azure AD dizinini silmeye hazırlanma
Azure AD dizinini portaldan genel yönetici silebilir. Bir dizin silindiğinde dizinde bulunan tüm kaynaklar da silinir. Silmeden önce dizinin gerekmediğini doğrulayın.

> [!NOTE]
> Kullanıcı, bir iş veya okul hesabıyla oturum açtıysa kendi giriş dizinini silmeye çalışmamalıdır. Örneğin, kullanıcı joe@contoso.onmicrosoft.com olarak oturum açtıysa, varsayılan etki alanı contoso.onmicrosoft.com olan dizini silemez.

Azure AD, bir dizinin silinmesi için belirli koşulların sağlanmasını gerektirir. Bu, bir dizinin silinmesinin kullanıcılar ve uygulamalar üzerinde olumsuz bir etki oluşturması (örneğin, kullanıcıların Office 365'te oturum açamamaları veya Azure'daki kaynaklara erişememeleri) riskini azaltır. Örneğin, bir aboneliğe ilişkin dizin yanlışlıkla silinirse kullanıcılar o aboneliğe ilişkin Azure kaynaklarına erişemez.

Şu koşullar denetlenir:

* Dizin içinde dizini silebilecek tek kullanıcının genel yönetici olması gerekir. Dizin silinmeden önce diğer tüm kullanıcılar silinmelidir. Kullanıcılar şirket içinden eşitlenmişse, eşitlemenin kapatılması ve Azure portalı ya da Azure PowerShell cmdlet’leri kullanılarak bulut dizininde kullanıcıların silinmesi gerekir. Grupların veya kişilerin (örneğin, Office 365 Yönetim Merkezi'nden eklenen kişiler) silinmesi gerekmez.
* Dizinde uygulama bulunamaz. Dizin silinmeden önce tüm uygulamaların silinmesi gerekir.
* Dizine çok faktörlü kimlik doğrulaması sağlayıcıları bağlanamaz.
* Microsoft Çevrimiçi Hizmetlerine (dizinle ilişkili Azure AD Premium, Microsoft Azure veya Office 365 gibi) ilişkin hiçbir aboneliğin bulunmaması gerekir. Örneğin, sizin için Azure'da varsayılan bir dizin oluşturulduysa ve Azure aboneliğinizin kimlik doğrulaması için hâlâ bu dizini kullanıyor olması halinde bu dizini silemezsiniz. Benzer şekilde, başka bir kullanıcı dizinle bir aboneliği ilişkilendirdiyse o dizini silemezsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Forumu](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=windowsazuread)
* [Azure Multi-Factor Authentication Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=windowsazureactiveauthentication)
* [Azure soruları için StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
* [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory)
* [Azure AD’de yönetici rolü atama](active-directory-assign-admin-roles-azure-portal.md)
