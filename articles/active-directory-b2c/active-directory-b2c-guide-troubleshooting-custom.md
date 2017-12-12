---
title: "Azure Active Directory B2C: özel ilke sorunlarını giderme | Microsoft Docs"
description: "Azure Active Directory'de özel ilkelerle çalışırken hataları çözmek yaklaşımlar hakkında bilgi edinin."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: joroja
ms.openlocfilehash: 8718f9c1dfce81682174eec11e8cbb731cbdf796
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Azure AD B2C özel ilkeleri ve kimlik deneyimi Framework sorun giderme

Azure Active Directory B2C kullanıyorsanız (Azure AD B2C) özel ilkeler, ilke dil XML biçiminde kimlik deneyimi Framework ayarlama zorluklar deneyimi.  Özel ilkeler yazmak öğrenme yeni bir dil öğrenme gibi olabilir. Bu makalede, biz araçları açıklamak ve hızlı bir şekilde Yardım ipuçları bulmak ve sorunları çözün. 

> [!NOTE]
> Bu makalede, Azure AD B2C özel ilke yapılandırma sorunlarını gidermeye üzerine odaklanır. Bağlı olan taraf uygulaması veya kendi kimlik kitaplığı adresi değil.

## <a name="xml-editing"></a>XML düzenleme

Özel ilkelerini ayarlama en sık karşılaşılan hata yanlış olan XML biçimli. İyi bir XML Düzenleyicisi neredeyse gereklidir. İyi bir XML Düzenleyicisi XML yerel olarak görüntüler, içerik renk kodları, Ortak terimleri doldurur, XML öğeleri dizini tutar ve şemasıyla doğrulayabilirsiniz. İki bizim sık kullanılan XML düzenleyicileri şunlardır:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Not Defteri'ni ++](https://notepad-plus-plus.org/)

XML dosyanızı karşıya yüklemeden önce XML Şeması doğrulama hataları tanımlar. Başlangıç paketi kök klasöründe XML şema tanımı TrustFrameworkPolicy_0.3.0.0.xsd alın. XML düzenleyicinizi belgelerinde daha fazla bilgi aramak *XML araçları* ve *XML doğrulama*.

XML kuralları gözden yararlı bulabilirsiniz. Azure AD B2C algıladığı hataları biçimlendirme XML reddeder. Bazen, hatalı biçimlendirilmiş XML yanıltıcı hata iletileri neden olabilir.

## <a name="upload-policies-and-policy-validation"></a>İlkeleri ve ilke doğrulaması karşıya yükle

 XML dosya karşıya yükleme doğrulaması otomatik olarak yapılır. Çoğu hatalara karşıya yükleme başarısız olmasına neden olabilir. Doğrulama karşıya yüklemekte olduğunuz ilke dosyası içerir. Ayrıca, dosyaları karşıya yükleme dosyasını (bağlı olan taraf ilke dosyası, uzantıları dosyası ve temel dosyanın) başvuruyor zinciri içerir. 
 
 Sık karşılaşılan doğrulama hataları aşağıda verilmiştir.

Hata parçacığını:`... makes a reference to ClaimType with id "displaName" but neither the policy nor any of its base policies contain such an element`
* ClaimType değeri yanlış yazılmış veya şemada yok.
* ClaimType değerleri en az bir ilke dosyalarında tanımlanmalıdır. 
    Örneğin, ` <ClaimType Id="socialIdpUserId">`
* ClaimType uzantıları dosyasında tanımlı, ancak ayrıca temel dosyanın TechnicalProfile değerindeki kullanılır, temel dosyanın karşıya bir hatayla sonuçlanır.

Hata parçacığını:`...makes a reference to a ClaimsTransformation with id...`
* Hatanın nedeni ClaimType hata aynı olması.

Hata parçacığını:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order to manage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Tenantıd değeri onay  **\<TrustFrameworkPolicy\>**  ve  **\<BasePolicy\>**  öğeleri eşleşen hedef Azure AD B2C kiracınızın.  

## <a name="troubleshoot-the-runtime"></a>Çalışma zamanı sorun giderme

* Kullanım `Run Now` ve `https://jwt.io` ilkelerinizi bağımsız olarak, web veya mobil uygulamanızı test etmek için. Bu Web sitesine bir bağlı olan taraf uygulaması gibi davranır. İçeriği, JSON Web Token (Azure AD B2C İlkesi tarafından oluşturulan JWT) görüntüler. Kimlik deneyimi Framework bir test uygulaması oluşturmak için aşağıdaki değerleri kullanın:
    * Ad: TestApp
    * Web uygulaması/Web API: Hayır
    * Yerel istemci: Hayır

* İstemci tarayıcısına ve Azure AD B2C arasında ileti alışverişi izlemek için kullanımı [Fiddler](http://www.telerik.com/fiddler). Kullanıcı Yolculuğunuzun orchestration adımlarınızı nerede başarısız bir gösterge size yardımcı olabilir.

* İçinde **geliştirme modunu**, kullanın **Application Insights** kimlik deneyimi Framework kullanıcı Yolculuğunuzun etkinliğini izlemek için. İçinde **geliştirme modunu**, kimlik deneyimi Framework ve kimlik sağlayıcısı, API tabanlı hizmetler, Azure AD B2C kullanıcı dizini ve Azure çok / multi-Factor-Authentication gibi diğer hizmetler gibi teknik profillerini tarafından tanımlanan çeşitli talep sağlayıcıları arasında taleplerin exchange görebilirsiniz.  

## <a name="recommended-practices"></a>Önerilen uygulamalar

**Birden fazla sürümünü senaryolarınızı tutun. Uygulamanız ile projesinde gruplandırın.** Temel, uzantıları ve bağlı olan taraf dosyaları birbirlerine doğrudan bağımlı. Bir grup olarak kaydedin. Yeni özellikler, ilkeler eklendikçe ayrı çalışma sürümleri tutun. Aşama çalışma sürümlerinde kendi dosya sistemi ile etkileşim uygulama kodu.  Uygulamalarınızın birçok farklı bağlı olan taraf ilkelerinde Kiracı çağırabilir. Bunlar, Azure AD B2C ilkelerden bekledikleri talep bağımlı hale gelebilir.

**Geliştirme ve bilinen kullanıcı Yolculuklar teknik profilleriyle sınayın.** Teknik profillerini kurmak için test edilmiş başlangıç paketi ilkelerini kullanın. Bunları ayrı olarak kendi kullanıcı Yolculuklar dahil önce test edin.

**Geliştirme ve test edilmiş teknik profilleriyle kullanıcı Yolculuklar sınayın.** Bir kullanıcı gezisine orchestration adımları artımlı olarak değiştirin. Aşamalı olarak hedeflenen senaryolarınızı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* Github'da, [active-directory-b2c-custom-policy-starterpack] (https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) .zip dosyasını indirin.
