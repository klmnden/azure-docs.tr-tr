<properties
    pageTitle="API Management’ta geliştirici portalını özelleştirme | Microsoft Azure"
    description="Azure API Management’ta geliştirici portalını özelleştirmeyi öğrenin."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="sdanie"/>

# Azure API Management’ta geliştirici portalını özelleştirme

Bu kılavuz, markanızla tutarlılık için Azure API Management’ta geliştirici portalının görünümünü nasıl değiştireceğinizi gösterir.

## <a name="change-page-headers"> </a>Sayfa üst bilgisindeki metin veya logoyu değiştirme

Portal özelleştirmenin temel özelliklerinden biri tüm sayfaların üst kısmındaki metni şirketinizin adı veya logosu ile değiştirmektir.

Geliştirici portalındaki içerik, Klasik Azure Portalı ile erişilen yayımcı portalı aracılığıyla değiştirilir. API yayımcı portalına erişmek isterseniz API Management hizmetiniz için Klasik Azure Portalı'nda **Yönet**’e tıklayın.

![Yayımcı portalı][api-management-management-console]

Geliştirici portalı bir içerik yönetimi sistemini veya CMS’yi temel alır. Her sayfada görünen üst bilgi, pencere öğesi olarak bilinen özel bir içerik türüdür. Bu pencere öğesinin içeriğini düzenlemek için sol kısımdaki **Geliştirici Portalı** menüsünde **Pencere Öğeleri**’ne tıklayın ve ardından listede **Üst Bilgi** pencere öğesini seçin.

![Pencere öğeleri üst bilgisi][api-management-widgets-header]

Üst bilgi içeriği **Gövde** alanında düzenlenebilir. Metni "Fabrikam Geliştirici Portalı" olarak değiştirin ve ardından sayfanın alt kısmında **Kaydet**’e tıklayın.

Şimdi geliştirici portalındaki her sayfada yeni üst bilgiyi görüyor olmalısınız.

> Geliştirici portalındayken yayımcı portalını açmak için, üst kısımdaki çubukta **Geliştirici portalı**’na tıklayın.

## <a name="change-headers-styling"> </a>Üst bilgilerin stilini değiştirme

Bir sayfadaki renk, yazı tipi, boyut, boşluklar ve stille ilgili diğer öğeler stil kuralları tarafından tanımlanır. Stilleri düzenlemek için, yayımcı portalında **Geliştirici portalı** menüsünde **Görünüm**’e tıklayın ve ardından stil düzenleyicisini etkinleştirmek için **Özelleştirmeye başla**’ya tıklayın.

Tarayıcınız, geliştirici portalında, sitenin herhangi bir yerinde kullanılan tüm stil kurallarının örnekleriyle içerik örneklerini içeren gizli bir sayfaya geçiş yapar. Stil düzenleyicisini açmak için, imleci sayfanın en solundaki ince gri dikey çizginin üzerine getirin. Düzenleyici araç çubuğu görüntülenmelidir.

![Özelleştirme araç çubuğu][api-management-customization-toolbar]

Stil kurallarını düzenlemenin iki ana modu vardır: **Tüm kuralları düzenle** herhangi bir yerde kullanılan tüm stil kurallarının bir listesini gösterirken, **Öğe seç** bulunduğunuz sayfadan bir öğeyi seçmenize olanak sağlar ve yalnızca o öğeye ilişkin stilleri gösterir.

Bu bölümde, yalnızca üst bilgilerin stilini değiştirmek istiyoruz. Stil düzenleyicisi araç çubuğunda **Öğe seç**’e ve ardından **Özelleştirilecek öğeyi seçin**’e tıklayın. Fareyle üzerine geldiğiniz öğeler vurgulanır ve tıkladığınızda hangi öğe stillerini düzenlemeye başlayacağınızı gösterir. Fareyi üst bilgide şirket adını gösteren metnin üzerine getirin (önceki bölümde yer alan yönergeleri izlediyseniz "Fabrikam Geliştirici Portalı") ve metni tıklayın. Stil düzenleyicisinde, adlandırılmış ve kategorilere ayrılmış bir grup stil kuralı görüntülenir.

