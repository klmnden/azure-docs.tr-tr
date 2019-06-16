---
title: Azure Market SaaS uygulamaları teknik yayımlama Kılavuzu
description: Adım adım kılavuz ve SaaS uygulamaları için Azure Marketi yayımlama yayımlama denetim listeleri
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
author: keithcharlie
ms.service: marketplace
ms.topic: article
ms.date: 07/09/2018
ms.author: keithcharlie
ms.openlocfilehash: 4501a343b406f07b4775f3ad0e84d71825412a4b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752732"
---
# <a name="saas-applications-offer-publishing-guide"></a>SaaS uygulamaları sunma yayımlama Kılavuzu

SaaS uygulamaları, üç farklı eylem çağrı Market'te yayımlanabilir: "Benimle iletişim kurun," "Şimdi deneyin" ve "şimdi edinin." Bu kılavuz, her biri için gereksinimleri dahil olmak üzere bu üç seçenek açıklar. 

## <a name="offer-overview"></a>Teklif genel bakış  

SaaS uygulamalarını hem Azure Vitrinler geçerli kullanılabilir seçenekler aşağıdaki tabloda açıklanmıştır:

| StoreFront seçeneği | Listeleme | Deneme/Transact |  
| --- | --- | --- |  
| AppSource | Evet (benimle iletişime geçin) | Evet (Power BI/Dynamics) |
| Azure Market | Hayır | Evet (SaaS uygulamaları) |   

**Listesi:**  Bir kişi bana Teklif türü ve deneme veya işlem düzeyi katılım uygun olmadığı durumlarda kullanılır liste yayımlama seçeneği oluşur. Bu yaklaşımın avantajı işletmenizi artırmak için anlaşmalar kapatılabilir müşteri adayları alma hemen başlamak bir çözüm içinde pazara açılma olan yayımcı sunmasıdır.  
**Deneme/işlem:**  Müşteri, doğrudan satın almanız veya çözümünüz için bir deneme sürümü iste seçeneği vardır. Bir deneme sürümü deneyimi sağlama çalışmaları müşterilerine sunulan katılım düzeyini artırır ve satın almadan önce çözümünüzü incelemek müşterilerin sağlar. Bir deneme sürümü deneyimi ile vitrinler yükseltme daha iyi olasılığı vardır ve müşterilerle yaşadığımız daha zengin ve daha fazla müşteri adaylarını beklemelisiniz. Deneme ücretsiz destek deneme süresi boyunca en az içermelidir.  

| SaaS uygulamaları teklifi | İş Gereksinimleri | Teknik Gereksinimler |  
| --- | --- | --- |  
| **Bize Ulaşın** | Evet | Hayır |  
| **Power BI / Dynamics** | Evet | Evet (Azure AD tümleştirmesi) |  
| **SaaS Uygulamaları**| Evet | Evet (Azure AD tümleştirmesi) |     

## <a name="saas-list"></a>SaaS listesi

Bir SaaS dökümüyle hiçbir deneme ve fatura hiçbir işlevsellik için eylem çağrısı "Kişi Me." olan 

Bir SaaS uygulaması listelemek için Azure Active Directory'yi yapılandırma gerekmez. 

|Gereksinimler  |Ayrıntılar  |
|---------|---------|
|Uygulamanız bir SaaS teklifidir.  |   Sunan bir SaaS çözümünüz olduğu ve çok kiracılı bir SaaS ürün sunar.      |


## <a name="saas-trial"></a>SaaS denemesi

Bir çözüm veya bir boş--çalışırsanız, hizmet olarak yazılım (SaaS) kullanarak app-deneme bağlı. Ücretsiz deneme teklifleri sınırlı kullanımlı veya sınırlı süreli bir deneme hesabı olarak sunulan. 


|Gereksinimler  |Ayrıntılar  |
|---------|---------|
|Uygulamanız bir SaaS teklifidir.  |   Sunan bir SaaS çözümünüz olduğu ve çok kiracılı bir SaaS ürün sunar.      |
|Uygulamanız AAD etkin değil     |   Müşteri etki alanınıza yeniden yönlendirilmiş ve müşteri ile doğrudan transact       |


## <a name="saas-trial-technical-requirements"></a>SaaS deneme teknik gereksinimler

SaaS uygulamalarına yönelik teknik gereksinimler basittir. Yayımcılar, yalnızca Azure Active yayımlanacak Directory'nizle (Azure AD) tümleştirilebilmesi için gereklidir. Azure AD tümleştirmesi uygulamalarıyla iyi belgelenmiştir ve SDK'ları ve kaynakları bunu gerçekleştirmek için Microsoft sağlar.  

Başlatmak için diğer girişimler işten yalıtmak olanak tanıyan, Azure Marketi'nde yayımlama için adanmış bir aboneliğe sahip olmanızı öneririz. Bunu yaptıktan sonra SaaS uygulamanızı geliştirme işini başlatmak için bu abonelikte dağıtmaya başlayabilir.  

Aşağıdaki sitelerde en iyi Azure Active Directory belgeler, örnekler ve yönergeler bulunur: 

