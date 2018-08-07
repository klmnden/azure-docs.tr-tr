---
title: Marka yönergeleri için uygulamalar | Microsoft Docs
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
ms.reviewer: arielgo, skwan
ms.custom: aaddev
ms.openlocfilehash: 0eb410f957f962375371f5c31a4589463ac9bed3
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581998"
---
# <a name="branding-guidelines-for-applications"></a>Uygulamalar için markalama

Bu makalede, Azure Active Directory (Azure AD) ile uygulamalar geliştirirken kullanmalısınız markalama açıklanmaktadır. Bu yönergeler, Azure AD'de yönetilen uygulamaya iş veya Okul hesabı kullanmak istedikleri ya da kendi kişisel hesap için kaydolma ve oturum açma, uygulamanız için müşterilerinize doğrudan yardımcı olur.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>İş ve kişisel hesaplar veya Microsoft Okul hesapları

Microsoft, iki tür kullanıcı hesapları yönetir:

* **Kişisel hesaplar** (eski adıyla Windows Live ID da bilinir). Bu hesapları arasındaki ilişkiyi göstermek *bireysel* kullanıcılar ve Microsoft ve olan Microsoft tüketici cihazlara ve hizmetlere erişmek için kullanılabilir. Bu hesaplar, kişisel kullanım için tasarlanmıştır.
* **İş veya Okul hesapları.** Bu hesaplar, Azure Active Directory kullanan kuruluşlar adına Microsoft tarafından yönetilir. Bu hesaplar, Office 365 ve diğer iş Hizmetleri için Microsoft oturum açmak için kullanılır.

Microsoft iş veya Okul hesapları genellikle (şirket, okul, kamu kurumunda), kuruluşların son kullanıcılara (çalışanlar, Öğrenciler, federal çalışan) atanır. Bu hesapları doğrudan bulutta (Azure AD platformu) yönetilen veya Windows Server Active Directory gibi bir şirket içi dizinden Azure AD'ye eşitlenmiş. Microsoft, *Emanetçisi* iş veya Okul hesapları, ancak hesaplarına ait ve kuruluş tarafından denetlenir.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Uygulamanızı Azure AD hesaplarının başvurma

Microsoft Azure veya Active Directory marka adları ve bunların hiçbiri son kullanıcılara gerektiğini ortaya çıkarmıyor.

* Kullanıcılar oturum açtıktan sonra kuruluş adını ve logosunu mümkün olduğunca kullanın. Bu "kuruluşunuzun." gibi genel koşulları kullanmaktan daha iyidir
* Kullanıcılar oturum açmadıysanız, hesaplarına bakın "iş veya Okul hesapları" ve Microsoft bu hesaplar yönetir iletmek için Microsoft logosu kullanın. Kullanım Koşulları "Kurumsal hesap" gibi olmayan "iş hesabı" veya "kullanıcı karışıklık oluşturur Kurumsal hesap,".

## <a name="user-account-pictogram"></a>Kullanıcı hesabı piktogram

Önceki bir sürümde bu yönergelerin, "mavi rozeti" piktogram kullanılması önerilir. Kullanıcı ve geliştirici geri bildirimi doğrultusunda, şimdi ise Microsoft logosu kullanımını yerine öneririz. Microsoft logosu, kullanıcıların Office 365 veya diğer Microsoft ile iş Hizmetleri uygulamanıza oturum açmak için kullandıkları hesabı yeniden kullanabilir anlamasına yardımcı olur.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Kaydolma ve Azure AD'de oturum imzalama

Uygulamanıza kaydolma ve oturum açma için ayrı yollar sunabilir ve aşağıdaki bölümler her iki senaryo için görsel kılavuz sağlar.

**Uygulamanızın son kullanıcı (örneğin, deneme veya freemium modeline ücretsiz) kaydolma destekliyorsa**: göstermek bir **oturum** kendi iş hesabını veya kişisel hesapları uygulamanızla erişmesine olanak tanır düğmesi. Azure AD, uygulamanıza erişebilmek ilk kez bir onay istemi gösterir.

**Uygulamanızı yalnızca Yöneticiler onay verebilir izinleri gerektiriyorsa veya uygulamanızı Kurumsal lisanslama gerektiriyorsa**: yönetici alımdaki kullanıcı oturum açma ayırın. **"Bu uygulamayı Al" düğmesine** yöneticilerin oturum açın, sonra son kullanıcı onayı istemlerini uygulamanıza gizleme eklenen avantajına sahiptir, kuruluş içindeki kullanıcılar adına izin vermek için isteyin yönlendirir.

## <a name="visual-guidance-for-app-acquisition"></a>Uygulama alma için görsel kılavuz

"Uygulama Al" bağlantınız kullanıcı Azure AD erişim izni verme yeniden yönlendirmeniz gerekir (yetkilendirme) sayfasında, uygulamanızı Microsoft tarafından barındırılan kuruluş verilerine erişimi için yetkilendirmek üzere kuruluşun Yöneticisi izin vermek için. Erişim isteği hakkında ayrıntılar açıklanmıştır [uygulamaları Azure Active Directory ile tümleştirme](quickstart-v1-integrate-apps-with-azure-ad.md) makalesi.

