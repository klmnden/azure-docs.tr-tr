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
ms.openlocfilehash: fc06da3b1ad66aa15237a25d2f50374043c860ba
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293671"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanıza ekleme

Bu adımda, uygulama kayıt bilgileri yeniden yönlendirme URL'sini yapılandırın ve uygulama kimliği, JavaScript SPA'ya uygulamanıza eklemek gerekir.

### <a name="configure-redirect-url"></a>Yeniden yönlendirme URL'sini yapılandırın

Yapılandırma `Redirect URL` alan index.html sayfanızı web sunucunuzda URL'si ile'a tıklayın *güncelleştirme*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Yeniden yönlendirme URL'si almak için visual Studio yönergeleri
> Yeniden yönlendirme URL'nizi elde etmek için:
> 1.    İçinde *Çözüm Gezgini*, projeyi seçin ve bakmak `Properties` penceresi (Özellikler penceresini görmüyorsanız, basın `F4`)
> 2.    Değeri Şuradan Kopyala: `URL` Pano için:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Değeri olarak yapıştırın bir `Redirect URL` bu sayfanın üst kısmındaki'ye tıklayın `Update`

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Düğüm ayarı yeniden yönlendirme URL'si
> Node.js için web sunucusu bağlantı noktası ayarlayabilirsiniz *server.js* dosya. Bu öğreticide, başvuru ancak tüm diğer bağlantı noktası kullanılabilir ücretsiz kullanım için bağlantı noktası 30662 kullanılır. Herhangi bir durumda, bir uygulama kayıt bilgileri yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri kullanın:<br/>
> Ayarlama `http://localhost:30662/` olarak bir `Redirect URL` bu sayfa veya kullanımı üst kısmındaki `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız (burada *[bağlantı noktası]* özel TCP bağlantı noktası numarası) 'Update' i tıklatın

### <a name="configure-your-javascript-spa-application"></a>JavaScript SPA'ya uygulamanızı yapılandırma

1.  Adlı bir dosya oluşturun `msalconfig.js` uygulama kayıt bilgilerini içeren. Visual Studio kullanıyorsanız, proje (projenin kök klasöründe), sütuna sağ tıklayıp seçin: `Add`  >  `New Item`  >  `JavaScript File`. Adlandırın `msalconfig.js`
2.  Aşağıdaki kodu ekleyin, `msalconfig.js` dosyası:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