* [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

* [Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

* [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

* [Azure yol haritası - güvenlik ve kimlik](https://azure.microsoft.com/roadmap/?category=security-identity)

Video öğreticiler için aşağıdakileri gözden geçirin:

* [Azure Active Directory kimlik doğrulaması ile Vittorio Bertocci'nin](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Azure Active Directory kimlik teknik bilgilendirme - bölüm 1 / 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Azure Active Directory kimlik teknik bilgilendirme - 2 2. Bölüm](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Microsoft Azure Active Directory ile uygulamaları oluşturma](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Microsoft Azure Active Directory odaklanmış videoları](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Ücretsiz Azure Active Directory eğitim kullanılabilir  
* [Microsoft Azure için BT uzmanları içerik serisi: Azure Active Directory](https://mva.microsoft.com/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Ayrıca, Azure Active Directory Hizmet güncelleştirmeleri denetlemek için site sağlar   
* [Azure AD hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)

## <a name="using-azure-active-directory-to-enable-trials"></a>Deneme için Azure Active Directory kullanma  

Microsoft kimlik doğrulaması ile Azure AD, bu nedenle tüm Market kullanıcılarının kimliği doğrulanmış bir kullanıcı Market'teki deneme listenizi aracılığıyla tıkladığında ve deneme ortamınıza yeniden yönlendirildiğinde, gerek kalmadan kullanıcıya doğrudan bir deneme sağlayabilirsiniz bir ek oturum açma adımı. Uygulamanız kimlik doğrulaması sırasında Azure AD'den aldığı belirteci sağlama deneyimini otomatikleştirmek ve dönüştürme olasılığını artırmak sağlayarak uygulamanızı, bir kullanıcı hesabı oluşturmak için kullanabileceğiniz değerli kullanıcı bilgilerini içerir. Belirteç hakkında daha fazla bilgi için bkz: [örnek belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims) .

Uygulamanızı veya deneme 1-tıklatma kimlik doğrulamasını etkinleştirmek için Azure AD kullanarak şunları yapar:  
* Deneme sürümü için müşteri deneyimini Market kolaylaştırır.  
* 'Ürün içi deneyimi' görünümünü tutar bile, kullanıcı Market'ten, etki alanı veya deneme ortamı yönlendirilir.  
* Ek bir oturum açma adımına olmadığından yeniden yönlendirme üzerinde abandonment olasılığını azaltır.  
* Azure AD kullanıcılarının büyük popülasyonu dağıtım engellerini azaltır.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Azure AD tümleştirmenizi Market için sertifikalandırma  

Uygulamanız tek kiracılı veya çok kiracılı olup olmamasına bağlı olarak birkaç farklı şekilde, Azure AD tümleştirmesi onaylamak ve yeni için Azure AD Federasyon çoklu oturum açma (SSO)'yı mı zaten desteklemektedir.  

**Çok kiracılı uygulamalar için:**  

Azure AD destekliyorsa, aşağıdakileri yapın:
1.  Azure portalında uygulamanızı kaydetme
2.  Çok kiracılı desteği özelliği 'tek tıklamayla' deneme sürümü deneyimi almak için Azure AD'de etkinleştirin. Daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Azure AD Federasyon SSO için yeni başladıysanız, aşağıdakileri yapın: 
1.  Azure portalında uygulamanızı kaydetme
2.  Azure AD kullanarak SSO geliştirme [Openıd Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) veya [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Çok kiracılılık desteğini etkinleştir 'tek tıklamayla' deneme sürümü deneyimi daha ayrıntılı bilgi almak için AAD özelliğinde bulunabilir [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Tek kiracılı bir uygulama için aşağıdaki seçeneklerden birini kullanın:**  
* Kullanıcılar Konuk kullanıcılar'ı kullanarak dizininize eklemek [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
* 'Benimle iletişim kurun' kullanarak denemeler müşteriler için el ile sağlama
* Bir başına Müşterisi 'Test Sürüşü' geliştirin
* SSO ile çok kiracılı örnek bir demo uygulaması derleme

## <a name="saas-subscriptions"></a>SaaS abonelikler

SaaS tabanlı, teknik çözümünüz olarak bir abonelik satın almak müşteri etkinleştirmek için SaaS uygulama Teklif türü kullanın. SaaS uygulamanız için aşağıdaki gereksinimler karşılanmalıdır:
- Fiyat ve faturanızı hizmet düz, aylık fiyatı.
- Yükseltme veya hizmet dilediğiniz zaman iptal etmek için bir yöntem sağlar.
Microsoft commerce işlemi'ni barındırır. Microsoft, müşterinizin sizin adınıza düzenler. Bir SaaS uygulaması bir abonelik kullanmak için kendi Abonelik Yönetim hizmeti API'si etkinleştirmeniz gerekir. Abonelik Yönetimi Hizmeti API'nizi, Azure Resource Manager API'leri ile doğrudan iletişim kurması gerekir. Abonelik Yönetimi Hizmeti API, hizmet sağlama, yükseltme ve iptal etme desteklemesi gerekir.

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
|Faturalama ve ölçüm | Teklifiniz, aylık bir sabit ücretle fiyatlandırılır. Kullanım tabanlı fiyatlandırma ve kullanım tabanlı "true-yukarı" özellikleri şu anda desteklenmiyor. |  
|İptal etme | Teklifinizi istediğiniz zaman müşteri tarafından iptal edilebilir. |  
|İşlem giriş sayfası | Kullanıcılar nerede oluşturabilir ve SaaS hizmet hesaplarını yönetmek bir Azure ortak markalı işlem giriş sayfası barındırabileceğiniz. |   
| Abonelik API | Oluşturmak, güncelleştirmek ve bir kullanıcı hesabı ve hizmet planını silmek için SaaS abonelikle etkileşime hizmet kullanıma sunar. 24 saat içinde kritik API değişiklikleri desteklenmesi gerekir. Kritik olmayan API değişiklikleri düzenli olarak kullanıma sunulacaktır. |  

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](./cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="next-steps"></a>Sonraki adımlar
Zaten yapmadıysanız,

- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te.

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com) oluşturmak veya teklifiniz tamamlayın.
- Bkz: [Azure SaaS uygulaması teklif](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer) daha fazla bilgi için.
