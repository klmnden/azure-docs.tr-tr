---
title: Azure Active Directory (Azure AD) nedir? | Microsoft Docs
description: Azure Active Directory'yi kullanarak var olan şirket içi kimliklerinizi buluta taşıyabilir veya Azure AD ile tümleşik uygulamalar geliştirebilirsiniz.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 5087f759d682382bc22b5f95f462fe0f08619cc8
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "39450003"
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?
Azure Active Directory (Azure AD), Microsoft'un çekirdek dizin hizmetleri, uygulama erişim yönetimi ve kimlik korumasını tek bir çözüm altında birleştiren çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD ayrıca, geliştiricilerin uygulamalarına erişim denetimi sunmalarına olanak tanıyan, merkezi ilke ve kurallara dayanan, zengin, standartlara dayanan bir platform sunar.

![Azure AD Connect Stack](./media/active-directory-whatis/Azure_Active_Directory.png)

- **BT yöneticileri için.** Azure AD binlerce [bulut tabanlı SaaS uygulamaları](../saas-apps/tutorial-list.md) ve şirket içi uygulamalar için sunduğu daha güçlü kimlik yönetimi ve çoklu oturum açma (SSO) erişimi sayesinde kuruluşunuz için daha güvenli bir çözüm sunar. Bu uygulamaları kullanarak çalışanlarınız için bulut tabanlı uygulama güvenliği, sorunsuz erişim, gelişmiş işbirliği ve kimlik yaşam döngüsü otomasyonu özelliklerinden faydalanabilir, bu sayede hem güvenlik hem de uyumluluk alanında gelişme sağlayabilirsiniz.

    Ayrıca yalnızca [dört tıklamayla](./../connect/active-directory-aadconnect-get-started-express.md) Azure AD'yi var olan Windows Server Active Directory ile tümleştirebilir, bulut tabanlı SaaS erişimini kuruluşunuzun var olan şirket içi kimlik yatırımlarını kullanabilirsiniz.

- **Uygulama geliştiricileri için.** Azure AD, dünya üzerinde milyonlarca kuruluş tarafından kullanılan bir kimlik yönetimi çözümü ile tümleştirme yaparak uygulamalarınızı oluşturmaya odaklanmanızı sağlar.

- **Office 365, Azure veya Dynamics CRM Online müşterileri için.** Azure AD'yi zaten kullanıyorsunuz. Office 365, Azure ve Dynamics CRM Online kiracıları aslında birer Azure AD kiracısıdır ve tümleşik bulut uygulamalarınıza çalışan erişimini yönetmeye hemen başlayabilirsiniz.

## <a name="how-reliable-is-azure-ad"></a>Azure AD ne kadar güvenilir?
Çok kiracılı, coğrafi olarak dağıtılan ve yüksek kullanılabilirlik özellikli bir tasarıma sahip olan Azure AD'ye en kritik iş gereksinimlerinizde bile güvenebilirsiniz. Dünya üzerinde otomatik yük devretme özelliğine sahip 28 veri merkezi ile Azure AD'nin yüksek güvenilirliğe sahip olduğunu bilmenin rahatlığını yaşarsınız ve veri merkezlerinden biri devre dışı kalsa da dizin verilerinizin kopyaları anında erişim için en az iki farklı bölgede bulunan veri merkezinde daha kullanılabilir durumda olur.

