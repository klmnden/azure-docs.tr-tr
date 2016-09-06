<properties
    pageTitle=" Azure portal kullanarak isteğe bağlı içerik göndermeye başlama | Microsoft Azure"
    description="Bu öğretici, Azure portalı kullanarak Azure Media Services (AMS) uygulaması ile temel bir İsteğe Bağlı Video (VoD) içerik teslim hizmeti uygulamanın adımlarını açıklar."
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
    ms.date="08/30/2016"
    ms.author="juliako"/>


# Azure portal kullanarak isteğe bağlı içerik göndermeye başlama

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Bu öğretici, Azure portalı kullanarak Azure Media Services (AMS) uygulaması ile temel bir İsteğe Bağlı Video (VoD) içerik teslim hizmeti uygulamanın adımlarını açıklar.

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/). 

Bu öğretici aşağıdaki görevleri içerir:

1.  Azure Media Services hesabı oluşturun.
2.  Akış uç noktasını yapılandırın.
1.  Bir video dosyası yükleyin.
1.  Kaynak dosyayı uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın.
1.  Varlığı yayımlayın, akış ve aşamalı indirme URL’lerini alın.  
1.  İçeriğinizi oynatın.


## Azure Media Services hesabı oluşturma

Bu bölümdeki adımlar bir AMS hesabının nasıl oluşturulacağını gösterir.

