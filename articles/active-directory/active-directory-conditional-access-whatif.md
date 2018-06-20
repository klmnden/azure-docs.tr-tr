---
title: Ne ne ise, Azure Active Directory koşullu erişim aracı? -Önizleme | Microsoft Docs
description: Koşullu erişim ilkelerinizi etkisini ortamınızda nasıl anlayabilirsiniz öğrenin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 661ada8de8821d489732e1f36dc2406eaa0ee4a7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232533"
---
# <a name="what-is-the-what-if-tool-in-azure-active-directory-conditional-access---preview"></a>Ne ne ise, Azure Active Directory koşullu erişim aracı? -Önizleme

[Koşullu erişim](active-directory-conditional-access-azure-portal.md) olan Azure Active denetimine nasıl sağlayan Directory (Azure AD) yeteneğini yetkili kullanıcılara erişimi, bulut uygulamalarınızı. Nasıl, ne form bekleneceği koşullu erişim ilkeleri, ortamınızda biliyor musunuz? Bu soruyu yanıtlamak için kullanabileceğiniz **koşullu erişim ne aracı**.

Bu makalede, koşullu erişim ilkelerinizi test etmek için bu aracı nasıl kullanabileceğiniz açıklanır.

## <a name="what-it-is"></a>Nedir?

