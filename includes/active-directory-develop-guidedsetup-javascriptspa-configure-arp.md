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
ms.openlocfilehash: eead4c6a66a317c7404205415cbf04c442ffe8d1
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060354"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanıza ekleme

Bu adımda, uygulama kayıt bilgileri yeniden yönlendirme URL'sini yapılandırın ve uygulama kimliği, JavaScript SPA'ya uygulamanıza eklemek gerekir.

### <a name="configure-redirect-url"></a>Yeniden yönlendirme URL'sini yapılandırın

Yapılandırma `Redirect URL` alan index.html sayfanızı web sunucunuzda URL'si ile'a tıklayın *güncelleştirme*.


> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Yeniden yönlendirme URL'sini almak için visual Studio yönergeleri
> Yeniden yönlendirme URL'sini almak için aşağıdaki adımları izleyin:
> 1.    İçinde **Çözüm Gezgini**, projeyi seçin ve bakmak **özellikleri** penceresi. Görmüyorsanız, bir **özellikleri** penceresinde, tuşuna **F4**.
> 2.    Değeri Şuradan Kopyala: **URL** Pano için:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Değeri olarak yapıştırın bir **tekrar yönlendirme URL'sini** bu sayfanın üst kısmındaki ardından **güncelleştirme**

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Düğüm ayarı yeniden yönlendirme URL'si
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğretici için başvuru 30662 bağlantı noktasını kullanır ancak kullanılabilir herhangi bir bağlantı kullanabilirsiniz. Uygulama kayıt bilgileri bir yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri izleyin:<br/>
> Ayarlama `http://localhost:30662/` olarak bir **tekrar yönlendirme URL'sini** bu sayfa veya kullanımı üst kısmındaki `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız (burada *[bağlantı noktası]* özel TCP bağlantı noktası numarası) ve ardından  **Güncelleştirme**

### <a name="configure-your-javascript-spa-application"></a>JavaScript SPA'ya uygulamanızı yapılandırma

1.  İçinde `index.html` dosyası proje Kurulum sırasında oluşturulur, uygulama kayıt bilgilerini ekleyin. Üst içinde aşağıdaki kodu ekleyin `<script></script>` gövdesinde etiketleri, `index.html` dosyası:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
