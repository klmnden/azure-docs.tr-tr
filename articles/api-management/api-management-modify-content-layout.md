---
title: "API Management'ta geliştirici portalındaki sayfaların içeriğini değiştirme | Microsoft Docs"
description: "Azure API Management'ta geliştirici portalındaki sayfaların içeriğini düzenlemeyi öğrenin."
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: 708c803c36c182ed90e04731b12d4ade00ae7ffb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="modify-the-content-and-layout-of-pages-on-the-developer-portal-in-azure-api-management"></a>Azure API Management geliştirici portalındaki sayfaların içeriğini ve düzenini değiştirme
Azure API Management'ta geliştirici portalını özelleştirmek için kullanılabilecek üç temel yöntem vardır:

* [Statik sayfaların ve sayfa düzeni öğelerinin içeriğini düzenleme][modify-content-layout] (bu kılavuzda açıklanmıştır)
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme][portal-templates] (API belgeleri, ürünler, kullanıcı kimlik doğrulaması vs.)

## <a name="page-structure"> </a>Geliştirici portalı sayfalarının yapısı

Geliştirici portalı bir içerik yönetimi sistemini temel alır. Her sayfanın düzeni, pencere öğesi olarak bilinen küçük sayfa öğelerinin bir araya getirilmesiyle oluşturulur:

![Geliştirici portalı sayfa yapısı][api-management-customization-widget-structure]

Tüm pencere öğeleri düzenlenebilir. 
* Her bir sayfaya özgü temel içerik, "İçerik" pencere öğesinde yer alır. Bir sayfayı düzenlemek, bu pencere öğesinin içeriğini düzenleme anlamına gelir.
* Sayfa düzeniyle ilgili tüm öğeler diğer pencere öğelerinin içindedir. Bu pencere öğelerinde yapılan değişiklikler tüm sayfalara uygulanır. Bunlara "düzen pencere öğeleri" denir.

Sayfaların her gün düzenlenmesi durumunda genelde yalnızca İçerik pencere öğesi değiştirilerek sayfalarda farklı içeriklerin yer alması sağlanır.

## <a name="modify-layout-widget"> </a>Bir düzen pencere öğesinin içeriğini değiştirme

Geliştirici portalındaki içerik, Azure Portal ile erişilen yayımcı portalı aracılığıyla değiştirilir. Ulaşmak için API Management örneğinizin hizmet araç çubuğundan **Yayımcı portalı**’na tıklayın.

![Yayımcı portalı][api-management-management-console]

Bu pencere öğesinin içeriğini düzenlemek için sol kısımdaki **Geliştirici Portalı** menüsünde **Pencere Öğeleri**'ne tıklayın. Bu örnekte Üst Bilgi pencere öğesinin içeriğini değiştireceğiz. Listeden **Üst Bilgi** pencere öğesini seçin.

![Pencere öğeleri üst bilgisi][api-management-widgets-header]

Üst bilgi içeriği **Gövde** alanında düzenlenebilir. Metni istediğiniz şekilde değiştirin ve ardından sayfanın alt kısmında **Kaydet**'e tıklayın.

Şimdi geliştirici portalındaki her sayfada yeni üst bilgiyi görüyor olmalısınız.

> Geliştirici portalındayken yayımcı portalını açmak için, üst kısımdaki çubukta **Geliştirici portalı**’na tıklayın.
> 
> 

## <a name="edit-page-contents"> </a>Bir sayfanın içeriğini düzenleme

Mevcut tüm içerik sayfalarının listesini görmek için, yayımcı portalındaki **Geliştirici portalı** menüsünde **İçerik** seçeneğine tıklayın.

![İçeriği yönetme][api-management-customization-manage-content]

Geliştirici portalının giriş sayfasında ne göründüğünü düzenlemek için **Hoş Geldiniz** sayfasına tıklayın. İstediğiniz değişiklikleri yapın, gerekirse önizleme yapın ve ardından herkese görünür hale getirmek için **Şimdi Yayımla**’ya tıklayın.

> Giriş sayfası, üst kısımda bir başlık göstermesine izin veren özel bir düzen kullanır. Bu başlık **İçerik** bölümünden düzenlenemez. Bu başlığı düzenlemek için, **Geliştirici Portalı** menüsünde **Pencere Öğeleri**’ne tıklayın **Geçerli Katman** açılır listesinde **Giriş sayfası**’nı seçin ve ardından **Öne çıkan bölüm** altında **Başlık** seçeneğine tıklayın. Bu pencere öğesi içeriği diğer sayfalar gibi düzenlenebilir.
> 
> 

## <a name="next-steps"> </a>Sonraki adımlar
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme][portal-templates] (API belgeleri, ürünler, kullanıcı kimlik doğrulaması vs.)

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
