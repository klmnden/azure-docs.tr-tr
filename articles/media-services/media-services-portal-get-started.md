<properties
    pageTitle=" Klasik Azure Portalı’nı kullanarak isteğe bağlı içerik teslim etmeye başlama | Microsoft Azure"
    description="Bu öğreticide, Klasik Azure Portalı’nı kullanarak Azure Media Services ile bir İsteğe Bağlı Video (VoD) içerik teslim uygulaması gerçekleştirilmesinin adımları açıklanmaktadır."
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


# Klasik Azure Portalı’nı kullanarak isteğe bağlı içerik teslim etmeye başlama


[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


Bu öğreticide, Klasik Azure Portalı’nı kullanarak bir İsteğe Bağlı Video (VoD) içerik teslim uygulaması gerçekleştirilmesinin adımları açıklanmaktadır.

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](/pricing/free-trial/?WT.mc_id=A261C142F). 


Bu öğretici aşağıdaki görevleri içerir:

1.  Azure Media Services hesabı oluşturun.
2.  Akış uç noktasını yapılandırın.
1.  Bir video dosyası yükleyin.
1.  Kaynak dosyayı uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın.
1.  Varlığı yayımlayın, akış ve aşamalı indirme URL’lerini alın.  
1.  İçeriğinizi oynatın.


## Azure Media Services hesabı oluşturma

1. [Klasik Azure Portalı](https://manage.windowsazure.com/)’nda **Yeni**’ye, sonra **Medya Hizmeti**’ne ve **Hızlı Oluştur**’a tıklayın.

    ![Media Services Hızlı Oluştur](./media/media-services-portal-get-started/wams-QuickCreate.png)

2. **AD** alanına yeni hesabın adını girin. Media Services hesabı adı, boşluk olmadan, tümü küçük harf ve sayılardan oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.

3. **BÖLGE** alanında Media Services hesabınıza ait meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir.

4. **DEPOLAMA HESABI** alanında, Media Services hesabınızdan gelen medya içeriğine blob depolama sağlamak üzere bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur.

5. Yeni bir depolama hesabı oluşturduysanız **YENİ DEPOLAMA HESABI ADI** alanına depolama hesabı için bir ad girin. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.

6. Formun altındaki **Hızlı Oluştur**’a tıklayın.

    İşlemin durumunu pencerenin altındaki ileti alanında izleyebilirsiniz.

    Hesap başarıyla oluşturulduktan sonra, durum Etkin olarak değişir.

    Sayfanın altında **ANAHTARLARI YÖNET** düğmesi görünür. Bu düğmeye tıkladığınızda Media Services hesap adını, birincil ve ikincil anahtarları içeren bir iletişim kutusu görüntülenir. Media Services hesabına programlı olarak erişmek için hesap adına ve birincil anahtar bilgilerine ihtiyacınız vardır.

    ![Media Services Sayfası](./media/media-services-portal-get-started/wams-mediaservices-page.png)

    Hesap adına çift tıkladığınızda, varsayılan olarak Hızlı Başlangıç sayfası görüntülenir. Bu sayfada, portalın diğer sayfalarında da bulunan bazı yönetim görevleri gerçekleştirilebilir. Örneğin, bir video dosyasını bu sayfadan veya İÇERİK sayfasından yükleyebilirsiniz.


## Portalı kullanarak akış uç noktasını yapılandırma

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Bit hızı uyarlamalı akış ile video geçerli ağ bant genişliği, CPU kullanımı ve diğer etkenlere bağlı olarak görüntülendiğinden istemci, daha yüksek veya daha düşük bit hızlı bir akışa geçebilir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için).

Media Services, bit hızı uyarlamalı MP4 veya Kesintisiz Akış kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış, HDS), bu akış biçimlerine yeniden paketlemeden iletmenize olanak tanıyan dinamik paketleme sağlar.

