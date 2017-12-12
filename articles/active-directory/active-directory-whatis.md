---
title: Azure Active Directory nedir?
description: "Mevcut şirket içi kimliklerinizi buluta genişletmek veya Azure AD tümleşik uygulamalar geliştirmek için Azure Active Directory'yi kullanır."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: jeffgilb
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.custom: it-pro
ms.openlocfilehash: e5eafd4d25d2638ab5d9f904a7e0c00b36501d68
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?
Azure Active Directory (Azure AD) olduğu Microsoft çok kiracılı, bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti. Azure AD çekirdek Dizin Hizmetleri, Gelişmiş Kimlik Yönetimi ve uygulamaya erişim yönetimi birleştirir. Ayrıca, Azure AD erişim denetimi merkezi ilke ve kural göre uygulamalarını sunmak geliştiricilere sağlayan zengin, standartlara dayalı platformu sunar. 

BT yöneticileri için Azure AD çalışanlar ve iş ortakları çoklu oturum açma (SSO) erişimi vermek için uygun maliyetli, kullanımı kolay bir çözüm sağlar. [bulut SaaS uygulamaları binlerce](active-directory-saas-tutorial-list.md) Office365, Salesforce.com, DropBox ve Concur gibi.

Uygulama geliştiricileri için hızlı ve kuruluşların tüm dünyada milyonlarca tarafından kullanılan bir world sınıfı kimlik yönetimi çözümü ile tümleştirmek basit hale getirerek uygulamanızı oluşturmaya odaklanmanıza Azure AD sağlar.

Rol tabanlı erişim denetimi, uygulama kullanımını izleme, zengin denetim ve güvenlik izleme ve uyarı, Azure AD kimlik yönetimi özelliklerini çok faktörlü kimlik doğrulaması, cihaz kaydı, Self Servis parola yönetimi, Self Servis Grup Yönetimi, ayrıcalıklı hesap yönetimi dahil olmak üzere, tam bir paketi de içerir. Bu özellikler güvenli bulut tabanlı uygulamalar Yardım, BT işlemlerini kolaylaştırır, maliyetleri kesebilir ve kurumsal uyumluluk hedefleri karşılandığından emin olun yardımcı.

İle ek olarak, yalnızca [dört tıklama](./connect/active-directory-aadconnect-get-started-express.md)kuruluşların mevcut şirket içi yararlanan yeteneğini buluta erişimi yönetmek üzere kimlik Yatırımlar tabanlı SaaS uygulamaları, Azure AD mevcut bir Windows Server Active Directory ile tümleştirilebilir.

Bir Office 365, Azure veya Dynamics CRM Online müşteri varsa, Azure AD zaten kullanmakta olduğunuz farkına varmazsınız. Her Office 365, Azure ve Dynamics CRM Kiracı gerçekten Azure AD kiracısı zaten var. Kiracı yönetmek için kullanmaya başlayabilmeniz için istediğiniz zaman erişim binlerce diğer bulut uygulamasına Azure AD ile entegre olur!

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="how-reliable-is-azure-ad"></a>Azure AD nasıl güvenilir mi?
Çok kiracılı, coğrafi olarak dağıtılan, yüksek kullanılabilirlik tasarımı, buna yönelik en kritik işletme ihtiyaçlarınıza bağlı Azure AD anlamına gelir. Tüm dünyada 28 veri merkezleri otomatik yük devretme kümelemesiyle azalıyor, Azure AD yüksek oranda güvenilir ve başka bir veri merkezi dizininize en az iki daha bölgesel veri merkezleri dağınık verileri canlı bir kopyasını aşağı kalırsa bile ve anında erişim için kullanılabilir olduğunu bilmesinin rahatlık sahip olacaksınız.

Daha fazla ayrıntı için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Bir sürüm seçin
Tüm Microsoft Online iş Hizmetleri, oturum açma için Azure Active Directory'ye (Azure AD) bağlı ve diğer kimlik gerekiyor. Herhangi bir Microsoft Online iş Hizmetleri (örneğin, Office 365 veya Microsoft Azure) için abone olursanız, Azure AD ücretsiz özelliklerin tümü, erişimi ile alın. Azure Active Directory ücretsiz sürüm ile kullanıcıları ve grupları yönetin, ile şirket içi dizinleri eşitleyin, Azure, Office 365 ve binlerce Salesforce, Workday, Concur, DocuSign, Google Apps, kutusunda, ServiceNow, Dropbox ve daha fazlası gibi popüler SaaS uygulamasına çoklu oturum açma alın. 

Azure Active Directory'yi geliştirmek için Azure Active Directory temel, Premium P1 ve Premium P2 sürümleri kullanılarak Ücretli özellikleri ekleyebilirsiniz. Sürümleri Ücretli Azure Active Directory yerleşiktir varolan ücretsiz directory üstünde, Kurumsal Self Servis, Gelişmiş izleme, raporlama güvenliği, çok faktörlü kimlik doğrulama (MFA) ve güvenli erişim için mobil iş gücünüzü kapsayıcı sınıfı özelliklerinin sağlanması.

