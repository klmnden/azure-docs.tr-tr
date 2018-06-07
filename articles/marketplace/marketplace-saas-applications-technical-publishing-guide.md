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
ms.date: 05/09/2018
ms.author: pabutler
ms.openlocfilehash: 2ac8119e36843e38e334fb5772ea4ade9962b4f9
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809480"
---
# <a name="saas-applications-technical-publishing-guide"></a>SaaS uygulamaları teknik yayımlama Kılavuzu

Azure Market SaaS uygulamaları teknik Kılavuzu yayımlama için Hoş Geldiniz. Bu kılavuz, aday ve varolan yayımcılar uygulamaları ve hizmetleri sunan SaaS uygulamaları kullanarak Azure markette listelemek için yardımcı olmak için tasarlanmıştır. 

SaaS teklif yayımlamak nasıl daha iyi anlamak için bu kılavuz aşağıdaki bölümlere ayrılır:
* Teklif genel bakış
* İş Gereksinimleri
* Teknik Gereksinimler
* Yayımlama işlemi
* Denemeler etkinleştirmek için Azure Active Directory kullanma
* Market, Azure AD tümleştirme onaylıyor

## <a name="offer-overview"></a>Teklif genel bakış  

SaaS uygulamaları hem Azure veriş kullanılabilir geçerli kullanılabilir seçenekleri aşağıdaki tabloda açıklanmaktadır:

| StoreFront seçeneği | Listeleme | Deneme ve Transact |  
| --- | --- | --- |  
| AppSource | Evet (benimle iletişim) | Evet (Powerbı/Dynamics) |
| Azure Market | Hayır | Evet (SaaS uygulamaları) |   

**Liste:** listeleme yayımlama seçeneği bir kişi benim Teklif türü oluşur ve deneme veya işlem düzeyi katılım uygun olmadığında kullanılır. Bu yaklaşımın avantajı, işletmenizin artırmak için anlaşmalar açık müşteri adayları almaya hemen başlamak bir çözüm temelinde piyasaya olan yayımcı etkinleştirir ' dir.  
**Deneme/Transact:** müşteri doğrudan satın alın veya deneme çözümünüz için istek seçeneğine sahiptir. Bir deneme sürümü deneyimi sağlama müşterilerine sunulan katılım düzeyini artırır ve satın almadan önce çözümünüzü incelemek müşterilerin sağlar. Bir deneme sürümü deneyimi ile daha iyi olasılığını yükseltme veriş sahip olur ve müşteri katılımlar daha zengin ve daha fazla müşteri adaylarını beklemelisiniz. Denemeler ücretsiz destek en az ve deneme süresi boyunca eklemeniz gerekir.  

| SaaS uygulamaları teklif | İş Gereksinimleri | Teknik Gereksinimler |  
| --- | --- | --- |  
| **Bizimle bağlantı kurun** | Evet | Hayır |  
| **Powerbı / Dynamics** | Evet | Evet (Azure AD tümleştirmesi) |  
| **SaaS uygulamaları**| Evet | Evet (Azure AD tümleştirmesi) |     

