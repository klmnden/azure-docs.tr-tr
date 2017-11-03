---
title: "Sayfa Şablonları Azure API Management | Microsoft Docs"
description: "Azure API Management'te şablonları kümesi kullanılarak Geliştirici portal sayfalarına içeriğini özelleştirmeyi öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: aca44e14ab85fcfeb9d1eb3c3eadfff7831c372f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="page-templates-in-azure-api-management"></a>Azure API Management sayfası şablonları
Azure API Management Geliştirici portal sayfalarına içeriklerini yapılandırma şablonları kümesini kullanarak içeriği özelleştirme yeteneği sağlar. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) sözdizimi ve düzenleyiciyi, gibi [DotLiquid tasarımcıları için](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), ve sağlanan bir dizi yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [karakter kaynakları](api-management-template-resources.md#glyphs), ve [sayfa denetimleri](api-management-page-controls.md), bu şablonları kullanarak uygun gördüğünüz şekilde sayfaların yapılandırmak için büyük esneklik vardır.  
  
 Bu bölümdeki şablonları, oturum açma, oturum içeriğini yukarı özelleştirmenize olanak tanır ve sayfa sayfaları Geliştirici Portalı'nda bulunamadı.  
  
-   [Oturum Aç](#SignIn)  
  
-   [Kaydol](#SignUp)  
  
-   [Sayfa bulunamadı](#PageNotFound)  
  
> [!NOTE]
>  Örnek varsayılan şablonları aşağıdaki belgelerde yer alır ancak değişikliği sürekli geliştirmeler nedeniyle tabidir. İstenen tek tek şablonları giderek Geliştirici Portalı'nda Canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="SignIn"></a>Oturum Aç  
 **Oturum** şablonu oturum açma sayfasında Geliştirici portalında özelleştirmenizi sağlar.  
  
 ![Oturum açma sayfası](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM oturum açma sayfasında Geliştirici Portalı şablonları")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 Bu şablon aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [temel oturum açma](api-management-page-controls.md#basic-signin)  
  
-   [sağlayıcıları](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a>Veri modeli  
 [Kullanıcı oturum açma](api-management-template-data-model-reference.md#UseSignIn) varlık.  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Email": null,  
    "Password": null,  
    "ReturnUrl": null,  
    "RememberMe": false,  
    "RegistrationEnabled": true,  
    "DelegationEnabled": false,  
    "DelegationUrl": null,  
    "SsoSignUpUrl": null,  
    "AuxServiceUrl": "https://manage.windowsazure.com/#Workspaces/ApiManagementExtension/service/contoso5/dashboard",  
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
    ],  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsEnabled": false  
}  
```  
  
##  <a name="SignUp"></a>Kaydol  
 **Kaydolun** şablonu kayıt sayfasını Geliştirici portalında özelleştirmenizi sağlar.  
  
 ![Oturum açma sayfasına](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "sayfasında Geliştirici Portalı şablonları APIM kaydolma")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 Bu şablon aşağıdaki kullanabilir [sayfasında denetimleri](api-management-page-controls.md).  
  
-   [Kaydolma](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a>Veri modeli  
 [Kullanıcı Kaydolma](api-management-template-data-model-reference.md#UserSignUp) varlık.  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <a name="PageNotFound"></a>Sayfa bulunamadı  
 **Sayfa bulunamadı** şablon sayfa Geliştirici Portalı'nda bulunamadı sayfayı özelleştirmek sağlar.  
  
 ![Sayfa bulunamadı](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Portal şablonları sayfasında Geliştirici bulunamadı")  
  
### <a name="default-template"></a>Varsayılan şablonu  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a>Denetimler  
 Bu şablon herhangi kullanamazsınız [sayfasında denetimleri](api-management-page-controls.md).  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|referenceCode|Dize|Bu sayfa bir iç hata sonucunda görüntüleniyorsa oluşturulan kod.|  
|hata kodu|Dize|Bu sayfa bir iç hata sonucunda görüntüleniyorsa oluşturulan kod.|  
|emailBody|Dize|Bu sayfa bir iç hata sonucunda görüntüleniyorsa oluşturulan gövde e-posta.|  
|requestedUrl|Dize|Sayfa bulunamadı, istenen URL.|  
|referrerUrl|Dize|İstenen URL başvuran URL.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).