> [!NOTE]
> Bu sürümlere fiyatlandırma seçenekleri için bkz [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/). Şu anda Azure Active Directory Premium P1, Premium P2 ve Azure Active Directory temel Çin'de desteklenmez. Lütfen daha fazla bilgi için Azure Active Directory Forumu adresindeki iletişime geçin.
>

* **Azure Active Directory temel** -ilk bulut ihtiyaçları olan görev çalışanları için tasarlandığından, bu sürüm bulut merkezli uygulama erişimi ve Self Servis kimlik yönetimi çözümleri sağlar. Azure Active Directory’nin Temel sürümü size grup tabanlı erişim yönetimi, bulut uygulamaları için self servis parola sıfırlama ve Azure Active Directory Uygulama Proxy’si (Azure Active Directory’yi kullanarak şirket içi web uygulamalarında yayımlama için) gibi üretkenliği geliştiren ve maliyeti düşüren özellikler sunar. Bu özelliklerin tamamı, %99,9 oranındaki kurumsal düzeyde çalışma süresi SLA’sı ile desteklenir.
* **Azure Active Directory Premium P1** - daha fazla yoğun olan kuruluşlar ihtiyaçları yardımcı olmak amacıyla tasarlanmış kimlik ve erişim yönetimi gereksinimlerini, Azure Active Directory Premium edition zengin kuruluş düzeyi kimlik yönetimi özelliklerini ekler ve karma kullanıcıların sorunsuz bir şekilde şirket içi erişmek ve yetenekleri bulut olanak tanır. Bu sürüm, karma ortamlarda bilgi çalışanı ve kimlik yöneticileri için gerekli olan; bulutta uygulama erişimi, self servis kimlik ve erişim yönetimi (IAM), kimlik koruma ve güvenlik gibi özelliklerin tamamını içerir. Dinamik grupların ve Self Servis Grup Yönetimi gibi gelişmiş yönetim ve temsilci kaynaklarına destekler. Microsoft Identity Manager (bir şirket içi kimlik ve erişim yönetimi paketi) içerir ve bulut çözümleri gibi Self Servis parola sıfırlama, şirket içi kullanıcılarınız için etkinleştirme geri yazma özellikleri sağlar.
* **Azure Active Directory Premium P2** -tüm kullanıcılar ve Yöneticiler için gelişmiş koruma ile tasarlandığından, bu yeni sunum tüm özelliklerini Azure AD Premium P1 yanı sıra bizim yeni kimlik koruması ve Privileged Identity Management içerir. Azure Active Directory kimlik koruması kritik şirket verileri ve uygulamalar için risk bağlı olarak koşullu erişim sağlamak için sinyal milyarlarca yararlanır. Biz de yönetmek ve bulabilmesi için şekilde ayrıcalıklı hesaplara Azure Active Directory Privileged Identity Management ile korumak Yardım kısıtlamak ve yöneticilerin ve kullanıcıların kaynaklara erişimi izlemek ve gerektiğinde tam zamanında erişim sağlar.  

> [!NOTE]
> Azure Active Directory özellikleri sayısı "Kullandıkça Öde" sürümleri kullanılabilir:
>
> * Active Directory B2C, tüketiciye yönelik uygulamalarınız için kimlik ve erişim yönetimi çözümü ' dir. Daha fazla ayrıntı için bkz: [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure multi-Factor Authentication kimlik doğrulama sağlayıcıları veya kullanıcı başına aracılığıyla kullanılabilir. Daha fazla ayrıntı için bkz: [Azure multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Çalışmaya nasıl?

**Bir BT yöneticisi olarak ayarlanırsa:**

* [Deneyin!](https://azure.microsoft.com/trial/get-started-active-directory/) -bir ücretsiz 30 günlük deneme sürümüne kaydolun bugün oturum ve ilk bulut çözümünüzü altında 5 bu bağlantıyı kullanarak dakika içinde dağıtma

* Okuma [Azure AD ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) ipuçları ve püf noktaları Azure AD alma üzerinde çalışır hızlı Kiracı için

**Bir geliştirici varsa:**
 
* Kullanıma bizim [Geliştirici Kılavuzu](active-directory-developers-guide.md) Azure Active Directory'ye

* [Bir deneme sürümünü Başlat](https://azure.microsoft.com/trial/get-started-active-directory/) – bir ücretsiz 30 günlük deneme sürümüne kaydolun bugün oturum ve uygulamalarınızı Azure AD ile tümleştirme Başlat

## <a name="next-steps"></a>Sonraki adımlar
[Azure kimlik ve erişim yönetimi ile ilgili temel bilgileri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
