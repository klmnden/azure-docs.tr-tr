---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 7a6fa45d0f4ea09fbc84d3185aa0e58db165ac19
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947365"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanıza ekleme

Bu adımda, uygulama kayıt bilgilerini yeniden yönlendirme URL'sini yapılandırın ve uygulama kimliği, JavaScript SPA'ya uygulamanıza eklemek gerekir.

### <a name="configure-redirect-url"></a>Yeniden yönlendirme URL'sini yapılandırın

Yapılandırma `Redirect URL` alan index.html sayfanızı web sunucunuzda URL'si ile'a tıklayın *güncelleştirme*.

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Yeniden yönlendirme URL'sini almak için visual Studio yönergeleri
> Yeniden yönlendirme URL'sini almak için aşağıdaki adımları izleyin:
> 1. İçinde **Çözüm Gezgini**, projeyi seçin ve bakmak **özellikleri** penceresi. Görmüyorsanız, bir **özellikleri** penceresinde, tuşuna **F4**.
> 2. Değeri Şuradan Kopyala: **URL** Pano için:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Değeri olarak yapıştırın bir **tekrar yönlendirme URL'sini** bu üst kısmındaki sayfasında ve ardından **güncelleştirme**.

<p>

> #### <a name="setting-redirect-url-for-node"></a>Düğüm ayarı yeniden yönlendirme URL'si
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğretici için başvuru 30662 bağlantı noktasını kullanır ancak kullanılabilir herhangi bir bağlantı kullanabilirsiniz. Uygulama kayıt bilgileri bir yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri izleyin:<br/>
> Ayarlama `http://localhost:30662/` olarak bir **tekrar yönlendirme URL'sini** bu sayfa veya kullanımı üst kısmındaki `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız (burada *[bağlantı noktası]* özel TCP bağlantı noktası numarası) ve ardından  **Güncelleştirme**

### <a name="configure-your-javascript-spa-application"></a>JavaScript SPA'ya uygulamanızı yapılandırma

1. İçinde `index.html` dosyası proje Kurulum sırasında oluşturulur, uygulama kayıt bilgilerini ekleyin. Üst içinde aşağıdaki kodu ekleyin `<script></script>` gövdesinde etiketleri, `index.html` dosyası:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    authority: "https://login.microsoftonline.com/common",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