Yöneticilerin uygulamanıza onay sonra bunlar için kullanıcı Office 365 uygulama Başlatıcısı deneyimini eklemeyi seçebilirsiniz (gelen ve giden waffle Menüsü'nden erişilebilir [ https://portal.office.com/myapps ](https://portal.office.com/myapps)). Bu özellik bildirmek istiyorsanız, bir düğme Göster ve "kuruluşunuz bu uygulama Ekle" gibi terimler kullanabilirsiniz aşağıdaki örnekteki gibi:

![Uygulama türleri ve senaryolar](./media/howto-add-branding-in-azure-ad-apps/add-to-my-org.png)

Ancak, düğmelerini kalmak yerine açıklayıcı metin yazma öneririz. Örneğin:

> *Office 365 veya Microsoft'un diğer iş hizmeti zaten kullanıyorsanız, < your_app_name > kuruluşunuzun verilerine erişim izni verebilirsiniz. Bu, var olan iş hesaplarıyla < your_app_name > kullanıcılarınızın olanak tanır.*

Uygulamanızda kullanmak için resmi Microsoft logosu yüklemek için bilgisayarınıza kaydedin ve kullanmak istediğiniz bir sağ tıklayın.

| Varlık                                | PNG biçimi | SVG biçimi |
| ------------------------------------ | ---------- | ---------- |
| Microsoft logosu  | ![Microsoft logosu PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_MSSymbol_19.png) | ![SVG Microsoft logosu](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_MSSymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Oturum açma için görsel kılavuz

Uygulamanız kullanıcıların Azure AD ile tümleştirmek için kullandığınız Protokolü karşılık gelen oturum açma uç noktaya yönlendiren bir oturum açma düğmesi görüntülemelidir. Aşağıdaki bölümde, bu düğmeyi gibi görünmelidir ayrıntılar sağlar.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogram ve "Microsoft ile oturum"

Microsoft logosu ilişkidir ve uygulamanızı Azure AD'ye diğer kimlik sağlayıcılardan arasında benzersiz olarak temsil eden "Sign in ile Microsoft" koşulları destekleyebilir. "Sign in ile Microsoft" yeterli alan yoksa, "Oturum açma." kısaltmak için Tamam Açık veya koyu renk şeması düğmelerini kullanabilirsiniz.

Aşağıdaki diyagramda, Microsoft tarafından önerilen varlıkları uygulamanızla kullanırken redlines gösterilmektedir. Redlines "Sign in ile Microsoft" veya "Oturum Aç" kısa sürümü için geçerlidir.

![Microsoft'ta oturum redlines](./media/howto-add-branding-in-azure-ad-apps/Sign-in-with-Microsoft-redlines.png)

Uygulamanızda resmi resimleri indirmek için bilgisayarınıza kaydedin ve kullanmak istediğiniz bir sağ tıklayın.

| Varlık                                | PNG biçimi | SVG biçimi |
| ------------------------------------ | ---------- | ---------- |
| Microsoft'ta (Koyu tema) oturum açın  | ![Düğme koyu tema PNG oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark.png) | ![Microsoft düğme koyu tema oturum SVG oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark.svg) |
| Microsoft'ta (açık tema) oturum açın | ![Düğme açık tema PNG oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light.png) | ![Microsoft düğme açık tema oturum SVG oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light.svg) |
| (Koyu tema) oturum açın                 | ![Düğme koyu tema PNG kısa oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark_short.png) | ![Düğme koyu tema SVG kısa oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark_short.svg) |
| (Açık tema) oturum açın                | ![Düğme açık tema PNG kısa oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light_short.png) | ![Düğme açık tema SVG kısa oturum](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light_short.svg) |


## <a name="branding-dos-and-donts"></a>İlgilenmenin markalama

**YAPMAK** "iş veya Okul hesabı" bunu kullanıp kullanamayacağını tanımak son kullanıcılara yardımcı olmak için ek açıklamayla "Sign in ile Microsoft" düğmesi ile birlikte kullanın. **Yoksa** "Kurumsal hesap", "iş hesabı" veya "Kurumsal hesap." gibi diğer terimleri kullanın

**Yoksa** "Office 365 kimliği" veya "Azure ID" kullanın Office 365 de bir Microsoft Azure AD kimlik doğrulaması için kullanmaz, sunan bir tüketici adıdır.

**Yoksa** Microsoft logosu değiştirin.

**Yoksa** Azure veya Active Directory markalar için son kullanıcıların kullanıma sunar. Ancak Tamam olarak bu kullanım koşullarını geliştiriciler, BT uzmanları ve yöneticiler ile kullanmak için.

## <a name="navigation-dos-and-donts"></a>Gezinti yapılması ve yapılmaması gerekenler

**YAPMAK** kullanıcıların oturumu kapatın ve başka bir kullanıcı hesabına geçmek bir yol sağlar. Çoğu kişi Microsoft/Facebook/Google/Twitter tek kişisel bir hesaptan olsa da, genellikle birden fazla kuruluşla ilişkili kişilerdir. Birden çok oturum açmış kullanıcılar için destek yakında sunulacaktır.
