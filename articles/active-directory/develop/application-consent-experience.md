---
title: Azure AD uygulama onay anlama deneyimleri | Microsoft Docs
description: Azure AD hakkında daha fazla onayı deneyimlerinde, yönetme ve Azure AD uygulamaları geliştirirken nasıl kullanabileceğinizi görmek için bilgi edinin
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: ryanwi
ms.reviewer: zachowd
ms.collection: M365-identity-device-management
ms.openlocfilehash: d71bfd5e560bb1509337ac371fbe101b4c6d63b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540635"
---
# <a name="understanding-azure-ad-application-consent-experiences"></a>Azure AD uygulama onayı deneyimlerini anlama

Kullanıcı deneyimi onay Azure Active Directory (Azure AD) uygulama hakkında daha fazla bilgi edinin. Bu nedenle, akıllı bir şekilde kuruluşunuz için uygulamaları yönetme ve/veya daha sorunsuz bir onayı deneyimi içeren uygulamalar geliştirin.

## <a name="consent-and-permissions"></a>İzni ve izinleri

İzin verme yetkilendirme gerçekleştirilemeyeceğine ilişkin korunan kaynaklara erişim için bir uygulama için bir kullanıcının işlemidir. Bir yönetici veya kullanıcı, kuruluşun/tek verilerine erişmesine izin vermek onay sorulabilir.

Onay verme gerçek kullanıcı deneyimi kullanıcının Kiracı, kullanıcının kapsamını yetkilisi (veya rol) ve türünü ayarlama ilkelerine bağlı olarak farklılık gösterecektir [izinleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-permissions) istemci uygulama tarafından istenen. Bu, uygulama geliştiriciler ve Kiracı yöneticileri onayı deneyimi üzerinde bazı denetim olduğu anlamına gelir. Yöneticileri ayarlama ve bir kiracı veya uygulama onay deneyiminde, kiracılarının denetlemek için ilkeler devre dışı bırakma esnekliğine sahipsiniz. Hangi tür izinler isteyen uygulama geliştiricileri okuyabilirsiniz ve kullanıcı kullanıcılara kılavuzluk isteyip istemediğiniz akış veya yönetici onayı akışı onay vermiş olursunuz.

- **Kullanıcı onay akışı** ise bir uygulama geliştiricisi amacıyla yalnızca geçerli kullanıcının kayıt onayı için kullanıcıları yetkilendirme uç noktasına yönlendirir.
- **Yönetici onay akışı** yönetici kullanıcılar onay uç noktası için tüm Kiracı kayıt onayı ıntent'e ile bir uygulama geliştiricisi yönlendirir andır. Yönetici onay akışı düzgün çalıştığından emin olmak için uygulama geliştiriciler tüm izinleri listelemek `RequiredResourceAccess` uygulama bildiriminde özellik. Daha fazla bilgi için bkz. [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest).

## <a name="building-blocks-of-the-consent-prompt"></a>Onay istemi yapı taşları

Onay istemi kullanıcıların kendileri adına korunan kaynaklara erişim için istemci uygulaması güven belirlemek için yeterli bilgiye sahip olmak için tasarlanmıştır. Yapı taşlarını anlamak, geliştiricilerin daha iyi kullanıcı deneyimleri oluşturmasına yardımcı olur ve daha fazla kararları hakkında bilgi sahibi olmak onay yapma verme kullanıcılar yardımcı olur.

Aşağıdaki tablo ve diyagram onay istemi yapı taşları hakkında bilgi sağlar.

![Onay istemi yapı taşları](./media/application-consent-experience/consent_prompt.png)

