---
title: "Yönergeleri uygulamalar için markalama | Microsoft Docs"
description: "Azure Active Directory'nin Geliştirici yönelimli kaynakları için kapsamlı bir kılavuz"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 4f6806cde52ce965a8f78a5cce8a24c3d1248594
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="branding-guidelines-for-applications"></a>Uygulamalar için markalama talimatları
Bu konuda, Azure Active Directory (Azure AD) ile uygulamaları geliştirirken kullanmalısınız markalama talimatları anlatılmaktadır. Bu yönergeleri müşterilerinizin doğrudan Azure AD'de yönetilen kendi iş veya Okul hesabı kullanmak istedikleri veya kullanıcıların kişisel hesap için kaydolma ve oturum açma ve uygulamanıza yardımcı olur.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Microsoft iş ve kişisel hesapları veya Okul
Microsoft, iki tür kullanıcı hesaplarının yönetir:

* **Kişisel hesaplar** (eski adıyla Windows Live ID olarak). Bu hesapları arasındaki ilişkiyi göstermek *tek tek* kullanıcıların ve Microsoft ve misiniz Microsoft'tan tüketici cihazlarının ve hizmetlerine erişmek için kullanılabilir. Bu hesapları kişisel kullanım için tasarlanmıştır.
* **İş veya Okul hesapları.** Bu hesaplar, Azure Active Directory kullanan kuruluşlar adına Microsoft tarafından yönetilir. Bu hesapları, Microsoft Office 365 ve diğer iş hizmetlerinde oturum açmak için kullanılır.

Microsoft iş veya Okul hesapları genellikle kuruluşlarına (şirket, okul, devlet dairesi) tarafından son kullanıcılara (çalışanlar, Öğrenciler, federal çalışan) atanır. Bu hesapları doğrudan bulutta, Azure AD platformu yönetilen ya da Windows Server Active Directory gibi bir şirket içi dizininden Azure AD ile eşitlenir. Microsoft *koruyucu* iş veya Okul hesapları, ancak hesapları ait ve kuruluş tarafından denetlenir.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Azure AD hesapları, uygulamanızda başvurma
Microsoft Azure veya Active Directory marka ve ikisi son kullanıcılara, gereken açığa çıkarmıyor.

* Kullanıcı oturum açtıktan sonra kuruluş adını ve logosunu mümkün olduğunca kullanmanız gerekir. Bu, "kuruluşunuzun" gibi genel koşulları kullanmaktan daha iyidir.
* Kullanıcılar oturum değil, kendi hesapları olarak başvurmalıdır "iş veya Okul hesapları" ve Microsoft tarafından yönetilen bu hesapları iletmek için Microsoft logosu kullanın. Kullanıcı karışıklığı oluşturan koşulları "Kuruluş hesabı", "iş hesabı" veya "şirket hesabı" gibi kullanmayın.

## <a name="user-account-pictogram"></a>Kullanıcı hesabı piktogram
Bu yönergeleri önceki sürümünde, "mavi rozet" piktogram kullanarak önerilir. Kullanıcı ve geliştirici geri bildirimi doğrultusunda, artık Microsoft logosu kullanımını yerine öneririz. Bu, kullanıcıların, Office 365 veya diğer Microsoft iş Hizmetleri uygulamanıza oturum açmak için kullandığınız hesabın yeniden kullanabilir anlamasına yardımcı olur.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Kaydolan ve Azure AD ile imzalama
Uygulamanıza kaydolma ve oturum açma için ayrı yollar var ve aşağıdaki bölümler her iki senaryoları için görsel kılavuz sağlar.

**Uygulamanız son kullanıcı oturum destekliyorsa Yukarı (örneğin deneme veya freemium modeline ücretsiz)**: göstermek bir **oturum açma** kullanıcıların kendi iş hesabını veya kişisel hesabıyla uygulamanızla erişmesine olanak sağlayan düğmeyi. Azure AD uygulamanızı eriştiklerinde ilk kez bir onay istemi gösterir.

**Uygulamanızı yalnızca yöneticileri onayı izinleri gerektiriyorsa ya da uygulamanızı kuruluş lisanslama gerektiriyorsa**: kullanıcı oturum açma yönetici alımdaki ayırın. **"Bu uygulamayı Al" düğmesini** oturum sonra kullanıcılar kuruluşlarındaki adına izin vermesini isteyin admins yönlendirir. Bu, son kullanıcıların onay istekleri uygulamanıza gizleme eklenen avantajına sahiptir.

