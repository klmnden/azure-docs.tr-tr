---
title: Azure API Management şablonlarında sayfasında | Microsoft Docs
description: Azure API Yönetimi'nde bir dizi şablonları kullanarak Geliştirici portal sayfalarının içeriğini özelleştirme öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: apimpm
ms.openlocfilehash: 1fbafcdab938a0f8653df48631d7733cc58a3668
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60656816"
---
# <a name="page-templates-in-azure-api-management"></a>Azure API Management sayfası şablonları
Azure API Management içeriklerini yapılandıran bir dizi kullanarak Geliştirici portal sayfalarının içeriğini özelleştirme becerisi sunuyor. Kullanarak [DotLiquid](http://dotliquidmarkup.org/) söz dizimi ve tercih ettiğiniz düzenleyiciyi gibi [tasarımcılarına yönelik DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), ve sağlanan bir dizi yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [karakter Kaynakları](api-management-template-resources.md#glyphs), ve [sayfasında denetimleri](api-management-page-controls.md), sayfaların içeriğini bu şablonları kullanarak dilediğiniz şekilde yapılandırmak için harika esnekliğine sahip olursunuz.  
  
 Bu bölümdeki şablonları yukarı oturum açma, oturum içeriğini özelleştirmenize olanak sağlar ve sayfaları Geliştirici portalında sayfa bulunamadı.  
  
-   [Oturum Aç](#SignIn)  
  
-   [Kaydolma](#SignUp)  
  
-   [Sayfa bulunamadı](#PageNotFound)  
  
> [!NOTE]
>  Örnek varsayılan şablonları aşağıdaki belgelerde bulunan, ancak sürekli geliştirmeler nedeniyle değiştirilebilir. İstenen bireysel şablonlara giderek Canlı varsayılan şablonları Geliştirici Portalı'nda görüntüleyebilirsiniz. Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="SignIn"></a> Oturum Aç  
 **Oturum** şablon oturum açma sayfasında Geliştirici Portalı özelleştirmenize olanak sağlar.  
  
 ![Oturum açma sayfasında](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM giriş sayfasında Geliştirici Portal şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
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
  
-   [temel oturum aç](api-management-page-controls.md#basic-signin)  
  
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
    "AuxServiceUrl": "https://portal.azure.com/#resource/subscriptions/{subscription ID}/resourceGroups/Api-Default-West-US/providers/Microsoft.ApiManagement/service/contoso5",
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
  
##  <a name="SignUp"></a> Kaydol  
 **Kaydolun** şablon Geliştirici Portalı sayfa kaydolma özelleştirmenize olanak sağlar.  
  
 ![Oturum açma sayfasına](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM kaydolma sayfasında Geliştirici Portal şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
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
  
##  <a name="PageNotFound"></a> Sayfa bulunamadı  
 **Sayfa bulunamadı** şablon Geliştirici portalında sayfa bulunamadı sayfayı özelleştirmek sağlar.  
  
 ![Sayfa bulunamadı](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Portal şablonları sayfasında Geliştirici bulunamadı")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
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
|referenceCode|string|Bu sayfa bir iç hata sonucunda görüntülendiyse üretilen kod.|  
|errorCode|string|Bu sayfa bir iç hata sonucunda görüntülendiyse üretilen kod.|  
|emailBody|string|Bu sayfa bir iç hata sonucunda görüntülendiyse oluşturulan gövdesi e-posta.|  
|requestedUrl|string|Sayfa bulunamadı, istenen URL.|  
|referrerUrl|string|İstenen URL başvuran URL'si.|  
  
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
Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](api-management-developer-portal-templates.md).
