---
title: Azure Active Directory B2C'de kullanıcı erişimini yönetme | Microsoft Docs
description: Reşit olmayanların tanımlamak, tarih Doğum ve ülke/bölge veri toplamak ve Azure AD B2C'yi kullanarak uygulamanızda kullanım koşullarının kabulü alma hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/24/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6aead01ec0084eb75ea385a67f7c85ea185b017a
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510564"
---
# <a name="manage-user-access-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı erişimini yönetme

Bu makalede, Azure Active Directory (Azure AD) B2C'yi kullanarak uygulamalarınız için kullanıcı erişimini yönetmek anlatılmaktadır. Uygulama erişim yönetimi içerir:

- Reşit olmayanların tanımlama ve uygulamanız için kullanıcı erişimini denetleme.
- Reşit olmayanların uygulamalarınızı kullanmak için ebeveyn izni gerektirir.
- Kullanıcılardan Doğum ve ülke/bölge veri toplanıyor.
- Kullanım koşulları sözleşmesini yakalamak ve erişim geçişi sağlayarak.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="control-minor-access"></a>İkincil erişim denetimi

Uygulamalar ve kuruluşların, bu İzleyici için hedeflenmeyen uygulamalar ve hizmetler kullanarak reşit olmayanların engellemek karar verebilirsiniz. Alternatif olarak, uygulamaları ve kuruluşların reşit olmayanların kabul edin ve daha sonra ebeveyn izni yönetmek ve iş kuralları tarafından belirlenen reşit olmayanların için izin verilen deneyimler sunmak karar verebilir ve düzenlemeleriyle izin verilir. 

Bir kullanıcı bir ikincil belirlenirse, kullanıcı akışını Azure AD B2C'ye üç seçenekten birini ayarlayabilirsiniz:

- **Bir imzalı JWT id_token uygulamasına geri göndermek**: Kullanıcı dizinde kayıtlı ve bir belirteç uygulamaya döndürülür. Uygulama, iş kuralları uygulayarak devam eder. Örneğin, uygulama bir ebeveyn izni işlemiyle devam edebilirsiniz. Bu yöntemi kullanmak için almayı seçtiğiniz **yaş** ve **consentProvidedForMinor** uygulamadan gelen talepler.

- **İşaretsiz bir JSON belirteci uygulamaya gönderir**: Azure AD B2C kullanıcı küçük ve kullanıcının ebeveyn izni durumunu sağlar. uygulama bildirir. Uygulama, iş kuralları uygulayarak devam eder. Bir JSON belirteç başarılı bir kimlik doğrulaması ile uygulama tamamlamaz. Uygulama Kimliği doğrulanmamış kullanıcı içerebilecek JSON belirtecinde bulunan talepleri göre işlemelisiniz **adı**, **e-posta**, **yaş**ve **consentProvidedForMinor**.

- **Kullanıcı engelleme**: Bir kullanıcı küçük ebeveyn izni sağlanmamış ise, Azure AD B2C kullanıcı engellenip engellenmeyeceğini bildirebilir. Hiçbir belirteç verilir, erişim engellenir ve kayıt yolculuğu sırasında kullanıcı hesabı oluşturulmaz. Bu bildirim uygulamak için mevcut uygun seçenekleri ve kullanıcıya bildirmek için uygun HTML/CSS içerik sayfası sağlar. Yeni kayıtlar için uygulama tarafından başka bir eylem gerekmez.

## <a name="get-parental-consent"></a>Ebeveyn izni alın

Uygulama düzenleme bağlı olarak, ebeveyn izni yetişkin doğrulanmış bir kullanıcı tarafından verilmiş olması gerekebilir. Azure AD B2C, bir kişinin yaşı doğrulayın ve ardından bir ikincil için ebeveyn izni vermek doğrulanmış bir yetişkin izin vermek için bir deneyim sağlamaz. Bu deneyim, uygulama veya başka bir hizmet sağlayıcısı tarafından sağlanmalıdır.

