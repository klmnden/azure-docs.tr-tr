---
title: Azure Active Directory (Azure AD) nedir? | Microsoft Docs
description: Mevcut şirket içi kimliklerinizi buluta genişletmek veya Azure AD tümleşik uygulamalar geliştirmek için Azure Active Directory'yi kullanır.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 003ce2edda3e2069eb7e05f58ecc2e208c818946
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?
Azure Active Directory (Azure AD), Microsoft'un çok kiracılı, bulut tabanlı dizin ve çekirdek Dizin Hizmetleri, uygulama erişim yönetimi ve tek bir çözüm içine kimlik koruması birleştirir Identity management hizmeti. Ayrıca, Azure AD erişim denetimi merkezi ilke ve kural göre uygulamalarını sunmak geliştiricilere sağlayan zengin, standartlara dayalı platformu sunar.

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

- **BT yöneticileri için.** Azure AD daha güçlü kimlik yönetimi ve çoklu oturum açma (SSO) erişimi binlerce kullanarak kuruluşunuz için daha güvenli bir çözüm sağlar [bulut tabanlı SaaS uygulamaları](active-directory-saas-tutorial-list.md) ve şirket içi uygulamalar. Bu uygulamaları de bulut tabanlı bir uygulamaya güvenlik, sorunsuz erişim, Gelişmiş İşbirliği ve Otomasyon kimlik yaşam döngüsünün Çalışanlarınız için güvenlik ve uyumluluk artırmaya yardımcı olur elde edersiniz.

    İle ek olarak, yalnızca [dört tıklama](./connect/active-directory-aadconnect-get-started-express.md), Azure AD mevcut bir Windows Server Active Directory ile tümleştirme, mevcut şirket içi kullanın, kuruluşunuzun izin vererek bulut tabanlı SaaS uygulamasını yönetmek için kimlik Yatırımlar erişim .

- **Uygulama geliştiriciler için.** Kuruluşların tüm dünyada milyonlarca tarafından kullanılan bir kimlik yönetimi çözümü ile tümleştirmenize olanak tanır tarafından uygulamalarınızı oluşturmaya odaklanmanıza azure AD olanak sağlar.

- **Office 365, Azure veya Dynamics CRM Online müşteriler için.** Azure AD zaten kullanmakta olduğunuz. Her Office 365, Azure ve Dynamics CRM Online Kiracı tümleşik bulut uygulamalarınız için çalışan erişimi yönetmek üzere hemen başlatmak izin vererek gerçekten Azure AD kiracısı, ' dir.

## <a name="how-reliable-is-azure-ad"></a>Azure AD nasıl güvenilir mi?
Çok kiracılı, coğrafi olarak dağıtılan, yüksek kullanılabilirlik tasarımı, buna yönelik en kritik işletme ihtiyaçlarınıza bağlı Azure AD anlamına gelir. Tüm dünyada 28 veri merkezleri otomatik yük devretme kümelemesiyle azalıyor, Azure AD yüksek oranda güvenilir ve başka bir veri merkezi dizininize en az iki daha bölgesel veri merkezleri dağınık verileri canlı bir kopyasını aşağı kalırsa bile ve anında erişim için kullanılabilir olduğunu bilmesinin rahatlık sahip olacaksınız.

Hizmet düzeyi sözleşmelerine hakkında daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Sürüm seçin
Tüm Microsoft Online iş Hizmetleri, oturum açma için Azure Active Directory'ye (Azure AD) bağlı ve diğer kimlik gerekiyor. Herhangi bir Microsoft Online iş Hizmetleri (örneğin, Office 365 veya Microsoft Azure) için abone olursanız, Azure AD ücretsiz özelliklerin tümü, erişimi ile alın. Azure Active Directory ücretsiz sürüm ile kullanıcıları ve grupları yönetin, ile şirket içi dizinleri eşitleyin, Azure, Office 365 ve binlerce Salesforce, Workday, Concur, DocuSign, Google Apps, kutusunda, ServiceNow, Dropbox ve daha fazlası gibi popüler SaaS uygulamasına çoklu oturum açma alın. 

Azure Active Directory'yi geliştirmek için Azure Active Directory temel, Premium P1 ve Premium P2 sürümleri kullanılarak Ücretli özellikleri ekleyebilirsiniz. Sürümleri Ücretli Azure Active Directory yerleşiktir varolan ücretsiz directory üstünde, Kurumsal Self Servis, Gelişmiş izleme, raporlama güvenliği, çok faktörlü kimlik doğrulama (MFA) ve güvenli erişim için mobil iş gücünüzü kapsayıcı sınıfı özelliklerinin sağlanması.