| # | Bileşen | Amaç |
| ----- | ----- | ----- |
| 1 | Kullanıcı tanımlayıcısı | Bu tanımlayıcı, istemci uygulaması adına korunan kaynaklara erişim isteyen kullanıcıyı temsil eder. |
| 2 | Unvan | Kullanıcıların kullanıcı veya yönetici onay akışını olup oluşturacağımızı tabanlı başlık değişiklikler. Yönetici onayı akışı başlığı ek bir satırı "Kuruluşunuz için kabul et" olsa kullanıcı onay akışını, başlığı "İzinleri istenen" olacaktır. |
| 3 | Uygulama logosu | Bu görüntü görsel bir ipucu sahip kullanıcılara yardımcı olmalıdır olup bu uygulamayı uygulama bunlar erişmek tasarlanmıştır. Bu görüntü, uygulama geliştiriciler tarafından sağlanır ve bu görüntüyü sahipliğini doğrulanmış değil. |
| 4 | Uygulama adı | Bu değer, hangi uygulama verilerine erişim isteyen kullanıcılara bildirin. Bu ad, geliştiriciler tarafından sağlanır ve bu uygulama adı sahipliğini doğrulanmış değil unutmayın. |
| 5 | Yayımcı etki alanı | Bu değer için güvenilirliği değerlendirilmesi mümkün olabilir bir etki alanı kullanıcılarıyla sağlamanız gerekir. Bu etki alanı, geliştiriciler tarafından sağlanır ve bu yayımcı etki alanı sahipliği doğrulandı. |
| 6 | İzinler | Bu liste, istemci uygulama tarafından istenen izinleri içerir. Kullanıcıların her zaman istemci uygulaması, kabul ederseniz kendi adınıza erişmek için yetkili verileri anlamak için istenen izinleri türlerini değerlendirmelidir. Bir uygulama geliştiricisi olarak erişim için en az ayrıcalığa sahip izinleri istemek için en iyisidir. |
| 7 | İzin açıklaması | Bu değer, izinleri doğrulamasıyla açığa çıkaran hizmet tarafından sağlanır. İzin açıklamaları görmek için izin yanındaki köşeli çift Ayraca geçiş gerekir. |
| 8 | Uygulama koşulları | Bu terimler, hizmet ve gizlilik bildirimi uygulamanın koşullarını bağlantılar içerir. Yayımcı, kendi service şartlarında kendi kurallara anahat oluşturma için sorumludur. Ayrıca, yayımcı kullanın ve kendi gizlilik bildirimi kullanıcı verilerini paylaşma biçimini açığa çıkarmaktan için sorumludur. Yayımcı, çok kiracılı uygulamalar için bu değerleri bağlantılar sağlamıyorsa bir onay istemi kalın uyarıyı olacaktır. |
| 9 | https://myapps.microsoft.com | Bu kullanıcılar burada gözden geçirin ve şu anda verilerine erişimi olan herhangi bir Microsoft dışı uygulamaları Kaldır bağlantıdır. |

## <a name="common-consent-scenarios"></a>Onay senaryoları

Bir kullanıcı ortak onay senaryolarda görebilirsiniz onay deneyimleri şunlardır:

1. Kullanıcıya'na yönlendirmez bir uygulamaya erişmeden kişiler, kendi yetkilisi kapsamında olmayan bir izin kümesi gerektiren sırasında akışı vermiş olursunuz.
    
    1. Yöneticileri, ek bir denetim üzerinde bunları tüm Kiracı adına onay olanak tanıyan geleneksel onay istemi görürsünüz. Kapalı olarak bunu ne zaman yöneticileri açıkça kutusu onayı denetleyin, tüm Kiracı adına verilmesi yalnızca denetim olarak ayarlanacak. Bugünden itibaren bulut yönetimi ve uygulama yönetici bu onay kutusunu görmezsiniz şekilde bu onay kutusu genel yönetici rolü için yalnızca gösterir.

        ![Senaryo 1a için onay istemi](./media/application-consent-experience/consent_prompt_1a.png)
    
    2. Kullanıcılar, geleneksel bir onay istemi görürsünüz.

        ![Senaryo 1b için onay istemi](./media/application-consent-experience/consent_prompt_1b.png)

2. Bunların yetki kapsamı dışında olan en az bir izin gerektiren bir uygulama erişen kişiler.
    1. Yöneticiler, yukarıda gösterilen 1.i olarak aynı istemi görürsünüz.
    2. Kullanıcılar uygulamanıza onay verme gelen engellenir ve uygulamaya erişim için kullanıcıların yönetici istemeniz söylenebilir. 
                
        ![Senaryo 1b için onay istemi](./media/application-consent-experience/consent_prompt_2b.png)

3. Gidin veya yönetici yönlendirilmiş kişiler flow onay vermiş olursunuz.
    1. Yönetici kullanıcılar, yönetici onay istemi görürsünüz. Başlık ve tüm Kiracı adına Bu istem kabul uygulama vereceği olgu erişmek için istenen veri değişiklikleri vurgulama bu isteminde değiştirilmiş izin açıklamaları.
        
        ![Senaryo 1b için onay istemi](./media/application-consent-experience/consent_prompt_3a.png)
        
    1. Yönetici olmayan kullanıcılar, yukarıda gösterilen 2 II olarak aynı ekran görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
- Adım adım bir bakış elde [Azure AD'ye onay çerçevesine onayı nasıl uyguladığını](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
- Daha fazla ayrıntı için bilgi [çok kiracılı uygulama onay çerçevesine nasıl kullanabileceğinizi](active-directory-devhowto-multi-tenant-overview.md) "kullanıcı" ve "Yönetici" onayı, daha fazla destek uygulamak için çok katmanlı uygulama desenleri Gelişmiş.
- Bilgi [uygulamanın yayımcı etki alanı yapılandırma](howto-configure-publisher-domain.md).