Dinamik paketlemeden yararlanmak için aşağıdakileri yapmanız gerekir:

- Ara (kaynak) dosyanızı bit hızı uyarlamalı bir MP4 veya Kesintisiz Akış dosyaları kümesine kodlayın (kodlama adımları bu öğreticinin ilerleyen bölümlerinde gösterilmektedir).  
- Kendisinden içeriğinizi iletmek istediğiniz *akış uç noktası* için en az bir akış birimi alın.

Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Akışa ayrılan birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Klasik Azure Portalı](https://manage.windowsazure.com/)’nda **Media Services**’e tıklayın. Ardından, medya hizmetinin adına tıklayın.

2. AKIŞ UÇ NOKTALARI sayfasını seçin. Değiştirmek istediğiniz akış uç noktasına tıklayın.

3. Akış birim sayısını belirtmek için, **ÖLÇEK** sekmesini seçin ve **ayrılan kapasite** kaydırıcısını hareket ettirin.

    ![Ölçek sayfası](./media/media-services-portal-get-started/media-services-origin-scale.png)

4. Yaptığınız değişiklikleri kaydetmek için **KAYDET** düğmesine tıklayın.

    Yeni birimlerin ayrılması yaklaşık 20 dakikada tamamlanır.

    >[AZURE.NOTE] Şu anda, herhangi bir pozitif sayıda akış birimi bulunan bir durumdan hiçbir akış birimi bulunmamasına dönmek, akışı bir saate varan bir süre boyunca devre dışı bırakabilir.
    >
    > Maliyet hesaplamasında, 24 saatlik dönem için belirtilen en yüksek birim sayısı kullanılır. Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz. [Media Services Fiyatlandırma Ayrıntıları](http://go.microsoft.com/fwlink/?LinkId=275107).

## İçerik yükleme


1. [Klasik Azure Portalı](http://go.microsoft.com/fwlink/?LinkID=256666&clcid=0x409)’nda **Media Services**’e ve ardından Media Services hesap adına tıklayın.
2. İÇERİK sayfasını seçin.
3. Sayfadaki veya portalın alt kısmındaki **Yükle** düğmesine tıklayın.
4. **İçerik yükle** iletişim kutusunda, istenen varlık dosyasına gidin. Dosyaya tıklayıp ardından **Aç**’a tıklayın veya Enter tuşuna basın.

    ![UploadContentDialog][uploadcontent]

5. **İçerik yükle** iletişim kutusunda **Dosya** ve **İçerik Adı** değerlerini kabul etmek için onay düğmesine tıklayın.
6. Yükleme başlar ve ilerleme durumunu portalın alt kısmından izleyebilirsiniz.  

    ![JobStatus][status]

Yükleme işlemi tamamlandığında yeni varlığı İçerik listesinde görürsünüz. Yeni içeriğin, kodlama görevlerine ilişkin kaynak içerik olarak izlenmesine yardım etmek amacıyla kural gereği adın sonuna "**-Source**" eklenir.

![ContentPage][contentpage]

Dosya boyutu değeri, yükleme işlemi durduktan sonra güncelleştirilmezse **Meta Verileri Eşitle** düğmesini seçin. Böylece varlık dosyası boyutu, depolama alanındaki gerçek dosya boyutuna eşitlenir ve İçerik sayfasındaki değer yenilenir.


## İçerik kodlama

### Genel Bakış

İnternet üzerinden dijital video teslim etmek için medyayı sıkıştırmanız gerekir. Media Services, içeriğinizin nasıl kodlanmasını istediğinizi (örneğin kullanılacak codec’ler, dosya biçimi, çözünürlük ve bit hızı) belirtmenize olanak tanıyan bir medya kodlayıcı sunar.

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Bit hızı uyarlamalı akış ile video geçerli ağ bant genişliği, CPU kullanımı ve diğer etkenlere bağlı olarak görüntülendiğinden istemci, daha yüksek veya daha düşük bit hızlı bir akışa geçebilir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için).

Media Services, bit hızı uyarlamalı MP4 veya Kesintisiz Akış kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış veya HDS), bu akış biçimlerine yeniden paketlemeden teslim etmenize olanak tanıyan dinamik paketleme sağlar.

Dinamik paketlemeden yararlanmak için aşağıdakileri yapmanız gerekir:

- Ara (kaynak) dosyanızı bit hızı uyarlamalı bir MP4 veya Kesintisiz Akış dosyaları kümesine kodlayın (kodlama adımları bu öğreticinin ilerleyen bölümlerinde gösterilmektedir).
- Kendisinden içeriğinizi teslim etmek istediğiniz akış uç noktası için en az bir İsteğe Bağlı akış birimi alın. Daha fazla bilgi için bkz. [İsteğe Bağlı Akışa ayrılan birimleri ölçeklendirme](media-services-manage-origins.md#scale_streaming_endpoints/).

Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Dinamik paketleme özelliklerini kullanabilmeye ek olarak, İsteğe Bağlı Akışa ayrılan birimlerin 200 MB/sn’lik artışlarla satın alabileceğiniz adanmış çıkış kapasitesi sağladığını da unutmayın. İsteğe bağlı akış varsayılan olarak, sunucu kaynaklarının (işlem veya çıkış kapasitesi gibi) diğer tüm kullanıcılarla paylaşıldığı bir paylaşılan örnek modelinde yapılandırılır. İsteğe bağlı akışın verimini artırmak için İsteğe Bağlı Akışa ayrılan birimler satın almanız önerilir.

### Kodlama

Bu bölümde, Klasik Azure Portalı’nı kullanarak içeriğinizi Medya Kodlayıcısı Standart ile kodlamak için atabileceğiniz adımlar açıklanmaktadır.

1.  Kodlamak istediğiniz dosyayı seçin.
    Bu dosya türü için kodlama destekleniyorsa İÇERİK sayfasının alt kısmındaki **İŞLE** düğmesi etkinleştirilir.
4. **İşle** iletişim kutusunda **Medya Kodlayıcısı Standart** işlemcisini seçin.
5. **Kodlama yapılandırmaları** arasından birini seçin.

    ![Process2][process2]

    [Medya Kodlayıcısı Standart için Görev Önayar Dizeleri](https://msdn.microsoft.com/en-US/library/mt269960) konusunda tüm önayarların anlamı açıklanmaktadır.  

5. Ardından, arzu edilen çıkış içeriği kolay adını girin veya varsayılan adı kabul edin. Onay düğmesine tıklayarak kodlama işlemini başlatın. İlerleme durumunu portalın alt kısmından izleyebilirsiniz.
6. **Tamam**’ı seçin.

    Kodlama tamamlandıktan sonra İÇERİK sayfası kodlanmış dosyayı içerir.

    Kodlama işinin ilerleme durumunu görüntülemek için **İŞLER** sayfasına geçin.  

    Dosya boyutu değeri, kodlama tamamlandıktan sonra güncelleştirilmezse **Meta Verileri Eşitle** düğmesini seçin. Böylece çıkış varlık dosyası boyutu, depolama alanındaki gerçek dosya boyutuna eşitlenir ve İçerik sayfasındaki değer yenilenir.


## İçerik yayımlama

### Genel Bakış

Kullanıcınıza içeriğinizin akışını sağlamak veya indirmek için kullanılabilecek bir URL sağlamak üzere, önce bulucu oluşturarak varlığınızı “yayımlamanız” gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. Media Services, iki tür bulucuyu destekler: Medyayı akışla aktarmak (örneğin MPEG DASH, HLS veya Kesintisiz Akış) için kullanılan OnDemandOrigin bulucuları ve medya dosyalarını indirmek için kullanılan Erişim İmzası (SAS) bulucuları.

Varlıklarınızı yayımlamak için Klasik Azure Portalı’nı kullandığınızda, bulucular sizin için oluşturulur ve OnDemand tabanlı bir URL (varlığınız bir .ism dosyası içeriyorsa) veya bir SAS URL'si sağlanır.

SAS URL'leri aşağıdaki biçimdedir.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Akış URL’si aşağıdaki biçime sahiptir ve bunu Kesintisiz Akış varlıklarını oynatmak için kullanabilirsiniz.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS akış URL’si oluşturmak için, URL’ye (format=m3u8-aapl) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH akış URL’si oluşturmak için, URL’ye (format=mpd-time-csf) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Bulucuların bir sona erme tarihi vardır. Varlıklarınızı yayımlamak için portalı kullandığınızda sona erme tarihi 100 yıl sonra gelecek olan bulucular oluşturulur.

>[AZURE.NOTE] Mart 2015 öncesinde portalı kullanarak bulucu oluşturduysanız kullandıysanız, sona erme tarihleri iki yıl sonrası olan bulucular oluşturulmuştur.  

Bir bulucunun sona erme tarihini güncelleştirmek için [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) ya da [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API’lerini kullanın. SAS bulucunun sona erme tarihini güncelleştirdiğinizde URL’nin değiştiğini unutmayın.

### Yayımlama

Portalı kullanarak bir varlık yayımlamak için aşağıdakileri yapın:

1. Varlığı seçin.
2. Yayımla düğmesine tıklayın.

 ![PublishedContent][publishedcontent]


## Portaldan içerik oynatma

Klasik Azure Portalı, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

İstediğiniz videoya tıklayın ve ardından portalın alt kısmındaki **Oynat** düğmesine tıklayın.

Bazı dikkate alınması gereken noktalar vardır:

- Videonun yayımlandığından emin olun.
- **MEDYA HİZMETLERİ İÇERİK OYNATICISI** varsayılan akış uç noktasından oynatır. Varsayılan dışı bir akış uç noktasından oynatmak istiyorsanız başka bir oynatıcı kullanın. Örneğin, [Azure Media Services Oynatıcı](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]



##Sonraki Adımlar: Media Services’i öğrenme yolları

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## Başka bir şey mi arıyorsunuz?

Beklediklerinizi bu konuda bulamadıysanız, eksik bir şeyler varsa veya herhangi bir nedenle gereksinimleriniz karşılanmadıysa, lütfen aşağıdaki Disqus yazışmasını kullanarak bize geri bildirimde bulunun.

### Ek kaynaklar
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-101-Get-your-video-online-now-">Azure Media Services 101 - Videonuzu hemen çevrimiçi yapın!</a>
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-102-Dynamic-Packaging-and-Mobile-Devices">Azure Media Services 102 - Dinamik Paketleme ve Mobil Cihazlar</a>


<!-- Anchors. -->


<!-- URLs. -->
[Klasik Azure Portalı]: http://manage.windowsazure.com/


<!-- Images -->
[portaloverview]: ./media/media-services-portal-get-started/media-services-content-page.png
[publishedcontent]: ./media/media-services-portal-get-started/media-services-upload-content-published.png
[uploadcontent]: ./media/media-services-portal-get-started/UploadContent.png
[status]: ./media/media-services-portal-get-started/Status.png
[kodlayıcı]: ./media/media-services-manage-content/EncoderDialog2.png
[marka]: ./media/branding-reporting.png
[contentpage]: ./media/media-services-portal-get-started/media-services-content-page.png
[işlem]: ./media/media-services-manage-content/media-services-process-video.png
[process2]: ./media/media-services-portal-get-started/media-services-process-video2.png
[şifrele]: ./media/media-services-manage-content/media-services-encrypt-content.png
[AMSPlayer]: ./media/media-services-portal-get-started/media-services-portal-player.png



<!--HONumber=Aug16_HO1-->