> [!NOTE]
> Bu sürümlere fiyatlandırma seçenekleri için bkz [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/). Şu anda Azure Active Directory Premium P1, Premium P2 ve Azure Active Directory temel Çin'de desteklenmez. Azure AD fiyatlandırma hakkında daha fazla bilgi için Azure Active Directory Forumu başvurabilirsiniz.
>

* **Azure Active Directory temel** -ilk bulut ihtiyaçları olan görev çalışanları için tasarlandığından, bu sürüm merkezli bulut uygulama erişimi ve Self Servis kimlik yönetimi çözümleri sağlar. Azure Active Directory’nin Temel sürümü size grup tabanlı erişim yönetimi, bulut uygulamaları için self servis parola sıfırlama ve Azure Active Directory Uygulama Proxy’si (Azure Active Directory’yi kullanarak şirket içi web uygulamalarında yayımlama için) gibi üretkenliği geliştiren ve maliyeti düşüren özellikler sunar. Bu özelliklerin tamamı, %99,9 oranındaki kurumsal düzeyde çalışma süresi SLA’sı ile desteklenir.
* **Azure Active Directory Premium P1** - daha fazla yoğun olan kuruluşlar ihtiyaçları yardımcı olmak amacıyla tasarlanmış kimlik ve erişim yönetimi gereksinimlerini, Azure Active Directory Premium edition zengin kuruluş düzeyi kimlik yönetimi özelliklerini ekler ve karma kullanıcıların sorunsuz bir şekilde şirket içi erişmek ve yetenekleri bulut olanak tanır. Bu sürüm uygulama erişimi, Self Servis kimlik ve erişim yönetimi (IAM) kimlik koruması, güvenlik ve bulut arasında bilgi çalışanı ve karma ortamlarda kimlik yöneticileri için gereken her şeyi içerir. Dinamik grupların ve Self Servis Grup Yönetimi gibi gelişmiş yönetim ve temsilci kaynaklarına destekler. Microsoft Identity Manager (bir şirket içi kimlik ve erişim yönetimi paketi) içerir ve bulut çözümleri gibi Self Servis parola sıfırlama, şirket içi kullanıcılarınız için etkinleştirme geri yazma özellikleri sağlar.
* **Azure Active Directory Premium P2** -tüm kullanıcılar ve Yöneticiler için gelişmiş koruma ile tasarlandığından, bu yeni sunum tüm özelliklerini Azure AD Premium P1 yanı sıra kimlik koruması ve Privileged Identity Management içerir. Azure Active Directory kimlik koruması kritik şirket verileri ve uygulamalar için risk bağlı olarak koşullu erişim sağlamak için sinyal milyarlarca yararlanır. Biz de yönetmek ve bulabilmesi için şekilde ayrıcalıklı hesaplara Azure Active Directory Privileged Identity Management ile korumak Yardım kısıtlamak ve yöneticilerin ve kullanıcıların kaynaklara erişimi izlemek ve gerektiğinde tam zamanında erişim sağlar.  

> [!NOTE]
> Azure Active Directory özellikleri sayısı "Kullandıkça Öde" sürümleri kullanılabilir:
>
> * Active Directory B2C, tüketiciye yönelik uygulamalarınız için kimlik ve erişim yönetimi çözümü ' dir. Daha fazla bilgi için bkz: [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure multi-Factor Authentication kimlik doğrulama sağlayıcıları veya kullanıcı başına aracılığıyla kullanılabilir. Daha fazla bilgi için bkz: [Azure multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Çalışmaya nasıl?

**Bir BT yöneticisi olarak ayarlanırsa:**

* [Deneyin!](https://azure.microsoft.com/trial/get-started-active-directory/) -bir ücretsiz 30 günlük deneme için kaydolabilirsiniz Bugün ve altında beş bu bağlantıyı kullanarak dakika içinde ilk bulut çözümünüzü dağıtma

* Okuma [Azure AD ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) ipuçları ve püf noktaları Azure AD alma üzerinde çalışır hızlı Kiracı için

**Bir geliştirici varsa:**
 
* Kullanıma [Geliştirici Kılavuzu](active-directory-developers-guide.md) Azure Active Directory'ye

* [Bir deneme sürümünü Başlat](https://azure.microsoft.com/trial/get-started-active-directory/) – bir ücretsiz 30 günlük deneme için kaydolabilirsiniz Bugün ve uygulamalarınızı Azure AD ile tümleştirme Başlat

## <a name="next-steps"></a>Sonraki adımlar
[Azure kimlik ve erişim yönetimi ile ilgili temel bilgileri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
