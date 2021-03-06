---
title: JavaScript örnekleri - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de JavaScript kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9e19df7c50ca9d2c57ab385a567f4911b200c5e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510883"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanmak için JavaScript örnekleri

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory (Azure AD) B2C uygulamalarınızın, kendi JavaScript istemci tarafı kod ekleyebilirsiniz. Uygulamalarınız için JavaScript'i etkinleştirmek için bir öğeye ekleyin, [özel ilke](active-directory-b2c-overview-custom.md)seçin bir [sayfa sözleşme](page-contract.md)ve [b2clogin.com](b2clogin.md) isteklerinizdeki. Bu makalede, özel ilkeniz betik yürütmesini etkinleştirme nasıl değiştirebileceğiniz açıklanır.

> [!NOTE]
> Kullanıcı akışları için JavaScript etkinleştirmek istiyorsanız, bkz. [sözleşme JavaScript ve sayfa, Azure Active Directory B2C sürümlerinde](user-flow-javascript-overview.md).

## <a name="prerequisites"></a>Önkoşullar

Uygulamanızın kullanıcı arabirimi öğeleri için bir sayfa Sözleşmesi'ni seçin. JavaScript kullanmak istiyorsanız, özel ilkeniz tüm içerik tanımları için bir sayfa sözleşme sürümü tanımlamak gerekir.

## <a name="add-the-scriptexecution-element"></a>ScriptExecution öğe ekleyin

Ekleyerek betik yürütmesini etkinleştirme **ScriptExecution** öğesine [RelyingParty](relyingparty.md) öğesi.

1. Özel ilke dosyanızı açın. Örneğin, *SignUpOrSignin.xml*.
2. Ekleme **ScriptExecution** öğesine **UserJourneyBehaviors** öğesinin **RelyingParty**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. Kaydedin ve dosyayı karşıya yükleyin.

## <a name="guidelines-for-using-javascript"></a>JavaScript kullanma yönergeleri

JavaScript kullanarak uygulamanızı arabiriminin'nı özelleştirdiğinizde, aşağıdaki yönergeleri izleyin:

- Bir tıklama olayı bağlamayın `<a>` HTML öğeleri.
- Azure AD B2C kod ya da yorumlarınız bağımlılık almaz.
- Sipariş veya Azure AD B2C HTML öğeleri hiyerarşisini değiştirmeyin. UI öğelerin sırasını denetlemek için bir Azure AD B2C İlkesi'ni kullanın.
- Bu konuları ile herhangi bir RESTful hizmeti çağırabilirsiniz:
    - İstemci tarafı HTTP çağrıları izin vermek için CORS'yi RESTful hizmetinizi ayarlamanız gerekebilir.
    - RESTful hizmetiniz güvendedir ve yalnızca HTTPS protokolünü kullanan emin olun.
    - JavaScript, Azure AD B2C uç noktaları doğrudan çağırmak için kullanmayın.
- Javascript'inizi ekleyebilir veya dış JavaScript dosyalarına bağlayabilirsiniz. Harici bir JavaScript dosyasını kullanırken, mutlak bir URL ve göreli bir URL değil kullanmaya dikkat edin.
- Yeni JavaScript çerçevesi:
    - Azure AD B2C, jQuery belirli bir sürümünü kullanır. JQuery başka bir sürümünü içermez. Aynı sayfa üzerinde birden fazla sürümünü kullanarak sorunlarına neden olur.
    - RequireJS kullanımı desteklenmiyor.
    - En yeni JavaScript çerçevesi, Azure AD B2C tarafından desteklenmez.
- Azure AD B2C ayarlarını çağırarak okunabilir `window.SETTINGS`, `window.CONTENT` geçerli kullanıcı Arabirimi dili gibi nesneler. Bu nesneler değerini değiştirmeyin.
- Azure AD B2C hata iletisini özelleştirmek için bir ilke yerelleştirme kullanın.
- Herhangi bir ilke kullanarak gerçekleştirilebilir, genellikle bu önerilen yoludur.

## <a name="javascript-samples"></a>JavaScript örnekleri

### <a name="show-or-hide-a-password"></a>Bir parolayı Gizle veya Göster

Kaydolma başarılarını ile müşterilerinize yardımcı olmak için yaygın bir yolu, bunları ne bunlar kendi parolanızı girdikten görmek izin vermektir. Bu seçenek, kullanıcıların kolayca görebilir ve gerekirse parolasını düzeltmeler yapmak sağlayarak kaydolma yardımcı olur. Checkbox ile herhangi bir alan türü parolası olan bir **Show parola** etiketi.  Bu düz metin parolayı görmesine olanak sağlar. Bu kod parçacığı uygulamasına kaydolma veya oturum açma şablonunuz için bir otomatik olarak onaylanan sayfa şunları içerir:

```Javascript
function makePwdToggler(pwd){
  // Create show-password checkbox
  var checkbox = document.createElement('input');
  checkbox.setAttribute('type', 'checkbox');
  var id = pwd.id + 'toggler';
  checkbox.setAttribute('id', id);

  var label = document.createElement('label');
  label.setAttribute('for', id);
  label.appendChild(document.createTextNode('show password'));

  var div = document.createElement('div');
  div.appendChild(checkbox);
  div.appendChild(label);

  // Add show-password checkbox under password input
  pwd.insertAdjacentElement('afterend', div);

  // Add toggle password callback
  function toggle(){
    if(pwd.type === 'password'){
      pwd.type = 'text';
    } else {
      pwd.type = 'password';
    }
  }
  checkbox.onclick = toggle;
  // For non-mouse usage
  checkbox.onkeydown = toggle;
}

function setupPwdTogglers(){
  var pwdInputs = document.querySelectorAll('input[type=password]');
  for (var i = 0; i < pwdInputs.length; i++) {
    makePwdToggler(pwdInputs[i]);
  }
}

setupPwdTogglers();
```

### <a name="add-terms-of-use"></a>Kullanım koşulları ekleme

Aşağıdaki kod, eklemek istediğiniz sayfanıza dahil bir **kullanım** onay kutusu. Bu onay kutusunu, genellikle, yerel hesap kaydolma ve sosyal hesap kaydolma sayfalarında gereklidir.

```Javascript
function addTermsOfUseLink() {
    // find the terms of use label element
    var termsOfUseLabel = document.querySelector('#api label[for="termsOfUse"]');
    if (!termsOfUseLabel) {
        return;
    }

    // get the label text
    var termsLabelText = termsOfUseLabel.innerHTML;

    // create a new <a> element with the same inner text
    var termsOfUseUrl = 'https://docs.microsoft.com/legal/termsofuse';
    var termsOfUseLink = document.createElement('a');
    termsOfUseLink.setAttribute('href', termsOfUseUrl);
    termsOfUseLink.setAttribute('target', '_blank');
    termsOfUseLink.appendChild(document.createTextNode(termsLabelText));

    // replace the label text with the new element
    termsOfUseLabel.replaceChild(termsOfUseLink, termsOfUseLabel.firstChild);
}
```

Kod içinde `termsOfUseUrl` kullanım anlaşma koşullarınıza bağlantı. Dizininiz için adlı yeni bir kullanıcı özniteliği oluşturma **termsOfUse** ve ardından **termsOfUse** kullanıcı özniteliği olarak.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı arabirimi, uygulamalarınızın nasıl özelleştirebileceğiniz hakkında daha fazla bilgi [özel bir ilke kullanarak Azure Active Directory B2C'de, uygulamanızın kullanıcı arabirimini özelleştirme](active-directory-b2c-ui-customization-custom.md).
