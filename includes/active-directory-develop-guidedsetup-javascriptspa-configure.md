---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 569a7e74df3016fae133066607fdc0bc32c0044d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293500"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Bir uygulama oluşturmak, bunlardan birini seçmek için birden çok yolu vardır:

### <a name="option-1-register-your-application-express-mode"></a>1. seçenek: Register uygulamanızı (hızlı mod)
Artık uygulamanızı kaydetmek gerekiyor *Microsoft uygulama kayıt portalı*:

1.  Uygulamanızı kaydetmek [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Uygulamanız ve e-posta adresiniz için bir ad girin
3.  Seçenek emin olmak *destekli Kurulum* iade edildiğinde
4.  Kodunuza yapıştırın ve uygulama Kimliğini almak için yönergeleri izleyin

### <a name="option-2-register-your-application-advanced-mode"></a>2. seçenek: Register uygulamanızı (Gelişmiş mod)

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetme
2. Uygulamanız ve e-posta adresiniz için bir ad girin 
3. Seçenek emin olmak *destekli Kurulum* işaretli değil
4.  Tıklayın `Add Platform`seçin `Web`
5. Ekleme `Redirect URL` , uygulamanın URL'sine göre web sunucunuzda karşılık gelir. Visual Studio ve Python yeniden yönlendirme URL'sini almak / ayarlamak hakkında yönergeler için aşağıdaki bölümlere bakın.
6. *Kaydet*’e tıklayın

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Yeniden yönlendirme URL'si almak için visual Studio yönergeleri
> Yeniden yönlendirme URL'nizi elde etmek için yönergeleri izleyin:
> 1.    İçinde *Çözüm Gezgini*, projeyi seçin ve bakmak `Properties` penceresi (Özellikler penceresini görmüyorsanız, basın `F4`)
> 2.    Değeri Şuradan Kopyala: `URL` Pano için:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Dönmek *uygulama kayıt portalı* değeri olarak yapıştırın bir `Redirect URL` 'Kaydet' tıklayın

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Düğüm ayarı yeniden yönlendirme URL'si
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğreticide, başvuru ancak tüm diğer bağlantı noktası kullanılabilir ücretsiz kullanım için bağlantı noktası 30662 kullanılır. Herhangi bir durumda, bir uygulama kayıt bilgileri yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri izleyin:<br/>
> - Dönmek *uygulama kayıt portalı* ayarlayıp `http://localhost:30662/` olarak bir `Redirect URL`, veya `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız (burada *[bağlantı noktası]* özel TCP bağlantı noktası sayı) ve 'Kaydet' tıklayın


#### <a name="configure-your-javascript-spa"></a>JavaScript SPA'ya yapılandırın

1.  Adlı bir dosya oluşturun `msalconfig.js` uygulama kayıt bilgilerini içeren. Visual Studio kullanıyorsanız, proje (projenin kök klasöründe), sütuna sağ tıklayıp seçin: `Add`  >  `New Item`  >  `JavaScript File`. Adlandırın `msalconfig.js`
2.  Aşağıdaki kodu ekleyin, `msalconfig.js` dosyası:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Değiştirin <code>Enter_the_Application_Id_here</code> yalnızca kayıtlı uygulama kimliğine sahip
</li>
</ol>
