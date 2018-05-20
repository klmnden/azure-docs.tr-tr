---
title: Azure AD B2C'de yaş geçişi kullanarak | Microsoft Docs
description: Uygulamanızı kullanarak reşit olmayanların tanımlama hakkında bilgi edinin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.openlocfilehash: 9186579126525cc269f7e3f9e778e06902b30eb4
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
#<a name="using-age-gating-in-azure-ad-b2c"></a>Azure AD B2C'de yaş geçişi kullanma

>[!IMPORTANT]
>Bu özellik, özel önizlemede değil.  Lütfen bakın bizim [hizmet blog](https://blogs.msdn.microsoft.com/azureadb2c/) ayrıntılar bu olarak için kullanılabilir veya kişi hale AADB2CFeedback@microsoft.com.  Bu üretim dizinleri kullanmaz, bu yeni özellikleri kullanma ve veri kaybına neden olabilir biz genel kullanılabilirlik oluncaya kadar davranış beklenmeyen değişiklikler.  
>

##<a name="age-gating"></a>Geçerlilik süresi geçişi
Geçerlilik süresi geçişi, uygulamanızda reşit olmayanların tanımlamak için Azure AD B2C kullanmanıza olanak sağlar.  Kullanıcının uygulamaya açmasını Engelle veya izin vermeniz uygulama kullanıcının yaş grubu ve ebeveyn izni durumlarını tanımlamak ek talepleri ile dönün seçebilirsiniz.  

>[!NOTE]
>Ebeveyn izni adlı bir kullanıcı özniteliği izlenen `consentProvidedForMinor`.  Bu özelliği grafik API'si aracılığıyla güncelleştirebilir ve güncelleştirirken bu alanı kullanın `legalAgeGroupClassification`.
>

##<a name="setting-up-your-directory-for-age-gating"></a>Dizininizi yaş geçişi için ayarlama
Bir kullanıcı akışı yaş geçişi kullanmak için ek özellikler sağlamak için dizininizin yapılandırmanız gerekir. Bu işlem aracılığıyla yapılabilir `Properties` (yalnızca özel Önizleme parçası iseniz kullanıma sunulacak) menüden.  
1. Azure AD B2C uzantısı'nda tıklatın **özellikleri** soldaki menüde kiracınız için.
2. Altında **yaş geçişi** bölümünde, tıklayın **yapılandırma** düğmesi.
3. İşlemin tamamlanmasını bekleyin ve dizininize yaş geçişi sağlamak için ayarlanır.

##<a name="enabling-age-gating-in-your-user-flow"></a>Geçerlilik süresi, kullanıcı akışınızı geçişi etkinleştirme
Geçerlilik süresi geçişi kullanmak için dizininizin ayarladıktan sonra bu özellik Önizleme sürümü kullanıcı akışlarında sonra kullanabilirsiniz.  Bu özellik var olan kullanıcı akışları türleriyle uyumsuz yaptığınız değişiklikler gerektirir.  Aşağıdaki adımlarla yaş geçişi etkinleştirin:
1. Önizleme kullanıcı akışı oluşturun.
2. Oluşturulduktan sonra Git **özellikleri** menüde.
3. İçinde **yaş geçişi** bölümünde, özelliğini etkinleştirmek için Değiştir'e basın.
4. Sonra nasıl reşit olmayanların tanımlamak kullanıcıları yönetmek istediğinizi seçebilirsiniz.

##<a name="what-does-enabling-age-gating-do"></a>Geçerlilik süresi geçişi etkinleştirme ne yapar?
Geçerlilik süresi geçişi kullanıcı akışınız etkinleştirildikten sonra kullanıcı değişiklikleri karşılaşırsınız.  Oturum açma, kullanıcıları artık kendi doğum tarihi ve kullanıcı akışı için yapılandırılmış kullanıcı özniteliklerini birlikte ikamet ettiğiniz ülke istenir.  Oturum açma, oturum bu bilgiler sonraki için daha önce doğum tarihi ve ikamet ettiğiniz ülke girmediyseniz kullanıcılar istenir.  Bu iki değerden Azure AD B2C kullanıcının bir küçük olup olmadığını belirlemek ve güncelleştirme `ageGroup` alanın değeri olabilir `null`, `Undefined`, `Minor`, `Adult`, ve `NotAdult`.  `ageGroup` Ve `consentProvidedForMinor` alanları hesaplamak için kullanılan sonra `legalAgeGroupClassification`. 

##<a name="age-gating-options"></a>Seçenekler geçişi yaşı
Azure AD B2C blok reşit olmayanların ebeveyn izni olmadan veya bunlara izin ve bunlarla yapmanız gerekenler hakkında kararlar uygulama seçebilirsiniz.  

###<a name="allowing-minors-without-parental-consent"></a>Ebeveyn izni olmadan reşit olmayanların izin verme
Yukarı ya da oturum izin, oturum kullanıcı akışları veya her ikisi için izniniz olmadan reşit olmayanların uygulamanıza izin vermeyi seçebilirsiniz.  Ebeveyn izni olmadan reşit olmayanların için bunlar oturum açın veya bir kimliği belirteciyle normal ve Azure AD B2C sorunları gibi kaydolun izin verilmez `legalAgeGroupClassification` talep.  Bu kullanıcıların deneyimini seçebilirsiniz bu talep kullanarak gibi ebeveyn izni toplamak için bir deneyim giderek (ve güncelleştirme `consentProvidedForMinor` alan).

###<a name="blocking-minors-without-parental-consent"></a>Ebeveyn izni olmadan reşit olmayanların engelleme
Yukarı ya da oturum izin, oturum kullanıcı akışları veya her ikisi için reşit olmayanların uygulamasından izniniz olmadan engellemeyi seçebilirsiniz.  Azure AD B2C engellenen kullanıcılar işlemek için iki seçenek vardır:
* JSON uygulamasına geri göndermek - bu seçenek bir ikincil engellendiğini uygulama geri yanıt gönderir.
* Kullanıcı uygulamaya erişemezsiniz bildiren bir sayfası gösterilecek bir hata sayfası - Göster

##<a name="known-issues"></a>Bilinen sorunlar
###<a name="format-for-the-response-when-a-minor-is-blocked"></a>Bir ikincil engellendiğinde, yanıt biçimi.
Yanıt şu anda doğru biçimlendirilmemiş, bu hatayı gelecek bir güncelleştirmede ele alınacaktır.

###<a name="deleting-specific-attributes-that-were-added-during-setup-can-make-your-directory-unable-to-use-age-gating"></a>Kurulum sırasında eklenen özel öznitelikler silme dizininize yaş geçişi kullanamadı yapabilirsiniz.
Kurulum yaş geçişi için yapılandırdığınız dizininize aracılığıyla bir seçenek olarak, `Properties`.  Ya da silerseniz `legalCountry` veya `dateOfBirth` grafik dizininize artık yaş geçişi kullanabilirsiniz ve bu özellikleri yeniden oluşturulamaz.

###<a name="list-of-countries-is-incomplete"></a>Ülkelerin listesi eksik
Şu anda legalCountry ülkelerin listesi tam değil, gelecek bir güncelleştirmede ülkelerin kalan ekleyeceğiz.