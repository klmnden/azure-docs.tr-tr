<properties
    pageTitle=" Azure portalı ile Azure Media Services hesabı oluşturma | Microsoft Azure"
    description="Bu öğretici, Azure portalıyla bir Azure Media Services hesabı oluşturma adımlarında size kılavuzluk eder."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# Azure portalı ile Azure Media Services hesabı oluşturma

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 

Azure portalı bir Azure Media Services (AMS) hesabını hızlıca oluşturmanın yolunu sağlar. Azure’da medya içeriği depolamanıza, şifrelemenize, kodlamanıza, yönetmenize ve akış yapmanıza imkan tanıyan Media Services’e erişmek için hesabınızı kullanabilirsiniz. Bir Media Services hesabı oluşturduğunuzda Media Services hesabı ile aynı coğrafi bölgede ilişkili bir depolama hesabı da oluşturursunuz (ya da var olanı kullanırsınız).

Bu makalede bazı genel kavramlar ve Azure portalı ile Media Services hesabı oluşturma işlemi açıklanmaktadır.

## Kavramlar

Media Services’e erişim iki ilişkili hesap gerektirir:

- Bir Media Services hesabı. Hesabınız Azure’da mevcut olan bir dizi bulut tabanlı Media Services hizmetine erişmenizi sağlar. Bir Media Services hesabı gerçek medya verilerini depolamaz. Bunun yerine, hesabınızdaki medya içeriği ve medya işleme işleri hakkındaki meta verileri depolar. Hesabı oluşturduğunuz sırada mevcut Media Services bölgelerinden birini seçin. Seçtiğiniz bölge hesabınız için meta veri kayıtlarını saklayan veri merkezidir.

    Mevcut Media Services (AMS) bölgeleri şunlardır: Kuzey Avrupa, Batı Avrupa, Batı ABD, Doğu ABD, Güneydoğu Asya, Doğu Asya, Japonya Batı, Japonya Doğu. Media Services benzeşim gruplarını kullanmaz.
    
    AMS bundan böyle şu veri merkezlerinde de mevcuttur: Brezilya Güney, Hindistan Batı, Hindistan Güney ve Hindistan Orta. Azure portalını bundan böyle Media Service hesapları oluşturmak ve burada açıklanan çeşitli görevleri gerçekleştirmek için kullanabilirsiniz. Ancak, Live Encoding bu veri merkezlerinde etkin değildir. Ayrıca, bu veri merkezlerinde Kodlamaya Ayrılan Birimlerin tüm türleri kullanılabilir değildir.
    
    - Brezilya Güney: Yalnızca Standart ve Temel Kodlamaya Ayrılan Birimler kullanılabilir.
    - Hindistan Batı, Hindistan Güney, medya dosyaları için blob depolama alanı sağlar ve Media Services hesabıyla aynı coğrafi bölgede olmalıdır. Bir Media Services hesabı oluşturduğunuzda aynı bölgede var olan bir depolama hesabını seçebilir veya aynı bölgede yeni bir depolama hesabı oluşturabilirsiniz. Bir Media Services hesabını silerseniz ilişkili depolama hesabınızdaki blob’lar silinmez.

## AMS hesabı oluşturma

Bu bölümdeki adımlar bir AMS hesabının nasıl oluşturulacağını gösterir.

1. [Azure portal](https://portal.azure.com/)’da oturum açın.
2. **+Yeni** > **Medya + CDN** > **Media Services**’e tıklayın.

    ![Media Services Oluşturma](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **MEDYA HİZMETLERİ HESABI OLUŞTUR**’a gerekli değerleri girin.

    ![Media Services Oluşturma](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. **Hesap Adı**’nda, yeni AMS hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü küçük harf ve sayılardan oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.
    2. Abonelik’te, erişiminiz bulunan farklı Azure abonelikleri arasından seçim yapın.
    
    2. **Kaynak Grubu**’nda yeni veya mevcut bir kaynağı seçin.  Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](resource-group-overview.md#resource-groups) daha fazla bilgi edinin.
    3. **Konum**’da, Media Services hesabınız için medya ve meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. 
    
    3. **Depolama Hesabı** alanında, Media Services hesabınızdan gelen medya içeriğine blob depolama sağlamak üzere bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.

        Depolama hakkında daha fazla bilgi [burada](storage-introduction.md).

    4. Hesap dağıtımını ilerleme durumunu görmek için **Panoya sabitle**’yi seçin.
    
7. Formun alt kısmındaki **Oluştur**’a tıklayın.

    Hesap başarıyla oluşturulduktan sonra, durum **Çalışıyor** olarak değişir. 

    ![Media Services ayarları](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS hesabınızı yönetmek için (örneğin, videoları karşıya yüklemek, varlıkları kodlamak, işin ilerleme durumunu izlemek) **Ayarlar** penceresini kullanın.

## Anahtarları Yönet

Media Services hesabına program aracılığıyla erişmek için hesap adına ve birincil anahtara bilgilerine ihtiyacınız vardır.

1. Azure portalda hesabınızı seçin. 

    Sağda **Ayarlar** penceresi görüntülenir. 

2. **Ayarlar** penceresinde **Anahtarlar**’ı seçin. 

    **Anahtarları yönet** pencereleri hesap adını gösterir ve birincil ve ikincil anahtarlar görüntülenir. 
3. Değerleri kopyalamak için kopyala düğmesine basın.
    
    ![Media Services Anahtarları](./media/media-services-portal-vod-get-started/media-services-keys.png)

## Sonraki adımlar

Bundan böyle dosyaları AMS hesabınıza yükleyebilirsiniz. Daha fazla bilgi için bkz. [Dosya yükleme](media-services-portal-upload-files.md).

## Media Services’i öğrenme yolları

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





<!--HONumber=ago16_HO5-->


