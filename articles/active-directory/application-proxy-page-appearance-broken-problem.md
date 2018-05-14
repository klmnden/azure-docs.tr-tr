---
title: Uygulama sayfası için uygulama proxy'si uygulama düzgün görüntülemez | Microsoft Docs
description: Azure AD ile tümleşik sayfa doğru bir uygulama Proxy uygulamada değil görüntülenirken Kılavuzu
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: asteen
ms.openlocfilehash: d15c02d86dc4a61e7e92b4c896cfc13dd0218c61
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Uygulama sayfası için uygulama proxy'si uygulama düzgün görüntülenmez

Bu makale sayfasına gidin, ancak sayfasında bir şey doğru görünmüyor Azure Active Directory Uygulama proxy'si uygulamalar ile ilgili sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış
Bir uygulama proxy'si uygulama yayımladığınızda, uygulama erişirken yalnızca, kök altındaki sayfalar daha erişilebilir. Sayfa doğru görüntülemiyorsa, uygulama için kullanılan kök İç URL bazı sayfası kaynakları eksik olabilir. Çözmek için yayımlanan emin olun *tüm* sayfa uygulamanızın parçası olarak için kaynaklar.

Kaynakları eksik sorunun Ağ İzleyicisi'ni açarak olup olmadığını doğrulayabilirsiniz (Fiddler veya F12 gibi araçlar Internet Explorer/kenar), sayfa yüklenirken ve 404 hatalarını aranıyor. Sayfaları şu anda bulunamıyor ve bunları yayınlamak gerektiğini gösterir.

Bu durumda bir örnek olarak, dahili URL'yi kullanarak giderleri uygulama yayımlandı varsayalım http://myapps/expenses, ancak stil uygulamanın kullandığı http://myapps/style.css. Bu durumda, stil sayfası giderleri uygulama yüklenirken throw şekilde 404 hatası style.css yüklenmeye çalışılırken, uygulamanızda yayınlanmadı. Bu örnekte, bir iç URL ile uygulama yayımlama tarafından Sorun çözülene http://myapp/.

## <a name="problems-with-publishing-as-one-application"></a>Bir uygulama yayımlama sorunları

Aynı uygulama içindeki tüm kaynakların yayımlamak mümkün değilse, birden çok uygulama yayımlama ve bunları arasındaki bağlantıları etkinleştirmeniz gerekir.

Bunu yapmak için kullanmanızı öneririz [özel etki alanlarını](manage-apps/application-proxy-configure-custom-domain.md) çözümü. Ancak, bu çözüm, etki alanınız için sertifika sahibi ve uygulamalarınızı tam etki alanı adlarını (FQDN) kullanmak gerektirir. Diğer seçenekler için bkz: [bağlantıların belgelerine sorun giderme](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md)