## <a name="visual-guidance-for-app-acquisition"></a>Uygulama alım için görsel kılavuz
"Uygulamayı Al" bağlantı kullanıcı Azure AD erişim izni verme yönlendirme gerekir (yetkilendirme) sayfasında, uygulamanızın Microsoft tarafından barındırılan kuruluşun verilere erişimi yetkilendirmek bir kuruluş yöneticisi izin vermek için. Erişim isteği hakkında ayrıntılar içinde ele alınmıştır [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md) makalesi.

Yöneticiler, uygulamanızın kabul edildikten sonra bunlar için kullanıcı Office 365 uygulama Başlatıcı deneyimini eklemeyi seçebilirsiniz (waffle'ndan ve erişilebilir [https://portal.office.com/myapps](https://portal.office.com/myapps)). Bu özellik bildirmek isterseniz, "Bu uygulamayı kuruluşunuzda Ekle" ve böyle bir düğme Göster gibi koşulları kullanabilirsiniz:

![Uygulama türleri ve senaryolar](./media/active-directory-branding-guidelines/add-to-my-org.png)

Ancak, düğmeleri kalmak yerine açıklayıcı metin yazma öneririz. Örneğin:

> *Office 365 veya Microsoft'un diğer iş hizmeti zaten kullanıyorsanız, yalnızca kuruluşunuzun verilerini < your_app_name > erişim izni verebilir. Bu, kullanıcılarınızın kendi mevcut iş hesaplarıyla < your_app_name > erişmesine izin verir.*
> 
> 

## <a name="visual-guidance-for-sign-in"></a>Oturum açma için görsel kılavuz
Uygulamanızı bir oturum kullanıcıları Azure AD ile tümleştirmek için kullandığınız Protokolü karşılık gelen oturum açma uç noktasına yönlendirir düğmesi görüntülemesi gerekir. Aşağıdaki bölümde, ne bu düğme gibi görünmelidir ayrıntılar sağlar.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogram ve "Microsoft ile oturum"
Microsoft logosu ilişkilendirmedir ve diğer kimlik sağlayıcılardan arasında Azure AD benzersiz olarak temsil eder "Oturum in ile Microsoft" Koşulları'nı uygulamanızı destekleyebilir. "Oturum in ile Microsoft" yeterli alan yoksa, oturum açma"ile" kısaltın Tamam.

![Uygulama türleri ve senaryolar](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Uygulama türleri ve senaryolar](./media/active-directory-branding-guidelines/sign-in-light.png)

Koyu Renk düzenini düğmeleri için de kullanabilirsiniz.

![Uygulama türleri ve senaryolar](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Uygulama türleri ve senaryolar](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Marka yapılması ve yapılmaması gerekenler
**YAPMAK** "iş veya Okul hesabı" ile birlikte "Sign in ile Microsoft" düğmesi bunu kullanıp kullanmayacağınızı tanıması son kullanıcılara yardımcı olmak için ek açıklama sağlamak için kullanın. **Verme** "Kuruluş hesabı", "iş hesabı" veya "şirket hesabı." gibi diğer kullanım koşulları

**Verme** "Office 365 ID" veya "Azure kimliği" kullanın. Office 365 de Azure AD kimlik doğrulaması için kullanmayan bir Microsoft sunan bir tüketici adıdır.

**Verme** Microsoft logosu alter.

**Verme** Azure veya Active Directory markalar son kullanıcılara kullanıma sunar. Ancak Tamam bu koşulları geliştiriciler, BT uzmanları ve yöneticileri ile kullanmak için.

## <a name="navigation-dos-and-donts"></a>Gezinti yapılması ve yapılmaması gerekenler
**YAPMAK** kullanıcıların oturumu kapatın ve başka bir kullanıcı hesabına geçmek bir yol sağlar. Kişiler, genellikle çoğu kişi Microsoft/Facebook/Google/Twitter tek bir kişisel hesabından olmakla birlikte, birden fazla kuruluşla ilişkilendirilir. Çoklu oturum açmış kullanıcı desteği yakında geliyor.