Kullanıcı akışı ebeveyn izni toplamak için bir örnek verilmiştir:

1. Bir [Azure Active Directory Graph API'si](/previous-versions/azure/ad/graph/api/api-catalog) işlemi kullanıcı küçük olarak tanımlar ve işaretsiz bir JSON belirteci biçiminde uygulama kullanıcı verilerini döndürür.

2. Uygulama, JSON belirteci işler ve ebeveyn izni gerekli olduğunu bildiren ve çevrimiçi bir üst izni isteyen küçük için gösteren bir ekranla karşılaşırsınız. 

3. Azure AD B2C kullanıcı normal şekilde oturum açabilir ve bir belirteç içerecek şekilde ayarlamak uygulamanın yayınlar bir oturum açma yolculuğu gösterir **legalAgeGroupClassification "minorWithParentalConsent" =** . Uygulama, e-posta adresi üst toplar ve üst yetişkin olduğunu doğrular. Bunu yapmak için bir Ulusal kimliği office, lisans doğrulama veya düzeltme kredi kartı gibi güvenilen bir kaynak kullanır. Doğrulama başarılı olursa, uygulamanın Azure AD B2C kullanıcı akışı kullanarak oturum açmak için alt ister. Onay reddedilirse (örneğin, **legalAgeGroupClassification "minorWithoutParentalConsent" =** ), Azure AD B2C, onay işlemini yeniden başlatmak için uygulamaya bir JSON belirteç (oturum açma değil) döndürür. Bu, isteğe bağlı olarak bir yetişkin veya küçük küçük'ın hesabına erişimi küçük'ın e-posta adresi ya da bir yetişkinin e-posta adresi kaydı için bir kayıt kodu göndererek kazanabilirsiniz böylece kullanıcı akışını özelleştirmek mümkündür.

4. Uygulama onayı iptal etmek için küçük bir seçenek sunar.

5. Küçük veya yetişkin onayı iptal eder, Azure AD Graph API'si değiştirmek için kullanılabilir **consentProvidedForMinor** için **reddedildi**. Alternatif olarak, uygulama, onayı iptal edilmiş küçük silmek tercih edebilirsiniz. Kimliği doğrulanmış küçük (veya ikincil'ın hesabını kullanarak üst) onayı iptal edebilir, böylece kullanıcı akışını özelleştirmek isteğe bağlı olarak mümkündür. Azure AD B2C kayıtları **consentProvidedForMinor** olarak **reddedildi**.

Hakkında daha fazla bilgi için **legalAgeGroupClassification**, **consentProvidedForMinor**, ve **yaş**, bkz: [kullanıcı kaynak türü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/user). Özel öznitelikler hakkında daha fazla bilgi için bkz. [tüketicileriniz hakkında bilgi toplamak için özel öznitelikler kullanma](active-directory-b2c-reference-custom-attr.md). Genişletilmiş öznitelikleri Azure AD Graph API'sini kullanarak adres, öznitelik uzun sürümü gibi kullanmalısınız *extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth*: *2011-01-01T00:00:00Z*.

## <a name="gather-date-of-birth-and-countryregion-data"></a>Tarih Doğum ve ülke/bölge veri toplayın

Uygulama doğum tarihi (DOB) ve ülke/bölge bilgileri tüm kullanıcıların kayıt sırasında toplamak için Azure AD B2C kullanır. Uygulama, bu bilgileri zaten mevcut değilse, kullanıcıdan bir sonraki kimlik doğrulaması (oturum açma) yolculuğu sırasında isteyebilir. Kullanıcılar kendi DOB ve ülke/bölge bilgilerini sağlamadan devam edemiyor. Azure AD B2C, tek tek ülke/bölge, Mevzuat standartlarına göre küçük olup olmadığını belirlemek için bilgileri kullanır. 

Özelleştirilmiş kullanıcı akışı DOB toplayabilir ve ülke/bölge bilgileri ve kullanım Azure AD B2C talep belirlemek için **yaş** ve sonucu kalıcı (veya doğrudan DOB ve ülke/bölge bilgileri kalıcı hale getirmek), Dizin.

