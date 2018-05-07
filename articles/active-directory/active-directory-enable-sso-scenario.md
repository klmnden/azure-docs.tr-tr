---
title: Azure Active Directory ile uygulamaları yönetme | Microsoft Docs
description: Bu şirket içi, Bulut ve SaaS uygulamaları ile Azure Active Directory Tümleştirme avantajlarını makalesi.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 95b96f10-2d5c-4b78-8af8-d3657a24140f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: e05b2d515b997e769306146a5390d4d44fd5cf50
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="managing-applications-with-azure-active-directory"></a>Azure Active Directory ile uygulamaları yönetme
Gerçek iş akışı veya içeriği işletmeler tüm uygulamalar için iki temel gereksinimlere sahiptir:

1. Üretkenliği artırmak için uygulamaları bulmak ve erişmek kolay olmalıdır
2. Güvenlik ve idare etkinleştirmek için kuruluşun denetimi ve Gözetimi kimlerin ve her uygulamanın gerçekten erişiyor üzerinde gerekir

Bu en iyi sonucu elde edilebilir denetlemek için kimlik bilgileriniz kullanılarak bulut uygulamalarının dünyada "*KİMİN verilir ne*".

Terminolojisi bilgisayar:

* *Kimin* olarak bilinen *kimlik* -kullanıcıların ve grupların Yönetimi
* *Ne* olarak bilinen *erişim yönetimi* – korunan kaynaklara erişim yönetimi

Her iki bileşenin birlikte olarak da bilinir *kimlik ve erişim yönetimi (IAM)*, tarafından tanımlanan [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) grup olarak "*sağ sağ kaynaklara erişmek doğru kişiler sağlayan güvenlik uzmanlık kez sağ nedeniyle*".

Böylece sorunun ne olduğunu istiyor musunuz? IAM ise *yönetilmiyor* tümleşik bir çözüm ile tek bir yerde:

* Kimlik Yöneticiler sahip tek tek oluşturmak ve kullanıcı hesaplarını tüm uygulamalarda ayrı olarak güncelleştirmek bir yedek ve zaman alıcı etkinlik.
* Kullanıcıların çalışmak için gereksinim duydukları uygulamalara erişmek için birden çok kimlik bilgisi ezberleme gerekmez. Sonuç olarak, kullanıcılar parolalarını yazmak veya diğer veri güvenlik risklerini de beraberinde getirir diğer parola yönetimi çözümlerini kullanmak için eğilimi gösterir.
* Yedekli, uzun süren etkinlikler kullanıcılar miktarını azaltın ve yöneticiler işletmenizin alt çizgi artan iş etkinliklerini çalışıyoruz.

Bu nedenle, kuruluşlar tümleşik IAM çözümleri benimsenmesi gelen engeller genellikle ne?

* Birçok teknik çözümleri dağıtılması ve kendi uygulamaları için her bir kuruluş tarafından uyarlanan gerek yazılım platformlarının dayanır.
* Bulut uygulamalarını genellikle BT organizasyonu mevcut IAM çözümleriyle tümleştirilebilir daha yüksek bir hızda benimsenen.
* Güvenlik ve izleme araçları ek özelleştirme ve kapsamlı E2E senaryoları elde etmek için tümleştirme gerektirir.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory uygulamalarıyla tümleşik
Azure Active Directory, Microsoft Identity Service (Idaas) çözümü olarak şu:

* Bulut hizmeti olarak IAM sağlar 
* Merkezi erişim yönetimi, çoklu oturum açma (SSO) ve raporlama sağlar 
* Destekleyen tümleşik erişim yönetimi için [uygulamaları binlerce](https://azure.microsoft.com/marketplace/active-directory/) uygulama galerisinde Salesforce, Google Apps, kutusunda, Concur ve daha fazlası dahil olmak üzere. 

Azure Active Directory ile tüm uygulamalar, iş ortakları için yayımlayın ve müşteriler (iş veya tüketici) aynı kimliğe sahip ve erişim yönetimi özellikleri.<br> Bu, önemli ölçüde işletim maliyetlerini azaltmak sağlar.

Ne uygulama galerisinde henüz listelenmeyen bir uygulama uygulamanız gerekir? Bu uygulama Galerisi uygulamalardan için SSO yapılandırmaktan biraz daha uzun süren olmakla birlikte, Azure AD yapılandırmayla yardımcı olan bir sihirbaz sağlar.

Azure AD değeri "yalnızca" bulut uygulamalarını gider. Ayrıca, ile şirket içi uygulamalara güvenli uzaktan erişim sağlayarak kullanabilirsiniz. Güvenli uzaktan erişim ile ortadan kaldırabileceğiniz gereken VPN veya diğer geleneksel uzaktan erişim yönetimi uygulamaları için.

Merkezi erişim yönetimi ve çoklu oturum açma (SSO) için tüm uygulamalarınızı sağlayarak, Azure AD ana veri güvenliği ve üretkenlik soruna çözüm sağlar.

* Kullanıcılar daha uzun geliri bitti oluşturma veya iş işlemleri etkinlikleri vermiş üzerinde bir oturum ile birden çok uygulama erişebilir.
* Kimlik Yöneticiler tek bir yerde uygulamalara erişimi yönetebilir.

Şirket ve kullanıcı için avantajı açıktır. Bir daha yakından avantajları kimlik yönetici ve kuruluş için bakalım.

## <a name="integrated-application-benefits"></a>Tümleşik uygulama avantajları
SSO işleminin iki adımı vardır:

* Kimlik doğrulama, kullanıcının kimliğini doğrulama işlemi.
* Yetkilendirme, bir erişim ilkesi ile bir kaynağa erişimi etkinleştir veya engelleyecek şekilde karar.

Azure AD uygulamaları yönetmek ve SSO'yu etkinleştirmek için kullanırken:

* Kullanıcının şirket içi (örneğin, AD) veya Azure AD hesabının kimlik doğrulaması yapılır.
* Tutarlı bir son kullanıcı deneyimi sağlamak ve atama, konumları ve MFA koşullar iç yeteneklerini bağımsız olarak herhangi bir uygulama eklemek sağlayarak Azure AD atama ve koruma ilkesinde yetkilendirme yürütür.

Yetkilendirme hedef uygulamanın geçirilmeden şekilde nasıl uygulama Azure AD ile tümleşik bağlı olarak değişir anlamak önemlidir.

* **Uygulama hizmeti sağlayıcısı tarafından önceden tümleştirilmiş** gibi Office 365 ve Azure, bunlar doğrudan Azure AD'de oluşturulmuş ve kapsamlı kimlik ve erişim yönetimi yeteneklerini için ona bağlı olan uygulamalar. Bu uygulamalara erişim dizin bilgilerini ve belirteç verme aracılığıyla etkinleştirilir.
* **Uygulamalar önceden tümleştirilmiş Microsoft ve özel uygulamalar tarafından** bir iç uygulama dizini kullanır ve Azure AD bağımsız olarak çalışabilir bağımsız bulut uygulamaları şunlardır. Bir uygulama hesabına eşlenen bir uygulama belirli kimlik bilgileri vererek bu uygulamalara erişimi etkindir. Uygulama özellikleri bağlı olarak, bir Federasyon belirteç veya kullanıcı adı ve uygulamanın önceden hazırlanmış olan bir hesap için parola kimlik bilgisi olabilir.
* **Şirket içi uygulamalara** yayımlanan uygulamaları öncelikle şirket içi uygulamalara erişimi etkinleştirme Azure AD uygulama proxy'si aracılığıyla. Bu uygulamaları Windows Server Active Directory gibi bir merkezi şirket içi dizin kullanır. Bu uygulamalara erişim, şirket içi oturum açma gereksinimi uygularken sırasında son kullanıcıya uygulama içerik ulaştırmak için proxy tetikleme tarafından etkinleştirilir.

Örneğin, bir kullanıcı, kuruluşunuzun katılırsa, birincil oturum açma işlemleri için Azure AD'de kullanıcı için bir hesap oluşturmanız gerekir. Bu kullanıcı Salesforce gibi yönetilen bir uygulamaya erişim gerektiriyorsa, Salesforce'ta bu kullanıcı için bir hesap oluşturun ve iş SSO yapmak için Azure hesabınıza bağlamak gerekir. Kullanıcı kuruluşunuzdan ayrıldığında, Azure AD hesabının silmeniz önerilir ve IAM tüm karşılık gelen hesaplarında depolar kullanıcının erişebildiği uygulamalar.

## <a name="access-detection"></a>Erişim algılama
Modern kuruluşlarda, BT departmanları kullanılmakta olan tüm bulut uygulamaları tanımaz genellikle. Azure AD Cloud App Discovery ile birlikte, bu uygulamaları algılamak için bir çözüm sağlar.

## <a name="account-management"></a>Hesap yönetimi
Geleneksel olarak, çeşitli uygulamaları hesaplarını yönetme tarafından gerçekleştirilen elle yapılan bir işlemdir BT veya destek personeli kuruluşu. Azure AD hesap yönetimi tüm hizmet sağlayıcısı tümleşik uygulamalar ve otomatik kullanıcı sağlamayı veya SAML Just-In-Time sağlama destekleyen Microsoft tarafından önceden tümleştirilmiş uygulamaların tam olarak otomatikleştirir.

## <a name="automated-user-provisioning"></a>Otomatik kullanıcı hazırlama
Bazı uygulamalar Otomasyon arabirimleri oluşturma ve temizleme (veya devre dışı bırakma) hesaplarının sağlayın. Bir sağlayıcı böyle bir arabirim sunuyorsa, Azure AD tarafından yararlanır. Bu yönetim görevlerini otomatik olarak gerçekleşir, işletim maliyetlerini azaltır ve yetkisiz erişim olasılığını azaltır için ortamınızın güvenliğini artırır.

## <a name="access-management"></a>Erişim Yönetimi
Azure AD ile tek tek kullanan uygulamalar veya atamaları güdümlü kural erişimi yönetebilirsiniz. Ayrıca temsilci erişim en iyi gözetim sağlamaya ve Yardım Masası üzerindeki yükü azaltarak kuruluşun doğru kişilere yönetimi.

## <a name="on-premises-applications"></a>Şirket içi uygulamalar
Yerleşik uygulama proxy, bunun sonucunda, kullanıcılarınıza şirket içi uygulamalarınızı yayımlamak sağlar tutarlı hem Azure AD izleme, raporlama ve güvenlik özelliklerini modern bulut uygulama ve avantajları deneyim erişim.

## <a name="reporting-and-monitoring"></a>Raporlama ve izleme
Azure AD, önceden tümleştirilmiş raporlama ve izleme bilmeniz sağlayan özellikler ile uygulamalara ve bunlar aslında bunları kullanıldığında erişimi sağlar.

## <a name="related-capabilities"></a>İlgili özellikleri
Azure AD ile ayrıntılı erişim ilkeleri ve önceden tümleştirilmiş MFA uygulamalarınızla güvenliğini sağlayabilirsiniz. Azure MFA bakın hakkında daha fazla bilgi edinmek için [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Başlarken
Uygulamaları Azure AD ile tümleştirme başlamak için bir göz atalım [Azure Active Directory Tümleştirme alma uygulamalarla Başlangıç Kılavuzu](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [SaaS uygulamasına SSO için adım adım dağıtım planı](http://aka.ms/ssodeploymentplan)