Market veriş ve descripiton her yayımlama seçeneği hakkında daha fazla bilgi için bkz: [Market yayımcı Kılavuzu](https://aka.ms/sellerguide) ve [yayımlama seçeneklerini](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide#select-a-publishing-option).

## <a name="business-requirements"></a>İş gereksinimleri
SaaS teknik gereksinimleri ile paralel iş gereksinimlerini tamamlanabilir sunar. Bulut iş ortağı portalında SaaS teklif oluştururken birçok iş gereksinimlerini ve bilgileri toplanır. İş gereksinimleri şunlardır: 
* Katılım ilkeleri kabul ettiğinizi belirten
* Microsoft ile tümleştirme 
* Teklif'ın İzleyici tanımlayın
* Kullanılacak sağlama yönetim belirlemek ve tanımlayın
* Gizlilik İlkesi ve kullanım koşulları ayarlama
* Destek kişileri tanımlama  

Daha fazla bilgi için konu başlığı altında bulunabilir [Market yayımlama için Önkoşullar](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide#prerequisites-for-marketplace-publishing)

## <a name="technical-requirements"></a>Teknik gereksinimler

SaaS uygulamaları için teknik gereksinimleri basittir. Yayımcılar yalnızca Azure Active yayımlanmasını dizinle (Azure AD) tümleşik gerekir. Azure AD tümleştirme uygulamalarla belgelendiğinden ve birden çok SDK'ları ve kaynakları bunu gerçekleştirmek için Microsoft sağlar.  

Başlatmak için diğer girişimleri işten yalıtmak sağlayarak, Azure Marketi'nde yayımlama için adanmış bir aboneliğinizin olması önerilir. Bunu yaptıktan sonra SaaS uygulamanızın bu abonelikte geliştirme iş başlangıç dağıtmaya başlatabilirsiniz.  

En iyi Azure Active Directory belgelerindeki, örnekler ve yönergeler aşağıdaki sitelerde bulunan: 

* [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

* [Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

* [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

* [Azure yol haritası - güvenlik ve kimlik](https://azure.microsoft.com/roadmap/?category=security-identity)

Video öğreticileri için aşağıdakileri gözden geçirin:

* [Vittorio Bertocci ile Azure Active Directory kimlik doğrulaması](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Azure Active Directory kimlik teknik brifing - bölüm 1 / 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Azure Active Directory kimlik teknik brifing - Kısım 2 / 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Microsoft Azure Active Directory ile uygulamaları oluşturma](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Microsoft Azure Active Directory odaklanmış videolar](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Ücretsiz Azure Active Directory eğitim şu adresten edinilebilir  
* [BT uzmanları içerik seri için Microsoft Azure: Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Ayrıca, Azure Active Directory Hizmet güncelleştirmeleri denetlemek için bir site sağlar   
* [Azure AD hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)

Desteği için aşağıdaki kaynakları kullanabilirsiniz:
* [MSDN Forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="publishing-process"></a>Yayımlama işlemi

İşlem yayımlama SaaS teknik ve işle adım vardır.  Geliştirme ve Azure Active Directory Tümleştirme yapılır işlerin çoğunu teklif iş gereksinimlerini karşılamak için gerekli iş paralel yapılabilir. Toplu iş gereksinimlerini bulut iş ortağı portalını SaaS uygulama teklifini yapılandırmasının bir parçası olarak.  
Aşağıdaki diyagramda, deneme/Transact ana yayımlama adımları sunar gösterilmektedir:  

![SaaS yayımlama adımları](./media/marketplace-saas-applications-technical-publishing-guide/saaspublishingsteps.png)  

Aşağıdaki tabloda ana yayımlama adımların her biri açıklanmaktadır:  

| Yayımlama adımı | Açıklama |   
| --- | --- |  
| **SaaS uygulaması oluşturma** | Bulut iş ortağı portalını seçin oturum açma **yeni**ve ardından **SaaS uygulamaları** sunar. |  
| **Azure AD ile tümleştirme oluşturma** | Azure AD ile sunumu, SaaS tümleştirmek için önceki bölümde açıklanan teknik gereksinimleri izleyin. |  
| **Teklif ayarlarını belirleme**| Tüm SaaS teklif ilk bilgileri girin. Teklif kimliği ve kullanmak istediğiniz adı sunar. |     
| **Teknik bilgiler ayarlayın** | Teklif hakkında teknik bilgi girin. SaaS uygulamaları için çözümün URI ve teklif'ın edinme düğmesinin (ücretsiz, izleme veya kişi benim) türü gereklidir. |  
| **Test Drive(Optional)** | Bu, deneme, çoğunlukla diğer türleri, Market sunar için gereken isteğe bağlı bir türde değil. Publisher'ın Aboneliklerde son müşteriye karşılaştırması dağıtılan deneme sahip olmanızı sağlar. |  
| **Teklif Storefront malzemeleri ayarlayın**| Bu bölümde publisher bağlamak ve pazarlama malzemeleri, yasal belgeler logolar, karşıya yükleme ve müşteri adayları yönetim sistemi yapılandırın. |
| **Teklif kişiler ayarlayın** | Mühendislik kişiler ve SaaS teklifin destek iletişim bilgileri girin. |  
| **SaaS uygulama Azure AD tümleştirme doğrulayın** | SaaS uygulamanızı yayımlama göndermeden önce uygulamayı Azure AD ile tümleşiktir doğrulamalısınız |  
| **Teklifi yayımlama**| Teklif ve teknik varlıkları tamamlandıktan sonra teklifi gönderebilirsiniz. Bu, çözüm şablonu test, doğrulanır, sertifikalı ve yayımlama için onaylanmış yayımlama işlemi başlatır. |

## <a name="using-azure-active-directory-to-enable-trials"></a>Denemeler etkinleştirmek için Azure Active Directory kullanma  

Microsoft kimliği doğrulanmış bir kullanıcı, deneme listesindeki Market üzerinden tıkladığında ve deneme ortamınıza yönlendirildiği Azure AD, bu nedenle ile tüm Market kullanıcıların kimliğini doğrular, gerek kalmadan doğrudan bir deneme sürümü'nü kullanıcı sağlayabilirsiniz bir ek oturum açma adımı. Uygulamanızı Azure AD kimlik doğrulama sırasında alan belirteci sağlama deneyimi otomatikleştirmek ve dönüştürme olasılığını artırmak etkinleştirme uygulamanızda, bir kullanıcı hesabı oluşturmak için kullanabileceğiniz değerli kullanıcı bilgilerini içerir. Belirteç hakkında daha fazla bilgi için bkz: [örnek belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims) .

Uygulamanızı veya deneme için 1-tıklatma kimlik doğrulamasını etkinleştirmek için Azure AD kullanarak şunları yapar:  
* Market müşteri deneyimine deneme kolaylaştırır.  
* 'Ürün Deneyimi' yapısını tutar bile zaman kullanıcı Market'ten, etki alanı veya deneme ortamı yönlendirilir.  
* Ek bir oturum açma adımı olmadığından yeniden yönlendirme üzerinde abandonment olasılığını azaltır.  
* Azure AD kullanıcıları büyük popülasyonunu dağıtım engelleri azaltır.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Market, Azure AD tümleştirme onaylıyor  

Uygulamanızın tek Kiracı ya da çok kiracılı olup olmamasına bağlı olarak birkaç farklı şekilde, Azure AD tümleştirme onaylamak ve yeni Azure AD Federasyon çoklu oturum açma (SSO) özelliğini mı zaten destekler.  

**Çok kiracılı uygulamalar için:**  

Azure AD destekliyorsa, aşağıdakileri yapın:
1.  Azure portalında uygulamanızı kaydetme
2.  'Tek tıklatmayla' deneme sürümü deneyimi almak için Azure AD çoklu kiracı destek özelliğini etkinleştirin. Daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Azure AD Federasyon SSO için yeniyseniz, aşağıdakileri yapın: 
1.  Azure portalında uygulamanızı kaydetme
2.  Azure AD kullanarak SSO geliştirmek [Openıd Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) veya [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Çoklu kiracı desteğini etkinleştir 'tek tıklatmayla' deneme sürümü deneyimi daha ayrıntılı bilgi almak için AAD özelliğinde bulunabilir [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Tek kiracılı uygulama için aşağıdaki seçeneklerden birini kullanın:**  
* Kullanıcılar, Konuk kullanıcılar olarak kullanarak dizininize eklemek [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
* El ile sağlama denemeler 'Kişi benim' kullanarak müşteriler için
* Başına-Müşteri 'Test sürücü' geliştirin
* Çok kiracılı örnek Tanıtım uygulamasını SSO ile derleme