Aşağıdaki adımlarda hesaplamak için kullanılan mantıksal **yaş** kullanıcının doğum tarihi'nden:

1. Ülke kodu tarafından listesinde ülke bulmaya çalışın. Ülke bulunamazsa geri döner **varsayılan**.

2. Varsa **MinorConsent** ülke öğe düğümü varsa:

    a. Kullanıcı üzerinde bir yetişkin olarak kabul edilmesi için geliştirilen gerekir tarihi hesaplayın. Örneğin, geçerli tarihi 14 Mart 2015 ise ve **MinorConsent** 18, doğum tarihi 14 Mart 2000'den daha geç olmalıdır.

    b. En düşük doğum tarihi gerçek doğum tarihi ile karşılaştırın. Hesaplamanın en düşük doğum tarihi kullanıcının doğum tarihi olup olmadığını döndürür **küçük** yaş grubu hesaplaması olarak.

3. Varsa **MinorNoConsentRequired** düğümüdür ülke öğesi, yineleme adımları 2a ve 2b mevcut değerini kullanarak **MinorNoConsentRequired**. 2b çıktısını verir **MinorNoConsentRequired** minimum doğum tarihi, kullanıcının doğum tarihi ise. 

4. Hiçbiri hesaplama true olursa, hesaplama döndürür **yetişkinlere yönelik**.

Uygulamanın güvenilir bir şekilde diğer yöntemlerle DOB ya da ülkeniz/bölgeniz veri topladığı, uygulama bu bilgileri kullanarak kullanıcı kaydı güncelleştirmek için Graph API'sini kullanabilir. Örneğin:

- Bir kullanıcı bir yetişkin olarak bilinen, dizin özniteliği güncelleştirme **yaş** değeriyle **yetişkinlere yönelik**.
- Bir kullanıcı küçük olmasını biliniyorsa, dizin özniteliği güncelleştirme **yaş** değerini **küçük** ve **consentProvidedForMinor**uygun şekilde.

