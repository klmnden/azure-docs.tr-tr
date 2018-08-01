---
title: Azure Active Directory ile uygulamaları yönetme | Microsoft Docs
description: Bu Azure Active Directory, şirket içi, Bulut ve SaaS uygulamalarını tümleştirme avantajlarını makalesi.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: bf53829a2d2578132f9a3595c0bac5e8eb588916
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366780"
---
# <a name="managing-applications-with-azure-active-directory"></a>Azure Active Directory ile uygulamaları yönetme
Gerçek iş akışı veya içerik, işletmelerin tüm uygulamalar için iki temel gereksinimlere sahiptir:

1. Üretkenliği artırmak için uygulamaları bulmak ve erişmek kolay olmalıdır
2. Güvenlik ve idare etkinleştirmek için kuruluşun denetimi ve Gözetimi kimlerin erişebileceğini ve kimin gerçekten her uygulamaya erişmeyi üzerinden gerekir

Bulut uygulamaları dünyasında, bu en iyi denetlemek için kimlik bilgileriniz kullanılarak gerçekleştirilebilir "*KİMLERİN ne için verilir*."

Bilgi işlem terminolojisini içinde:

* *Kimin* olarak da bilinen *kimlik* -kullanıcıların ve grupların Yönetimi
* *Hangi* olarak da bilinen *erişim yönetimi* – korumalı kaynaklara erişimi Yönetimi

Her iki bileşenin birlikte olarak bilinen *kimlik ve erişim yönetimi (IAM)*, tarafından tanımlanan [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) grubu olarak "*doğru kişilere sağlayan güvenlik uzmanlık alanı sağa doğru kaynaklarınıza erişmek kez doğru nedenlerle*".

Tamamdır, bu nedenle sorun nedir? IAM ise *yönetilmeyen* tümleşik bir çözüm ile tek bir yerde:

* Kimlik yöneticileri sahip ayrı ayrı oluşturup ayrı ayrı kullanıcı hesapları, tüm uygulamaları güncelleştirmek yedekli ve zaman alıcı bir etkinlik.
* Kullanıcıların çalışmak için ihtiyaç duydukları uygulamalara erişmek için birden çok kimlik bilgilerini ezberleme gerekmez. Sonuç olarak, kullanıcılar parolalarını yazma veya diğer parola yönetimi çözümü kullanma eğilimindedir. Diğer veri güvenlik riskleri bu alternatifleri sunar.
* Kullanıcılar artık, uzun süren etkinlikler azaltmak ve işletmenizin artırmak iş faaliyetlerinin ekiplerinde Yöneticiler ayırabilirsiniz.

Bu nedenle, kuruluşların tümleşik IAM çözümler benimseme öğesinden engeller genellikle ne?

* Dağıtılabilir ve kendi uygulamalar için her bir kuruluşa göre uyarlanmış gereken yazılım platformlarının en teknik çözümlerin dayanır.
* Bulut uygulamaları genellikle BT kuruluşu mevcut IAM çözümleriyle tümleştirerek daha yüksek bir hızda benimsenen.
* Güvenlik ve izleme araçları ek özelleştirme ve kapsamlı E2E senaryoları elde etmek için tümleştirme gerektirir.

## <a name="azure-active-directory-integrated-with-applications"></a>Uygulamaları Azure Active Directory tümleşik
Azure Active Directory, Microsoft'un kapsamlı kimlik (Idaas) hizmet çözümü olarak:

