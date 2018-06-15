---
title: Azure Active Directory ile tümleştirme | Microsoft Docs
description: Bir kılavuz avantajları ve Azure Active Directory ile tümleştirme için kaynaklar.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: celested
ms.reviewer: bryanla
ms.custom: aaddev
ms.openlocfilehash: be7dec07597b0a82633d330a72274a94dbb9bf67
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34156053"
---
# <a name="integrating-with-azure-active-directory"></a>Azure Active Directory ile tümleştirme
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory, bulut uygulamaları için kurumsal düzeyde kimlik yönetimi kuruluşlarla sağlar. Azure AD tümleştirme kullanıcılarınızın kolaylaştırılmış bir oturum açma deneyimi sağlar ve BT ilkesine uygun uygulamanızı yardımcı olur.

## <a name="how-to-integrate"></a>Tümleştirme
Azure AD ile tümleştirmek, uygulamanız için birkaç yolu vardır. Birçok veya uygulamanız için uygun olduğu şekilde bu senaryoların az sayıda olarak yararlanın.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Uygulamanız için oturum açma için bir yol olarak Azure AD desteği
**Oturum açma uyuşmazlık ve destek maliyetlerini azaltmak.** Uygulamanıza oturum açmak için Azure AD kullanarak, kullanıcılarınızın daha fazla bir ada sahip olmaz ve parolayı unutmayın. Bir geliştirici olarak depolamak ve korumak için daha az bir parola gerekir. Unutulmuş parola sıfırlama işlemek olmaması önemli tasarruf tek başına olabilir. Azure AD oturum açma bazı Office 365 ve Microsoft Azure gibi dünyanın en popüler bulut uygulamaları için çalıştırır. Milyonlarca yüzlerce kuruluşlar milyonlarca kullanıcılardan, olasılığı olan kullanıcınız zaten oturum için Azure AD. Daha fazla bilgi edinmek [Azure AD oturum açma için destek ekleyen](active-directory-authentication-scenarios.md).

**Oturum, uygulamanız için yukarı basitleştirin.**  Böylece form kaydolma önceden doldurun veya tamamen ortadan kaydolma sırasında uygulamanız için Azure AD kullanıcı hakkındaki temel bilgileri gönderebilirsiniz. Kullanıcılar kendi Azure AD hesabının sosyal medya ve mobil uygulamaları bulunan benzer bir bilinen onayı deneyimi aracılığıyla kullanarak uygulamanız için kaydolabilirsiniz. Herhangi bir kullanıcı, kaydolma ve BT katılımı gerek kalmadan Azure AD ile tümleşik bir uygulama için oturum açın. Daha fazla bilgi edinmek [uygulamanızı Azure AD hesabının oturum açma için kaydolduğunuz](../../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Kullanıcılar için Gözat, kullanıcı sağlama yönetmek ve uygulamanıza erişimi denetleme
**Dizindeki kullanıcıları için göz atın.**  Arayın ve diğerleri davet zaman kendi kuruluşunuzdaki diğer kişilerin göz kullanıcılara yardımcı olmak için grafik API'sini kullanın veya bunları e-posta türü için gerektirmek yerine erişim verilmesi giderir. Kullanıcıların Kurumsal hiyerarşiye ayrıntılarını görüntüleme dahil olmak üzere bir bilinen adres defteri stili arabirimini kullanarak göz atabilirsiniz. Daha fazla bilgi edinmek [grafik API'si](active-directory-graph-api.md).

**Active Directory grupları ve dağıtım listeleri müşteri zaten yönetme yeniden kullanır.**  Azure AD müşteri zaten e-posta dağıtım için kullanarak ve erişim yönetimi gruplarını içerir. Grafik API'sini kullanarak, bu grupları oluşturmak ve uygulamanızı gruplarında ayrı bir dizi yönetmek müşteri gerektirmek yerine yeniden kullanın. Grup bilgileri, uygulamanıza oturum açma belirteçleri da gönderilebilir. Daha fazla bilgi edinmek [grafik API'si](active-directory-graph-api.md).

