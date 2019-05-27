---
title: Bilinen sorunlar (JavaScript için Microsoft kimlik doğrulama kitaplığı) tarayıcılarda | Azure
description: Microsoft kimlik doğrulama kitaplığı (MSAL.js) JavaScript için kullanırken bilinen sorunlar ile Internet Explorer ve Microsoft Edge tarayıcılarını öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c57ed956ec50c8bac26720a27894c07353928336
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873908"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Internet Explorer ve Microsoft Edge tarayıcılarda MSAL.js bilinen sorunlar

## <a name="issues-due-to-security-zones"></a>Sorunları nedeniyle güvenlik bölgeleri
Birden çok rapor IE ve Microsoft Edge kimlik doğrulaması ile ilgili sorunları vardı (güncelleştirilmesini beri *40.15063.0.0 Microsoft Edge tarayıcı sürümüne*). Biz bu izleme ve Microsoft Edge ekibi bildirdi. Microsoft Edge bir çözüm üzerinde çalışırken, sık karşılaşılan sorunlar ve olası geçici çözümler uygulanabilir açıklaması aşağıda verilmiştir.

### <a name="cause"></a>Nedeni
Neden bu sorunların çoğu için aşağıdaki gibidir. Oturum depolama ve yerel depolama, güvenlik bölgeleri'nde Microsoft Edge tarayıcısı tarafından bölümlenir. Uygulama alanları genelinde yönlendirildiğinde Microsoft Edge bu belirli sürümünde oturumu depolama ve yerel depolama temizlenir. Özellikle, oturumu depolama normal tarayıcı Gezinti temizlenir ve hem oturum hem de yerel depolama InPrivate tarayıcı modunda temizlenir. MSAL.js oturumu depolamada belirli durumunu kaydeder ve bu durum sırasında kimlik doğrulama akışları denetimini kullanır. Oturum depolama temizlendiğinde, bu durum kaybolur ve bu nedenle bozuk deneyimlerinde sonuçlanır.

### <a name="issues"></a>Sorunlar

- **Sonsuz bir yeniden yönlendirme döngüleri ve sayfa kimlik doğrulaması sırasında yeniden yükler**. Microsoft Edge üzerinde uygulamaya kullanıcılar oturum açtığında AAD oturum açma sayfasında geri yönlendirilirsiniz ve içinde yinelenen sayfası yüklemelere kaynaklanan bir sonsuz yeniden yönlendirme döngüde takılı kalıyor. Bu genellikle eşlik ettiği bir `invalid_state` oturumu depolama hatası.

- **Sonsuz alma belirteci döngüler ve AADSTS50058 hata**. Bir kaynak için bir belirteç almak Microsoft Edge üzerinde çalışan bir uygulama çalıştığında, uygulama alma belirteci çağrısı şu hata ile birlikte aad'den, ağ izleme sonsuz bir döngüde takılabilir:

    `Error :login_required; Error description:AADSTS50058: A silent sign-in request was sent but no user is signed in. The cookies used to represent the user's session were not sent in the request to Azure AD. This can happen if the user is using Internet Explorer or Edge, and the web app sending the silent sign-in request is in different IE security zone than the Azure AD endpoint (login.microsoftonline.com)`

- **Açılan pencere kapatmadığına veya oturumu açılır menüsü aracılığıyla kimlik doğrulaması kullanırken takılı**. Microsoft Edge ya da IE(InPrivate), açılan pencerede aracılığıyla kimlik bilgilerini girme ve birden çok etki alanı güvenlik bölgeleri arasında gezintide söz konusuysa oturum açtıktan sonra kimlik doğrulaması yapılırken MSAL.js tanıtıcısını kaybeder çünkü açılan pencere kapatmadığına açılan pencere.  

    Microsoft Edge sorun İzleyicisi, bu sorunları için bağlantılar şunlardır:  
    - [Hata 13861050](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861050/)
    - [Hata 13861663](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861663/)

### <a name="update-fix-available-in-msaljs-023"></a>Güncelleştir: Düzeltme 0.2.3 MSAL.js içinde kullanılabilir
Kimlik doğrulaması yeniden yönlendirme döngüsü sorunlarını yayınlanmıştır gideren [MSAL.js 0.2.3](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases). Etkinleştirme bayrağını `storeAuthStateInCookie` MSAL.js yapılandırmada bu düzeltmenin yararlanın. Varsayılan olarak bu bayrağı false olarak ayarlanır.

Zaman `storeAuthStateInCookie` bayrağı etkin olduğunda, MSAL.js kimlik doğrulama akışları doğrulanması için gerekli istek durumunu depolamak için tarayıcı tanımlama bilgileri kullanır.

> [!NOTE]
> Bu düzeltme henüz msal angular ve msal angularjs sarmalayıcıları için kullanılamıyor. Bu düzeltme, açılır pencereleri sorun adres değil.

Aşağıdaki geçici çözümleri kullanın.

#### <a name="other-workarounds"></a>Diğer geçici çözümler
Bu çözümler benimsemeden önce sorunu Microsoft Edge tarayıcısı diğer tarayıcılar üzerinde çalışır ve yalnızca belirli sürümünde oluştuğunu test emin olun.  
1. Bu sorunların çözüm almak için ilk adım, uygulama etki alanı olduğundan emin olun, ve böylece aynı güvenlik bölgesine ait oldukları kimlik doğrulaması akışı yeniden yönlendirmelerini katılan diğer siteleri, tarayıcının güvenlik ayarlarını Güvenilen siteler olarak eklenir.
Bunu yapmak için aşağıdaki adımları izleyin:
    - Açık **Internet Explorer** tıklayın **ayarları** (dişli simgesi) sağ üst köşedeki
    - Seçin **Internet Seçenekleri**
    - Seçin **güvenlik** sekmesi
    - Altında **Güvenilen siteler** seçeneğinde, tıklayarak **siteleri** düğmesine tıklayın ve açılan iletişim kutusunda URL'leri ekleyin.

2. Yalnızca bu yana oturumu sırasında normal Gezinti depolama temizlenmeden önce bahsedildiği gibi yerel depolama kullanmayı MSAL.js yapılandırabilirsiniz. Bu, olarak ayarlanabilir `cacheLocation` MSAL başlatılırken yapılandırma parametresi.

Not: Bu hem oturum hem de yerel depolama temizlenmiş olduğundan InPrivate Gözatma için sorunu çözmek için değil.

## <a name="issues-due-to-popup-blockers"></a>Açılan pencere engelleyici nedeniyle sorunları

Açılan pencereler IE veya Microsoft Edge, örneğin, çok faktörlü kimlik doğrulaması sırasında ikinci bir açılır pencere oluştuğunda engellendiğinde durumlar vardır. Popup için bir kez veya her zaman izin vermek için tarayıcıda bir uyarı alırsınız. İzin vermeyi seçerseniz, tarayıcı açılır pencere otomatik olarak açılır ve döndüren bir `null` işlemek için. Sonuç olarak, kitaplık penceresi için bir tanıtıcı yok ve açılır pencereyi kapatmak için bir yolu yoktur. Açılan pencereler bir açılan pencere otomatik olarak açılmaz için izin istediğinde Chrome'da aynı sorun gerçekleşmez.

Olarak bir **geçici çözüm**, geliştiriciler, bu sorunu önlemek için uygulamayı kullanmaya başlamadan önce IE ve Microsoft Edge açılan pencereler izin gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Internet Explorer'da MSAL.js kullanılarak](msal-js-use-ie-browser.md).
