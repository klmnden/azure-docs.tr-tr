---
title: Azure Active Directory dağıtım planları | Microsoft Docs
description: Azure Active Directory özelliklerini dağıtma konusunda ayrıntılı bir rehber sunar
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.openlocfilehash: f5a148cba172b4e1e4490b52afb3b500da3a593c
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434169"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory dağıtım planları
Azure Active Directory (AD) özelliklerini dağıtma konusunda ayrıntılı bir rehbere mi ihtiyacınız var? Aşağıdaki dağıtım planları yaygın Azure AD özelliklerinin başarıyla kullanıma alınması için gerekli olan iş değeri, planlama noktaları, tasarım ve operasyon yordamlarını anlatmaktadır. 

Belgelerin içinde e-posta şablonlarını, sistem mimarisi diyagramlarını, yaygın test çalışmalarını ve daha fazlasını bulacaksınız. 

Belgeler hakkındaki geri bildirimlerinizi bekliyoruz. Belgeler hakkındaki fikirlerinizi paylaşmak için bu kısa [anketi](https://aka.ms/deploymentplanfeedback) yanıtlayın. 

|Senaryo |Açıklama |
|-|-|
|[Çoklu oturum açma](https://aka.ms/SSODPDownload)|Çoklu oturum açma, bir kez oturum açarak ve tek bir kullanıcı hesabı kullanarak işinizi yapmak için gerekli tüm uygulamalara ve kaynaklara erişmenize yardımcı olur. Oturum açtıktan sonra Microsoft Office'ten SalesForce ve Box uygulamalarına yeniden kimlik doğrulamasından (parola yazma gibi) geçmeden erişebilirsiniz.|
|[Workday odaklı gelen kullanıcı sağlama](https://aka.ms/WorkdayDeploymentPlan)|Workday temelli gelen kullanıcı sağlamayı Active Directory, devam eden kimlik yönetimi için bir temel oluşturur ve yetkili kimlik verilere dayanan iş süreçlerini kalitesini artırır. Bu özelliği kullanarak, sorunsuz bir şekilde çalışanları ve bağlı çalışanları kimlik yaşam döngüsü Birleştirici taşıyıcısı Leaver işlemler (örneğin, yeni işe, sonlandırma, aktarım) BT sağlama Eylemler (örneğin, oluşturma, etkinleştir, eşleyen kuralları yapılandırarak yönetebilirsiniz "Devre dışı bırakmak, hesapları silme).|
|[Erişim Paneli](https://aka.ms/AccessPanelDPDownload)|Kullanıcılarınızın bulmak ve tüm uygulamalara erişmek için basit bir hub'ı sunar. Yeni uygulamalar ve gruplar için erişim isteme imkanını gibi Self Servis özellikler ile daha üretken olmalarını sağlamak veya diğerleri adına bu kaynaklara erişimi yönetin.|
|[Kullanıcı sağlama](https://aka.ms/UserProvisioningDPDownload)|Azure AD; Dropbox, Salesforce ve ServiceNow gibi bulut (SaaS) uygulamalarında kullanıcı oluşturma, bakım ve kaldırma adımlarını otomatikleştirmenize yardımcı olur.|
|[Multi-Factor Authentication](https://aka.ms/MFADPDownload)|Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Yönetici onaylı kimlik doğrulama yöntemlerini kullanan Azure MFA, oturum açma sürecini karmaşık hale getirmeden verilerinize ve uygulamalarınıza erişimin güvenli hale getirilmesine yardımcı olur.|
|[Koşullu erişim](https://aka.ms/CADPDownload)|Koşullu erişimle bulut uygulamalarınıza erişim sağlamak üzere koşullara bağlı otomatik erişim denetimi kararlarını uygulamaya alabilirsiniz.|
|[Doğrudan Geçiş Kimlik Doğrulaması için ADFS](https://aka.ms/ADFSTOPTADPDownload)|Azure AD Doğrudan Geçiş Kimlik Doğrulaması, kullanıcılarınızın aynı parolalarla hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmasına yardımcı olur. Bu özellik hatırlamaları gereken parola sayısını azaltarak kullanıcılarınıza daha iyi bir deneyim sağlar ve oturum açma bilgileri muhtemelen daha az unutulacağından BT yardım masası maliyetlerinizi de düşürür. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular.|
|[Parola Karması Eşitleme için ADFS](https://aka.ms/ADFSTOPHSDPDownload)|Parola Karması Eşitleme özelliği ile kullanıcı parolalarının karmaları şirket içi Active Directory ile Azure AD arasında eşitlenerek kullanıcıların şirket içi Active Directory ile etkileşim kurmadan kimlik doğrulamasından geçmesi sağlanır.|
|[Sorunsuz çoklu oturum açma](https://aka.ms/SeamlessSSODPDownload)|Azure Active Directory Sorunsuz Çoklu Oturum Açma (Azure AD Sorunsuz SSO) özelliği, kurumsal ağınıza bağlı kuruluş cihazlarını kullanan kullanıcıların otomatik olarak oturum açmasını sağlar. Bu özelliği açtığınızda kullanıcılarınızın Azure AD oturumu açmak için parolalarını ve hatta kullanıcı adlarını yazmalarına gerek kalmaz. Bu özellik kullanıcılarınızın şirket içi ek bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolayca erişmesini sağlar.|
|[Self servis parola sıfırlama](https://aka.ms/SSPRDPDownload)|Self servis parola sıfırlama, kullanıcılarınızın istediği yerden ve istediği zaman yönetici müdahalesi olmadan parolalarını sıfırlamasına yardımcı olur.|
|[Azure AD Uygulama Ara Sunucusu](https://aka.ms/AppProxyDPDownload)|Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Kendi tabletlerinden, telefonlarından ve dizüstü bilgisayarlarından da iş yapma istekleri vardır. Çalışanlar ayrıca hem buluttaki SaaS uygulamalarına hem de şirket içi kurumsal uygulamalarına erişmek istemektedir. Şirket içi uygulamalara erişmek için bugüne kadar sanal özel ağlar (VPN) veya temiz bölgeler (DMZ) kullanılıyordu. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir. Artık çok daha iyi bir seçenek var! - Azure AD Uygulama Ara Sunucusu|

