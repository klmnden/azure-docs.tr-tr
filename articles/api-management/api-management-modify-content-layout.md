---
title: API Management'ta geliştirici portalındaki sayfaların içeriğini değiştirme | Microsoft Docs
description: Azure API Management'ta geliştirici portalındaki sayfaların içeriğini düzenlemeyi öğrenin.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: vlvinogr
editor: ''
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/09/2017
ms.author: vlvinogr
ms.openlocfilehash: fd9902a83a813daffc52e089e6e8bfc4137c5b8f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60656644"
---
# <a name="modify-the-content-and-layout-of-pages-on-the-developer-portal-in-azure-api-management"></a>Azure API Management geliştirici portalındaki sayfaların içeriğini ve düzenini değiştirme
Azure API Management'ta geliştirici portalını özelleştirmek için kullanılabilecek üç temel yöntem vardır:

* [Statik sayfaların ve sayfa düzeni öğelerinin içeriğini düzenleme][modify-content-layout] (bu kılavuzda açıklanmıştır)
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme][portal-templates] (örneğin, API belgeleri, ürünler, kullanıcı kimlik doğrulaması vs.)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="page-structure"> </a>Geliştirici portalı sayfalarının yapısı

Geliştirici portalı bir içerik yönetimi sistemini temel alır. Her sayfanın düzeni, pencere öğesi olarak bilinen küçük sayfa öğelerinin bir araya getirilmesiyle oluşturulur:

![Geliştirici portalı sayfa yapısı][api-management-customization-widget-structure]

Tüm pencere öğeleri düzenlenebilir.
* Her bir sayfaya özgü temel içerik, "İçerik" pencere öğesinde yer alır. Bir sayfayı düzenlemek, bu pencere öğesinin içeriğini düzenleme anlamına gelir.
* Sayfa düzeniyle ilgili tüm öğeler diğer pencere öğelerinin içindedir. Bu widget’lara yapılan değişiklikler tüm sayfalara uygulanır. Bunlara "düzen widget’ları" denir.

Sayfaların her gün düzenlenmesi durumunda genelde yalnızca İçerik widget’ı değiştirilerek sayfalarda farklı içeriklerin yer alması sağlanır.

## <a name="modify-layout-widget"> </a>Bir düzen pencere öğesinin içeriğini değiştirme

Geliştirici portalı Azure Portal üzerinden erişilebilir.

1. API Management örneğinizin araç çubuğundan **Geliştirici Portalı**’na tıklayın.
2. Widget’ların içeriklerini düzenlemek için, sol taraftaki **Geliştirici** portalı menüsündeki iki boya fırçasından oluşan simgeyi tıklayın.
3. Üst bilginin içeriklerini değiştirmek için listede sol tarafta bulunan **Üst bilgi** bölümüne kaydırın.

    Widget’lar alanların içinden düzenlenebilir.
4. Değişikliklerinizi yayımlamaya hazır olduğunuzda sayfanın altında bulunan **Yayımla**’ya tıklayın.

Şimdi geliştirici portalındaki her sayfada yeni üst bilgiyi görüyor olmalısınız.

## <a name="next-steps"> </a>Sonraki adımlar
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme][portal-templates] (örneğin, API belgeleri, ürünler, kullanıcı kimlik doğrulaması vs.)

[Structure of developer portal pages]: #page-structure
[Modifying the contents of a layout widget]: #modify-layout-widget
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png
