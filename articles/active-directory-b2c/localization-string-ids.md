---
title: Yerelleştirme dize kimlikleri - Azure Active Directory B2C | Microsoft Docs
description: İçerik tanımı kimliklerini Azure Active Directory B2C, özel bir ilkede api.signuporsignin kimliği belirtin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 41a72013f1538b0a857c76bc949a7109e1cd54b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510916"
---
# <a name="localization-string-ids"></a>Yerelleştirme dize kimlikleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi kullanıcı yolculuklarından ilkesinde birden çok yerel ayarları veya dil desteği sağlar. Bu makalede, yerelleştirme ilkenizde kullanabileceğiniz kimlikleri listesini sağlar. UI yerelleştirme ile hakkında bilgi edinmek için bkz: [yerelleştirme](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Kaydolma veya oturum açma sayfası öğeleri

İçerik tanımı kimliği için kullanılan aşağıdaki kimlikler `api.signuporsignin`.

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **local_intro_email** | Mevcut hesabınızla oturum açın |
| **logonIdentifier_email** | E-posta adresi |
| **requiredField_email** | Lütfen e-postanızı girin |
| **invalid_email** | Lütfen geçerli bir e-posta adresi girin |
| **email_pattern** | ^ [a-zA-Z0-9.! #$% &'' *+/ =? ^ _\`{\|} ~-]+@[a-zA-Z0-9-]+ (?:\\. [ bir-zA-Z0 - 9-] +)* $ |
| **local_intro_username** | Kullanıcı adınız ile oturum açın |
| **logonIdentifier_username** | Kullanıcı adı |
| **requiredField_username** | Lütfen kullanıcı adınızı girin |
| **Parola** | Parola |
| **requiredField_password** | Lütfen parolanızı girin |
| **invalid_password** | Girdiğiniz parola beklenen biçimde değil. |
| **forgotpassword_link** | Parolanızı mı unuttunuz? |
| **createaccount_intro** | Hesabınız yok mu? |
| **createaccount_link** | Hemen kaydolun |
| **divider_title** | OR |
| **cancel_message** | Kullanıcı parolasını unutmuş. |
| **button_signin** | Oturum aç |
| **social_intro** | Sosyal hesabınızla oturum açın |
  **remember_me** |Oturumumu açık bırak|
| **unknown_error** | Oturumunuzu açarken sorun yaşıyoruz. Lütfen daha sonra tekrar deneyin. |

Aşağıdaki örnek kullanımı bazı kullanıcı arabirimi öğeleri kaydolma veya oturum açma sayfasında gösterir:

![Kaydolma veya oturum açma sayfası UX öğeleri](./media/localization-string-ids/localization-susi.png)

Kimlik sağlayıcıları kimliği kullanıcı yolculuğunda yapılandırılmış **ClaimsExchange** öğesi. Kimlik sağlayıcısının başlığı yerelleştirmek için **ElementType** ayarlanır `ClaimsProvider`, ancak **Stringıd** Kimliğine ayarlanır `ClaimsExchange`.

```XML
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="MicrosoftExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccount" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Aşağıdaki örnek Arapça Facebook kimlik sağlayıcısına yerelletirilmesi:

```XML
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

## <a name="sign-up-or-sign-in-error-messages"></a>Kaydolma veya oturum açma hata iletileri

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **UserMessageIfInvalidPassword** | Parola doğru değil. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Hesabınız gerçekleştiremiyorum. |
| **UserMessageIfOldPasswordUsed** | Eski bir parola kullandığınız görülüyor. |  
| **DefaultMessage** | Geçersiz kullanıcı adı veya parola. |  
| **UserMessageIfUserAccountDisabled** | Hesabınız kilitlendi. Kilidini açmak ve ardından yeniden denemek için destek personelinize başvurun. |  
| **UserMessageIfUserAccountLocked** | Hesabınız yetkisiz kullanımı önlemek için geçici olarak kilitlendi. Daha sonra tekrar deneyin. |  
| **AADRequestsThrottled** | Şu anda çok fazla istek var. Lütfen bir süre bekleyin ve yeniden deneyin. |  

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Kaydolma ve kendi kullanıcı arabirimi öğeleri sayfaları onaylanan

İçerik tanımı kimliği kimlikleri şunlardır `api.localaccountsignup` veya ile başlayan herhangi bir içerik tanım `api.selfasserted`, gibi `api.selfasserted.profileupdate` ve `api.localaccountpasswordreset`.

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **ver_sent** | Doğrulama kodu gönderildi: |
| **ver_but_default** | Varsayılan |
| **cancel_message** | Otomatik olarak onaylanan bilgileri girerek kullanıcı iptal etti |
| **preloader_alt** | Lütfen bekleyin |
| **ver_but_send** | Doğrulama kodu Gönder |
| **alert_yes** | Evet |
| **error_fieldIncorrect** | Bir veya daha fazla alan yanlış doldurulur. Lütfen girişlerinizi denetleyin ve yeniden deneyin. |
| **Yıl** | Yıl |
| **verifying_blurb** | Biz bilgileriniz işlenirken lütfen bekleyin. |
| **button_cancel** | İptal |
| **ver_fail_no_retry** | Çok fazla sayıda yanlış denemesi yaptınız. Lütfen daha sonra tekrar deneyin. |
| **Ay** | Ay |
| **ver_success_msg** | E-posta adresi doğrulandı. Artık devam edebilirsiniz. |
| **months** | Ocak, Şubat ve Mart, Nisan, olabilir Haziran, Temmuz, Ağustos, Eylül, Ekim, Kasım, aralık |
| **ver_fail_server** | E-posta adresinizi doğrulama konusunda sorun yaşıyoruz. Lütfen geçerli bir e-posta adresini girin ve yeniden deneyin. |
| **error_requiredFieldMissing** | Gerekli alan eksik. Lütfen tüm gerekli alanları doldurun ve yeniden deneyin. |
| **initial_intro** | Lütfen şu bilgileri sağlayın. |
| **ver_but_resend** | Yeni kod Gönder |
| **button_continue** | Create |
| **error_passwordEntryMismatch** | Parola giriş alanları eşleşmiyor. Lütfen her iki alana aynı parolayı girin ve yeniden deneyin. |
| **ver_incorrect_format** | Biçimi hatalı. |
| **ver_but_edit** | E-posta değiştirme |
| **ver_but_verify** | Kodu doğrulayın |
| **alert_no** | Hayır |
| **ver_info_msg** | Gelen kutunuza doğrulama kodu gönderildi. Lütfen aşağıdaki giriş kutusuna kopyalayın. |
| **gün** | Gün |
| **ver_fail_throttled** | Bu e-posta adresini doğrulamak için çok fazla istek yapıldı. Lütfen bir süre bekleyin ve yeniden deneyin. |
| **helplink_text** | Bu nedir? |
| **ver_fail_retry** | Bu kod doğru değil. Lütfen yeniden deneyin. |
| **alert_title** | Bilgilerinizi girme işlemini iptal et |
| **required_field** | Bu bilgiler gereklidir. |
| **alert_message** | Bilgilerinizi girme iptal etmek istediğinizden emin misiniz? |
| **ver_intro_msg** | Doğrulama gerekli değildir. Lütfen gönder düğmesine tıklayın. |
| **ver_input** | Doğrulama kodu |

## <a name="sign-up-and-self-asserted-pages-error-messages"></a>Kaydolma ve kendi kendini sayfaları hata iletileri onaylanan

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **UserMessageIfClaimsPrincipalAlreadyExists** | Belirtilen Kimliğe sahip bir kullanıcı zaten var. Lütfen bir abonelik seçin. |
| **UserMessageIfClaimNotVerified** | Talep doğrulanamadı: {0} |
| **UserMessageIfIncorrectPattern** | Yanlış deseni için: {0} |
| **UserMessageIfMissingRequiredElement** | Eksik gerekli öğe: {0} |
| **UserMessageIfValidationError** | Tarafından doğrulama hatası: {0} |
| **UserMessageIfInvalidInput** | {0} Geçersiz giriş var. |
| **ServiceThrottled** | Şu anda çok fazla istek var. Lütfen bir süre bekleyin ve yeniden deneyin. |

Aşağıdaki örnek, kayıt sayfasındaki kullanıcı arabirimi öğelerinden bazıları kullanımını gösterir:

![Kayıt sayfasına UX öğeleri](./media/localization-string-ids/localization-sign-up.png)

Aşağıdaki örnek, kayıt sayfasındaki kullanıcı arabirimi öğelerinden bazıları kullanımını gösterir, doğrulama kodu düğmesine tıkladığında kullanıcı sonra gönder:

![Kayıt sayfasına e-posta doğrulama UX öğeleri](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Faktörlü kimlik doğrulaması sayfası kullanıcı arabirimi öğeleri telefon

İçerik tanımı kimliği kimlikleri şunlardır `api.phonefactor`. 

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **button_verify** | Beni ara |
| **country_code_label** | Ülke kodu |
| **cancel_message** | Kullanıcı çok faktörlü kimlik doğrulaması iptal etti |
| **text_button_send_second_code** | Yeni bir kod gönderin |
| **code_pattern** | \\d{6} |
| **intro_mixed** | Aşağıdaki numarasını kaydında sizin için sahibiz. SMS yoluyla bir kod gönderin veya, kimliğinizi doğrulamak telefon. |
| **intro_mixed_p** | Aşağıdaki numaralarını kaydında sizin için sahibiz. Biz telefon veya böylelikle Gönder, kimliğinizi doğrulamak için SMS ile kod bir sayı seçin. |
| **button_verify_code** | Kodu doğrulayın |
| **requiredField_code** | Lütfen aldığınız doğrulama kodunu girin |
| **invalid_code** | Lütfen aldığınız 6 basamaklı kodu girin |
| **button_cancel** | İptal |
| **local_number_input_placeholder_text** | Telefon numarası |
| **button_retry** | Yeniden Dene |
| **alternative_text** | Telefonumu sahip değilsiniz |
| **intro_phone_p** | Aşağıdaki numaralarını kaydında sizin için sahibiz. Biz, kimliğinizi doğrulamak için telefon bir sayı seçin. |
| **intro_phone** | Aşağıdaki numarasını kaydında sizin için sahibiz. Kimliğinizi doğrulamak telefon. |
| **enter_code_text_intro** | Doğrulama kodunuzu aşağıdaki girin veya  |
| **intro_entry_phone** | Aşağıdaki bir sayı girin. biz, kimliğinizi doğrulamak telefon. |
| **intro_entry_sms** | Aşağıda, SMS, kimliğinizi doğrulamak için bir kodu gönderebiliriz bir sayı girin. |
| **button_send_code** | Kod Gönder |
| **invalid_number** | Lütfen geçerli bir telefon numarası girin |
| **intro_sms** | Aşağıdaki numarasını kaydında sizin için sahibiz. SMS, kimliğinizi doğrulamak için bir kod göndereceğiz. |
| **intro_entry_mixed** | Aşağıdaki bir sayı girin biz SMS yoluyla bir kod gönderin veya böylelikle, kimliğinizi doğrulamak telefon. |
| **number_pattern** | ^\\+(?:[0-9][\\x20-]?){6,14}[0-9]$ |
| **intro_sms_p** |Aşağıdaki numaralarını kaydında sizin için sahibiz. Bir sayı seçin, kimliğinizi doğrulamak için SMS ile kod gönderebiliriz. |
| **requiredField_countryCode** | Lütfen ülke kodunuzu seçin |
| **requiredField_number** | Lütfen telefon numaranızı girin |
| **country_code_input_placeholder_text** |Ülke veya bölge |
| **number_label** | Telefon numarası |
| **error_tryagain** | Belirttiğiniz telefon numarasını meşgul veya kullanılamıyor. Lütfen numarayı denetleyin ve yeniden deneyin. |
| **error_incorrect_code** | Girdiğiniz doğrulama kodu, kayıtlarımızla eşleşmiyor. Lütfen yeniden deneyin veya yeni bir kod isteyin. |
| **countryList** | {\"Varsayılan\":\"ülke/bölge\",\"AF\":\"Afganistan\",\"AX\":\"Aland Adaları\",\"AL\":\"Arnavutluk\",\"DZ\":\"Cezayir\",\"AS\":\" Amerikan Samoası\",\"AD\":\"Andora\",\"AO\":\"Angola\",\"AI\": \"Avustralya\",\"AQ\":\"Antarktika\",\"AG\":\"Antigua ve Barbuda\",\"AR\":\"Arjantin\",\"AM\":\"Ermenistan\",\"AW\":\"Aruba \",\"AU\":\"Avustralya\",\"ADRESİNDEKİ\":\"Avusturya\",\" AZ\":\"Azerbaycan\",\"BS\":\"Bahamalar\",\"BH\":\" Bahreyn\",\"BD\":\"Bangladeş\",\"BB\":\"Barbados\",\" TARAFINDAN\":\"Belarus\",\"BE\":\"Belçika\",\"BZ\":\" Belize\",\"BJ\":\"Benin\",\"BM\":\"teni\",\"BT\":\"Bhutan\",\"BO\":\"Bolivya\",\"BQ\":\" Bonaire\",\"BA\":\"Bosna-Hersek\",\"BW\":\"Botsvana<span class="notransla class=""></span class="notransla> Harici Adaları\",\"VI\":\"ABD Virgin Adaları\",\"UG\":\"Uganda\",\"UA\":\"Ukrayna\",\"AE\":\" Birleşik Arap Emirlikleri\",\"GB\":\"Birleşik Krallık\",\"ABD\":\"Amerika Birleşik Devletleri\",\"UY \":\"Uruguay\",\"UZ\":\"Özbekistan\",\"VU\":\"Vanuatu\", \"VA\":\"Vatikan\",\"VE\":\"Venezuela\",\"VN\":\"Vietnam \",\"WF\":\"Wallis ve Futuna\",\"YE\":\"Yemen\",\"ZM\":\"Zambiya\",\"ZW\":\"Zimbabve\"} |
| **error_448** | Belirttiğiniz telefon numarasını erişilemiyor. |
| **error_449** | Kullanıcı, yeniden deneme sayısını aştı. |
| **verification_code_input_placeholder_text** | Doğrulama kodu |

Aşağıdaki örnek MFA kayıt sayfasında kullanıcı arabirimi öğelerinden bazıları kullanımını gösterir:

![Kayıt sayfasına e-posta doğrulama UX öğeleri](./media/localization-string-ids/localization-mfa1.png)

Aşağıdaki örnek MFA doğrulama sayfasında kullanıcı arabirimi öğelerinden bazıları kullanımını gösterir:

![Kayıt sayfasına e-posta doğrulama UX öğeleri](./media/localization-string-ids/localization-mfa2.png)







