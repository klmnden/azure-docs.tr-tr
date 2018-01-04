---
title: "Kullanıcı Arabirimi (UI) özelleştirme - Azure AD B2C | Microsoft Docs"
description: "Bir konu Azure Active Directory B2C, kullanıcı arabirimi (UI) özelleştirme özellikleri hakkında"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: be3fe7199308606aaab002290319df9c82149433
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Azure AD B2C kullanıcı arabirimini (UI) özelleştirme

Kullanıcı deneyimi uygulama kullanıma yönelik bir müşteri dönüştürmektir.  Müşteri, marka Görünüm ve yapısını ile kullanıcı deneyimleri hazırlayın tarafından temel artar. Azure Active Directory B2C (Azure AD B2C) kaydolma, oturum açma profili düzenleme özelleştirmenize olanak tanır ve parola sıfırlama piksel mükemmel denetimiyle sayfaları.

> [!NOTE]
> Bu makalede açıklanan sayfası kullanıcı Arabirimi özelleştirme özelliğini tek ilke oturum açma, eşlik eden parola sıfırlama sayfası ve doğrulama için e-postaları geçerli değildir.  Bu özellikleri kullanan [şirket markası özelliğini](../active-directory/customize-branding.md) yerine.
>
> Benzer şekilde, bir kullanıcı intiates, bir düzenleme profili İlkesi *önce* oturum açma kullanıcı kullanılarak özelleştirilebilir bir sayfaya yönlendirilir [şirket markası özelliğini](../active-directory/customize-branding.md).

Bu makalede aşağıdaki konuları içerir:

* Sayfanın UI Özelleştirme özelliği.
* HTML içeriğini sayfası kullanıcı Arabirimi özelleştirme özelliği ile kullanmak için Azure Blob depolama alanına yüklemek için kullanılan bir araçtır.
* Azure AD B2C tarafından geçişli stil sayfaları (CSS) kullanarak özelleştirebileceğiniz kullanılan kullanıcı Arabirimi öğeleri.
* Bu özelliği kullanan olduğunda en iyi yöntemler.

## <a name="the-page-ui-customization-feature"></a>Sayfanın UI Özelleştirme özelliği

Müşteri kaydolma, oturum açma parolası görünümünü özelleştirebilirsiniz sıfırlama ve profil düzenleme sayfaları (yapılandırarak [ilkeleri](active-directory-b2c-reference-policies.md)). Uygulama ve Azure AD B2C tarafından sunulan sayfaları arasında gezinme edilirken, müşterilerinizin sorunsuz bir deneyim kullanır.

Burada kullanıcı Arabirimi seçenekleri, Azure AD B2C kullanan kullanıcı Arabirimi özelleştirme için basit ve modern bir yaklaşım diğer hizmetler farklı.

İşte nasıl çalışır?: Azure AD B2C, müşterinizin tarayıcıda kodu çalıştırır ve adlı modern bir yaklaşım kullanır [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/).  Çalışma zamanında, ilkede belirttiğiniz URL'den içeriği yüklenir. Farklı sayfaları için farklı URL'ler belirtebilirsiniz. Azure AD B2C ' eklenen bir HTML parçasını, URL'den yüklenen içeriğe birleştirilen sonra müşterinize sayfası görüntülenir. Tüm yapmanız gereken şöyledir:

1. Doğru biçimlendirilmiş HTML5 ile boş bir içerik oluşturma `<div id="api"></div>` öğesi bulunan bir yerde `<body>`. Bu öğe işaretleri Azure AD B2C içerik nerede eklenir.
1. Bir HTTPS uç noktası, içerik ana bilgisayar (izin verilen CORS'yi). Her ikisini de almak ve seçenekleri istek yöntemleri CORS yapılandırırken etkinleştirilmelidir unutmayın.
1. CSS Azure AD B2C ekler kullanıcı Arabirimi öğeleri stilini belirlemek için kullanın.

### <a name="a-basic-example-of-customized-html"></a>Özelleştirilmiş HTML temel örneği

Aşağıdaki örnek, bu özelliği test etmek için kullanabileceğiniz en basit bir HTML içeriktir. Kullanım [Yardımcısı aracı](active-directory-b2c-reference-ui-customization-helper-tool.md) karşıya yükleyin ve bu içerik, Azure Blob Depolama alanında yapılandırın. Ardından temel, stilize olmayan düğmeleri & form alanlarını her sayfada görüntülenen ve işlevsel olduğunu doğrulayın.

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
    </body>
</html>
```

## <a name="test-out-the-ui-customization-feature"></a>UI Özelleştirme özelliği test

Bizim örnek HTML ve CSS içeriğini kullanarak kullanıcı Arabirimi özelleştirme özelliği denemek mi istiyorsunuz?  Biz sağladığınız [Yardımcısı aracı](active-directory-b2c-reference-ui-customization-helper-tool.md) yükler ve Azure Blob Depolama alanınızda örnek içeriği yapılandırır.

> [!NOTE]
> Herhangi bir yere UI içeriğinizi barındırabilir: web sunucularında, CDN'ler, AWS S3, dosya paylaşım sistemleri, vs. İçerik CORS'yi genel kullanıma açık bir HTTPS uç noktası üzerinde barındırılıyorsa sürece hazırsınız. Azure Blob Depolama yalnızca yalnızca tanım amaçlıdır kullanıyoruz.
>

## <a name="the-ui-fragments-embedded-by-azure-ad-b2c"></a>Azure AD B2C tarafından katıştırılmış UI parçaları

Aşağıdaki bölümlerde Azure AD B2C birleştirir HTML5 parçaları listesinde `<div id="api"></div>` öğesi bulunan, içeriği. **Bu parçasının HTML 5 içeriğinizi koymayın.** Azure AD B2C hizmetiyle bunlarda çalışma zamanında ekler. Bu parçasının kendi geçişli stil sayfaları (CSS) tasarlarken başvuru olarak kullanabilirsiniz.

### <a name="fragment-inserted-into-the-identity-provider-selection-page"></a>"Kimlik sağlayıcısı seçimi sayfaya" eklenen parçası

Bu sayfa kullanıcı kaydolma veya oturum açma sırasında seçebileceği kimlik sağlayıcıları listesini içerir. Bu düğme, Facebook ve Google + veya yerel hesaplar (e-posta adresi veya kullanıcı adına göre) gibi sosyal kimlik sağlayıcıları içerir.

```HTML
<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-local-account-sign-up-page"></a>"Yerel hesap kayıt sayfasına" eklenen parçası

Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre kaydolma yerel hesap için bir form içerir. Form, metin giriş kutusuna, parola giriş kutusu, radyo düğmesi, tek seçimlik açılan kutuları ve çoklu seçim onay kutuları gibi farklı giriş denetimlerini içerebilir.

```HTML
<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-social-account-sign-up-page"></a>"" Sosyal hesabı kayıt sayfasına"eklenen parçası

Bu sayfa, Facebook veya Google + gibi sosyal kimlik sağlayıcısından var olan bir hesabı kullanarak kaydolmak görünebilir.  Kayıt formunu kullanarak son kullanıcıdan ek bilgileri toplanması gereken olduğunda kullanılır. Bu sayfa (önceki bölümde gösterilmiştir) yerel hesap kayıt sayfasına parola giriş alanları dışında benzerdir.

### <a name="fragment-inserted-into-the-unified-sign-up-or-sign-in-page"></a>"Birleşik kayıt veya oturum açma sayfasına" eklenen parçası

Bu sayfa, hem kaydolma ve oturum açma Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanan müşteriler, işler.

```HTML
<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>
```

### <a name="fragment-inserted-into-the-multi-factor-authentication-page"></a>"Çok faktörlü kimlik doğrulama sayfasına" eklenen parçası

Bu sayfada, kullanıcıların telefon numaralarına (metin veya sesli kullanarak) kaydolma veya oturum açma sırasında doğrulayabilirsiniz.

```HTML
<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-error-page"></a>"" Hata sayfasına"eklenen parçası

```HTML
<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>
```

## <a name="localizing-your-html-content"></a>HTML içeriğinizi yerelleştirme

Açarak HTML içeriğinizi yerelleştirebilirsiniz ['Dil özelleştirme'](active-directory-b2c-reference-language-customization.md).  Bu özelliği etkinleştirmek sağlayan Open ID Connect parametre iletmek Azure AD B2C `ui-locales`, uç noktanız için.  İçeriğinize dile özgü özelleştirilmiş HTML sayfaları sağlamak için bu parametreyi kullanabilirsiniz.

## <a name="things-to-remember-when-building-your-own-content"></a>Bunları kendi içeriğinizi oluştururken unutmayın

Sayfa UI Özelleştirme özelliğini kullanmayı planlıyorsanız, aşağıdaki en iyi uygulamaları gözden geçirin:

* Yoksa, Azure AD B2C'ın varsayılan içeriği Kopyala ve onu değiştirme girişimi. HTML5 içeriğinizi sıfırdan oluşturmak ve varsayılan içerik referans olarak kullanmak için en iyisidir.
* Güvenlik nedenleriyle, biz, içeriği tüm JavaScript dahil etmenize izin vermez. Gerekenler çoğu kutu dışı kullanılabilir olması gerekir. Aksi takdirde, kullanın [kullanıcı sesi](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) yeni işlevsellik istemek için.
* Tarayıcı sürümleri desteklenir:
  * Internet Explorer 11, 10, sınır
  * Internet Explorer 9, 8 için sınırlı destek
  * Google Chrome 42.0 ve üstü
  * Mozilla Firefox 38.0 ve üstü
