---
title: "Azure API Management&quot;ta geliştirici portalı stillerini özelleştirme | Microsoft Docs"
description: "Azure API Management geliştirici portalı sayfalarında kullanılan stilleri nasıl değiştireceğinizi öğrenin."
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
translationtype: Human Translation
ms.sourcegitcommit: 336d4f80f0357796fb29eb9314c11edfce831a69
ms.openlocfilehash: bd08eb476a4bd7298c5650977b88ba0b24deddec
ms.lasthandoff: 02/23/2017


---
# <a name="customize-the-styling-of-the-developer-portal-in-azure-api-management"></a>Azure API Management'ta geliştirici portalı stillerini özelleştirme
Azure API Management'ta geliştirici portalını özelleştirmek için kullanılabilecek üç temel yöntem vardır:

* [Statik sayfaların ve sayfa düzeni öğelerinin içeriğini düzenleme][modify-content-layout]
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles] (bu kılavuzda açıklanmıştır)
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme][portal-templates] (API belgeleri, ürünler, kullanıcı kimlik doğrulaması vs.)

## <a name="change-headers-styling"> </a>Sayfa öğelerinin stilini değiştirme

Bir sayfadaki renk, yazı tipi, boyut, boşluklar ve stille ilgili diğer öğeler stil kuralları tarafından tanımlanır. 

Stil kurallarını düzenlemek için yönetici olarak oturum açtıktan sonra **Geliştirici portalına** gidin. Bu sayfaya ulaşmak için Azure Portal'ı açın ve API Management örneğinizin hizmet araç çubuğundan **Yayımcı portalı**'na tıklayın.

![Yayımcı portalı][api-management-management-console]

Ardından sağ üst köşedeki **Geliştirici portalı**'na tıklayın. 

![Yayımcı portalındaki geliştirici portalı bağlantısı][api-management-pp-dp-link]

Özelleştirme araç çubuğunu açmak için farenizi özelleştirme simgesinin üzerine götürün (veya simgeyi seçin) ve araç çubuğundan "stiller"e tıklayın.

![Özelleştirme araç çubuğu düğmesi][api-management-customization-toolbar-button]

Stil kurallarını iki şekilde düzenleyebilirsiniz: varsayılan olarak gösterilen ve her yerde kullanılan tüm stil kurallarının listesine bakabilir veya **Sayfadaki bir öğeyi seçin** öğesini seçip sayfadaki herhangi bir yere tıklayarak yalnızca ilgili öğenin stillerini görebilirsiniz.

![Özelleştirme araç çubuğu][api-management-customization-toolbar]

Bu örnek için **Sayfadaki bir öğeyi seçin**'e tıklayın.  Fareyle üzerine geldiğiniz öğeler vurgulanır ve tıkladığınızda hangi öğe stillerini düzenlemeye başlayacağınızı gösterir. Fareyi üst bilgideki metnin üzerine götürün (genelde burada şirket adı yazar) ve tıklayın. Stil düzenleyicisinde, adlandırılmış ve kategorilere ayrılmış bir grup stil kuralı görüntülenir. Her kural seçilen öğenin bir stil özelliğini temsil eder. Örneğin, yukarıda seçilen üst bilgi metni için metin boyutu @font-size-h1 biçimindeyken, alternatiflerle birlikte yazı tipinin adı @headings-font-family biçimindedir.

> [Önyükleme][bootstrap] yapmayı biliyorsanız, bu kurallar aslında geliştirici portalı tarafından kullanılan önyükleme temasının [LESS değişkenleridir][LESS variables].
> 
> 

Şimdi üst bilgi metninin rengini değiştirelim. **@headings-color** alanındaki girişi seçin ve **#000000** yazın. Bu, siyah renk için onaltılı koddur. Bunu yaparken, metin kutusunun sonunda kare şeklinde bir renk göstergesi görürsünüz. Bu göstergeye tıklarsanız, bir renk seçici renk seçmenizi sağlar.

![Renk seçici][api-management-customization-toolbar-color-picker]

Değişiklikler yapıldıkça gerçek zamanlı olarak önizlemesi gösterilir, ancak yalnızca yöneticilere görünür. Bu değişikliği herkese görünür yapmak için stil düzenleyicisinde **Yayımla** düğmesine tıklayın ve değişiklikleri onaylayın.

![Yayımlama menüsü][api-management-customization-toolbar-publish-form]

> Sayfadaki başka bir öğeye uygulanan stil kurallarını değiştirmek isterseniz üst bilgi için uyguladığınız aynı yordamı izleyin. Stil düzenleyicisinde **Sayfadaki bir öğeyi seçin**'e tıklayın, ilgilendiğiniz öğeyi seçin ve ekranda görünen stil kurallarının değerlerini değiştirmeye başlayın.
> 
> 


## <a name="next-steps"> </a>Sonraki adımlar
* [Geliştirici portal şablonları](api-management-developer-portal-templates.md)’nı kullanarak geliştirici portal sayfalarının içeriğini özelleştirme hakkında bilgi edinin.

[Change the styling of the headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/

