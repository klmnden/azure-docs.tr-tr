---
title: Dağıtım planı - Azure Active Directory | Microsoft Docs
description: Birçok Azure Active Directory özellikleri dağıtmak nasıl uçtan uca yönergeler.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: lizross
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa7c4865313a48ae5d9041d2dceb6c91787bb6be
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733546"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory dağıtım planları
Azure Active Directory (AD) özelliklerini dağıtma konusunda ayrıntılı bir rehbere mi ihtiyacınız var? Aşağıdaki dağıtım planları yaygın Azure AD özelliklerinin başarıyla kullanıma alınması için gerekli olan iş değeri, planlama noktaları, tasarım ve operasyon yordamlarını anlatmaktadır. 

Belgelerin içinde e-posta şablonlarını, sistem mimarisi diyagramlarını, yaygın test çalışmalarını ve daha fazlasını bulacaksınız. 

Belgeler hakkındaki geri bildirimlerinizi bekliyoruz. Belgeler hakkındaki fikirlerinizi paylaşmak için bu kısa [anketi](https://aka.ms/deploymentplanfeedback) yanıtlayın. 

## <a name="include-the-right-stakeholders"></a>Doğru proje katılımcılarının

Yeni bir özellik için planlama, dağıtım başlayarak, kuruluşunuz genelinde önemli proje katılımcılarının önemlidir. Tanımlamak ve kişi veya aşağıdaki rollerin her birini gerçekleştirmek ve bunları kendi katılımını belirlemeye çalışmak kişiler belgeyi öneririz.  

Rolleri aşağıdakileri içerebilir. 

|Rol |Açıklama |
|-|-|
|Son kullanıcı|Temsili bir yetenek uygulanacak kullanıcı grubu. Genellikle bir deneme programı değişiklikleri önizlemeleri sağlanır.
|BT desteği Yöneticisi|Bu değişikliğin bir Yardım Masası açısından desteklenebilirliği üzerinde giriş sağlayabilen BT destek kuruluş temsilcisi.  
|Kimlik Mimarı ya da Azure genel yönetici|Bu değişiklik, kuruluşunuzdaki çekirdek Kimlik Yönetimi altyapısı ile nasıl hizalanacağını tanımlama sorumlu Kimlik Yönetimi ekip temsilcisi.|
|Uygulama iş sahibi |Erişimi yönetme içerebilecek etkilenen uygulamaları genel iş sahibi.  Ayrıca kullanıcı deneyimini ve bu değişiklik, bir son kullanıcının açısından kullanışlılığını giriş sağlayabilir.
|Güvenlik sahibi|Bir temsilcisi, devre dışı plan oturum açabilir güvenlik ekibinin, kuruluşunuzun güvenlik gereksinimlerini karşılar.|
|Uyumluluk Yöneticisi|Kuruluşunuzdaki Kurumsal uyumluluğu, sektör ve kamu gereksinimleri sağlamaktan sorumlu kişi.|

**Katılımı düzeylerini şunlar olabilir:**

- **R**esponsible proje planı ve sonuç uygulamak için 

- **A**onay proje planı ve sonucu 

- **C**tkıda bulunan proje planı ve sonucu 

- **Ben**nformed proje planı ve sonucu
 
## <a name="deployment-plans"></a>Dağıtım planı



|Senaryo |Açıklama |
|-|-|
|[Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)|Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Yönetici onaylı kimlik doğrulama yöntemlerini kullanan Azure MFA, oturum açma sürecini karmaşık hale getirmeden verilerinize ve uygulamalarınıza erişimin güvenli hale getirilmesine yardımcı olur.|
|[Koşullu erişim](https://aka.ms/deploymentplans/ca)|Koşullu erişimle bulut uygulamalarınıza erişim sağlamak üzere koşullara bağlı otomatik erişim denetimi kararlarını uygulamaya alabilirsiniz.|
|[Self servis parola sıfırlama](https://aka.ms/SSPRDPDownload)|Self servis parola sıfırlama, kullanıcılarınızın istediği yerden ve istediği zaman yönetici müdahalesi olmadan parolalarını sıfırlamasına yardımcı olur.|
|[Privileged Identity Management](../privileged-identity-management/pim-deployment-plan.md)|Azure AD Privileged Identity Management (PIM) Azure AD genelinde ayrıcalıklı yönetici rollerini yönetmenize yardımcı olan Azure kaynaklarını ve diğer Microsoft Çevrimiçi Hizmetler. PIM tam zamanında erişim isteği onay iş akışları ve tamamen tümleşik erişim incelemeleri belirleyebilmeniz, ortaya çıkarın ve ayrıcalıklı rollerin gerçek zamanlı kötü amaçlı etkinlikleri engellemek gibi çözümler sağlar.|
|[Çoklu oturum açma](https://aka.ms/SSODPDownload)|Çoklu oturum açma, bir kez oturum açarak ve tek bir kullanıcı hesabı kullanarak işinizi yapmak için gerekli tüm uygulamalara ve kaynaklara erişmenize yardımcı olur. Oturum açtıktan sonra Microsoft Office'ten SalesForce ve Box uygulamalarına yeniden kimlik doğrulamasından (parola yazma gibi) geçmeden erişebilirsiniz.|
|[Sorunsuz çoklu oturum açma](https://aka.ms/SeamlessSSODPDownload)|Azure Active Directory Sorunsuz Çoklu Oturum Açma (Azure AD Sorunsuz SSO) özelliği, kurumsal ağınıza bağlı kuruluş cihazlarını kullanan kullanıcıların otomatik olarak oturum açmasını sağlar. Bu özelliği açtığınızda kullanıcılarınızın Azure AD oturumu açmak için parolalarını ve hatta kullanıcı adlarını yazmalarına gerek kalmaz. Bu özellik kullanıcılarınızın şirket içi ek bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolayca erişmesini sağlar.|
|[Erişim Paneli](https://aka.ms/AccessPanelDPDownload)|Kullanıcılarınızın bulmak ve tüm uygulamalara erişmek için basit bir hub'ı sunar. Yeni uygulamalar ve gruplar için erişim isteme imkanını gibi Self Servis özellikler ile daha üretken olmalarını sağlamak veya diğerleri adına bu kaynaklara erişimi yönetin.|
|[Parola Karması Eşitleme için ADFS](https://aka.ms/deploymentplans/adfs2phs)|Parola Karması Eşitleme özelliği ile kullanıcı parolalarının karmaları şirket içi Active Directory ile Azure AD arasında eşitlenerek kullanıcıların şirket içi Active Directory ile etkileşim kurmadan kimlik doğrulamasından geçmesi sağlanır.|
|[Doğrudan Geçiş Kimlik Doğrulaması için ADFS](https://aka.ms/deploymentplans/adfs2pta)|Azure AD Doğrudan Geçiş Kimlik Doğrulaması, kullanıcılarınızın aynı parolalarla hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmasına yardımcı olur. Bu özellik hatırlamaları gereken parola sayısını azaltarak kullanıcılarınıza daha iyi bir deneyim sağlar ve oturum açma bilgileri muhtemelen daha az unutulacağından BT yardım masası maliyetlerinizi de düşürür. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular.|
|[Azure AD Uygulama Ara Sunucusu](https://aka.ms/deploymentplans/appproxy)|Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Kendi tabletlerinden, telefonlarından ve dizüstü bilgisayarlarından da iş yapma istekleri vardır. Çalışanlar ayrıca hem buluttaki SaaS uygulamalarına hem de şirket içi kurumsal uygulamalarına erişmek istemektedir. Şirket içi uygulamalara erişmek için bugüne kadar sanal özel ağlar (VPN) veya temiz bölgeler (DMZ) kullanılıyordu. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir. Artık çok daha iyi bir seçenek var! - Azure AD Uygulama Ara Sunucusu|
|[Kullanıcı sağlama](https://aka.ms/UserProvisioningDPDownload)|Azure AD; Dropbox, Salesforce ve ServiceNow gibi bulut (SaaS) uygulamalarında kullanıcı oluşturma, bakım ve kaldırma adımlarını otomatikleştirmenize yardımcı olur.|
|[Workday odaklı gelen kullanıcı sağlama](https://aka.ms/WorkdayDeploymentPlan)|Workday temelli gelen kullanıcı sağlamayı Active Directory, devam eden kimlik yönetimi için bir temel oluşturur ve yetkili kimlik verilere dayanan iş süreçlerini kalitesini artırır. Bu özelliği kullanarak, sorunsuz bir şekilde çalışanları ve bağlı çalışanları kimlik yaşam döngüsü Birleştirici taşıyıcısı Leaver işlemler (örneğin, yeni işe, sonlandırma, aktarım) BT sağlama Eylemler (örneğin, oluşturma, etkinleştir, eşleyen kuralları yapılandırarak yönetebilirsiniz "Devre dışı bırakmak, hesapları silme).|
