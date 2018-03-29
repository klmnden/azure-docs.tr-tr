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
ms.openlocfilehash: d187b545a486be28fc80e6baf8e58079ff94ec5e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Uygulama sayfası için uygulama proxy'si uygulama düzgün görüntülenmez

Bu makalede sayfasına gidin, ancak sayfasında bir şey doğru görünmüyor Azure Active Directory Uygulama proxy'si uygulamalar ile ilgili sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış
Bir uygulama proxy'si uygulama yayımladığınızda, uygulama erişirken yalnızca, kök altındaki sayfalar daha erişilebilir. Sayfa doğru görüntülemiyorsa, uygulama için kullanılan kök İç URL bazı sayfası kaynakları eksik olabilir. Çözmek için yayımlanan emin olun *tüm* sayfa uygulamanızın parçası olarak için kaynaklar.

Bu sorun olmadığından, Ağ İzleyicisi'ni açarak doğrulayabilirsiniz (Fiddler veya F12 gibi araçları Internet Explorer/kenar), sayfa yüklenirken ve 404 hatalarını aranıyor. Bu, şu anda bulunamıyor ve yayımlanmasını gerekebilir sayfaları gösterir.

Bu durumda bir örnek olarak, bir iç URL'sini kullanarak giderleri uygulama yayımlandı varsayalım <http://myapps/expenses>, ancak stil uygulamanın kullandığı <http://myapps/style.css>. Bu durumda, stil sayfası giderleri uygulama yüklenirken throw şekilde 404 hatası style.css yüklenmeye çalışılırken, uygulamanızda yayınlanmadı. Bu örnekte sorunun bir iç URL'si ile uygulama yayımlama tarafından çözülmüş <http://myapp/> yerine.

## <a name="problems-with-publishing-as-one-application"></a>Bir uygulama yayımlama sorunları

Aynı uygulama içindeki tüm kaynakların yayımlamak mümkün değilse, birden çok uygulama yayımlama ve bunları arasındaki bağlantıları etkinleştirmeniz gerekir.

Bunu yapmak için kullanmanızı öneririz [özel etki alanlarını](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) çözümü. Ancak, bu çözüm, etki alanınız için sertifika sahibi ve uygulamalarınızı tam etki alanı adlarını (FQDN) kullanmak gerektirir. Diğer seçenekler için bkz: [bağlantıların belgelerine sorun giderme](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md)
