---
title: "Azure AD v2 JS SPA destekli kurulum - yapılandırma | Microsoft Docs"
description: "JavaScript SPA uygulamaları Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: adff529bfdc40006666cc643d49a302d7f1d09ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Bir uygulama oluşturmak, Lütfen bunlardan birini seçmek için birden çok yolu vardır:

### <a name="option-1-register-your-application-express-mode"></a>Seçenek 1: Kayıt uygulamanız (hızlı mod)
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:

1.  Uygulamanızı aracılığıyla kaydetme [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Seçenek emin olmak *destekli Kurulum* denetlenir
4.  Uygulama Kimliğini almak ve kodunuza yapıştırmak için yönergeleri izleyin

### <a name="option-2-register-your-application-advanced-mode"></a>Seçenek 2: uygulamanızı (Gelişmiş mod) kaydetme

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için
2. Uygulamanız ve e-posta için bir ad girin 
3. Seçenek emin olmak *destekli Kurulum* işaretli değil
4.  Tıklatın `Add Platform`sonra seçin`Web`
5. Ekleme `Redirect URL` , web sunucunuzda tabanlı uygulamanın URL'ye karşılık gelir. Ayarlamak Visual Studio ve Python yeniden yönlendirme URL'sini elde hakkında yönergeler için aşağıdaki bölümlerde bakın.
6. Tıklatın *Kaydet*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio yönergeleri almak için yeniden yönlendirme URL'si
> Yeniden yönlendirme URL'sini elde etmek için yönergeleri izleyin:
> 1.    İçinde *Çözüm Gezgini*, projeyi seçin ve ardından bakmak `Properties` penceresi (Özellikler penceresi görmüyorsanız, basın `F4`)
> 2.    Değerinden kopyalama `URL` panoya:<br/> ![Proje Özellikleri](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Dönmek *uygulama kayıt portalı* değeri olarak Yapıştır bir `Redirect URL` ve 'Kaydet' düğmesini tıklatın

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Python için ayar yeniden yönlendirme URL'si
> Python için web sunucusu bağlantı noktası komut satırı aracılığıyla ayarlayabilirsiniz. Bu Destekli Kurulum, başvuru ancak kullanımında herhangi diğer bağlantı noktası kullanılabilir kullanmak boş 8080 bağlantı noktası kullanır. Herhangi bir durumda, uygulama kayıt bilgileri yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri izleyin:<br/>
> - Dönmek *uygulama kayıt portalı* ve `http://localhost:8080/` olarak bir `Redirect URL`, veya `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız, (burada *[bağlantı noktası]* özel TCP bağlantı noktası sayı) ve 'Kaydet' düğmesini tıklatın


#### <a name="configure-your-javascript-spa"></a>JavaScript SPA yapılandırın

1.  Adlı bir dosya oluşturun `msalconfig.js` uygulama kayıt bilgilerini içeren. Visual Studio kullanıyorsanız, proje (Proje kök klasöründe), sağ tıklatın ve seçin seçin: `Add`  >  `New Item`  >  `JavaScript File`. Adlandırın`msalconfig.js`
2.  Aşağıdaki kodu ekleyin, `msalconfig.js` dosyası:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Değiştir <code>Enter_the_Application_Id_here</code> yalnızca kayıtlı uygulama kimliği
</li>
</ol>