Hizmet düzeyi sözleşmeleri hakkında daha fazla bilgi için bkz. [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Sürüm seçin
Tüm Microsoft Online iş hizmetlerinde oturum açma ve diğer kimlik gereksinimleri için Azure Active Directory (Azure AD) kullanılır. Microsoft Online iş hizmetlerinden herhangi birine (Office 365 veya Microsoft Azure gibi) abone olursanız Azure AD'nin tüm Ücretsiz özelliklerine erişim sahibi olursunuz. Azure Active Directory Ücretsiz sürümü ile kullanıcıları ve grupları yönetebilir, şirket içi dizinlerle eşitleyebilir, Azure, Office 365 ve Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox gibi binlerce popüler SaaS uygulamasında çoklu oturum açma imkanına sahip olabilirsiniz. 

Azure Active Directory'nizi geliştirmek için Azure Active Directory Temel, Premium P1 ve Premium P2 sürümleriyle ücretli özellikler ekleyebilirsiniz. Azure Active Directory'nin ücretli sürümleri, var olan ücretsiz dizin üzerinde oluşturulur ve self servis, gelişmiş izleme, güvenlik raporlaması, Multi-Factor Authentication (MFA) ve mobil iş gücünüz için güvenli erişim gibi kurumsal sınıf özellikler sunar.

> [!NOTE]
> Bu sürümlerin fiyatlandırma seçenekleri için bkz. [Azure Active Directory Fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 ve Azure Active Directory Temel şu an için Çin'de desteklenmemektedir. Azure AD fiyatlandırması hakkında daha fazla bilgi için Azure Active Directory Forum sayfasından iletişime geçebilirsiniz.
>

* **Azure Active Directory Temel**: Buluta yönelik ihtiyaçları öncelikli olan görev çalışanları için tasarlanan bu sürüm, bulut odaklı uygulama erişimi ve self servis kimlik yönetimi çözümleri sağlar. Azure Active Directory’nin Temel sürümü size grup tabanlı erişim yönetimi, bulut uygulamaları için self servis parola sıfırlama ve Azure Active Directory Uygulama Proxy’si (Azure Active Directory’yi kullanarak şirket içi web uygulamalarında yayımlama için) gibi üretkenliği geliştiren ve maliyeti düşüren özellikler sunar. Bu özelliklerin tamamı, %99,9 oranındaki kurumsal düzeyde çalışma süresi SLA’sı ile desteklenir.
* **Azure Active Directory Premium P1**: Kimlik ve erişim yönetimine yönelik gereksinimleri daha yoğun olan kurumları desteklemek için tasarlanan Azure Active Directory Premium sürümü, özellik açısından zengin, kurumsal düzeyde kimlik yönetimi olanakları eklemenin yanı sıra karma kullanıcıların şirket içindeki ve buluttaki özelliklere sorunsuz bir şekilde erişmesine olanak tanır. Bu sürüm, karma ortamlarda bilgi çalışanı ve kimlik yöneticileri için gerekli olan; bulutta uygulama erişimi, self servis kimlik ve erişim yönetimi (IAM), kimlik koruma ve güvenlik gibi özelliklerin tamamını içerir. Dinamik gruplar ve self servis grup yönetimi gibi gelişmiş kaynak yönetimi ve devretme özelliklerini destekler. Microsoft Identity Manager'ı (şirket içi kimlik ve erişim yönetimi paketi) içerir ve şirket içi kullanıcılarınız için self servis parola sıfırlama gibi çözümler sunan bulut geri yazma özelliklerine sahiptir.
* **Azure Active Directory Premium P2**: Tüm kullanıcılarınız ve yöneticileriniz için gelişmiş koruma özellikleriyle tasarlanmış olan bu yeni teklif, Azure AD Premium P1'in tüm özelliklerinin yanı sıra Identity Protection ve Privileged Identity Management özelliklerini de içerir. Azure Active Directory Identity Protection, uygulamalarınıza ve kritik şirket verilerinize risk tabanlı koşullu erişim sağlamak için milyarlarca sinyali kullanır. Azure Active Directory Privileged Identity Management ile ayrıca ayrıcalıklı hesaplarınızı yönetmek ve korumak için sunulan destek sayesinde yöneticileri ve onların kaynaklara erişimlerini keşfedebilir, kısıtlayabilir ve izleyebilir, gerektiğinde anlık erişim sağlayabilirsiniz.  

> [!NOTE]
> Bazı Azure Active Directory özellikleri "kullandıkça öde" sürümüne sahiptir:
>
> * Active Directory B2C, tüketicilere yönelik uygulamalarınız için kullanabileceğiniz erişim yönetimi çözümüdür. Daha fazla bilgi için bkz. [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure Multi-Factor Authentication, kullanıcı başına veya kimlik doğrulaması sağlayıcı başına kullanılabilir. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Kullanmaya nasıl başlayabilirim?

**BT yöneticisiyseniz:**

* [Deneyin!](https://azure.microsoft.com/trial/get-started-active-directory/) 30 günlük ücretsiz deneme sürümüne kaydolabilir ve bu bağlantıyı kullanarak ilk bulut çözümünüzü beş dakikadan kısa bir sürede dağıtabilirsiniz

* Azure AD kiracısını hızla kurma ve çalıştırma hakkında ipuçları ve püf noktaları için bkz. [Azure AD'yi kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)

**Geliştiriciyseniz:**
 
* Azure Active Directory [Geliştirici Kılavuzu](../develop/azure-ad-developers-guide.md)'na bakın

* [Ücretsiz deneme başlatın](https://azure.microsoft.com/trial/get-started-active-directory/): Bugün ücretsiz 30 günlük deneme için kaydolun ve uygulamalarınızı Azure AD ile tümleştirmeye başlayın

## <a name="next-steps"></a>Sonraki adımlar
[Azure kimlik ve erişim yönetiminin temelleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
