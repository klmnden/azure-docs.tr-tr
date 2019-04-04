---
title: Azure Active Directory ile tümleştirme | Microsoft Docs
description: Avantajları ve kaynakları Azure Active Directory ile tümleştirme için bir kılavuz.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/04/2019
ms.author: celested
ms.reviewer: bryanla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 687b2848dc4bcf4e0c8935795eb66e07c3a5a7bd
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894636"
---
# <a name="integrating-with-azure-active-directory"></a>Azure Active Directory ile tümleştirme

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD), bulut uygulamalarınız için kurumsal düzeyde kimlik yönetimi ile kuruluşlar sağlar. Azure AD tümleştirmesi, kullanıcılarınızın kolaylaştırılmış bir oturum açma deneyimi sağlar ve uygulamanızın BT ilkesine uygun yardımcı olur.

## <a name="how-to-integrate"></a>Nasıl tümleştirilir

Uygulamanızın Azure AD ile tümleştirmek birkaç yol vardır. Birçok veya uygulamanız için uygun olduğu gibi Bu senaryolardan birkaç olarak avantajlarından yararlanın.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Azure AD uygulamanız için oturum açmak için bir yol olarak desteği

**Oturum açma uyuşmazlıkları azaltın ve destek maliyetlerini azaltır.** Uygulamanıza oturum açmak için Azure AD'yi kullanarak, kullanıcılarınızın bir daha fazla ad olmaz ve parolayı unutmayın. Bir geliştirici olarak depolamak ve korumak için daha az bir parola gerekir. Unutulmuş parola sıfırlama işlemlerini işlemek zorunda değil, tek başına bir önemli ölçüde tasarruf olabilir. Azure AD oturum açma bazı Office 365 ve Microsoft Azure dahil olmak üzere dünyanın en popüler bulut uygulamaları için güçlendirir. Yüz milyonlarca ile kullanıcılara kuruluşları milyonlarca olasılığı olan kullanıcı zaten işaretli Azure AD'ye. Daha fazla bilgi edinin [Azure AD oturum açma desteği eklendi](authentication-scenarios.md).

**Oturum, uygulamanız için yedekleme basitleştirin.**  Böylece form kaydolma önceden doldurun veya tamamen ortadan kaydolma sırasında uygulamanız için Azure AD kullanıcı hakkındaki temel bilgileri gönderebilirsiniz. Kullanıcıların uygulamanızı sosyal medya ve mobil uygulamalarda bulunan benzer bir tanıdık onayı deneyimi aracılığıyla kendi Azure AD hesabını kullanarak kaydolabilirsiniz. Herhangi bir kullanıcı, kaydolma ve BT'ye gerek kalmadan Azure AD ile tümleştirilmiş bir uygulama için oturum açın. Daha fazla bilgi edinin [uygulamanız Azure AD hesap oturum açma için imzalama-yukarı](../../app-service/configure-authentication-provider-aad.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Kullanıcılar için göz, kullanıcı sağlamayı yönetme ve uygulamanıza erişimi denetleme

**Dizininde yer alan kullanıcılar için göz atın.**  Arama ve diğer davet ettiğinizde kuruluşlarındaki diğer kişiler için Gözat kullanıcılara yardımcı olmak için Graph API'sini kullanın veya e-posta türü için gerek yerine erişim verme yöneliktir. Kullanıcılar, kuruluş hiyerarşisini ayrıntılarını görüntüleme dahil olmak üzere bir tanıdık adres defteri stili arabirimini kullanarak göz atabilirsiniz. Daha fazla bilgi edinin [Graph API'si](active-directory-graph-api.md).

**Active Directory grupları ve dağıtım listeleri müşteri zaten yönetiyor yeniden kullanın.**  Azure AD, müşteri zaten e-posta dağıtım için kullanarak ve erişim yönetimi grupları içerir. Graph API'sini kullanarak, bu grupları oluşturmak ve uygulamanızı gruplarında ayrı bir dizi yönetmek, müşteri göstermek zorunda kalmadan yeniden kullanın. Grup bilgileri uygulamanıza oturum açma belirteçleri de da gönderilebilir. Daha fazla bilgi edinin [Graph API'si](active-directory-graph-api.md).

