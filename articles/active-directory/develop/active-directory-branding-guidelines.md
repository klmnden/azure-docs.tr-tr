---
title: Yönergeleri uygulamalar için markalama | Microsoft Docs
description: Azure Active Directory'nin Geliştirici yönelimli kaynakları için kapsamlı bir kılavuz
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: celested
ms.reviewer: arielgo; skwan
ms.custom: aaddev
ms.openlocfilehash: 22d10dfc87777746182068ce9a8162420c0c95d4
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713322"
---
# <a name="branding-guidelines-for-applications"></a>Uygulamalar için markalama talimatları

Bu makalede, Azure Active Directory (Azure AD) ile uygulamaları geliştirirken kullanmalısınız markalama talimatları anlatılmaktadır. Bu yönergeleri müşterilerinizin doğrudan Azure AD'de yönetilen kendi iş veya Okul hesabı kullanmak istedikleri veya kullanıcıların kişisel hesap için kaydolma ve oturum açma ve uygulamanıza yardımcı olur.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Microsoft iş ve kişisel hesapları veya Okul

Microsoft, iki tür kullanıcı hesaplarının yönetir:

* **Kişisel hesaplar** (eski adıyla Windows Live ID olarak). Bu hesapları arasındaki ilişkiyi göstermek *tek tek* kullanıcıların ve Microsoft ve misiniz Microsoft'tan tüketici cihazlarının ve hizmetlerine erişmek için kullanılabilir. Bu hesapları kişisel kullanım için tasarlanmıştır.
* **İş veya Okul hesapları.** Bu hesaplar, Azure Active Directory kullanan kuruluşlar adına Microsoft tarafından yönetilir. Bu hesapları, Microsoft Office 365 ve diğer iş hizmetlerinde oturum açmak için kullanılır.

Microsoft iş veya Okul hesapları genellikle kuruluşlarına (şirket, okul, devlet dairesi) tarafından son kullanıcılara (çalışanlar, Öğrenciler, federal çalışan) atanır. Bu hesapları doğrudan bulutta (Azure AD platformu) yönetilen veya Windows Server Active Directory gibi bir şirket içi dizininden Azure AD ile eşitlenir. Microsoft *koruyucu* iş veya Okul hesapları, ancak hesapları ait ve kuruluş tarafından denetlenir.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Azure AD hesapları, uygulamanızda başvurma

Microsoft, son kullanıcılara Azure veya Active Directory marka ve ne yapmanız gerekenler açığa çıkarmıyor.

* Kullanıcı oturum açtıktan sonra kuruluş adını ve logo mümkün olduğunca kullanın. Bu "kuruluşunuzun." gibi genel koşulları kullanmaktan daha iyi?
* Kullanıcıların oturumunuz açık değil, kendi hesapları olarak duyduğunuzda "iş veya Okul hesapları" ve Microsoft bu hesapları yönetir iletmek için Microsoft logosu kullanın. "Kuruluş hesabı," benzer terimleri kullanma "iş hesabı" veya "kullanıcı karışıklığı oluşturan şirket hesabı,".

## <a name="user-account-pictogram"></a>Kullanıcı hesabı piktogram

Bu yönergeleri önceki sürümünde, "mavi rozet" piktogram kullanarak önerilir. Kullanıcı ve geliştirici geri bildirimi doğrultusunda, artık Microsoft logosu kullanımını yerine öneririz. Microsoft logosu, kullanıcıların, Office 365 veya diğer Microsoft iş Hizmetleri uygulamanıza imzalamak için kullandığınız hesabın yeniden kullanabilir anlamasına yardımcı olur.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Kaydolan ve Azure AD ile imzalama

Uygulamanıza kaydolma ve oturum açma için ayrı yollar var ve aşağıdaki bölümler her iki senaryoları için görsel kılavuz sağlar.

**Son kullanıcı kayıt (örneğin, ücretsiz deneme sürümü veya freemium modeline) uygulamanız destekliyorsa,**: göstermek bir **oturum açma** kullanıcıların kendi iş hesabını veya kişisel hesabıyla uygulamanızla erişmesine olanak sağlayan düğmeyi. Azure AD uygulamanızı eriştiklerinde ilk kez bir onay istemi gösterir.

**Uygulamanızı yalnızca yöneticileri onayı izinleri gerektiriyorsa ya da uygulamanızı kuruluş lisanslama gerektiriyorsa**: yönetici alımdaki kullanıcı oturum açma ayırın. **"Bu uygulamayı Al" düğmesini** oturum sonra son kullanıcı onayı istemleri uygulamanıza gizleme eklenen avantajına sahiptir, kuruluşunuzdaki kullanıcılar adına izin vermesini isteyin admins yönlendirir.

## <a name="visual-guidance-for-app-acquisition"></a>Uygulama alım için görsel kılavuz

"Uygulamayı Al" bağlantı kullanıcı Azure AD erişim izni verme yönlendirme gerekir (yetkilendirme) sayfasında, uygulamanızın Microsoft tarafından barındırılan kuruluşun verilere erişimi yetkilendirmek bir kuruluş yöneticisi izin vermek için. Erişim isteği hakkında ayrıntılar içinde ele alınmıştır [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md) makalesi.