DOB veri toplama hakkında daha fazla bilgi için bkz. [Azure AD B2C'de yaş geçidi kullanmak](basic-age-gating.md).

## <a name="capture-terms-of-use-agreement"></a>Kullanım koşulları sözleşmesi yakalama

Uygulamanızı geliştirirken, genellikle kullanıcıların olmadan uygulamalarıyla kullanım koşullarının kabulü yakalamak veya ikincil yalnızca kullanıcı dizinden katılım. Ancak, bir kullanıcının kullanım koşullarını kabul toplamak, kabul verilen değilse, erişimi kısıtlamak için bir Azure AD B2C kullanıcı akışı kullanın ve gelecekteki değişikliklerin tarihini ve son kabul tarihi temel alarak, kullanım koşullarını kabul zorlamak için mümkündür  kullanım koşullarını en son sürümü.

**Kullanım koşullarını** "verilerini üçüncü taraflarla paylaşılmasını onaylamış olursunuz." yer alabilir İlgili yerel düzenlemelere ve iş kuralları, bağlı olarak birleştirilmiş koşulların her ikisi de bir kullanıcının kabulü toplayabilir veya bir koşul ve diğer kabul etmesi izin verebilirsiniz.

Aşağıdaki adımlar, kullanım koşulları nasıl yönetebileceğinizi göstermektedir:

1. Kullanım koşullarını ve kabul tarihi kabulü genişletilmiş öznitelikleri ve Graph API'si kullanarak kaydedin. Her iki yerleşik ve özel kullanıcı Akışları'ni kullanarak bunu yapabilirsiniz. Oluşturma ve kullanma öneririz **extension_termsOfUseConsentDateTime** ve **extension_termsOfUseConsentVersion** öznitelikleri.

2. "Kullanım koşulları kabul et" etiketli gerekli onay kutusu oluşturmak ve kayıt sırasında sonucu kaydedin. Her iki yerleşik ve özel kullanıcı Akışları'ni kullanarak bunu yapabilirsiniz.

3. Azure AD B2C kullanımı sözleşmesi ve kullanıcının kabul koşulları depolar. Herhangi bir kullanıcı durumu için Graph API için sorgu yanıt kaydetmek için kullanılan uzantı özniteliğine okuyarak kullanabilirsiniz (örneğin, okuma **termsOfUseTestUpdateDateTime**). Her iki yerleşik ve özel kullanıcı Akışları'ni kullanarak bunu yapabilirsiniz.

4. Güncellenmiş kullanım koşulları kabulünüz Kullanım Koşulları'nın en son sürümünün tarih kabul tarihi karşılaştırarak gerektirir. Yalnızca özel kullanıcı akışı kullanarak tarihleri karşılaştırabilirsiniz. Genişletilmiş öznitelik kullanın **extension_termsOfUseConsentDateTime**ve talep değerine karşılaştırın **termsOfUseTextUpdateDateTime**. Kabul eski ise, yeni bir kabul otomatik olarak onaylanan bir ekran görüntüleyerek zorlar. Aksi takdirde, erişim ilkesi mantığı kullanarak engelleyin.

5. Güncellenmiş kullanım koşulları kabulünüz kabul en son kabul edilen sürüm numarası uygulamanın sürüm sayısını karşılaştırarak gerektirir. Sürüm numaraları yalnızca özel kullanıcı akışı kullanarak karşılaştırabilirsiniz. Genişletilmiş öznitelik kullanın **extension_termsOfUseConsentDateTime**ve talep değerine karşılaştırın **extension_termsOfUseConsentVersion**. Kabul eski ise, yeni bir kabul otomatik olarak onaylanan bir ekran görüntüleyerek zorlar. Aksi takdirde, erişim ilkesi mantığı kullanarak engelleyin.

Aşağıdaki senaryolar altında kullanımı kabul koşullarını yakalayabilirsiniz:

- Yeni bir kullanıcı kaydolan. Kullanım koşullarını görüntülenir ve kabul sonuç depolanır.
- Bir kullanıcı, kimin daha önce en son veya etkin kullanım koşullarını kabul olarak imzalanıyor. Kullanım koşulları görüntülenmez.
- Bir kullanıcı, kimin zaten son veya etkin kullanım koşullarını kabul etmedi içinde imzalama. Kullanım koşullarını görüntülenir ve kabul sonuç depolanır.
- Bir kullanıcı, kimlerin zaten daha eski bir sürümü artık en son sürüme güncelleştirilir kullanım şartları kabul ettiğini de imzalama. Kullanım koşullarını görüntülenir ve kabul sonuç depolanır.

Aşağıdaki görüntüde önerilen kullanıcı akışı gösterilmektedir:

![Kabul kullanıcı akışı](./media/manage-user-access/user-flow.png) 

Bir DateTime temel kullanım koşulları onayı bir talep örneği verilmiştir:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentDateTime" TransformationMethod="GetCurrentDateTime">
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="currentDateTime" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequired" TransformationMethod="IsTermsOfUseConsentRequired">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="termsOfUseConsentDateTime" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="termsOfUseTextUpdateDateTime" DataType="dateTime" Value="2098-01-30T23:03:45" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="result" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Bir temel sürümü kullanım koşulları onayı bir talep örneği verilmiştir:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetEmptyTermsOfUseConsentVersionForNewUser" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value=""/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentVersion" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value="V1"/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="compareTo" DataType="string" Value="V1" />
      <InputParameter Id="operator" DataType="string" Value="not equal" />
      <InputParameter Id="ignoreCase" DataType="string" Value="true" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations> 
```

## <a name="next-steps"></a>Sonraki adımlar

- Silmek ve kullanıcı verilerini dışarı aktarma hakkında bilgi edinmek için [kullanıcı verilerini yönetme](manage-user-data.md).
