---
title: Azure Market SaaS uygulamaları teknik Kılavuzu yayımlama
description: Adım adım kılavuz ve SaaS uygulamaları Azure Marketi'nde yayımlama için yayımlama denetim listeleri
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: ''
author: BrentL-Collabera
manager: ''
editor: BrentL-Collabera
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/28/2018
ms.author: pabutler
ms.openlocfilehash: eb6db45ca0fcb6879aeaeaaf70715691cac438b0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="saas-applications-technical-publishing-guide"></a>SaaS uygulamaları teknik yayımlama Kılavuzu

Azure Market SaaS uygulamaları teknik Kılavuzu yayımlama için Hoş Geldiniz. Bu kılavuz, aday ve varolan yayımcılar uygulamaları ve hizmetleri sunan SaaS uygulamaları kullanarak Azure markette listelemek için yardımcı olmak için tasarlanmıştır.  
SaaS uygulamaları teklif çözümünüzü kendi Azure aboneliğinizde dağıtılır ve müşteriler tasarım ve uygulama test etmek yöneten bir arabirim ile oturum açın, kullanmak istediğiniz. Bunu kullanarak yapar [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) mevcut deneme ortamınızın yararlanmak için. Diğer bir deyişle, müşteri neden, iş ortağı tarafından barındırılan bir ücretsiz deneme olduğunu. Çözümünüzü bulut alıcılar, çözümünüzün bağımsız olarak herhangi bir ücret veya ücret deneyimi fırsatı verir şekilde kullanıma sunmak için kritik öneme sahiptir ve bu teklif tür bir deneme sürümü deneyimi sağlar, böylece müşteriler için bulut çözümleri nasıl arama verirsiniz.  

Diğer tüm Market tekliflerini genel bakış için lütfen [Market yayımcı Kılavuzu](https://aka.ms/sellerguide).

## <a name="saas-application-technical-guidance"></a>SaaS uygulama teknik Kılavuzu
SaaS uygulamaları için teknik gereksinimleri basittir. Yayımcılar yalnızca yayımlanmasını Azure AD ile tümleşik gerekir.  Azure AD tümleştirme uygulamalarla belgelendiğinden ve birden çok SDK'ları ve kaynakları bunu gerçekleştirmek için Microsoft sağlar.  

Başlatmak için diğer girişimleri işten yalıtmak sağlayarak, Azure Marketi'nde yayımlama için adanmış bir aboneliğinizin olması önerilir. Ayrıca, aksi takdirde zaten yüklü, geliştirme ortamınızı bir parçası olarak aşağıdaki araçları sahip olmasını öneririz: 
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)  
- [Azure powerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-5.0.0)  
- [Azure Geliştirici Araçları (inceleyin. kullanılabilir)](https://azure.microsoft.com/tools/)  
- [Visual Studio Code](https://code.visualstudio.com/)  

### <a name="resources"></a>Kaynaklar
Aşağıdaki listeler, başlamanıza yardımcı olmak için en iyi Azure AD kaynaklara bağlantılar sağlar: 

**Belgeleri**

- [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

- [Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

- [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

- [Azure yol haritası - güvenlik ve kimlik](https://azure.microsoft.com/roadmap/?category=security-identity)

**Videolar**

- [Vittorio Bertocci ile Azure Active Directory kimlik doğrulaması](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

- [Azure Active Directory kimlik teknik brifing - bölüm 1 / 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

- [Azure Active Directory kimlik teknik brifing - Kısım 2 / 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

- [Microsoft Azure Active Directory ile uygulamaları oluşturma](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

- [Microsoft Azure Active Directory odaklanmış videolar](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

**Eğitim**  
- [BT uzmanları içerik seri için Microsoft Azure: Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

**Azure Active Directory Hizmet güncelleştirmeleri**  
- [Azure AD hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)

Desteği için aşağıdaki kaynakları kullanabilirsiniz:
- [MSDN Forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
- [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="the-azure-active-directory-gallery"></a>Azure Active Directory Galerisi
Uygulamanızı Azure Market/AppSource listelenen sahip olmaya ek olarak, Azure Market AppStore parçası olan Azure AD uygulama galerisinde listelenen uygulama da olabilir. Azure AD kimlik sağlayıcısı farklı SaaS uygulama bağlayıcıları bulabilirsiniz olarak kullanan müşteriler burada yayımladı. BT yöneticileri bağlayıcılar uygulama Galeriden ekleyebilir ve sonra yapılandırmak ve bağlayıcıları çoklu oturum açma (SSO) ve sağlama için kullanabilirsiniz. Azure AD SAML 2.0, Openıd Connect, OAuth ve WS-Fed dahil olmak üzere SSO için tüm önemli Federasyon protokollerini destekler.  

Uygulama tümleştirmesi Azure AD ile çalışıp çalışmadığını test ettikten sonra uygulama ağ Portal erişimi için isteğinizi gönderebilirsiniz. Office 365 hesabı varsa, bu portalda oturum açmak için kullanın. Office 365 hesabı yoksa, oturum açmak için Microsoft hesabınızı (örneğin, Outlook veya Hotmail) kullanabilirsiniz.

## <a name="program-benefits"></a>Program avantajları
- Müşteriler için en iyi olası tek oturum açma deneyimi sağlar
- Uygulamasının basit ve en az yapılandırma
- Müşteriler için uygulama arayın ve galeride Bul
- Herhangi bir müşteriye hangi Azure AD SKU bağımsız olarak (ücretsiz, Basic veya Premium) kullandıkları Bu tümleştirme özelliğini kullanabilirsiniz
- Karşılıklı müşterilerimiz için adım adım yapılandırma Öğreticisi
- SCIM'yi kullanıyorsanız, kullanıcı için aynı uygulama sağlama sağlar

## <a name="prerequisites"></a>Önkoşullar
Uygulamanın Azure AD galerisinde listelemek için uygulamanın ilk Azure AD tarafından desteklenen Federasyon protokollerden birini uygulamanız gerekir. Lütfen hüküm ve koşulları konumunda bulunan Azure AD uygulama Galerisi okuyun [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/).  

Hangi protokole bağlı olarak, kullandığınız ek önkoşul bilgileri açıklanmaktadır:

**Openıd Connect** - Azure AD çok kiracılı uygulama oluşturma ve uygulamanız için bir onay çerçeve. Böylece tüm müşteri uygulama onay sağlayabilir ortak uç noktasına oturum açma isteği gönderin. Kiracı kimliği ve belirteçte alınan kullanıcı UPN bağlı müşteri kullanıcı erişimi denetleyebilirsiniz.  
**SAML 2.0 veya WS-Fed** -uygulamanızı SP veya IDP modunda SAML veya WS-Fed SSO tümleştirme yapmak için bir özellik olması gerekir.