**Azure AD uygulama erişimi denetlemek için kullanın.**  Yöneticileri ve Azure AD'de uygulama sahipleri erişimi belirli kullanıcılara ve gruplara uygulamaları atayabilirsiniz. Graph API'sini kullanarak, bu liste okuyun ve hazırlama ve sağlamayı kaynakları denetlemek ve uygulamanızın içinden erişmek için kullanın.

**Tabanlı erişim denetimi rolleri için Azure AD kullanın.**  Yöneticiler ve uygulama sahipleri kullanıcıları ve grupları Azure AD'de uygulamanızı kaydettiğinizde, tanımladığınız rolleri atayabilirsiniz. Rol bilgilerini uygulamanıza oturum açma belirteçleri de gönderilir ve Graph API'si kullanılarak da okunabilir. Daha fazla bilgi edinin [yetkilendirme için Azure AD kullanarak](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles/).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Kullanıcının profil, takvim, e-posta, kişiler, dosyalar ve daha fazla erişin

**Azure AD Office 365 ve diğer Microsoft hizmetlerini yetkilendirme sunucusudur.**  Azure AD uygulamanız veya OAuth 2.0 kullanarak Azure AD kullanıcı hesapları için geçerli kullanıcı hesaplarınızı bağlama desteği için oturum açma desteği, okuma ve yazma erişimi için bir kullanıcının profilini, takvim, e-posta, kişiler, dosyalar ve diğer bilgileri isteyebilir. Sorunsuz bir şekilde kullanıcının takvime olayları yazma ve da okuma veya kendi OneDrive dosyalarını yazma. Daha fazla bilgi edinin [Office 365 API'lerine erişme](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Azure ve Office 365 Marketlerden uygulamanızda Yükselt

**Zaten Azure AD kullanan kuruluşlar milyonlarca uygulamanızı tanıtın.**  Arama ve bu marketlerden Gözat kullanıcılar zaten kullanıyorsanız veya yönetilmelerini daha fazla bulut Hizmetleri, bulut hizmet müşterileri tam ad. Uygulamanızı yükseltme hakkında daha fazla bilgi [Azure Marketi'nde](https://azure.microsoft.com/marketplace/partner-program/).

**Uygulamanız için kullanıcıların oturum açarken Azure AD erişim panelinde ve Office 365 uygulama Başlatıcısı içinde görünür.**  Kullanıcı etkileşimini iyileştirme kullanıcılar daha sonra uygulamanızı hızlı ve kolay bir şekilde geri dönmek mümkün olacaktır. Daha fazla bilgi edinin [Azure AD erişim paneli](../user-help/active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Cihazın hizmete ve hizmetten hizmete iletişimi güvenli hale getirme

**Azure AD Kimlik Yönetimi, hizmet ve cihazları yazmanız gereken kodu azaltır için kullanarak ve BT'nin erişimi yönetmek için.**  Hizmetler ve cihazlar, Azure AD kullanarak OAuth belirteçlerini almak ve web API'lerine erişmek için bu belirteçleri kullanın. Azure AD kullanarak karmaşık kimlik doğrulama kodu yazarken önleyebilirsiniz. Azure AD'de cihazları ve Hizmetleri kimliklerini depolandığından BT anahtarları ve iptal bunu ayrı olarak uygulamanızda yapmak zorunda kalmak yerine tek bir yerden yönetebilirsiniz.

## <a name="benefits-of-integration"></a>Tümleştirmesinin avantajları

Azure AD ile tümleştirme, ek kod yazmayı gerektirmeyen avantaj sunar.

### <a name="integration-with-enterprise-identity-management"></a>Kurumsal kimlik yönetimi ile tümleştirme

**Uygulamanızı BT ilkeleriyle uyumlu yardımcı olur.**  Kuruluşlar, Azure AD ile Kurumsal kimlik yönetimi sistemlerini tümleştirin bir kişi bir kuruluş ayrıldığında, otomatik olarak uygulamanıza kaybederler için ek adımlar uygulaması gerek kalmadan BT. BT, uygulamanızın erişebilecek ve hangi erişim ilkeleri - örnek çok faktörlü kimlik doğrulaması - karmaşık şirket ilkeleriyle uyumlu için kod yazma gereksiniminizi azaltır gerektiğini belirlemek yönetebilirsiniz. Azure AD ile uygulamanıza şekilde açan için olan, bir ayrıntılı denetim günlüğü yöneticilere BT kullanımını izleyebilirsiniz.

**Uygulamanızı AD ile tümleştirilebilir. böylece azure AD Active Directory buluta genişletir.**  Dünyanın birçok kuruluşun kendi asıl oturum açma ve kimlik yönetimi sistemi Active Directory kullanın ve AD ile çalışmaya uygulamalarının gerektirdiği. Uygulamanızı Azure AD ile tümleştirme Active Directory ile tümleşir.

### <a name="advanced-security-features"></a>İleri düzey güvenlik özellikleri

**Çok öğeli kimlik doğrulama.**  Azure AD yerel çok faktörlü kimlik doğrulaması sağlar. Böylece bu desteğin kodunu kendiniz gerekmez, BT yöneticileri uygulamanıza erişim için multi-Factor authentication isteyebilir. Daha fazla bilgi edinin [multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Anormal oturum açma algılama.**  Azure AD, şüpheli etkinlikleri algılamak ve BT yöneticileri olası sorunları bildirmek için makine öğrenimi algoritmaları kullanırken, günde bir milyardan fazla oturum açma işlemlerini işler. Azure AD oturum açma destekleyerek, uygulamanız bu koruma avantajı alır. Daha fazla bilgi edinin [Azure Active Directory erişim raporu görüntüleyen](../active-directory-view-access-usage-reports.md).

**Koşullu erişim.**  Çok faktörlü kimlik doğrulamasına ek olarak, yöneticiler gerektirebilir belirli koşullar onlara kullanıcıların uygulamanıza oturum önce. Ayarlanabilir koşullar istemci cihazlar, belirtilen grup üyeliği ve erişim için kullanılan cihaz durumu IP adres aralığı içerir. Daha fazla bilgi edinin [Azure Active Directory koşullu erişim](../active-directory-conditional-access-azure-portal.md).

### <a name="easy-development"></a>Kolay geliştirme

**Endüstri standardı protokoller.**  Microsoft, sektör standartları desteklemekte kararlıdır. Microsoft kimlik platformu endüstri standardı OAuth 2.0 ve Openıd Connect 1.0 protokollerini destekler. Daha fazla bilgi edinin [Microsoft kimlik platformu kimlik doğrulama protokolleri](active-directory-v2-protocols.md).

**Açık kaynak kitaplıkları.**  Microsoft, popüler diller ve platformlar için geliştirme sürecini hızlandırın için tam olarak desteklenen açık kaynak kitaplıkları sağlar. Kaynak kodu Apache 2.0 altında lisanslanmıştır ve çatalını oluşturmanız ve projelere katkıda bulunmak ücretsizdir. Daha fazla bilgi edinin [Microsoft Authentication Library (MSAL)](reference-v2-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Dünya çapında varlık ve yüksek kullanılabilirlik

**Azure AD dünyanın dört bir yanındaki veri merkezlerinde dağıtılır ve yönetilir ve sistemlerimizdeki izlenir.**  Azure AD, Microsoft Azure ve Office 365 için Kimlik Yönetimi sistemidir ve dünyanın dört bir yanındaki 28 veri merkezlerinde dağıtılır. Dizin verilerini en az üç veri merkezlerine çoğaltılması garanti edilir. Genel load balancer'ları en yakın kopyasını verilerini içeren Azure AD kullanıcıların erişim sağlamak ve bir sorun algılandığında, otomatik olarak diğer veri merkezlerine istekleri yeniden yönlendirme.

## <a name="next-steps"></a>Sonraki adımlar

[Kod yazmaya başlama](v2-overview.md#getting-started).

[Microsoft kimlik platformu kullanarak kullanıcıların oturumunu](authentication-scenarios.md)

