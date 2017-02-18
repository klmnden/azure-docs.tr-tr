---
title: "API Management’ta geliştirici portalını özelleştirme | Microsoft Belgeleri"
description: "Azure API Management’ta geliştirici portalını özelleştirmeyi öğrenin."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 13431648e87d085161ad278dc991d49f7872be34
ms.openlocfilehash: 60213f885020a5ba36d6ada0812f755e06b3c48b


---
# <a name="customize-the-developer-portal-in-azure-api-management"></a>Azure API Management’ta geliştirici portalını özelleştirme
Bu kılavuz, markanızla tutarlılık için Azure API Management’ta geliştirici portalının görünümünü nasıl değiştireceğinizi gösterir.

## <a name="change-page-headers"> </a>Sayfa üst bilgisindeki metin veya logoyu değiştirme
Portal özelleştirmenin temel özelliklerinden biri tüm sayfaların üst kısmındaki metni şirketinizin adı veya logosu ile değiştirmektir.

Geliştirici portalındaki içerik, Azure Portal ile erişilen yayımcı portalı aracılığıyla değiştirilir. Ulaşmak için API Management örneğinizin hizmet araç çubuğundan **Yayımcı portalı**’na tıklayın.

![Yayımcı portalı][api-management-management-console]

Geliştirici portalı bir içerik yönetimi sistemini veya CMS’yi temel alır. Her sayfada görünen üst bilgi, pencere öğesi olarak bilinen özel bir içerik türüdür. Bu pencere öğesinin içeriğini düzenlemek için sol kısımdaki **Geliştirici Portalı** menüsünde **Pencere Öğeleri**’ne tıklayın ve ardından listede **Üst Bilgi** pencere öğesini seçin.

![Pencere öğeleri üst bilgisi][api-management-widgets-header]

Üst bilgi içeriği **Gövde** alanında düzenlenebilir. Metni "Fabrikam Geliştirici Portalı" olarak değiştirin ve ardından sayfanın alt kısmında **Kaydet**’e tıklayın.

Şimdi geliştirici portalındaki her sayfada yeni üst bilgiyi görüyor olmalısınız.

> Geliştirici portalındayken yayımcı portalını açmak için, üst kısımdaki çubukta **Geliştirici portalı**’na tıklayın.
> 
> 

## <a name="change-headers-styling"> </a>Üst bilgilerin stilini değiştirme
Bir sayfadaki renk, yazı tipi, boyut, boşluklar ve stille ilgili diğer öğeler stil kuralları tarafından tanımlanır. Stilleri düzenlemek için, **Geliştirici portalında** özelleştirme simgesinin üzerine gelip soldaki özelleştirme araç çubuğunu açın ve araç çubuğundan "stilleri" seçin.

![Özelleştirme araç çubuğu düğmesi][api-management-customization-toolbar-button]

Stil kurallarını iki şekilde düzenleyebilirsiniz: varsayılan olarak gösterilen ve her yerde kullanılan tüm stil kurallarının listesine bakabilir veya **Sayfadaki bir öğeyi seçin** öğesini seçip sayfadaki herhangi bir yere tıklayarak yalnızca ilgili öğenin stillerini görebilirsiniz.

Bu bölümde, yalnızca üst bilgilerin stilini değiştirmek istiyoruz. Stil düzenleyici araç çubuğundan **Sayfadaki bir öğeyi seçin** seçeneğine tıklayın. 

![Özelleştirme araç çubuğu][api-management-customization-toolbar]

Fareyle üzerine geldiğiniz öğeler vurgulanır ve tıkladığınızda hangi öğe stillerini düzenlemeye başlayacağınızı gösterir. Fareyi üst bilgide şirket adını gösteren metnin üzerine getirin (önceki bölümde yer alan yönergeleri izlediyseniz "Fabrikam Geliştirici Portalı") ve metni tıklayın. Stil düzenleyicisinde, adlandırılmış ve kategorilere ayrılmış bir grup stil kuralı görüntülenir. Her kural seçilen öğenin bir stil özelliğini temsil eder. Örneğin, yukarıda seçilen üst bilgi metni için metin boyutu @font-size-h1 biçimindeyken, alternatiflerle birlikte yazı tipinin adı @headings-font-family biçimindedir.

> [Önyükleme][bootstrap] yapmayı biliyorsanız, bu kurallar aslında geliştirici portalı tarafından kullanılan önyükleme temasının [LESS değişkenleridir][LESS variables].
> 
> 

Şimdi üst bilgi metninin rengini değiştirelim. **@headings-color** alanındaki girişi seçin ve **#000000** yazın. Bu, siyah renk için onaltılı koddur. Bunu yaparken, metin kutusunun sonunda kare şeklinde bir renk göstergesi görürsünüz. Bu göstergeye tıklarsanız, bir renk seçici renk seçmenizi sağlar.

![Renk seçici][api-management-customization-toolbar-color-picker]

Değişiklikler yapıldıkça gerçek zamanlı olarak önizlemesi gösterilir, ancak yalnızca yöneticilere görünür. Bu değişikliği herkese görünür yapmak için stil düzenleyicisinde **Yayımla** düğmesine tıklayın ve değişiklikleri onaylayın.

![Yayımlama menüsü][api-management-customization-toolbar-publish-form]

> Sayfadaki başka bir öğeye uygulanan stil kurallarını değiştirmek isterseniz üst bilgi için uyguladığınız aynı yordamı izleyin. Stil düzenleyicisinde **Bir öğe seçin**’e tıklayın, ilgilendiğiniz öğeyi seçin ve ekranda görünen stil kurallarının değerlerini değiştirmeye başlayın.
> 
> 

## <a name="edit-page-contents"> </a>Bir sayfanın içeriğini düzenleme
Geliştirici portalı API'ler, Ürünler, Uygulamalar, Sorunlar gibi otomatik oluşturulan sayfalar ve el ile yazılmış içerikten oluşur. Bir içerik yönetimi sistemini temel aldığından, bu tür içeriği gerektiği gibi oluşturabilirsiniz.

Mevcut tüm içerik sayfalarının listesini görmek için, yayımcı portalındaki **Geliştirici portalı** menüsünde **İçerik** seçeneğine tıklayın.

![İçeriği yönetme][api-management-customization-manage-content]

Geliştirici portalının giriş sayfasında ne göründüğünü düzenlemek için **Hoş Geldiniz** sayfasına tıklayın. İstediğiniz değişiklikleri yapın, gerekirse önizleme yapın ve ardından herkese görünür hale getirmek için **Şimdi Yayımla**’ya tıklayın.

> Giriş sayfası, üst kısımda bir başlık göstermesine izin veren özel bir düzen kullanır. Bu başlık **İçerik** bölümünden düzenlenemez. Bu başlığı düzenlemek için, **Geliştirici Portalı** menüsünde **Pencere Öğeleri**’ne tıklayın **Geçerli Katman** açılır listesinde **Giriş sayfası**’nı seçin ve ardından **Öne çıkan bölüm** altında **Başlık** seçeneğine tıklayın. Bu pencere öğesi içeriği diğer sayfalar gibi düzenlenebilir.
> 
> 

## <a name="next-steps"> </a>Sonraki adımlar
* [Geliştirici portal şablonları](api-management-developer-portal-templates.md)’nı kullanarak geliştirici portal sayfalarının içeriğini özelleştirme hakkında bilgi edinin.

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-portal/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/



<!--HONumber=Feb17_HO1-->