1. [Azure portal](https://portal.azure.com/)’da oturum açın.
2. **+Yeni** > **Medya + CDN** > **Media Services**’e tıklayın.

    ![Media Services Oluşturma](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **MEDYA HİZMETLERİ HESABI OLUŞTUR**’a gerekli değerleri girin.

    ![Media Services Oluşturma](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. **Hesap Adı**’nda, yeni AMS hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü küçük harf ve sayılardan oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.
    2. Abonelik’te, erişiminiz bulunan farklı Azure abonelikleri arasından seçim yapın.
    
    2. **Kaynak Grubu**’nda yeni veya mevcut bir kaynağı seçin.  Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](resource-group-overview.md#resource-groups) daha fazla bilgi edinin.
    3. **Konum**’da, Media Services hesabınız için medya ve meta veri kayıtlarını depolamak için kullanılan coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. 
    
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

## Akış uç noktalarını yapılandırma

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için).

Media Services, bu akış biçimlerinin her birinin önceden paketlenmiş sürümlerini depolamanıza gerek kalmadan, uyarlamalı bit hızı MP4 ile kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış, HDS) tam vaktinde göndermenize olanak tanıyan dinamik paketleme özelliğine sahiptir.

Dinamik paketlemeden yararlanmak için aşağıdakileri yapmanız gerekir:

- Mezzanine dosyanızı uyarlamalı bit hızlı MP4 dosyaları (kodlama adımları daha sonra bu öğreticide gösterilmiştir) grubu olarak kodlayın.  
- İçeriğinizi teslim etmek istediğiniz *akış uç noktası* için en az bir akış birimi oluşturun. Aşağıdaki adımlar akış birim sayısının nasıl değiştirileceğini göstermektedir.

Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Akışa ayrılan birim sayısını oluşturmak ve değiştirmek için, aşağıdakileri yapın:


1. **Ayarlar** penceresinde, **Akış uç noktaları**’na tıklayın. 

2. Varsayılan akış uç noktasına tıklayın. 

    **VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILAR** penceresi görüntülenir.

3. Akış birimi sayısını belirtmek için, **Akış birimleri** kaydırıcısını kaydırın.

    ![Akış birimleri](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Yaptığınız değişiklikleri kaydetmek için **Kaydet** düğmesine tıklayın.

    >[AZURE.NOTE]Yeni birimleri ayırmanın tamamlanması 20 dakika sürebilir.

## Dosyaları karşıya yükleme

Azure Media Services kullanarak video akışı sağlamak için kaynak videoları karşıya yüklemeniz, bunları çoklu bit hızlarında kodlamanız ve sonucu yayımlamanız gerekir. İlk adım, bu bölümde ele alınmıştır. 

1. **Ayar** penceresinde **Varlıklar**’a tıklayın.

    ![Dosyaları karşıya yükleme](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. **Karşıya Yükle** düğmesine tıklayın.

    **Varlığı karşıya yükle** penceresi görüntülenir.

    >[AZURE.NOTE] Dosya boyutu sınırlaması yoktur.
    
4. Bilgisayarınızda istediğiniz videoyu bulun, seçin ve Tamam'a tıklayın.  

    Karşıya yükleme başlar ve dosya adı altında ilerleme durumunu görebilirsiniz.  

Karşıya yükleme işlemi tamamlandıktan sonra, yeni varlık **Varlıklar** penceresinde listelenir. 

## Varlıkları kodlama

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için). Videolarınızı uyarlamalı bit hızı akışına hazırlamak için, kaynak videonuzu çoklu bit hızı dosyalarına kodlamanız gerekir. Videolarınızı kodlamak için **Medya Kodlayıcısı Standart** kodlayıcıyı kullanmalısınız.  

Media Services, çoklu bit hızlı MP4’leri şu akış biçimlerinde yeniden paketlemenize gerek kalmadan göndermenizi sağlayan dinamik paketleme olanağı da sağlar: MPEG DASH, HLS, Kesintisiz Akış veya HDS. Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Dinamik paketlemeden yararlanmak için aşağıdakileri yapmanız gerekir:

- Kaynak dosyanızı çoklu bit hızlı MP4 dosyaları (kodlama adımları daha sonra bu bölümde gösterilmiştir) grubu olarak kodlayın.
- Kendisinden içeriğinizi iletmek istediğiniz akış uç noktası için en az bir akış birimi alın. Daha fazla bilgi için bkz. [akış uç noktalarını yapılandırma](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### Kodlama için portalı kullanmak üzere

Bu bölüm, içeriğinizi Medya Kodlayıcısı Standart ile kodlamak için atabileceğiniz adımları açıklar.

1.  **Ayarlar** penceresinde **Varlıklar**’ı seçin.  
2.  **Varlıklar** penceresinde kodlamak istediğiniz varlığı seçin.
3.  **Kodla** düğmesine basın.
4.  **Bir varlık kodla** penceresinde, seçin "Medya Kodlayıcısı Standart" işlemcisini ve bir ön ayarı seçin. Örneğin, girdi videonuzun 1920 x 1080 piksel çözünürlüğü olduğunu biliyorsanız, "H264 Çoklu Bit hızı 1080p" ön ayarını kullanabilirsiniz. Ön ayarlar hakkında daha fazla bilgi için [bu](https://msdn.microsoft.com/library/azure/mt269960.aspx) makaleye bakın. Girdi videonuzla en fazla ilgili olan ön ayarı seçmeniz önemlidir. Düşük çözünürlüklü (640 x 360) bir videonuz olması durumunda, varsayılan "H264 Çoklu Bit hızı 1080p" ön ayarını kullanmamalısınız.
    
    Daha kolay yönetim için, çıktı varlık adını ve işin adını düzenleme seçeneğine sahipsiniz.
        
    ![Varlıkları kodlama](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. **Oluştur**’a basın.

### Kodlama işi ilerleme durumunu izleme

Kodlama işinin ilerleme durumunu izlemek için, **Ayarlar** (sayfanın üst kısmında) ve ardından **İşler**’e tıklayın.

![İşler](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## İçerik yayımlama

Kullanıcınıza içeriğinizin akışını sağlamak veya indirmek için kullanılabilecek bir URL sağlamak üzere, önce bulucu oluşturarak varlığınızı “yayımlamanız” gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. Media Services iki tür bulucuyu destekler: 

- Akış (OnDemandOrigin) bulucuları, uyarlamalı akış için kullanılır (örneğin, MPEG DASH, HLS ya da Kesintisiz Akış için). Akış bulucusu oluşturmak için varlığınız bir .ism dosyası içermelidir. 
- Aşamalı indirme aracılığıyla video teslimi için kullanılan aşamalı (SAS) bulucular.


Akış URL’si aşağıdaki biçime sahiptir ve bunu Kesintisiz Akış varlıklarını oynatmak için kullanabilirsiniz.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS akış URL’si oluşturmak için, URL’ye (format=m3u8-aapl) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH akış URL’si oluşturmak için, URL’ye (format=mpd-time-csf) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


SAS URL'leri aşağıdaki biçimdedir.

    {blob container name}/{asset name}/{file name}/{SAS signature}

>[AZURE.NOTE] Mart 2015 öncesinde portalı kullanarak bulucu oluşturduysanız, iki yıllık bir sona erme tarihi olan bulucular oluşturulmuştur.  

Bir bulucunun sona erme tarihini güncelleştirmek için [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) ya da [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API’lerini kullanın. SAS bulucunun sona erme tarihini güncelleştirdiğinizde URL değişir.

### Bir varlık yayımlamak için portal kullanmak üzere

Portalı kullanarak bir varlık yayımlamak için aşağıdakileri yapın:

1. **Ayarlar** > **Varlıklar**’ı seçin.
1. Yayımlamak istediğiniz varlığı seçin.
1. **Yayımla** düğmesine tıklayın.
1. Bulucu türünü seçin.
2. **Ekle**’ye basın.

    ![Yayımlama](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL **Yayımlanan URL’ler** listesine eklenir.

## Portaldan içerik oynatma

Azure portal, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

İstediğiniz videoya tıklayın ve ardından **Oynat** düğmesine tıklayın.

![Yayımlama](./media/media-services-portal-vod-get-started/media-services-play.png)

Bazı dikkate alınması gereken noktalar vardır:

- Videonun yayımlandığından emin olun.
- **Medya oynatıcı** varsayılan akış uç noktasından oynatılır. Varsayılan olmayan bir akış uç noktasından oynatmak istiyorsanız, URL'yi kopyalamak için tıklayın ve başka bir oynatıcı kullanın. Örneğin, [Azure Media Services Oynatıcı](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##Sonraki adımlar

Media Services öğrenme yollarını gözden geçirin.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





<!--HONumber=ago16_HO5-->