**Ne yapmalı koşullu erişim ilkesi aracı** koşullu erişim ilkelerinizi ortamınızda etkisini anlamak sağlar. El ile çoklu oturum açma işlemleri gerçekleştirerek ilkelerinizi yürüten test yerine, bu araç, bir sanal oturum açma, bir kullanıcının değerlendirmek sağlar. Bu oturum açma etki ilkelerinizi sahiptir ve benzetimi raporunu oluşturur benzetimi tahmin eder. Rapor yalnızca uygulanan koşullu listelenmez ilkeleri de erişim [Klasik ilkeleri](active-directory-conditional-access-migration.md#classic-policies) varsa.    

Araçlar da sağlar olursa bir şekilde hızlı bir şekilde ne belirli bir kullanıcıya uygulanan ilkeler belirler. Bir sorunu gidermek gerekiyorsa, örneğin, bilgileri kullanabilirsiniz.  

## <a name="how-it-works"></a>Nasıl çalışır?

İçinde **koşullu erişim ne aracı**, ilk senaryonun benzetimini yapmak istediğiniz oturum açma ayarlarını yapılandırmanız gerekir. Bu ayarlar şunlardır:

- Test etmek istediğiniz kullanıcı 

- Kullanıcı erişme girişimi bulut uygulamaları

- Koşullar altında hangi erişimi bulut yapılandırır uygulamaları gerçekleştirilir
     
Sonraki adım olarak, ayarlarınızı değerlendiren çalıştırmak benzetimini başlatabilirsiniz. Etkinleştirilen ilkeler çalıştırmak değerlendirme bir parçasıdır.


Değerlendirme tamamlandığında, aracı etkilenen ilkeleri bir rapor oluşturur.


## <a name="running-the-tool"></a>Aracını çalıştırma

Bulabileceğiniz **ne** aracındaki **[koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)** Azure portalında sayfası.

Araç çubuğunda ilkelerin listesini üstünde aracını başlatmak için tıklatın **ne**.

![Ya](./media/active-directory-conditional-access-whatif/01.png)

Bir değerlendirme çalıştırmadan önce ayarlarını yapılandırmanız gerekir.

## <a name="settings"></a>Ayarlar

Bu bölümde benzetimi çalıştırma ayarları hakkında bilgi ile sağlar.

![Ya](./media/active-directory-conditional-access-whatif/02.png)


### <a name="user"></a>Kullanıcı

Yalnızca bir kullanıcı seçebilirsiniz. Bu yalnızca gerekli alandır.

### <a name="cloud-apps"></a>Bulut uygulamaları

Bu ayar için varsayılan değer **tüm bulut uygulamaları**. Varsayılan ayar, tüm kullanılabilir ilkelerin değerlendirme ortamınızda gerçekleştirir. Özel bulut uygulamaları etkileyen ilkeleri kapsama daraltabilirsiniz.


### <a name="ip-address"></a>IP adresi

Taklit etmek üzere tek bir IPv4 adresi IP adresidir [konumu koşul](active-directory-conditional-access-locations.md). Adresi oturum açmak için bir kullanıcı tarafından kullanılan aygıtın adresi Internet'e temsil eder. Gezinme, örneğin, bir aygıt tarafından IP adresini doğrulayabilirsiniz [IP adresimi nedir](https://whatismyipaddress.com).    

### <a name="device-platforms"></a>Cihaz platformları

Bu ayar taklit eder [cihaz platformları koşulu](active-directory-conditional-access-conditions.md#device-platforms) ve denk temsil eden **tüm platformlar (desteklenmeyen dahil olmak üzere)**. 
### <a name="client-apps"></a>İstemci uygulamaları

Bu ayar taklit eder [istemci uygulamaları koşul](active-directory-conditional-access-conditions.md#client-apps).
Varsayılan olarak, bu ayar, bir değerlendirme sahip tüm ilkelerin neden olur **tarayıcı** veya **mobil uygulamalar ve Masaüstü istemcileri** ya da tek başına veya her ikisini de seçili. Zorunlu ilkeleri de algılar **Exchange ActiveSync (EAS)**. Seçerek bu ayar daraltabilirsiniz:

- **Tarayıcı** en az olan tüm ilkelerini değerlendirmek için **tarayıcı** seçili. 

- **Mobil uygulamalar ve Masaüstü istemcileri** en az olan tüm ilkelerini değerlendirmek için **mobil uygulamalar ve Masaüstü istemcileri** seçili. 


### <a name="sign-in-risk"></a>Oturum açma riski

Bu ayar taklit eder [oturum açma riski koşul](active-directory-conditional-access-conditions.md#sign-in-risk).   


## <a name="evaluation"></a>Değerlendirme 

Tıklayarak bir değerlendirme Başlat **ne**. Değerlendirme sonucu oluşan bir rapor sağlar: 

![Ya](./media/active-directory-conditional-access-whatif/03.png)

- Bir gösterge olup, ortamınızda Klasik İlkesi yok
- Kullanıcıya uygulanan ilkeler
- Kullanıcı için uygulama ilkeleri


Varsa [Klasik ilkeleri](active-directory-conditional-access-migration.md#classic-policies) seçili bulut uygulamaları için mevcut, bir gösterge size sunulur. Göstergenin tıklayarak Klasik ilkeleri sayfasına yönlendirilirsiniz. Klasik ilkeleri sayfasında klasik bir ilke geçirmek ya da yalnızca devre dışı bırakın. Bu sayfayı kapatarak, değerlendirme sonucu geri dönebilirsiniz.

Listede seçilen kullanıcıya uygulanan ilkeler, ayrıca bir listesini bulabilirsiniz [denetimleri vermek](active-directory-conditional-access-controls.md#grant-controls) ve [oturum](active-directory-conditional-access-controls.md#session-controls) denetimleri kullanıcı karşılaması gerekir.

Kullanıcınız için uygulama ilkeleri listesi üzerinde olabilir ve ayrıca neden bu ilkeler uygulanmaz nedenleri bulun. Listelenen her ilke için bir nedenle karşılanmadı ilk koşul temsil eder. Daha fazla değerlendirilmez çünkü uygulanmaz bir ilke için bir olası neden devre dışı bırakılmış bir ilkedir.   



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory koşullu erişimi olan belirli uygulamalar için MFA gerektiren](active-directory-conditional-access-app-based-mfa.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 

- Klasik ilkeleri geçirmek istiyorsanız, bkz: [Azure portalında Klasik ilkelerine](active-directory-conditional-access-migration.md)  