**Azure AD, uygulamanıza erişimi denetlemek için kullanın.**  Yöneticiler ve uygulama sahipleri Azure AD'de uygulamalar belirli kullanıcılar ve gruplar için erişimi atayabilirsiniz. Grafik API'sini kullanarak, bu liste okuyun ve sağlama ve kaynakları sağlamayı kaldırma özelliklerini denetlemek ve uygulamanızdaki erişmek için kullanın.

**Tabanlı erişim denetimi rolleri için Azure AD kullanın.**  Yöneticiler ve uygulama sahipleri kullanıcıların ve grupların Azure AD içinde uygulamanızı kaydederken tanımladığınız roller atayabilirsiniz. Rol bilgilerini oturum belirteçlerinde uygulamanızda gönderilir ve ayrıca grafik API'sini kullanarak okunabilir. Daha fazla bilgi edinmek [yetkilendirme için Azure AD kullanarak](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Kullanıcı profili, takvim, e-posta, kişiler, dosyaları ve daha fazla erişmek
**Azure AD, Office 365 ve diğer Microsoft iş Hizmetleri için yetkilendirme sunucusudur.**  Uygulamanızı veya OAuth 2.0 kullanan Azure AD kullanıcı hesapları için geçerli kullanıcı hesaplarınızı bağlama desteği için oturum açma için Azure AD destekliyorsa, okuma ve yazma erişimi bir kullanıcının profilini, takvim, e-posta, kişiler, dosyalara ve diğer bilgileri isteyebilir. Sorunsuz bir şekilde kullanıcının takvime yazma olayları ve da okumak veya kendi OneDrive dosyalarını yazabilirsiniz. Daha fazla bilgi edinmek [Office 365 API'leri erişme](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Uygulamanızı Azure ve Office 365 Pazar Yükselt
**Uygulamanızı Azure AD zaten kullanan kuruluşlar milyonlarca yükseltin.**  Arama ve bu Pazar Gözat kullanıcılar bir zaten kullanıyor veya Bulut hizmeti müşteriler bunları yaparak daha fazla bulut hizmeti yetkili. Uygulamanızda yükseltme hakkında daha fazla bilgi [Azure Marketi](https://azure.microsoft.com/marketplace/partner-program/).

**Kullanıcılar, uygulamanız için kaydolduğunuzda, kendi Azure AD erişim paneli ve Office 365 uygulama Başlatıcı görünür.**  Kullanıcıların kullanıcı etkileşimini geliştirme hızla ve kolayca daha sonra uygulamanızı iade etme olanağınız olur. Daha fazla bilgi edinmek [Azure AD erişim paneli](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Aygıt hizmeti ve hizmet iletişimin güvenliğini sağlama
**Azure AD Kimlik Yönetimi Hizmetleri ve cihazların yazmanız gereken kod azaltır için kullanarak ve BT'nin sağlar erişimi yönetmek üzere.**  Hizmetler ve cihazlar, OAuth kullanan Azure AD'den belirteçleri almak ve web API'leri erişmek için bu simgeleri kullanın. Azure AD kullanarak karmaşık kimlik doğrulama kodu yazma önleyebilirsiniz. Azure AD içinde cihazları ve Hizmetleri kimlikleri depolandığından BT anahtarları ve bunu ayrı olarak uygulamanızda yapmak zorunda kalmak yerine tek bir yerde iptal yönetebilirsiniz.

## <a name="benefits-of-integration"></a>Tümleştirme yararları
Azure AD ile tümleştirme ek kod yazmanız gerekmez yararları ile birlikte gelir.

### <a name="integration-with-enterprise-identity-management"></a>Kurumsal kimlik yönetimi ile tümleştirme
**BT ilkelerine uygun uygulamanızı yardımcı olur.**  Kuruluşlar, Kurumsal kimlik yönetimi sistemlerini Azure AD ile tümleştirmek bir kişi bir kuruluş ayrıldığında, otomatik olarak erişim uygulamanıza kaybederler için ek adımlar gerek BT. BT, uygulamanızın erişebilecek ve hangi erişim ilkeleri - örnek çok faktörlü kimlik doğrulama - karmaşık şirket ilkeleriyle uyumlu için kod yazma, gereksinimini azaltır gerektiğini belirlemek yönetebilirsiniz. Azure AD uygulamanıza nedenle açtığınız için kimin, ayrıntılı denetim günlüğü yöneticilerine sağlar BT kullanımını izleyebilirsiniz.

**Böylece uygulamanız AD ile tümleştirebilirsiniz azure AD bulut için Active Directory genişletir.**  Birçok kuruluş dünyanın kendi asıl oturum açma ve kimlik yönetimi sistemi Active Directory'yi kullanmak ve AD ile çalışmak üzere uygulamalarını gerektirir. Azure AD ile tümleştirme, uygulamanızın Active Directory ile tümleştirir.

### <a name="advanced-security-features"></a>Gelişmiş güvenlik özellikleri
**Çok faktörlü kimlik doğrulaması.**  Azure AD yerel çok faktörlü kimlik doğrulaması sağlar. Bu destek kendiniz kod gerekmez böylece BT yöneticileri, uygulamanızın erişmek için çok faktörlü kimlik doğrulaması gerektirebilir. Daha fazla bilgi edinmek [çok faktörlü kimlik doğrulaması](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Anormal oturum açma algılama.**  Azure AD bir milyardan fazla oturum açma işlemleri şüpheli etkinlikleri algılamak ve BT yöneticileri olası sorunları bildirmek için machine learning algoritmaları kullanılırken günde işler. Azure AD oturum açma destekleyerek, uygulamanız bu koruma yararı alır. Daha fazla bilgi edinmek [Azure Active Directory erişimi raporu görüntüleme](../active-directory-view-access-usage-reports.md).

**Koşullu erişim.**  Çok faktörlü kimlik doğrulaması ek olarak, yöneticiler gerektirebilir belirli koşullar kullanıcılar oturum uygulamanıza açma önce. Ayarlanabilir koşullar istemci aygıtları, belirtilen gruplarının üyeliği ve erişim için kullanılan aygıt durumunu IP adres aralığı içerir. Daha fazla bilgi edinmek [Azure Active Directory koşullu erişim](../active-directory-conditional-access-azure-portal.md).

### <a name="easy-development"></a>Kolay geliştirme
**Endüstri standardı protokoller.**  Microsoft destek endüstri standartları taahhüt eder. Azure AD SAML 2.0, Openıd Connect 1.0, OAuth 2.0 ve WS-Federasyon 1.2 kimlik doğrulama protokollerini destekler. Grafik API'si OData 4.0 uyumlu değil. Uygulama zaten federe oturum açma için SAML 2.0 veya Openıd Connect 1.0 protokollerini destekliyorsa, Azure AD için destek eklenmesi basit olabilir. Daha fazla bilgi edinmek [Azure AD kimlik doğrulama protokolleri desteklenen](active-directory-authentication-protocols.md).

**Açık kaynak kitaplıkları.**  Microsoft, popüler diller ve platformlar için hızlı geliştirme için tam olarak desteklenen açık kaynak kitaplıkları sağlar. Kaynak kodu Apache 2.0 altında lisanslanmıştır ve çatallaştırma ve geri projelerine katkıda ücretsizdir. Daha fazla bilgi edinmek [Azure AD kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Dünya çapında varlığı ve yüksek kullanılabilirlik
**Azure AD dünyanın veri merkezlerinde dağıtılır ve yönetilmesi ve aralıksız izlenen.**  Azure AD Microsoft Azure ve Office 365 için Kimlik Yönetimi sistemidir ve dünyanın 28 veri merkezlerinde dağıtılır. Dizin verilerini en az üç veri merkezlerini çoğaltılacak garanti edilmez. Genel yük dengeleyicileri en yakın kopyalanması verilerini içeren Azure AD ile kullanıcılara erişim sağlamak ve bir sorun algılandığında, otomatik olarak diğer veri merkezleri isteklerini yeniden yönlendirmek.

## <a name="next-steps"></a>Sonraki Adımlar
[Kod yazmaya başlamak](active-directory-developers-guide.md#get-started).

[Azure AD kullanarak kullanıcıların oturumunu](active-directory-authentication-scenarios.md)

