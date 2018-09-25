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
ms.openlocfilehash: 07aac49e7aed7c95863a2058a9de3d1e8f2cd1ad
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060325"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Bir uygulama oluşturmak, bunlardan birini seçmek için birden çok yolu vardır:

### <a name="option-1-register-your-application-express-mode"></a>1. seçenek: Register uygulamanızı (hızlı mod)
Artık uygulamanızı kaydetmek gerekiyor *Microsoft uygulama kayıt portalı*:

1.  Uygulamanızı kaydetmek [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Uygulamanız ve e-posta adresiniz için bir ad girin.
3.  Seçenek emin olmak **destekli Kurulum** denetlenir.
4.  Uygulama Kimliğini almak ve kodunuza yapıştırmak için yönergeleri izleyin.

### <a name="option-2-register-your-application-advanced-mode"></a>2. seçenek: Register uygulamanızı (Gelişmiş mod)

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetme.
2. Uygulamanız ve e-posta adresiniz için bir ad girin.
3. Seçenek emin olmak **destekli Kurulum** olarak işaretli değildir.
4.  Tıklayın **Platform Ekle**, ardından **Web**.
5. Ekleme **tekrar yönlendirme URL'sini** karşılık gelen web sunucunuzda tabanlı uygulamanın URL'si. Ayarlayın ve Visual Studio ve düğüm yeniden yönlendirme URL'sini almak hakkında yönergeler için aşağıdaki bölümlere bakın.
6. **Kaydet**’i seçin.

> #### <a name="setting-redirect-url-for-node"></a>Düğüm ayarı yeniden yönlendirme URL'si
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğreticide, başvuru ancak tüm diğer bağlantı noktası kullanılabilir ücretsiz kullanım için bağlantı noktası 30662 kullanılır. Herhangi bir durumda, bir uygulama kayıt bilgileri yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri izleyin:<br/>
> - Dönmek *uygulama kayıt portalı* ayarlayıp `http://localhost:30662/` olarak bir `Redirect URL`, veya `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız (burada *[bağlantı noktası]* özel TCP bağlantı noktası sayı) ve 'Kaydet' tıklayın

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Yeniden yönlendirme URL'sini almak için visual Studio yönergeleri
> Yeniden yönlendirme URL'sini almak için aşağıdaki adımları izleyin:
> 1.    İçinde **Çözüm Gezgini**, projeyi seçin ve bakmak **özellikleri** penceresi. Görmüyorsanız, bir **özellikleri** penceresinde, tuşuna **F4**.
> 2.    Değeri Şuradan Kopyala: **URL** Pano için:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Dönmek *uygulama kayıt portalı* değeri olarak yapıştırın bir **tekrar yönlendirme URL'sini** seçip **Kaydet**


#### <a name="configure-your-javascript-spa"></a>JavaScript SPA'ya yapılandırın

1.  İçinde `index.html` dosyası proje Kurulum sırasında oluşturulur, uygulama kayıt bilgilerini ekleyin. Üst içinde aşağıdaki kodu ekleyin `<script></script>` gövdesinde etiketleri, `index.html` dosyası:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
<ol start="3">
<li>
Değiştirin <code>Enter the application Id here</code> yalnızca kayıtlı uygulama kimliğine sahip.
</li>
</ol>