Yöneticiler, uygulamanızın kabul edildikten sonra bunlar için kullanıcı Office 365 uygulama Başlatıcı deneyimini eklemeyi seçebilirsiniz (waffle'ndan ve erişilebilir [ https://portal.office.com/myapps ](https://portal.office.com/myapps)). Bu özellik bildirmek isterseniz, "Bu uygulamayı kuruluşunuzda Ekle" ve bir düğme Göster gibi koşulları kullanabilirsiniz aşağıdaki örnekteki gibi:

![Uygulama türleri ve senaryolar](./media/active-directory-branding-guidelines/add-to-my-org.png)

Ancak, düğmeleri kalmak yerine açıklayıcı metin yazma öneririz. Örneğin:

> *Office 365 veya Microsoft'un diğer iş hizmeti zaten kullanıyorsanız, kuruluşunuzun verilerini < your_app_name > erişim izni verebilir. Bu, kullanıcılarınızın kendi mevcut iş hesaplarıyla < your_app_name > erişmesine izin verir.*

Resmi Microsoft logosu, uygulamanızda kullanmak için karşıdan yüklemek için kullanın ve bilgisayarınıza kaydetmek istediğiniz bir sağ tıklayın.

| Varlık                                | PNG biçimi | SVG biçimi |
| ------------------------------------ | ---------- | ---------- |
| Microsoft logosu  | ![Microsoft logosu PNG](./media/active-directory-branding-guidelines/MS-SymbolLockup_MSSymbol_19.png) | ![Microsoft logosu SVG](./media/active-directory-branding-guidelines/MS-SymbolLockup_MSSymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Oturum açma için görsel kılavuz

Uygulamanız kullanıcıların Azure AD ile tümleştirmek için kullandığınız Protokolü karşılık gelen oturum açma uç noktası yönlendiren bir oturum açma düğmesi görüntülemelidir. Aşağıdaki bölümde, ne bu düğme gibi görünmelidir ayrıntılar sağlar.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogram ve "Microsoft ile oturum"

Microsoft logosu ilişkilendirmedir ve diğer kimlik sağlayıcılardan arasında Azure AD benzersiz olarak temsil "Oturum in ile Microsoft" Koşulları'nı uygulamanızı destekleyebilir. "Oturum in ile Microsoft" yeterli alan yoksa, oturum açma"ile." kısaltın Tamam Açık veya koyu renk düzenini düğmelerini kullanabilirsiniz.

Aşağıdaki diyagramda Microsoft tarafından önerilen varlıklar, uygulamanızı kullanırken redlines gösterir. Redlines "Sign in ile Microsoft" veya "Oturum" kısa sürüm için geçerlidir.

![Microsoft ile oturum redlines](./media/active-directory-branding-guidelines/Sign-in-with-Microsoft-redlines.png)

Uygulamanızda resmi görüntülerde indirmek için bilgisayarınıza kaydedin ve kullanmak istediğiniz bir sağ tıklayın.

| Varlık                                | PNG biçimi | SVG biçimi |
| ------------------------------------ | ---------- | ---------- |
| Microsoft (Koyu tema) ile oturum açın  | ![Düğme koyu tema PNG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark.png) | ![Microsoft düğmesi koyu tema oturum SVG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark.svg) |
| Microsoft (açık tema) ile oturum açın | ![Düğme açık tema PNG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light.png) | ![Microsoft düğmesi açık tema oturum SVG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light.svg) |
| (Koyu tema) oturum açın                 | ![Kısacası düğmesi koyu tema PNG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark_short.png) | ![Kısacası düğmesi koyu tema SVG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark_short.svg) |
| (Açık tema) oturum açın                | ![Kısacası düğmesi açık tema PNG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light_short.png) | ![Kısacası düğmesi açık tema SVG oturum](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light_short.svg) |


## <a name="branding-dos-and-donts"></a>Marka yapılması ve yapılmaması gerekenler

**YAPMAK** "iş veya Okul hesabı" ile birlikte "Sign in ile Microsoft" düğmesi son kullanıcıların bunu kullanıp kullanmayacağınızı tanıması yardımcı olmak için ek açıklama sağlamak için kullanın. **Verme** "Kuruluş hesabı", "iş hesabı" veya "şirket hesabı." gibi diğer kullanım koşulları

**Verme** "Office 365 ID" veya "Azure ID" kullanın Office 365 de Azure AD kimlik doğrulaması için kullanmayan Microsoft sunan bir tüketici adıdır.

**Verme** Microsoft logosu alter.

**Verme** Azure veya Active Directory markalar son kullanıcılara kullanıma sunar. Ancak Tamam bu koşulları geliştiriciler, BT uzmanları ve yöneticileri ile kullanmak için.

## <a name="navigation-dos-and-donts"></a>Gezinti yapılması ve yapılmaması gerekenler

**YAPMAK** kullanıcıların oturumu kapatın ve başka bir kullanıcı hesabına geçmek bir yol sağlar. Kişiler, genellikle çoğu kişi Microsoft/Facebook/Google/Twitter tek bir kişisel hesabından olmakla birlikte, birden fazla kuruluşla ilişkilendirilir. Çoklu oturum açmış kullanıcı desteği yakında geliyor.
