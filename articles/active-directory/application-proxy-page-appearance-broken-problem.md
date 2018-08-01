---
title: Uygulama sayfası için bir uygulama proxy'si uygulaması düzgün görüntülemez | Microsoft Docs
description: Azure AD ile tümleştirilmiş sayfanın içinde uygulama proxy'si uygulamasını doğru görüntülenmiyor olduğunda Kılavuzu
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 118d5780145d0421160c70546f01dc930190185e
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365218"
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Uygulama sayfası için bir uygulama proxy'si uygulaması düzgün görüntülenmiyor

Bu makalede sayfasına gidin, ancak sayfadaki bir şey doğru görünmüyor Azure Active Directory Uygulama Proxy uygulamaları ile ilgili sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış
Bir uygulama proxy'si uygulama yayımladığınızda, uygulamaya erişirken yalnızca, kök altında sayfalarını daha erişilebilir. Uygulama için kullanılan kök İç URL sayfası doğru görüntülenmiyor, bazı sayfa kaynaklar eksik olabilir. Çözümlemek için yayımladığınız emin olun *tüm* kaynaklar için sayfa, uygulamanızın bir parçası olarak.

Eksik sorunun Ağ İzleyicisi'ni açarak olup olmadığını doğrulayabilirsiniz (Fiddler veya F12 gibi araçları Internet Explorer/Edge), sayfa yükleme ve 404 hatalarını aranıyor. Bu sayfa şu anda bulunamıyor ve bunları yayımlamak gereken gösterir.

Bu durumda bir örnek olarak, dahili URL'yi kullanarak masrafları uygulama yayımladığınız varsayılmaktadır http://myapps/expenses, ancak stil uygulamanın kullandığı http://myapps/style.css. Bu durumda, stil sayfası, masrafları uygulama yükleme durum için bir 404 hatası style.css yüklenmeye çalışılırken, uygulamanızda yayımlanmaz. Bu örnekte, bir iç URL uygulamayla yayımlayarak Sorun çözülene http://myapp/.

## <a name="problems-with-publishing-as-one-application"></a>Bir uygulama yayımlama ile ilgili sorunlar

Aynı uygulama içindeki tüm kaynaklar yayımlamak mümkün değildir, birden çok uygulama yayımlama ve aralarındaki bağlantıları etkinleştirmek gerekir.

Bunu yapmak için kullanmanızı öneririz [özel etki alanları](manage-apps/application-proxy-configure-custom-domain.md) çözüm. Ancak, bu çözüm, etki alanınız için sertifika kendi ve uygulamalarınızı tam etki alanı adlarını (FQDN) kullanmak gerektirir. Diğer seçenekler için bkz. [bağlantıların belgeleri sorun giderme](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md)
