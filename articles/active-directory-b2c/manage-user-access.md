---
title: Azure AD B2C'de kullanıcı erişimini yönetme | Microsoft Docs
description: Reşit olmayanların tanımlamak, tarih Doğum ve ülke veri toplamak ve Azure AD B2C kullanarak uygulamanızda kullanım koşullarının kabulü alma hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 05/04/2018
ms.author: davidmu
ms.openlocfilehash: c44135a3069966b14d8760e4daa009ab8d39ccca
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="manage-user-access-in-azure-ad-b2c"></a>Azure AD B2C'de kullanıcı erişimini yönetme

Bu makalede kullanıcı erişimi Azure Active Directory (AD) B2C kullanarak uygulamalarınızı nasıl yönetebileceğiniz hakkında bilgi sağlar. Uygulamanızdaki erişim yönetimi içerir:

- Reşit olmayanların tanımlayan ve uygulamanızı erişimi denetleme
- Uygulamalarınızı kullanmaya reşit olmayanların için ebeveyn izni gerektiren
- Kullanıcıdan veri Doğum ve ülke veri toplama
- Sözleşme ve erişim geçişi sağlayarak yakalama koşullarını kullanın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

>[!Note] 
>Bu makalede, sorumlulukları GDPR altında desteklemek için kullanılan bilgiler sağlanmaktadır. GDPR hakkında genel bilgi arıyorsanız bkz [Hizmeti'ne güvenen portal GDPR bölümünü](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="control-minor-access"></a>İkincil erişimi denetleme

Uygulamalar ve kuruluşlar, bu İzleyici yönlendirilmemiş uygulamalar ve hizmetler kullanarak reşit olmayanların engellemek karar verebilirsiniz. Alternatif olarak, uygulamaları ve kuruluşların reşit olmayanların kabul edin ve daha sonra ebeveyn izni yönetmek ve izin verilen reşit olmayanların için iş kuralları tarafından gerektiği şekilde deneyimleri karar verebilir ve düzenleme tarafından izin verilen. 

Bir kullanıcı küçük tanımlanırsa, ardından Azure AD B2C kullanıcı akışında üç seçenekten birini ayarlanabilir:

- **İmzalı JWT id_token uygulamasına geri göndermek** - kullanıcı dizinde kayıtlı ve uygulama için bir belirteç döndürdü. Uygulama, iş kurallarını kullanarak devam eder. Örneğin, uygulama bir ebeveyn izni işlemiyle devam edebilirsiniz. Bunu yapmak için almayı seçtiğiniz **ageGroup** ve **consentProvidedForMinor** uygulamadan gelen talepleri.
- **İmzasız bir JSON belirteci uygulamaya gönder** -Azure AD B2C bildirir uygulama kullanıcı bir ikincil ve kullanıcının ebeveyn izni durumunu sağlar. Uygulama, iş kurallarını kullanarak devam eder. Bir JSON belirteci başarılı bir kimlik doğrulaması uygulamayla tamamlanmaz. Uygulama Kimliği doğrulanmamış kullanıcı içerebilen JSON belirtecinde bulunan talepleri göre işlemelidir **adı**, **e-posta**, **ageGroup**ve **consentProvidedForMinor**.
- **Kullanıcıyı engellemek** - kullanıcı bir ikincil ebeveyn izni olmayan sağlamış ise.  Azure AD B2C, engellenmiş sizi bilgilendirir kullanıcıya bir ekran sonra sunabilir.  Hiçbir belirteç erişimi engellenir ve kullanıcı hesabının kayıt seyahat sırasında oluşturulmaz. Bu uygulama için mevcut uygun seçenekleri ve kullanıcı bildirmek üzere bir uygun HTML/CSS içerik sayfasını sağlayın. Yeni kayıtlar için uygulama tarafından başka eylem gerekmiyor.

## <a name="get-parental-consent"></a>Ebeveyn izni al

Uygulama düzenleme bağlı olarak ebeveyn izni yetişkin olarak doğrulanmış bir kullanıcı tarafından verilmesi gerekli olabilir.  Azure AD B2C, bir kişinin yaşı doğrulayın ve bir ikincil ebeveyn izni vermek doğrulanmış bir yetişkin izin vermek için bir deneyimi sağlamaz.  Bu deneyim, uygulama veya başka bir hizmet sağlayıcısı tarafından sağlanmalıdır.

Ebeveyn izni toplamak için bir kullanıcı Akış örneği verilmiştir:

1. Bir [Azure Active Directory grafik API'si](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog) işlemi kullanıcı bir ikincil olarak tanımlar ve imzasız bir JSON belirteci biçiminde uygulama için kullanıcı verilerini döndürür.
2. Uygulama JSON belirteci işler ve bu ebeveyn izni bildiren küçük bir ekrana gerekli olduğunu gösterir ve çevrimiçi bir üst izin ister. 
3. Azure AD B2C kullanıcının yapabildiği izin normal şekilde oturum açmak için bir oturum açma gezisine gösterir ve bir belirteç içerecek şekilde ayarlandığını uygulamaya sorunları **legalAgeGroupClassification "minorWithParentalConsent" =** uygulama toplar e-posta adresi üst ve üst yetişkin bir Ulusal kimliği office, lisans doğrulama veya kredi kartı kanıtını gibi güvenilir bir kaynaktan olduğunu doğrular. Başarılı olursa, uygulama Azure AD B2C kullanıcı akışı kullanarak oturum küçük ister. Onay reddedildi varsa (örneğin **legalAgeGroupClassification "minorWithoutParentalConsent" =**, Azure AD B2C onay işlemini yeniden başlatmak için uygulamaya bir JSON belirteci (oturum açma değil) döndürür. Adlı bir alt veya yetişkin küçük'ın hesabına erişim izni küçük'ın e-posta adresi veya yetişkinin e-posta adresi kaydı için bir kayıt kodu göndererek kazanabilirsiniz kullanıcı akışı özelleştirmek isteğe bağlı olarak mümkündür.
4. Uygulama iznini iptal etmek için ikincil bir seçenek sunar.
5. Küçük veya yetişkin izin iptal eder, Azure AD Graph API değiştirmek için kullanılabilir **consetProvidedForMinor** için **reddedildi**. Alternatif olarak, uygulama, izni iptal küçük silmek tercih edebilirsiniz. Yapabildiği kimliği doğrulanmış küçük (veya üst küçük'ın hesabı kullanarak) iznini iptal edebilirsiniz kullanıcı akışı özelleştirmek isteğe bağlı olarak mümkündür. Azure AD B2C kayıtları **consentProvidedForMinor** olarak **reddedildi**.

Hakkında daha fazla bilgi için **legalAgeGroupClassification**, **consentProvidedForMinor**, ve **ageGroup**, bkz: [kullanıcı kaynak türü](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/user). Özel öznitelikler hakkında daha fazla bilgi için bkz: [tüketicileriniz hakkında bilgi toplamak için özel öznitelikler kullanın](active-directory-b2c-reference-custom-attr.md). Genişletilmiş öznitelikler Azure AD Graph API kullanarak belirtirken "gibi extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth" öznitelik uzun sürümü kullanılmalıdır: "2011-01-01T00:00:00Z"

## <a name="gather-date-of-birth-and-country-data"></a>Tarih Doğum ve ülke veri toplayın

Uygulamaları doğum tarihi (DOB) ve ülke tüm kullanıcıların kayıt sırasında toplamak için Azure AD B2C kullanır. Varsa DOB veya ülke bilgileri zaten yok, bu kullanıcısı sırasında sonraki kimlik doğrulaması (oturum açma) gezisine sorulacaktır. Kullanıcılar DOB sağlamadan devam mümkün olmayacak ve ülke bilgileri. Ülke ve sağlanan DOB bağlı olarak, Azure AD B2C tek tek bir ikincil o ülkenin yasal standartlarına göre değerlendirilir belirler. 

Özelleştirilmiş kullanıcı akışı DOB toplayabilir ve ülke bilgileri ve kullanım Azure AD B2C talep belirlemek için dönüşümleri **ageGroup** ve sonucu devam (veya ülke ve DOB bilgileri doğrudan kalıcı) dizininde.

Aşağıdaki adımlar hesaplamak için kullanılan mantığı gösterir **ageGroup** başlangıç Doğum Tarihi:

1. Listede ülke kodu tarafından ülke bulmaya çalışın. Ülke bulunmazsa geri döner **varsayılan**.
2. Varsa **MinorConsent** ülke öğesinde düğüm varsa:  <br>a. Kullanıcı üzerinde bir yetişkin olarak kabul edilmesi için doğacak zorunda kalacaktır minimum tarih hesaplayın. Örnek: Doğum tarihi olan 3/14/2015 ve **MinorConsent** 18, minimum doğum tarihi olacak 14/3/2000.
    <br>b. Minimum doğum tarihi gerçek doğum tarihi ile karşılaştırın. Hesaplamanın en düşük doğum tarihi kullanıcının doğum tarihi olup olmadığını döndürür **küçük** yaş Grup hesaplama olarak.
3. Varsa **MinorNoConsentRequired** düğüm varsa ülke öğe, yineleme Adım 2a ve 2b değerini kullanarak **MinorNoConsentRequired**. 2b çıktısını döndürür **MinorNoConsentRequired** minimum doğum tarihi kullanıcının doğum tarihi ise. 
4. Hiçbiri hesaplamalar true ise, hesaplama döndürür. döndürülen **yetişkin**.

Uygulamanın güvenilir bir şekilde toplanan DOB veya diğer yöntemlerle ülke veri varsa, uygulamanın kullanıcı kaydı bu bilgilerle güncelleştirmek için grafik API'si kullanabilir. Örneğin:

- Bir kullanıcı bir yetişkinin olduğu bilinen, dizin özniteliği güncelleştirin. **ageGroup** değerini **yetişkin**.
- Bir kullanıcı, bir küçük olacak şekilde biliniyorsa dizin özniteliği güncelleştirme **ageGroup** değerini **küçük** ve **consentProvidedForMinor** uygun şekilde.

DOB veri toplama hakkında daha fazla bilgi için bkz: [yaş Azure AD B2C'de geçişi kullanarak](basic-age-gating.md).

## <a name="capture-terms-of-use-agreement"></a>Kullanım koşulları sözleşmesi yakalama

Uygulamanızı geliştirirken, genellikle hiçbir, kendi uygulama içinden kullanım koşulları kullanıcı kabulü yakalama veya yalnızca kullanıcı dizininden katılım küçük.  Mümkündür, ancak, bir Azure AD B2C kullanmak için kullanıcı Akış kullanım koşulları sözleşmesini toplamak için kabul verilir ve Kabulü zorlamak için kullanım koşullarını ileride yapılacak değişiklikler son kabul tarihini ve son versio tarihini göre sürece erişimi kısıtlama kullanım koşulları n.

**Kullanım koşulları** ayrıca "verilerini üçüncü taraflarla paylaşmak için onay." içerebilir  Pozitif bir kullanıcıdan bu koşulların kabulü teknik birleştirilmiş olarak toplanması veya kullanıcı biri ve diğer iş kurallarını ve yerel düzenlemelerle bağlı olarak kabul etmek mümkün olabilir.

Aşağıdaki adımlar, kullanım koşulları yönetmeye yönelik özellikleri açıklamaktadır:

1. Kayıt kabul kullanım koşullarını ve kabul grafik API'si ve genişletilmiş öznitelikleri kullanma tarihi. Bu yapılabilir hem yerleşik ve özel kullanıcı akışları kullanma. Oluşturma ve kullanma önerilen **extension_termsOfUseConsentDateTime** ve **extension_termsOfUseConsentVersion** öznitelikleri.
2. Gerekli bir onay kutusu "Kullanım koşullarını kabul et" başlıklı oluşturabilir ve kaydolma sırasında sonucu kaydedebilirsiniz. Bu yapılabilir hem yerleşik ve özel kullanıcı akışları kullanma.
3. Azure AD B2C koşullarını kullanım sözleşmesi ve onay depolar. Grafik API'si kullanılabilir yanıt kaydetmek için kullanılan uzantı öznitelik okuyarak herhangi bir kullanıcının durumunu sorgulamak için örneğin okuma **termsOfUseTestUpdateDateTime**. Bu yapılabilir hem yerleşik ve özel kullanıcı akışları kullanma.
4. Güncelleştirilmiş koşulları kabul ettiğini, kabul tarihine Kullanım Koşulları'nın en son sürümünün tarih karşılaştırarak gerektirir. Bu yalnızca yapılabilir özel kullanıcı akışı kullanarak. Genişletilmiş özniteliğini kullanın **extension_termsOfUseConsentDateTime** ve talep değerine karşılaştırma **termsOfUseTextUpdateDateTime**, kabul eski daha sonra yeni bir kabul zorla Ekran otomatik olarak uygulanan, aksi takdirde ilkesi mantığı kullanarak erişimi engeller.
5. Güncelleştirilmiş koşulları kabul ettiğini, kabul son kabul edilen sürüm numarası için sürüm numarasını karşılaştırarak gerektirir. Bu yalnızca yapılabilir özel kullanıcı akışı kullanarak. Genişletilmiş özniteliğini kullanın **extension_termsOfUseConsentDateTime** ve talep değerine karşılaştırma **extension_termsOfUseConsentVersion**, kabul eski daha sonra yeni bir kabul zorla Ekran otomatik olarak uygulanan, aksi takdirde ilkesi mantığı kullanarak erişimi engeller.

Kullanıcı aşağıdaki senaryoları altında kullan onay koşullarını yakalama görüntülenebilir:

- Yeni bir kullanıcı kaydolan. Kullanım koşulları görüntülenen onay sonuç depolanır.
- Bir kullanıcı oturum açma ve onay Sözleşmesi'nin en son veya etkin koşullarını için daha önce zaten kabul etti. Kullanım koşulları görüntülenmez.
- Bir kullanıcı oturum açma ve zaten onay için son ya da etkin kullanım koşullarını kabul etmedi. Kullanım koşulları görüntülenen onay sonuç depolanır.
- Bir kullanıcı oturum açma ve zaten onay için eski bir sonraki bir sürüme güncellenir kullanım koşullarını kabul etti. Kullanım koşulları görüntülenen onay sonuç depolanır.

Aşağıdaki resimde önerilen kullanıcı akışı gösterilmektedir:

![kabul kullanıcı akışı](./media/manage-user-access/user-flow.png) 

Kullan onay bir talep tabanlı DateTime koşullarını örneği verilmiştir:

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

Bir talep kullan onay sürümü koşullarını örneği verilmiştir:

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

- Silme ve kullanıcı verilerini dışarı aktarma hakkında bilgi edinin [kullanıcı verilerini yönetme](manage-user-data.md)