* Bir bulut hizmeti olarak IAM sağlar 
* Merkezi erişim yönetimi, çoklu oturum açma (SSO) ve raporlama sağlar 
* Destekleyen tümleşik access management için [uygulamaları binlerce](https://azure.microsoft.com/marketplace/active-directory/) Salesforce, Google Apps, kutusu, Concur ve daha fazla uygulama galerisinde dahil. 

Azure Active Directory ile tüm uygulamalar, iş ortakları için yayımlama ve müşteriler (işletme veya tüketici) aynı kimliğe sahip ve erişim yönetimi özellikleri.<br> Bu, önemli ölçüde işletim maliyetlerini azaltmak sağlar.

Peki uygulama galerisinde bulunan henüz listelenmeyen bir uygulama gerekiyor? Bu uygulama galerisinden uygulamaları için SSO yapılandırmaktan biraz daha uzun süren olmakla birlikte, Azure AD ile yapılandırma yardımcı olan bir sihirbaz sağlar.

Azure AD değerini "Yeni" bulut uygulamalarının ötesine gider. Bunu şirket içi uygulamalara güvenli uzaktan erişim sağlayarak kullanabilirsiniz. Güvenli uzaktan erişim VPN veya diğer geleneksel uzaktan erişim yönetim uygulamaları gereksinimini ortadan kaldırabilir.

Tüm uygulamalarınız için merkezi erişim yönetimi ve çoklu oturum açma (SSO) sağlayarak Azure AD ana veri güvenliği ve üretkenlik sorunların çözümü sağlar.

* Kullanıcılar, bir oluşturma veya iş operations etkinlikleri bitti geliri daha çok zaman ayırabilir ile oturum açma birden çok uygulama erişebilir.
* Kimlik Yöneticiler tek bir yerden uygulamalara erişimi yönetebilir.

Avantajı, şirket ve kullanıcı için açıktır. Şimdi daha yakından avantajları kimlik yönetici ve kuruluş için göz atın.

## <a name="integrated-application-benefits"></a>Tümleşik uygulama avantajları
SSO işleminin iki adımı vardır:

* Kimlik doğrulaması, kullanıcının kimlik doğrulama süreci.
* Yetkilendirme, bir erişim ilkesi ile bir kaynağa erişimi etkinleştirin veya engelleyecek şekilde karar.

Azure AD uygulamaları yönetmek ve SSO'yu etkinleştirmek için kullanırken:

* Kullanıcının şirket içi (örneğin, AD) veya Azure AD hesabı kimlik doğrulaması yapılır.
* Tutarlı bir son kullanıcı deneyimi sağlamaya ve böylece iç yeteneklerini bağımsız olarak herhangi bir uygulama ataması, konumları ve MFA koşullar eklemek Azure AD atama ve koruma ilkesinde yetkilendirme yürütür.

Uygulamanın Azure AD ile nasıl tümleştirildiğini bağlı olarak, hedef uygulamayı yetkilendirme geçirilmeden yolu değişir anlamak önemlidir.

* **Uygulamaları, hizmet sağlayıcısı tarafından önceden tümleştirilmiş** gibi Office 365 ve Azure sayesinde bunlar üzerinde kapsamlı, kimlik ve erişim yönetimi işlevleri için bağlı olan ve doğrudan Azure AD'de oluşturulmuş uygulamalar. Bu uygulamalara erişim, belirteç verme ile dizin bilgileri ile etkinleştirilir.
* **Uygulamalar önceden tümleşik Microsoft ve özel uygulamaları tarafından** üzerinde iç uygulama dizini kullanır ve Azure AD bağımsız olarak çalışabilir bağımsız bulut uygulamaları şunlardır. Bu uygulamalara erişim, bir uygulama eşlenmiş bir uygulamaya özgü kimlik bilgisi göndererek etkinleştirilir. Uygulama özelliklerine bağlı olarak, bir Federasyon belirteç veya kullanıcı adı ve parola uygulamada daha önce sağlanan bir hesabın kimlik bilgileri olabilir.
* **Şirket içi uygulamalara** öncelikle şirket içi uygulamalara erişimi etkinleştirerek Azure AD uygulama proxy'si aracılığıyla yayımlanan uygulamaları. Bu uygulama bir Windows Server Active Directory gibi merkezi şirket içi dizini kullanır. Bu uygulamalara erişim proxy tetikleyerek, şirket içi oturum açma gereksinimi uygularken sırasında uygulama içeriği son kullanıcıya teslim etmek için etkinleştirilir.

Örneğin, bir kullanıcı, kuruluşa katılan, birincil oturum açma işlemleri için Azure AD'de kullanıcı için bir hesap oluşturmanız gerekir. Bu kullanıcı, Salesforce gibi yönetilen bir uygulamaya erişim gerektiriyorsa, Salesforce'ta bu kullanıcı için bir hesap oluşturun ve SSO çalışır hale getirmek için Azure hesabına bağlamanız gerekir. Bir kullanıcı kuruluşunuzdan ayrıldığında, Azure AD hesabını silmek için önerilir ve IAM tüm karşılığı hesaplarında depolar uygulamaların kullanıcının erişebildiği.

## <a name="access-detection"></a>Erişim algılama
Modern kuruluşlarda, BT departmanlarının kullanılmakta olan tüm bulut uygulamaları farkında değildir. Azure AD bulut uygulaması bulma ile birlikte, bu uygulamaları algılamak için bir çözüm sağlar.

## <a name="account-management"></a>Hesap yönetimi
Geleneksel olarak, çeşitli uygulamalarda hesaplarını yönetme tarafından gerçekleştirilen el ile bir işlem olan BT veya destek personeli kuruluşta. Azure AD hesap yönetimi hizmeti sağlayıcılarının tümleşik uygulamaları ve otomatik kullanıcı hazırlama veya SAML tam zamanında sağlama destekleyen Microsoft tarafından önceden tümleştirilmiş uygulamalar arasında tam olarak otomatikleştirir.

## <a name="automated-user-provisioning"></a>Otomatik kullanıcı hazırlama
Bazı uygulamalar oluşturma ve kaldırma (veya devre dışı bırakma) hesabı için Otomasyon arabirimlerini sağlar. Bir sağlayıcı böyle bir arabirim sunar, Azure AD tarafından kullanılır. Bu, yönetim görevleri otomatik olarak gerçekleştirilmesi gerektiğinden, işletim maliyetlerini azaltır ve yetkisiz erişim olasılığını azaltır çünkü ortamınızın güvenliğini artırır.

## <a name="access-management"></a>Erişim Yönetimi
Azure AD ile tek tek kullanarak uygulamalara veya kural tabanlı atamalar erişimi yönetebilir. Ayrıca temsilci erişim en iyi gözetim sağlamaya ve Yardım Masası üzerindeki yükü azaltarak, kuruluşunuzdaki doğru kişilere yönetimi.

## <a name="on-premises-applications"></a>Şirket içi uygulamalar
Yerleşik uygulama proxy'si, bunun sonucunda, kullanıcılarınıza şirket içi uygulamalarınızı yayımlamanızı sağlar tutarlı bir hem Azure AD izleme, raporlama ve güvenlik özelliklerini modern bulut uygulaması ve avantajları deneyimiyle erişim.

## <a name="reporting-and-monitoring"></a>Raporlama ve izleme
Azure AD, önceden tümleştirilmiş raporlama ve izleme bilmenizi sağlayan özelliklerini kimlerin erişebildiğini uygulamalara ve bunlar gerçekten bunları kullanıldığında sağlar.

## <a name="related-capabilities"></a>Bununla ilgili özellikleri
Azure AD ile ayrıntılı erişim ilkeleri ve önceden tümleştirilmiş mfa'yı uygulamalarınızla güvenli hale getirebilirsiniz. Azure mfa'yı bakın hakkında daha fazla bilgi edinmek için [Azure mfa'yı](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Başlarken
Azure AD ile uygulamaları tümleştirme kullanmaya başlamak için göz atın [Azure Active Directory Tümleştirme alma uygulamaları ile çalışmaya başlama Kılavuzu](plan-an-application-integration.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Bir SaaS uygulamasına SSO için adım adım dağıtım planı](http://aka.ms/ssodeploymentplan)

