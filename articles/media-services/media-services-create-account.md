<properties
    pageTitle="Media Services hesabı oluşturma | Microsoft Azure"
    description="Azure’da yeni bir Azure Media Services hesabı oluşturmayı açıklar."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/22/2016"
    ms.author="juliako"/>


# Azure Media Services hesabı oluşturma

> [AZURE.SELECTOR]
- [Portal](media-services-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)


> [AZURE.NOTE] Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](/pricing/free-trial/?WT.mc_id=A261C142F).
 
Klasik Azure Portalı bir Azure Media Services hesabını hızlıca oluşturmanın yolunu sağlar. Azure’da medya içeriği depolamanıza, şifrelemenize, kodlamanıza, yönetmenize ve akış yapmanıza imkan tanıyan Media Services’e erişmek için hesabınızı kullanabilirsiniz. Bir Media Services hesabı oluşturduğunuzda Media Services hesabı ile aynı coğrafi bölgede ilişkili bir depolama hesabı da oluşturursunuz (ya da var olanı kullanırsınız).

Bu makalede yeni bir Media Services hesabı oluşturmak ve sonra bu hesabı bir depolama hesabıyla ilişkilendirmek için Hızlı Oluşturma yönteminin nasıl kullanılacağı açıklanmaktadır.

<a id="concepts"></a>
## Kavramlar

Media Services’e erişim iki ilişkili hesap gerektirir:

-   **Bir Media Services hesabı**. Hesabınız Azure’da mevcut olan bir dizi bulut tabanlı Media Services hizmetine erişmenizi sağlar. Bir Media Services hesabı gerçek medya verilerini depolamaz. Bunun yerine, hesabınızdaki medya içeriği ve medya işleme işleri hakkındaki meta verileri depolar. Hesabı oluşturduğunuz sırada mevcut Media Services bölgelerinden birini seçin. Seçtiğiniz bölge hesabınız için meta veri kayıtlarını saklayan veri merkezidir.

    Mevcut Media Services (AMS) bölgeleri şunlardır: Kuzey Avrupa, Batı Avrupa, Batı ABD, Doğu ABD, Güneydoğu Asya, Doğu Asya, Japonya Batı, Japonya Doğu. Media Services benzeşim gruplarını kullanmaz.
    
    AMS bundan böyle şu veri merkezlerinde de mevcuttur: Brezilya Güney, Hindistan Batı, Hindistan Güney ve Hindistan Orta. Klasik Azure Portalı’nı bundan böyle [Media Service hesapları oluşturmak](media-services-create-account.md#create-a-media-services-account-using-quick-create) ve [burada](https://azure.microsoft.com/documentation/services/media-services/) açıklanan çeşitli görevleri gerçekleştirmek için kullanabilirsiniz. Ancak, Live Encoding bu veri merkezlerinde etkin değildir. Ayrıca, bu veri merkezlerinde Kodlamaya Ayrılan Birimlerin tüm türleri kullanılabilir değildir.
    
    - Brezilya Güney:                                          Yalnızca Standart ve Temel Kodlamaya Ayrılan Birimler kullanılabilir
    - Hindistan Batı, Hindistan Güney ve Hindistan Orta:             Yalnızca Temel Kodlamaya Ayrılan Birimler kullanılabilir


-   **İlişkili depolama hesabı**. Storage hesabınız Media Services hesabınızla ilişkili olan bir Azure depolama hesabıdır. Storage hesabı, medya dosyaları için blob depolama alanı sağlar ve Media Services hesabıyla aynı coğrafi bölgede olmalıdır. Bir Media Services hesabı oluşturduğunuzda aynı bölgede var olan bir depolama hesabını seçebilir veya aynı bölgede yeni bir depolama hesabı oluşturabilirsiniz. Bir Media Services hesabını silerseniz ilişkili depolama hesabınızdaki blob’lar silinmez.

<a id="quick"></a>
## Hızlı Oluşturma yöntemini kullanarak Media Services hesabı oluşturma

1. [Klasik Azure Portalı][]’nda **Yeni**, **Medya Hizmeti** ve sonra **Hızlı Oluştur**’a tıklayın.

![Media Services Hızlı Oluştur](./media/media-services-create-account/wams-QuickCreate.png)

2. **AD** alanına yeni hesabın adını girin. Media Services hesabı adı, boşluk olmadan, tümü küçük harf ve sayılardan oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.

3. **BÖLGE** alanında Media Services hesabınıza ait meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Açılan listede yalnızca kullanılabilir Media Services bölgeleri görüntülenir.

4. **DEPOLAMA HESABI** alanında, Media Services hesabınızdan gelen medya içeriğine blob depolama sağlamak üzere bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur.

5. Yeni bir depolama hesabı oluşturduysanız **YENİ DEPOLAMA HESABI ADI** alanına depolama hesabı için bir ad girin. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.

6. Formun altındaki **Hızlı Oluştur**’a tıklayın.

İşlemin durumunu pencerenin altındaki ileti alanında izleyebilirsiniz.

Hesap başarıyla oluşturulduğunda durum Etkin olarak değişir. Yeni hesabı gösteren **media services** sayfası açılır.

Sayfanın altında **ANAHTARLARI YÖNET** düğmesi görünür. Bu düğmeye tıkladığınızda Media Services hesap adını, birincil ve ikincil anahtarları içeren bir sayfa görüntülenir. Media Services hesabına programlı olarak erişmek için hesap adına ve birincil anahtar bilgilerine ihtiyacınız vardır.

![Media Services Sayfası](./media/media-services-create-account/wams-mediaservices-page.png)

Hesap adına çift tıkladığınızda varsayılan olarak **Hızlı Başlangıç** sayfası görüntülenir. Bu sayfada portalın diğer sayfalarında da bulunan bazı yönetim görevleri gerçekleştirilebilir. Örneğin, bir video dosyasını bu sayfadan veya **İÇERİK** sayfasından yükleyebilirsiniz.

Ayrıca, videoları karşıya yükleme, kodlama ve yayımlama görevlerini gerçekleştirmek üzere Azure Media Services SDK’sını kullanan kodu görüntüleyebilirsiniz. **KOD YAZIN** bölümü altındaki bağlantılardan birine tıklayabilir, kodu kopyalayabilir ve uygulamanızda kullanabilirsiniz.



##Media Services öğrenme yolları

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## Sonraki adımlar

- [.NET SDK kullanarak İsteğe Bağlı Video (VoD) iletmeye başlama](media-services-dotnet-get-started.md)

- [Tekli bit hızından çoklu bit hızı akışına kadar gerçek zamanlı kodlama gerçekleştiren kanallar oluşturmak için .NET SDK’sını kullanın](media-services-dotnet-creating-live-encoder-enabled-channel.md)

<!-- Reusable paths. -->

<!-- Anchors. -->
  [Kavramlar]: #concepts
  [Başlamadan önce]: #begin
  [Nasıl yapılır: Hızlı Oluşturma yöntemini kullanarak Media Services hesabı oluşturma]: #quick

<!-- URLs. -->
  [Web Platformu Yükleyicisi]: http://go.microsoft.com/fwlink/?linkid=255386

  [Klasik Azure Portalı]: http://manage.windowsazure.com/



<!--HONumber=Aug16_HO1-->


