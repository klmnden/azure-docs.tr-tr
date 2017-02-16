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
ms.sourcegitcommit: 30ec6f45da114b6c7bc081f8a2df46f037de61fd
ms.openlocfilehash: cbd2c3e915b93340c1a1478c09b23480c4565a98


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
Bir sayfadaki renk, yazı tipi, boyut, boşluklar ve stille ilgili diğer öğeler stil kuralları tarafından tanımlanır. Stilleri düzenlemek için, yayımcı portalında **Geliştirici portalı** menüsünde **Görünüm**’e tıklayın ve ardından stil düzenleyicisini etkinleştirmek için **Özelleştirmeye başla**’ya tıklayın.

Tarayıcınız, geliştirici portalında, sitenin herhangi bir yerinde kullanılan tüm stil kurallarının örnekleriyle içerik örneklerini içeren gizli bir sayfaya geçiş yapar. Stil düzenleyicisini açmak için, imleci sayfanın en solundaki ince gri dikey çizginin üzerine getirin. Düzenleyici araç çubuğu görüntülenmelidir.

![Özelleştirme araç çubuğu][api-management-customization-toolbar]

Stil kurallarını düzenlemenin iki ana modu vardır: **Tüm kuralları düzenle** herhangi bir yerde kullanılan tüm stil kurallarının bir listesini gösterirken, **Öğe seç** bulunduğunuz sayfadan bir öğeyi seçmenize olanak sağlar ve yalnızca o öğeye ilişkin stilleri gösterir.

Bu bölümde, yalnızca üst bilgilerin stilini değiştirmek istiyoruz. Stil düzenleyicisi araç çubuğunda **Öğe seç**’e ve ardından **Özelleştirilecek öğeyi seçin**’e tıklayın. Fareyle üzerine geldiğiniz öğeler vurgulanır ve tıkladığınızda hangi öğe stillerini düzenlemeye başlayacağınızı gösterir. Fareyi üst bilgide şirket adını gösteren metnin üzerine getirin (önceki bölümde yer alan yönergeleri izlediyseniz "Fabrikam Geliştirici Portalı") ve metni tıklayın. Stil düzenleyicisinde, adlandırılmış ve kategorilere ayrılmış bir grup stil kuralı görüntülenir.

Her kural seçilen öğenin bir stil özelliğini temsil eder. Örneğin, yukarıda seçilen üst bilgi metni için metin boyutu @font-size-h1 biçimindeyken, alternatiflerle birlikte yazı tipinin adı @headings-font-family. biçimindedir

> [Önyükleme][bootstrap] yapmayı biliyorsanız, bu kurallar aslında geliştirici portalı tarafından kullanılan önyükleme temasının [LESS değişkenleridir][LESS variables].
> 
> 

Şimdi üst bilgi metninin rengini değiştirelim. **@headings-color** alanındaki girişi seçin ve **#000000** yazın. Bu, siyah renk için onaltılı koddur. Bunu yaparken, metin kutusunun sonunda kare şeklinde bir renk göstergesi görürsünüz. Bu göstergeye tıklarsanız, bir renk seçici renk seçmenizi sağlar.

![Renk seçici][api-management-customization-toolbar-color-picker]

Seçili öğenin stillerindeki değişiklikleri tamamladığınızda **Değişiklikleri Önizle**’ye tıklayarak sonuçları ekranda görün. Şu anda bunlar yalnızca yöneticiler tarafından görülebilir. Bu değişikliği herkese görünür yapmak için stil düzenleyicisinde **Yayımla** düğmesine tıklayın ve değişiklikleri onaylayın.

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
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/



<!--HONumber=Dec16_HO3-->