Her kural seçilen öğenin bir stil özelliğini temsil eder. Örneğin, yukarıda seçilen üst bilgi metni için, metin boyutu @font-size-h1 içinde iken, alternatiflerle birlikte yazı tipi adı @headings-font-family içindedir.

> [Önyükleme][] yapmayı biliyorsanız, bu kurallar aslında geliştirici portalı tarafından kullanılan önyükleme temasının [LESS değişkenleridir][].

Şimdi üst bilgi metninin rengini değiştirelim. **@headings-color** alanını seçin ve **#000000** yazın. Bu, siyah renk için onaltılı koddur. Bunu yaparken, metin kutusunun sonunda kare şeklinde bir renk göstergesi görürsünüz. Bu göstergeye tıklarsanız, bir renk seçici renk seçmenizi sağlar.

![Renk seçici][api-management-customization-toolbar-color-picker]

Seçili öğenin stillerindeki değişiklikleri tamamladığınızda **Değişiklikleri Önizle**’ye tıklayarak sonuçları ekranda görün. Şu anda bunlar yalnızca yöneticiler tarafından görülebilir. Bu değişikliği herkese görünür yapmak için stil düzenleyicisinde **Yayımla** düğmesine tıklayın ve değişiklikleri onaylayın.

![Yayımlama menüsü][api-management-customization-toolbar-publish-form]

> Sayfadaki başka bir öğeye uygulanan stil kurallarını değiştirmek isterseniz üst bilgi için uyguladığınız aynı yordamı izleyin. Stil düzenleyicisinde **Bir öğe seçin**’e tıklayın, ilgilendiğiniz öğeyi seçin ve ekranda görünen stil kurallarının değerlerini değiştirmeye başlayın.

## <a name="edit-page-contents"> </a>Bir sayfanın içeriğini düzenleme

Geliştirici portalı API'ler, Ürünler, Uygulamalar, Sorunlar gibi otomatik oluşturulan sayfalar ve el ile yazılmış içerikten oluşur. Bir içerik yönetimi sistemini temel aldığından, bu tür içeriği gerektiği gibi oluşturabilirsiniz.

Mevcut tüm içerik sayfalarının listesini görmek için, yayımcı portalındaki **Geliştirici portalı** menüsünde **İçerik** seçeneğine tıklayın.

![İçeriği yönetme][api-management-customization-manage-content]

Geliştirici portalının giriş sayfasında ne göründüğünü düzenlemek için **Hoş Geldiniz** sayfasına tıklayın. İstediğiniz değişiklikleri yapın, gerekirse önizleme yapın ve ardından herkese görünür hale getirmek için **Şimdi Yayımla**’ya tıklayın.

> Giriş sayfası, üst kısımda bir başlık göstermesine izin veren özel bir düzen kullanır. Bu başlık **İçerik** bölümünden düzenlenemez. Bu başlığı düzenlemek için, **Geliştirici Portalı** menüsünde **Pencere Öğeleri**’ne tıklayın **Geçerli Katman** açılır listesinde **Giriş sayfası**’nı seçin ve ardından **Öne çıkan bölüm** altında **Başlık** seçeneğine tıklayın. Bu pencere öğesi içeriği diğer sayfalar gibi düzenlenebilir.

## <a name="next-steps"> </a>Sonraki adımlar

-   [Gelişmiş API yapılandırmasını kullanmaya başlama][] öğreticisindeki diğer konulara göz atın.

[Sayfa üst bilgisindeki metni/logoyu değiştirme]: #change-page-headers
[Üst bilgilerin stilini değiştirme]: #change-headers-styling
[Bir sayfanın içeriğini düzenleme]: #edit-page-contents
[Sonraki adımlar]: #next-steps

[Klasik Azure Portalı]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[Gelişmiş API yapılandırmasını kullanmaya başlama]: api-management-get-started-advanced.md
[Önyükleme]: http://getbootstrap.com/
[LESS değişkenleri]: http://getbootstrap.com/css/



<!--HONumber=Jun16_HO2-->


