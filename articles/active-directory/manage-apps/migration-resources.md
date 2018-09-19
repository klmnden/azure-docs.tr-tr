---
title: Kaynakları geçirmek için Azure Active Directory uygulamaları | Microsoft Docs
description: Uygulama erişimi ve kimlik doğrulaması Azure Active Directory (Azure AD) geçirmenize yardımcı olacak kaynaklar'ı tıklatın.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2018
ms.author: barbkess
ms.reviewer: baselden
ms.openlocfilehash: 8338ae097f6376b8d46244831cfa7a0c6553d825
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128101"
---
# <a name="resources-for-migrating-applications-to-azure-active-directory"></a>Kaynakları geçirmek için Azure Active Directory uygulamaları

Uygulama erişimi ve kimlik doğrulaması Azure Active Directory (Azure AD) geçirmenize yardımcı olacak kaynaklar'ı tıklatın. 

| Kaynak  | Açıklama  |
|:-----------|:-------------|
|[Uygulamalarınızı Azure AD'ye geçirme](https://aka.ms/migrateapps/whitepaper) | Bu teknik incelemeyi geçişin yararları sunar ve net bir şekilde açıklanan dört aşamaya geçiş planlama açıklar: bulma, Sınıflandırma, geçiş ve devam eden yönetimi. İşlemleri hakkında düşünmek ve projenize kolayca kullanma parçalara ayırmanız nasıl destekli. Belge, süreç boyunca size yardımcı olacak önemli kaynakların bağlantılarını bağlıdır. |
|[Çözüm Kılavuzu: Azure AD'ye geçirme uygulamalardan Active Directory Federasyon Hizmetleri (AD FS)](https://aka.ms/migrateapps/adfssolutionguide) | Bu çözüm kılavuzunda, planlama ve uygulama, geçiş teknik incelemeyi daha yüksek bir düzeyde tanımlanan bir geçiş projesi yürütme aynı dört aşaması gösterilmektedir. Bu kılavuzda, belirli bir uygulamayı Azure Directory Federasyon Hizmetleri'nde (AD FS) Azure AD'ye taşınıyor amacı bu aşamaları uygulamak öğreneceksiniz.|
| [Aracı: Active Directory Federasyon Hizmetleri geçiş hazırlığı betiği](https://aka.ms/migrateapps/adfsscript) | Bir betik budur uygulamaları Azure AD'ye geçiş için hazır olup olmadığını belirlemek için şirket içi Active Directory Federasyon Hizmetleri (AD FS) sunucunuzda çalıştırın.|
| [Dağıtım planı: AD FS'den parola karması Eşitleme'ye geçirme](https://aka.ms/ADFSTOPHSDPDownload) | Parola Karması eşitleme ile kullanıcı parola karmalarının Azure AD'ye şirket içi Active Directory'nizden eşitlenen. Bu, şirket içi Active Directory ile etkileşim olmadan kullanıcıların kimliğini doğrulamak Azure AD'ye verir.| 
| [Dağıtım planı: AD FS'den doğrudan kimlik doğrulama için geçirme](https://aka.ms/ADFSTOPTADPDownload)|Azure AD geçişli kimlik doğrulaması, hem şirket içi hem de bulut tabanlı uygulamalar için aynı parolayı kullanarak oturum açmasına yardımcı olur. Bunlar daha az bir parolayı anımsa sahip olduğundan bu özellik, kullanıcılarınıza daha iyi bir deneyim sağlar. Kullanıcılar yalnızca bir parolayı anımsa istedikleri zaman oturum açmak nasıl oluşturabileceğinize değil daha olası olduğundan ayrıca BT Yardım Masası maliyetlerini azaltır. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular.|
| [Dağıtım planı: çoklu oturum açma bir SaaS uygulaması için Azure AD ile etkinleştirme](https://aka.ms/SSODPDownload) | Tüm uygulamaları ve iş, bir tek kullanıcı hesabıyla yalnızca bir kez oturum açma sırasında yapmanız gereken kaynaklara erişim çoklu oturum açma (SSO) yardımcı olur. Bir kullanıcı oturum açtıktan sonra Örneğin, kullanıcının Microsoft Office, SalesForce, (örneğin bir parola yazmak) ikinci kez kimlik doğrulaması olmadan kutusuna taşıyabilirsiniz. 
| [Dağıtım planı: Azure ad uygulama ara sunucusu ile uygulamalara genişletme](https://aka.ms/AppProxyDPDownload)| Erişime karşı çalışan dizüstü bilgisayarlar ve diğer cihazlara uygulamaları geleneksel olarak söz konusu sanal özel ağlar (VPN) veya (DMZ'ler) arındırılmış bölge şirket içi. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir. Azure AD uygulama proxy'si, şirket uygulamalarına erişmek için kolaylaştırır. |
| [Dağıtım planı](../fundamentals/active-directory-deployment-plans.md) | Çok faktörlü kimlik doğrulaması, koşullu erişim, kullanıcı sağlamayı, sorunsuz SSO, Self Servis parola sıfırlama ve daha fazlası gibi özellikler dağıtmak için daha fazla dağıtım planlarını edinin! |


