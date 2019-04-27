---
title: Ne olursa nedir, Azure Active Directory koşullu erişim aracı?
description: Koşullu erişim ilkelerinizi etkisini ortamınızda nasıl anlayabilirsiniz öğrenin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2018
ms.author: joflore
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6271c7a99e82fb1fff2815167b0149c74212f260
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60354341"
---
# <a name="what-is-the-what-if-tool-in-azure-active-directory-conditional-access"></a>Ne olursa nedir, Azure Active Directory koşullu erişim aracı?

[Koşullu erişim](../active-directory-conditional-access-azure-portal.md) olduğu bulut uygulamalarınızı Azure Active denetime nasıl sağlayan Directory (Azure AD) bir özellik yetkili kullanıcılara erişimi. Nasıl form beklenmesi gerekenler koşullu erişim ilkelerini ortamınızda bilebilirsiniz? Bu soruyu cevaplamak için kullanabileceğiniz **koşullu erişim durum aracı**.

Bu makalede, koşullu erişim ilkelerinizi test etmek için bu aracı nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="what-it-is"></a>Nedir?

**Ne koşullu erişim ilkesi aracı** koşullu erişim ilkelerinizi ortamınızda etkisini anlamanıza olanak tanır. İlkelerinizi test etmek için elle birden fazla oturum açma işlemi gerçekleştirmek yerine, bu aracı kullanarak bir kullanıcının oturum açmasının simülasyonunu yapabilirsiniz. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur. Rapor yalnızca uygulanan koşullu listelemez erişim ilkeleri de [Klasik ilkeleri](policy-migration.md#classic-policies) varsa.    

Araçlar da sağlar bir şekilde hızlı bir şekilde yenilikler belirli bir kullanıcı için uygulanan tüm ilkeler belirleyin. Bir sorunu gidermek gerekiyorsa, örneğin, bilgileri kullanabilirsiniz.  

## <a name="how-it-works"></a>Nasıl çalışır?

İçinde **koşullu erişim durum aracı**, önce senaryonun benzetimini yapmak istediğiniz oturum açma ayarlarını yapılandırmanız gerekir. Bu ayarlar şunlardır:

- Test etmek istediğiniz kullanıcı 

- Bulut uygulamaları kullanıcı erişme girişimi

- Erişimin koşullarda bulut yapılandırır uygulamaları gerçekleştirilir.
     
Sonraki adım olarak, ayarlarınızı değerlendirir bir simülasyon çalıştırma başlatabilirsiniz. Etkinleştirilen ilkeler, bir değerlendirme çalıştırın bir parçasıdır.


Değerlendirme sona erdiğinde, araç etkilenen ilkelerinin bir rapor oluşturur.



## <a name="running-the-tool"></a>Aracı çalıştırma

Bulabilirsiniz **ne** aracındaki **[koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)** Azure portalında sayfası.

Araç çubuğunda üzerinde ilkeleri listesinde aracı başlatmak için tıklatın **ne**.

![Ya](./media/what-if-tool/01.png)

Bir değerlendirmeyi çalıştırmadan önce ayarlarını yapılandırmanız gerekir.

## <a name="settings"></a>Ayarlar

Bu bölümde simülasyonu çalıştırma ayarları hakkında bilgi ile sağlar.

![Ya](./media/what-if-tool/02.png)


### <a name="user"></a>Kullanıcı

Yalnızca bir kullanıcı seçebilirsiniz. Bu yalnızca gerekli alandır.

### <a name="cloud-apps"></a>Bulut uygulamaları

Bu ayar için varsayılan değer **tüm bulut uygulamaları**. Varsayılan ayar tüm mevcut ilkelerin değerlendirme ortamınızda gerçekleştirir. Kapsamı belirli bulut uygulamaları etkileyen ilkeleri daraltabilirsiniz.


### <a name="ip-address"></a>IP adresi

Taklit etmek için tek bir IPv4 adresi IP adresidir [konum koşulu](location-condition.md). Adres, Internet'e yönelik adresi, kullanıcı tarafından oturum açmak için kullanılan cihazın temsil eder. Gezinme, bir cihazın IP adresi gibi doğrulayabilirsiniz [IP Adresim nedir](https://whatismyipaddress.com).    

### <a name="device-platforms"></a>Cihaz platformları

Bu ayar taklit eder [cihaz platformları koşul](conditions.md#device-platforms) ve denk temsil **tüm platformlar (desteklenmeyen dahil olmak üzere)**. 
### <a name="client-apps"></a>İstemci uygulamaları

Bu ayar taklit eden [istemci uygulamalar koşulunu](conditions.md#client-apps).
Varsayılan olarak, bu ayar tüm ilkelerin sahip bir değerlendirme neden olur. **tarayıcı** veya **mobil uygulamalar ve masaüstü istemciler** ya da tek tek veya her ikisini de seçildi. Ayrıca takım politikaları algıladığı **Exchange ActiveSync (EAS)**. Seçerek bu ayar daraltabilirsiniz:

- **Tarayıcı** en az olan tüm ilkelerini değerlendirme için **tarayıcı** seçili. 

- **Mobil uygulamalar ve masaüstü istemciler** en az olan tüm ilkelerini değerlendirme için **mobil uygulamalar ve masaüstü istemciler** seçili. 


### <a name="sign-in-risk"></a>Oturum açma riski

Bu ayar taklit eden [oturum açma riski koşuluna](conditions.md#sign-in-risk).   


## <a name="evaluation"></a>Değerlendirme 

Tıklayarak bir değerlendirme başlamadan **ne**. Değerlendirme sonucu oluşan bir rapor sağlar: 

![Ya](./media/what-if-tool/03.png)

- Bir gösterge olup, ortamınızda Klasik ilkeleri var
- Kullanıcı için geçerli olan ilkeler
- Kullanıcı için uygulama ilkeleri


Varsa [Klasik ilkeleri](policy-migration.md#classic-policies) seçilen bulut uygulamaları için mevcut, bir göstergesi size sunulur. Göstergenin tıklayarak Klasik ilkeleri sayfasına yönlendirilirsiniz. Klasik ilkeler sayfasında, bir Klasik ilke geçişi veya yalnızca devre dışı bırakabilirsiniz. Bu sayfa kapatarak, değerlendirme sonucu döndürebilir.

Listede seçilen kullanıcı için uygulanan ilkeler, bir listesini de bulabilirsiniz [verme denetimleri](controls.md#grant-controls) ve [oturumu](controls.md#session-controls) gerekir kullanıcınızın karşılamak denetimleri.

Kullanıcı için uygulama ilkeleri listesinde, oluşturabilir ve neden bu ilkeleri uygulama nedeniyle de bulabilirsiniz. Listelenen her ilke için neden olmayan karşılanan ilk koşul temsil eder. Daha fazla değerlendirilmez çünkü uygulanmaz bir ilke için bir olası neden devre dışı bırakılmış bir ilkedir.   



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).

- Ortamınızda koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz. [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 

- Klasik ilkeleri geçirme istiyorsanız bkz [Azure portalında Klasik ilkeleri geçirme](policy-migration.md)  
